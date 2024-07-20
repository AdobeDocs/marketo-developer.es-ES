---
title: Importación masiva de posibles clientes
feature: REST API
description: Importación por lotes de datos de posibles clientes.
exl-id: 615f158b-35f9-425a-b568-0a7041262504
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '803'
ht-degree: 0%

---

# Importación masiva de posibles clientes

[Referencia de extremo de importación masiva de posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads)

Para grandes cantidades de registros de posibles clientes, los posibles clientes se pueden importar asincrónicamente con la [API en bloque](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/importLeadUsingPOST). Esto permite importar una lista de registros en Marketo mediante un archivo plano con delimitadores (coma, tabulación o punto y coma). El archivo puede contener cualquier número de registros, siempre que el tamaño total del archivo sea inferior a 10 MB. La operación de registro es sólo &quot;insertar o actualizar&quot;.

## Límites de procesamiento

Se le permite enviar más de una solicitud de importación masiva, con limitaciones. Cada solicitud se agrega como un trabajo a una cola FIFO para su procesamiento. Se procesan un máximo de dos trabajos al mismo tiempo. Se permite un máximo de diez trabajos en cola en un momento determinado (incluidos los dos que se están procesando actualmente). Si supera el máximo de diez trabajos, se devuelve el error &quot;1016, Demasiadas importaciones&quot;.

## Importar archivo

La primera fila del archivo debe ser un encabezado que enumere los campos de API de REST correspondientes para asignar los valores de cada fila a. Un archivo típico seguiría este patrón básico:

```
email,firstName,lastName
test@example.com,John,Doe
```

El campo `externalCompanyId` se puede usar para vincular el registro de posible cliente a un registro de empresa. El campo `externalSalesPersonId` se puede usar para vincular el registro de cliente potencial a un registro de vendedor.

La llamada en sí se realiza con el tipo de contenido `multipart/form-data`.

Este tipo de solicitud puede ser difícil de implementar, por lo que se recomienda encarecidamente que utilice una implementación de biblioteca existente.

## Creación de un trabajo

Para realizar una solicitud de importación masiva, debe establecer el encabezado de tipo de contenido en &quot;multipart/form-data&quot; e incluir al menos un parámetro de archivo con el contenido del archivo, y un parámetro de formato con el valor &quot;csv&quot;, &quot;tsv&quot; o &quot;ssv&quot; que indique el formato de archivo.

```
POST /bulk/v1/leads.json?format=csv
```

```
Content-Type: multipart/form-data; boundary=------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Length: 311
Host: <munchkinId>.mktorest.com
```

```
------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Disposition: form-data; name="file"; filename="leads.csv"
Content-Type: text/csv

FirstName,LastName,Email,Company
Able,Baker,ablebaker@marketo.com,Marketo
Charlie,Dog,charliedog@marketo.com,Marketo
Easy,Fox,easyfox@marketo.com,Marketo
------WebKitFormBoundaryBQACkJZyaiIAXogC--
```

```json
{
    "requestId": "d01f#15d672f8560",
    "result": [
        {
            "batchId": 3404,
            "importId": "3404",
            "status": "Queued"
        }
    ],
    "success": true
}
```

Este extremo usa [multipart/form-data como tipo de contenido](https://www.w3.org/Protocols/rfc1341/7_2_Multipart.html). Esto puede resultar difícil de hacer correctamente, por lo que la práctica recomendada es utilizar una biblioteca de compatibilidad con HTTP en el idioma que elija. Una forma sencilla de hacerlo con cURL desde la línea de comandos es la siguiente:

```
curl -i -F format=csv -F file=@lead_data.csv -F access_token=<Access Token> <REST API Endpoint Base URL>/bulk/v1/leads.json
```

Donde el archivo de importación &quot;lead_data.csv&quot; contiene lo siguiente:

```
FirstName,LastName,Email,Company
Able,Baker,ablebaker@marketo.com,Marketo
Charlie,Dog,charliedog@marketo.com,Marketo
Easy,Fox,easyfox@marketo.com,Marketo
```

Opcionalmente, también puede incluir los parámetros `lookupField`, `listId` y `partitionName` en su solicitud. `lookupField` le permite seleccionar un campo específico en el que deduplicar, al igual que Sincronizar posibles clientes, y el valor predeterminado es correo electrónico. Puede especificar `id` como `lookupField` para indicar una operación de &quot;solo actualización&quot;. `listId` le permite seleccionar una lista estática a la que importar la lista de posibles clientes; esto hará que los posibles clientes de la lista se conviertan en miembros de esta lista estática, además de las creaciones o actualizaciones causadas por la importación. `partitionName` selecciona una partición específica a la cual importar. Consulte la sección Espacios de trabajo y particiones para obtener más información.

Observe en la respuesta a nuestra llamada que no hay un listado de éxitos o errores como con Sincronizar posibles clientes, sino un batchId y un campo de estado para el registro en la matriz de resultados. Esto se debe a que esta API es asíncrona y puede devolver el estado En cola, Importando o Error. Debe conservar batchId para obtener el estado del trabajo de importación y para recuperar errores y/o advertencias tras la finalización. batchId sigue siendo válido durante siete días.

## Estado del trabajo de sondeo

Se recomienda sondear el trabajo cada 5-30 segundos, según la latencia necesaria y las limitaciones de llamadas de API, para ver el estado del trabajo de importación. Puede hacerlo con la API Obtener estado del posible cliente de importación.

```
GET /bulk/v1/leads/batch/{id}.json
```

```json
{
   "requestId":"8136#146daebc2ed",
   "success":true,
   "result":[
      {
         "batchId":1022,
         "status":"Complete",
         "numOfLeadsProcessed":2,
         "numOfRowsFailed":1,
         "numOfRowsWithWarning":0,
         "message":"Import completed with errors, 2 records imported (2 members), 1 failed"
      }
   ]
}
```

Esta respuesta muestra una importación completada, pero el estado puede ser uno de los siguientes:

- Completo
- En cola
- Importando
- Error

Si el trabajo se ha completado, tiene un listado con el número de filas procesadas, con errores y sin advertencias. El parámetro message también puede proporcionar el mensaje de error si el estado es Failed.

## Errores de

Los errores se indican mediante el atributo &quot;numOfRowsFailed&quot; en la respuesta Obtener estado del posible cliente de importación. Si &quot;numOfRowsFailed&quot; es mayor que cero, ese valor indica el número de errores que se produjeron.

Para recuperar los registros y las causas de las filas con errores, debe recuperar el archivo de errores:

```
GET /bulk/v1/leads/batch/{id}/failures.json
```

La API responde con un archivo que indica qué filas fallaron, junto con un mensaje que indica por qué falló el registro. El formato del archivo es el mismo que se especifica en el parámetro &quot;format&quot; durante la creación del trabajo. Se anexa un campo adicional a cada registro con una descripción del error.

## Advertencias

Las advertencias se indican mediante el atributo &quot;numOfRowsWithWarning&quot; en la respuesta Obtener estado del posible cliente de importación. Si &quot;numOfRowsWithWarning&quot; es mayor que cero, ese valor indica el número de advertencias que se produjeron.

Para recuperar los registros y las causas de las filas de advertencia, recupere el archivo de advertencia:

```
GET /bulk/v1/leads/batch/{id}/warnings.json
```

La API responde con un archivo que indica qué filas produjeron advertencias, junto con un mensaje que indica por qué falló el registro. El formato del archivo es el mismo que se especifica en el parámetro &quot;format&quot; durante la creación del trabajo. Se anexa un campo adicional a cada registro con una descripción de la advertencia.
