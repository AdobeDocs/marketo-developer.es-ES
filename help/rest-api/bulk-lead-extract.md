---
title: Extracción de posibles clientes
feature: REST API
description: Aprenda a utilizar las API de REST de extracción masiva de posibles clientes de Marketo para exportar posibles clientes de forma masiva con filtros de fecha, lista y lista inteligente, campos personalizados y formatos CSV/TSV.
exl-id: 42796e89-5468-463e-9b67-cce7e798677b
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '1195'
ht-degree: 2%

---

# Extracción de posibles clientes

[Referencia de extremo de extracción masiva de posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads)

El conjunto de API de REST de extracción masiva de posibles clientes proporciona una interfaz programática para recuperar grandes conjuntos de registros de posibles clientes/personas de Marketo. Además, se puede utilizar para recuperar posibles clientes de forma incremental en función de la fecha creada del registro, la actualización más reciente, la pertenencia a una lista estática o la pertenencia a una lista inteligente. La interfaz recomendada para casos de uso que requieren un intercambio continuo de datos entre Marketo y uno o más sistemas externos, para fines de ETL, almacenamiento de datos y archivo.

## Permisos

Las API de extracción masiva de posibles clientes requieren que el usuario de la API propietaria tenga una función con uno o ambos permisos de solo lectura o de lectura-escritura.

## Filtros

Los posibles clientes admiten varias opciones de filtro. Algunos filtros, incluidos `updatedAt`, `smartListName` y `smartListId`, requieren componentes de infraestructura adicionales que aún no se han implementado en todas las suscripciones. Solo se puede especificar un tipo de filtro por trabajo de exportación.

| Tipo de filtro | Tipo de datos | Notas |
|---|---|---|
| createdAt | Date Range | Acepta un objeto JSON con los miembros `startAt` y `endAt`. `startAt` acepta una fecha y hora que representa la marca de agua baja y `endAt` acepta una fecha y hora que representa la marca de agua alta. El intervalo debe ser de 31 días o menos. Las horas de la fecha deben estar en formato ISO-8601, sin milisegundos. Los trabajos con este tipo de filtro devuelven todos los registros accesibles que se crearon dentro del intervalo de fechas. |
| updatedAt* | Date Range | Acepta un objeto JSON con los miembros `startAt` y `endAt`. `startAt` acepta una fecha y hora que representa la marca de agua baja y `endAt` acepta una fecha y hora que representa la marca de agua alta. El intervalo debe ser de 31 días o menos. Las horas de la fecha deben estar en formato ISO-8601, sin milisegundos. Nota: Este filtro no filtra en el campo visible &quot;updatedAt&quot;, que solo refleja las actualizaciones de los campos estándar. Filtra en función de cuándo se realizó la actualización de campo más reciente en un registro de posibles clientesLos trabajos con este tipo de filtro devuelven todos los registros accesibles que se actualizaron más recientemente dentro del intervalo de fechas. |
| staticListName | Cadena | Acepta el nombre de una lista estática. Los trabajos con este tipo de filtro devuelven todos los registros accesibles que son miembros de la lista estática en el momento en que comienza a procesarse el trabajo. Recupere nombres de listas estáticas utilizando el extremo Obtener listas. |
| staticListId | Entero | Acepta el ID de una lista estática. Los trabajos con este tipo de filtro devuelven todos los registros accesibles que son miembros de la lista estática en el momento en que comienza a procesarse el trabajo. Recupere los identificadores de lista estática mediante el extremo Obtener listas. |
| smartListName* | Cadena | Acepta el nombre de una lista inteligente. Los trabajos con este tipo de filtro devuelven todos los registros accesibles que son miembros de las listas inteligentes en el momento en que comienza a procesarse el trabajo. Recupere nombres de listas inteligentes utilizando el extremo Obtener listas inteligentes. |
| smartListId* | Entero | Acepta el identificador de una lista inteligente. Los trabajos con este tipo de filtro devuelven todos los registros accesibles que son miembros de las listas inteligentes en el momento en que comienza a procesarse el trabajo. Recupere los identificadores de las listas inteligentes mediante el extremo Obtener listas inteligentes. |

El tipo de filtro no está disponible para algunas suscripciones. Si no está disponible para la suscripción, recibirá un error al llamar al punto final de Crear trabajo de posible cliente de exportación (&quot;1035, Tipo de filtro no compatible para la suscripción de destino&quot;). Los clientes pueden ponerse en contacto con el servicio de asistencia de Marketo para habilitar esta funcionalidad en su suscripción.

## Opciones

El punto final Crear trabajo de posible cliente de exportación proporciona varias opciones de formato, lo que permite al usuario incluir campos concretos en el archivo exportado, cambiar el nombre de los encabezados de columna de estos campos y cambiar el formato del archivo exportado.

| Parámetro | Tipo de datos | Obligatorio | Notas |
|---|---|---|---|
| campos | Matriz[Cadena] | Sí | El parámetro fields acepta una matriz de cadenas JSON. Cada cadena debe ser el nombre de la API de REST de un campo de posible cliente de Marketo. Los campos enumerados se incluyen en el archivo exportado. El encabezado de columna de cada campo será el nombre de la API de REST de cada campo, a menos que se anule con columnHeader. Nota: Cuando la característica [!DNL Adobe Experience Cloud Audience Sharing] está habilitada, se produce un proceso de sincronización de cookies que asocia el ID de [!DNL Adobe Experience Cloud] (ECID) con posibles clientes de Marketo. Puede especificar el campo &quot;ecids&quot; para incluir los ECID en el archivo de exportación. |
| columnHeaderNames | Objeto | No | Objeto JSON que contiene pares de clave-valor de nombres de campo y encabezado de columna. La clave debe ser el nombre de un campo incluido en el trabajo de exportación. Este es el nombre de API del campo que se puede recuperar llamando a Describir posible cliente. El valor es el nombre del encabezado de columna exportado para ese campo. |
| formato | Cadena | No | Acepta uno de: CSV, TSV, SSV. El archivo exportado se representa como un archivo de valores separados por comas, valores separados por tabulaciones o valores separados por espacios, respectivamente, si se establece. Si no se establece, el valor predeterminado es CSV. |

## Creación de un trabajo

Los parámetros del trabajo se definen antes de iniciar la exportación con el punto de conexión [Crear trabajo de posible cliente de exportación](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/createExportLeadsUsingPOST). Debemos definir los `fields` necesarios para la exportación, el tipo de parámetros de `filter`, `format` del archivo y los nombres de encabezado de columna, si los hay.

```
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
         "`endAt`": "2017-01-31T00:00:00Z"
      }
   }
}
```

Esta solicitud comenzará a exportar un conjunto de posibles clientes creados entre el 1 de enero de 2017 y el 31 de enero de 2017, incluidos los valores de los campos `firstName`, `lastName`, `id` y `email` correspondientes.

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

Devuelve una respuesta de estado que indica que el trabajo se ha creado. Se ha definido y creado el trabajo, pero aún no se ha iniciado. Para ello, se debe llamar al extremo [Enqueue Export Lead Job](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/enqueueExportLeadsUsingPOST) mediante el exportId de la respuesta de estado de creación:

```
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

Esto responde con un `status` de &quot;En cola&quot; después del cual se establecerá en &quot;Procesando&quot; cuando haya una ranura de exportación disponible.

## Estado del trabajo de sondeo

`Note:` El estado solo se puede recuperar para trabajos creados por el mismo usuario de API.

Dado que este es un extremo asincrónico, después de crear el trabajo, debemos sondear su estado para determinar su progreso. Encuesta usando el punto de conexión [Obtener estado del trabajo del posible cliente de exportación](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/getExportLeadsStatusUsingGET). El estado solo se actualiza una vez cada 60 segundos, por lo que no se recomienda una frecuencia de sondeo inferior a esta, y en casi todos los casos sigue siendo excesiva. Echemos un vistazo rápido a las encuestas.

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
         "status": "Processing",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "queuedAt": "2017-01-21T11:48:30-08:00",
         "format": "CSV"
      }
   ]
}
```

El punto final de estado responde indicando que el trabajo aún se está procesando, por lo que el archivo aún no está disponible para su recuperación. Una vez que el estado del trabajo cambia a &quot;Completado&quot;, se prepara para la descarga.

El campo de estado puede responder con cualquiera de las siguientes opciones:

- Creado
- En cola
- Procesamiento
- Cancelado
- Completado
- Error

## Recuperación de datos

Para recuperar el archivo de una exportación de posibles clientes completada, simplemente llame al extremo [Obtener archivo de posibles clientes de exportación](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/getExportLeadsFileUsingGET) con su `exportId`.

```
GET /bulk/v1/leads/export/{exportId}/file.json
```

La respuesta contiene un archivo con el formato que tenía el trabajo configurado. El punto final responde con el contenido del archivo.

Si un campo de posible cliente solicitado está vacío (no contiene datos), `null` se coloca en el campo correspondiente del archivo de exportación. En el siguiente ejemplo, el campo de correo electrónico del posible cliente devuelto está vacío.

```csv
firstName,lastName,email,cookies
Russell,Wilson,null,_mch-localhost-1536605780000-12105
```

Para admitir la recuperación parcial y fácil de reanudar de los datos extraídos, el extremo del archivo admite opcionalmente el encabezado HTTP Range de los bytes de tipo. Si no se establece el encabezado, se devuelve todo el contenido. Obtenga más información acerca del uso del encabezado Range con Marketo [Bulk Extract](bulk-extract.md).

## Cancelación de un trabajo

Si un trabajo se configuró incorrectamente o se vuelve innecesario, se puede cancelar fácilmente usando el punto de conexión [Cancelar trabajo de posible cliente de exportación](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/cancelExportLeadsUsingPOST):

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
         "format": "CSV"
      }
   ]
}
```

Esto responde con un estado que indica que el trabajo se ha cancelado.
