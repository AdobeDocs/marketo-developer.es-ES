---
title: Objetos personalizados
feature: REST API, Custom Objects
description: Obtenga información sobre cómo crear y administrar objetos personalizados de Marketo mediante la API de REST, incluidos extremos de lista y descripción, metadatos, relaciones, campos y consultas.
exl-id: 88e8829b-f8f1-46d7-a753-5aa6e20e2c40
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '2925'
ht-degree: 0%

---

# Objetos personalizados

[**Referencia de extremo de objeto personalizado**](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects) Marketo permite a los usuarios definir objetos personalizados de Marketo relacionados con objetos estándar de Marketo (posibles clientes, empresas) u otros objetos personalizados de Marketo.  Los objetos personalizados de Marketo se pueden crear usando la interfaz de usuario de Marketo como se describe [aquí](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/create-marketo-custom-objects), o usando la API de metadatos de objeto personalizado como se describe a continuación.

Se requiere un tipo de suscripción de Marketo adecuado para acceder a la API de metadatos de objeto personalizado.  Consulte su CSM para obtener más información.

## Lista

Además de las llamadas estándar de descripción, consulta, actualización y eliminación disponibles para los objetos de base de datos de clientes potenciales, los objetos personalizados tienen disponible una [llamada de lista](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectsUsingGET).  Llamar a este extremo devolverá una respuesta con una lista de objetos personalizados disponibles en la instancia de destino, junto con metadatos adicionales sobre los objetos.

```
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

La respuesta proporcionará una lista de las relaciones presentes en cada objeto.  Una relación tendrá un miembro `field` que indica el campo del objeto que contiene el valor de vínculo, un miembro `type` que indica si la relación es con un objeto de tipo primario o secundario, y un objeto `relatedTo` que indica el nombre del objeto relacionado y el campo de vínculo de ese objeto.

## Describir

La llamada [describir](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/describeUsingGET_1) para objetos personalizados sigue el mismo patrón que el de Oportunidades y Compañías, con la adición de la matriz `relationships` en la respuesta y un parámetro de ruta de acceso `apiName` en el URI que toma el nombre de API del tipo de objeto personalizado que se va a describir.  Al igual que la llamada de lista, esto enumerará todas las relaciones disponibles para este tipo de objeto personalizado.

```
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

[La consulta de objetos personalizados](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectsUsingGET) difiere ligeramente de otras API de base de datos de posibles clientes y usa `apiName` parámetro de ruta como se describe.  Para los parámetros filterType normales, la consulta es un GET simple como consultas para otros tipos de registros y requiere un `filterType` y un `filterValues`.  Opcionalmente, aceptará `**fields**`, `batchSize` y `nextPageToken` parámetros.  Al solicitar una lista de campos, si se solicita un campo en particular pero no se devuelve, el valor se entiende como nulo.

```
GET /rest/v1/customobjects/{apiName}.json?filterType=idField&filterValues=dff23271-f996-47d7-984f-f2676861b5fa,dff23271-f996-47d7-984f-f2676861b5fb
```

```
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

Alternativamente, al consultar con claves compuestas, la API se comporta de la misma manera que la API de funciones de oportunidad, al aceptar una PUBLICACIÓN con un cuerpo JSON.  El cuerpo de JSON puede tener los mismos miembros que una consulta de GET, excepto `filterValues`.  En lugar de filtrar valores, hay una matriz `input` que toma objetos que contienen un miembro denominado para cada uno de los `dedupeFields` del tipo de objeto.

```
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

Use el extremo [Sincronizar objetos personalizados](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST) para crear o actualizar objetos personalizados; puede especificar la operación con el parámetro `action`.  Se pueden crear o actualizar hasta 300 registros en una llamada.  Los valores utilizados en la matriz `input` se basan en gran medida en la información devuelta por el extremo [Describir objetos personalizados](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/endpoint-reference#!/Custom_Objects/describeUsingGET_1). En un objeto de carro de ejemplo, hay un solo campo de desduplicación, `vin`.  Para actualizar o crear registros al utilizar el modo deduplicar campos, cada registro de la matriz de entrada debe incluir al menos un campo `vin`.

```
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

Al realizar actualizaciones a través del modo `idField`, `idField` siempre será `marketoGUID`, por lo que cada registro siempre requerirá un campo `marketoGUID`.  Recuerde que `idField` solo es válido para el tipo de acción updateOnly, ya que este campo siempre está administrado por el sistema.  Su respuesta incluirá el **estado** de cada registro individual en la matriz de resultados y una matriz `marketoGUID` o `reasons`, dependiendo de si la operación se realizó correctamente para un registro individual o no.

## Eliminar

[Eliminar registros](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/deleteCustomObjectsUsingPOST) es muy sencillo.  Solo tiene que seleccionar el modo `deleteBy`, ya sea `idField` o `dedupeFields`, e incluir los campos correspondientes en cada uno de los registros de la matriz `input`. Se permite un máximo de 300 registros por llamada.

```
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

Al igual que la actualización, el resultado contendrá un estado para cada registro individual y una matriz `marketoGUID` o `reasons` en función de si la eliminación se realizó correctamente.

## Tipos de objetos personalizados

La API de metadatos de objeto personalizada permite administrar de forma remota esquemas de objeto personalizados.  La API permite crear un nuevo tipo de objeto personalizado o modificar uno existente.  Una vez creado o modificado el tipo de objeto personalizado, debe aprobarse para su uso.  Para obtener más información acerca de los objetos personalizados, consulte la documentación del producto [aquí](https://experienceleague.adobe.com/es/docs/marketo/using/home).

* Los tipos de objetos personalizados creados por la API no se pueden modificar mediante la IU de Marketo
* El número máximo de tipos de objetos personalizados permitidos es 10
* El número máximo de campos de objeto personalizados permitido es 50 por tipo
* Los nombres de API y los nombres para mostrar de tipo de objeto personalizado pueden contener caracteres alfanuméricos y guiones bajos &quot;_&quot;

### Tipo de consulta

Existen dos formas de recuperar los metadatos de tipo de objeto personalizado: Describir tipo de objeto personalizado, que devuelve  el registro de un solo tipo de objeto personalizado, y se puede filtrar por estado de aprobación, y List Custom Object Types, que devuelve una lista de todos los tipos de objetos personalizados de la suscripción, y se puede filtrar por nombre y por estado de aprobación.

### Describir tipo

El extremo [Describir tipo de objeto personalizado](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/describeUsingGET_1) devuelve metadatos para un único tipo de objeto personalizado. El parámetro de ruta de acceso `apiName` requerido es el nombre de API del tipo de objeto personalizado que se describe.  Si existe una versión aprobada, se devuelve.  En caso contrario, se devuelve la versión de borrador.  El parámetro `state` opcional se usa para especificar la versión que se va a devolver: `draft`, `approved` o `approvedWithDraft`.

```
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

Aquí podemos ver los siguientes atributos:

* Metadatos: state, displayName, description, apiName, idField, createdAt, updatedAt, dedupeFields, searchableFields, Relations
* Campos estándar: marketoGUID, createdAt, updatedAt
* Campos personalizados leadId, vin, make,  modelo, año

### Tipos de lista

El extremo [List Custom Object Types](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/listCustomObjectTypesUsingGET) devuelve metadatos para todos los tipos de objetos personalizados disponibles en la instancia de destino.  Tenga en cuenta que este extremo es similar a [Lista de objetos personalizados](https://experienceleague.adobe.com/docs/marketo-developer/marketo/soap/custom-objects/custom-objects.html?lang=en), pero es más completo e incluye metadatos adicionales como estado, relaciones y campos. Si existe una versión aprobada, se devuelve.  En caso contrario, se devuelve la versión de borrador.  El parámetro **state** opcional se usa para especificar la versión del tipo de objeto personalizado que se va a devolver: **draft**, **approved** o **approvedWithDraft**.  El parámetro **names** opcional se usa para especificar nombres específicos de tipos de objetos personalizados que se van a devolver; está estructurado como una lista de nombres de API separados por comas.

```
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
            "description": "No really, it's a car!",
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

El punto de conexión [Sincronizar tipo de objeto personalizado](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST) se usa para crear o actualizar un tipo de objeto personalizado.  La operación de registro que se va a realizar está controlada por el atributo opcional **action**, que puede ser **createOnly**, **createOrUpdate** o **updateOnly**.  La configuración predeterminada es createOrUpdate. Los atributos **displayName** y **apiName** son obligatorios, excepto cuando se usa updateOnly como acción.   Ambos deben ser únicos para evitar conflictos con los tipos proporcionados por el cliente.  Si es socio de LaunchPoint, debe anteponer un área de nombres representativa a estos nombres.  Para apiName, la convención es utilizar minúsculas o camelCase para ayudar a distinguir entre otras cadenas de texto. El atributo opcional **pluralName** especifica la forma plural de displayName.  El atributo opcional **description** se usa para describir el tipo de objeto personalizado.  El atributo booleano opcional **showInLeadDetail** se usa para permitir la visualización de los datos de objetos personalizados en la página de la base de datos de posibles clientes de la interfaz de usuario de Marketo.  La configuración predeterminada es false.

Tenga cuidado al nombrar los objetos personalizados. Al crear un nuevo objeto personalizado, se recomienda codificar el nombre con una cadena que indique el nombre de su empresa (alfanumérico o guion bajo permitido). Esto facilita la búsqueda en el objeto personalizado en la IU de MLM y también ayuda a no estar seguro de que el nombre sea único.

Este es un ejemplo de creación de un nuevo tipo de objeto personalizado con API  Nombre &quot;transacción&quot;.

```
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

Esta es una llamada posterior para describir el tipo recién creado.

```
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

Aquí podemos ver los siguientes datos personalizados relacionados con objetos:

* Metadatos: state, displayName, description, apiName, idField, createdAt, updatedAt, dedupeFields, searchableFields, Relations
* Campos estándar: marketoGUID, createdAt, updatedAt

#### Tipo de actualización

A continuación, se muestra un ejemplo de actualización de la descripción de un tipo existente cuyo nombre de API es &quot;transaction&quot;.  Se requiere el atributo **apiName**.  Aquí supondremos que el tipo ya existe y usaremos updateOnly para el atributo opcional **action**.  Además de **apiName**, es posible que se actualicen los atributos disponibles para la creación.

```
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

Los tipos de objetos personalizados deben aprobarse antes de poder utilizarse. Cuando se crea un nuevo tipo de objeto personalizado con el punto de conexión [Sincronizar tipo de objeto personalizado](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/syncCustomObjectTypeUsingPOST), se crea como una versión de borrador. Cuando haya terminado de agregar campos personalizados, debe aprobar la versión de borrador. Esto crea una versión aprobada y elimina la versión de borrador. Cuando se modifica un tipo de objeto personalizado existente mediante el punto de conexión Sincronizar tipo de objeto personalizado o mediante uno de los extremos del campo Agregar/Actualizar/Eliminar tipo de objeto personalizado, se crea una versión de borrador. Todas las modificaciones del tipo o de sus campos afectan únicamente a la versión de borrador. Cuando haya terminado de modificar, debe aprobar la versión de borrador. Esto reemplaza la versión aprobada por la versión de borrador y elimina la versión de borrador. Para obtener más información acerca de la aprobación de objetos personalizados, consulte la documentación del producto [aquí](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/approve-a-custom-object).

Una vez aprobado un tipo de objeto personalizado, no puede:

* Actualizar `displayName` o `apiName`
* Adición o eliminación de un campo de vínculo
* Adición o eliminación de un campo desduplicado

Por estos motivos, es importante revisar cuidadosamente el esquema y la convención de nombres que planea utilizar.

### Tipo de aprobación

Use el extremo [Aprobar tipo de objeto personalizado](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/approveCustomObjectTypeUsingPOST) para publicar una versión de borrador como la nueva versión aprobada.  **apiName** es el único parámetro requerido como parámetro de ruta.  No se puede aprobar un tipo a menos que esté en estado de borrador y cumpla un conjunto de reglas de validación descritas [aquí](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/approve-a-custom-object).

```
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

Use el extremo [Descartar tipo de objeto personalizado Borrador](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/discardCustomObjectTypeUsingPOST) para eliminar una versión de borrador. `apiName` es el único parámetro requerido como parámetro de ruta de acceso. Un tipo debe estar en estado de borrador para descartarse, es decir, un tipo aprobado no se puede descartar.

```
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

Use el extremo [Eliminar tipo de objeto personalizado](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/deleteCustomObjectsUsingPOST) para eliminar una versión aprobada.  `apiName` es el único parámetro requerido como parámetro de ruta de acceso.  Tenga en cuenta que se trata de una operación destructiva; no se puede deshacer.  No se puede eliminar un tipo a menos que se haya eliminado de su uso por algún recurso, como déclencheur o filtros.  El punto de conexión de Assets Obtener objeto personalizado dependiente se puede utilizar para recuperar una lista de recursos dependientes para un tipo determinado.

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

* GUID de Marketo: identificador único del tipo de objeto personalizado
* Creado a las - Fecha y hora en que se creó el tipo de objeto personalizado
* Actualizado a las: fecha y hora en la que se actualizó por última vez el tipo de objeto personalizado

Puede agregar, cambiar o eliminar campos personalizados con los extremos que se describen a continuación.

* El número máximo de campos permitidos es 50
* Una vez aprobado un objeto personalizado, puede agregar un máximo de 20 campos adicionales al objeto personalizado
* Se requiere al menos un campo de desduplicación y se permiten un máximo de tres
* Los nombres de API de campo y los nombres para mostrar pueden contener caracteres alfanuméricos y guiones bajos &quot;_&quot;

Para obtener más información acerca de los campos de objeto personalizados, consulte la documentación del producto [aquí](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields).

### Añadir campos

El extremo [Agregar campos de tipo de objeto personalizados](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/addCustomObjectTypeFieldsUsingPOST) le permite agregar uno o más campos al objeto personalizado.  El cuerpo de la solicitud contiene una matriz `input` con uno o más elementos.  Cada elemento es un objeto JSON con atributos que describen un campo. El atributo `name` requerido es el nombre de API del campo y debe ser único para el objeto personalizado.   La convención es utilizar minúsculas o camelCase para ayudar a distinguir entre otras cadenas de texto. El atributo `displayName` requerido es el nombre del campo en lenguaje natural y debe ser único para el objeto personalizado. El atributo `dataType` requerido es el tipo de datos del campo.  A  La lista de tipos de datos permitidos se puede obtener llamando al extremo [Obtener tipos de datos del campo de tipo de objeto personalizado](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeFieldDataTypesUsingGET).  Los objetos personalizados pueden contener campos con el tipo de datos &quot;vínculo&quot;.  Los campos de vínculo se utilizan para establecer relaciones entre objetos personalizados y otros tipos de objetos del sistema, por ejemplo, Posible cliente, Compañía.  Encontrará más información sobre los campos de vínculo [aquí](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields). El atributo opcional `description` es la descripción del campo. El atributo booleano opcional `isDedupeField` especifica si el campo se utiliza para la anulación de duplicación durante las operaciones de actualización de objetos personalizados.  La configuración predeterminada es false.  Para las relaciones &quot;uno a varios&quot;, se requiere un campo de desduplicación. El atributo de objeto `relatedTo` opcional especifica un campo de vínculo.  Para las relaciones &quot;uno a varios&quot;, este objeto contiene un atributo `name` que es el &quot;objeto de vínculo&quot; o el objeto principal al que se va a vincular, y un atributo `field` que es el &quot;campo de vínculo&quot;,  o el campo dentro del objeto principal que se utilizará como atributo clave.  Llame al extremo [Get Custom Object Linkable Objects](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeLinkableObjectsUsingGET) para recuperar una lista de objetos de vínculo permitidos.  Para obtener más información sobre los campos de vínculo, consulte la documentación del producto [aquí](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields). Un objeto personalizado no puede vincularse a otro objeto personalizado que tenga un campo de vínculo existente.

### Relación &quot;uno a varios&quot;

Para una estructura de objeto personalizada de uno a varios, utilice un campo de vínculo en un objeto personalizado para conectarlo a un objeto estándar: Posible cliente o Empresa. Utilizando el ejemplo del propietario del automóvil de la documentación del producto de Marketo [aquí](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-link-fields#AddMarketoCustomObjectLinkFields-CreateaLinkFieldforaOne-to-ManyStructure), creamos un objeto personalizado que contiene información relacionada con el automóvil para conectarnos con posibles clientes.

1. Crear un objeto **Car**
1. Agregar campos al objeto **Car**: desduplicación en **VIN**, vínculo a **posible cliente**&#x200B;**/ID de posible cliente**
1. Aprobar objeto **Car**

En primer lugar, cree el tipo de objeto personalizado para que contenga información específica del coche.

```
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

Ahora, agregue campos al tipo de objeto personalizado Car. Utilizamos un campo de vínculo para especificar el objeto y el campo al que se va a conectar. En este caso, el objeto de vínculo es Posible cliente y el campo de vínculo es ID. Utilice un campo de cadena para la anulación de duplicación (VIN).  Añadiremos tres campos más para almacenar atributos de coche adicionales (Marca, Modelo, Año).

```
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

```
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

Las relaciones &quot;varios a varios&quot; se representan mediante un &quot;puente&quot; o un objeto personalizado intermedio entre un objeto personalizado estándar, como cliente potencial o compañía, y un objeto personalizado &quot;Edge&quot;. El objeto edge es la entidad principal que contiene atributos descriptivos (campos). El objeto Bridge contiene los datos para resolver las relaciones de objetos mediante 2 campos de vínculo.  Un campo de vínculo señala al objeto estándar principal como en una  configuración de relación uno a varios.  El otro campo de vínculo apunta al objeto edge, que es un objeto personalizado sin vínculos.  El objeto bridge también puede contener atributos descriptivos (campos). Utilizando el ejemplo de inscripción en un curso universitario de la documentación de producto de Marketo [aquí](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-link-fields#AddMarketoCustomObjectLinkFields-CreateaLinkFieldforaOne-to-ManyStructure), creamos un objeto personalizado perimetral para contener información relacionada con el curso y un objeto de puente de inscripción utilizado para conectar cursos con posibles clientes. Estos son los pasos:

1. Crear un objeto Edge **Course**
1. Agregar campos a **Curso:** desduplicación en **ID de curso**
1. Aprobar **curso**
1. Crear un objeto de puente **Enrollment**
1. Agregar campos a **Inscripción:** desduplicada en **ID de inscripción**, vínculo al campo **Curso**&#x200B;**/ID de curso** y vínculo a **ID de posible cliente**&#x200B;**/ID de posible cliente**
1. Aprobar **inscripción**

En primer lugar, cree el tipo de objeto edge para contener información específica del curso:

```
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

A continuación, vamos a agregar campos personalizados al tipo de objeto edge.  En este ejemplo, añadiremos los cuatro campos personalizados siguientes para modelar un curso universitario: ID de curso, instructor de curso, ubicación del curso, nombre del curso.  Tenga en cuenta que se designa el ID de curso como campo de desduplicación, ya que se requiere al menos un campo de desduplicación.

```
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

```
{
    "requestId": "cc36#16895b82a41",
    "result": [],
    "success": true
}
```

Ahora necesitamos aprobar el tipo de objeto edge para que podamos hacer referencia a él más adelante al vincularlo al tipo de objeto bridge.  Tenga en cuenta que los tipos de objetos personalizados deben aprobarse para poder seleccionarlos como objetos de vínculo.

```
POST /rest/v1/customobjects/schema/course/approve.json
```

```json
{
    "requestId": "460b#16896055fa3",
    "result": [],
    "success": true
}
```

El objeto edge ha finalizado.  Ahora pasemos a crear el tipo de objeto puente para que contenga información específica de la inscripción.

```
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

Para agregar campos personalizados al tipo de objeto bridge, agregue dos campos de vínculo: uno que vincule al objeto Lead y otro que vincule al objeto Course que acabamos de crear. Para vincular al objeto de posible cliente, utilice el campo ID de posible cliente. Para vincular al objeto de curso, utilice el campo ID de curso.  A continuación, añada un identificador único de ID de inscripción como campo de desduplicación, ya que se requiere al menos un campo de desduplicación. Por último, agregue un campo Grado para realizar el seguimiento del rendimiento del alumno.

```
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

```
POST /rest/v1/customobjects/schema/enrollment/approve.json
```

```json
{
    "requestId": "9a76#16897b0e84b",
    "result": [],
    "success": true
}
```

Puede rellenar registros de objetos personalizados mediante programación usando [Sincronizar objeto personalizado](#create_and_update) o [Importación masiva de objeto personalizado](https://experienceleague.adobe.com/docs/marketo-developer/marketo/rest/bulk-import/bulk-custom-object-import.html?lang=en). También puede usar la funcionalidad de la interfaz de usuario de Marketo [Importar datos de objeto personalizados](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/import-custom-object-data).

## Actualizar campo

El punto de conexión [Actualizar campo de tipo de objeto personalizado](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/updateCustomObjectTypeFieldUsingPOST) le permite actualizar un campo en su objeto personalizado de borrador.  El parámetro de ruta de acceso requerido `apiName` es el nombre de API del tipo de objeto personalizado.  El parámetro de ruta de acceso requerido `fieldAPIName` es el nombre de API del campo de tipo de objeto personalizado.  El cuerpo de la solicitud contiene un objeto JSON que contiene pares de clave/valor que especifican los atributos de campo que se van a actualizar.

```
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

El punto de conexión [Eliminar campos de tipo de objeto personalizados](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/deleteCustomObjectTypeFieldsUsingPOST) permite eliminar uno o varios campos del objeto personalizado.  El parámetro de ruta de acceso requerido `apiName` es el nombre de API del tipo de objeto personalizado.  El cuerpo de la solicitud contiene un objeto JSON con una matriz `input` con uno o más elementos.  Cada elemento es un objeto JSON con un atributo `name` que especifica el nombre de API del campo que se va a eliminar.

```
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

El extremo [Obtener tipos de datos de campo de tipo de objeto personalizado](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeFieldDataTypesUsingGET) devuelve la lista de todos los tipos de datos de campo permitidos. Esto resulta útil al modelar el tipo de objeto personalizado para identificar los tipos de datos de campo personalizado admitidos.

```
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

El extremo [Obtener objetos enlazables de objetos personalizados](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeLinkableObjectsUsingGET) devuelve una lista de todos los objetos de vínculo permitidos y sus campos de vínculo.  La lista contendrá Objetos estándar (Posible cliente, Compañía) y cualquier Objeto personalizado que se haya creado en la instancia.

```
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

El extremo [Get Custom Object Dependent Assets](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeDependentAssetsUsingGET) devuelve una lista de recursos dependientes de un tipo de objeto personalizado, incluida su ubicación en la instancia.  Esto resulta útil al eliminar una integración y es necesario identificar en todas partes que se está utilizando un tipo de objeto personalizado.

```
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

* Los extremos de objetos personalizados tienen un tiempo de espera de 30 segundos a menos que se indique a continuación
   * Sincronizar objetos personalizados: 120s
   * Eliminar objetos personalizados: 60 s
