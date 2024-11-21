---
title: Extracción en lote
feature: REST API
description: Operaciones por lotes para extraer datos de Marketo.
exl-id: 6a15c8a9-fd85-4c7d-9f65-8b2e2cba22ff
source-git-commit: e7d893a81d3ed95e34eefac1ee8f1ddd6852f5cc
workflow-type: tm+mt
source-wordcount: '1683'
ht-degree: 0%

---

# Extracción en lote

Marketo proporciona interfaces para la recuperación de grandes conjuntos de datos relacionados con personas y otras personas, llamados extracción masiva. Actualmente, se ofrecen interfaces para tres tipos de objetos:

- Posibles clientes (personas)
- Actividades
- Miembros del programa
- Objetos personalizados

La extracción masiva se realiza creando un trabajo, definiendo el conjunto de datos que se van a recuperar, poniendo el trabajo en cola, esperando a que el trabajo complete la escritura de un archivo y, a continuación, recuperando el archivo a través de HTTP. Estos trabajos se ejecutan de forma asíncrona y se pueden sondear para recuperar el estado de la exportación.

`Note:` Los extremos de API en lotes no tienen el prefijo &#39;/rest&#39; como otros extremos.

## Autenticación

Las API de extracción masiva utilizan el mismo método de autenticación OAuth 2.0 que otras API de REST de Marketo. Esto requiere que se envíe un token de acceso válido como un encabezado HTTP `Authorization: Bearer {_AccessToken_}`.

>[!IMPORTANT]
>
>El 30 de junio de 2025 se eliminará la compatibilidad con la autenticación mediante el parámetro de consulta **access_token**. Si el proyecto usa un parámetro de consulta para pasar el token de acceso, debe actualizarse para usar el encabezado **Autorización** lo antes posible. El nuevo desarrollo debe usar el encabezado **Authorization** exclusivamente.

## Límites

- Máximo de trabajos de exportación simultáneos: 2
- Máximo de trabajos de exportación en cola (incluidos los trabajos de exportación actuales): 10
- Período de retención de archivos: siete días
- Asignación de exportación diaria predeterminada: 500 MB (que se restablece diariamente a las 12:00 (CST). Aumentos disponibles para la compra.
- Intervalo de tiempo máximo para el filtro de intervalo de fechas (createdAt o updatedAt): 31 días

Los filtros de extracción masiva de posibles clientes para UpdatedAt y Smart List no están disponibles para algunos tipos de suscripción. Si no está disponible, una llamada al extremo del trabajo de creación de posibles clientes de exportación devuelve el error &quot;1035, Unsupported filter type for target subscription&quot;. Los clientes pueden ponerse en contacto con el servicio de asistencia de Marketo para habilitar esta funcionalidad en su suscripción.

### Cola

Las API de extracción masiva utilizan una cola de trabajos (compartida entre posibles clientes, actividades, miembros de programa y objetos personalizados). Primero deben crearse los trabajos de extracción y, a continuación, ponerlos en cola llamando a los extremos de los trabajos Crear posible cliente/actividad/miembro del programa de exportación y Poner en cola el cliente potencial/actividad/miembro del programa. Una vez puestos en cola, los trabajos se extraen de la cola y se inician cuando los recursos de computación están disponibles.

El número máximo de trabajos en cola es 10. Si intenta poner un trabajo en cola cuando la cola está llena, el punto final del trabajo de exportación en cola devuelve el error &quot;1029, Demasiados trabajos en cola&quot;. Se puede ejecutar un máximo de dos trabajos simultáneamente (el estado es &quot;Procesando&quot;).

### Tamaño de archivo

Las API de extracción masiva se miden en función del tamaño en disco de los datos recuperados por un trabajo de extracción masiva. El tamaño explícito en bytes de un trabajo se puede determinar leyendo el atributo `fileSize` de la respuesta de estado completada de un trabajo de exportación.

La cuota diaria es de un máximo de 500 MB por día, que se comparten entre posibles clientes, actividades, miembros de programa y objetos personalizados. Cuando se supera la cuota, no puede crear ni poner en cola otro trabajo hasta que la cuota diaria se restablezca a medianoche [hora central](https://en.wikipedia.org/wiki/Central_Time_Zone). Hasta ese momento, se devuelve el error &quot;1029, Export daily quota expanded&quot;. Aparte de la cuota diaria, no hay un tamaño de archivo máximo.

Una vez que un trabajo se pone en cola o se procesa, se ejecuta hasta su finalización (salvo error o cancelación del trabajo). Si un trabajo falla por algún motivo, debe volver a crearlo. Los archivos sólo se escriben completamente cuando un trabajo alcanza el estado completado (los archivos parciales nunca se escriben). Puede verificar que un archivo se escribió completamente calculando su hash SHA-256 y comparándolo con la suma de comprobación que devuelven los extremos de estado del trabajo.

Puede determinar la cantidad total de disco utilizado para el día actual llamando a Obtener resultados de cliente potencial/Actividad/Trabajos de miembro del programa. Estos extremos devuelven una lista de todos los trabajos de los últimos siete días. Puede filtrar esa lista para que solo se muestren los trabajos que se completaron en el día actual (con los atributos `status` y `finishedAt`). A continuación, sume los tamaños de archivo de esos trabajos para obtener la cantidad total utilizada. No hay forma de eliminar un archivo para recuperar espacio en disco.

## Permisos

La extracción masiva utiliza el mismo modelo de permisos que la API de REST de Marketo y no requiere ningún permiso especial adicional para su uso, aunque se requieren permisos específicos para cada conjunto de extremos.

Los trabajos de extracción masiva solo son accesibles para el usuario de API que los creó, incluido el sondeo del estado y la recuperación del contenido del archivo.

Los extremos de extracción masiva no tienen en cuenta los espacios de trabajo de Marketo. Las solicitudes de extracción siempre incluyen datos en todos los espacios de trabajo, independientemente de cómo defina el Usuario solo de API para el servicio personalizado.

## Creación de un trabajo

Las API de extracción masiva de Marketo utilizan el concepto de trabajo para iniciar y ejecutar la extracción de datos. Veamos la creación de un trabajo de exportación de posibles clientes simple.

```
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

Esta solicitud simple construirá un trabajo que devolverá los valores contenidos en los campos &quot;firstName&quot; y &quot;lastName&quot;, con los encabezados de columna &quot;First Name&quot; y &quot;Last Name&quot; como archivo CSV, que contienen cada posible cliente creado entre el 1 de enero de 2023 y el 31 de enero de 2023.

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

Cuando creamos el trabajo, devuelve un id. de trabajo en el atributo `exportId`. A continuación, podemos utilizar este ID de trabajo para poner el trabajo en cola, cancelarlo, comprobar su estado o recuperar el archivo completado.

### Parámetros comunes

Cada extremo de creación de trabajo comparte algunos parámetros comunes para configurar el formato de archivo, los nombres de campo y el filtro de un trabajo de extracción masiva. Cada subtipo del trabajo de extracción puede tener parámetros adicionales:

| Parámetro | Tipo de datos | Notas |
|---|---|---|
| formato | Cadena | Determina el formato de archivo de los datos extraídos con opciones para valores separados por comas, valores separados por tabulaciones y valores separados por punto y coma. Acepta uno de: CSV, SSV, TSV. El formato predeterminado es CSV. |
| columnHeaderNames | Objeto | Permite establecer los nombres de los encabezados de columna en el archivo devuelto. Cada clave de miembro es el nombre del encabezado de columna cuyo nombre se va a cambiar, y el valor es el nuevo nombre del encabezado de columna. Por ejemplo, &quot;columnHeaderNames&quot;: { &quot;firstName&quot;: &quot;First Name&quot;, &quot;lastName&quot;: &quot;Last Name&quot; }, |
| filter | Objeto | Filtro aplicado al trabajo de extracción. Los tipos y las opciones varían según el tipo de trabajo. |


## Recuperando trabajos

A veces, es posible que deba recuperar sus trabajos recientes. Esto se realiza fácilmente con Obtener trabajos de exportación para el tipo de objeto correspondiente. Cada extremo de Obtener trabajos de exportación admite un campo de filtro `status`, un  `batchSize` para limitar el número de trabajos devueltos y `nextPageToken` para paginar a través de grandes conjuntos de resultados. El filtro de estado admite cada estado válido para un trabajo de exportación: Creado, En cola, Procesando, Cancelado, Completado y Fallido. batchSize tiene un valor máximo y predeterminado de 300. Vamos a obtener la lista de trabajos de exportación de clientes potenciales:

```
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

El extremo responde con `status` respuesta de cada trabajo creado en los últimos siete días para ese tipo de objeto en la matriz de resultados. La respuesta solo incluirá resultados para trabajos propiedad del usuario de API que realiza la llamada.

## Inicio de trabajos

Con nuestro id de trabajo en la mano, vamos a comenzar el trabajo:

```
POST /bulk/v1/leads/export/{exportId}/enqueue.json
```

Esto inicia la ejecución del trabajo y devuelve una respuesta de estado. Dado que la exportación siempre se realiza de forma asíncrona, debemos sondear el estado del trabajo para determinar si se ha completado. El estado de un trabajo determinado no se actualizará con más frecuencia que una vez cada 60 segundos, por lo que el estado nunca debe sondearse con más frecuencia que eso. Sin embargo, tenga en cuenta que la mayoría de los casos de uso nunca deben requerir sondeo con más frecuencia que una vez cada 5 minutos. Los datos de cada exportación correcta se conservan durante 10 días.

## Estado del trabajo de sondeo

Determinar el estado del trabajo es sencillo.

El estado solo se puede sondear para los trabajos creados por el mismo usuario de API que los creó.

```
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

El miembro interno `status` indica el progreso del trabajo y puede ser uno de los siguientes valores: Creado, En cola, Procesando, Cancelado, Completado, Fallido. En este caso, nuestro trabajo se ha completado, por lo que podemos detener el sondeo y continuar con la recuperación del archivo. Cuando se completa, el miembro `fileSize` indica la longitud total del archivo en bytes, y el miembro `fileChecksum` contiene el hash SHA-256 del archivo. El estado del trabajo está disponible durante 30 días después de alcanzar el estado Completado o Error.

## Recuperación de datos

Una vez finalizado el trabajo, puede recuperar fácilmente el archivo.

```
GET /bulk/v1/leads/export/{exportId}/file.json
```

La respuesta contiene un archivo con el formato que tenía el trabajo configurado. El punto final responde con el contenido del archivo. Si no se ha completado un trabajo o se ha pasado un ID de trabajo incorrecto, los extremos de archivo responden con un estado de 404 No encontrado y un mensaje de error de texto sin formato como carga útil, a diferencia de la mayoría de los demás extremos REST de Marketo.

Para admitir la recuperación parcial y fácil de reanudar de los datos extraídos, el extremo de archivo admite opcionalmente el encabezado HTTP `Range` del tipo `bytes` (según [RFC 7233](https://datatracker.ietf.org/doc/html/rfc7233)). Si no se establece el encabezado, se devuelve todo el contenido. Para recuperar los primeros 10 000 bytes de un archivo, pasaría el siguiente encabezado como parte de la solicitud de GET al extremo, empezando por el byte 0:

```
Range: bytes=0-9999
```

Al recuperar el archivo parcial, el extremo responde con el código de estado 206 y devuelve los encabezados Accept-range, Content-Length y Content-Range:

```
Accept-Ranges: bytes
Content-Length: 1000
Content-Range: bytes 0-9999/123424
```

### Recuperación y reanudación parciales

Los archivos se pueden recuperar en parte o reanudar más tarde mediante el encabezado `Range`. El intervalo de un archivo comienza en el byte 0 y finaliza en el valor de `fileSize` menos 1. La longitud del archivo también se indica como denominador en el valor del encabezado de respuesta `Content-Range` al llamar a un extremo de Obtener archivo de exportación. Si una recuperación falla parcialmente, se puede reanudar más adelante. Por ejemplo, si intenta recuperar un archivo de 1000 bytes de longitud, pero solo se recibieron los primeros 725 bytes, la recuperación se puede volver a intentar desde el punto del error llamando de nuevo al extremo y pasando un nuevo intervalo:

```
Range: bytes 724-999
```

Devuelve los 275 bytes restantes del archivo.

#### Comprobación de integridad de archivos

Los extremos de estado del trabajo devuelven una suma de comprobación en el atributo `fileChecksum` cuando `status` está &quot;Completado&quot;. La suma de comprobación es un hash SHA-256 del archivo exportado. Puede comparar la suma de comprobación con el hash SHA-256 del archivo recuperado para comprobar que está completo.

Esta es una respuesta de ejemplo que contiene la suma de comprobación:

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

A continuación se muestra un ejemplo de creación del hash SHA-256 de un archivo recuperado denominado &quot;bulk_lead_export.csv&quot; mediante la utilidad de línea de comandos sha256sum:

```
$ sha256sum bulk_lead_export.csv
83aca1351c9398d2770330e21a9e278880fd2f1eeaf8c8238bf7676d5c21d1c6 *bulk_lead_export.csv
```

## Cancelación de un trabajo

Si un trabajo se ha configurado incorrectamente o se vuelve innecesario, se puede cancelar fácilmente:

```
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

Esto responde con un estado que indica que el trabajo se ha cancelado.
