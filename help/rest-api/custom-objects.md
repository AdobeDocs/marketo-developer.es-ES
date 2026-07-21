---
title: Objetos personalizados
feature: REST API, Custom Objects
description: Obtenga información sobre cómo crear y administrar objetos personalizados de Marketo mediante la API de REST, incluidos extremos de lista y descripción, metadatos, relaciones, campos y consultas.
exl-id: 88e8829b-f8f1-46d7-a753-5aa6e20e2c40
TQID: https://experienceleague.adobe.com/NWm9CjFVqQdVDJRrnE4nA299-Lg53-JR7xvY-82dUqY
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
  - id: d1d0a9cd-295d-4976-8c39-ddae266f240e
subfeature_v2:
  - id: ea4e3ff5-e7b9-4b4c-a5a0-dc27cc3f4275
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 2938
ht-degree: 0%

---

# Objetos personalizados

[**Referencia de extremo de objeto personalizado**](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects)

Los objetos personalizados de Marketo se pueden relacionar con objetos estándar de Marketo, como posibles clientes y empresas, u otros objetos personalizados de Marketo. Cree objetos personalizados de Marketo en la [interfaz de usuario de Marketo](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/create-marketo-custom-objects) o use la API de metadatos de objeto personalizado que se describe en este documento.

El acceso a la API de metadatos de objeto personalizada requiere un tipo de suscripción de Marketo adecuado. Póngase en contacto con su CSM para obtener más información.

## Lista

Además de las llamadas estándar de describir, consultar, actualizar y eliminar para los objetos de base de datos de posibles clientes, los objetos personalizados proporcionan una [llamada de lista](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/getCustomObjectsUsingGET). El extremo devuelve los objetos personalizados disponibles en la instancia de destino y los metadatos sobre cada objeto.

```http
GET /rest/v1/customobjects.json
```

```json
{
   "requestId":"185d6#14b51985ff0",
   "success":true,
   "result":[
      {
         "name":"Car",
         "displayName":"Car",
         "description":"Car owner",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:24Z",
         "idField":"marketoGUID",
         "dedupeFields":["vin"],
         "searchableFields":[
            ["vin"],
            ["marketoGUID"],
            ["siebelId"]
         ],
         "relationships":[
            {
               "field":"siebelId",
               "type":"parent",
               "relatedTo":{
                  "name":"Lead",
                  "field":"siebelId"
               }
            }
         ]
      }
   ]
}
```

La respuesta enumera las relaciones para cada objeto. Cada relación contiene:

- `field`: campo del objeto que contiene el valor del vínculo.
- `type`: si el objeto relacionado es un objeto principal o secundario.
- `relatedTo`: nombre del objeto relacionado y su campo de vínculo.

## Describir

La llamada [Describir](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/describeUsingGET_1) para los objetos personalizados sigue el mismo patrón que Oportunidades y Compañías, con dos adiciones:

- El parámetro de ruta de acceso `apiName` especifica el nombre de API del tipo de objeto personalizado que se va a describir.
- La respuesta incluye una matriz `relationships` que enumera las relaciones disponibles para el tipo de objeto personalizado.

```http
GET /rest/v1/customobjects/{apiName}/describe.json
```

```json
{
   "requestId":"185d6#14b51985ff0",
   "success":true,
   "result":[
      {
         "name":"Car",
         "displayName":"Car",
         "description":"Car owner",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:24Z",
         "idField":"marketoGUID",
         "dedupeFields":["vin"],
         "searchableFields":[
            ["vin"],
            ["marketoGUID"],
            ["siebelId"]
         ],
         "relationships":[
            {
               "field":"siebelId",
               "type":"parent",
               "object":{
                  "name":"Lead",
                  "field":"siebelId"
               }
            }
         ],
         "fields":[
            {
               "name":"marketoGUID",
               "displayName":"Marketo GUID",
               "dataType":"string",
               "length":36,
               "updateable":false
            },
            {
               "name":"createdAt",
               "displayName":"Created At",
               "dataType":"datetime",
               "updateable":false
            },
            {
               "name":"updatedAt",
               "displayName":"Updated At",
               "dataType":"datetime",
               "updateable":false
            },
            {
               "name":"vin",
               "displayName":"VIN",
               "description":"Vehicle Identification Number",
               "dataType":"string",
               "length":36,
               "updateable":false
            },
            {
               "name":"siebelId",
               "displayName":"External Id",
               "description":"External Id",
               "dataType":"string",
               "length":36,
               "updateable":true
            },
            {
               "name":"make",
               "displayName":"Make",
               "dataType":"string",
               "length":36,
               "updateable":true
            },
            {
               "name":"model",
               "displayName":"Model",
               "description":"Vehicle Model",
               "dataType":"string",
               "length":255,
               "updateable":true
            },
            {
               "name":"year",
               "displayName":"Year",
               "dataType":"integer",
               "updateable":true
            },
            {
               "name":"color",
               "displayName":"Color",
               "description":"Vehicle color",
               "dataType":"String",
               "length": 255,
               "updateable":true
            }
         ]
      }
   ]
}
```

## Consulta

[La consulta de objetos personalizados](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/getCustomObjectsUsingGET) difiere ligeramente de la consulta de otros objetos de base de datos de posibles clientes. Como con Describir, la solicitud toma un parámetro de ruta de acceso `apiName`.

Para un filterType normal, envíe una petición GET con los parámetros `filterType` y `filterValues` necesarios. También puede incluir los parámetros opcionales `**fields**`, `batchSize` y `nextPageToken`.

Cuando se solicita una lista de campos, un campo solicitado que no se devuelve tiene un valor implícito de nulo.

```http
GET /rest/v1/customobjects/{apiName}.json?filterType=idField&filterValues=dff23271-f996-47d7-984f-f2676861b5fa,dff23271-f996-47d7-984f-f2676861b5fb
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fa",
         "vin":"19UYA31581L000000",
         "createdAt":"2015-02-23T18:21:53Z",
         "updatedAt":"2015-02-23T18:23:41Z"
      },
      {
         "seq":1,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb",
         "vin":"29UYA31581L000000",
         "createdAt":"2015-02-23T18:21:53Z",
         "updatedAt":"2015-02-23T18:23:41Z"
      },
   ]
}
```

Al consultar con claves compuestas, la API se comporta como la API de funciones de oportunidad y acepta una solicitud POST con un cuerpo JSON. El cuerpo puede contener los mismos miembros que una consulta GET excepto `filterValues`.

En lugar de filtrar valores, proporcione una matriz de objetos `input`. Cada objeto contiene un miembro para cada campo en el `dedupeFields` del tipo de objeto.

```http
POST /rest/v1/customobjects/{apiName}.json?_method=GET
```

```json
{
   "filterType":"dedupeFields",
   "fields":[
      "marketoGuid",
      "Bedrooms",
      "yearBuilt"
   ],
   "input":[
      {
         "mlsNum":"1962352",
         "houseOwnerId":"42645756"
      },
      {
         "mlsNum":"2962352",
         "houseOwnerId":"52645756"
      },
      {
         "mlsNum":"3962352",
         "houseOwnerId":"62645756"
      }
   ]
}
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fa",
         "Bedrooms":3,
         "yearBuilt":1948,
         "createdAt":"2015-02-23T18:21:53Z",
         "updatedAt":"2015-02-23T18:23:41Z"
      },
      {
         "seq":1,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb",
         "Bedrooms":4,
         "yearBuilt":1956,
         "createdAt":"2015-02-23T18:21:53Z",
         "updatedAt":"2015-02-23T18:23:41Z"
      },
      {
         "seq":2,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fc",
         "Bedrooms":3,
         "yearBuilt":2001,
         "createdAt":"2015-02-23T18:21:53Z",
         "updatedAt":"2015-02-23T18:23:41Z"
      }
   ]
}
```

## Crear y actualizar

Use el extremo [Sincronizar objetos personalizados](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST) para crear o actualizar objetos personalizados. Especifique la operación con el parámetro `action`. Cada llamada puede crear o actualizar hasta 300 registros.

Base los valores de la matriz `input` en la información devuelta por el extremo [Describir objetos personalizados](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/endpoint-reference#!/Custom_Objects/describeUsingGET_1). En el objeto de automóvil de ejemplo, el único campo de desduplicación es `vin`. Cuando utilice el modo deduplicar campos para crear o actualizar registros, incluya al menos un campo `vin` en cada objeto de la matriz de entrada.

```http
POST /rest/v1/customobjects/{apiName}.json
```

```json
{
   "action":"updateOnly",
   "dedupeBy":"dedupeFields",
   "input":[
      {
         "vin":"19UYA31581L000000",
         "siebelId":"f2676861b5fb",
         "make":"BMW",
         "model":"3-Series 330i",
         "year":2003
      },
      {
         "vin":"29UYA31581L000000",
         "siebelId":"f2676861b5fc",
         "make":"BMW",
         "model":"3-Series 330i",
         "year":2003
      },
      {
         "vin":"39UYA31581L000000",
         "siebelId":"f2676861b5fd",
         "make":"BMW",
         "model":"3-Series 330i",
         "year":2003
      }
   ]
}
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "status": "updated",
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb"
      },
      {
         "seq":1,
         "status": "created",
         "marketoGUID":"cff23271-f996-47d7-984f-f2676861b5fb"
      },
      {
         "seq":2,
         "status": "skipped"
         "reasons":[
            {
               "code":"1004",
               "message":"Lead not found"
            }
         ]
      }
   ]
}
```

Cuando actualiza registros en modo `idField`, `idField` siempre es `marketoGUID`. Incluir un campo `marketoGUID` en cada registro.

Dado que este campo está administrado por el sistema, `idField` es válido solamente para el tipo de acción updateOnly. La matriz de resultados incluye el **estado** de cada registro. También incluye un `marketoGUID` para una operación correcta o una matriz `reasons` para una operación fallida.

## Eliminar

Para [eliminar registros](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/deleteCustomObjectsUsingPOST), seleccione un modo `deleteBy` de `idField` o `dedupeFields`. Incluya los campos correspondientes en cada registro de la matriz `input`. Cada llamada permite un máximo de 300 registros.

```http
POST /rest/v1/customobjects/{apiName}/delete.json
```

```json
{
   "deleteBy":"dedupeFields",
   "input":[
      {
         "vin":"19UYA31581L000000"
      },
      {
         "vin":"29UYA31581L000000"
      },
      {
         "vin":"39UYA31581L000000"
      }
   ]
}

{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb",
         "status": "deleted"
      },
      {
         "seq":1,
         "marketoGUID":"da42707c-4dc4-4fc1-9fef-f30a3017240a",
         "status": "deleted"
      },
      {
         "seq":2,
         "status": "skipped"
         "reasons":[
            {
               "code":"1013",
               "message":"Object not found"
            }
         ]
      }
   ]
}
```

Al igual que con las actualizaciones, el resultado contiene un estado para cada registro. También incluye una matriz `marketoGUID` para una eliminación correcta o una matriz `reasons` para una eliminación fallida.

## Tipos de objetos personalizados

La API de metadatos de objeto personalizada permite administrar de forma remota esquemas de objeto personalizados. Utilícelo para crear un Tipo de objeto personalizado o modificar uno existente. Después de crear o modificar un tipo, debe aprobarlo antes de utilizarlo.

Para obtener más información, consulte la [documentación personalizada del producto](https://experienceleague.adobe.com/es/docs/marketo/using/home).

- No puede modificar los tipos de objetos personalizados creados por la API en la interfaz de usuario de Marketo.
- El número máximo de tipos de objetos personalizados es de 10.
- El número máximo de campos de objeto personalizados es 50 por tipo.
- Los nombres de API y los nombres para mostrar de tipo de objeto personalizado pueden contener caracteres alfanuméricos y el carácter de guion bajo &quot;_&quot;.

### Tipo de consulta

Recupere los metadatos de tipo de objeto personalizado de cualquiera de estas formas:

- Describir tipo de objeto personalizado devuelve un registro de tipo de objeto personalizado y admite el filtrado por estado de aprobación.
- List Custom Object Types devuelve todos los tipos de objetos personalizados de la suscripción y admite el filtrado por nombre y estado de aprobación.

### Describir tipo

El extremo [Describir tipo de objeto personalizado](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/describeUsingGET_1) devuelve metadatos para un tipo de objeto personalizado. El parámetro de ruta de acceso `apiName` requerido especifica el nombre de API del tipo que se va a describir.

Si existe una versión aprobada, el extremo la devuelve. De lo contrario, devuelve la versión de borrador. Use el parámetro `state` opcional para solicitar `draft`, `approved` o `approvedWithDraft`.

```http
GET /rest/v1/customobjects/schema/{apiName}/describe.json?state=approved
```

```json
{
    "requestId": "d9bf#16876fa84b9",
    "result": [
        {
            "state": "approved",
            "version": "approved",
            "displayName": "Car",
            "description": "Automobile owned",
            "apiName": "car",
            "idField": "marketoGUID",
            "createdAt": "2019-01-22T19:12:18Z",
            "updatedAt": "2019-01-22T19:12:18Z",
            "dedupeFields": [
                "vin"
            ],
            "searchableFields": [
                [
                    "vin"
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
                        "field": "id"
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
                    "name": "vin",
                    "displayName": "VIN",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "year",
                    "displayName": "Year",
                    "dataType": "integer",
                    "updateable": true,
                    "crmManaged": false
                }
            ]
        }
    ],
    "success": true
}
```

La respuesta contiene:

- Metadatos: state, displayName, description, apiName, idField, createdAt, updatedAt, dedupeFields, searchableFields, Relations.
- Campos estándar: marketoGUID, createdAt, updatedAt.
- Campos personalizados: leadId, vin, make, model, year.

### Tipos de lista

El extremo [List Custom Object Types](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/listCustomObjectTypesUsingGET) devuelve metadatos para todos los tipos de objetos personalizados disponibles en la instancia de destino. Es similar a [Enumerar objetos personalizados](https://experienceleague.adobe.com/docs/marketo-developer/marketo/soap/custom-objects/custom-objects.html?lang=en), pero incluye metadatos adicionales como estado, relaciones y campos.

Si existe una versión aprobada, el extremo la devuelve. De lo contrario, devuelve la versión de borrador.

Los parámetros opcionales son:

- **estado**: Especifica la versión que se va a devolver. Los valores válidos son **draft**, **approved** y **approvedWithDraft**.
- **names**: especifica los tipos de objetos personalizados que se devolverán como una lista de nombres de API separados por comas.

```http
GET /rest/v1/customobjects/schema.json?names=purchaseHistory
```

```json
{
    "requestId": "a181#167ebe94703",
    "result": [
        {
            "state": "approved",
            "displayName": "Purchases",
            "description": "Purchase data",
            "apiName": "purchaseHistory",
            "idField": "marketoGUID",
            "createdAt": "2014-09-12T16:13:37Z",
            "updatedAt": "2014-09-12T16:13:42Z",
            "dedupeFields": [
                "lead_id",
                "product_name"
            ],
            "searchableFields": [
                [
                    "lead_id",
                    "product_name"
                ],
                [
                    "marketoGUID"
                ],
                [
                    "lead_id"
                ]
            ],
            "relationships": [
                {
                    "field": "lead_id",
                    "type": "child",
                    "relatedTo": {
                        "name": "Lead",
                        "field": "lead_id"
                    }
                }
            ],
            "fields": [
                {
                    "name": "marketoGUID",
                    "displayName": "marketoGUID",
                    "dataType": "string",
                    "length": 36,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "amount",
                    "displayName": "Amount",
                    "dataType": "float",
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "lead_id",
                    "displayName": "lead_id",
                    "dataType": "integer",
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "product_name",
                    "displayName": "Product Name",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "purchase_date",
                    "displayName": "Transaction Date",
                    "dataType": "datetime",
                    "updateable": true,
                    "crmManaged": false
                }
            ]
        },
        {
            "state": "approved",
            "version": "approved",
            "displayName": "Car",
            "description": "No really, it is a car!",
            "apiName": "car_c",
            "idField": "marketoGUID",
            "createdAt": "2017-02-22T19:55:51Z",
            "updatedAt": "2018-12-11T23:52:56Z",
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
            "relationships": [],
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
                    "name": "vin",
                    "displayName": "VIN",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "year",
                    "displayName": "Year",
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

### Crear y actualizar tipo

#### Crear tipo

Use el punto de conexión [Sincronizar tipo de objeto personalizado](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST) para crear o actualizar un tipo de objeto personalizado.

Los atributos son:

- **action**: Un atributo opcional que controla la operación de registro. Los valores válidos son **createOnly**, **createOrUpdate** y **updateOnly**. El valor predeterminado es createOrUpdate.
- **displayName** y **apiName**: Necesario a menos que la acción sea updateOnly. Ambos deben ser únicos para evitar conflictos con los tipos proporcionados por el cliente. Los socios de LaunchPoint deben anteponer un área de nombres representativa. Para apiName, utilice minúsculas o camelCase para distinguirlo de otras cadenas de texto.
- **pluralName**: atributo opcional que especifica la forma plural de displayName.
- **description**: Atributo opcional que describe el tipo de objeto personalizado.
- **showInLeadDetail**: Atributo booleano opcional que habilita los datos de objeto personalizados en la página Base de datos de posibles clientes de la interfaz de usuario de Marketo. El valor predeterminado es false.

Elija cuidadosamente los nombres de objetos personalizados. Agregue a cada nuevo nombre de objeto personalizado una cadena que identifique a su empresa. El prefijo puede contener caracteres alfanuméricos o guiones bajos. Esta convención facilita la búsqueda del objeto en la interfaz de usuario de MLM y ayuda a garantizar que su nombre sea único.

En el siguiente ejemplo se crea un tipo de objeto personalizado con el nombre de API &quot;transaction&quot;.

```http
POST /rest/v1/customobjects/schema.json
```

```json
{
  "action":"createOnly",
  "displayName": "Transaction",
  "apiName": "transaction",
  "description": "Commerce happens"
}
```

```json
{
    "requestId": "fb9d#167f2879557",
    "result": [],
    "success": true
}
```

La siguiente solicitud describe el tipo recién creado.

```http
GET /rest/v1/customobjects/schema/transaction/describe.json
```

```json
{
    "requestId": "cf9b#167f28db0a9",
    "result": [
        {
            "state": "draft",
            "displayName": "Transaction",
            "description": "Commerce happens",
            "apiName": "transaction",
            "idField": null,
            "createdAt": null,
            "updatedAt": null,
            "dedupeFields": [],
            "searchableFields": [
                []
            ],
            "relationships": [],
            "fields": [
                {
                    "name": "marketoGUID",
                    "displayName": "Marketo GUID",
                    "dataType": "string",
                    "length": 36,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "createdAt",
                    "displayName": "Created At",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "updatedAt",
                    "displayName": "Updated At",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                }
            ]
        }
    ],
    "success": true
}
```

La respuesta contiene:

- Metadatos: state, displayName, description, apiName, idField, createdAt, updatedAt, dedupeFields, searchableFields, Relations.
- Campos estándar: marketoGUID, createdAt, updatedAt.

#### Tipo de actualización

En el siguiente ejemplo se actualiza la descripción de un tipo existente cuyo nombre de API es &quot;transaction&quot;. Se requiere el atributo **apiName**. Como el tipo ya existe, la solicitud usa updateOnly para el atributo **action** opcional.

Además de **apiName**, puede actualizar los atributos disponibles durante la creación.

```http
POST /rest/v1/customobjects/schema.json
```

```json
{
  "action":"updateOnly",
  "apiName": "transaction",
  "description":"No really, commerce happens!"
}
```

```json
{
    "requestId": "103c3#167f2223fd7",
    "result": [],
    "success": true
}
```

## Aprobación del tipo

Apruebe los tipos de objetos personalizados antes de utilizarlos. Cuando crea un tipo con el extremo [Tipo de objeto personalizado de sincronización](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/syncCustomObjectTypeUsingPOST), Marketo crea una versión de borrador. Después de agregar campos personalizados, apruebe el borrador. Approval crea una versión aprobada y elimina el borrador.

Cuando se modifica un tipo existente con Sincronizar tipo de objeto personalizado o un punto final Agregar/Actualizar/Eliminar tipo de objeto personalizado, Marketo crea un borrador. Los cambios en el tipo o en sus campos afectan únicamente a la versión de borrador. Después de realizar los cambios, apruebe el borrador. Approval reemplaza la versión aprobada por el borrador y lo elimina.

Para obtener más información, consulte la [documentación de aprobación de objetos personalizados](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/approve-a-custom-object).

Una vez aprobado un tipo de objeto personalizado, no puede:

- Actualice `displayName` o `apiName`.
- Agregar o quitar un campo de vínculo.
- Agregar o quitar un campo desduplicado.

Planifique cuidadosamente el esquema y la convención de nombres antes de aprobar el tipo.

### Tipo de aprobación

Use el extremo [Aprobar tipo de objeto personalizado](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/approveCustomObjectTypeUsingPOST) para publicar un borrador como la nueva versión aprobada. El único parámetro requerido es el parámetro de ruta **apiName**.

Solo puede aprobar un tipo cuando esté en estado de borrador y cumpla las [reglas de validación](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/approve-a-custom-object) documentadas.

```http
POST /rest/v1/customobjects/schema/{apiName}/approve.json
```

```json
{
    "requestId": "11d86#1685304a983",
    "result": [],
    "success": true
}
```

### Tipo de descarte

Use el extremo [Descartar tipo de objeto personalizado Borrador](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/discardCustomObjectTypeUsingPOST) para eliminar una versión de borrador. El único parámetro requerido es el parámetro de ruta de acceso `apiName`.

Solo se puede descartar un tipo en estado de borrador. No se puede descartar un tipo aprobado.

```http
POST /rest/v1/customobjects/schema/{apiName}/discardDraft.json
```

```json
{
    "requestId": "5228#1684edde793",
    "result": [],
    "success": true
}
```

### Eliminar tipo

Use el extremo [Eliminar tipo de objeto personalizado](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/deleteCustomObjectsUsingPOST) para eliminar una versión aprobada. El único parámetro requerido es el parámetro de ruta de acceso `apiName`.

Esta operación es destructiva y no se puede deshacer. Antes de eliminar un tipo, elimine su uso de recursos como déclencheur y filtros. Utilice el punto de conexión de Assets Obtener objeto personalizado dependiente para recuperar los recursos dependientes de un tipo.

POST /rest/v1/customobjects/schema/{apiName}/delete.json

```json
{
    "requestId": "14e36#1684efc4227",
    "result": [],
    "success": true
}
```

## Campos de objeto personalizados

De forma predeterminada, todos los tipos de objetos personalizados contienen los siguientes campos estándar:

- GUID de Marketo: identificador único del tipo de objeto personalizado.
- Creado a las: Fecha y hora en que se creó el tipo de objeto personalizado.
- Se actualizó en: Fecha y hora en que se actualizó por última vez el tipo de objeto personalizado.

Utilice los siguientes extremos para agregar, cambiar o eliminar campos personalizados.

- El número máximo de campos es 50.
- Una vez aprobado un objeto personalizado, puede agregarle un máximo de 20 campos adicionales.
- Se requiere al menos un campo de desduplicación. Se permite un máximo de tres campos de desduplicación.
- Los nombres de API de campo y los nombres para mostrar pueden contener caracteres alfanuméricos y el carácter de guion bajo &quot;_&quot;.

Para obtener más información, consulte la [documentación sobre campos de objeto personalizados](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields).

### Añadir campos

Use el extremo [Agregar campos de tipo de objeto personalizados](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/addCustomObjectTypeFieldsUsingPOST) para agregar uno o más campos a un objeto personalizado. El cuerpo de la solicitud contiene una matriz `input` con uno o más elementos. Cada elemento es un objeto JSON con atributos que describen un campo.

Los atributos de campo son:

- `name`: obligatorio. El nombre de la API del campo, que debe ser único para el objeto personalizado. Utilice minúsculas o camelCase para distinguir el nombre de otras cadenas de texto.
- `displayName`: obligatorio. El nombre del campo legible en lenguaje natural, que debe ser único para el objeto personalizado.
- `dataType`: obligatorio. El tipo de datos del campo. Use el extremo [Obtener tipos de datos del campo de tipo de objeto personalizado](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/getCustomObjectTypeFieldDataTypesUsingGET) para recuperar los tipos de datos permitidos.
- `description`: Opcional. La descripción del campo.
- `isDedupeField`: booleano opcional que especifica si el campo se utiliza para la anulación de duplicación durante las operaciones de actualización de objetos personalizados. El valor predeterminado es false. Se requiere un campo de desduplicación para las relaciones uno a varios.
- `relatedTo`: objeto opcional que especifica un campo de vínculo. Para una relación uno a varios, `name` identifica el &quot;objeto de vínculo&quot; o el objeto principal y `field` identifica el &quot;campo de vínculo&quot; o campo de clave en el objeto principal.

Los objetos personalizados pueden contener campos con el tipo de datos &quot;vínculo&quot;. Los campos de vínculo establecen relaciones entre los objetos personalizados y otros tipos de objeto, como Posible cliente y Compañía. Consulte la [documentación del campo de objeto personalizado](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields) para obtener detalles sobre los campos de vínculo. Use el extremo [Obtener objetos enlazables de objetos personalizados](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/getCustomObjectTypeLinkableObjectsUsingGET) para recuperar los objetos de vínculo permitidos.

Un objeto personalizado no puede vincularse a otro objeto personalizado que tenga un campo de vínculo existente. Para obtener más información, consulte la [documentación de campos de vínculo](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields).

### Relación &quot;uno a varios&quot;

Para una estructura de objeto personalizada de uno a varios, utilice un campo de vínculo para conectar un objeto personalizado a un objeto de cliente potencial o compañía estándar. El siguiente flujo de trabajo usa el [ejemplo del propietario del automóvil](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-link-fields#AddMarketoCustomObjectLinkFields-CreateaLinkFieldforaOne-to-ManyStructure) para crear un objeto personalizado que almacena información del automóvil y se conecta a los posibles clientes.

1. Crear un objeto **Car**.
1. Agregar campos al objeto **Car**: desduplicar en **VIN** y vincular a **ID de posible cliente**&#x200B;**/ID de posible cliente**.
1. Apruebe el objeto **Car**.

En primer lugar, cree el tipo de objeto personalizado que contenga información específica del coche.

```http
POST /rest/v1/customobjects/schema.json
```

```json
{
    "action":"createOnly",
    "displayName": "Car",
    "pluralName": "Cars"
    "apiName": "car",
    "description": "Automobile owned",
    "showInLeadDetail": true
}
```

```json
{
    "requestId": "cbaa#16876dd3da6",
    "result": [],
    "success": true
}
```

A continuación, agregue campos al tipo de objeto personalizado Car. Utilice un campo de vínculo para especificar el objeto y el campo al que desea conectarse. En este ejemplo, el objeto de vínculo es posible cliente y el campo de vínculo es ID.

Utilice un campo de cadena para la anulación de duplicación (VIN). Agregue tres campos más para almacenar los atributos Marca, Modelo y Año.

```http
POST /rest/v1/customobjects/schema/car/addField.json
```

```json
{
  "input": [
    {
      "displayName": "Lead ID",
      "description": "Link field to Lead object",
      "name": "leadID",
      "dataType": "link",
      "relatedTo": {
        "field": "id",
        "name": "lead"
      }
    },
    {
      "displayName": "VIN",
      "description": "Vehicle ID number",
      "name": "vin",
      "dataType": "string",
      "isDedupeField": true
    },
    {
      "displayName": "Make",
      "description": "Vehicle make",
      "name": "make",
      "dataType": "string"
    },
    {
      "displayName": "Model",
      "description": "Vehicle model",
      "name": "model",
      "dataType": "string"
    },
    {
      "displayName": "Year",
      "description": "Vehicle year",
      "name": "year",
      "dataType": "integer"
    }
  ]
}

{
    "requestId": "b359#16876f17996",
    "result": [],
    "success": true
}
```

Finalmente, apruebe el tipo de objeto personalizado.

```http
POST /rest/v1/customobjects/schema/course/approve.json
```

```json
{
    "requestId": "460b#16896055fa3",
    "result": [],
    "success": true
}
```

### Relación &quot;varios a varios&quot;

Una relación &quot;varios a varios&quot; utiliza un objeto personalizado &quot;puente&quot; entre un objeto estándar, como Posible cliente o Empresa, y un objeto personalizado &quot;borde&quot;. El objeto edge es la entidad principal y contiene campos descriptivos.

El objeto bridge resuelve la relación con dos campos de vínculo. Un campo señala al objeto estándar principal, como en una relación uno a varios. El otro señala al objeto edge, que es un objeto personalizado sin vínculos. El objeto bridge también puede contener campos descriptivos.

El siguiente flujo de trabajo usa el [ejemplo de inscripción en curso universitario](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-link-fields#AddMarketoCustomObjectLinkFields-CreateaLinkFieldforaOne-to-ManyStructure). Crea un objeto perimetral de curso y un objeto de puente de inscripción que conecta cursos con posibles clientes.

1. Crear un objeto Edge **Course**.
1. Agregar campos a **Curso:** desduplicado en **ID de curso**.
1. Aprobar **curso**.
1. Crear un objeto de puente **Enrollment**.
1. Agregar campos a **Inscripción:** desduplicada en **ID de inscripción**, vincular al campo **Curso**&#x200B;**/ID de curso** y vincular a **ID de posible cliente**&#x200B;**/ID de posible cliente**.
1. Aprobar **inscripción**.

En primer lugar, cree el tipo de objeto edge que contenga información específica del curso:

```http
POST /rest/v1/customobjects/schema.json
```

```json
{
    "action":"createOnly",
    "displayName": "Course",
    "pluralName": "Courses",
    "apiName": "course",
    "description": "Modeling a college course, an edge object in Marketo",
    "showInLeadDetail": true
}
```

```json
{
    "requestId": "4aec#168879ede00",
    "result": [],
    "success": true
}
```

A continuación, añada cuatro campos personalizados para modelar un curso universitario: ID de curso, Instructor de curso, Ubicación del curso y Nombre del curso. Designe el ID de curso como campo de desduplicación porque se requiere al menos un campo de desduplicación.

```http
POST /rest/v1/customobjects/schema/course/addField.json
```

```json
{
    "input": [
        {
            "displayName": "Course ID",
            "name": "courseID",
            "dataType": "string",
            "isDedupeField": true
        },
        {
            "displayName": "Course Instructor",
            "name": "courseInstructor",
            "dataType": "string"
        },
        {
            "displayName": "Course Location",
            "name": "courseLocation",
            "dataType": "string"
        },
        {
            "displayName": "Course Name",
            "name": "courseName",
            "dataType": "string"
        }
    ]
}
```

```json
{
    "requestId": "cc36#16895b82a41",
    "result": [],
    "success": true
}
```

Apruebe el tipo de objeto de borde para que pueda hacer referencia a él al vincularlo al tipo de objeto de puente. Se debe aprobar un tipo de objeto personalizado para poder seleccionarlo como objeto de vínculo.

```http
POST /rest/v1/customobjects/schema/course/approve.json
```

```json
{
    "requestId": "460b#16896055fa3",
    "result": [],
    "success": true
}
```

Después de completar el objeto Edge, cree el tipo de objeto Bridge que contenga información específica de la inscripción.

```http
POST /rest/v1/customobjects/schema.json
```

```json
{
    "action": "createOnly",
    "displayName": "Enrollment",
    "pluralName": "Enrollments",
    "apiName": "enrollment",
    "description": "Bridge object for Course custom object",
    "showInLeadDetail": true
}
```

```json
{
    "requestId": "8fbb#168960f671b",
    "result": [],
    "success": true
}
```

Agregue dos campos de vínculo al tipo de objeto puente: uno que se vincule al objeto Posible cliente y otro que se vincule al objeto Curso. Utilice el campo ID de posible cliente para vincular al posible cliente y el campo ID de curso para vincular al curso.

Añada el ID de inscripción como campo de desduplicación porque se requiere al menos un campo de desduplicación. A continuación, agregue un campo Grado para realizar el seguimiento del rendimiento del alumno.

```http
POST /rest/v1/customobjects/schema/enrollment/addField.json
```

```json
{
    "input": [
        {
            "displayName": "Lead ID",
            "description": "Link field to Lead object",
            "name": "leadID",
            "dataType": "link",
            "relatedTo": {
                "field": "id",
                "name": "lead"
            }
        },
        {
            "displayName": "Course ID",
            "description": "Link field to Course object",
            "name": "courseID",
            "dataType": "link",
            "relatedTo": {
                "field": "courseID",
                "name": "course"
            }
        },
        {
            "displayName": "Enrollment ID",
            "description": "Unique ID for deduplication",
            "name": "enrollmentID",
            "dataType": "string",
            "isDedupeField": true
        },
        {
            "displayName": "Grade",
            "description": "Grade for the course",
            "name": "grade",
            "dataType": "string"
        }
    ]
}
```

```json
{
    "requestId": "7be5#168973f5052",
    "result": [],
    "success": true
}
```

Finalmente, apruebe el objeto bridge.

```http
POST /rest/v1/customobjects/schema/enrollment/approve.json
```

```json
{
    "requestId": "9a76#16897b0e84b",
    "result": [],
    "success": true
}
```

Rellene registros de objetos personalizados mediante programación usando [Sincronizar objeto personalizado](#create_and_update) o [Importación masiva de objeto personalizado](https://experienceleague.adobe.com/docs/marketo-developer/marketo/rest/bulk-import/bulk-custom-object-import.html?lang=en). También puede usar [Importar datos de objeto personalizados](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/import-custom-object-data) en la interfaz de usuario de Marketo.

## Actualizar campo

Use el extremo [Actualizar campo de tipo de objeto personalizado](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/updateCustomObjectTypeFieldUsingPOST) para actualizar un campo en un objeto personalizado de borrador.

Los parámetros de ruta requeridos son:

- `apiName`: nombre de API del tipo de objeto personalizado.
- `fieldAPIName`: nombre de API del campo de tipo de objeto personalizado.

El cuerpo de la solicitud contiene un objeto JSON con pares clave/valor que especifican los atributos de campo que se van a actualizar.

```http
POST /rest/v1/customobjects/schema/{apiName}/{fieldApiName}/updateField.json
```

```json
{
  "displayName": "Very Long Title",
  "dataType": "text"
}
```

```json
{
    "requestId": "d523#1684f355db9",
    "result": [],
    "success": true
}
```

## Eliminar campos

Use el extremo [Eliminar campos de tipo de objeto personalizados](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/deleteCustomObjectTypeFieldsUsingPOST) para eliminar uno o varios campos de un objeto personalizado. El parámetro de ruta de acceso `apiName` requerido especifica el nombre de API del tipo de objeto personalizado.

El cuerpo de la solicitud contiene un objeto JSON con una matriz `input` de uno o más elementos. Cada elemento es un objeto JSON cuyo atributo `name` especifica el nombre de API de un campo que se va a eliminar.

```http
POST /rest/v1/customobjects/schema/{apiName}/deleteField.json
```

```json
{
    "input":
    [
        {
            "name": "title"
        },
        {
            "name": "author"
        }
    ]
}
```

```json
{
"requestId": "b359#19934f17996",
"result": [],
"success": true
}
```

## Tipos de datos de campos de lista

El extremo [Obtener tipos de datos de campo de tipo de objeto personalizado](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/getCustomObjectTypeFieldDataTypesUsingGET) devuelve todos los tipos de datos de campo permitidos. Utilice este extremo para identificar los tipos de datos de campo personalizado disponibles al modelar un tipo de objeto personalizado.

```http
GET /rest/v1/customobjects/schema/fieldDataTypes.json
```

```json
{
    "requestId": "c405#167ed49e866",
    "result": [
        "string",
        "boolean",
        "integer",
        "float",
        "link",
        "email",
        "currency",
        "date",
        "datetime",
        "phone",
        "text"
    ],
    "success": true
}
```

## Objetos personalizados enlazables a lista

El extremo [Obtener objetos enlazables de objetos personalizados](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/getCustomObjectTypeLinkableObjectsUsingGET) devuelve todos los objetos de vínculo permitidos y sus campos de vínculo. La respuesta incluye objetos estándar, como cliente potencial y compañía, y cualquier objeto personalizado creado en la instancia.

```http
GET /rest/v1/customobjects/schema/linkableObjects.json
```

```json
{
    "requestId": "11e62#167f1160e4e",
    "result": [
        {
            "name": "lead",
            "displayName": "Lead",
            "fields": [
                {
                    "name": "Account Balance",
                    "displayName": "Account Balance",
                    "dataType": "integer"
                },
                {
                    "name": "Email Address",
                    "displayName": "Email Address",
                    "dataType": "email"
                },
                {
                    "name": "Id",
                    "displayName": "Id",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Facebook Display Name",
                    "displayName": "Marketo Social Facebook Display Name",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Facebook Id",
                    "displayName": "Marketo Social Facebook Id",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Facebook Photo URL",
                    "displayName": "Marketo Social Facebook Photo URL",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Facebook Profile URL",
                    "displayName": "Marketo Social Facebook Profile URL",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Facebook Reach",
                    "displayName": "Marketo Social Facebook Reach",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Facebook Referred Enrollments",
                    "displayName": "Marketo Social Facebook Referred Enrollments",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Facebook Referred Visits",
                    "displayName": "Marketo Social Facebook Referred Visits",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Gender",
                    "displayName": "Marketo Social Gender",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social LinkedIn Display Name",
                    "displayName": "Marketo Social LinkedIn Display Name",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social LinkedIn Id",
                    "displayName": "Marketo Social LinkedIn Id",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social LinkedIn Photo URL",
                    "displayName": "Marketo Social LinkedIn Photo URL",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social LinkedIn Profile URL",
                    "displayName": "Marketo Social LinkedIn Profile URL",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social LinkedIn Reach",
                    "displayName": "Marketo Social LinkedIn Reach",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social LinkedIn Referred Enrollments",
                    "displayName": "Marketo Social LinkedIn Referred Enrollments",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social LinkedIn Referred Visits",
                    "displayName": "Marketo Social LinkedIn Referred Visits",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Syndication Id",
                    "displayName": "Marketo Social Syndication Id",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Total Referred Enrollments",
                    "displayName": "Marketo Social Total Referred Enrollments",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Total Referred Visits",
                    "displayName": "Marketo Social Total Referred Visits",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Twitter Display Name",
                    "displayName": "Marketo Social Twitter Display Name",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Twitter Id",
                    "displayName": "Marketo Social Twitter Id",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Twitter Photo URL",
                    "displayName": "Marketo Social Twitter Photo URL",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Twitter Profile URL",
                    "displayName": "Marketo Social Twitter Profile URL",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Twitter Reach",
                    "displayName": "Marketo Social Twitter Reach",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Twitter Referred Enrollments",
                    "displayName": "Marketo Social Twitter Referred Enrollments",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Twitter Referred Visits",
                    "displayName": "Marketo Social Twitter Referred Visits",
                    "dataType": "integer"
                }
            ]
        },
        {
            "name": "company",
            "displayName": "Company",
            "fields": [
                {
                    "name": "Id",
                    "displayName": "Id",
                    "dataType": "integer"
                }
            ]
        },
        {
            "name": "car_c",
            "displayName": "Car",
            "fields": [
                {
                    "name": "marketoGUID",
                    "displayName": "Marketo GUID",
                    "dataType": "string"
                },
                {
                    "name": "vin",
                    "displayName": "VIN",
                    "dataType": "string"
                }
            ]
        }
    ],
    "success": true
}
```

## Obtener Assets dependiente del objeto personalizado

El extremo [Get Custom Object Dependent Assets](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/getCustomObjectTypeDependentAssetsUsingGET) devuelve los recursos dependientes de un tipo de objeto personalizado y sus ubicaciones en la instancia. Utilícela al eliminar una integración para identificar en qué lugar se utiliza un tipo de objeto personalizado.

```http
GET /rest/v1/customobjects/schema/{apiName}/dependentAssets.json
```

```json
{
    "requestId": "71cf#16a21f30ed6",
    "result": [
        {
            "assetType": "Smart Campaign",
            "assetId": 3773,
            "assetName": "CarTest.HasCar (Smart List)"
        },
        {
            "assetType": "Smart Campaign",
            "assetId": 3773,
            "assetName": "CarTest.HasCar (Smart List)",
            "usedFields": [
                "leadID",
                "make",
                "model",
                "vin",
                "year"
            ]
        }
    ],
    "success": true
}
```

## Tiempos de espera

- Los extremos de objetos personalizados tienen un tiempo de espera de 30 segundos a menos que se indique lo contrario.
- El tiempo de espera de Sincronizar objetos personalizados es de 120 segundos.
- Eliminar objetos personalizados tiene un tiempo de espera de 60 segundos.
