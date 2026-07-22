---
title: Extracción de posibles clientes
feature: REST API
description: Aprenda a utilizar las API de REST de extracción masiva de posibles clientes de Marketo para exportar posibles clientes de forma masiva con filtros de fecha, lista y lista inteligente, campos personalizados y formatos CSV/TSV.
exl-id: 42796e89-5468-463e-9b67-cce7e798677b
TQID: https://experienceleague.adobe.com/4eMJR87fHDdccrVid3wHtspvBVQmrBGHYMlIwFCSdEI
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1037
ht-degree: 2%

---

# Extracción de posibles clientes

[Referencia de extremo de extracción masiva de posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Leads)

Las API de REST de extracción masiva de posibles clientes recuperan grandes conjuntos de registros de posibles clientes/personas de Marketo. También puede recuperar posibles clientes de forma incremental en función de la fecha de creación del registro, la actualización más reciente, la pertenencia a listas estáticas o la pertenencia a listas inteligentes.

Utilice la extracción masiva de posibles clientes para el intercambio continuo de datos entre Marketo y sistemas externos, incluidos ETL, almacenamiento de datos y flujos de trabajo de archivo.

## Permisos

El usuario de API propietario del trabajo debe tener una función con el permiso de solo lectura del posible cliente, el permiso de lectura-escritura del posible cliente o ambos permisos.

## Filtros

Los trabajos de exportación de posibles clientes admiten varios tipos de filtros. Cada trabajo de exportación solo puede utilizar un tipo de filtro.

Los filtros `updatedAt`, `smartListName` y `smartListId` requieren una infraestructura que no está disponible en todas las suscripciones.

| Tipo de filtro | Tipo de datos | Notas |
| --- | --- | --- |
| createdAt | Date Range | Un objeto JSON con `startAt` y `endAt` miembros. `startAt` es la fecha y hora de nivel bajo de agua, y `endAt` es la fecha y hora de nivel alto de agua. Utilice valores de fecha y hora ISO-8601 sin milisegundos. El intervalo debe ser de 31 días o menos. El trabajo devuelve todos los registros accesibles creados dentro del intervalo de fechas. |
| updatedAt* | Date Range | Un objeto JSON con `startAt` y `endAt` miembros. `startAt` es la fecha y hora de nivel bajo de agua, y `endAt` es la fecha y hora de nivel alto de agua. Utilice valores de fecha y hora ISO-8601 sin milisegundos. El intervalo debe ser de 31 días o menos. Este filtro no utiliza el campo `updatedAt` visible, que refleja solamente las actualizaciones de los campos estándar. En su lugar, utiliza la hora de la última actualización del campo en un registro de posibles clientes. El trabajo devuelve todos los registros accesibles actualizados más recientemente dentro del intervalo de fechas. |
| staticListName | Cadena | Nombre de una lista estática. El trabajo devuelve todos los registros accesibles que son miembros de la lista estática cuando el trabajo comienza a procesarse. Recupere nombres de listas estáticas utilizando el extremo Obtener listas. |
| staticListId | Entero | El ID de una lista estática. El trabajo devuelve todos los registros accesibles que son miembros de la lista estática cuando el trabajo comienza a procesarse. Recupere los ID de lista estática mediante el extremo Obtener listas. |
| smartListName* | Cadena | Nombre de una lista inteligente. El trabajo devuelve todos los registros accesibles que son miembros de la lista inteligente cuando el trabajo comienza a procesarse. Recupere nombres de listas inteligentes utilizando el extremo Obtener listas inteligentes. |
| smartListId* | Entero | El ID de una lista inteligente. El trabajo devuelve todos los registros accesibles que son miembros de la lista inteligente cuando el trabajo comienza a procesarse. Recupere los ID de listas inteligentes mediante el extremo Obtener listas inteligentes. |

Los tipos de filtro marcados con un asterisco no están disponibles para algunas suscripciones. Si un tipo de filtro no está disponible para la suscripción, el punto final Crear trabajo de posible cliente de exportación devuelve el error &quot;1035, Tipo de filtro no compatible para la suscripción de destino&quot;. Póngase en contacto con el Soporte técnico de Marketo para habilitar esta funcionalidad para su suscripción.

## Opciones

El punto final Crear trabajo de posible cliente de exportación proporciona opciones para seleccionar campos exportados, cambiar el nombre de los encabezados de columna y establecer el formato de archivo.

| Parámetro | Tipo de datos | Obligatorio | Notas |
| --- | --- | --- | --- |
| campos | Matriz[Cadena] | Sí | Una matriz de cadenas JSON. Cada cadena debe ser el nombre de la API de REST de un campo de posible cliente de Marketo. La exportación incluye cada campo de la lista y utiliza su nombre de API de REST como encabezado de columna a menos que `columnHeaderNames` lo anule. Cuando la característica [!DNL Adobe Experience Cloud Audience Sharing] está habilitada, un proceso de sincronización de cookies asocia el ID de [!DNL Adobe Experience Cloud] (ECID) con posibles clientes de Marketo. Especifique el campo `ecids` para incluir los ECID en el archivo de exportación. |
| columnHeaderNames | Objeto | No | Un objeto JSON de pares de clave-valor de campo y encabezado de columna. Cada clave debe ser el nombre de API de un campo incluido en el trabajo de exportación. Recupere el nombre de la API llamando a Describir posible cliente. Cada valor es el encabezado de columna exportado para ese campo. |
| formato | Cadena | No | El formato del archivo de exportación: CSV para valores separados por comas, TSV para valores separados por tabulaciones o SSV para valores separados por espacios. El valor predeterminado es CSV. |

## Creación de un trabajo

Use el extremo [Crear trabajo de posible cliente de exportación](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Leads/operation/createExportLeadsUsingPOST) para definir un trabajo de exportación. Especifique `fields` para exportar, un tipo `filter` y sus parámetros, el archivo `format` y cualquier nombre de encabezado de columna personalizado.

```http
POST /bulk/v1/leads/export/create.json
```

```json
{
   "fields": [
      "firstName",
      "lastName",
      "id",
      "email"
   ],
   "format": "CSV",
   "columnHeaderNames": {
      "firstName": "First Name",
      "lastName": "Last Name",
      "id": "Marketo Id",
      "email": "Email Address"
   },
   "filter": {
      "createdAt": {
         "startAt": "2017-01-01T00:00:00Z",
         "endAt": "2017-01-31T00:00:00Z"
      }
   }
}
```

Esta solicitud crea un trabajo de exportación para los posibles clientes creados entre el 1 de enero de 2017 y el 31 de enero de 2017. La exportación incluye valores de los campos `firstName`, `lastName`, `id` y `email`.

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Created",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "queuedAt": "2017-01-21T11:48:30-08:00",
         "format": "CSV"
      }
   ]
}
```

La respuesta confirma que el trabajo se ha creado pero no se ha iniciado. Para iniciar el trabajo, llame al extremo [Trabajo de cliente potencial de exportación en cola](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Leads/operation/enqueueExportLeadsUsingPOST) con `exportId` desde la respuesta de creación.

```http
POST /bulk/v1/leads/export/{exportId}/enqueue.json
```

```json
{
    "requestId": "147e4#16b24d9b913",
    "result": [
        {
            "exportId": "fad2cd1b-e822-4025-be1e-9caa9cf1d4b8",
            "format": "CSV",
            "status": "Queued",
            "createdAt": "2019-06-04T23:35:43Z",
            "queuedAt": "2019-06-04T23:36:17Z"
        }
    ],
    "success": true
}
```

La respuesta en cola tiene un `status` de &quot;En cola&quot;. Cuando hay una ranura de exportación disponible, el estado cambia a &quot;Procesando&quot;.

## Estado del trabajo de sondeo

Solo puede recuperar el estado de los trabajos creados por el mismo usuario de API.

Los trabajos de exportación de posibles clientes se ejecutan asincrónicamente. Encuesta el extremo [Obtener estado del trabajo del posible cliente de exportación](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Leads/operation/getExportLeadsStatusUsingGET) para rastrear el progreso del trabajo.

El estado se actualiza solo una vez cada 60 segundos. No sondee con más frecuencia; en casi todos los casos, ese intervalo sigue siendo excesivo.

```http
GET /bulk/v1/leads/export/{exportId}/status.json
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Processing",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "queuedAt": "2017-01-21T11:48:30-08:00",
         "format": "CSV"
      }
   ]
}
```

Esta respuesta muestra que el trabajo aún se está procesando, por lo que el archivo no está disponible. Cuando el estado del trabajo cambie a &quot;Completado&quot;, el archivo estará listo para descargarse.

El campo `status` puede devolver cualquiera de los siguientes valores:

- Creado
- En cola
- Procesamiento
- Cancelado
- Completado
- Error

## Recuperación de datos

Para recuperar una exportación de posibles clientes completada, llame al extremo [Obtener archivo de posibles clientes de exportación](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Leads/operation/getExportLeadsFileUsingGET) con `exportId`.

```http
GET /bulk/v1/leads/export/{exportId}/file.json
```

El cuerpo de respuesta contiene el archivo en el formato configurado para el trabajo.

Si un campo de posible cliente solicitado no contiene datos, el campo correspondiente del archivo de exportación contiene `null`. En el siguiente ejemplo, el posible cliente devuelto tiene un campo de correo electrónico vacío.

```csv
firstName,lastName,email,cookies
Russell,Wilson,null,_mch-localhost-1536605780000-12105
```

Para la recuperación parcial o reanudable, el extremo de archivo admite el encabezado HTTP `Range` opcional con el tipo `bytes`. Si no establece el encabezado, el extremo devolverá todo el contenido. Obtenga más información sobre cómo usar el encabezado `Range` con Marketo [Bulk Extract](bulk-extract.md).

## Cancelación de un trabajo

Para cancelar un trabajo innecesario o configurado incorrectamente, llame al extremo [Cancelar trabajo de posible cliente de exportación](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Leads/operation/cancelExportLeadsUsingPOST).

```http
POST /bulk/v1/leads/export/{exportId}/cancel.json
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Cancelled",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "format": "CSV"
      }
   ]
}
```

La respuesta confirma que el trabajo se ha cancelado.
