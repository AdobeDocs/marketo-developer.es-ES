---
title: Importación masiva de objetos personalizados
feature: Custom Objects
description: Obtenga información sobre cómo importar por lotes objetos personalizados de Marketo a través de REST mediante archivos CSV, TSV o SSV.
exl-id: e795476c-14bc-4e8c-b611-1f0941a65825
TQID: https://experienceleague.adobe.com/C1LKLZDEvv95XXH3AEoxIXsLK55tgKTrvyxvs4LnYWw
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: d1d0a9cd-295d-4976-8c39-ddae266f240e
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 736
ht-degree: 0%

---

# Importación masiva de objetos personalizados

[Referencia de extremo de importación de objeto personalizado masivo](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Custom-Objects)

Utilice la API masiva para importar grandes cantidades de registros de objetos personalizados de forma asíncrona. Proporcione los registros en un archivo plano delimitado por comas, tabuladores o puntos y comas que tenga menos de 10 MB. Si el archivo es más grande, la API devuelve un código de estado HTTP 413.

El contenido del archivo depende de la definición de objeto personalizada. La primera fila debe ser un encabezado, y cada campo de encabezado debe coincidir con un nombre de API. Cada fila restante contiene un registro.

La importación masiva de objetos personalizados solo admite la operación de registro &quot;insertar o actualizar&quot;.

## Límites de procesamiento

Cada solicitud de importación masiva se agrega como un trabajo a una cola FIFO (primera entrada, primera salida). Se aplican los límites siguientes:

- Se puede procesar simultáneamente un máximo de dos trabajos.
- Se pueden colocar un máximo de 10 trabajos en cola, incluidos los dos trabajos que se están procesando.

Si supera el máximo de 10 trabajos, la API devuelve un error de `1016, Too many imports`.

## Ejemplo de objeto personalizado

Antes de usar la API en bloque, usa la IU de administración de Marketo para [crear tu objeto personalizado](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/administration/marketo-custom-objects/create-marketo-custom-objects).

Este ejemplo utiliza un objeto personalizado `Car` con los campos `Color`, `Make`, `Model` y `VIN`. El campo VIN se utiliza para la deduplicación. Las pantallas de la IU de administración resaltan los nombres de API requeridos por los extremos de API masivos.

![Insertar objeto personalizado](assets/bulk-insert-co-car-1.png)

Estos son los campos de objeto personalizados tal como se presentan en la IU de administración.

![Insertar campos de objeto personalizados](assets/bulk-insert-co-car-fields.png)

### Nombres de API

Para recuperar nombres de API mediante programación, pase el nombre de API de objeto personalizado al extremo [Describir objeto personalizado](#describe).

```text
/rest/v1/customobjects/{apiName}/describe.json
```

```json
{
    "requestId": "46ff#15a686e66de",
    "result": [
        {
            "name": "car_c",
            "displayName": "Car",
            "description": "It is a car.",
            "createdAt": "2017-02-22T19:55:51Z",
            "updatedAt": "2017-02-22T19:55:51Z",
            "idField": "marketoGUID",
            "dedupeFields": [
                "vin"
            ],
            "searchableFields": [
                [
                    "vin"
                ],
                [
                    "marketoGUID"
                ]
            ],
            "fields": [
                {
                    "name": "createdAt",
                    "displayName": "Created At",
                    "dataType": "datetime",
                    "updateable": false
                },
                {
                    "name": "marketoGUID",
                    "displayName": "Marketo GUID",
                    "dataType": "string",
                    "length": 36,
                    "updateable": false
                },
                {
                    "name": "updatedAt",
                    "displayName": "Updated At",
                    "dataType": "datetime",
                    "updateable": false
                },
                {
                    "name": "color",
                    "displayName": "Color",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true
                },
                {
                    "name": "make",
                    "displayName": "Make",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true
                },
                {
                    "name": "model",
                    "displayName": "Model",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true
                },
                {
                    "name": "vin",
                    "displayName": "VIN",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true
                }
            ]
        }
    ],
    "success": true
}
```

### Importar archivo

El siguiente archivo CSV contiene tres registros de objeto personalizados `Car`:

```text
color,make,model,vin
red,bmw,2002,WBA4R7C55HK895912
yellow,bmw,320i,WBA4R7C30HK896061
blue,bmw,325i,WBS3U9C52HP970604
```

La primera línea es el encabezado. Las líneas 2-4 contienen los registros de datos de objeto personalizados.

## Creación de un trabajo

Para crear el trabajo de importación masiva, incluya el nombre de la API de objeto personalizada en la ruta del extremo [Importar objetos personalizados](https://developer.adobe.com/marketo-apis/api/mapi#tag/Identity/operation/identityUsingPOST). Incluya estos parámetros:

- `file`: nombre del archivo de importación.
- `format`: el formato de delimitador de archivo (`csv`, `tsv` o `ssv`).

```http
POST /bulk/v1/customobjects/{apiName}/import.json?format=csv
```

```text
Transfer-Encoding: chunked
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryXjWP6BP8Ciq6bPeo
Content-Length: 290
Host: <munchkinId>.mktorest.com
```

```text
------WebKitFormBoundaryXjWP6BP8Ciq6bPeo
Content-Disposition: form-data; name="file"; filename="custom_object_import.csv"
Content-Type: text/csv

color,make,model,vin
red,bmw,2002,WBA4R7C55HK895912
yellow,bmw,320i,WBA4R7C30HK896061
blue,bmw,325i,WBS3U9C52HP970604
------WebKitFormBoundaryXjWP6BP8Ciq6bPeo--
```

```json
{
    "requestId": "c015#15a68a23418",
    "result": [
        {
            "batchId": 1013,
            "status": "Queued",
            "objectApiName": "car_c"
        }
    ],
    "success": true
}
```

Este ejemplo especifica el formato `csv` y asigna un nombre al archivo de importación `custom_object_import.csv`.

Como la llamada es asincrónica, la respuesta contiene `batchId` en lugar de los éxitos y errores individuales devueltos por el extremo de objetos personalizados de sincronización. `status` puede ser `Queued`, `Importing` o `Failed`.

Conserve `batchId` para comprobar el estado de la importación y recuperar errores o advertencias una vez finalizada. `batchId` sigue siendo válido por siete días.

La siguiente solicitud cURL de la línea de comandos envía el trabajo de ejemplo:

```bash
curl -X POST -i -F format='csv' -F file='@custom_object_import.csv' -F access_token='<Access Token>' <REST API Endpoint URL>/bulk/v1/customobjects/car_c/import.json
```

En este ejemplo, el archivo `custom_object_import.csv` contiene los siguientes datos:

```text
color,make,model,vin
red,bmw,2002,WBA4R7C55HK895912
yellow,bmw,320i,WBA4R7C30HK896061
blue,bmw,325i,WBS3U9C52HP970604
```

## Estado del trabajo de sondeo

Después de crear el trabajo de importación, sondee cada 5-30 segundos. Pase el nombre de API de objeto personalizado y `batchId` en la ruta de acceso al extremo [Obtener estado de objeto personalizado de importación](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectStatusUsingGET).

```http
GET /bulk/v1/customobjects/{apiName}/import/{batchId}/status.json
```

```json
{
    "requestId": "2a5#15a68dd9be1",
    "result": [
        {
            "batchId": 1013,
            "operation": "import",
            "status": "Complete",
            "objectApiName": "car_c",
            "numOfObjectsProcessed": 3,
            "numOfRowsFailed": 0,
            "numOfRowsWithWarning": 0,
            "importTime": "2 second(s)",
            "message": "Import succeeded, 3 records imported (3 members)"
        }
    ],
    "success": true
}
```

Esta respuesta muestra una importación completada. `status` puede ser `Complete`, `Queued`, `Importing` o `Failed`.

Cuando se completa el trabajo, la respuesta muestra los números de filas procesadas, fallidas y procesadas con advertencias. El atributo `message` puede proporcionar información de trabajo adicional.

## Errores de

El atributo `numOfRowsFailed` de la respuesta [Obtener estado de objeto personalizado de importación](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectStatusUsingGET) indica el número de filas con errores. Un valor mayor que cero significa que se produjeron errores.

Pase el nombre de API de objeto personalizado y `batchId` en la ruta de acceso al extremo [Obtener errores de importación de objeto personalizado](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectFailuresUsingGET). El extremo devuelve un archivo con detalles del error. Si no existe ningún archivo de error, devuelve un código de estado HTTP 404.

Para mostrar un error, modifique el encabezado cambiando `vin` a ` vin` y agregando un espacio entre la coma y `vin`.

```text
color,make,model, vin
```

Después de volver a importar el archivo, la respuesta de estado muestra `numRowsFailed`: 3, lo que indica tres errores.

```http
GET /bulk/v1/customobjects/car_c/import/{batchId}/status.json
```

```json
{
    "requestId": "12260#15a68f491ed",
    "result": [
        {
            "batchId": 1016,
            "operation": "import",
            "status": "Complete",
            "objectApiName": "car_c",
            "numOfObjectsProcessed": 0,
            "numOfRowsFailed": 3,
            "numOfRowsWithWarning": 0,
            "importTime": "1 second(s)",
            "message": "Import completed with errors, 0 records imported (0 members), 3 failed"
        }
    ],
    "success": true
}
```

Llame al extremo de obtener errores de importación de objetos personalizados para obtener más información:

```http
GET /bulk/v1/customobjects/car_c/import/{batchId}/failures.json
```

```text
color,make,model, vin,Import Failure Reason
red,bmw,2002,WBA4R7C55HK895912,missing.dedupe.fields
yellow,bmw,320i,WBA4R7C30HK896061,missing.dedupe.fields
blue,bmw,325i,WBS3U9C52HP970604,missing.dedupe.fields
```

La respuesta muestra que falta el campo de deduplicación `vin`.

## Advertencias

El atributo `numOfRowsWithWarning` de la respuesta Obtener estado de objeto personalizado de importación indica el número de filas con advertencias. Un valor mayor que cero significa que se han producido advertencias.

Pase el nombre de API de objeto personalizado y `batchId` en la ruta de acceso al extremo [Obtener advertencias de objeto personalizado de importación](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectWarningsUsingGET). El extremo devuelve un archivo con detalles de advertencia. Si no existe ningún archivo de advertencia, devuelve un código de estado HTTP 404.

```http
GET /bulk/v1/customobjects/car_c/import/{batchId}/warnings.json
```
