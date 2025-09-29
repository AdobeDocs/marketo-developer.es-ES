---
title: Compañías
feature: REST API
description: Utilice la API de REST de Compañías de Marketo para describir, consultar y sincronizar registros de compañía, administrar campos y desduplicar por externalCompanyId y anotar la sincronización de CRM de solo lectura.
exl-id: 80e514a2-1c86-46a7-82bc-e4db702189b0
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 1%

---

# Compañías

[Referencia de extremo de compañías](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies)

Las compañías representan la organización a la que pertenecen los registros de posibles clientes. Los posibles clientes se agregan a una compañía rellenando su campo `externalCompanyId` correspondiente con [Sincronizar posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/syncLeadUsingPOST) o [Importación masiva de posibles clientes](bulk-lead-import.md) puntos finales. Una vez agregado un posible cliente a una compañía, no puede eliminarlo de esa compañía (a menos que agregue el posible cliente a una compañía diferente). Los posibles clientes vinculados a un registro de compañía heredarán directamente los valores de un registro de compañía como si los valores existieran en el propio registro del posible cliente.

Las API de compañía son de solo lectura para las suscripciones que tienen [SFDC Sync](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync.html?lang=es) o [Microsoft Dynamics Sync](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync.html?lang=es) habilitadas.

## Describir

Al describir el objeto de compañía, obtiene toda la información necesaria para interactuar con ellos.

```
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

El patrón para [consultar compañías](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/getCompaniesUsingGET) sigue de cerca el de la API de posibles clientes con la restricción agregada de que el parámetro `filterType` acepta los campos enumerados en la matriz searchableFields de la llamada a Describir compañías, o dedupeFields.

`filterType` y `filterValues` son parámetros de consulta requeridos.  `fields`, `nextPageToken` y `batchSize` son parámetros opcionales.  Los parámetros funcionan igual que los parámetros correspondientes en las API de posibles clientes y oportunidades. Al solicitar una lista de `fields`, si se solicita un campo en particular, pero no se devuelve, el valor se considera nulo.

Si se omite el parámetro fields, se devuelve el conjunto predeterminado de campos:

- Identificación
- deduplicarCampos
- updatedAt
- createdAt

```
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

El extremo [Sync Companies](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/syncCompaniesUsingPOST) acepta el parámetro `input` necesario que contiene una matriz de objetos de la compañía. Al igual que las oportunidades, existen tres modos para crear y actualizar compañías: createOnly, updateOnly y createOrUpdate.  Los modos se especifican en el parámetro `action` de la solicitud. Los parámetros `dedupeBy` y `action` son opcionales, y los predeterminados son los modos deduplicarFields y crearOrUpdate, respectivamente.

```
POST /rest/v1/companies.json
```

```
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

El objeto de compañía contiene un conjunto de campos. Cada definición de campo está compuesta por un conjunto de atributos que describen el campo. Algunos ejemplos de atributos son nombre para mostrar, nombre de API y dataType. Estos atributos se conocen colectivamente como metadatos.

Los siguientes extremos permiten consultar campos en el objeto de compañía. Estas API requieren que el usuario de la API propietaria tenga una función con uno o ambos permisos `Read-Write Schema Standard Field` o `Read-Write Schema Custom Field`.

### Campos de consulta

La consulta de campos de empresa es sencilla. Puede consultar un único campo de empresa por nombre de API o consultar el conjunto de todos los campos de empresa.

#### Por nombre

El extremo [Obtener campo de la compañía por nombre](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/getCompanyFieldByNameUsingGET) recupera los metadatos de un único campo en el objeto de la compañía. El parámetro de ruta de acceso obligatorio `fieldApiName` especifica el nombre de API del campo. La respuesta es como el extremo Describir compañía, pero contiene metadatos adicionales como el atributo `isCustom` que indica si el campo es un campo personalizado.

```
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

El extremo [Obtener campos de empresa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/getCompanyFieldsUsingGET) recupera los metadatos de todos los campos del objeto de empresa. De forma predeterminada, se devuelve un máximo de 300 registros. Puede usar el parámetro de consulta `batchSize` para reducir este número. Si el atributo `moreResult` es true, significa que hay más resultados disponibles. Continúe llamando a este extremo hasta que el atributo moreResult devuelva false, lo que significa que no hay resultados disponibles. Los `nextPageToken` devueltos por esta API siempre se deben reutilizar para la siguiente iteración de esta llamada.

```
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

Los criterios de eliminación se especifican en la matriz `input`, que contiene una lista de valores de búsqueda.  El método de eliminación se ha especificado en el parámetro `deleteBy`.  Los valores permitidos son: dedupeFields, idField.  El valor predeterminado es dedupeFields.

```
Content-Type: application/json
```

```
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

- Los extremos de las compañías tienen un tiempo de espera de 30 segundos a menos que se indique a continuación
   - Compañías de sincronización: 60s
   - Eliminar compañías: 60s
