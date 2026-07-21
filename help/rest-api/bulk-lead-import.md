---
title: Importación masiva de posibles clientes
feature: REST API
description: Cree y supervise importaciones asíncronas masivas de posibles clientes en Marketo con CSV o CSV.
exl-id: 615f158b-35f9-425a-b568-0a7041262504
TQID: https://experienceleague.adobe.com/UamXYWis5J1ERqnp5lAnfUf3pFcgfSOLfKRXRB-Yg4I
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: e2290edd-b061-4880-9d79-dee306cf5aa9
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 623
ht-degree: 1%

---

# Importación masiva de posibles clientes

[Referencia de extremo de importación masiva de posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Leads)

Use la [API en bloque](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Leads/operation/importLeadUsingPOST) para importar grandes cantidades de registros de posibles clientes de forma asíncrona. Proporcione los registros en un archivo plano delimitado por comas, tabuladores o puntos y comas que tenga menos de 10 MB.

La importación masiva de posibles clientes solo admite la operación de registro &quot;insertar o actualizar&quot;.

## Límites de procesamiento

Cada solicitud de importación masiva se agrega como un trabajo a una cola FIFO (primera entrada, primera salida). Se aplican los límites siguientes:

- Se puede procesar simultáneamente un máximo de dos trabajos.
- Se pueden colocar un máximo de 10 trabajos en cola, incluidos los dos trabajos que se están procesando.

Si supera el máximo de 10 trabajos, la API devuelve un error de `1016, Too many imports`.

## Importar archivo

La primera fila del archivo debe ser un encabezado que enumere los campos de API de REST a los que se asignan los valores de cada fila. Un archivo típico sigue este patrón:

```csv
email,firstName,lastName
test@example.com,John,Doe
```

Use `externalCompanyId` para vincular un registro de posible cliente a un registro de empresa. Use `externalSalesPersonId` para vincular un registro de posible cliente a un registro de vendedor.

Envíe la solicitud utilizando el tipo de contenido `multipart/form-data`. Utilice una implementación de biblioteca existente para construir la solicitud de varias partes.

## Creación de un trabajo

Para crear un trabajo de importación masiva, establezca el tipo de contenido en `multipart/form-data` e incluya estos parámetros:

- `file`: el contenido del archivo de importación.
- `format`: el formato de archivo. Los valores válidos son `csv`, `tsv` y `ssv`.

```http
POST /bulk/v1/leads.json?format=csv
```

```text
Content-Type: multipart/form-data; boundary=------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Length: 311
Host: <munchkinId>.mktorest.com
```

```text
------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Disposition: form-data; name="file"; filename="leads.csv"
Content-Type: text/csv

firstName,lastName,email,company
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

Este extremo usa [multipart/form-data como tipo de contenido](https://www.w3.org/Protocols/rfc1341/7_2_Multipart.html). Utilice una biblioteca de soporte HTTP para el idioma preferido para construir la solicitud correctamente. El siguiente ejemplo utiliza cURL desde la línea de comandos:

```bash
curl -i -F format=csv -F file=@lead_data.csv -F access_token=<Access Token> <REST API Endpoint Base URL>/bulk/v1/leads.json
```

En este ejemplo, el archivo de importación `lead_data.csv` contiene los datos siguientes:

```text
firstName,lastName,email,company
Able,Baker,ablebaker@marketo.com,Marketo
Charlie,Dog,charliedog@marketo.com,Marketo
Easy,Fox,easyfox@marketo.com,Marketo
```

También puede incluir estos parámetros opcionales:

- `lookupField`: selecciona el campo usado para la deduplicación y el valor predeterminado es `email`. Especifique `id` para realizar una operación de &quot;solo actualización&quot;.
- `listId`: selecciona una lista estática. Los posibles clientes importados se convierten en miembros de esta lista, además de los registros creados o actualizados por la importación.
- `partitionName`: selecciona la partición a la que se va a importar. Consulte la sección Espacios de trabajo y particiones para obtener más información.

Como la API es asincrónica, la respuesta contiene `batchId` y `status` campos en lugar de éxitos y errores individuales. El estado puede ser `Queued`, `Importing` o `Failed`.

Conservar `batchId` para comprobar el estado del trabajo y recuperar errores o advertencias una vez finalizado. `batchId` sigue siendo válido por siete días.

## Estado del trabajo de sondeo

Utilice la API Obtener estado del posible cliente de importación para sondear el trabajo cada 5-30 segundos, según los requisitos de latencia y las limitaciones de llamadas de la API.

```http
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

Esta respuesta muestra una importación completada. El estado puede ser uno de los siguientes valores:

- Completo
- En cola
- Importando
- Error

Cuando se completa el trabajo, la respuesta muestra los números de filas procesadas, fallidas y procesadas con advertencias. El parámetro `message` también puede proporcionar un mensaje de error cuando el estado es `Failed`.

## Errores de

El atributo `numOfRowsFailed` de la respuesta Obtener estado del posible cliente de importación indica el número de filas con errores. Un valor mayor que cero significa que se produjeron errores.

Para recuperar los registros con errores y sus causas, solicite el archivo de error:

```http
GET /bulk/v1/leads/batch/{id}/failures.json
```

La API devuelve un archivo que identifica cada fila fallida y explica por qué falló el registro. El archivo utiliza el formato especificado por el parámetro `format` durante la creación del trabajo. Un campo adicional en cada registro describe el error.

## Advertencias

El atributo `numOfRowsWithWarning` de la respuesta Obtener estado del posible cliente de importación indica el número de filas con advertencias. Un valor mayor que cero significa que se han producido advertencias.

Para recuperar los registros afectados y sus causas, solicite el archivo de advertencia:

```http
GET /bulk/v1/leads/batch/{id}/warnings.json
```

La API devuelve un archivo que identifica cada fila con una advertencia y explica por qué se produjo la advertencia. El archivo utiliza el formato especificado por el parámetro `format` durante la creación del trabajo. Un campo adicional en cada registro describe la advertencia.
