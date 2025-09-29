---
title: Miembros del programa
feature: REST API
description: Utilice la API de REST de Marketo para leer, crear, actualizar y eliminar miembros del programa, administrar campos estándar y personalizados, y consultar mediante campos en los que se puede buscar.
exl-id: 22f29a42-2a30-4dce-a571-d7776374cf43
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '1726'
ht-degree: 2%

---

# Miembros del programa

[Referencia de extremo de miembros del programa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members)

Marketo expone las API para leer, crear, actualizar y eliminar registros de miembros del programa. Los registros de miembros del programa están relacionados con los registros de posibles clientes a través del campo de ID de posible cliente. Los registros están compuestos por un conjunto de campos estándar y, opcionalmente, hasta 20 campos personalizados adicionales. Los campos contienen datos específicos del programa para cada miembro y se pueden utilizar en formularios, filtros, déclencheur y acciones de flujo. Estos datos se pueden ver en la [ficha de miembros](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/manage-and-view-members) del programa en la interfaz de usuario de Marketo Engage.

## Describir

El extremo [Describir miembro del programa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2) sigue el patrón estándar para los objetos de la base de datos de posibles clientes. La matriz `searchableFields` le proporciona el conjunto de campos válidos para la consulta. La matriz `fields` contiene metadatos de campo, incluido el nombre de la API de REST, el nombre para mostrar y la capacidad de actualización de campos.

```
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

El extremo [Obtener miembros del programa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/getProgramMembersUsingGET) le permite recuperar miembros de un programa. Requiere un parámetro de ruta de acceso `programId` y `filterType` y `filterValues` parámetros de consulta.

`programId` se usa para especificar qué programa buscar.

`filterType` se usa para especificar qué campo usar como filtro de búsqueda. Acepta cualquier campo de la lista &quot;searchableFields&quot; devuelta por el extremo [Describir miembro del programa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2). Si especifica un filterType que es un campo personalizado, el dataType del campo personalizado debe ser &quot;string&quot; o &quot;integer&quot;. Si especifica un filterType distinto de &quot;leadId&quot;, la solicitud puede procesar un máximo de 100 000 registros de miembros del programa. Según la configuración de la instancia de Marketo, recibirá uno de los siguientes errores:

- Si el número total de miembros del programa supera los 100 000, se devuelve un error: &quot;1003, Total de miembros: 100 001 supera el límite permitido de 100 000 para el filtro&quot;.
- Si el número total de miembros del programa _que coinciden con el filtro_ supera los 100 000, se devuelve el siguiente error: &quot;1003, Tamaño de pertenencia coincidente: 100 001 supera el límite permitido (100 000) para esta API&quot;.

Para consultar un programa cuyo recuento de miembros exceda el límite, use la [API de extracción masiva de miembros del programa](bulk-program-member-extract.md) en su lugar.

`filterValues` se usa para especificar qué valores buscar y acepta hasta 300 valores en un formato separado por comas. La llamada busca los registros en los que el campo del miembro del programa coincide con uno de los filterValues incluidos.

Como alternativa, puede filtrar por intervalo de fechas especificando `updatedAt` como filterType con `startAt` y `endAt` parámetros datetime. El intervalo debe ser de siete días o menos. Las horas de la fecha deben estar en formato ISO-8601, sin milisegundos.

El parámetro de consulta opcional `fields` acepta una lista separada por comas de nombres de API de campo que devolvió el extremo [Describir miembro del programa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2). Cuando se incluye, cada registro de la respuesta incluye los campos especificados. Si se omite, se devuelve el conjunto predeterminado de campos: `acquiredBy`, `leadId`, `membershipDate`, `programId` y `reachedSuccess`.

De forma predeterminada, se devuelve un máximo de 300 registros. Puede usar el parámetro de consulta `batchSize` para reducir este número. Si el atributo **moreResult** es true, hay más resultados disponibles. Continúe llamando a este extremo hasta que el atributo moreResult devuelva false, lo que significa que no hay resultados disponibles. Los `nextPageToken` devueltos por esta API siempre se deben reutilizar para la siguiente iteración de esta llamada.

Si la longitud total de la solicitud de GET supera los 8 KB, se devuelve el siguiente error HTTP: &quot;414, URI too long&quot;. Como solución alternativa, puede cambiar su GET a POST, agregar el parámetro `_method=GET` y colocar la cadena de consulta en el cuerpo de la solicitud.

```
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

Existen dos extremos que admiten la operación de creación o actualización en los miembros del programa. Uno le permite actualizar solamente el estado de miembro del programa. El otro le permite actualizar el conjunto de campos de miembros del programa marcados como &quot;actualizables&quot;. Ambos extremos permiten modificar hasta 300 registros de miembros de programa por llamada.

### Estado de miembro del programa

El punto de conexión [Estado de miembro del programa de sincronización](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/syncProgramMemberStatusUsingPOST) se usa para crear o actualizar el estado del programa para uno o varios miembros.

El parámetro de ruta de acceso `programId` requerido especifica el programa que contiene los miembros que se van a crear o actualizar.

El parámetro `statusName` requerido especifica el estado del programa que se aplicará a una lista de posibles clientes. El statusName debe coincidir con un estado disponible para el canal del programa. Se pueden recuperar los estados válidos mediante el extremo [Get Channels](https://developer.adobe.com/marketo-apis/api/asset/#tag/Channels/operation/getAllChannelsUsingGET). Si el estado de un posible cliente tiene un valor de paso mayor que el statusName designado, ese posible cliente se omitirá.

El parámetro `input` requerido es una matriz de `leadId` que corresponde a miembros del programa. Puede enviar hasta 300 leadIds por llamada. Se realiza una operación de actualización en cada registro. Si el leadId está asociado con un miembro del programa, se actualiza su estado de pertenencia. Si no es así, se crea un nuevo registro de miembro del programa, el registro se asocia al leadId y se asigna el estado de pertenencia.

El extremo responde con un `status` de &quot;actualizado&quot;, &quot;creado&quot; u &quot;omitido&quot;. Si se omite, también se incluirá una matriz `reasons`. El extremo también responderá con un campo `seq`, que es un índice que se puede utilizar para correlacionar los registros enviados con el orden de la respuesta.

Si la llamada se realiza correctamente, se escribe la actividad &quot;Cambiar estado del programa&quot; en el registro de actividades del posible cliente.

```
POST /rest/v1/programs/{programId}/members/status.json
```

```
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

El punto de conexión [Sincronizar datos de miembros del programa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/syncProgramMemberDataUsingPOST) se usa para actualizar los datos de campo de miembros del programa para uno o varios miembros. Puede modificar cualquier campo personalizado o campos estándar que sean &quot;actualizables&quot; (consulte [Describir miembro del programa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2) punto de conexión).

El parámetro de ruta de acceso `programId` requerido especifica el programa que contiene los miembros que se van a actualizar.

El parámetro `input` requerido es una matriz. Cada elemento de matriz contiene un `leadId` y uno o más campos que se van a actualizar (mediante el nombre de la API). Se realiza una operación de actualización en cada registro. El leadId debe estar asociado con un miembro del programa. Los campos deben ser actualizables. Puede enviar hasta 300 leadIds por llamada.

El extremo responde con un `status` de &quot;actualizado&quot; u &quot;omitido&quot;. Si se omite, también se incluirá una matriz `reasons`. El extremo también responderá con un campo `seq`, que es un índice que se puede utilizar para correlacionar los registros enviados con el orden de la respuesta.

Si la llamada se realiza correctamente, se escribe la actividad &quot;Cambiar datos de miembros del programa&quot; en el registro de actividades del posible cliente.

```
POST /rest/v1/programs/{programId}/members.json
```

```
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

El objeto de miembro de programa contiene campos estándar y campos personalizados opcionales. Los campos estándar están presentes en cada suscripción de Marketo Engage, mientras que el usuario crea los campos personalizados según sea necesario. Cada definición de campo está compuesta por un conjunto de atributos que describen el campo. Algunos ejemplos de atributos son nombre para mostrar, nombre de API y dataType. Estos atributos se conocen colectivamente como metadatos.

Los siguientes extremos permiten consultar, crear y actualizar campos en el objeto de miembro de programa. Estas API requieren que el usuario de la API propietaria tenga una función con uno o ambos de los permisos **Campo estándar de esquema de lectura-escritura** o **Campo personalizado de esquema de lectura-escritura**.

### Campos de consulta

La consulta de los campos de miembros del programa es sencilla. Puede consultar un único campo de miembro del programa por nombre de API o consultar el conjunto de todos los campos de miembro del programa. Se pueden recuperar tanto los campos estándar como los campos personalizados, en función de los permisos de función que se utilicen. También se recuperan los campos ocultos.

#### Por nombre

El extremo [Obtener campo de miembro del programa por nombre](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/getProgramMemberFieldByNameUsingGET) recupera los metadatos de un único campo en el objeto de miembro del programa. El parámetro de ruta de acceso obligatorio `fieldApiName` especifica el nombre de API del campo. La respuesta es como el extremo Describir miembro del programa, pero contiene metadatos adicionales como el atributo `isCustom` que indica si el campo es un campo personalizado.

```
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

El extremo [Obtener campos de miembros del programa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/getProgramMemberFieldsUsingGET) recupera los metadatos de todos los campos del objeto de miembro del programa. De forma predeterminada, se devuelve un máximo de 300 registros. Puede usar el parámetro de consulta `batchSize` para reducir este número. Si el atributo `moreResult` es true, significa que hay más resultados disponibles. Continúe llamando a este extremo hasta que el atributo moreResult devuelva false, lo que significa que no hay resultados disponibles. Los `nextPageToken` devueltos por esta API siempre se deben reutilizar para la siguiente iteración de esta llamada.

```
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

El extremo [Crear campos de miembros de programa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/createProgramMemberFieldUsingPOST) crea uno o varios campos personalizados en el objeto de miembro de programa. Este extremo proporciona una funcionalidad comparable a la que está [disponible en la interfaz de usuario de Marketo Engage](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/program-member-custom-fields). Puede crear un máximo de 20 campos personalizados con este extremo.

Tenga en cuenta cuidadosamente cada campo que cree en la instancia de producción de Marketo Engage mediante la API. Una vez creado un campo, no se puede eliminar ([sólo se puede ocultar](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/administration/field-management/delete-a-custom-field-in-marketo)). La proliferación de campos no utilizados es una mala práctica que añadirá desorden a su instancia.

El parámetro `input` requerido es una matriz de objetos de campo de miembros de programa. Cada objeto contiene uno o más atributos. Los atributos requeridos son `displayName`, `name` y `dataType`, que corresponden al nombre para mostrar del campo en la interfaz de usuario, el nombre de API del campo y el tipo de campo respectivamente. Opcionalmente, puede especificar `description`, `isHidden`, `isHtmlEncodingInEmail` y `isSensitive`.

Hay algunas reglas asociadas con el nombre de `name` y `displayName`. El atributo `name` debe ser único, comenzar con una letra y contener solo letras, números o guiones bajos. *`isplayName` debe ser único y no puede contener caracteres especiales. Una convención de nombres común es aplicar [minúscula](https://en.wikipedia.org/wiki/Camel_case#) a `displayName` para generar `name`. Por ejemplo, un `displayName` de &quot;Mi campo personalizado&quot; produciría un `name` de &quot;myCustomField&quot;.

```
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

El extremo [Actualizar campo de miembro de programa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/updateProgramMemberFieldUsingPOST) actualiza un único campo personalizado en el objeto de miembro de programa. Por lo general, las operaciones de actualización de campos realizadas con la IU de Marketo Engage se pueden realizar con la API. En la tabla siguiente se resumen algunas diferencias.

| Atributo | ¿Actualizable por API? | ¿Actualizable por IU? | ¿Actualizable por API? | ¿Actualizable por IU? |
|---|---|---|---|---|
| dataType | no | no | no | sí |
| Descripción | sí | sí | sí | sí |
| displayName | no | no | sí | sí |
| isCustom | no | no | no | no |
| isHidden | no | sí | sí (si lo crea la API) | sí |
| isHtmlEncodingInEmail | sí | sí | sí | sí |
| isSensitive | sí | sí | sí | sí |
| length | no | no | no | no |
| name | no | no | no | no |

El parámetro de ruta de acceso `fieldApiName` requerido especifica el nombre de API del campo que se va a actualizar. El parámetro `input` requerido es una matriz que contiene un solo objeto de campo de posible cliente. El objeto de campo contiene uno o varios atributos.

```
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

El punto de conexión [Eliminar miembros del programa](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/deleteProgramMemberUsingPOST) se usa para eliminar registros de miembros del programa. El parámetro de ruta de acceso `programId` requerido especifica el programa que contiene los miembros que se van a eliminar. El cuerpo de la solicitud contiene una matriz `input` de identificadores de posibles clientes. Un máximo de 300 ID de posibles clientes  se permiten por llamada.

El extremo responde con un `status` de &quot;eliminado&quot; u &quot;omitido&quot;. Si se omite, también se incluirá una matriz `reasons`. El extremo también responderá con un campo `seq`, que es un índice que se puede utilizar para correlacionar los registros enviados con el orden de la respuesta.

```
POST /rest/v1/programs/{programId}/members/delete.json
```

```
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
