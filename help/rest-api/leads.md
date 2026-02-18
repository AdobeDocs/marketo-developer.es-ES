---
title: Clientes potenciales
feature: REST API
description: Explore las funciones de la API de REST de Marketo Leads, incluida la descripción, la consulta por ID o filtro, los campos predeterminados, los límites y la recuperación de ECID.
exl-id: 0a2f7c38-02ae-4d97-acfe-9dd108a1f733
source-git-commit: d674384b3ab979df2322ece3f02155259d05431a
workflow-type: tm+mt
source-wordcount: '3409'
ht-degree: 3%

---

# Clientes potenciales

[Referencia de extremo de posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads)

La API del posible cliente de Marketo proporciona un gran conjunto de funcionalidades para aplicaciones CRUD sencillas con registros de posibles clientes, así como la capacidad de modificar la pertenencia de un posible cliente a listas y programas estáticos e iniciar el procesamiento de campañas inteligentes para posibles clientes.

## Describir

Una de las funciones clave de la API de posibles clientes es el método Describir. Utilice Describir posibles clientes para recuperar una lista completa de los campos disponibles para la interacción mediante la API de REST, así como metadatos para cada uno:

* Tipo de datos
* Nombres de API de REST
* Longitud (si corresponde)
* Solo lectura
* Etiqueta descriptiva

Describir es la principal fuente fiable para saber si los campos están disponibles para su uso, así como los metadatos sobre esos campos.

### Solicitud

```
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

Normalmente, las respuestas incluyen un conjunto de campos mucho más grande en la matriz de resultados, pero los estamos omitiendo para fines de demostración. Cada elemento de la matriz de resultados corresponde a un campo disponible en el registro de posibles clientes y tendrá como mínimo un id, un displayName y un tipo de datos. Los objetos secundarios rest y soap pueden estar presentes o no para un campo determinado, y su presencia indicará si el campo es válido para su uso en las API de REST o SOAP. La propiedad `readOnly` indica si el campo es de solo lectura mediante la API correspondiente (REST o SOAP). La propiedad length indica la longitud máxima del campo si está presente. La propiedad dataType indica el tipo de datos del campo.

## Consulta

Existen dos métodos principales para la recuperación de posibles clientes: los métodos Obtener posible cliente por ID y Obtener posibles clientes por tipo de filtro. Obtener posible cliente por ID toma un único ID de posible cliente como parámetro de ruta y devuelve un único registro de posible cliente.

Opcionalmente, puede pasar un parámetro de campos que contenga una lista de nombres de campo separados por comas para que se devuelvan. Si el parámetro fields no se incluye en esta solicitud, se devolverán los siguientes campos predeterminados: `email`, `updatedAt`, `createdAt`, `lastName`, `firstName` y `id`. Al solicitar una lista de campos, si se solicita un campo en particular pero no se devuelve, el valor se entiende como nulo.

### Solicitud

```
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

Para este método, siempre habrá un único registro en la primera posición de la matriz de resultados.

Obtener posibles clientes por tipo de filtro devolverá el mismo tipo de registros, pero puede devolver hasta 300 por página. Requiere los parámetros de consulta `filterType` y `filterValues`.

`filterType` acepta cualquier campo personalizado o la mayoría de los campos utilizados con frecuencia. Llame al extremo `Describe2` para obtener una lista completa de los campos en los que se pueden realizar búsquedas que se pueden usar en `filterType`. Al buscar por campo personalizado, solo se admiten los siguientes tipos de datos: `string`, `email`, `integer`. Puede obtener detalles de campo (descripción, tipo, etc.) utilizando el método mencionado Describir.

`filterValues` acepta hasta 300 valores en formato separado por comas. La llamada busca los registros donde el campo del posible cliente coincide con uno de los `filterValues` incluidos. Si el número de posibles clientes que coinciden con el filtro de posibles clientes es mayor que 1000, se devuelve el siguiente error: &quot;1003, Demasiados resultados coinciden con el filtro&quot;.

Si la longitud total de la solicitud de GET supera los 8 KB, se devuelve un error HTTP: &quot;414, URI too long&quot; (según RFC 7231). Como solución alternativa, puede cambiar GET a POST, agregar el parámetro _method=GET y colocar una cadena de consulta en el cuerpo de la solicitud.

### Solicitud

```
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

Esta llamada busca los registros que coinciden con los identificadores incluidos en `filterValues` y devuelve todos los registros coincidentes.

Si no se encuentran registros, la respuesta indica éxito, pero la matriz de resultados estará vacía.

### Respuesta

```json
{
"requestId": "177a1#1578b643357",
"result": [],
"success": true
}
```

Tanto el Obtener posible cliente por ID como el Obtener posibles clientes por tipo de filtro también aceptarán un parámetro de consulta de campos, que acepta una lista de campos API separados por comas. Si se incluye, cada registro de la respuesta incluirá esos campos enumerados.  Si se omite, se devolverá un conjunto predeterminado de campos: `id`, `email`, `updatedAt`, `createdAt`, `firstName` y `lastName`.

## ADOBE ECID

Cuando se habilita la función Compartir audiencias de Adobe Experience Cloud, se produce un proceso de sincronización de cookies que asocia el Adobe Experience Cloud ID (ECID) con posibles clientes de Marketo.  Los métodos de recuperación de posibles clientes mencionados anteriormente se pueden utilizar para recuperar valores ECID asociados.  Para ello, especifique `ecids` en el parámetro fields. Por ejemplo, `&fields=email,firstName,lastName,ecids`.

## Crear y actualizar

Además de recuperar los datos de posibles clientes, puede crear, actualizar y eliminar el registro de posibles clientes a través de la API. La creación y actualización de posibles clientes comparten el mismo punto de conexión con el tipo de operación que se define en la solicitud y se pueden crear o actualizar hasta 300 registros al mismo tiempo.

>[!NOTE]
>
> No se admite la actualización de los campos de la compañía mediante el extremo [Sync Leads](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/syncLeadUsingPOST). En su lugar, use el punto de conexión [Compañías de sincronización](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/syncCompaniesUsingPOST).

>[!NOTE]
>
> Al crear o actualizar el valor de correo electrónico en un registro de persona, solo se admiten caracteres ASCII en el campo de dirección de correo electrónico.

### Solicitud

```
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

En esta solicitud, verá dos campos importantes, `action` y `lookupField`.  `action` especifica el tipo de operación de la solicitud y puede ser `createOrUpdate`, `createOnly`, `updateOnly` o `createDuplicate`. Si se omite, el valor predeterminado de la acción es `createOrUpdate`.  El parámetro `lookupField` especifica la clave que se debe usar cuando la acción es `createOrUpdate` o `updateOnly`. Si se omite `lookupField`, la clave predeterminada es `email`.

De forma predeterminada, se utiliza la partición predeterminada. Opcionalmente, puede especificar el parámetro `partitionName`, que solo funciona si la acción es `createOnly` o `createOrUpdate`. Para que `partitionName` funcione como criterio de deduplicación adicional, debe formar parte del tipo de origen en las reglas de desduplicación personalizadas. Durante una operación de actualización, si no existe un posible cliente en la partición especificada, se devuelve un error. Si el usuario solo de API no tiene permiso para acceder a la partición especificada, se devuelve un error.

El campo `id` solo se puede incluir como parámetro al usar la acción `updateOnly`, ya que `id` es una clave única administrada por el sistema.

La solicitud también debe tener un parámetro `input`, que es una matriz de registros de posibles clientes. Cada registro de posible cliente es un objeto JSON con cualquier número de campos de posible cliente. Las claves incluidas en un registro deben ser únicas para ese registro y todas las cadenas JSON deben estar codificadas en UTF-8. El campo `externalCompanyId` se puede usar para vincular el registro de posible cliente a un registro de empresa. El campo `externalSalesPersonId` se puede usar para vincular el registro de cliente potencial a un registro de vendedor.

Nota: Al realizar solicitudes de inserción de posibles clientes de forma simultánea o en sucesión rápida, pueden producirse registros duplicados al realizar varias solicitudes con el mismo valor clave si se realiza una llamada posterior con el mismo valor antes de que se devuelva la primera. Esto se puede evitar usando `createOnly` o `updateOnly`, según corresponda, o poniendo en cola las llamadas y esperando a que se devuelva la llamada antes de realizar llamadas de inserción subsiguientes con la misma clave.

## Campos

El objeto de posible cliente contiene campos estándar y, opcionalmente, campos personalizados. Los campos estándar están presentes en cada suscripción de Marketo Engage, mientras que el usuario crea los campos personalizados según sea necesario. Cada definición de campo consta de un conjunto de atributos que describen el campo. Algunos ejemplos de atributos son nombre para mostrar, nombre de API y dataType. Estos atributos se conocen colectivamente como metadatos.

Los siguientes extremos permiten consultar, crear y actualizar campos en el objeto de posible cliente. Estas API requieren que el usuario de la API propietaria tenga una función con uno o ambos permisos Campo estándar de esquema de lectura-escritura o Campo personalizado de esquema de lectura-escritura.

## Campos de consulta

La consulta de campos de posibles clientes es sencilla. Puede consultar un solo campo de posible cliente por nombre de API o consultar el conjunto de todos los campos de posible cliente. Se pueden recuperar tanto los campos estándar como los campos personalizados, en función de los permisos de función que se utilicen. También se recuperan los campos ocultos.

## Por nombre

El extremo Obtener campo de posible cliente por nombre recupera los metadatos de un único campo en el objeto de posible cliente. El parámetro de ruta fieldApiName requerido especifica el nombre de API del campo. La respuesta es como el punto final Describir posible cliente, pero contiene metadatos adicionales como el atributo isCustom, que indica si el campo es un campo personalizado.

### Solicitud

```
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

El extremo Obtener campos de posible cliente recupera los metadatos de todos los campos del objeto de posible cliente, incluido. De forma predeterminada, se devuelve un máximo de 300 registros. Puede usar el parámetro de consulta `batchSize` para reducir este número. Si el atributo `moreResult` es true, significa que hay más resultados disponibles. Continúe llamando a este extremo hasta que el atributo `moreResult` devuelva el valor &quot;False&quot;, lo que significa que no hay resultados disponibles. Los `nextPageToken` devueltos por esta API siempre se deben reutilizar para la siguiente iteración de esta llamada.

### Solicitud

```
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

El extremo Crear campos de posible cliente crea uno o varios campos personalizados en el objeto de posible cliente. Este extremo proporciona una funcionalidad comparable a la disponible en la interfaz de usuario de Marketo Engage. Puede crear un máximo de 100 campos personalizados con este extremo.
Tenga en cuenta cuidadosamente cada campo que cree en la instancia de producción de Marketo Engage mediante la API.  Una vez creado un campo, no se puede eliminar (solo se puede ocultar). La proliferación de campos no utilizados es una mala práctica que añadirá desorden a su instancia.

El parámetro de entrada requerido es una matriz de objetos de campo de posibles clientes. Cada objeto contiene uno o más atributos. Los atributos requeridos son `displayName`, `name` y `dataType`, que corresponden al nombre para mostrar del campo en la interfaz de usuario, el nombre de API del campo y el tipo de campo respectivamente.  Opcionalmente, puede especificar `description`, `isHidden`, `isHtmlEncodingInEmail` y `isSensitive`.

Hay algunas reglas asociadas con el nombre y el nombre de `displayName`. El atributo name debe ser único, comenzar con una letra y contener solo letras, números o guiones bajos. `displayName` debe ser único y no puede contener caracteres especiales.  Una convención de nombres común es aplicar mayúsculas y minúsculas a `displayName` para generar el nombre. Por ejemplo, un `displayName` de &quot;Mi campo personalizado&quot; produciría el nombre &quot;myCustomField&quot;.

### Solicitud

```
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

El punto final Actualizar campo de posible cliente actualiza un único campo personalizado en el objeto de posible cliente. En su mayor parte, las operaciones de actualización de campos realizadas mediante la interfaz de usuario de Marketo Engage se pueden realizar mediante la API. En la tabla siguiente se resumen algunas diferencias.

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

El parámetro de ruta de acceso `fieldApiName` requerido especifica el nombre de API del campo que se va a actualizar. El parámetro de entrada requerido es una matriz que contiene un único objeto de campo de posible cliente.  El objeto de campo contiene uno o varios atributos.

### Solicitud

```
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

El posible cliente push es una alternativa para sincronizar posibles clientes con Marketo, diseñada principalmente para permitir un mayor grado de capacidad de déclencheur que los posibles clientes estándar (similar en uso a un formulario de Marketo). Además de la sincronización de los campos de posibles clientes, este extremo permite la asociación de posibles clientes en función de los valores de las cookies, que se pasan al extremo. Para ello, pase el valor `mkt_tok` generado al hacer clic en un mensaje de correo electrónico de Marketo o al pasar un nombre de programa en la llamada. Este punto de conexión también crea una sola actividad activable, que está asociada a un programa o a una campaña en Marketo. Esto permite activar eventos de captura de posibles clientes atribuidos a una campaña o programa específico para iniciar flujos de trabajo asociados desde Marketo.

La interfaz del posible cliente push es muy similar a Sincronizar posibles clientes. Todas las claves principales son válidas y se utilizan los mismos nombres de API para los campos (no hay parámetro de acción porque siempre es una operación de actualización). Se requieren `programName` y los parámetros de entrada, y los parámetros `lookupField`, `source` y `reason` son opcionales. El parámetro de entrada es una matriz de objetos de posibles clientes. La actividad resultante se atribuye al programa con nombre correspondiente. Los parámetros `source` y `reason` son campos de cadena arbitrarios que se pueden agregar a la solicitud para incrustar esos valores en las actividades resultantes. Pueden utilizarse como restricciones en los déclencheur correspondientes (el posible cliente se inserta en Marketo) y (el posible cliente se inserta en Marketo).

Nota sobre las actividades anónimas. Si desea asociar actividades anónimas anteriores con el posible cliente recién creado, no especifique el atributo de cookies en el objeto de posible cliente y llame a Asociar posible cliente después de Insertar posible cliente. Si desea crear un nuevo posible cliente sin historial de actividad, simplemente especifique el atributo cookies en el objeto de posible cliente.

### Solicitud

```
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

Para pasar el parámetro `mkt_tok`, asigne el valor al miembro mktToken dentro de un registro de posible cliente en el parámetro de entrada de la siguiente manera.

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

Enviar formulario es una alternativa para sincronizar posibles clientes con Marketo y está diseñado para proporcionar una funcionalidad equivalente a un envío de formulario Marketo. Esto permite activar eventos de captura de posibles clientes atribuidos a una campaña o programa específico para iniciar flujos de trabajo asociados desde Marketo.

El extremo del formulario de envío admite la siguiente funcionalidad:

* Actualiza un registro de posibles clientes utilizando el campo de correo electrónico como clave principal
* Crea una actividad &quot;Rellenar formulario&quot; asociada a un programa o a una campaña
* Permite la asociación de posibles clientes en función del valor de cookie
* Realiza validación de campo de formulario

El envío de un formulario sigue el patrón estándar de la base de datos de posibles clientes. Se pasa un único registro de objeto en el miembro de entrada requerido del cuerpo de JSON de una petición POST. El miembro `formId` requerido contiene el id. de formulario de Marketo de destino.

El elemento opcional `programId` se puede usar para especificar el programa al que se va a agregar el posible cliente o para especificar el programa al que se van a agregar los campos personalizados de miembro del programa. Si se proporciona `programId`, el posible cliente se agregará al programa y también se agregarán todos los campos de miembros del programa presentes en el formulario. Tenga en cuenta que el programa especificado debe estar en el mismo espacio de trabajo que el formulario. Si el formulario no contiene campos personalizados de miembro de programa y no se proporciona `programId`, el posible cliente no se agrega a un programa. Si el formulario reside en un programa y no se proporciona `programId`, ese programa se utiliza cuando hay uno o más campos personalizados de miembro del programa en el formulario.

En el registro de entrada, se requiere el objeto `leadFormFields`. Este objeto contiene uno o más pares de nombre/valor que corresponden a los campos de formulario que se van a rellenar.  Todos los campos especificados deben definirse dentro del formulario especificado. El nombre es el nombre de la API de REST para el campo. Tenga en cuenta que el campo `email` es obligatorio.

El objeto de miembro `visitorData` es opcional y contiene pares de nombre/valor que corresponden a datos de visita a la página, incluidos `pageURL`, `queryString`, `leadClientIpAddress` y `userAgentString`. Se puede utilizar para rellenar campos de actividad adicionales con fines de filtrado y activación.

La cadena de miembro de la cookie es opcional y le permite asociar una cookie de Munchkin con un registro de persona en Marketo. Cuando se crea un nuevo posible cliente, cualquier actividad anónima anterior se asocia con ese posible cliente, a menos que el valor de la cookie se haya asociado anteriormente con otro registro conocido. Si el valor de la cookie se asoció anteriormente, las nuevas actividades se rastrearán con el registro, pero las actividades antiguas no se migrarán fuera del registro conocido existente. Para crear un nuevo posible cliente sin historial de actividades, simplemente omita el miembro de la cookie.

Los nuevos posibles clientes se crean en la partición principal del espacio de trabajo en el que reside el formulario.

### Solicitud

```
POST /rest/v1/leads/submitForm.json
```

### Encabezado

```
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

Aquí podemos ver los detalles de la actividad &quot;Rellenar formulario&quot; correspondientes desde la interfaz de usuario de Marketo Engage:

![Rellenar la interfaz de usuario del formulario](assets/fill_out_form_activity_details.png)

## Combinar

>[!NOTE]
>A partir del 31 de marzo de 2026, las llamadas que incluyan más de 25 ID en el parámetro `leadIds` de una llamada a la API de Merge Leads generarán un código de error 1080, y se omitirá la llamada. Los trabajos que requieren la fusión de más de 25 registros en uno deben dividirse en varios trabajos para garantizar el éxito de esas llamadas.
>

A veces es necesario combinar registros duplicados y Marketo lo facilita mediante la API de combinación de posibles clientes. Al combinar los posibles clientes combinarán sus registros de actividades, programas, campañas y suscripciones a listas, así como información de CRM, y combinarán todos sus valores de campo en un único registro. Combinar posibles clientes toma un id. de posible cliente como parámetro de ruta de acceso y un solo `leadId` como parámetro de consulta o una lista de 25 id. separados por comas o menos en el parámetro `leadIds`


### Solicitud

```
POST /rest/v1/leads/{id}/merge.json?leadId=1324
```

### Respuesta

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true
}
```

El posible cliente especificado en el parámetro de ruta es el posible cliente ganador, por lo que si hay algún campo en conflicto entre los registros que se combinan, se tomará el valor del ganador, excepto si el campo del registro ganador está vacío y el campo correspondiente del registro perdedor no. Los posibles clientes especificados en el parámetro `leadId` o `leadIds` son los posibles clientes perdedores.

Si tiene una suscripción habilitada para la sincronización con SFDC, también puede usar el parámetro `mergeInCRM` en la solicitud. Si se establece en true, también se realizará la combinación correspondiente en el CRM. Si ambos posibles clientes están en SFDC y uno es un posible cliente de CRM y el otro es un contacto de CRM, el ganador es el contacto de CRM (independientemente del posible cliente especificado como ganador). Si uno de los posibles clientes está en SFDC y el otro solo en Marketo, el ganador es el posible cliente de SFDC (independientemente del posible cliente especificado como ganador).

## Asociar actividad web

A través del seguimiento de posibles clientes (Munchkin), Marketo registra la actividad web de los visitantes de su sitio web y de las páginas de destino de Marketo. Estas actividades, visitas y clics, se registran con una clave que corresponde a una cookie &quot;_mkto_trk&quot; establecida en el explorador del posible cliente, que Marketo utiliza para realizar un seguimiento de las actividades de la misma persona. Normalmente, la asociación a registros de posibles clientes se produce cuando un posible cliente hace clic desde un correo electrónico de Marketo o rellena un formulario de Marketo, pero a veces una asociación se puede activar mediante un tipo de evento diferente y puede utilizar el punto final Asociar posible cliente para hacerlo. El punto de conexión toma el ID del registro de posible cliente conocido como parámetro de ruta y el valor de cookie &quot;_mkto_trk&quot; en el parámetro de consulta de cookie.

### Solicitud

```
POST /rest/v1/leads/{id}/associate.json?cookie=id:287-GTJ-838%26token:_mch-marketo.com-1396310362214-46169
```

### Respuesta

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true
}
```

Si una cookie ya está asociada con un registro de posibles clientes conocido, el uso de esta API en un registro de posibles clientes diferente provoca que se registre una nueva actividad web en ese registro, pero no moverá ninguna actividad web existente al nuevo registro.
Membresía

Los registros de posibles clientes también se pueden recuperar en función de su pertenencia a una lista estática o a un programa. Además, puede recuperar todas las listas estáticas, programas o campañas inteligentes a los que pertenece un posible cliente.

La estructura de respuesta y los parámetros opcionales son idénticos a los de Obtener posibles clientes por tipo de filtro, aunque filterType y filterValues no se pueden utilizar con esta API.
Para acceder al ID de lista a través de la IU de Marketo, vaya a la lista. La lista `id` se encuentra en la dirección URL de la lista estática `https://app-****.marketo.com/#ST1001A1`. En este ejemplo, 1001 es `id` para la lista.

### Solicitud

```
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

El extremo de obtención de listas por ID de posible cliente toma un parámetro de ruta de acceso de registro de posible cliente `id` y devuelve todos los registros de lista estática a los que pertenece el posible cliente.

### Solicitud

```
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

La pertenencia a programas se puede recuperar de forma similar a las listas. Los mismos parámetros de solicitud opcionales están disponibles al llamar al extremo Obtener posibles clientes por id. de programa y pasar el parámetro de ruta de acceso `programId`.

Opcionalmente, puede pasar un parámetro de campos que contenga una lista de nombres de campo separados por comas para que se devuelvan. Si el parámetro fields no se incluye en esta solicitud, se devolverán los siguientes campos predeterminados: `email`, `updatedAt`, `createdAt`, `lastName`, `firstName`, `membership` y `id`. Al solicitar una lista de campos, si se solicita un campo en particular pero no se devuelve, el valor se entiende como nulo.

La estructura de la respuesta es muy similar, ya que cada elemento de la matriz de resultados es un posible cliente, con la diferencia de que cada registro también tiene un objeto secundario denominado &quot;pertenencia&quot;. Este objeto de pertenencia incluye datos sobre la relación del posible cliente con el programa indicado en la llamada, mostrando siempre sus `progressionStatus`, `acquiredBy`, `reachedSuccess` y `membershipDate`. Si el programa principal también es un programa de participación, la pertenencia tendrá miembros `stream`, `nurtureCadence` y `isExhausted` para indicar su posición y actividad en el programa de participación.

### Solicitud

```
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

El punto de conexión Obtener programas por ID de posible cliente toma un parámetro de ruta de acceso de ID de registro de posible cliente y devuelve todos los registros de programa de los que es miembro el posible cliente. Los parámetros opcionales `filterType` y `filterValues` le permiten filtrar por Id. de programa.

### Solicitud

```
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

El punto de conexión Obtener campañas inteligentes por ID de posible cliente toma un parámetro de ruta de ID de registro de posible cliente y devuelve todos los registros de campaña inteligente de los que es miembro el posible cliente.

### Solicitud

```
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

La eliminación de posibles clientes es sencilla mediante el punto de conexión Eliminar posibles clientes.  Especifique los ID de posible cliente que se eliminarán mediante los atributos de ID del cuerpo.  El máximo es de 300 posibles clientes por solicitud.  Utilice el encabezado Content-Type: application/json.

### Solicitud

```
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

* Compañías a través del campo externalCompanyId en el registro de posibles clientes
* SalesPersons a través del campo externalSalesPersonId en el registro de posibles clientes
* Programas a través de la membresía del programa
* Listas a través de pertenencia a listas
* Actividades a través del campo leadId en la actividad
* Segmentación mediante campos de segmento individuales en registro de posibles clientes
* Particiones mediante leadPartitionId en el registro de posibles clientes

## Tiempos de espera

Los puntos de conexión de posibles clientes tienen un tiempo de espera de 30 segundos a menos que se indique lo siguiente:

* Posibles clientes de sincronización: 90s
* Posible cliente asociado: 60 años
* Combinar posibles clientes: años 180
* Actualizar partición del posible cliente: 60s
* Insertar posible cliente en Marketo: 90 s
* Obtener posibles clientes por tipo de filtro: 60s
* Obtener posibles clientes por ID de lista: 60 s
