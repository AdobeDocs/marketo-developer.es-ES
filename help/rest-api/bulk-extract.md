---
title: Extracción en lote
feature: REST API
description: Aprenda a utilizar la API de REST de Marketo Bulk Extract para exportar posibles clientes, actividades, miembros de programa y objetos personalizados, con OAuth, colas de trabajos y límites diarios de 500 MB.
exl-id: 6a15c8a9-fd85-4c7d-9f65-8b2e2cba22ff
TQID: https://experienceleague.adobe.com/ECSchsjqp8fyxXbUGl5DgXHUkXuN0sIUc3yJfVaIe1E
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: f71e690b-4480-4b67-9ef5-88f42f9cdfdb
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1549
ht-degree: 1%

---

# Extracción en lote

Marketo Bulk Extract proporciona interfaces para recuperar grandes conjuntos de datos relacionados con personas y personas. Actualmente hay interfaces disponibles para cuatro tipos de objetos:

- Posibles clientes (personas)
- Actividades
- Miembros del programa
- Objetos personalizados

Para realizar una extracción masiva:

1. Cree un trabajo y defina los datos que desea recuperar.
1. Ponga el trabajo en cola.
1. Espere a que el trabajo termine de escribir el archivo.
1. Recupere el archivo a través de HTTP.

Los trabajos de extracción masiva se ejecutan asincrónicamente. Encueste el trabajo para recuperar el estado de exportación.

`Note:` Los extremos de API en lotes no tienen el prefijo &#39;/rest&#39; como otros extremos.

## Autenticación

Las API de extracción masiva utilizan el mismo método de autenticación OAuth 2.0 que otras API de REST de Marketo. Envíe un token de acceso válido en el encabezado HTTP `Authorization: Bearer {_AccessToken_}`.

>[!IMPORTANT]
>
>El 31 de agosto de 2026 se eliminará la compatibilidad con la autenticación mediante el parámetro de consulta **access_token**. Si el proyecto usa un parámetro de consulta para pasar el token de acceso, debe actualizarse para usar el encabezado **Autorización** lo antes posible. El nuevo desarrollo debe usar el encabezado **Authorization** exclusivamente.

## Límites

- Máximo de trabajos de exportación simultáneos: 2
- Máximo de trabajos de exportación en cola, incluidos los trabajos que se están exportando actualmente: 10
- Período de retención de archivos: siete días
- Asignación de exportación diaria predeterminada: 500 MB. La asignación se restablece diariamente a las 12:00 (hora central europea). Los incrementos están disponibles para la compra.
- Intervalo de tiempo máximo para el filtro de intervalo de fecha (`createdAt` o `updatedAt`): 31 días

Los filtros de extracción masiva de posibles clientes para UpdatedAt y Smart List no están disponibles para algunos tipos de suscripción. Si estos filtros no están disponibles, el extremo del trabajo de creación de posibles clientes devuelve el error &quot;1035, Unsupported filter type for target subscription&quot;. Póngase en contacto con el Soporte técnico de Marketo para habilitar esta funcionalidad para su suscripción.

### Cola

Las API de extracción masiva utilizan una cola de trabajos que se comparte entre posibles clientes, actividades, miembros de programa y objetos personalizados. En primer lugar, llame al punto final Crear trabajo de cliente potencial/Actividad/Miembro de programa de exportación para crear un trabajo de extracción. A continuación, llame al punto final del trabajo de cliente potencial/actividad/miembro del programa de exportación en cola correspondiente para poner en cola el trabajo. El trabajo se inicia cuando hay recursos informáticos disponibles.

La cola puede contener un máximo de 10 trabajos. Si intenta poner un trabajo en cola cuando la cola está llena, el punto final del trabajo de exportación en cola devuelve el error &quot;1029, Demasiados trabajos en cola&quot;. Un máximo de dos trabajos pueden tener el estado &quot;Procesando&quot; y ejecutarse simultáneamente.

### Tamaño de archivo

Las API de extracción masiva se miden en función del tamaño en disco de los datos que recupera un trabajo de extracción masiva. Para determinar el tamaño del archivo en bytes, lea el atributo `fileSize` en la respuesta de estado completada para un trabajo de exportación.

La cuota diaria es de 500 MB y se comparte entre posibles clientes, actividades, miembros de programa y objetos personalizados. Cuando se supera la cuota, no se puede crear ni poner en cola otro trabajo hasta que la cuota se restablezca a medianoche [Hora central](https://en.wikipedia.org/wiki/Central_Time_Zone). Hasta el restablecimiento, la API devuelve el error &quot;1029, Export daily quota expanded&quot;. Aparte de la cuota diaria, no hay un tamaño de archivo máximo.

Una vez que un trabajo se ha puesto en cola o se ha procesado, se ejecuta hasta su finalización a menos que se produzca un error o que cancele el trabajo. Si un trabajo falla, debe volver a crearlo.

La API escribe el archivo completo solo cuando el trabajo alcanza el estado completado. No escribe archivos parciales. Para comprobar el archivo, calcule su hash SHA-256 y compárelo con la suma de comprobación que devuelve el extremo de estado del trabajo.

Para determinar el espacio en disco total utilizado para el día actual, llame al punto final Obtener resultados de cliente potencial/Actividad/Trabajos de miembro del programa. Estos extremos devuelven todos los trabajos de los últimos siete días.

Filtre la lista a los trabajos que se completaron durante el día actual utilizando los atributos `status` y `finishedAt`. A continuación, agregue los tamaños de archivo para esos trabajos. No puede eliminar un archivo para recuperar espacio en disco.

## Permisos

La extracción masiva utiliza el mismo modelo de permisos que la API de REST de Marketo. No requiere permisos especiales adicionales, pero cada conjunto de extremos requiere permisos específicos.

Solo el usuario de API que ha creado un trabajo de extracción masiva puede acceder a él, sondear su estado o recuperar el contenido del archivo.

Los extremos de extracción masiva no tienen en cuenta los espacios de trabajo de Marketo. Las solicitudes de extracción incluyen datos de todos los espacios de trabajo, independientemente de cómo defina el Usuario solo de API para el servicio personalizado.

## Creación de un trabajo

Las API de extracción masiva de Marketo utilizan trabajos para iniciar y ejecutar extracciones de datos. La siguiente solicitud crea un trabajo de exportación de posibles clientes:

```http
POST /bulk/v1/leads/export/create.json
```

```json
{
   "fields": [
      "firstName",
      "lastName"
   ],
   "format": "CSV",
   "columnHeaderNames": {
      "firstName": "First Name",
      "lastName": "Last Name"
   },
   "filter": {
      "createdAt": {
         "startAt": "2023-01-01T00:00:00Z",
         "endAt": "2023-01-31T00:00:00Z"
      }
   }
}
```

Esta solicitud crea un trabajo que exporta cada posible cliente creado entre el 1 de enero de 2023 y el 31 de enero de 2023. El archivo CSV contiene valores de los campos &quot;firstName&quot; y &quot;lastName&quot;, y utiliza los encabezados de columna &quot;First Name&quot; y &quot;Last Name&quot;.

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Created",
         "createdAt": "2023-01-21T11:47:30-08:00",
         "queuedAt": "2023-01-21T11:48:30-08:00",
         "format": "CSV",
      }
   ]
}
```

La respuesta devuelve el identificador de trabajo en el atributo `exportId`. Utilice este ID de trabajo para poner en cola o cancelar el trabajo, comprobar su estado o recuperar el archivo completado.

### Parámetros comunes

Cada extremo de creación de trabajo tiene parámetros comunes para configurar el formato de archivo, los nombres de campo y el filtro. Cada subtipo del trabajo de extracción también puede tener parámetros adicionales:

| Parámetro | Tipo de datos | Notas |
| --- | --- | --- |
| formato | Cadena | Determina el formato de archivo de los datos extraídos con opciones para valores separados por comas, valores separados por tabulaciones y valores separados por punto y coma. Acepta uno de: CSV, SSV, TSV. El formato predeterminado es CSV. |
| columnHeaderNames | Objeto | Permite establecer los nombres de los encabezados de columna en el archivo devuelto. Cada clave de miembro es el nombre del encabezado de columna cuyo nombre se va a cambiar, y el valor es el nuevo nombre del encabezado de columna. Por ejemplo, &quot;columnHeaderNames&quot;: { &quot;firstName&quot;: &quot;First Name&quot;, &quot;lastName&quot;: &quot;Last Name&quot; }, |
| filter | Objeto | Filtro aplicado al trabajo de extracción. Los tipos y las opciones varían según el tipo de trabajo. |

## Recuperando trabajos

Utilice el punto final Obtener trabajos de exportación para el tipo de objeto correspondiente para recuperar trabajos recientes. Cada extremo de Obtener trabajos de exportación admite estos parámetros:

- `status` filtra los trabajos por estado de exportación. Los valores válidos son Created, Queued, Processing, Canceled, Completed y Failed.
- `batchSize` limita el número de trabajos devueltos. El valor predeterminado y máximo es 300.
- `nextPageToken` páginas a través de grandes conjuntos de resultados.

La siguiente solicitud recupera los trabajos de exportación de posibles clientes con el estado Completado o Error:

```http
GET /bulk/v1/leads/export.json?status=Completed,Failed
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Completed",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "queuedAt": "2017-01-21T11:48:30-08:00",
         "startedAt": "2017-01-21T11:51:30-08:00",
         "finishedAt": "2017-01-21T12:59:30-08:00",
         "format": "CSV",
         "numberOfRecords": 122323,
         "fileSize": 123424,
         "fileChecksum": "sha256:c16514c7e80fcac5ea055dacae9617fc3c29aff5365e3743071313ce0ed2a815"
      }
      ...
   ]
}
```

La matriz de resultados contiene la respuesta de estado de cada trabajo creado para ese tipo de objeto durante los últimos siete días. La respuesta solo incluye los trabajos que posee el usuario de API que realiza la llamada.

## Inicio de trabajos

Después de crear un trabajo, utilice su ID de trabajo para ponerlo en cola e iniciarlo:

```http
POST /bulk/v1/leads/export/{exportId}/enqueue.json
```

La solicitud inicia el trabajo y devuelve una respuesta de estado. Dado que las exportaciones se ejecutan de forma asíncrona, sondee el estado del trabajo para determinar cuándo se ha completado la exportación.

## Estado del trabajo de sondeo

Encuesta el punto final de estado para determinar el progreso de un trabajo. Solo el usuario de API que ha creado un trabajo puede sondear su estado.

Un estado de trabajo no se actualiza con más frecuencia que una vez cada 60 segundos. No sondear con más frecuencia que eso. Para la mayoría de los casos de uso, es suficiente sondear una vez cada 5 minutos. Los datos de cada exportación correcta se conservan durante 10 días.

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
         "status": "Completed",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "queuedAt": "2017-01-21T11:48:30-08:00",
         "startedAt": "2017-01-21T11:51:30-08:00",
         "finishedAt": "2017-01-21T12:59:30-08:00",
         "format": "CSV",
         "numberOfRecords": 122323,
         "fileSize": 123424,
         "fileChecksum": "sha256:d9c73f0b6960c71623c8bafe29603b3e8e20fd0e4eeaefd119c0227506ea9be4"
      }
   ]
}
```

El miembro interno `status` indica el progreso del trabajo. Su valor puede ser Creado, En cola, Procesando, Cancelado, Completado o Error.

En este ejemplo, el trabajo se ha completado, por lo que puede detener el sondeo y recuperar el archivo. Para un trabajo completado, el miembro `fileSize` indica la longitud total del archivo en bytes, y el miembro `fileChecksum` contiene el hash SHA-256 del archivo. El estado del trabajo está disponible durante 30 días después de que el trabajo alcance el estado Completado o Error.

## Recuperación de datos

Una vez finalizado el trabajo, recupere el archivo exportado:

```http
GET /bulk/v1/leads/export/{exportId}/file.json
```

La respuesta contiene el archivo en el formato configurado para el trabajo. Si el trabajo está incompleto o la solicitud contiene un ID de trabajo no válido, el punto final del archivo devuelve el estado 404 No encontrado y un mensaje de error de texto sin formato. Esta respuesta difiere de la mayoría de las demás respuestas de extremo REST de Marketo.

Para admitir la recuperación parcial y reanudable, el extremo de archivo admite el encabezado HTTP `Range` opcional con el tipo `bytes`, tal como se define en [RFC 7233](https://datatracker.ietf.org/doc/html/rfc7233). Si no establece el encabezado, el extremo devolverá todo el archivo.

Para recuperar los primeros 10 000 bytes de un archivo, pase el siguiente encabezado en la petición GET. El rango comienza en el byte 0:

```text
Range: bytes=0-9999
```

Para un archivo parcial, el punto final devuelve el código de estado 206 y los encabezados Accept-range, Content-Length y Content-Range:

```text
Accept-Ranges: bytes
Content-Length: 10000
Content-Range: bytes 0-9999/123424
```

### Recuperación y reanudación parciales

Utilice el encabezado `Range` para recuperar parte de un archivo o reanudar una recuperación. El intervalo de archivos comienza en el byte 0 y finaliza en el valor de `fileSize` menos 1. El extremo de Obtener archivo de exportación también indica la longitud del archivo como denominador en el encabezado de respuesta `Content-Range`.

Si una recuperación falla parcialmente, puede reanudarla. Por ejemplo, si intenta recuperar un archivo de 1000 bytes pero sólo recibe los primeros 725 bytes, vuelva a llamar al extremo y pase un nuevo intervalo:

```text
Range: bytes=725-999
```

Esta solicitud devuelve los 275 bytes restantes del archivo.

#### Comprobación de integridad de archivos

Cuando `status` está &quot;Completado&quot;, los extremos del estado del trabajo devuelven una suma de comprobación en el atributo `fileChecksum`. La suma de comprobación es el hash SHA-256 del archivo exportado. Compare el archivo con el hash SHA-256 del archivo recuperado para comprobar que el archivo está completo.

La siguiente respuesta contiene una suma de comprobación:

```json
{
    "exportId": "45547609-6732-418a-bb7b-17b0160b2317",
    "format": "CSV",
    "status": "Completed",
    "createdAt": "2019-06-04T23:13:12Z",
    "queuedAt": "2019-06-04T23:14:02Z",
    "startedAt": "2019-06-04T23:15:19Z",
    "finishedAt": "2019-06-04T23:36:40Z",
    "numberOfRecords": 1776,
    "fileSize": 400785,
    "fileChecksum": "sha256:83aca1351c9398d2770330e21a9e278880fd2f1eeaf8c8238bf7676d5c21d1c6"
}
```

El ejemplo siguiente utiliza la utilidad de línea de comandos sha256sum para crear el hash SHA-256 de un archivo recuperado denominado &quot;bulk_lead_export.csv&quot;:

```bash
$ sha256sum bulk_lead_export.csv
83aca1351c9398d2770330e21a9e278880fd2f1eeaf8c8238bf7676d5c21d1c6 *bulk_lead_export.csv
```

## Cancelación de un trabajo

Si un trabajo está configurado incorrectamente o ya no es necesario, cancele:

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
         "format": "CSV",
      }
   ]
}
```

El estado de respuesta indica que el trabajo se ha cancelado.
