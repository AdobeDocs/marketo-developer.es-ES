---
title: Miembros del programa
feature: REST API
description: Utilice la API de REST de Marketo para leer, crear, actualizar y eliminar miembros del programa, administrar campos estándar y personalizados, y consultar mediante campos en los que se puede buscar.
exl-id: 22f29a42-2a30-4dce-a571-d7776374cf43
TQID: https://experienceleague.adobe.com/scEHyXYq9C7cCS1kIX810wG7ahT9fsa448NwIfBmzQM
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: b0bb9048-d951-48d8-8232-45cf248a7e27id: c5f60233-d5ea-4453-a799-0ad258b4d399id: d1d0a9cd-295d-4976-8c39-ddae266f240eid: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dcid: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1670
ht-degree: 2%

---

# Miembros del programa

[Referencia de extremo de miembros de programa](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members)

Marketo proporciona API para leer, crear, actualizar y eliminar registros de miembros del programa. El campo de ID de posible cliente relaciona los registros de miembros del programa con los registros de posibles clientes.

Cada registro contiene campos estándar y puede contener hasta 20 campos personalizados. Estos campos almacenan datos de miembros específicos del programa para su uso en formularios, filtros, déclencheur y acciones de flujo. Puede ver estos datos en la [ficha de miembros](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/manage-and-view-members) del programa en la interfaz de usuario de Marketo Engage.

## Describir

El extremo [Describir miembro del programa](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/describeProgramMemberUsingGET2) sigue el patrón estándar para los objetos de la base de datos de posibles clientes.

- La matriz `searchableFields` identifica campos válidos para consultas.
- La matriz `fields` contiene metadatos como el nombre de la API de REST, el nombre para mostrar y si el campo se puede actualizar.

```http
GET /rest/v1/programs/members/describe.json
```

```json
{
    "requestId": "f813#1791563c7cc",
    "result": [
        {
            "name": "API Program Membership",
            "description": "Map for API program membership fields",
            "createdAt": "2021-03-20T01:30:05Z",
            "updatedAt": "2021-03-20T01:30:05Z",
            "dedupeFields": [
                "leadId",
                "programId"
            ],
            "searchableFields": [
                [
                    "leadId"
                ],
                [
                    "myCustomField"
                ],
                [
                    "reachedSuccess"
                ],
                [
                    "statusName"
                ]
            ],
            "fields": [
                {
                    "name": "acquiredBy",
                    "displayName": "acquiredBy",
                    "dataType": "boolean",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "attendanceLikelihood",
                    "displayName": "attendanceLikelihood",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "createdAt",
                    "displayName": "createdAt",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "isExhausted",
                    "displayName": "isExhausted",
                    "dataType": "boolean",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "leadId",
                    "displayName": "leadId",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "membershipDate",
                    "displayName": "membershipDate",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "nurtureCadence",
                    "displayName": "nurtureCadence",
                    "dataType": "string",
                    "length": 4,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "program",
                    "displayName": "program",
                    "dataType": "string",
                    "length": 255,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "programId",
                    "displayName": "programId",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "reachedSuccess",
                    "displayName": "reachedSuccess",
                    "dataType": "boolean",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "reachedSuccessDate",
                    "displayName": "reachedSuccessDate",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "registrationLikelihood",
                    "displayName": "registrationLikelihood",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "statusName",
                    "displayName": "statusName",
                    "dataType": "string",
                    "length": 255,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "statusReason",
                    "displayName": "statusReason",
                    "dataType": "string",
                    "length": 255,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "trackName",
                    "displayName": "trackName",
                    "dataType": "string",
                    "length": 255,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "updatedAt",
                    "displayName": "updatedAt",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "waitlistPriority",
                    "displayName": "waitlistPriority",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "myCustomField",
                    "displayName": "myCustomField",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "registrationCode",
                    "displayName": "registrationCode",
                    "dataType": "string",
                    "length": 100,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "webinarUrl",
                    "displayName": "webinarUrl",
                    "dataType": "string",
                    "length": 2000,
                    "updateable": true,
                    "crmManaged": false
                }
            ]
        }
    ],
    "success": true
}
```

## Consulta

Use el extremo [Obtener miembros del programa](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/getProgramMembersUsingGET) para recuperar miembros de un programa. La solicitud requiere un parámetro de ruta de acceso `programId` y `filterType` y `filterValues` parámetros de consulta.

`programId` especifica el programa que se va a buscar.

`filterType` especifica el campo que se usará como filtro de búsqueda. Acepta cualquier campo de la lista &quot;searchableFields&quot; devuelta por el extremo [Describir miembro del programa](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/describeProgramMemberUsingGET2). Para un campo personalizado, el dataType debe ser &quot;cadena&quot; o &quot;entero&quot;.

Cuando filterType no es &quot;leadId&quot;, la solicitud puede procesar un máximo de 100 000 registros de miembros del programa. Según la configuración de la instancia de Marketo, recibirá uno de estos errores:

- Si el número total de miembros del programa supera los 100 000, se devuelve un error: &quot;1003, Total de miembros: 100 001 supera el límite permitido de 100 000 para el filtro&quot;.
- Si el número total de miembros del programa _que coinciden con el filtro_ supera los 100 000, se devuelve el siguiente error: &quot;1003, Tamaño de pertenencia coincidente: 100 001 supera el límite permitido (100 000) para esta API&quot;.

Para consultar un programa cuyo recuento de miembros exceda el límite, use la [API de extracción masiva de miembros del programa](bulk-program-member-extract.md) en su lugar.

`filterValues` especifica los valores que se van a buscar y acepta hasta 300 valores separados por comas. La llamada busca los registros en los que el campo de miembro del programa coincide con uno de los filterValues incluidos.

Como alternativa, filtre por intervalo de fechas especificando `updatedAt` como filterType y proporcionando los parámetros datetime `startAt` y `endAt`. El intervalo debe ser de siete días o menos. Utilice el formato ISO-8601 sin milisegundos para los valores de fecha y hora.

El parámetro de consulta opcional `fields` acepta una lista separada por comas de nombres de API de campo devueltos por el extremo [Describir miembro del programa](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/describeProgramMemberUsingGET2). Cuando se incluye, cada registro de respuesta contiene los campos especificados. Si se omite, la respuesta devolverá `acquiredBy`, `leadId`, `membershipDate`, `programId` y `reachedSuccess` de forma predeterminada.

De forma predeterminada, el extremo devuelve un máximo de 300 registros. Utilice el parámetro de consulta `batchSize` para reducir este número.

Si el atributo **moreResult** es true, hay más resultados disponibles. Continúe llamando al extremo con el `nextPageToken` devuelto hasta que moreResult sea false.

Si la longitud total de la solicitud GET supera los 8 KB, el extremo devuelve el error HTTP &quot;414, URI too long&quot;. Para solucionar este límite, cambie la solicitud de GET a POST, agregue el parámetro `_method=GET` y coloque la cadena de consulta en el cuerpo de la solicitud.

```http
GET /rest/v1/programs/{programId}/members.json?filterType=statusName&filterValues=Influenced
```

```json
{
    "requestId": "109da#17915eec072",
    "result": [
        {
            "seq": 0,
            "leadId": 1789,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 1,
            "leadId": 1790,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 2,
            "leadId": 1791,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 3,
            "leadId": 1792,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 4,
            "leadId": 1793,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 5,
            "leadId": 1794,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 6,
            "leadId": 1795,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 7,
            "leadId": 1796,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 8,
            "leadId": 1797,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 9,
            "leadId": 1798,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 10,
            "leadId": 1799,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 11,
            "leadId": 1800,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        }
    ],
    "success": true,
    "moreResult": false
}
```

## Crear y actualizar

Dos extremos admiten las operaciones de creación y actualización en los miembros del programa:

- Un punto final solo actualiza el estado de miembro del programa.
- Un extremo actualiza los campos de miembros del programa marcados como &quot;actualizables&quot;.

Cada extremo puede modificar hasta 300 registros de miembros de programa por llamada.

### Estado de miembro del programa

Use el punto de conexión [Estado de miembro del programa de sincronización](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/syncProgramMemberStatusUsingPOST) para crear o actualizar el estado del programa para uno o varios miembros.

Los parámetros necesarios son:

- `programId`: un parámetro de ruta de acceso que especifica el programa que contiene los miembros que se van a crear o actualizar.
- `statusName`: especifica el estado del programa que se aplicará a una lista de posibles clientes. El statusName debe coincidir con un estado disponible para el canal del programa. Recupere estados válidos con el extremo [Get Channels](https://developer.adobe.com/marketo-apis/api/asset#tag/Channels/operation/getAllChannelsUsingGET). Si el estado de un posible cliente tiene un valor de paso mayor que el statusName designado, la solicitud omite ese posible cliente.
- `input`: matriz de `leadId` valores que corresponden a miembros del programa. Puede enviar hasta 300 leadIds por llamada.

El extremo realiza una actualización en cada registro. Si el leadId está asociado con un miembro del programa, el punto final actualiza su estado de pertenencia. De lo contrario, crea un registro de miembro del programa, asocia el registro con el leadId y asigna el estado de pertenencia.

La respuesta incluye un `status` de &quot;actualizado&quot;, &quot;creado&quot; u &quot;omitido&quot;. Un resultado omitido también incluye una matriz `reasons`. El campo `seq` es un índice que correlaciona cada registro enviado con el orden de respuesta.

Si la llamada se realiza correctamente, se escribe la actividad &quot;Cambiar estado del programa&quot; en el registro de actividades del posible cliente.

```http
POST /rest/v1/programs/{programId}/members/status.json
```

```text
Content-Type: application/json
```

```json
{
    "statusName":"Influenced",
    "input":[
        {
            "leadId": 1800
        },
        {
            "leadId": 1801
        },
        {
            "leadId": 1235
        }
    ]
}
```

```json
{
    "requestId": "14b2d#17916378ec5",
    "result": [
        {
            "seq": 0,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1037",
                    "message": "Lead skipped because it is already in or past this status"
                }
            ]
        },
        {
            "seq": 1,
            "status": "updated",
            "leadId": 1801
        },
        {
            "seq": 2,
            "status": "created",
            "leadId": 1235
        }
    ],
    "success": true
}
```

### Datos de miembros del programa

Use el punto de conexión [Sincronizar datos de miembros del programa](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/syncProgramMemberDataUsingPOST) para actualizar los datos de campo de miembros del programa para uno o varios miembros. Puede modificar cualquier campo personalizado o estándar marcado como &quot;actualizable&quot; por el extremo [Describir miembro del programa](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/describeProgramMemberUsingGET2).

Los parámetros necesarios son:

- `programId`: un parámetro de ruta de acceso que especifica el programa que contiene los miembros que se van a actualizar.
- `input`: una matriz cuyos elementos contienen `leadId` y uno o más campos para actualizar por nombre de API. Puede enviar hasta 300 leadIds por llamada.

El extremo actualiza cada registro. El leadId debe estar asociado con un miembro del programa y cada campo debe ser actualizable.

La respuesta incluye un `status` de &quot;actualizado&quot; u &quot;omitido&quot;. Un resultado omitido también incluye una matriz `reasons`. El campo `seq` es un índice que correlaciona cada registro enviado con el orden de respuesta.

Si la llamada se realiza correctamente, se escribe la actividad &quot;Cambiar datos de miembros del programa&quot; en el registro de actividades del posible cliente.

```http
POST /rest/v1/programs/{programId}/members.json
```

```text
Content-Type: application/json
```

```json
{
    "input":[
        {
            "leadId": 1789,
            "registrationCode": "dcff5f12-a7c7-11eb-bcbc-0242ac130002"
        },
        {
            "leadId": 1790,
            "registrationCode": "c0404b78-d3fd-47bf-82c4-d16f3852ab3a"
        },
        {
            "leadId": 1003,
            "registrationCode": "aa880c57-75b8-426b-a33a-fbf6302d7cb4"
        }
    ]
}
```

```json
{
    "requestId": "edc3#1791659b8d2",
    "result": [
        {
            "seq": 0,
            "status": "updated",
            "leadId": 1789
        },
        {
            "seq": 1,
            "status": "updated",
            "leadId": 1790
        },
        {
            "seq": 2,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1013",
                    "message": "Membership not found"
                }
            ]
        }
    ],
    "success": true
}
```

## Campos

El objeto de miembro de programa contiene campos estándar y campos personalizados opcionales. Los campos estándar están presentes en cada suscripción de Marketo Engage, mientras que los usuarios crean campos personalizados según sea necesario.

Cada campo está definido por atributos como nombre para mostrar, nombre de API y dataType. Juntos, estos atributos se denominan metadatos.

Los siguientes extremos consultan, crean y actualizan campos en el objeto de miembro de programa. El usuario de API debe tener una función con el permiso **Campo estándar de esquema de lectura-escritura**, el permiso **Campo personalizado de esquema de lectura-escritura** o ambos.

### Campos de consulta

Consulte un campo de miembro de programa por nombre de API o recupere todos los campos de miembro de programa. Los permisos de funciones determinan si la respuesta puede incluir campos estándar, campos personalizados o ambos. La respuesta también incluye campos ocultos.

#### Por nombre

El extremo [Obtener campo de miembro del programa por nombre](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/getProgramMemberFieldByNameUsingGET) recupera los metadatos de un campo en el objeto de miembro del programa. El parámetro de ruta de acceso `fieldApiName` requerido especifica el nombre de API del campo.

La respuesta se parece a la respuesta Describir miembro del programa, pero incluye metadatos adicionales. Por ejemplo, el atributo `isCustom` indica si el campo es personalizado.

```http
GET /rest/v1/programs/members/schema/fields/{fieldApiName}.json
```

```json
{
    "requestId": "15416#17e955554de",
    "result": [
        {
            "displayName": "Status",
            "name": "statusName",
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

El extremo [Obtener campos de miembros del programa](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/getProgramMemberFieldsUsingGET) recupera los metadatos de todos los campos del objeto de miembro del programa. De forma predeterminada, devuelve un máximo de 300 registros. Utilice el parámetro de consulta `batchSize` para reducir este número.

Si el atributo `moreResult` es verdadero, hay más resultados disponibles. Continúe llamando al extremo con el `nextPageToken` devuelto hasta que moreResult sea false.

```http
GET /rest/v1/programs/members/schema/fields.json?batchSize=5
```

```json
{
    "requestId": "102f6#17e9557f123",
    "result": [
        {
            "displayName": "Acquired By",
            "name": "acquiredBy",
            "description": null,
            "dataType": "boolean",
            "isHidden": false,
            "isHtmlEncodingInEmail": false,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Nurture Cadence",
            "name": "nurtureCadence",
            "description": null,
            "dataType": "string",
            "length": 4,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Nurture Exhausted",
            "name": "isExhausted",
            "description": null,
            "dataType": "boolean",
            "isHidden": false,
            "isHtmlEncodingInEmail": false,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Member Date",
            "name": "membershipDate",
            "description": null,
            "dataType": "datetime",
            "isHidden": false,
            "isHtmlEncodingInEmail": false,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Program",
            "name": "program",
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
    "nextPageToken": "BC7J6EPVLT6T4B5FKUU3APCYN4======",
    "moreResult": true
}
```

### Crear campos

El extremo [Crear campos de miembros de programa](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/createProgramMemberFieldUsingPOST) crea campos personalizados en el objeto de miembro de programa. Proporciona una funcionalidad comparable a la de [Marketo Engage UI](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/program-member-custom-fields). Puede crear hasta 20 campos personalizados con este punto de conexión.

Tenga en cuenta cada campo antes de crearlo en una instancia de Marketo Engage de producción. Después de crear un campo, no se puede eliminar; [sólo se puede ocultar](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/field-management/delete-a-custom-field-in-marketo). Los campos no utilizados añaden desorden a la instancia.

El parámetro `input` requerido es una matriz de objetos de campo de miembros de programa. Cada objeto contiene uno o más atributos.

- Los atributos requeridos son `displayName`, `name` y `dataType`. Se corresponden con el nombre para mostrar de la IU, el nombre de la API y el tipo de campo, respectivamente.
- Los atributos opcionales son `description`, `isHidden`, `isHtmlEncodingInEmail` y `isSensitive`.

Los atributos `name` y `displayName` tienen estas reglas de nomenclatura:

- El atributo `name` debe ser único, comenzar con una letra y contener solo letras, números o guiones bajos.
- *`isplayName` debe ser único y no puede contener caracteres especiales.

Una convención común es aplicar [camel case](https://en.wikipedia.org/wiki/Camel_case#) a `displayName` para producir `name`. Por ejemplo, un(a) `displayName` de &quot;Mi campo personalizado&quot; genera un(a) `name` de &quot;myCustomField&quot;.

```http
POST /rest/v1/programs/members/schema/fields.json
```

```json
{
  "input": [
    {
        "displayName": "PMCF Custom Field 03",
        "name": "pMCFCustomField03",
        "description": "My third custom field",
        "dataType": "string"
    }
  ]
}
```

```json
{
    "requestId": "13a7#17e955fcb44",
    "result": [
        {
            "name": "pMCFCustomField03",
            "status": "created"
        }
    ],
    "success": true
}
```

### Actualizar campo

El extremo [Actualizar campo de miembro de programa](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/updateProgramMemberFieldUsingPOST) actualiza un campo personalizado en el objeto de miembro de programa. La mayoría de las actualizaciones de campo disponibles en la interfaz de usuario de Marketo Engage también están disponibles a través de la API. La siguiente tabla resume las diferencias.

| Atributo | ¿Actualizable por API? | ¿Actualizable por IU? | ¿Actualizable por API? | ¿Actualizable por IU? |
| --- | --- | --- | --- | --- |
| dataType | no | no | no | sí |
| Descripción | sí | sí | sí | sí |
| displayName | no | no | sí | sí |
| isCustom | no | no | no | no |
| isHidden | no | sí | sí (si lo crea la API) | sí |
| isHtmlEncodingInEmail | sí | sí | sí | sí |
| isSensitive | sí | sí | sí | sí |
| length | no | no | no | no |
| name | no | no | no | no |

La solicitud requiere estos parámetros:

- `fieldApiName`: un parámetro de ruta de acceso que especifica el nombre de API del campo que se va a actualizar.
- `input`: matriz que contiene un objeto de campo de posible cliente con uno o más atributos.

```http
POST /rest/v1/programs/members/schema/fields/pMCFCustomField03.json
```

```json
{
  "input": [
      {
        "displayName": "Lunch Preference",
        "description": "Attendee food preference",
        "isHtmlEncodingInEmail": true
      }
  ]
}
```

```json
{
    "requestId": "215f#17e95663955",
    "result": [
        {
            "name": "pMCFCustomField03",
            "status": "updated"
        }
    ],
    "success": true
}
```

## Eliminar

Use el extremo [Eliminar miembros del programa](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/deleteProgramMemberUsingPOST) para eliminar registros de miembros del programa. El parámetro de ruta de acceso `programId` requerido especifica el programa que contiene los miembros que se van a eliminar.

El cuerpo de la solicitud contiene una matriz `input` de ID de posibles clientes. Cada llamada permite un máximo de 300 ID de posibles clientes.

La respuesta incluye un `status` de &quot;eliminado&quot; u &quot;omitido&quot;. Un resultado omitido también incluye una matriz `reasons`. El campo `seq` es un índice que correlaciona cada registro enviado con el orden de respuesta.

```http
POST /rest/v1/programs/{programId}/members/delete.json
```

```text
Content-Type: application/json
```

```json
{
    "input":[
        {
            "leadId": 1235
        },
        {
            "leadId": 77
        }
    ]
}
```

```json
{
    "requestId": "302a#17916619417",
    "result": [
        {
            "seq": 0,
            "status": "deleted",
            "leadId": 1235
        },
        {
            "seq": 1,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1037",
                    "message": "Lead not in program"
                }
            ]
        }
    ],
    "success": true
}
```
