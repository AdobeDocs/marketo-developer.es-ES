---
title: Compañías
feature: REST API
description: Utilice la API de REST de Compañías de Marketo para describir, consultar y sincronizar registros de compañía, administrar campos y desduplicar por externalCompanyId y anotar la sincronización de CRM de solo lectura.
exl-id: 80e514a2-1c86-46a7-82bc-e4db702189b0
TQID: https://experienceleague.adobe.com/LdJYN4lx9JfcE-02zTz8ktfYXm4EdPtxMYOx9gGR0sg
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 582
ht-degree: 1%

---

# Compañías

[Referencia de extremo de compañías](https://developer.adobe.com/marketo-apis/api/mapi#tag/Companies)

Las compañías representan las organizaciones a las que pertenecen los registros de posibles clientes. Para agregar un posible cliente a una compañía, rellene su campo `externalCompanyId` mediante los extremos [Sincronizar posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/syncLeadUsingPOST) o [Importación masiva de posibles clientes](bulk-lead-import.md).

No puede eliminar un posible cliente de una compañía a menos que agregue el posible cliente a otra compañía. Los posibles clientes vinculados a un registro de compañía heredan los valores de ese registro como si existieran en el registro de posibles clientes.

Las API de empresa proporcionan acceso de solo lectura a las suscripciones que tienen [SFDC Sync](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync.html?lang=en) o [Microsoft Dynamics Sync](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync.html?lang=en) habilitado.

## Describir

Describa el objeto de compañía para recuperar la información necesaria para interactuar con los registros de compañía.

```http
GET /rest/v1/companies/describe.json
```

```json
{
   "success":true,
   "requestId":"5847#14d44113ad7",
   "result":[
      {
         "name":"Company",
         "description":"Company object",
         "createdAt":"2015-05-11T17:11:32Z",
         "updatedAt":"2015-05-11T17:11:32Z",
         "idField":"id",
         "dedupeFields":[
            "externalCompanyId"
         ],
         "searchableFields":[
            [
               "externalCompanyId"
            ],
            [
               "id"
            ],
            [
               "company"
            ]
         ],
         "fields":[
            {
               "name":"createdAt",
               "displayName":"Created At",
               "dataType":"datetime",
               "updateable":false
            },
            {
               "name":"externalCompanyId",
               "displayName":"External Company Id",
               "dataType":"string",
               "length":100,
               "updateable":false
            },
            {
               "name":"id",
               "displayName":"Id",
               "dataType":"integer",
               "updateable":false
            },
            {
               "name":"updatedAt",
               "displayName":"Updated At",
               "dataType":"datetime",
               "updateable":false
            },
            {
               "name":"annualRevenue",
               "displayName":"Annual Revenue",
               "dataType":"currency",
               "updateable":true
            }
            {
               "name":"company",
               "displayName":"Company Name",
               "dataType":"string",
               "length":255,
               "updateable":true
            }
         ]
      }
   ]
}
```

## Consulta

El patrón para [consultar compañías](https://developer.adobe.com/marketo-apis/api/mapi#tag/Companies/operation/getCompaniesUsingGET) sigue de cerca la API de posibles clientes. Sin embargo, el parámetro `filterType` solo acepta campos enumerados en la matriz searchableFields de la respuesta Describir compañías o deduplicar campos.

Los parámetros de consulta son:

- `filterType` y `filterValues`: parámetros requeridos.
- `fields`, `nextPageToken` y `batchSize`: parámetros opcionales que funcionan como los parámetros correspondientes en las API de posibles clientes y oportunidades.

Cuando solicita una lista de `fields`, un campo solicitado que no se devuelve tiene un valor implícito de nulo.

Si omite el parámetro fields, la respuesta devolverá estos campos de forma predeterminada:

- Identificación
- deduplicarCampos
- updatedAt
- createdAt

```http
GET /rest/v1/companies.json?filterType=id&filterValues=3433,5345
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "id":3433,
         "externalCompanyId":"19UYA31581L000000",
         "company":"Google"
      },
      {
         "seq":1,
         "id":5345,
         "externalCompanyId":"29UYA31581L000000",
         "company":"Yahoo"
      }
   ]
}
```

## Crear y actualizar

El extremo [Sync Companies](https://developer.adobe.com/marketo-apis/api/mapi#tag/Companies/operation/syncCompaniesUsingPOST) acepta un parámetro `input` requerido que contiene una matriz de objetos de la compañía.

Al igual que con las oportunidades, el punto final admite tres modos de creación y actualización: createOnly, updateOnly y createOrUpdate. Especifique el modo en el parámetro `action` de la solicitud.

Los parámetros `dedupeBy` y `action` son opcionales. De forma predeterminada, desduplican Fields y createOrUpdate, respectivamente.

```http
POST /rest/v1/companies.json
```

```text
Content-Type: application/json
```

```json
{
   "action":"createOrUpdate",
   "dedupeBy":"dedupeFields",
   "input":[
      {
         "externalCompanyId":"19UYA31581L000000",
         "company":"Google"
      },
      {
         "externalCompanyId":"29UYA31581L000000",
         "company":"Yahoo"
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
         "id":1232
      },
      {
         "seq":1,
         "status":"created",
         "id":1323
      }
   ]
}
```

### Campos

El objeto de compañía contiene campos definidos por atributos como nombre para mostrar, nombre de API y dataType. Juntos, estos atributos se denominan metadatos.

Los siguientes extremos consultan campos en el objeto de compañía. El usuario de API debe tener una función con el permiso `Read-Write Schema Standard Field`, el permiso `Read-Write Schema Custom Field` o ambos.

### Campos de consulta

Consulte un campo de empresa por nombre de API o recupere todos los campos de empresa.

#### Por nombre

El extremo [Obtener campo de la compañía por nombre](https://developer.adobe.com/marketo-apis/api/mapi#tag/Companies/operation/getCompanyFieldByNameUsingGET) recupera los metadatos de un campo en el objeto de la compañía. El parámetro de ruta de acceso `fieldApiName` requerido especifica el nombre de API del campo.

La respuesta se parece a la respuesta Describir compañía, pero incluye metadatos adicionales. Por ejemplo, el atributo `isCustom` indica si el campo es personalizado.

```http
GET /rest/v1/companies/schema/fields/industry.json
```

```json
{
    "requestId": "88f6#17e976d6ab4",
    "result": [
        {
            "displayName": "Industry",
            "name": "industry",
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
    "success": true
}
```

#### Examinar

El extremo [Obtener campos de empresa](https://developer.adobe.com/marketo-apis/api/mapi#tag/Companies/operation/getCompanyFieldsUsingGET) recupera los metadatos de todos los campos del objeto de empresa. De forma predeterminada, devuelve un máximo de 300 registros. Utilice el parámetro de consulta `batchSize` para reducir este número.

Si el atributo `moreResult` es verdadero, hay más resultados disponibles. Continúe llamando al extremo con el `nextPageToken` devuelto hasta que `moreResult` sea falso.

```http
GET /rest/v1/companies/schema/fields.json?batchSize=5
```

```json
{
    "requestId": "b50e#17e995c2d35",
    "result": [
        {
            "displayName": "Company Name",
            "name": "company",
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
            "displayName": "Site",
            "name": "site",
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
            "displayName": "Website",
            "name": "website",
            "description": null,
            "dataType": "url",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Main Phone",
            "name": "mainPhone",
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
            "displayName": "Annual Revenue",
            "name": "annualRevenue",
            "description": null,
            "dataType": "currency",
            "isHidden": false,
            "isHtmlEncodingInEmail": false,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        }
    ],
    "success": true,
    "nextPageToken": "L7XD3EFJ3OLFZKXKJBWYULOTRA======",
    "moreResult": true
}
```

### Eliminar

Especifique criterios de eliminación como una lista de valores de búsqueda en la matriz `input`. Especifique el método de eliminación en el parámetro `deleteBy`.

Los valores permitidos son dedupeFields y idField. El valor predeterminado es deduplicarCampos.

```text
Content-Type: application/json
```

```http
POST /rest/v1/companies/delete.json
```

```json
{
   "deleteBy":"dedupeFields",
   "input":[
      {
         "externalCompanyId":"19UYA31581L000000"
      },
      {
         "externalCompanyId":"29UYA31581L000000"
      },
      {
         "externalCompanyId":"39UYA31581L000000"
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
         "id":1234,
         "status":"deleted"
      },
      {
         "seq":1,
         "id":56456,
         "status":"deleted"
      },
      {
         "seq":2,
         "status":"skipped",
         "reasons":[
            {
               "code":"1013",
               "message":"Record not found"
            }
         ]
      }
   ]
}
```

## Tiempos de espera

- Los extremos de las compañías tienen un tiempo de espera de 30 segundos a menos que se indique lo contrario.
- Las Compañías de sincronización tienen un tiempo de espera de 60 segundos.
- Eliminar compañías tiene un tiempo de espera de 60 segundos.
