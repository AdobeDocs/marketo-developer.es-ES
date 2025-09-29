---
title: Importación masiva de miembros del programa
feature: REST API
description: Obtenga información sobre cómo importar miembros de programa de forma masiva mediante la API de REST de Marketo utilizando archivos CSV TSV o SSV de menos de 10 MB, límites de cola, parámetros obligatorios y estado del trabajo de sondeo.
exl-id: b0e1039a-fe9b-4fb7-9aa6-9980a06da673
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '860'
ht-degree: 0%

---

# Importación masiva de miembros del programa

[Referencia de extremo de importación masiva de miembros del programa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members)

Para grandes cantidades de registros de miembros del programa, los miembros del programa se pueden importar de manera asincrónica con la [API en bloque](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members). Esto permite importar una lista de registros en Marketo mediante un archivo plano con delimitadores (coma, tabulación o punto y coma). El archivo puede contener cualquier número de registros, siempre que el tamaño total del archivo sea inferior a 10 MB. La operación de registro es sólo &quot;insertar o actualizar&quot;.

## Límites de procesamiento

Se le permite enviar más de una solicitud de importación masiva, con limitaciones. Cada solicitud se agrega como un trabajo a una cola FIFO para su procesamiento. Se procesan un máximo de dos trabajos al mismo tiempo. Se permite un máximo de diez trabajos en cola en un momento determinado (incluidos los dos que se están procesando actualmente). Si supera el máximo de diez trabajos, se devuelve el error &quot;1016, Demasiadas importaciones&quot;.

## Importar archivo

La primera fila del archivo debe ser un encabezado que enumere los nombres de API de REST correspondientes como campos para asignar los valores de cada fila a. Los nombres de las API de REST se pueden recuperar mediante los extremos [Describir posible cliente](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) o [Describir miembro del programa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeProgramMemberUsingGET). Los registros pueden contener campos de posibles clientes, campos de posibles clientes personalizados y campos de miembros de programa personalizados.

Un archivo típico seguiría este patrón básico:

```
email,firstName,lastName
test@example.com,John,Doe
```

La llamada en sí se realiza con el tipo de contenido `multipart/form-data`.

Este tipo de solicitud puede ser difícil de implementar, por lo que se recomienda encarecidamente que utilice una implementación de biblioteca existente.

## Creación de un trabajo

El extremo [Importar miembros de programa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/importProgramMemberUsingPOST) lee un archivo que contiene registros de miembros de programa y los agrega a un programa con un estado determinado. Los registros pueden contener tanto campos de posibles clientes como campos personalizados de miembros del programa. Todos los registros deben incluir el campo de correo electrónico, que se utiliza con fines de deduplicación.

El parámetro de ruta de acceso `programId` especifica el programa al que se agregan los miembros.

Se requieren tres parámetros de consulta. El parámetro `format` especifica el formato de archivo de importación (CSV, TSV o SSV), el parámetro `programMemberStatus` especifica el estado del programa para los miembros que se agregan al programa y el parámetro `file` contiene el nombre del archivo de importación que contiene registros de miembros del programa.

```
POST /bulk/v1/program/{programId}/members/import.json?format=csv&programMemberStatus=On List
```

```
Content-Type: multipart/form-data; boundary=--------------------------118046853683028616211319
Content-Length: 772
Host: <munchkinId>.mktorest.com
```

```
----------------------------118046853683028616211319
Content-Disposition: form-data; name="file"; filename="Lead-House-Lannister.csv"
Content-Type: text/csv

firstName,lastName,email,title,company,leadScore
Joanna,Lannister,Joanna@Lannister.com,Lannister,House Lannister,0
Tywin,Lannister,Tywin@Lannister.com,Lannister,House Lannister,0
Cersei,Lannister,Cersei@Lannister.com,Lannister,House Lannister,0
Jamie,Lannister,Jamie@Lannister.com,Lannister,House Lannister,0
Tyrion,Lannister,Tyrion@Lannister.com,Lannister,House Lannister,0
Kevan,Lannister,Kevan@Lannister.com,Lannister,House Lannister,0
Dorna,Lannister,Dorna@Lannister.com,Lannister,House Lannister,0
Lancel,Lannister,Lancel@Lannister.com,Lannister,House Lannister,0

----------------------------118046853683028616211319--
```

```json
{
    "requestId": "17f4a#16f87f87325",
    "result": [
        {
            "batchId": 1040,
            "importId": "1040",
            "status": "Queued"
        }
    ],
    "success": true
}
```

Observe en la respuesta a nuestra llamada que hay un campo `batchId` y un campo `status` para el registro en la matriz de resultados. Dado que este extremo es asincrónico, puede devolver el estado En cola, Importando o Error. Debe conservar `batchId` para obtener el estado del trabajo de importación y para recuperar errores o advertencias al finalizar. `batchId` sigue siendo válido por siete días.

En el ejemplo anterior, una manera sencilla de llamar al extremo es utilizar cURL desde la línea de comandos:

```bash
curl -i -F format='csv' -F programMemberStatus='On List' -F file='@Lead-House-Lannister.csv' -F access_token='<Access Token>' <REST API Endpoint Base URL>/bulk/v1/program/{programId}/members/import.json
```

Cuando el archivo de importación &quot;Lead-House-Lannister.csv&quot; contenga lo siguiente:

```
firstName,lastName,email,title,company,leadScore
Joanna,Lannister,Joanna@Lannister.com,Lannister,House Lannister,0
Tywin,Lannister,Tywin@Lannister.com,Lannister,House Lannister,0
Cersei,Lannister,Cersei@Lannister.com,Lannister,House Lannister,0
Jamie,Lannister,Jamie@Lannister.com,Lannister,House Lannister,0
Tyrion,Lannister,Tyrion@Lannister.com,Lannister,House Lannister,0
Kevan,Lannister,Kevan@Lannister.com,Lannister,House Lannister,0
Dorna,Lannister,Dorna@Lannister.com,Lannister,House Lannister,0
Lancel,Lannister,Lancel@Lannister.com,Lannister,House Lannister,0
```

## Estado del trabajo de sondeo

Una vez creado el trabajo de importación, debe consultar su estado. Se recomienda sondear el trabajo de importación cada 5-30 segundos. Para ello, pase el parámetro de ruta de acceso `batchId` al extremo [Obtener estado de miembro del programa de importación](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET).

```
GET /bulk/v1/program/members/import/{batchId}/status.json
```

```json
{
    "requestId": "e0cb#16f87f8b177",
    "result": [
        {
            "batchId": 1040,
            "importId": "1040",
            "status": "Complete",
            "numOfLeadsProcessed": 8,
            "numOfRowsFailed": 0,
            "numOfRowsWithWarning": 0,
            "message": "Import succeeded, 8 records imported (8 members)"
        }
    ],
    "success": true
}
```

Esta respuesta muestra una importación completada. El estado puede ser uno de los siguientes: Completado, En cola, Importando, Fallido.

Si el trabajo se ha completado, tiene un listado del número de filas procesadas, con errores o con advertencias. El parámetro message también puede proporcionar el mensaje de error si el estado es Failed.

## Errores de

Los errores se indican mediante el atributo `numOfRowsFailed` en la respuesta [Obtener estado de miembro del programa de importación](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET). Si numOfRowsFailed es mayor que cero, ese valor indica el número de errores que se produjeron.

Utilice el extremo Obtener errores de miembros del programa de importación para recuperar registros y causas de filas con errores pasando el parámetro de ruta de acceso `batchId`.

```
GET /bulk/v1/program/members/import/{batchId}/failures.json
```

El extremo responde con un archivo que indica qué filas fallaron, junto con un mensaje que indica por qué falló el registro. El formato del archivo es el mismo que el especificado en el parámetro `format` durante la creación del trabajo. Se anexa un campo adicional a cada registro con una descripción del error.

Por ejemplo, supongamos que importa el siguiente archivo con una puntuación de posible cliente no válida:

```
firstName,lastName,email,title,company,leadScore
Aerys,Targaryen,Aerys@Targaryen.com,Targaryen,House Targaryen,TEXT_VALUE_IN_INTEGER_FIELD
```

Cuando compruebe el estado del trabajo, verá `numOfRowsFailed` es 1, lo que indica que se ha producido un error:

```
GET /bulk/v1/program/members/import/{batchId}/status.json
```

```json
{
    "requestId": "4c2d#16f8b32c8ef",
    "result": [
        {
            "batchId": 1046,
            "importId": "1046",
            "status": "Complete",
            "numOfLeadsProcessed": 0,
            "numOfRowsFailed": 1,
            "numOfRowsWithWarning": 0,
            "message": "Import completed with errors, 0 records imported (0 members), 1 failed"
        }
    ],
    "success": true
}
```

A continuación, recupere el archivo de errores para obtener más detalles sobre el error:

```
GET /bulk/v1/program/members/import/{batchId}/failures.json
```

```
firstName,lastName,email,title,company,leadScore,Import Failure Reason
Aerys,Targaryen,Aerys@Targaryen.com,Targaryen,House Targaryen,TEXT_VALUE_IN_INTEGER_FIELD,Invalid data type in field Lead Score
```

## Advertencias

Las advertencias están indicadas por el atributo `numOfRowsWithWarning` en la respuesta [Obtener estado de miembro del programa de importación](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET). Si `numOfRowsWithWarning` es mayor que cero, ese valor indica el número de advertencias que se produjeron.

Use el extremo [Obtener advertencias de miembros del programa de importación](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberWarningsUsingGET) para recuperar registros y causas de filas de advertencia pasando el parámetro de ruta de acceso `batchId`.

```
GET /bulk/v1/program/members/import/{batchId}/warnings.json
```

El extremo responde con un archivo que indica qué filas produjeron advertencias, junto con un mensaje que indica por qué el registro produjo una advertencia. El formato del archivo es el mismo que el especificado en el parámetro `format` durante la creación del trabajo. Se anexa un campo adicional a cada registro con una descripción de la advertencia.

Por ejemplo, supongamos que importa el siguiente archivo con una dirección de correo electrónico no válida:

```
firstName,lastName,email,title,company,leadScore
Aerys,Targaryen,INVALID_EMAIL,Targaryen,House Targaryen,0
```

Cuando compruebe el estado del trabajo, verá `numOfRowsWithWarning` es 1, lo que indica que se ha producido una advertencia:

```
GET /bulk/v1/program/members/import/{batchId}/status.json
```

```json
{
   "requestId":"4ca1#16f883c2003",
   "result":[
      {
         "batchId":1041,
         "importId":"1041",
         "status":"Complete",
         "numOfLeadsProcessed":1,
         "numOfRowsFailed":0,
         "numOfRowsWithWarning":1,
         "message":"Import succeeded, 1 records imported (1 members), 1 warning."
      }
   ],
   "success":true
}
```

A continuación, recupere el archivo de advertencias para obtener más detalles sobre la advertencia:

```
GET /bulk/v1/program/members/import/{batchId}/warnings.json
```

```
firstName,lastName,email,title,company,leadScore,Import Warning Reason
Aerys,Targaryen,INVALID_EMAIL,Targaryen,House Targaryen,0,Invalid email address
```
