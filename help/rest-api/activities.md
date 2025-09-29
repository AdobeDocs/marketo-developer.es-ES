---
title: Actividades
feature: REST API
description: Utilice la API de REST de actividades de Marketo Engage para enumerar tipos de actividades, recuperar actividades de posible cliente con tokens de paginación y gestionar cambios personalizados y de valor de datos.
exl-id: 1e69af23-2b0c-467a-897c-1dcf81343e73
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '2046'
ht-degree: 0%

---

# Actividades

Marketo permite una gran variedad de tipos de actividades relacionadas con los registros de posibles clientes.  Casi todos los pasos de cambio, acción o flujo se registran en el registro de actividad de un posible cliente y se pueden recuperar mediante la API o aprovechar en los déclencheur y filtros de listas inteligentes y campañas inteligentes.  Las actividades siempre se relacionan con el registro de posibles clientes a través de leadId, correspondiente al campo ID del registro, y también tiene un ID único propio.

Existen un gran número de tipos de actividades potenciales, que pueden variar de una suscripción a otra y tienen definiciones únicas para cada uno. Aunque cada actividad tiene sus propios valores únicos `id`, `leadId` y `activityDate`, los valores `primaryAttributeValueId` y `primaryAttributeValue` varían en su significado.

Marketo también permite crear tipos de actividades personalizadas mediante la API de metadatos de actividades personalizadas. La adición de actividades personalizadas se realiza mediante la API Añadir actividades personalizadas.

La mayoría de las actividades se purgarán después de algún período de tiempo.

## Describir

Para recuperar una lista de tipos disponibles y sus definiciones para una instancia, puede utilizar el punto de conexión [Obtener tipos de actividades](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getAllActivityTypesUsingGET).

```
GET /rest/v1/activities/types.json
```

```json
  "requestId": "6e78#148ad3b76f1",
  "success": true,
  "result": [
    {
      "id": 2,
      "name": "Fill Out Form",
      "description": "User fills out and submits form on web page",
      "primaryAttribute": {
        "name": "Webform ID",
        "dataType": "integer"
      },
      "attributes": [
        {
          "name": "Client IP Address",
          "dataType": "string"
        },
        {
          "name": "Form Fields",
          "dataType": "text"
        },
        {
          "name": "Query Parameters",
          "dataType": "string"
        },
        {
          "name": "Referrer URL",
          "dataType": "string"
        },
        {
          "name": "User Agent",
          "dataType": "string"
        },
        {
          "name": "Webpage ID",
          "dataType": "integer"
        }
      ]
    }
  ]
}
```

Las respuestas del mundo real incluyen muchas más definiciones. En este ejemplo, el tipo mostrado es un &quot;Rellenar formulario&quot;, que tiene un atributo principal de &quot;ID de formulario web&quot;, que hace referencia al ID de Marketo del formulario rellenado y se puede utilizar para relacionarlo con ese recurso en particular en Marketo. Además, hay definiciones para cada uno de los atributos posibles de un registro de actividad en particular de este tipo y sus tipos de datos. Tenga en cuenta que si el campo está vacío, ese atributo en particular se omite en un registro de actividad individual.

## Consulta

Para recuperar actividades de Marketo, llame al extremo [Obtener actividades de posible cliente](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadActivitiesUsingGET). Primero debe recuperar un token de paginación para la fecha y hora desde la que desee comenzar a recuperar actividades. Luego pasa el token de paginación en el parámetro de consulta `nextPageToken`. Además, se pasan hasta diez identificadores de tipo de actividad en el parámetro de consulta `activityTypeIds` como una lista separada por comas.

Si lo desea, puede incluir un parámetro de consulta listId para restringir la búsqueda únicamente a los registros incluidos en una lista estática específica, o un parámetro de consulta leadIds y buscar actividades únicamente de un conjunto especificado de posibles clientes. Puede pasar hasta 30 leadIds como una lista separada por comas.

```
GET /rest/v1/activities.json?activityTypeIds=1&nextPageToken=WQV2VQVPPCKHC6AQYVK7JDSA3I3LCWXH3Y6IIZ7YSGQLXHCPVE5Q====
```

```json
{
  "requestId": "24fd#15188a88d7f",
  "result": [
    {
      "id": 102988,
      "marketoGUID": "102988",
      "leadId": 1,
      "activityDate": "2023-01-16T23:32:19Z",
      "activityTypeId": 1,
      "primaryAttributeValueId": 71,
      "primaryAttributeValue": "localhost/munchkintest2.html",
      "attributes": [
        {
          "name": "Client IP Address",
          "value": "10.0.19.252"
        },
        {
          "name": "Query Parameters",
          "value": ""
        },
        {
          "name": "Referrer URL",
          "value": ""
        },
        {
          "name": "User Agent",
          "value": "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.95 Safari/537.36"
        },
        {
          "name": "Webpage URL",
          "value": "/munchkintest2.html"
        }
      ]
    }
  ],
  "success": true,
  "nextPageToken": "WQV2VQVPPCKHC6AQYVK7JDSA3J62DUSJ3EXJGDPTKPEBFW3SAVUA====",
  "moreResult": false
}
```

Para la primera llamada, use la API Obtener token de paginación para obtener `nextPageToken`. Para las llamadas subsiguientes a este extremo, utilice el `nextPageToken returned` de la respuesta. Este extremo siempre devuelve `the nextPageToken`.

Si el atributo `moreResult` es true, significa que hay más resultados disponibles. Continúe llamando a este extremo hasta que el atributo `moreResult` devuelva el valor &quot;False&quot;, lo que significa que no hay resultados disponibles. Los `nextPageToken` devueltos por esta API siempre se deben reutilizar para la siguiente iteración de esta llamada.

En algunos casos, esta API puede responder con menos de 300 elementos de actividad, pero también tiene el atributo `moreResult` establecido en true.  Esto indica que hay más actividades que se pueden devolver y que se puede consultar el extremo para actividades más recientes incluyendo la `nextPageToken` devuelta en una llamada posterior.

Tenga en cuenta que dentro de cada elemento de matriz de resultados, el atributo entero `id` se reemplaza por el atributo de cadena `marketoGUID` como identificador único.

### Cambios en el valor de los datos

Para las actividades de Cambio de valor de datos, se proporciona una versión especializada de la API de actividades. El extremo [Obtener cambios de posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadChangesUsingGET) solo devuelve actividades de registros de cambio de valor de datos a campos de posibles clientes. La interfaz es la misma que la API Obtener actividades de posibles clientes con dos diferencias:

* No hay parámetro `activityTypeIds`, ya que el extremo solo devuelve las actividades Cambio de valor de datos y Nuevo posible cliente.
* El parámetro de consulta `fields` es obligatorio, donde puede pasar una lista de campos separados por comas para indicar para qué campos desea recuperar los cambios.

```
GET /rest/v1/activities/leadchanges.json?nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ&fields=firstName,lastName,department
```

```json
{
  "requestId": "a9ae#148add1e53d",
  "success": true,
  "nextPageToken": "GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBRGA3TQ===",
  "moreResult": true,
  "result": [
    {
      "id": 1078,
      "marketoGUID": "1078",
      "leadId": 775,
      "activityDate": "2014-09-17T22:31:49+0000",
      "activityTypeId": 13,
      "fields": [
        {
          "id": 48,
          "name": "firstName",
          "newValue": "FirstName_6176",
          "oldValue": "FirstName_4914"
        }
      ],
      "attributes": [
        {
          "name": "Reason",
          "value": "Web service API"
        },
        {
          "name": "Source",
          "value": "Web service API"
        },
        {
          "name": "Lead ID",
          "value": 775
        }
      ]
    }
  ]
}
```

Cada actividad de la respuesta tiene una matriz de campos, incluida una lista de cambios en la actividad, que especificará los `id` y `name` del campo cambiado, así como los valores nuevos y antiguos relativos al cambio.

Tenga en cuenta que dentro de cada elemento de matriz de resultados, el atributo entero `id` se reemplaza por el atributo de cadena `marketoGUID` como identificador único.

### Posibles clientes eliminados

También hay un punto final especial [Obtener posibles clientes eliminados](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getDeletedLeadsUsingGET) para recuperar las actividades eliminadas de Marketo.

```
GET /rest/v1/activities/deletedleads.json?nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ
```

```json
{
  "requestId": "a9ae#148add1e53d",
  "success": true,
  "nextPageToken": "GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBRGA3TQ===",
  "moreResult": true,
  "result": [
    {
      "id": 2,
      "marketoGUID": "2",
      "leadId": 6,
      "activityDate": "2013-09-26T06:56:35+0000",
      "activityTypeId": 37,
      "primaryAttributeValueId": 6,
      "primaryAttributeValue": "Owyliphys Iledil",
      "attributes": []
    },
    {
      "id": 3,
      "marketoGUID": "3",
      "leadId": 9,
      "activityDate": "2013-12-28T00:39:45+0000",
      "activityTypeId": 37,
      "primaryAttributeValueId": 4,
      "primaryAttributeValue": "First Last",
      "attributes": []
    }
  ]
}
```

Tenga en cuenta que dentro de cada elemento de matriz de resultados, el atributo entero `id` se reemplaza por el atributo de cadena `marketoGUID` como identificador único.

### Página a través de resultados

De forma predeterminada, los extremos mencionados en esta sección devuelven 300 elementos de actividad a la vez.  Si el atributo `moreResult` es verdadero, hay más resultados disponibles. Llame al extremo hasta que el atributo `moreResult` devuelva el valor &quot;false&quot;, lo que significa que no hay más resultados disponibles. El `nextPageToken` devuelto desde este extremo siempre se debe reutilizar para la siguiente iteración de esta llamada.

En algunos casos, este extremo puede responder con menos de 300 elementos de actividad, pero también tiene el atributo `moreResult` establecido en true.  Esto indica que hay actividades adicionales que se pueden devolver y que se pueden consultar actividades más recientes en el extremo incluyendo el elemento devuelto `nextPageToken` en una llamada posterior. Tenga en cuenta que `nextPageToken` debe estar codificado en la dirección URL de la solicitud.

## Tipos de actividades personalizadas

Las actividades personalizadas funcionan igual que las actividades estándar, excepto que el esquema lo administran terceros y no Marketo. Las instancias de actividades personalizadas están vinculadas a registros de posibles clientes a través de `leadId` del mismo modo que las actividades estándar, pero tanto los atributos primarios como los secundarios se definen arbitrariamente. Cuando se aprueba un tipo de actividad personalizada, se crean un déclencheur y un filtro de listas inteligentes correspondientes, de modo que los posibles clientes se puedan procesar en función de los datos de actividad personalizados actuales o históricos.

* Número máximo de actividades personalizadas: 10
* Número máximo de atributos por actividad personalizada: 20

La recuperación de datos de actividad personalizados se realiza de la misma manera que las actividades estándar, a través de la API [Obtener actividades principales](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadActivitiesUsingGET).

## Tipos de consulta

Además del extremo estándar de Obtener tipos de actividad, los extremos de [Obtener tipos de actividad personalizados](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getCustomActivityTypeUsingGET) y [Describir tipo de actividad personalizada](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/describeCustomActivityTypeUsingGET) devuelven detalles sobre los tipos de actividad aprovisionados en la instancia de Marketo y metadatos sobre los atributos de un tipo determinado. El objeto [Get Activity Types](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getAllActivityTypesUsingGET) normal sigue devolviendo metadatos sobre las actividades personalizadas, pero no indica si un tipo determinado es personalizado.

### Obtener tipos

```
GET /rest/v1/activities/external/types.json
```

```json
{
  "requestId": "185d6#14b51985ff0",
  "success": true,
  "result": [
    {
      "id": 100001,
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attends Conference",
      "filterName": "Attended Conference",
      "createdAt": "2016-02-03T22:36:23Z",
      "updatedAt": "2016-02-03T22:36:23Z",
      "status": "approved"
    }
  ]
}
```

### Describir tipos

Para las descripciones de tipo, debe pasar `apiName` como parámetro de ruta de acceso. De forma predeterminada, obtiene la versión aprobada de la actividad. Opcionalmente, puede pasar el parámetro `draft=true` para recuperar la versión de borrador de la actividad.

```
GET /rest/v1/activities/external/type/{apiName}/describe.json
```

```json
{
  "requestId": "185d6#14b51985ff0",
  "success": true,
  "result": [
    {
      "id": 100001,
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attends Conference",
      "filterName": "Attended Conference",
      "createdAt": "2016-02-03T22:36:23Z",
      "updatedAt": "2016-02-03T22:36:23Z",
      "status": "approved",
      "primaryAttribute": {
        "apiName": "conferenceName",
        "name": "Conference Name",
        "description": "Name of the conference",
        "dataType": "string"
      },
      "attributes": [
        {
          "apiName": "conferenceDate",
          "name": "Conference Date",
          "description": "Date of the conference",
          "dataType": "datetime"
        },
        {
          "apiName": "numberOfAttendees",
          "name": "Number of Attendees",
          "description": "Number of people attending conference",
          "dataType": "integer"
        }
      ]
    }
  ]
}
```

## Crear tipo

Cada tipo de actividad personalizada requiere un nombre para mostrar, un nombre de API, un nombre de déclencheur, un nombre de filtro y un atributo principal.

Para garantizar la coherencia de los tipos con las convenciones de Marketo y evitar conflictos, es importante seguir algunas directrices al crear los tipos:

**Nombre para mostrar:** El nombre para mostrar del tipo de actividad debe describir brevemente lo que representa un registro de actividad, como &quot;Enviar correo electrónico&quot; o &quot;Cambiar valor de datos&quot;. Estos nombres deben aparecer en forma infinitiva, es decir, &quot;Asistir al evento&quot;.  Los nombres para mostrar aceptan caracteres alfanuméricos, espacios y guiones bajos. Los nombres para mostrar deben contener al menos una letra.

**Nombre de API:** El nombre de API está compuesto por caracteres alfanuméricos (longitud máxima de 255). Si es socio de LaunchPoint, debe anteponer un área de nombres representativa a los nombres de API de tipo de actividad. Esto sirve para evitar conflictos con los tipos proporcionados por el cliente.  La convención consiste en utilizar minúscula o camelCase para ayudar a distinguir entre otras cadenas de texto.

**Descripción:** Para las actividades que pueden tener un comportamiento no obvio, debe incluir una descripción de lo que representa el tipo de actividad con relación al posible cliente.

**Nombre de Déclencheur:** Cada tipo de actividad debe tener un nombre de déclencheur único legible en lenguaje natural. Los nombres de los déclencheur deben estar en el tiempo presente de la tercera persona, como &quot;Asiste a un evento&quot;. Los socios de LaunchPoint deben incluir el nombre de su empresa en la actividad, como &quot;Asiste al seminario web - Empresa Acme&quot;.

**Nombre de filtro:**  Cada tipo de actividad debe tener un nombre de filtro único legible en lenguaje natural. Los nombres de los filtros deben estar en tiempo pasado en tercera persona, como &quot;Ha asistido a un evento&quot;. Los socios de LaunchPoint deben incluir el nombre de su empresa en la actividad, que es &quot;Seminario web al que asistió: empresa Acme&quot;.

**Atributo principal:** El atributo principal de una actividad personalizada debe ser el campo más significativo para el tipo de actividad. Por ejemplo, para una actividad &quot;Evento al que asistió&quot; este sería el nombre del evento. Los atributos principales se incluyen como parámetros de forma predeterminada en cada instancia de un déclencheur o filtro para ese tipo de actividad y el valor se muestra en el registro de actividad de una persona sin necesidad de profundizar en la actividad.

Cuando se crea una actividad personalizada, se crea como borrador y debe aprobarse antes de poder utilizarla para agregar registros de actividad de ese tipo. Todas las actualizaciones se aplican implícitamente a la versión de borrador del tipo. Para reflejar los cambios en la versión activa del tipo, debe aprobarse. Cuando se aprueba y se utiliza un tipo de actividad personalizada, no se pueden realizar cambios en los campos anteriores.

Al crear un tipo, el parámetro description es opcional, mientras que se requieren todos los parámetros siguientes: `apiName`, `name`, `triggerName`, `filterName`, `primaryAttribute`.

```
POST /rest/v1/activities/external/type.json
```

```json
{
  "apiName": "attendConference",
  "name": "Attend Conference",
  "description": "Attend the conference",
  "triggerName": "Attends Conference",
  "filterName": "Attended Conference",
  "primaryAttribute": {
    "apiName": "conferenceName",
    "name": "Conference Name",
    "description": "Name of the conference"
  }
}
```

```json
{
  "requestId": "e42b#14272d07d78",
  "success": true,
  "result": [
    {
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attends Conference",
      "filterName": "Attended Conference",
      "status": "draft",
      "primaryAttribute": {
        "apiName": "conferenceName",
        "name": "Conference Name",
        "description": "Name of the conference",
        "dataType": "string"
      }
    }
  ]
}
```

## Tipo de actualización

La actualización de un tipo es muy similar, excepto que apiName es el único parámetro requerido como parámetro de ruta.

```
POST /rest/v1/activities/external/type/{apiName}.json
```

```json
{
  "name": "Attend Conference",
  "description": "Attend the conference",
  "triggerName": "Attend Conference",
  "filterName": "Attended Conference",
  "primaryAttribute": {
    "apiName": "conferenceName",
    "name": "Conference Name",
    "description": "Name of the conference"
  }
}
```

```json
{
  "requestId": "e42b#14272d07d78",
  "success": true,
  "result": [
    {
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attend Conference",
      "filterName": "Attended Conference",
      "status": "draft",
      "primaryAttribute": {
        "apiName": "conferenceName",
        "name": "Conference Name",
        "description": "Name of the conference",
        "dataType": "string"
      }
    }
  ]
}
```

## Tipo de aprobación

Los tipos se pueden administrar con los tipos de actividad Aprobar actividad personalizada, Descartar tipo de actividad personalizada Borrador y Eliminar tipo de actividad personalizada, al igual que los recursos estándar de Marketo.

## Atributos de tipo de actividad personalizados

Cada tipo de actividad personalizada puede tener de 0 a 20 atributos secundarios. Los atributos secundarios pueden tener cualquier tipo de campo válido para un campo de Marketo. Se añaden, actualizan y eliminan por separado del tipo principal, pero se pueden editar mientras se utiliza un tipo de actividad y luego aprobar. Cuando los campos se editan en un tipo activo, todas las actividades de ese tipo creadas después de la aprobación tienen el nuevo conjunto de atributos secundario. Los cambios no se aplicarán de forma retroactiva a las actividades existentes que compartan ese tipo.

Tenga cuidado con la eliminación de atributos, ya que esto afectará a su disponibilidad para utilizar en los filtros correspondientes.

Las actualizaciones realizadas en la lista de atributos secundarios utilizan el nombre de API de cada atributo como clave principal. El nombre de la API para un atributo no se puede cambiar, debe eliminarse y añadirse de nuevo con el nombre de la API deseado.

Los tipos de datos válidos para los atributos son: cadena, booleano, entero, flotante, vínculo, correo electrónico, moneda, fecha, hora, teléfono, texto.

Al cambiar el atributo principal de un tipo de actividad, cualquier atributo principal existente debe disminuir de nivel estableciendo `isPrimary` en false primero.

### Crear atributos

La creación de un atributo toma un parámetro de ruta de acceso `apiName` necesario. También se requieren los parámetros `name` y `dataType`.`The description and` `isPrimary` parámetros son opcionales.

```
POST /rest/v1/activities/external/type/{apiName}/attributes/create.json
```

```json
{
  "attributes": [
    {
      "apiName": "conferenceDate",
      "name": "Conference Date",
      "description": "Date of the conference",
      "dataType": "datetime"
    },
    {
      "apiName": "numberOfAttendees",
      "name": "Number of Attendees",
      "description": "Number of people attending conference",
      "dataType": "integer"
    }
  ]
}
```

```json
{
  "requestId": "e42b#14272d07d78",
  "success": true,
  "result": [
    {
      "id": 100001,
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attend Conference",
      "filterName": "Attended Conference",
      "createdAt": "2016-02-03T22:36:23Z",
      "updatedAt": "2016-02-03T22:36:23Z",
      "status": "approved with draft",
      "primaryAttribute": {
        "apiName": "conferenceName",
        "name": "Conference Name",
        "description": "Name of the conference",
        "dataType": "string"
      },
      "attributes": [
        {
          "apiName": "conferenceDate",
          "name": "Conference Date",
          "description": "Date of the conference",
          "dataType": "datetime"
        },
        {
          "apiName": "numberOfAttendees",
          "name": "Number of Attendees",
          "description": "Number of people attending conference",
          "dataType": "integer"
        }
      ]
    }
  ]
}
```

### Actualizar atributos

Al realizar actualizaciones en los atributos, el `apiName` del atributo es la clave principal. El parámetro `apiName` debe existir para que la actualización se realice correctamente (es decir, no puede cambiar el parámetro `apiName` mediante la actualización).

```
POST /rest/v1/activities/external/type/{apiName}/attributes/update.json
```

```json
{
  "attributes": [
    {
      "apiName": "conferenceDate",
      "name": "Conference Date",
      "description": "Date of the conference",
      "dataType": "datetime"
    },
    {
      "apiName": "numberOfAttendee",
      "name": "Number of Attendee",
      "description": "Number of people attending conference",
      "dataType": "integer"
    }
  ]
}
```

```json
{
  "requestId": "e42b#14272d07d78",
  "success": true,
  "result": [
    {
      "id": 100001,
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attend Conference",
      "filterName": "Attended Conference",
      "createdAt": "2016-02-03T22:36:23Z",
      "updatedAt": "2016-02-03T22:36:23Z",
      "status": "approved with draft",
      "primaryAttribute": {
        "apiName": "conferenceName",
        "name": "Conference Name",
        "description": "Name of the conference",
        "dataType": "string"
      },
      "attributes": [
        {
          "apiName": "conferenceDate",
          "name": "Conference Date",
          "description": "Date of the conference",
          "dataType": "datetime"
        },
        {
          "apiName": "numberOfAttendee",
          "name": "Number of Attendee",
          "description": "Number of people attending conference",
          "dataType": "integer"
        }
      ]
    }
  ]
}
```

### Eliminar atributos

Al eliminar un atributo, se toma un parámetro de ruta de acceso `apiName` requerido que es el nombre de API de actividad personalizado.  También se requiere un parámetro de atributo que sea una matriz de objetos de atributo.  Cada objeto debe contener un parámetro `apiName` que sea el nombre de API del tipo de actividad personalizada.

```
POST /rest/v1/activities/external/type/{apiName}/attributes/delete.json
```

```json
{ "attributes":[ { "apiName":"conferenceDate" }, { "apiName":"numberOfAttendees" } ] }
```

```json
{
  "requestId": "e42b#14272d07d78",
  "success": true,
  "result": [
    {
      "id": 100001,
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attend Conference",
      "filterName": "Attended Conference",
      "createdAt": "2016-02-03T22:36:23Z",
      "updatedAt": "2016-02-03T22:36:23Z",
      "status": "approved with draft",
      "primaryAttribute": {
        "apiName": "conferenceName",
        "name": "Conference Name",
        "description": "Name of the conference",
        "dataType": "string"
      }
    }
  ]
}
```

## Añadir actividades personalizadas

Las actividades personalizadas son registros de una sola escritura de actividades históricas relacionadas con registros de personas individuales en Marketo. Estas actividades tienen un esquema administrado por administradores de Marketo o de forma remota a través de una integración de API. Las actividades personalizadas se agregan a los registros de posibles clientes a través del extremo [Agregar actividades personalizadas](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/addCustomActivityUsingPOST) y se relacionan con cada registro de posibles clientes a través de su campo `leadId`. Las actividades personalizadas se pueden ver en la interfaz de usuario a través del registro de actividad del posible cliente o recuperarse mediante el punto final Obtener actividades de posible cliente especificando el ID de tipo de actividad personalizada.

Las actividades personalizadas son adecuadas para registrar datos relacionados con un registro de una sola persona y que no necesitan actualizarse ni sobrescribirse. Un ejemplo sería grabar a una persona que asiste a un evento como una actividad de &quot;Asistencia al evento&quot;. Para los registros relacionados con una persona que puede cambiar, como la inscripción de estudiantes, se deben utilizar objetos personalizados en su lugar, ya que se pueden actualizar, donde las actividades personalizadas pueden no.

El miembro de entrada es una matriz de objetos de actividad. Se puede enviar un máximo de 300 registros de actividad a la vez.

Se requieren los miembros `leadId`, `activityDate`, `activityTypeId`, `primaryAttributeValue` y atributos. La matriz de atributos debe contener el atributo no principal. Esto se puede especificar con name (nombre de campo) o apiName (nombre de API) y el valor que corresponde al valor que está configurando.

```
POST /rest/v1/activities/external.json
```

```json
{
  "input": [
    {
      "leadId": 1001,
      "activityDate": "2016-09-26T06:56:35+07:00",
      "activityTypeId": 1001,
      "primaryAttributeValue": "Game Giveaway",
      "attributes": [
        {
          "apiName": "uRL",
          "value": "http://www.nvidia.com/game-giveaway"
        }
      ]
    },
    {
      "leadId": 1200,
      "activityDate": "2016-09-26T06:56:35+07:00",
      "activityTypeId": 1001,
      "primaryAttributeValue": "Game Giveaway",
      "attributes": [
        {
          "apiName": "uRL",
          "value": "http://www.nvidia.com/game-giveaway"
        }
      ]
    },
    {
      "leadId": 3000,
      "activityDate": "2016-09-26T06:56:35+07:00",
      "activityTypeId": 1001,
      "primaryAttributeValue": "Contest Form",
      "attributes": [
        {
          "apiName": "uRL",
          "value": "http://www.nvidia.com/game-giveaway"
        }
      ]
    }
  ]
}
```

```json
{
  "requestId": "e42b#14272d07d78",
  "success": true,
  "result": [
    {
      "id": 50,
      "marketoGUID": "50",
      "status": "added"
    },
    {
      "id": 51,
      "marketoGUID": "51",
      "status": "added"
    },
    {
      "status": "skipped",
      "errors": [
        {
          "code": "1004",
          "message": "Lead not found"
        }
      ]
    }
  ]
}
```

## Tiempos de espera

Los extremos de las actividades tienen un tiempo de espera de 30 segundos a menos que se indique a continuación.

* Obtener token de paginación: 300 s
* Agregar actividad personalizada: 90 s
