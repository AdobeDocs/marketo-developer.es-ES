---
title: Extracción masiva de objetos personalizados
feature: REST API, Custom Objects
description: Guía de las API de REST de extracción de objetos personalizados en lote de Marketo para exportar objetos personalizados vinculados al posible cliente con filtros de lista y de fecha actualizados, campos seleccionados y...
exl-id: 86cf02b0-90a3-4ec6-8abd-b4423cdd94eb
TQID: https://experienceleague.adobe.com/KAT-vab2uZq8FrRbZLy30PCJNfq01znDDuSSWuIu7WE
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
source-wordcount: 1231
ht-degree: 1%

---

# Extracción masiva de objetos personalizados

[Referencia de extremo de extracción de objeto personalizado masivo](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Custom-Objects)

Las API de REST de extracción de objetos personalizados en lote recuperan grandes conjuntos de registros de objetos personalizados de Marketo. Utilice estas API para el intercambio continuo de datos entre Marketo y sistemas externos, ETL, almacenamiento de datos y archivado.

La API exporta registros de objetos personalizados de Marketo de primer nivel vinculados directamente a posibles clientes. Especifique el nombre del objeto personalizado y una lista de posibles clientes vinculados. Para cada posible cliente, la API escribe los registros de objeto personalizado vinculados coincidentes como filas en el archivo de exportación.

Puede ver datos de objeto personalizados en la [pestaña Objeto personalizado de la página de detalles del posible cliente en la interfaz de usuario de Marketo](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/administration/marketo-custom-objects/understanding-marketo-custom-objects).

## Permisos

El usuario de la API debe tener una función con el permiso de objeto personalizado de solo lectura, el permiso de objeto personalizado de lectura y escritura o ambos.

## Filtros

Los filtros de extracción de objetos personalizados especifican una lista de posibles clientes vinculados al objeto personalizado. Si un posible cliente de la lista está vinculado a registros que coinciden con el nombre de objeto personalizado especificado, la API escribe esos registros en el archivo de exportación.

Especifique solo un tipo de filtro por trabajo de exportación.

| Tipo de filtro | Tipo de datos | Notas |
| --- | --- | --- |
| `updatedAt` | Date Range | Acepta un objeto JSON con los miembros `startAt` y `endAt` &amp;nbsp.;`startAt` acepta una fecha y hora que representa la marca de agua baja y `endAt` acepta una fecha y hora que representa la marca de agua alta. El intervalo debe ser de 31 días o menos. Los trabajos con este tipo de filtro devuelven todos los registros accesibles que se actualizaron dentro del intervalo de fechas. Las horas de la fecha deben estar en formato ISO-8601, sin milisegundos. |
| `staticListName` | Cadena | Acepta el nombre de una lista estática. Los trabajos con este tipo de filtro devuelven todos los registros accesibles que son miembros de la lista estática en el momento en que comienza a procesarse el trabajo. Recupere nombres de listas estáticas utilizando el extremo Obtener listas. |
| `staticListId` | Entero | Acepta el ID de una lista estática. Los trabajos con este tipo de filtro devuelven todos los registros accesibles que son miembros de la lista estática en el momento en que comienza a procesarse el trabajo. Recupere los identificadores de lista estática mediante el extremo Obtener listas. |
| `smartListName`* | Cadena | Acepta el nombre de una lista inteligente. Los trabajos con este tipo de filtro devuelven todos los registros accesibles que son miembros de las listas inteligentes en el momento en que comienza a procesarse el trabajo. Recupere nombres de listas inteligentes utilizando el extremo Obtener listas inteligentes. |
| `smartListId`* | Entero | Acepta el identificador de una lista inteligente. Los trabajos con este tipo de filtro devuelven todos los registros accesibles que son miembros de las listas inteligentes en el momento en que comienza a procesarse el trabajo. Recupere los identificadores de las listas inteligentes mediante el extremo Obtener listas inteligentes. |

Algunas suscripciones no admiten este tipo de filtro. Si no está disponible, el extremo del trabajo Crear posible cliente de exportación devuelve `1035, Unsupported filter type for target subscription`. Póngase en contacto con el Soporte técnico de Marketo para solicitar esta funcionalidad para su suscripción.

## Opciones

El extremo [Crear trabajo de exportación de objeto personalizado](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Custom-Objects/operation/createExportCustomObjectsUsingPOST) proporciona opciones para:

- Especifique los campos que desea incluir en el archivo de exportación.
- Cambie el nombre de los encabezados de columna exportados.
- Especifique el formato del archivo de exportación.

| Parámetro | Tipo de datos | Obligatorio | Notas |
| --- | --- | --- | --- |
| `fields` | Matriz[Cadena] | Sí | Matriz de cadenas que contienen el valor del nombre de atributo de objeto personalizado tal como lo devuelve el extremo Describir objeto personalizado. Los campos enumerados se incluyen en el archivo exportado. |
| `columnHeaderNames` | Objeto | No | Objeto JSON que contiene pares de clave-valor de nombres de campo y encabezado de columna. La clave debe ser el nombre de un campo incluido en el trabajo de exportación. El valor es el nombre del encabezado de columna exportado para ese campo. |
| `format` | Cadena | No | Acepta uno de: CSV, TSV, SSV. El archivo exportado se representa como un archivo de valores separados por comas, valores separados por tabulaciones o valores separados por espacios, respectivamente, si se establece. Si no se establece, el valor predeterminado es CSV. |

## Creación de un trabajo

Use el extremo [Crear trabajo de exportación de objeto personalizado](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Custom-Objects/operation/createExportCustomObjectsUsingPOST) para definir el trabajo de exportación.

La solicitud de utiliza estos parámetros:

- `apiName`: parámetro de ruta de acceso requerido. Especifica el objeto personalizado de Marketo que se va a exportar, utilizando el nombre devuelto por el extremo [Describir objeto personalizado](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/describeUsingGET_1). No se permiten objetos personalizados de CRM.
- `filter`: obligatorio. Especifica los posibles clientes vinculados haciendo referencia a una lista estática o lista inteligente.
- `fields`: obligatorio. Especifica los nombres de API de los atributos de objeto personalizados que se incluirán en el archivo de exportación.
- `format`: Opcional. Especifica el formato del archivo de exportación.
- `columnHeaderNames`: Opcional. Especifica nombres de encabezado de columna de reemplazo.

Este ejemplo utiliza un objeto personalizado `Car` con los campos `Color`, `Make`, `Model` y `VIN`. El campo de vínculo es el ID del posible cliente y el campo de anulación de duplicación es VIN.

Definición de objeto personalizada

![Objeto personalizado](assets/custom-object-car.png)

Campos de objeto personalizados

![Campos de objeto personalizados](assets/custom-object-car-fields.png)

Llame a [Describir objeto personalizado](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/describeUsingGET_1) para inspeccionar los atributos de objeto personalizados mediante programación. La respuesta devuelve los atributos de `fields`.

```http
GET /rest/v1/customobjects/car_c/describe.json
```

```json
{
    "requestId": "148ef#1793e00f64f",
    "result": [
        {
            "name": "car_c",
            "displayName": "Car",
            "description": "It's a car.",
            "createdAt": "2021-05-05T16:14:41Z",
            "updatedAt": "2021-05-05T16:14:42Z",
            "idField": "marketoGUID",
            "dedupeFields": [
                "vIN"
            ],
            "searchableFields": [
                [
                    "vIN"
                ],
                [
                    "marketoGUID"
                ],
                [
                    "leadID"
                ]
            ],
            "relationships": [
                {
                    "field": "leadID",
                    "type": "child",
                    "relatedTo": {
                        "name": "Lead",
                        "field": "Id"
                    }
                }
            ],
            "fields": [
                {
                    "name": "createdAt",
                    "displayName": "Created At",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "marketoGUID",
                    "displayName": "Marketo GUID",
                    "dataType": "string",
                    "length": 36,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "updatedAt",
                    "displayName": "Updated At",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "color",
                    "displayName": "Color",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "leadID",
                    "displayName": "Lead ID",
                    "dataType": "integer",
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "make",
                    "displayName": "Make",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "model",
                    "displayName": "Model",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "vIN",
                    "displayName": "VIN",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                }
            ]
        }
    ],
    "success": true
}
```

Use el extremo [Sincronizar objetos personalizados](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST) para crear registros de objetos personalizados y vincular cada uno a un posible cliente. Un posible cliente se puede vincular a varios registros de objeto personalizados y crear una relación uno a varios.

```http
POST /rest/v1/customobjects/car_c.json
```

```json
{
   "action":"createOrUpdate",
   "input":[
       {
           "leadId": 11,
           "color": "Pearl White",
           "make": "Tesla",
           "model": "Model S",
           "vIN": "5YJSA1E41FF156789"
       },
       {
           "leadId": 12,
           "color": "Midnight Silver Metallic",
           "make": "Tesla",
           "model": "Model X",
           "vIN": "LRWXB2B41FF198765"
       },
       {
           "leadId": 13,
           "color": "Fusion Red",
           "make": "Tesla",
           "model": "Roadster",
           "vIN": "SFGRC3C41FF154321"
       }
    ]
}
```

```json
{
    "requestId": "50d9#1793e066088",
    "result": [
        {
            "seq": 0,
            "marketoGUID": "d911eaa1-fd0b-4a99-9b71-c6a7233c782c",
            "status": "created"
        },
        {
            "seq": 1,
            "marketoGUID": "20d04ffb-51f0-4336-924c-c783b9bb4215",
            "status": "created"
        },
        {
            "seq": 2,
            "marketoGUID": "e7da4331-8e7a-473b-85c8-047638eb6c7f",
            "status": "created"
        }
    ],
    "success": true
}
```

Los tres posibles clientes de este ejemplo pertenecen a la lista estática `Car Buyers`, que tiene un `id` de 1081. Llame al extremo [Obtener posibles clientes por id. de lista](https://developer.adobe.com/marketo-apis/api/mapi#tag/Static-Lists/operation/getLeadsByListIdUsingGET_1) para recuperar los miembros de la lista.

```http
GET /rest/v1/lists/1081/leads.json
```

```json
{
    "requestId": "d023#1793e1e982b",
    "result": [
        {
            "id": 11,
            "firstName": "Hanna",
            "lastName": "Crawford",
            "email": "208161Hanna.Crawford@pookmail.com",
            "updatedAt": "2020-01-16T02:38:22Z",
            "createdAt": "2017-07-27T01:38:42Z"
        },
        {
            "id": 12,
            "firstName": "Bertha",
            "lastName": "Fulton",
            "email": "208160Bertha.Fulton@trashymail.com",
            "updatedAt": "2020-01-16T02:38:22Z",
            "createdAt": "2017-07-27T01:38:42Z"
        },
        {
            "id": 13,
            "firstName": "Faith",
            "lastName": "England",
            "email": "208159Faith.England@dodgit.com",
            "updatedAt": "2020-01-16T02:38:22Z",
            "createdAt": "2017-07-27T01:38:42Z"
        }
    ],
    "success": true
}
```

Para recuperar estos registros, llame al extremo [Crear trabajo de exportación de objeto personalizado](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Custom-Objects/operation/createExportCustomObjectsUsingPOST). Especifique los atributos de objeto personalizados en `fields` y el identificador de lista estática en `filter`.

```http
POST /bulk/v1/customobjects/car_c/export/create.json
```

```json
{
    "fields": [
        "leadId",
        "color",
        "make",
        "model",
        "vIN"
    ],
    "filter": {
        "staticListId": 1081
    }
}
```

```json
{
    "requestId": "8d2f#1793e289e87",
    "result": [
        {
            "exportId": "f2c03f1d-226f-47c1-a557-357af8c2b32a",
            "format": "CSV",
            "status": "Created",
            "createdAt": "2021-05-05T20:12:01Z"
        }
    ],
    "success": true
}
```

La respuesta confirma que el trabajo se ha creado, pero la exportación no se inicia automáticamente. Pase `apiName` y el `exportId` devuelto al extremo [Trabajo de exportación de objetos personalizados en cola](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Custom-Objects/operation/enqueueExportCustomObjectsUsingPOST) para iniciar el trabajo.

```http
POST /bulk/v1/customobjects/car_c/export/f2c03f1d-226f-47c1-a557-357af8c2b32a/enqueue.json
```

```json
{
    "requestId": "cfaf#1793e2a0762",
    "result": [
        {
            "exportId": "f2c03f1d-226f-47c1-a557-357af8c2b32a",
            "format": "CSV",
            "status": "Queued",
            "createdAt": "2021-05-05T20:12:01Z",
            "queuedAt": "2021-05-05T20:13:32Z"
        }
    ],
    "success": true
}
```

La respuesta en cola devuelve inicialmente un estado `Queued`. Cuando una ranura de exportación está disponible, el estado cambia a `Processing`.

## Estado del trabajo de sondeo

Solo puede recuperar el estado para los trabajos creados por el mismo usuario de API.

Dado que la exportación se ejecuta de forma asíncrona, use el extremo [Obtener estado de trabajo de objeto personalizado de exportación](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Custom-Objects/operation/getExportCustomObjectsStatusUsingGET) para sondear su progreso. El estado se actualiza solo una vez cada 60 segundos, por lo que no sondee con más frecuencia.

El estado puede ser `Created`, `Queued`, `Processing`, `Canceled`, `Completed` o `Failed`.

```http
GET /bulk/v1/customobjects/{apiName}/export/{exportId}/status.json
```

```json
{
    "requestId": "14daa#1793e2cf9de",
    "result": [
        {
            "exportId": "f2c03f1d-226f-47c1-a557-357af8c2b32a",
            "format": "CSV",
            "status": "Processing",
            "createdAt": "2021-05-05T20:12:01Z",
            "queuedAt": "2021-05-05T20:13:32Z",
            "startedAt": "2021-05-05T20:14:15Z"
        }
    ],
    "success": true
}
```

Esta respuesta muestra que el trabajo aún se está procesando, por lo que el archivo no está disponible. Cuando el estado del trabajo cambie a `Completed`, el archivo estará listo para descargarse.

```json
{
    "requestId": "14daa#1793e2cf9de",
    "result": [
        {
            "exportId": "f2c03f1d-226f-47c1-a557-357af8c2b32a",
            "format": "CSV",
            "status": "Completed",
            "createdAt": "2021-05-05T20:12:01Z",
            "queuedAt": "2021-05-05T20:13:32Z",
            "startedAt": "2021-05-05T20:14:15Z",
            "finishedAt": "2021-05-05T20:14:28Z",
            "numberOfRecords": 3,
            "fileSize": 182,
            "fileChecksum": "sha256:fac0cabc2352229c12e18b2fde03d1f24178bc71e9e926f520ae8d61bbe98c01"
        }
    ],
    "success": true
}
```

## Recuperación de datos

Para recuperar una exportación de objeto personalizado completada, pase `apiName` y `exportId` al extremo [Obtener archivo de objeto personalizado de exportación](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Custom-Objects/operation/getExportCustomObjectsFileUsingGET).

El punto final devuelve el archivo en el formato configurado para el trabajo. Si un atributo de objeto personalizado solicitado no contiene datos, el campo de exportación correspondiente contiene `null`.

```http
GET /bulk/v1/customobjects/car_c/export/f2c03f1d-226f-47c1-a557-357af8c2b32a/file.json
```

```csv
leadId,color,make,model,vIN
11,Pearl White,Tesla,Model S,5YJSA1E41FF156789
12,Midnight Silver Metallic,Tesla,Model X,LRWXB2B41FF198765
13,Fusion Red,Tesla,Roadster,SFGRC3C41FF154321
```

Para la recuperación parcial o reanudable, el extremo de archivo admite el encabezado HTTP `Range` opcional con un tipo de intervalo de `bytes`. Si no establece el encabezado, el extremo devolverá todo el archivo. Para obtener más información, consulte [Extracción en lotes](bulk-extract.md).

## Cancelación de un trabajo

Para cancelar un trabajo configurado incorrectamente o que ya no se necesita, llame al extremo [Cancelar trabajo de exportar objetos personalizados](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Custom-Objects/operation/getExportCustomObjectsFileUsingPOST). El estado de respuesta indica que el trabajo se ha cancelado.

```http
POST /bulk/v1/customobjects/car_c/export/f2c03f1d-226f-47c1-a557-357af8c2b32a/cancel.json
```

```json
{
    "requestId": "e5f9#179391286a7",
    "result": [
        {
            "exportId": "4a8cdd80-0d16-4dd6-9923-6ec97e30e91b",
            "format": "CSV",
            "status": "Cancelled",
            "createdAt": "2021-05-04T20:24:33Z"
        }
    ],
    "success": true
}
```
