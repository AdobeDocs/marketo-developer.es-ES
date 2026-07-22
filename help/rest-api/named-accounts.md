---
title: Cuentas nombradas
feature: REST API
description: Guía de Marketo REST para CRUD sobre cuentas con nombre para ABM, con descripción, consultas, creación de ejemplos de actualización, campos en los que se puede buscar, reglas de desduplicación y sin vinculación de posibles clientes.
exl-id: 2aa1d2a0-9e54-4a9a-abb1-0d0479ed3558
TQID: https://experienceleague.adobe.com/iY3UYVelm3aKuuDBCTxaVCbkXfwnJzDjV3Kvn9rcNbA
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 590
ht-degree: 1%

---

# Cuentas nombradas

[Referencia de extremo de cuentas con nombre](https://developer.adobe.com/marketo-apis/api/mapi#tag/Named-Accounts)

Marketo proporciona API para realizar operaciones de CRUD en cuentas con nombre para su uso con Marketo ABM. Estas API siguen el patrón de interfaz de la base de datos de posibles clientes estándar y proporcionan las opciones Describir, Crear/Actualizar, Eliminar y Consulta.

Actualmente, las API de Marketo solo admiten operaciones CRUD para cuentas con nombre. No puede vincular posibles clientes a cuentas con nombre a través de las API.

## Describir

Describir cuentas con nombre devuelve metadatos para utilizar cuentas con nombre a través de las API de Marketo. La respuesta incluye campos válidos para búsqueda y todos los campos disponibles para la API.

El `idField` de una cuenta con nombre siempre es `marketoGUID`. El campo `name` del objeto es la única clave de creación y `dedupeField` disponible.

```http
GET /rest/v1/namedaccounts/describe.json
```

```json
{
   "requestId":"d65e#156c27ac57d",
   "result":[
      {
         "name":"Named Account",
         "description":"Marketo standard account attribute map",
         "createdAt":"2016-08-18T20:16:41Z",
         "updatedAt":"2016-08-18T20:16:41Z",
         "idField":"marketoGUID",
         "dedupeFields":[
            "name"
         ],
         "searchableFields":[
            [
               "marketoGUID",
            ],
            [
               "annualRevenue"
            ],
            [
               "city"
            ],
            [
               "country"
            ],
            [
               "domainName"
            ],
            [
               "industry"
            ],
            [
               "logoUrl"
            ],
            [
               "membershipCount"
            ],
            [
               "name"
            ],
            [
               "numberOfEmployees"
            ],
            [
               "opptyAmount"
            ],
            [
               "opptyCount"
            ],
            [
               "score1"
            ],
            [
               "score2"
            ],
            [
               "score3"
            ],
            [
               "score4"
            ],
            [
               "score5"
            ],
            [
               "sicCode"
            ],
            [
               "state"
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
               "name":"annualRevenue",
               "displayName":"annualRevenue",
               "dataType":"currency",
               "updateable":true
            },
            {
               "name":"city",
               "displayName":"city",
               "dataType":"string",
               "length":255,
               "updateable":true
            },
            {
               "name":"country",
               "displayName":"country",
               "dataType":"string",
               "length":255,
               "updateable":true
            }
         ]
      }
   ],
   "success":true
}
```

### Consulta

Consulte las cuentas con nombre mediante un filterType y hasta 300 filterValues separados por comas. filterType puede ser cualquier campo devuelto en el miembro `searchableFields` de la respuesta Describir. Cada entrada filterValues debe ser un valor válido para el tipo de datos del campo.

Para devolver campos específicos, pase un parámetro fields con una lista de campos separados por comas. Una página de consulta contiene un máximo de 300 registros. Para recuperar registros adicionales, utilice el nextPageToken devuelto por la llamada.

```http
GET /rest/v1/namedaccounts.json?filterType=name&filterValues=Google,Yahoo
```

```json
{
    "requestId": "6dac#157d4ddc9d7",
    "result": [
        {
            "seq": 0,
            "marketoGUID": "16efafdd-0148-4ea7-8782-f451d7c6345d",
            "createdAt": "2016-10-17T22:49:04Z",
            "name": "Google",
            "updatedAt": "2016-10-17T22:49:04Z"
        },
        {
            "seq": 1,
            "marketoGUID": "44d62353-7f9d-4d43-b9cc-7ef0f7a09137",
            "createdAt": "2016-10-17T22:49:04Z",
            "name": "Yahoo",
            "updatedAt": "2016-10-17T22:49:04Z"
        }
    ],
    "success": true
}
```

### Crear y actualizar

Cree y actualice cuentas con nombre utilizando el patrón estándar de base de datos de posibles clientes. Pase registros en el miembro de entrada del cuerpo JSON de una petición POST. Se pueden incluir hasta 300 registros.

Los miembros de la solicitud son:

- `input`: el único miembro requerido.
- `action`: un miembro opcional que acepta createOnly, updateOnly o createOrUpdate. El valor predeterminado es createOrUpdate.
- `dedupeBy`: un miembro opcional disponible solo cuando la acción es updateOnly. Acepta deduplicationFields o idField, que corresponden a los campos name y marketoGUID, respectivamente.

```http
POST /rest/v1/namedaccounts.json
```

```text
Content-Type: application/json
```

```json
{
   "action":"updateOnly",
   "dedupeBy":"dedupeFields",
   "input":[
      {
         "name":"Google",
         "domainName":"www.google.com"
      },
      {
         "name":"Yahoo",
         "domainName":"www.yahoo.com"
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
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fc"
      }
   ]
}
```

### Campos

El objeto de cuenta con nombre contiene campos definidos por atributos como nombre para mostrar, nombre de API y dataType. Juntos, estos atributos se denominan metadatos.

Los siguientes extremos consultan campos en el objeto de compañía. El usuario de API debe tener una función con el permiso Campo estándar de esquema de lectura-escritura, el permiso Campo personalizado de esquema de lectura-escritura o ambos.

### Campos de consulta

Consulte un campo de cuenta con nombre por nombre de API o recupere todos los campos de empresa.

#### Por nombre

El extremo [Obtener campo de cuenta con nombre por nombre](https://developer.adobe.com/marketo-apis/api/mapi#tag/Named-Accounts/operation/getNamedAccountFieldByNameUsingGET) recupera los metadatos de un campo en el objeto de cuenta con nombre. El parámetro de ruta fieldApiName requerido especifica el nombre de API del campo.

La respuesta es similar a la respuesta Describir cuenta con nombre, pero incluye metadatos adicionales. Por ejemplo, el atributo isCustom indica si el campo es personalizado.

```http
GET /rest/v1/namedaccounts/schema/fields/annualRevenue.json
```

```json
{
    "requestId": "371c#17e979c5d1f",
    "result": [
        {
            "displayName": "Annual Revenue",
            "name": "annualRevenue",
            "description": null,
            "dataType": "currency",
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

El extremo [Obtener campos de cuenta con nombre](https://developer.adobe.com/marketo-apis/api/mapi#tag/Named-Accounts/operation/getNamedAccountFieldByNameUsingGET) recupera los metadatos de todos los campos del objeto de cuenta con nombre. De forma predeterminada, devuelve un máximo de 300 registros. Utilice el parámetro de consulta batchSize para reducir este número.

Si el atributo moreResult es true, hay más resultados disponibles. Continúe llamando al extremo con el nextPageToken devuelto hasta que moreResult sea false.

```http
GET /rest/v1/namedaccounts/schema/fields.json?batchSize=5
```

```json
{
    "requestId": "f287#17e995bd0c5",
    "result": [
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
            "displayName": "Domain Name",
            "name": "domainName",
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
        },
        {
            "displayName": "SIC Code",
            "name": "sicCode",
            "description": null,
            "dataType": "string",
            "length": 40,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "City",
            "name": "city",
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
    "nextPageToken": "N42LHXWEULHZ3N2I77DKOJUVOY======",
    "moreResult": true
}
```

### Eliminar

Elimine las cuentas con nombre enviando una petición POST con un cuerpo JSON. La solicitud incluye un miembro de entrada requerido y un miembro opcional deleteBy.

El miembro deleteBy acepta &quot;dedupeFields&quot; o &quot;idField&quot;, que corresponden a name y marketoGUID, respectivamente. Si no se establece, el valor predeterminado es deduplicarCampos. El miembro de entrada acepta hasta 300 registros. Cada registro contiene name o marketoGUID, según la configuración deleteBy.

```http
POST /rest/v1/namedaccounts/delete.json
```

```text
Content-Type: application/json
```

```json
{
   "deleteBy":"dedupeFields",
   "input":[
      {
         "name":"Google"
      },
      {
         "name":"Yahoo"
      },
      {
         "name":"Marketo"
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
         "id":"dff23271-f996-47d7-984f-f2676861b5fc",
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

- Los extremos de cuenta con nombre tienen un tiempo de espera de 30 segundos a menos que se indique lo contrario.
- La sincronización de cuentas con nombre tiene un tiempo de espera de 120 segundos.
- Eliminar cuentas con nombre tiene un tiempo de espera de 60 segundos.
