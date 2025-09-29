---
title: Cuentas nombradas
feature: REST API
description: Guía de Marketo REST para CRUD sobre cuentas con nombre para ABM, con descripción, consultas, creación de ejemplos de actualización, campos en los que se puede buscar, reglas de desduplicación y sin vinculación de posibles clientes.
exl-id: 2aa1d2a0-9e54-4a9a-abb1-0d0479ed3558
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '697'
ht-degree: 1%

---

# Cuentas nombradas

[Referencia de extremo de cuentas con nombre](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Named-Accounts)

Marketo ofrece un conjunto de API para realizar operaciones de CRUD en cuentas con nombre para su uso con Marketo ABM. Estas API siguen el patrón de interfaz estándar para las API de la base de datos de posibles clientes, y proporcionan las opciones Describir, Crear/actualizar, Eliminar y Consulta.

Actualmente, las únicas funciones relacionadas con ABM disponibles a través de las API de Marketo son las operaciones de CRUD para cuentas con nombre; los posibles clientes no se pueden vincular a cuentas con nombre a través de ninguna API.

## Describir

La descripción de cuentas con nombre devuelve metadatos relacionados con el uso de cuentas con nombre mediante las API de Marketo, incluida una lista de los campos válidos que se pueden buscar al consultar y una lista de todos los campos disponibles para el uso de API. El `idField` de una cuenta con nombre siempre es `marketoGUID`, y el único disponible `dedupeField`, y la clave para la creación es el campo `name` del objeto.

```
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

La consulta de cuentas con nombre se basa en el uso de un filterType y un conjunto de hasta 300 filterValues separados por comas. `filterType` puede ser cualquier campo único devuelto en el miembro `searchableFields` del resultado de descripción para cuentas con nombre, mientras que filterValues puede ser cualquier entrada válida para el tipo de datos del campo. Para devolver un conjunto específico de campos de, se debe pasar un parámetro fields, donde el valor es una lista de campos separados por comas que se van a devolver en la respuesta. Al igual que otras opciones de consulta, el número máximo de registros para una sola página de consulta es 300 y se deben solicitar registros adicionales en el conjunto con el uso del nextPageToken devuelto por la llamada.

```
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

La creación y actualización de cuentas con nombre sigue el patrón de base de datos de posibles clientes estándar. Los registros deben pasarse en el miembro de entrada de un cuerpo JSON en una petición POST. `input` es el único miembro requerido, con `action` y `dedupeBy` como miembros opcionales. Se pueden incluir hasta 300 registros en la entrada. La acción puede ser createOnly, updateOnly o createOrUpdate. Si no se especifica, el valor predeterminado de action es createOrUpdate. dedupeBy solo se puede especificar cuando action es updateOnly y solo acepta uno de dedupeFields o idField, que corresponden a los campos name y marketoGUID, respectivamente.

```
POST /rest/v1/namedaccounts.json
```

```
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

El objeto de cuenta con nombre contiene un conjunto de campos. Cada definición de campo está compuesta por un conjunto de atributos que describen el campo. Algunos ejemplos de atributos son nombre para mostrar, nombre de API y dataType. Estos atributos se conocen colectivamente como metadatos.

Los siguientes extremos permiten consultar campos en el objeto de compañía. Estas API requieren que el usuario de la API propietaria tenga una función con uno o ambos permisos Campo estándar de esquema de lectura-escritura o Campo personalizado de esquema de lectura-escritura.

### Campos de consulta

La consulta de campos de cuenta con nombre es sencilla. Puede consultar un único campo de cuenta con nombre por nombre de API o consultar el conjunto de todos los campos de compañía.

#### Por nombre

El extremo [Obtener campo de cuenta con nombre por nombre](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Named-Accounts/operation/getNamedAccountFieldByNameUsingGET) recupera los metadatos de un único campo en el objeto de cuenta con nombre. El parámetro de ruta fieldApiName requerido especifica el nombre de API del campo. La respuesta es como el extremo de Describir cuenta con nombre, pero contiene metadatos adicionales como el atributo isCustom que indica si el campo es un campo personalizado.

```
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

El extremo [Obtener campos de cuenta con nombre](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Named-Accounts/operation/getNamedAccountFieldByNameUsingGET) recupera los metadatos de todos los campos del objeto de cuenta con nombre. De forma predeterminada, se devuelve un máximo de 300 registros. Puede utilizar el parámetro de consulta batchSize para reducir este número. Si el atributo moreResult es true, hay más resultados disponibles. Continúe llamando a este extremo hasta que el atributo moreResult devuelva false, lo que significa que no hay resultados disponibles. El nextPageToken devuelto desde esta API siempre debe reutilizarse para la siguiente iteración de esta llamada.

```
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

Las eliminaciones se realizan mediante una solicitud de POST de JSON y tienen un miembro de entrada requerido y un miembro opcional deleteBy. deleteBy puede ser uno de &quot;dedupeFields&quot; o &quot;idField&quot;, correspondiente a name o marketoGUID, respectivamente, y de forma predeterminada dedupeFields si no se establece. El miembro de entrada acepta una matriz de hasta 300 registros, que contienen un miembro cada uno, ya sea name o marketoGUID, según la configuración de deleteBy.

```
POST /rest/v1/namedaccounts/delete.json
```

```
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

- Los extremos de cuenta con nombre tienen un tiempo de espera de 30 segundos a menos que se indique a continuación
   - Sincronizar cuentas con nombre: 120 s
   - Eliminar cuentas con nombre: 60s
