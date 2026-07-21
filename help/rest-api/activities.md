---
title: Actividades
feature: REST API
description: Utilice la API de REST de actividades de Marketo Engage para enumerar tipos de actividades, recuperar actividades de posible cliente con tokens de paginación y gestionar cambios personalizados y de valor de datos.
exl-id: 1e69af23-2b0c-467a-897c-1dcf81343e73
TQID: https://experienceleague.adobe.com/62keaj4uNoxIPCzr9AQzKrIsfuHBvC25knYisZRUvF4
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
source-wordcount: 1758
ht-degree: 0%

---

# Actividades

Marketo admite muchos tipos de actividades relacionadas con registros de posibles clientes. Casi todos los cambios, acciones o pasos de flujo se registran en el registro de actividades de un posible cliente. Puede recuperar estas actividades a través de la API o utilizarlas en déclencheur y filtros de listas inteligentes y campañas inteligentes.

Cada actividad tiene un(a) `id` único(a) y se conecta a un registro de posibles clientes a través de `leadId`, que corresponde al campo de ID del registro. Cada actividad también tiene un `activityDate`.

Los tipos de actividades disponibles varían según la suscripción y cada tipo tiene su propia definición. El significado de `primaryAttributeValueId` y `primaryAttributeValue` depende del tipo de actividad.

Utilice la API de metadatos de actividades personalizadas para crear tipos de actividades personalizados. Utilice la API Agregar actividades personalizadas para agregar registros de actividad personalizados.

La mayoría de las actividades se purgarán después de algún período de tiempo.

## Describir

Use el extremo [Obtener tipos de actividades](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getAllActivityTypesUsingGET) para recuperar los tipos de actividades disponibles y sus definiciones para una instancia.

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

Las respuestas reales incluyen más definiciones. Este ejemplo muestra el tipo de actividad &quot;Rellenar formulario&quot;. Su atributo principal, &quot;ID de formulario web&quot;, hace referencia al ID de Marketo del formulario enviado y vincula la actividad a ese recurso.

La respuesta también define cada atributo posible para el tipo de actividad y su tipo de datos. Si un campo está vacío, ese atributo se omite en el registro de actividad individual.

## Consulta

Use el extremo [Obtener actividades de posible cliente](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getLeadActivitiesUsingGET) para recuperar actividades. En primer lugar, recupere un token de paginación para la fecha y hora en la que debe comenzar la recuperación de la actividad. Pase ese token en el parámetro de consulta `nextPageToken`.

Pase hasta diez ID de tipo de actividad como una lista separada por comas en el parámetro de consulta `activityTypeIds`.

De forma opcional, limite la consulta con uno de estos parámetros:

- `listId` limita los resultados a los registros de una lista estática específica.
- `leadIds` limita los resultados a las actividades hasta para 30 posibles clientes, que se proporcionan como una lista separada por comas.

>[!CAUTION]
>
>A partir del 30 de diciembre de 2026, las llamadas a los extremos `Get Lead Activities` y `Get Lead Changes`, que incluye el parámetro `listId`, producirán un error (código de error 1003) si las listas de destino contienen 10 000 posibles clientes o más. Para evitar interrupciones en el servicio, asegúrese de que las llamadas de se dirijan correctamente a fin de evitar este límite. Consulte la [guía de migración](migration.md).

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

Para la primera llamada, use la API Obtener token de paginación para obtener `nextPageToken`. Para cada llamada subsiguiente, pase el `nextPageToken` devuelto por la respuesta anterior. Este extremo siempre devuelve `nextPageToken`.

Si `moreResult` es verdadero, hay más resultados disponibles. Continúe llamando al extremo con el `nextPageToken` devuelto hasta que `moreResult` sea falso.

La API puede devolver menos de 300 elementos de actividad al establecer `moreResult` en verdadero. En este caso, incluya el elemento `nextPageToken` devuelto en otra llamada para recuperar actividades más recientes.

Dentro de cada elemento de matriz de resultados, el atributo de cadena `marketoGUID` reemplaza el atributo entero `id` como identificador único.

### Cambios en el valor de los datos

Use el extremo [Obtener cambios de posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getLeadChangesUsingGET) para recuperar los registros de cambio de valor de datos para los campos de posibles clientes. Su interfaz difiere de la API Obtener actividades principales de dos maneras:

- El extremo no tiene parámetro `activityTypeIds` porque solo devuelve las actividades Cambio de valor de datos y Nuevo posible cliente.
- El parámetro de consulta obligatorio `fields` acepta una lista de campos separados por comas cuyos cambios desea recuperar.

>[!CAUTION]
>
>A partir del 30 de diciembre de 2026, las llamadas a los extremos `Get Lead Activities` y `Get Lead Changes`, que incluye el parámetro `listId`, producirán un error (código de error 1003) si las listas de destino contienen 10 000 posibles clientes o más. Para evitar interrupciones en el servicio, asegúrese de que las llamadas de se dirijan correctamente a fin de evitar este límite. Consulte la [guía de migración](migration.md).

```http
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

Cada actividad de la respuesta tiene una matriz de campos que enumera sus cambios. Cada cambio especifica los valores `id` y `name` del campo, junto con los valores nuevos y antiguos.

Dentro de cada elemento de matriz de resultados, el atributo de cadena `marketoGUID` reemplaza el atributo entero `id` como identificador único.

### Posibles clientes eliminados

Use el extremo [Obtener posibles clientes eliminados](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getDeletedLeadsUsingGET) para recuperar las actividades de posibles clientes eliminadas de Marketo.

```http
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

Dentro de cada elemento de matriz de resultados, el atributo de cadena `marketoGUID` reemplaza el atributo entero `id` como identificador único.

### Página a través de resultados

De forma predeterminada, los extremos de esta sección devuelven 300 elementos de actividad a la vez. Si `moreResult` es verdadero, hay más resultados disponibles. Pase el valor devuelto `nextPageToken` en cada llamada subsiguiente hasta que `moreResult` sea falso.

Un extremo puede devolver menos de 300 elementos de actividad al establecer `moreResult` como verdadero. En este caso, incluya el elemento `nextPageToken` devuelto en otra llamada para recuperar actividades más recientes. Codificación de URL `nextPageToken` en la solicitud.

## Tipos de actividades personalizadas

Las actividades personalizadas funcionan como actividades estándar, pero los terceros administran sus esquemas. Los registros de actividad personalizados vinculan los registros de posibles clientes a través de `leadId`, y sus atributos primarios y secundarios están definidos por el usuario.

Cuando se aprueba un tipo de actividad personalizada, Marketo crea un déclencheur de listas inteligentes y un filtro correspondientes. A continuación, puede procesar los posibles clientes en función de los datos de actividad personalizados actuales o históricos.

- Número máximo de actividades personalizadas: 10
- Atributos máximos por actividad personalizada: 20

Recupere datos de actividad personalizados a través de la API [Obtener actividades principales](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getLeadActivitiesUsingGET), de la misma manera que recupera actividades estándar.

## Tipos de consulta

Use [Obtener tipos de actividades personalizados](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getCustomActivityTypeUsingGET) para recuperar detalles sobre los tipos aprovisionados en una instancia de Marketo. Use [Describir tipo de actividad personalizada](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/describeCustomActivityTypeUsingGET) para recuperar los metadatos de atributo de un tipo específico.

El extremo estándar [Obtener tipos de actividad](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getAllActivityTypesUsingGET) también devuelve metadatos de actividad personalizados, pero no identifica si un tipo es personalizado.

### Obtener tipos

```http
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

Para describir un tipo, pase `apiName` como parámetro de ruta de acceso. De forma predeterminada, el extremo devuelve la versión aprobada de la actividad. Para recuperar la versión de borrador, pase el parámetro `draft=true` opcional.

```http
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

Cada tipo de actividad personalizada requiere un nombre para mostrar, un nombre de API, un nombre de déclencheur, un nombre de filtro y un atributo principal. Siga estas directrices para mantener los tipos coherentes con las convenciones de Marketo y evitar conflictos de nombres:

- **Nombre para mostrar:** Describa brevemente lo que representa un registro de actividad, como &quot;Enviar correo electrónico&quot; o &quot;Cambiar valor de datos&quot;. Utilice un formulario infinito, como &quot;Asistir a un evento&quot;. Los nombres para mostrar aceptan caracteres alfanuméricos, espacios y guiones bajos, y deben contener al menos una letra.

- **Nombre de API:** Use caracteres alfanuméricos, con una longitud máxima de 255. Si es socio de LaunchPoint, anteponga un área de nombres representativa a los nombres de API de tipo de actividad para evitar conflictos con los tipos proporcionados por el cliente. Utilice minúsculas o camelCase para distinguir los nombres de API de otras cadenas.

- **Descripción:** Para las actividades con un comportamiento no obvio, explique lo que representa el tipo de actividad en relación con el posible cliente.

- **Nombre del Déclencheur:** Proporcione un nombre único, legible en lenguaje natural, en el tiempo presente de tercera persona, como &quot;Asiste a un evento&quot;. Los socios de LaunchPoint deben incluir el nombre de su empresa, como &quot;Asiste al seminario web - Empresa Acme&quot;.

- **Nombre del filtro:** Proporcione un nombre único, legible en lenguaje natural, en el pasado, en tercera persona, como &quot;Ha asistido a un evento&quot;. Los socios de LaunchPoint deben incluir el nombre de su empresa, como &quot;Seminario web al que asistió: empresa Acme&quot;.

- **Atributo principal:** Seleccione el campo más significativo para el tipo de actividad. Para una actividad &quot;Evento al que ha asistido&quot;, este campo es el nombre del evento. El atributo principal aparece de forma predeterminada como un parámetro en cada déclencheur o filtro para el tipo de actividad. Su valor también aparece en el registro de actividad de una persona sin que sea necesario profundizar en la actividad.

Se crea un nuevo tipo de actividad personalizada como borrador. Apruebe el tipo antes de agregar registros de actividad de ese tipo. Las actualizaciones se aplican a la versión de borrador y deben aprobarse antes de que aparezcan en la versión activa. Una vez aprobado y en uso un tipo de actividad personalizada, no se pueden cambiar los campos anteriores.

Al crear un tipo, el parámetro de descripción es opcional. Los parámetros requeridos son `apiName`, `name`, `triggerName`, `filterName` y `primaryAttribute`.

```http
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

Para actualizar un tipo, pase el apiName requerido como parámetro de ruta. Se pueden proporcionar otros campos en el cuerpo de la solicitud.

```http
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

Administre tipos con Aprobar tipo de actividad personalizada, Descartar tipo de actividad personalizada Borrador y Eliminar tipo de actividad personalizada, como lo haría con los recursos estándar de Marketo.

## Atributos de tipo de actividad personalizados

Cada tipo de actividad personalizada puede tener de 0 a 20 atributos secundarios. Un atributo secundario puede utilizar cualquier tipo de campo Marketo válido. Agregar, actualizar y quitar atributos secundarios por separado del tipo principal.

Puede editar atributos mientras un tipo de actividad está en uso y luego aprobar los cambios. Las actividades creadas después de la aprobación utilizan el nuevo conjunto de atributos secundarios. Los cambios no se aplican de forma retroactiva a las actividades existentes de ese tipo.

Al eliminar atributos, también se elimina su disponibilidad en los filtros correspondientes.

Las actualizaciones de la lista de atributos secundarios utilizan el nombre de API de cada atributo como clave principal. Para cambiar un Nombre de API, elimine el atributo y agréguelo de nuevo con el nombre de API deseado.

Los tipos de datos válidos para los atributos son: cadena, booleano, entero, flotante, vínculo, correo electrónico, moneda, fecha, hora, teléfono, texto.

Antes de cambiar el atributo principal de un tipo de actividad, devuelva el atributo principal existente estableciendo `isPrimary` en false.

### Crear atributos

Para crear un atributo, pase el parámetro de ruta de acceso necesario `apiName`. Los parámetros `name` y `dataType` también son obligatorios. La descripción y los parámetros `isPrimary` son opcionales.

```http
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

Al actualizar atributos, el atributo `apiName` es la clave principal y ya debe existir. No puede cambiar `apiName` con una actualización.

```http
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

Para eliminar un atributo, pase el parámetro de ruta de acceso necesario `apiName` para la actividad personalizada. Pase también el parámetro de atributo requerido como una matriz de objetos de atributo. Cada objeto debe contener un parámetro `apiName` para el tipo de actividad personalizada.

```http
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

Las actividades personalizadas son registros de una sola escritura de actividades históricas para registros de personas individuales. Los administradores de Marketo pueden administrar su esquema en Marketo o una integración de API puede administrarlo de forma remota.

Use el extremo [Agregar actividades personalizadas](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/addCustomActivityUsingPOST) para agregar actividades personalizadas a los registros de posibles clientes. El campo `leadId` asocia cada actividad con un posible cliente. Vea las actividades personalizadas en el registro de actividad del posible cliente o recuperarlas mediante Obtener actividades de posible cliente especificando el ID de tipo de actividad personalizada.

Utilice actividades personalizadas para datos relacionados con una persona que no necesiten actualizarse ni sobrescribirse. Por ejemplo, registre la asistencia al evento como una actividad &quot;Asistencia al evento&quot;.

Utilice objetos personalizados para registros relacionados con personas que puedan cambiar, como la inscripción de estudiantes. Los objetos personalizados se pueden actualizar, pero las actividades personalizadas no.

El miembro de entrada es una matriz de objetos de actividad. Puede enviar un máximo de 300 registros de actividad a la vez.

Se requieren los miembros `leadId`, `activityDate`, `activityTypeId`, `primaryAttributeValue` y atributos. La matriz de atributos debe contener el atributo no principal. Especifíquelo con name (nombre de campo) o apiName (nombre de API) y value para el valor que desea establecer.

```http
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

Los extremos de las actividades tienen un tiempo de espera de 30 segundos, excepto para los siguientes extremos:

- Obtener token de paginación: 300 s
- Agregar actividad personalizada: 90 s
