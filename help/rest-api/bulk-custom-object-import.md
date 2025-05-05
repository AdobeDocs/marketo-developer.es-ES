---
title: Importación masiva de objetos personalizados
feature: Custom Objects
description: Importación por lotes de objetos personalizados.
exl-id: e795476c-14bc-4e8c-b611-1f0941a65825
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '855'
ht-degree: 0%

---

# Importación masiva de objetos personalizados

[Referencia de extremo de importación de objetos personalizados en lotes](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Custom-Objects)

Cuando tiene muchos registros de objeto personalizados para  Para la importación, se recomienda importarlos de forma asíncrona mediante la API en bloque. Para ello, importe un archivo plano que contenga registros delimitados (coma, tabulación o punto y coma). El archivo puede contener cualquier número de registros, siempre que su tamaño sea inferior a 10 MB (de lo contrario, un archivo HTTP  413 (se devuelve el código de estado). El contenido del archivo depende de la definición de objeto personalizada. La primera fila siempre contiene un encabezado que enumera los campos a los que asignar valores de cada fila. Todos los nombres de campo del encabezado deben coincidir con un nombre de API (como se describe a continuación). Las filas restantes contienen los datos que se van a importar, un registro por fila. La operación de registro es sólo &quot;insertar o actualizar&quot;.

## Límites de procesamiento

Se le permite enviar más de una solicitud de importación masiva, dentro de los límites. Cada solicitud se agrega como un trabajo a una cola FIFO para su procesamiento. Se procesan un máximo de dos trabajos al mismo tiempo. Se permite un máximo de diez trabajos en cola en un momento determinado (incluidos los dos que se están procesando actualmente). Si supera el máximo de diez trabajos, se devuelve el error &quot;1016, Demasiadas importaciones&quot;.

## Ejemplo de objeto personalizado

Antes de usar la API en bloque, primero debes usar la IU de administración de Marketo para [crear tu objeto personalizado](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/administration/marketo-custom-objects/create-marketo-custom-objects). Por ejemplo, supongamos que hemos creado un objeto personalizado &quot;Coche&quot; con los campos &quot;Color&quot;, &quot;Marca&quot;, &quot;Modelo&quot; y &quot;VIN&quot;. A continuación se muestran las pantallas de la IU de administración que muestran el objeto personalizado. Puede ver que hemos utilizado el campo VIN para la deduplicación. Los nombres de las API se resaltan, ya que deben utilizarse al llamar a extremos relacionados con API masivas.

![Insertar objeto personalizado](assets/bulk-insert-co-car-1.png)

Estos son los campos de objeto personalizados tal como se presentan en la IU de administración.

![Insertar campos de objeto personalizados](assets/bulk-insert-co-car-fields.png)

### Nombres de API

Puede recuperar nombres de API mediante programación pasando el nombre de API de objeto personalizado al extremo [Describir objeto personalizado](#describe).

```
/rest/v1/customobjects/{apiName}/describe.json
```

```json
{
    "requestId": "46ff#15a686e66de",
    "result": [
        {
            "name": "car_c",
            "displayName": "Car",
            "description": "It's a car.",
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

Ahora supongamos que desea importar tres registros de objetos personalizados &quot;Car&quot;. Si utiliza el formato delimitado por comas (CSV), el archivo podría tener el aspecto siguiente:

```
color,make,model,vin
red,bmw,2002,WBA4R7C55HK895912
yellow,bmw,320i,WBA4R7C30HK896061
blue,bmw,325i,WBS3U9C52HP970604
```

La línea 1 es el encabezado y las líneas 2-4 son los registros de datos de objeto personalizados.

## Creación de un trabajo

Para realizar la solicitud de importación masiva, debe incluir el nombre de API del objeto personalizado en la ruta del extremo [Importar objetos personalizados](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Identity/operation/identityUsingPOST). También debe incluir un parámetro &quot;file&quot; que haga referencia al nombre del archivo de importación y un parámetro &quot;format&quot; que especifique cómo se delimita el archivo de importación (&quot;csv&quot;, &quot;tsv&quot; o &quot;ssv&quot;).

```
POST /bulk/v1/customobjects/{apiName}/import.json?format=csv
```

```
Transfer-Encoding: chunked
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryXjWP6BP8Ciq6bPeo
Content-Length: 290
Host: <munchkinId>.mktorest.com
```

```
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

En este ejemplo, especificamos el formato &quot;csv&quot; y nombramos nuestro archivo de importación &quot;custom_object_import.csv&quot;.

Tenga en cuenta que en la respuesta a nuestra llamada, no hay una lista de éxitos o errores como la que obtendría del punto de conexión Sincronizar objetos personalizados. En su lugar, recibirá un `batchId`. Esto se debe a que la llamada es asincrónica y puede devolver un `status` de &quot;En cola&quot;, &quot;Importando&quot; o &quot;Error&quot;. Debe conservar batchId para poder obtener el estado del trabajo de importación o recuperar errores y/o advertencias al finalizar. batchId sigue siendo válido durante siete días.

Una manera sencilla de replicar la solicitud de importación masiva es utilizar curl desde la línea de comandos:

```
curl -X POST -i -F format='csv' -F file='@custom_object_import.csv' -F access_token='<Access Token>' <REST API Endpoint URL>/bulk/v1/customobjects/car_c/import.json
```

Donde el archivo de importación &quot;custom_object_import.csv&quot; contiene lo siguiente:

```
color,make,model,vin
red,bmw,2002,WBA4R7C55HK895912
yellow,bmw,320i,WBA4R7C30HK896061
blue,bmw,325i,WBS3U9C52HP970604
```

## Estado del trabajo de sondeo

Una vez creado el trabajo de importación, debe consultar su estado. Se recomienda sondear el trabajo de importación cada 5-30 segundos. Para ello, pase el nombre de API del objeto personalizado y `batchId` en la ruta de acceso al extremo [Obtener estado de objeto personalizado de importación](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectStatusUsingGET).

```
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

Esta respuesta muestra una importación completada, pero `status` puede ser una de las siguientes: Completado, En cola, Importando, Error. Si el trabajo se ha completado, tiene un listado del número de filas procesadas, con errores y con advertencias. El atributo de mensaje también es un buen lugar para buscar información de trabajo adicional.

## Errores de

Los errores están indicados por el atributo `numOfRowsFailed` en la respuesta [Obtener estado de objeto personalizado de importación](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectStatusUsingGET). Si numOfRowsFailed es mayor que cero, ese valor indica el número de errores que se produjeron. Llame al extremo [Obtener errores de importación de objeto personalizado](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectFailuresUsingGET) para obtener un archivo con detalles de error. De nuevo, debe pasar el nombre de API de objeto personalizado y `batchId` en la ruta de acceso. Si no existe ningún archivo de error, se devuelve un código de estado HTTP 404.

Continuando con el ejemplo, podemos forzar un error modificando el encabezado y cambiando &quot;vin&quot; a &quot; vin&quot; (añadiendo un espacio entre la coma y &quot;vin&quot;).

```
color,make,model, vin
```

Cuando volvemos a importar y comprobamos el estado, vemos esta respuesta con `numRowsFailed`: 3. Esto indica tres errores.

```
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

Ahora realizamos la llamada al extremo de obtener errores de importación de objeto personalizado para obtener detalles de error adicionales:

```
GET /bulk/v1/customobjects/car_c/import/{batchId}/failures.json
```

```
color,make,model, vin,Import Failure Reason
red,bmw,2002,WBA4R7C55HK895912,missing.dedupe.fields
yellow,bmw,320i,WBA4R7C30HK896061,missing.dedupe.fields
blue,bmw,325i,WBS3U9C52HP970604,missing.dedupe.fields
```

Y podemos ver que nos falta nuestro campo de deduplicación `vin`.

## Advertencias

Las advertencias se indican mediante el atributo `numOfRowsWithWarning` en la respuesta Obtener estado de objeto personalizado de importación. Si numOfRowsWithWarning es mayor que cero, ese valor indica el número de advertencias que se produjeron. Llame al extremo [Get Import Custom Object Warnings](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectWarningsUsingGET) para obtener un archivo con detalles de advertencia. De nuevo, debe pasar el nombre de API de objeto personalizado y `batchId` en la ruta de acceso. Si no existe ningún archivo de advertencia, se devuelve un código de estado HTTP 404.

```
GET /bulk/v1/customobjects/car_c/import/{batchId}/warnings.json
```
