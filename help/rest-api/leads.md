---
title: Clientes potenciales
feature: REST API
description: Explore las funciones de la API de REST de Marketo Leads, incluida la descripción, la consulta por ID o filtro, los campos predeterminados, los límites y la recuperación de ECID.
exl-id: 0a2f7c38-02ae-4d97-acfe-9dd108a1f733
TQID: https://experienceleague.adobe.com/jZ-ecWTmHwq9gvp4fMaeuuGba6cgwYx0QCCyfkrEDHQ
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: a7170d27-32ab-462b-a333-269abc654483
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 2728
ht-degree: 3%

---

# Clientes potenciales

[Referencia de extremo de posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads)

La API de Marketo Leads admite operaciones de CRUD en registros de posibles clientes. También puede modificar la pertenencia de un posible cliente a listas y programas estáticos e iniciar el procesamiento de campañas inteligentes para posibles clientes.

## Describir

Utilice Describir posibles clientes para recuperar los campos disponibles a través de la API de REST y los metadatos de cada campo:

- Tipo de datos
- Nombre de API de REST
- Longitud, si procede
- Estado de solo lectura
- Etiqueta descriptiva

Describir es la principal fuente fiable para la disponibilidad de campos y metadatos.

### Solicitud

```http
GET /rest/v1/leads/describe.json
```

### Respuesta

```json
{
   "requestId":"37ca#1475b74e276",
   "success":true,
   "result":[
      {
         "id":2,
         "displayName":"Company Name",
         "dataType":"string",
         "length":255,
         "rest":{
            "name":"company",
            "readOnly":false
         },
         "soap":{
            "name":"Company",
            "readOnly":false
         }
      }
}
```

Las respuestas reales incluyen más campos en la matriz de resultados. Cada elemento representa un campo disponible en el registro de cliente potencial y contiene al menos un id, un displayName y un tipo de datos.

Los objetos secundarios rest y soap solo aparecen cuando el campo es válido para la API correspondiente. La propiedad `readOnly` indica si la API correspondiente puede actualizar el campo. Cuando está presente, la propiedad length proporciona la longitud máxima del campo y la propiedad dataType proporciona el tipo de datos del campo.

## Consulta

Utilice uno de los dos métodos principales para recuperar posibles clientes:

- Obtener posible cliente por ID toma un ID de posible cliente como parámetro de ruta y devuelve un registro de posible cliente.
- Obtener posibles clientes por tipo de filtro busca los registros cuyo campo seleccionado coincida con uno de los valores proporcionados.

Para Obtener posible cliente por ID, pase de forma opcional un parámetro de campos con una lista de nombres de campo separados por comas que se va a devolver. Si la solicitud omite campos, la respuesta incluye `email`, `updatedAt`, `createdAt`, `lastName`, `firstName` y `id`. Si no se devuelve un campo solicitado, su valor implica que es nulo.

### Solicitud

```http
GET /rest/v1/lead/{id}.json
```

### Respuesta

```json
{
   "requestId": "10226#14d3049e51b",
   "success": true,
   "result": [
      {
         "id": 318581,
         "updatedAt":"2015-05-07T11:47:30-08:00"
         "lastName": "Doe",
         "email": "jdoe@marketo.com",
         "createdAt": "2015-05-01T16:47:30-08:00",
         "firstName": "John"
      }
   ]
}
```

Obtener posible cliente por id. siempre devuelve un registro en la primera posición de la matriz de resultados.

Obtener posibles clientes por tipo de filtro devuelve el mismo tipo de registro y puede devolver hasta 300 registros por página. Se requieren los parámetros de consulta `filterType` y `filterValues`.

`filterType` acepta cualquier campo personalizado y los campos más utilizados. Llame al extremo `Describe2` para recuperar los campos en los que se puede buscar permitidos para `filterType`. Al buscar por campo personalizado, los tipos de datos admitidos son `string`, `email` y `integer`. Utilice el método Describe para recuperar detalles de campo como la descripción y el tipo.

`filterValues` acepta hasta 300 valores separados por comas. La llamada devuelve los registros en los que el campo de posible cliente seleccionado coincide con uno de esos valores. Si más de 1000 posibles clientes coinciden con el filtro, la API devuelve &quot;1003, demasiados resultados coinciden con el filtro&quot;.

Si la solicitud GET total supera los 8 KB, la API devuelve &quot;414, URI demasiado largo&quot; en RFC 7231. Para solucionar este límite, cambie GET a POST, agregue el parámetro _method=GET y coloque la cadena de consulta en el cuerpo de la solicitud.

### Solicitud

```http
GET /rest/v1/leads.json?filterType=id&filterValues=318581,318592
```

### Respuesta

```json
{
    "requestId": "12951#15699db5c97",
    "result": [
        {
            "id": 318581,
            "updatedAt": "2016-05-17T22:11:45Z",
            "lastName": "Lincoln",
            "email": "abe@usa.gov",
            "createdAt": "2015-03-17T00:18:40Z",
            "firstName": "Abraham"
        },
        {
            "id": 318592,
            "updatedAt": "2016-05-17T22:20:51Z",
            "lastName": "Washington",
            "email": "george@usa.gov",
            "createdAt": "2015-04-06T16:29:21Z",
            "firstName": "George"
        }
    ],
    "success": true
}
```

Esta llamada devuelve registros cuyos identificadores coinciden con los valores de `filterValues`.

Si no coincide ningún registro, la respuesta indica éxito y contiene una matriz de resultados vacía.

### Respuesta

```json
{
"requestId": "177a1#1578b643357",
"result": [],
"success": true
}
```

Tanto Obtener posible cliente por ID como Obtener posibles clientes por tipo de filtro aceptan un parámetro de consulta de campos que contiene una lista de campos API separados por comas. Cuando los campos están presentes, cada registro de respuesta incluye los campos enumerados. Si se omite, la respuesta incluye `id`, `email`, `updatedAt`, `createdAt`, `firstName` y `lastName`.

## ADOBE ECID

Cuando el uso compartido de audiencias de Adobe Experience Cloud está habilitado, la sincronización de cookies asocia los valores de Adobe Experience Cloud ID (ECID) con posibles clientes de Marketo. Para recuperar los valores ECID asociados con los métodos de recuperación de posibles clientes anteriores, incluya `ecids` en el parámetro fields. Por ejemplo, `&fields=email,firstName,lastName,ecids`.

## Crear y actualizar

La API de posibles clientes puede crear, actualizar y eliminar registros de posibles clientes. Las operaciones de creación y actualización utilizan el mismo punto de conexión, con el tipo de operación definido en la solicitud. Una solicitud puede crear o actualizar hasta 300 registros.

>[!NOTE]
>
> No se admite la actualización de los campos de la compañía mediante el extremo [Sync Leads](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/syncLeadUsingPOST). En su lugar, use el punto de conexión [Compañías de sincronización](https://developer.adobe.com/marketo-apis/api/mapi#tag/Companies/operation/syncCompaniesUsingPOST).

>[!NOTE]
>
> Al crear o actualizar el valor de correo electrónico en un registro de persona, solo se admiten caracteres ASCII en el campo de dirección de correo electrónico.

### Solicitud

```http
POST /rest/v1/leads.json
```

### Cuerpo

```json
{
   "action":"createOnly",
   "lookupField":"email",
   "input":[
      {
         "email":"kjashaedd-1@klooblept.com",
         "firstName":"Kataldar-1",
         "postalCode":"04828"
      },
      {
         "email":"kjashaedd-2@klooblept.com",
         "firstName":"Kataldar-2",
         "postalCode":"04828"
      },
      {
         "email":"kjashaedd-3@klooblept.com",
         "firstName":"Kataldar-3",
         "postalCode":"04828"
      }
   ]
}
```

### Respuesta

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "id":50,
         "status":"created"
      },
      {
         "id":51,
         "status":"created"
      },
      {
         "id":52,
         "status":"created"
      }
   ]
}
```

La solicitud utiliza dos campos importantes:

- `action` especifica el tipo de operación: `createOrUpdate`, `createOnly`, `updateOnly` o `createDuplicate`. Si se omite, el valor predeterminado es `createOrUpdate`.
- `lookupField` especifica la clave cuando la acción es `createOrUpdate` o `updateOnly`. Si se omite, el valor predeterminado es `email`.

De forma predeterminada, la operación utiliza la partición predeterminada. El parámetro `partitionName` opcional solo funciona cuando la acción es `createOnly` o `createOrUpdate`. Para usar `partitionName` como criterio de deduplicación adicional, inclúyalo en el tipo de origen para las reglas de desduplicación personalizadas.

Durante una actualización, la API devuelve un error si el posible cliente no existe en la partición especificada o si el usuario solo de API no puede acceder a esa partición.

Dado que `id` es una clave única administrada por el sistema, inclúyala únicamente con la acción `updateOnly`.

La solicitud debe incluir un parámetro `input` que contenga una matriz de registros de posibles clientes. Cada registro de posible cliente es un objeto JSON con cualquier número de campos de posible cliente. Las claves deben ser únicas dentro de cada registro, y todas las cadenas JSON deben utilizar la codificación UTF-8.

Use `externalCompanyId` para vincular un registro de posible cliente a un registro de empresa. Use `externalSalesPersonId` para vincular un registro de posible cliente a un registro de vendedor.

Las solicitudes de inserción simultáneas o con un tiempo muy breve pueden crear registros duplicados cuando varias solicitudes utilizan el mismo valor clave antes de que se devuelva la primera solicitud. Para evitar duplicados, use `createOnly` o `updateOnly` según corresponda. Como alternativa, ponga en cola las llamadas y espere a que se devuelva cada llamada antes de enviar otra actualización con la misma clave.

## Campos

El objeto de posible cliente contiene campos estándar y campos personalizados opcionales. Los campos estándar existen en cada suscripción de Marketo Engage, mientras que los usuarios crean campos personalizados según sea necesario.

Cada definición de campo contiene atributos de metadatos como nombre para mostrar, nombre de API y dataType.

Utilice los siguientes extremos para consultar, crear y actualizar campos en el objeto de posible cliente. La función del usuario de API debe tener el permiso Campo estándar de esquema de lectura-escritura, el permiso Campo personalizado de esquema de lectura-escritura o ambos.

## Campos de consulta

Consultar un campo de posible cliente por nombre de API o consultar todos los campos de posible cliente. Según los permisos de la función, la respuesta puede incluir campos estándar, campos personalizados y campos ocultos.

## Por nombre

El punto de conexión Obtener posible cliente por nombre recupera los metadatos de un campo de posible cliente. El parámetro de ruta fieldApiName requerido especifica el nombre de API del campo.

La respuesta se parece a la respuesta Describir posible cliente, pero incluye metadatos adicionales. Por ejemplo, el atributo isCustom indica si el campo es personalizado.

### Solicitud

```http
GET /rest/v1/leads/schema/fields/{fieldApiName}.json
```

### Respuesta

```json
{
    "requestId": "cd97#1793ee0fec4",
    "result": [
        {
            "displayName": "Email Address",
            "name": "email",
            "description": null,
            "dataType": "email",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        }
    ],
    "success": true
}
```

## Examinar

El extremo Obtener campos de posible cliente recupera los metadatos de todos los campos del objeto de posible cliente. De forma predeterminada, devuelve un máximo de 300 registros. Utilice el parámetro de consulta `batchSize` para reducir este número.

Si `moreResult` es verdadero, hay más resultados disponibles. Pase el valor devuelto `nextPageToken` en cada llamada subsiguiente hasta que `moreResult` sea falso.

### Solicitud

```http
GET /rest/v1/leads/schema/fields.json
```

### Respuesta (truncada)

```json
{
    "requestId": "142c3#1793eb976d8",
    "result": [
        {
            "displayName": "Salutation",
            "name": "salutation",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "First Name",
            "name": "firstName",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Middle Name",
            "name": "middleName",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Last Name",
            "name": "lastName",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Date of Birth",
            "name": "dateOfBirth",
            "description": null,
            "dataType": "date",
            "isHidden": false,
            "isHtmlEncodingInEmail": false,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Email Address",
            "name": "email",
            "description": null,
            "dataType": "email",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Phone Number",
            "name": "phone",
            "description": null,
            "dataType": "phone",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Mobile Phone Number",
            "name": "mobilePhone",
            "description": null,
            "dataType": "phone",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Fax Number",
            "name": "fax",
            "description": null,
            "dataType": "phone",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Job Title",
            "name": "title",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Unsubscribed",
            "name": "unsubscribed",
            "description": null,
            "dataType": "boolean",
            "isHidden": false,
            "isHtmlEncodingInEmail": false,
            "isSensitive": true,
            "isCustom": false
        },
        ...
    ],
    "success": true,
    "moreResult": false
}
```

## Crear campos

El extremo Crear campos de posible cliente crea uno o varios campos personalizados en el objeto de posible cliente y proporciona una funcionalidad comparable a la de la interfaz de usuario de Marketo Engage. Puede crear hasta 100 campos personalizados con este punto de conexión.

Tenga en cuenta cada campo antes de crearlo en una instancia de producción. Una vez creado un campo, puede ocultarlo, pero no puede eliminarlo. Los campos no utilizados añaden desorden a la instancia.

El parámetro de entrada requerido es una matriz de objetos de campo de posibles clientes. Cada objeto requiere estos atributos:

- `displayName` es el nombre para mostrar de la IU del campo.
- `name` es el nombre de API del campo.
- `dataType` es el tipo de campo.

Los atributos opcionales son `description`, `isHidden`, `isHtmlEncodingInEmail` y `isSensitive`.

El atributo name debe ser único, comenzar con una letra y contener solo letras, números o guiones bajos. `displayName` debe ser único y no puede contener caracteres especiales.

Una convención común aplica mayúsculas y minúsculas a `displayName` para producir el nombre. Por ejemplo, un `displayName` de &quot;Mi campo personalizado&quot; genera el nombre &quot;myCustomField&quot;.

### Solicitud

```http
POST /rest/v1/leads/schema/fields.json
```

### Cuerpo

```json
{
  "input": [
      {
        "displayName": "Acme Access Code",
        "name": "acmeAccessCode",
        "description": "Acme Direct Mail Integration",
        "dataType": "string"
      },
      {
        "displayName": "Acme Mail Date",
        "name": "acmeMailDate",
        "description": "Acme Direct Mail Integration",
        "dataType": "string"
      }
  ]
}
```

### Respuesta

```json
{
    "requestId": "d9f1#17943666811",
    "result": [
        {
            "name": "acmeAccessCode",
            "status": "created"
        },
        {
            "name": "acmeMailDate",
            "status": "created"
        }
    ],
    "success": true
}
```

## Actualizar campo

El punto final Actualizar campo de posible cliente actualiza un campo personalizado en el objeto de posible cliente. La mayoría de las actualizaciones de campo disponibles en la interfaz de usuario de Marketo Engage también están disponibles a través de la API. La siguiente tabla resume las diferencias.

<table>
<tbody>
<tr>
<td style="width: 26.5306%;" rowspan="2"><strong>Atributo</strong></td>
<td style="width: 35%;" colspan="2"><strong>Campo estándar</strong></td>
<td style="width: 38.2654%;" colspan="2"><strong>Campo personalizado</strong></td>
</tr>
<tr>
<td style="width: 17.449%;"><strong>¿Actualizable por API?</strong></td>
<td style="width: 17.551%;"><strong>¿Actualizable por IU?</strong></td>
<td style="width: 19.3878%;"><strong>¿Actualizable por API?</strong></td>
<td style="width: 18.8776%;"><strong>¿Actualizable por IU?</strong></td>
</tr>
<tr>
<td style="width: 26.5306%;">dataType</td>
<td style="width: 17.449%;">no</td>
<td style="width: 17.551%;">no</td>
<td style="width: 19.3878%;">no</td>
<td style="width: 18.8776%;">sí</td>
</tr>
<tr>
<td style="width: 26.5306%;">Descripción</td>
<td style="width: 17.449%;">sí</td>
<td style="width: 17.551%;">sí</td>
<td style="width: 19.3878%;">sí</td>
<td style="width: 18.8776%;">sí</td>
</tr>
<tr>
<td style="width: 26.5306%;">displayName</td>
<td style="width: 17.449%;">no</td>
<td style="width: 17.551%;">no</td>
<td style="width: 19.3878%;">sí</td>
<td style="width: 18.8776%;">sí</td>
</tr>
<tr>
<td style="width: 26.5306%;">isCustom</td>
<td style="width: 17.449%;">no</td>
<td style="width: 17.551%;">no</td>
<td style="width: 19.3878%;">no</td>
<td style="width: 18.8776%;">no</td>
</tr>
<tr>
<td style="width: 26.5306%;">isHidden</td>
<td style="width: 17.449%;">no</td>
<td style="width: 17.551%;">sí</td>
<td style="width: 19.3878%;">sí (si lo crea la API)</td>
<td style="width: 18.8776%;">sí</td>
</tr>
<tr>
<td style="width: 26.5306%;">isHtmlEncodingInEmail</td>
<td style="width: 17.449%;">sí</td>
<td style="width: 17.551%;">sí</td>
<td style="width: 19.3878%;">sí</td>
<td style="width: 18.8776%;">sí</td>
</tr>
<tr>
<td style="width: 26.5306%;">isSensitive</td>
<td style="width: 17.449%;">sí</td>
<td style="width: 17.551%;">sí</td>
<td style="width: 19.3878%;">sí</td>
<td style="width: 18.8776%;">sí</td>
</tr>
<tr>
<td style="width: 26.5306%;">length</td>
<td style="width: 17.449%;">no</td>
<td style="width: 17.551%;">no</td>
<td style="width: 19.3878%;">no</td>
<td style="width: 18.8776%;">no</td>
</tr>
<tr>
<td style="width: 26.5306%;">name</td>
<td style="width: 17.449%;">no</td>
<td style="width: 17.551%;">no</td>
<td style="width: 19.3878%;">no</td>
<td style="width: 18.8776%;">no</td>
</tr>
</tbody>
</table>

El parámetro de ruta de acceso `fieldApiName` requerido especifica el nombre de API del campo que se va a actualizar. El parámetro de entrada requerido es una matriz que contiene un objeto de campo de posible cliente con uno o más atributos.

### Solicitud

```http
POST /rest/v1/leads/schema/fields/{fieldApiName}.json
```

### Cuerpo

```json
{
  "input": [
      {
        "displayName": "Acme Access Code",
        "description": "Acme Direct Mail Integration",
        "isHtmlEncodingInEmail": true
      }
  ]
}
```

### Respuesta

```json
{
    "requestId": "9f57#1794324f44c",
    "result": [
        {
            "name": "acmeAccessCode",
            "status": "updated"
        }
    ],
    "success": true
}
```

## Insertar el cliente potencial en Marketo

El posible cliente push es una alternativa a Sincronizar posibles clientes y proporciona más opciones de activación, similares a un formulario de Marketo. Además de sincronizar los campos de posibles clientes, el extremo puede asociar un posible cliente basado en un valor de cookie. Pase el valor `mkt_tok` generado al hacer clic desde un correo electrónico de Marketo, o pase un nombre de programa en la llamada.

El extremo también crea una actividad activable asociada a un programa, una campaña o ambos de Marketo. Utilice esta actividad para iniciar flujos de trabajo desde eventos de captura de posibles clientes atribuidos a una campaña o programa específico.

El posible cliente push utiliza las mismas claves principales y nombres de API de campo que los posibles clientes de sincronización. No tiene parámetro de acción porque siempre realiza una actualización.

Se requieren `programName` y parámetros de entrada. El parámetro de entrada es una matriz de objetos de posibles clientes y la actividad resultante se atribuye al programa denominado. Los parámetros `lookupField`, `source` y `reason` son opcionales. Agregue cadenas arbitrarias en `source` y `reason` para incluir esos valores en las actividades resultantes. Puede utilizar los valores como restricciones en los déclencheur correspondientes (el posible cliente se inserta en Marketo) y (el posible cliente se ha insertado en Marketo).

Para asociar actividades anónimas anteriores con un posible cliente recién creado, omita el atributo de cookies del objeto de posible cliente y llame a Asociar posible cliente después de Insertar posible cliente. Para crear un posible cliente sin historial de actividades, especifique el atributo cookies en el objeto de posible cliente.

### Solicitud

```http
POST /rest/v1/leads/push.json
```

### Cuerpo

```json
{
    "programName": "Big Blue Thing Product Launch",
    "source": "Cool Sales Site",
    "reason": "Downloaded pricing sheet",
    "lookupField": "email",
    "input": [
        {
             "email": "Theresa.May@westminister.gov.uk",
             "country": "united kingdom",
             "firstName": "Theresa",
             "website": "www.brexit.com",
             "leadScore": 45,
             "jobTitle": "Prime Minister"
         },
         {
             "email": "Justin.Trudeau@ottowa.gov.ca",
             "country": "canada",
             "firstName": "Justin",
             "website": "www.take-off-eh.com",
             "leadScore": 92,
             "jobTitle": "Sonny"
         }
     ]
}
```

### Respuesta

```json
{
    "requestId": "939079529805",
    "success": true,
    "warnings": [],
    "result": [
       {
           "id": 483894,
           "status": "created"
       },
       {
           "id": 1087425,
           "status": "updated"
       },
       {
           "id": 3525,
           "reasons": [
                    {
                        "code": "501",
                        "message": "Bad stuff happened"
                    }
           ]
       }
    ]
}
```

Para pasar el parámetro `mkt_tok`, asigne su valor al miembro mktToken en un registro de posibles clientes dentro del parámetro de entrada.

### Cuerpo

```json
{
  "programName": "Big Blue Thing Product Launch",
  "source": "Cool Sales Site",
  "reason": "Downloaded pricing sheet",
  "lookupField": "mktToken",
  "input" : [
     {
       "mktToken" : "<tokenValue>",
       "firstName" : "Thelma"
     },
     {
       "mktToken" : "<tokenValue>",
       "firstName" : "Louise"
     }
   ]
}
```

## Enviar formulario

Enviar formulario es una alternativa para sincronizar posibles clientes y proporciona una funcionalidad equivalente a un envío de formulario de Marketo. Utilícelo para iniciar flujos de trabajo desde eventos de captura de posibles clientes atribuidos a una campaña o programa específicos.

El extremo del formulario de envío admite la siguiente funcionalidad:

- Actualiza un registro de posibles clientes utilizando el campo de correo electrónico como clave principal.
- Crea una actividad &quot;Rellenar formulario&quot; asociada a un programa, una campaña o ambos.
- Asocia un posible cliente en función de un valor de cookie.
- Valida los campos del formulario.

Enviar un formulario con el patrón de base de datos de posibles clientes estándar. Pase un registro de objeto en el miembro de entrada requerido del cuerpo JSON de la solicitud POST. El miembro `formId` requerido contiene el id. de formulario de Marketo de destino.

Use el elemento opcional `programId` para identificar el programa que recibe el posible cliente, los campos personalizados de los miembros del programa o ambos. Si `programId` está presente, el posible cliente se agrega al programa junto con cualquier campo de miembro del programa en el formulario. El programa debe estar en el mismo espacio de trabajo que el formulario.

Si el formulario no contiene campos personalizados de miembro de programa y se omite `programId`, el posible cliente no se agrega a un programa. Si el formulario pertenece a un programa, contiene uno o varios campos personalizados de miembros del programa y omite `programId`, el extremo utilizará el programa del formulario.

El objeto `leadFormFields` requerido contiene uno o más pares de nombre/valor para que se rellenen los campos. Cada campo debe definirse en el formulario especificado y cada nombre debe ser el nombre de la API REST del campo. El campo `email` es obligatorio.

El objeto `visitorData` opcional contiene datos de visita a la página, incluidos `pageURL`, `queryString`, `leadClientIpAddress` y `userAgentString`. Utilícelo para rellenar campos de actividad adicionales para filtros y déclencheur.

El miembro de la cookie opcional asocia una cookie de Munchkin con un registro de persona de Marketo. Cuando el extremo crea un posible cliente, asocia actividades anónimas anteriores con ese posible cliente a menos que la cookie se haya asociado anteriormente con otro registro conocido.

Si la cookie se asoció anteriormente, se realiza un seguimiento de las nuevas actividades en relación con el nuevo registro, pero las actividades antiguas permanecen en el registro conocido existente. Para crear un posible cliente sin historial de actividades, omita el miembro cookie.

Los nuevos posibles clientes se crean en la partición principal del espacio de trabajo en el que reside el formulario.

### Solicitud

```http
POST /rest/v1/leads/submitForm.json
```

### Encabezado

```text
Content-Type: application/json
```

### Cuerpo

```json
{
  "formId": 1029,
  "input": [
    {
      "leadFormFields": {
        "firstName": "Marge",
        "lastName": "Simpson",
        "email": "marge.simpson@fox.com",
        "pMCFField": "PMCF value"
      },
      "visitorData": {
        "pageURL": "https://na-sjst.marketo.com/lp/063-GJP-217/UnsubscribePage.html",
        "queryString": "Unsubscribed=yes",
        "leadClientIpAddress": "192.150.22.5",
        "userAgentString": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/84.0.4147.89 Safari/537.36"
      },
      "cookie": "id:063-GJP-217&token:_mch-marketo.com-1594662481190-60776"
    }
  ]
}
```

### Respuesta

```json
{
  "requestId": "10667#173bc585ca5",
  "result": [
    {
      "id": 319174,
      "status": "updated"
    }
  ],
  "success": true
}
```

La siguiente imagen muestra los detalles de actividad &quot;Rellenar formulario&quot; correspondientes en la interfaz de usuario de Marketo Engage:

![Rellenar la interfaz de usuario del formulario](assets/fill_out_form_activity_details.png)

## Combinar

>[!NOTE]
>
>A partir del 31 de marzo de 2026, las llamadas que incluyan más de 25 ID en el parámetro `leadIds` de una llamada a la API de Merge Leads generarán un código de error 1080, y se omitirá la llamada. Los trabajos que requieren la fusión de más de 25 registros en uno deben dividirse en varios trabajos para garantizar el éxito de esas llamadas.
>

Utilice la API Merge Leads para combinar registros duplicados en un registro. Una combinación combina registros de actividad, suscripciones a programas, campañas y listas, información de CRM y valores de campo.

Pase el ID de posible cliente ganador como parámetro de ruta. Pase un `leadId` como parámetro de consulta o hasta 25 identificadores separados por comas en el parámetro `leadIds`.


### Solicitud

```http
POST /rest/v1/leads/{id}/merge.json?leadId=1324
```

### Respuesta

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true
}
```

El posible cliente del parámetro path es el posible cliente ganador. Cuando los valores de los campos entran en conflicto, la combinación utiliza el valor del ganador a menos que ese valor esté vacío y el valor del registro perdedor no. Los posibles clientes del parámetro `leadId` o `leadIds` son los posibles clientes perdedores.

Para una suscripción habilitada para la sincronización con SFDC, use el parámetro `mergeInCRM` para realizar también la combinación en CRM. Si ambos registros están en SFDC y uno es un posible cliente de CRM, mientras que el otro es un contacto de CRM, el contacto de CRM gana independientemente del ganador especificado. Si un registro está en SFDC y el otro solo existe en Marketo, el posible cliente de SFDC gana independientemente del ganador especificado.

## Asociar actividad web

El seguimiento de posibles clientes (Munchkin) registra las visitas y los clics de los visitantes del sitio web y de las páginas de aterrizaje de Marketo. Estas actividades utilizan una clave que corresponde a la cookie &quot;_mkto_trk&quot; en el explorador del posible cliente, lo que permite a Marketo rastrear las actividades de la misma persona.

La asociación con un registro de posibles clientes se produce normalmente cuando un posible cliente sigue un vínculo de un correo electrónico de Marketo o envía un formulario de Marketo. Para asociar un posible cliente después de otro tipo de evento, utilice el punto de conexión Asociar posible cliente. Pase el ID del registro de posibles clientes conocido como parámetro de ruta y el valor de cookie &quot;_mkto_trk&quot; en el parámetro de consulta de cookie.

### Solicitud

```http
POST /rest/v1/leads/{id}/associate.json?cookie=id:287-GTJ-838%26token:_mch-marketo.com-1396310362214-46169
```

### Respuesta

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true
}
```

Si la cookie ya está asociada a un posible cliente conocido, el uso de esta API para un posible cliente diferente registra la nueva actividad web en el nuevo registro. La actividad web existente no se desplaza al nuevo registro.
Suscripción

Recupere registros de posibles clientes en función de su pertenencia a una lista o programa estático. También puede recuperar todas las listas estáticas, programas o campañas inteligentes que incluyan un posible cliente específico.

La estructura de respuesta y los parámetros opcionales coinciden con Obtener posibles clientes por tipo de filtro, pero esta API no acepta `filterType` ni `filterValues`.

Para encontrar el ID de lista en la interfaz de usuario de Marketo, vaya a la lista e inspeccione su dirección URL. En `https://app-****.marketo.com/#ST1001A1`, 1001 es la lista `id`.

## Obtener programas por ID de posible cliente

### Solicitud

```http
GET /rest/v1/list/{listId}/leads.json?batchSize=3
```

### Respuesta

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "nextPageToken":
"PS5VL5WD4UOWGOUCJR6VY7JQO2KUXL7BGBYXL4XH4BYZVPYSFBAONP4V4KQKN4SSBS55U4LEMAKE6===",
    "result":[
       {
            "id":50,
            "email":"kjashaedd@klooblept.com",
            "firstName":"Kataldar",
             "postalCode":"04828"
       },
       {
           "id":2343,
           "email":"kjashaedd@klooblept.com",
           "firstName":"Kataldar",
           "postalCode":"04828"
       },
      {
           "id":88498,
           "email":"kjashaedd@klooblept.com",
           "firstName":"Kataldar",
         "postalCode":"04828"
         }
    ]
}
```

## Obtener listas por ID de posible cliente

El extremo de obtención de listas por ID de posible cliente toma un parámetro de ruta de acceso de registro de posible cliente `id` y devuelve todas las listas estáticas que incluyen al posible cliente.

### Solicitud

```http
GET /rest/v1/leads/{id}/listMembership.json?batchSize=3
```

### Respuesta

```json
{
    "requestId": "1184b#1706f0ec23f",
    "result": [
        {
            "listId": 3379,
            "createdAt": "2016-05-17T19:32:44Z",
            "updatedAt": "2016-05-17T19:32:44Z"
        },
        {
            "listId": 2792,
            "createdAt": "2009-05-19T18:29:15Z",
            "updatedAt": "2009-05-19T18:29:15Z"
        },
        {
            "listId": 42,
            "createdAt": "2009-04-22T19:24:22Z",
            "updatedAt": "2009-04-22T19:24:22Z"
        }
    ],
    "success": true,
    "nextPageToken": "BFRV7OMVSNJWDVKVTUFS3XHT4E======",
    "moreResult": true
}
```

## Programas

Recupere la pertenencia al programa del mismo modo que la pertenencia a la lista. Obtener posibles clientes por id. de programa acepta los mismos parámetros de solicitud opcionales y requiere el parámetro de ruta de acceso `programId`.

De forma opcional, pase un parámetro de campos que contenga una lista de nombres de campo separados por comas. Si se omite fields, la respuesta incluye `email`, `updatedAt`, `createdAt`, `lastName`, `firstName`, `membership` y `id`. Si no se devuelve un campo solicitado, su valor implica que es nulo.

Cada elemento de la matriz de resultados es un posible cliente con un objeto secundario denominado &quot;pertenencia&quot;. Este objeto describe la relación del posible cliente con el programa solicitado y siempre incluye `progressionStatus`, `acquiredBy`, `reachedSuccess` y `membershipDate`.

Si el programa principal es un programa de participación, la pertenencia también incluye `stream`, `nurtureCadence` y `isExhausted` para describir la posición y actividad del posible cliente en ese programa.

### Solicitud

```http
GET /rest/v1/leads/programs/{programId}.json?batchSize=3
```

### Respuesta

```json
{
    "requestId": "13ad4#1727b748a17",
    "result": [
        {
            "id": 319141,
            "firstName": "Meera",
            "lastName": "Reed",
            "email": "mree@housestark.com",
            "updatedAt": "2020-04-21T16:27:14Z",
            "createdAt": "2020-04-21T16:27:14Z",
            "membership": {
                "id": 1127,
                "progressionStatus": "Visited",
                "progressionStatusType": "Visited",
                "isExhausted": false,
                "acquiredBy": true,
                "reachedSuccess": false,
                "membershipDate": "2020-04-21T16:27:16Z",
                "updatedAt": "2020-04-21T16:27:16Z"
            }
        },
        {
            "id": 319142,
            "firstName": "Jon",
            "lastName": "Umber",
            "email": "jumb@housestark.com",
            "updatedAt": "2020-04-21T16:27:14Z",
            "createdAt": "2020-04-21T16:27:14Z",
            "membership": {
                "id": 1127,
                "progressionStatus": "Visited",
                "progressionStatusType": "Visited",
                "isExhausted": false,
                "acquiredBy": true,
                "reachedSuccess": false,
                "membershipDate": "2020-04-21T16:27:16Z",
                "updatedAt": "2020-04-21T16:27:16Z"
            }
        },
        {
            "id": 319143,
            "firstName": "Lyanna",
            "lastName": "Mormont",
            "email": "lmor@housestark.com",
            "updatedAt": "2020-04-21T16:27:14Z",
            "createdAt": "2020-04-21T16:27:14Z",
            "membership": {
                "id": 1127,
                "progressionStatus": "Visited",
                "progressionStatusType": "Visited",
                "isExhausted": false,
                "acquiredBy": true,
                "reachedSuccess": false,
                "membershipDate": "2020-04-21T16:27:16Z",
                "updatedAt": "2020-04-21T16:27:16Z"
            }
        }
    ],
    "success": true,
    "nextPageToken": "SW3PTMBVFCNHSHJGZ7LQH3ZWNUOHKADJZ3MOQ2LOZZVNO3WEIUPDKPRTTHBSMW756KOCWURTOF2XS==="
}
```

El punto de conexión Obtener programas por ID de posible cliente toma un parámetro de ruta de ID de registro de posible cliente y devuelve todos los programas que incluyen al posible cliente. Use los parámetros opcionales `filterType` y `filterValues` para filtrar por id. de programa.

### Solicitud

```http
GET /rest/v1/leads/{id}/programMembership.json
```

### Respuesta

```json
{
    "requestId": "12e84#1706f13a379",
    "result": [
        {
            "id": 1044,
            "progressionStatus": "Sent",
            "isExhausted": false,
            "acquiredBy": false,
            "reachedSuccess": false,
            "membershipDate": "2016-05-27T19:50:29Z",
            "updatedAt": "2016-05-27T19:50:29Z"
        }
    ],
    "success": true,
    "moreResult": false
}
```

## Campañas inteligentes

El punto de conexión Obtener campañas inteligentes por ID de posible cliente toma un parámetro de ruta de ID de registro de posible cliente y devuelve todas las campañas inteligentes que incluyen al posible cliente.

### Solicitud

```http
GET /rest/v1/leads/{id}/smartCampaignMembership.json?batchSize=3
```

### Respuesta

```json
{
    "requestId": "e7b0#1706f163632",
    "result": [
        {
            "smartCampaignId": 3746,
            "createdAt": "2018-06-01T18:00:04Z",
            "updatedAt": "2018-06-01T18:00:06Z"
        },
        {
            "smartCampaignId": 3678,
            "createdAt": "2015-04-06T18:37:30Z",
            "updatedAt": "2015-04-06T18:37:41Z"
        },
        {
            "smartCampaignId": 3680,
            "createdAt": "2015-04-06T18:37:30Z",
            "updatedAt": "2015-04-06T18:37:40Z"
        }
    ],
    "success": true,
    "nextPageToken": "TNGAH3NKDUFDHNXUVGTNBXJCQM======",
    "moreResult": true
}
```

## Eliminar

Utilice el punto de conexión Eliminar posibles clientes para eliminar registros de posibles clientes. Especifique los ID de posibles clientes en el cuerpo con atributos de ID. Una solicitud puede eliminar hasta 300 posibles clientes. Envíe el encabezado Content-Type: application/json.

### Solicitud

```http
POST /rest/v1/leads/delete.json
```

### Cuerpo

```json
{
   "input":[
      {
         "id": 235
      },
      {
         "id":766
      }
   ]
}
```

### Respuesta

```json
{
  "requestId":"3608#16664333670",
  "result":[
    {
      "id":235,
      "status":"deleted"
    },
    {
      "id":766,
      "status":"deleted"
    }
  ],
  "success":true
}
```

## Relaciones

- Compañías a través del campo externalCompanyId en el registro de posibles clientes
- SalesPersons a través del campo externalSalesPersonId en el registro de posibles clientes
- Programas a través de la membresía del programa
- Listas a través de pertenencia a listas
- Actividades a través del campo leadId en la actividad
- Segmentación mediante campos de segmento individuales en el registro de posibles clientes
- Particiones a través del campo leadPartitionId en el registro de posibles clientes

## Tiempos de espera

Los extremos de los posibles clientes tienen un tiempo de espera de 30 segundos, excepto para los siguientes extremos:

- Posibles clientes de sincronización: 90s
- Posible cliente asociado: 60 años
- Combinar posibles clientes: años 180
- Actualizar partición del posible cliente: 60s
- Insertar posible cliente en Marketo: 90 s
- Obtener posibles clientes por tipo de filtro: 60s
- Obtener posibles clientes por ID de lista: 60 s
