---
title: Oportunidades
feature: REST API
description: ' Configure las oportunidades con la API de Marketo.'
exl-id: 46451285-4125-4857-890a-575069a68288
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '786'
ht-degree: 0%

---

# Oportunidades

[Referencia de extremo de oportunidad](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities)

Marketo expone las API para leer, escribir, crear y actualizar registros de oportunidades. En Marketo, los registros de oportunidad están vinculados a los registros de posibles clientes y contactos a través del objeto de función de oportunidad intermedio, por lo que una oportunidad puede estar vinculada a muchos posibles clientes individuales.  Ambos tipos de objetos se exponen a través de la API y, como la mayoría de los tipos de objetos de la base de datos de posibles clientes, ambos tienen la llamada Describir correspondiente, que devuelve metadatos sobre los tipos de objetos.

Las API de oportunidad son de solo lectura para las suscripciones que tienen [SFDC Sync](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync.html?lang=es) o [Microsoft Dynamics Sync](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync.html?lang=es) habilitadas.

## Describir

La descripción de los registros de oportunidad sigue el patrón estándar para los objetos de base de datos de posibles clientes.

```
GET /rest/v1/opportunities/describe.json
```

```json
{
   "requestId":"185d6#14b51985ff0",
   "success":true,
   "result":[
      {
         "name":"opportunity",
         "displayName":"Opportunity",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:24Z",
         "idField":"marketoGUID",
         "dedupeFields":[
            "externalOpportunityId"
         ],
         "searchableFields":[
            [
               "externalOpportunityId"
            ],
            [
               "marketoGUID"
            ]
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
               "name":"externalOpportunityId",
               "displayName":"External Opportunity Id",
               "dataType":"string",
               "length":50,
               "updateable":false
            }
         ]
      }
   ]
}
```

Los campos más importantes para este tipo de respuesta son `idField`, `dedupeFields` y `searchableFields`.  idField indica la clave principal de las oportunidades, marketoGUID.  Se trata de una clave única generada por el sistema que puede utilizarse para operaciones de lectura y actualización, pero no para inserciones, ya que está gestionada por el sistema.  La matriz deduplicationFields indica qué campos son claves válidas para las operaciones de inserción; en el caso de las oportunidades, solo es externalOpportunityId.  La matriz searchableFields proporciona el conjunto de campos válidos para querying, externalOpportunityId y marketoGUID.

## Consulta

El patrón para [oportunidades de consulta](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/getOpportunitiesUsingGET) sigue de cerca el de la API de posibles clientes con la restricción agregada de que el parámetro `filterType` acepta los campos enumerados en la matriz `searchableFields` o de la llamada descrita o deduplicada correspondiente.  Tenga en cuenta que si utiliza campos de oportunidad personalizados, solo los campos de oportunidad personalizados de tipo cadena o entero se enumerarán en la matriz searchableFields.

```
GET /rest/v1/opportunities.json?filterType=marketoGUID&filterValues=dff23271-f996-47d7-984f-f2676861b5fa&dff23271-f996-47d7-984f-f2676861b5fc,dff23271-f996-47d7-984f-f2676861b5fb
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fa ",
         "externalOpportunityId":"19UYA31581L000000",
         "name":"Chairs",
         "description":"Chairs",
         "amount":"1604.47",
         "source":"Inbound Sales Call/Email"
      },
      {
         "seq":1,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fc ",
         "externalOpportunityId":"29UYA31581L000000",
         "name":"Big Dog Day Care-Phase12",
         "description":"Big Dog Day Care-Phase12",
         "amount":"1604.47",
         "source":"Email"
      }
   ]
}
```

También puede incluir los parámetros de consulta opcionales `fields`, para devolver campos de oportunidad adicionales, `nextPageToken`, para la paginación a través de conjuntos mayores que el tamaño del lote, `batchSize`, que toma el valor predeterminado y tiene un máximo de 300.  Al solicitar una lista de `fields`, si se solicita un campo en particular, pero no se devuelve, el valor se considera nulo.

## Crear y actualizar

Las oportunidades siguen de cerca el patrón de la API de posibles clientes, con algunas restricciones.  Los valores disponibles para `action` son: createOnly, createOrUpdate y updateOnly.  Cuando se utiliza el modo createOnly o createOrUpdate, el campo externalOpportunityId debe incluirse en cada registro.  Para el modo updateOnly, se puede utilizar marketoGUID o externalOpportunityId.  El modo predeterminado es createOrUpdate si no se especifica.

El parámetro `lookupField` de la API de posibles clientes no está disponible y se reemplaza por el parámetro dedupeBy, que solo es válido si action es updateOnly.  Los valores disponibles para dedupeBy son &quot;dedupeFields&quot; o &quot;idField&quot;, que se especifican mediante la llamada de descripción como externalOpportunityId y marketoGUID respectivamente.  Si dedupeBy no se especifica, el valor predeterminado es dedupeFields.  El campo &quot;nombre&quot; no debe ser nulo.

Puede enviar hasta 300 registros a la vez.

```
POST /rest/v1/opportunities.json
```

```json
{
   "action":"createOrUpdate",
   "dedupeBy":"dedupeFields",
   "input":[
      {
         "externalOpportunityId":"19UYA31581L000000",
         "name":"Chairs",
         "description":"Chairs",
         "amount":"1604.47",
         "source":"Inbound Sales Call/Email"
      },
      {
         "externalOpportunityId":"29UYA31581L000000",
         "name":"Big Dog Day Care-Phase12",
         "description":"Big Dog Day Care-Phase12",
         "amount":"1604.47",
         "source":"Email"
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
         "status":"updated",
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb"
      },
      {
         "seq":1,
         "status":"created",
         "marketoGUID":"cff23271-f996-47d7-984f-f2676861b5fb"
      }
   ]
}
```

La API responderá con `marketoGUID` para cada registro, así como con un campo `status` que indica el éxito o el error individual de cada registro y un campo `seq` que se utiliza para correlacionar los registros enviados con el orden de la respuesta.  El número del campo es el índice del registro enviado en la solicitud.

### Campos

El objeto de compañía contiene un conjunto de campos.  Cada definición de campo consta de un conjunto de atributos que describen el campo.  Algunos ejemplos de atributos son nombre para mostrar, nombre de API y dataType.  Estos atributos se conocen colectivamente como metadatos.

Los siguientes extremos permiten consultar campos en el objeto de compañía. Estas API requieren que el usuario de la API propietaria tenga una función con uno o ambos permisos `Read-Write Schema Standard Field` o `Read-Write Schema Custom Field`.

### Campos de consulta

La consulta de los campos de oportunidad es sencilla.  Puede consultar un único campo de empresa por nombre de API o consultar el conjunto de todos los campos de empresa.

#### Por nombre

El extremo [Obtener campo de oportunidad por nombre](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/getOpportunityFieldByNameUsingGET) recupera los metadatos de un único campo en el objeto de la compañía.  El parámetro de ruta de acceso obligatorio `fieldApiName` especifica el nombre de API del campo.  La respuesta es como el extremo Describir oportunidad, pero contiene metadatos adicionales, como el atributo `isCustom`, que indica si el campo es un campo personalizado.

```
GET /rest/v1/opportunities/schema/fields/externalOpportunityId.json
```

```json
{
    "requestId": "12331#17e9779cb4b",
    "result": [
        {
            "displayName": "SFDC Oppty Id",
            "name": "externalOpportunityId",
            "description": null,
            "dataType": "string",
            "length": 50,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        }
    ],
    "success": true
}
```

#### Examinar

El extremo [Obtener campos de oportunidad](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/getOpportunityFieldsUsingGET) recupera los metadatos de todos los campos del objeto de empresa.  De forma predeterminada, se devuelve un máximo de 300 registros.  Puede usar el parámetro de consulta `batchSize` para reducir este número.  Si el atributo `moreResult` es true, significa que hay más resultados disponibles.  Continúe llamando a este extremo hasta que el atributo moreResult devuelva false, lo que significa que no hay resultados disponibles.  Los `nextPageToken` devueltos por esta API siempre se deben reutilizar para la siguiente iteración de esta llamada.

```
GET /rest/v1/opportunities/schema/fields.json?batchSize=5
```

```json
{
    "requestId": "b4a#17e995b31da",
    "result": [
        {
            "displayName": "SFDC Oppty Id",
            "name": "externalOpportunityId",
            "description": null,
            "dataType": "string",
            "length": 50,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Name",
            "name": "name",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Description",
            "name": "description",
            "description": null,
            "dataType": "string",
            "length": 2000,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Type",
            "name": "type",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Stage",
            "name": "stage",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        }
    ],
    "success": true,
    "nextPageToken": "E5ZONGE4SAHALYYW6FS25KB5BM======",
    "moreResult": true
}
```

#### Eliminar

Puede eliminar oportunidades desduplicando campos o campos de ID. Especifique utilizando el parámetro `deleteBy` con un valor de dedupeFields o idField. Si no se especifica, el valor predeterminado es deduplicarCampos. El cuerpo de la solicitud contiene una matriz de `input` oportunidades para eliminar. Se permiten un máximo de 300 oportunidades por llamada.

```
POST /rest/v1/opportunities/delete.json
```

```json
{
   "deleteBy":"dedupeFields",
   "input":[
      {
         "externalOpportunityId":"19UYA31581L000000"
      },
      {
         "externalOpportunityId":"29UYA31581L000000"
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
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb",
         "status":"deleted"
      },
      {
         "seq":1,
         "marketoGUID":"cff23271-f996-47d7-984f-f2676861b5fb",
         "status":"deleted"
      }
   ]
}
```

## Tiempos de espera

- Los extremos de oportunidad tienen un tiempo de espera de 30 segundos a menos que se indique a continuación
   - Oportunidades de sincronización: 60s
   - Eliminar oportunidades: 60 s
