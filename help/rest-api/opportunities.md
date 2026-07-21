---
title: Oportunidades
feature: REST API
description: La API de REST de Marketo permite describir, consultar, crear y actualizar oportunidades, desduplicar y buscar campos, límites y comportamientos de solo lectura con la sincronización de SFDC o Dynamics.
exl-id: 46451285-4125-4857-890a-575069a68288
TQID: https://experienceleague.adobe.com/rBDJcXWQrN5qyKRWHyzVC-sc9BH2mQFLm7fKUk-NUn8
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 708
ht-degree: 0%

---

# Oportunidades

[Referencia de extremo de oportunidad](https://developer.adobe.com/marketo-apis/api/mapi#tag/Opportunities)

Marketo proporciona API para leer, escribir, crear y actualizar registros de oportunidades. En Marketo, el objeto de función de oportunidad intermedia vincula los registros de oportunidad con los registros de posibles clientes y contactos. Por lo tanto, una oportunidad se puede vincular a muchos posibles clientes individuales.

La API expone ambos tipos de objetos. Al igual que con la mayoría de los tipos de objetos de base de datos de posibles clientes, cada uno tiene una llamada Describir correspondiente que devuelve metadatos de objeto.

Las API de oportunidad proporcionan acceso de solo lectura para las suscripciones que tienen [SFDC Sync](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync.html?lang=en) o [Microsoft Dynamics Sync](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync.html?lang=en) habilitado.

## Describir

Describir registros de oportunidad utilizando el patrón estándar para los objetos de base de datos de posibles clientes.

```http
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

Los campos de respuesta clave son:

- `idField`: identifica la clave principal de la oportunidad, marketoGUID. Esta clave generada por el sistema admite operaciones de lectura y actualización, pero no inserta.
- `dedupeFields`: identifica claves válidas para operaciones de inserción. Para las oportunidades, la única clave es externalOpportunityId.
- `searchableFields`: identifica campos válidos para consultas. Estos campos son externalOpportunityId y marketoGUID.

## Consulta

El patrón para [consultar oportunidades](https://developer.adobe.com/marketo-apis/api/mapi#tag/Opportunities/operation/getOpportunitiesUsingGET) sigue de cerca la API de posibles clientes. Sin embargo, el parámetro `filterType` solo acepta campos enumerados en la matriz `searchableFields` de los campos Describe response o dedupeFields correspondientes.

Para los campos de oportunidad personalizados, solo los campos de tipo cadena o entero aparecen en la matriz searchableFields.

```http
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

Puede incluir estos parámetros de consulta opcionales:

- `fields`: devuelve campos de oportunidad adicionales.
- `nextPageToken`: páginas a través de conjuntos de resultados mayores que el tamaño del lote.
- `batchSize`: especifica el tamaño del lote. El valor predeterminado y máximo es 300.

Cuando solicita una lista de `fields`, un campo solicitado que no se devuelve tiene un valor implícito de nulo.

## Crear y actualizar

Las oportunidades siguen el patrón de la API de posibles clientes con algunas restricciones. Los valores `action` son createOnly, createOrUpdate y updateOnly.

- Para los modos createOnly o createOrUpdate, incluya el campo externalOpportunityId en cada registro.
- Para el modo updateOnly, utilice marketoGUID o externalOpportunityId.
- Si no se especifica, el modo predeterminado es createOrUpdate.

El parámetro `lookupField` de la API de posibles clientes no está disponible. El parámetro dedupeBy lo reemplaza y solo es válido cuando action es updateOnly.

Los valores dedupeBy son &quot;dedupeFields&quot; e &quot;idField&quot;, que la respuesta Describe identifica como externalOpportunityId y marketoGUID, respectivamente. Si dedupeBy no se especifica, el valor predeterminado es dedupeFields. El campo &quot;nombre&quot; no debe ser nulo.

Puede enviar hasta 300 registros a la vez.

```http
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

La respuesta incluye los siguientes valores para cada registro:

- `marketoGUID`: el identificador de registro.
- `status`: éxito o error del registro individual.
- `seq`: el índice del registro enviado, que correlaciona el registro de solicitud con el orden de respuesta.

### Campos

El objeto de compañía contiene campos definidos por atributos como nombre para mostrar, nombre de API y dataType. Juntos, estos atributos se denominan metadatos.

Los siguientes extremos consultan campos en el objeto de compañía. El usuario de API debe tener una función con el permiso `Read-Write Schema Standard Field`, el permiso `Read-Write Schema Custom Field` o ambos.

### Campos de consulta

Consulte un campo de empresa por nombre de API o recupere todos los campos de empresa.

#### Por nombre

El extremo [Obtener campo de oportunidad por nombre](https://developer.adobe.com/marketo-apis/api/mapi#tag/Opportunities/operation/getOpportunityFieldByNameUsingGET) recupera los metadatos de un campo en el objeto de la compañía. El parámetro de ruta de acceso `fieldApiName` requerido especifica el nombre de API del campo.

La respuesta se parece a la respuesta Describir oportunidad, pero incluye metadatos adicionales. Por ejemplo, el atributo `isCustom` indica si el campo es personalizado.

```http
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

El extremo [Obtener campos de oportunidad](https://developer.adobe.com/marketo-apis/api/mapi#tag/Opportunities/operation/getOpportunityFieldsUsingGET) recupera los metadatos de todos los campos del objeto de empresa. De forma predeterminada, devuelve un máximo de 300 registros. Utilice el parámetro de consulta `batchSize` para reducir este número.

Si el atributo `moreResult` es verdadero, hay más resultados disponibles. Continúe llamando al extremo con el `nextPageToken` devuelto hasta que moreResult sea false.

```http
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

Elimine oportunidades mediante campos desduplicados o campos de ID. Establezca el parámetro `deleteBy` en deduplicationFields o idField. El valor predeterminado es deduplicarCampos.

El cuerpo de la solicitud contiene una matriz de `input` oportunidades para eliminar. Cada llamada permite un máximo de 300 oportunidades.

```http
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

- Los extremos de oportunidad tienen un tiempo de espera de 30 segundos a menos que se indique lo contrario.
- Las oportunidades de sincronización tienen un tiempo de espera de 60 segundos.
- Eliminar oportunidades tiene un tiempo de espera de 60 segundos.
