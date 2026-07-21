---
title: Importación masiva de miembros del programa
feature: REST API
description: Obtenga información sobre cómo importar miembros de programa de forma masiva mediante la API de REST de Marketo utilizando archivos CSV TSV o SSV de menos de 10 MB, límites de cola, parámetros obligatorios y estado del trabajo de sondeo.
exl-id: b0e1039a-fe9b-4fb7-9aa6-9980a06da673
TQID: https://experienceleague.adobe.com/T1PAzLN1mnp38kJ0jwh6kPv6r1Uvxc7-o9zeTHetIV0
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 771
ht-degree: 0%

---

# Importación masiva de miembros del programa

[Referencia de extremo de importación masiva de miembros de programa](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Program-Members)

Use la [API en bloque](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Program-Members) para importar de forma asincrónica un gran número de registros de miembros del programa. Proporcione los registros en un archivo plano delimitado por comas, tabuladores o puntos y comas que tenga menos de 10 MB.

La importación masiva de miembros de programa sólo admite la operación de registro &quot;insertar o actualizar&quot;.

## Límites de procesamiento

Cada solicitud de importación masiva se agrega como un trabajo a una cola FIFO (primera entrada, primera salida). Se aplican los límites siguientes:

- Se puede procesar simultáneamente un máximo de dos trabajos.
- Se pueden colocar un máximo de 10 trabajos en cola, incluidos los dos trabajos que se están procesando.

Si supera el máximo de 10 trabajos, la API devuelve un error de `1016, Too many imports`.

## Importar archivo

La primera fila del archivo debe ser un encabezado que enumere los nombres de los campos de la API de REST a los que se asignan los valores de cada fila. Recupere estos nombres usando los extremos [Describir posible cliente](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/describeUsingGET_2) y [Describir miembro del programa](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/describeProgramMemberUsingGET).

Los registros pueden contener campos de posibles clientes, campos de posibles clientes personalizados y campos de miembros de programa personalizados.

Un archivo típico sigue este patrón:

```text
email,firstName,lastName
test@example.com,John,Doe
```

Envíe la solicitud utilizando el tipo de contenido `multipart/form-data`. Utilice una implementación de biblioteca existente para construir la solicitud de varias partes.

## Creación de un trabajo

El extremo [Importar miembros de programa](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Program-Members/operation/importProgramMemberUsingPOST) lee registros de miembros de programa de un archivo y los agrega a un programa con un estado especificado. Los registros pueden contener campos de posibles clientes y campos de miembros de programa personalizados.

Cada registro debe incluir el campo de correo electrónico, que se utiliza para la deduplicación.

El parámetro de ruta de acceso `programId` especifica el programa al que se agregan los miembros.

La solicitud requiere tres parámetros de consulta:

- `format`: el formato de archivo de importación (`CSV`, `TSV` o `SSV`).
- `programMemberStatus`: el estado del programa asignado a los miembros importados.
- `file`: nombre del archivo que contiene los registros de miembros del programa.

```http
POST /bulk/v1/program/{programId}/members/import.json?format=csv&programMemberStatus=On List
```

```text
Content-Type: multipart/form-data; boundary=--------------------------118046853683028616211319
Content-Length: 772
Host: <munchkinId>.mktorest.com
```

```text
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

Como el extremo es asincrónico, la respuesta contiene `batchId` y `status` campos. El estado puede ser `Queued`, `Importing` o `Failed`.

Conserve `batchId` para comprobar el estado de la importación y recuperar errores o advertencias una vez finalizada. `batchId` sigue siendo válido por siete días.

La siguiente solicitud cURL de la línea de comandos envía el trabajo de ejemplo:

```bash
curl -i -F format='csv' -F programMemberStatus='On List' -F file='@Lead-House-Lannister.csv' -F access_token='<Access Token>' <REST API Endpoint Base URL>/bulk/v1/program/{programId}/members/import.json
```

En este ejemplo, el archivo de importación `Lead-House-Lannister.csv` contiene los datos siguientes:

```text
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

Después de crear el trabajo de importación, sondee cada 5-30 segundos. Pase el parámetro de ruta de acceso `batchId` al extremo [Obtener estado de miembro del programa de importación](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET).

```http
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

Esta respuesta muestra una importación completada. El estado puede ser `Complete`, `Queued`, `Importing` o `Failed`.

Cuando se completa el trabajo, la respuesta muestra los números de filas procesadas, fallidas y procesadas con advertencias. El parámetro `message` también puede proporcionar un mensaje de error cuando el estado es `Failed`.

## Errores de

El atributo `numOfRowsFailed` de la respuesta [Obtener estado de miembro del programa de importación](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET) indica el número de filas con errores. Un valor mayor que cero significa que se produjeron errores.

Pase el parámetro de ruta de acceso `batchId` al extremo Obtener errores de miembros del programa de importación para recuperar los registros con errores y sus causas.

```http
GET /bulk/v1/program/members/import/{batchId}/failures.json
```

El extremo devuelve un archivo que identifica cada fila con error y explica por qué falló el registro. El archivo utiliza el formato especificado por el parámetro `format` durante la creación del trabajo. Un campo adicional en cada registro describe el error.

Por ejemplo, supongamos que importa el siguiente archivo con una puntuación de posible cliente no válida:

```text
firstName,lastName,email,title,company,leadScore
Aerys,Targaryen,Aerys@Targaryen.com,Targaryen,House Targaryen,TEXT_VALUE_IN_INTEGER_FIELD
```

El estado del trabajo devuelve `numOfRowsFailed` como 1, lo que indica que se produjo un error:

```http
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

Recupere el archivo de error para obtener más información:

```http
GET /bulk/v1/program/members/import/{batchId}/failures.json
```

```text
firstName,lastName,email,title,company,leadScore,Import Failure Reason
Aerys,Targaryen,Aerys@Targaryen.com,Targaryen,House Targaryen,TEXT_VALUE_IN_INTEGER_FIELD,Invalid data type in field Lead Score
```

## Advertencias

El atributo `numOfRowsWithWarning` de la respuesta [Obtener estado de miembro del programa de importación](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET) indica el número de filas con advertencias. Un valor mayor que cero significa que se han producido advertencias.

Pase el parámetro de ruta de acceso `batchId` al extremo [Obtener advertencias de miembros del programa de importación](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberWarningsUsingGET) para recuperar los registros afectados y sus causas.

```http
GET /bulk/v1/program/members/import/{batchId}/warnings.json
```

El extremo devuelve un archivo que identifica cada fila con una advertencia y explica por qué se produjo la advertencia. El archivo utiliza el formato especificado por el parámetro `format` durante la creación del trabajo. Un campo adicional en cada registro describe la advertencia.

Por ejemplo, supongamos que importa el siguiente archivo con una dirección de correo electrónico no válida:

```text
firstName,lastName,email,title,company,leadScore
Aerys,Targaryen,INVALID_EMAIL,Targaryen,House Targaryen,0
```

El estado del trabajo devuelve `numOfRowsWithWarning` como 1, lo que indica que se produjo una advertencia:

```http
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

Recupere el archivo de advertencia para obtener más información:

```http
GET /bulk/v1/program/members/import/{batchId}/warnings.json
```

```text
firstName,lastName,email,title,company,leadScore,Import Warning Reason
Aerys,Targaryen,INVALID_EMAIL,Targaryen,House Targaryen,0,Invalid email address
```
