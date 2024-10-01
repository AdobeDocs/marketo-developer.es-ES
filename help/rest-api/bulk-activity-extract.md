---
title: Extracción de actividades en lotes
feature: REST API
description: Datos de actividad de procesamiento por lotes de Marketo.
exl-id: 6bdfa78e-bc5b-4eea-bcb0-e26e36cf6e19
source-git-commit: 8c22255673fee1aa0f5b47393a241fcf6680778b
workflow-type: tm+mt
source-wordcount: '1343'
ht-degree: 7%

---

# Extracción de actividades en lotes

[Referencia de extremo de extracción masiva de actividades](https://developer.adobe.com/marketo-apis/api/mapi/)

El conjunto de API de REST de extracción masiva de actividades proporciona una interfaz de programación para recuperar grandes cantidades de datos de actividad de Marketo.  Para casos que no requieren baja latencia y deben transferir volúmenes significativos de datos de actividad fuera de Marketo, como integración de CRM, ETL, almacenamiento de datos y archivado de datos.

## Permisos

Las API de extracción masiva requieren que el usuario de la API tenga los permisos &quot;Actividad de solo lectura&quot; o &quot;Actividad de lectura y escritura&quot;.

## Filtros

| Tipo de filtro | Tipo de datos | Requerido | Notas |
| --- | --- | --- | --- |
| createdAt | Date Range | Sí | Acepta un objeto JSON con los miembros `startAt` y `endAt`. `startAt` acepta una fecha y hora que representa la marca de agua baja y `endAt` acepta una fecha y hora que representa la marca de agua alta. El intervalo debe ser de 31 días o menos. Los trabajos con este tipo de filtro devuelven todos los registros accesibles que se crearon dentro del intervalo de fechas. Las horas de la fecha deben estar en formato ISO-8601, sin milisegundos. |
| activityTypeIds | Matriz\[Entero\] | No | Acepta un objeto JSON con un miembro, `activityTypeIds`. El valor debe ser una matriz de enteros, correspondientes a los tipos de actividad deseados. No se admite la actividad &quot;Eliminar posible cliente&quot; (use el extremo [Obtener posibles clientes eliminados](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getDeletedLeadsUsingGET) en su lugar). Recupere los identificadores de tipo de actividad usando el extremo [Obtener tipos de actividad](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getAllActivityTypesUsingGET). |
| [primaryAttributeValueIds](#primaryattributevalueids-options) | Matriz\[Entero\] | No | Acepta un objeto JSON con un miembro, `primaryAttributeValueIds`. El valor es una matriz de ID que especifica los atributos principales por los que filtrar. Se puede especificar un máximo de 50 ID. Los ID son el identificador único de un campo de posible cliente o de un recurso y se pueden recuperar llamando al punto final de la API de REST adecuado. Por ejemplo, para filtrar un formulario específico para la actividad &quot;Rellenar formulario&quot;, pase el nombre del formulario al extremo [Obtener formulario por nombre](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET) para recuperar el ID del formulario. A continuación se muestra una lista de tipos de actividades donde se admite el filtrado de atributos principales. |
| [primaryAttributeValues](#primaryattributevalues-options) | Matriz\[Cadena\] | No | Acepta un objeto JSON con un miembro, `primaryAttributeValues`. El valor es una matriz de nombres que especifica los atributos principales por los que filtrar. Se puede especificar un máximo de 50 nombres. Los nombres son el identificador único de un campo de posible cliente o de un recurso y se pueden recuperar llamando al punto final de la API de REST adecuado. Por ejemplo, para filtrar un formulario específico para la actividad &quot;Rellenar formulario&quot;, pase el ID del formulario a [Obtener formulario por ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) para recuperar el nombre del formulario. A continuación se muestra una lista de tipos de actividades donde se admite el filtrado de atributos principales. |

### primaryAttributeValueIds, opciones {#primaryattributevalueids-options}

| Tipo de actividad | Identificador de valor de atributo principal | Extremo de recuperación | Grupo de recursos |
| --- | --- | --- | --- |
| Cambiar valor de datos | ID de campo de posible cliente | [Describir posible cliente](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) | Nombre del atributo |
| Cambiar calificación | ID de campo de posible cliente | [Describir posible cliente](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) | Nombre del atributo |
| Cambio de estado en progreso | ID de programa | [Obtener programa por nombre](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByNameUsingGET) | Programa de marketing |
| Agregar a Lista | ID de lista estática | [Obtener lista estática por nombre](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByNameUsingGET) | Lista estática |
| Quitar de Lista | ID de lista estática | [Obtener lista estática por nombre](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByNameUsingGET) | Lista estática |
| Completar formulario | ID de formulario | [Obtener formulario por nombre](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET) | Formulario web |

Al usar `primaryAttributeValueIds`, el filtro `activityTypeIds` debe estar presente y solo contener los identificadores de actividad que coincidan con el grupo de recursos correspondiente. Por ejemplo, si está filtrando recursos de formularios web, solo se permite el ID de tipo de actividad &quot;Rellenar formulario&quot; en `activityTypeIds`.

Ejemplo de cuerpo de la solicitud:

```json
{
  "filter": {
    "createdAt": {
      "startAt": "2021-07-01T23:59:59-00:00",
      "endAt": "2021-07-02T23:59:59-00:00"
    },
    "activityTypeIds": [
      2
    ],
    "primaryAttributeValueIds": [
      16,102,95,8
    ]
  }
}
```

`primaryAttributeValueIds` y `primaryAttributeValues` no se pueden usar juntos.

### primaryAttributeValues, opciones {#primaryattributevalues-options}

| Tipo de actividad | Valor de atributo principal | Extremo de recuperación | Grupo de recursos |
| --- | --- | --- | --- |
| Cambiar valor de datos | DisplayName del campo de posible cliente | [Describir posible cliente](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) | Nombre del atributo |
| Cambiar calificación | DisplayName del campo de posible cliente | [Describir posible cliente](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) | Nombre del atributo |
| Cambio de estado en progreso | Nombre del programa | [Obtener programa por identificador](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByIdUsingGET) | Programa de marketing |
| Agregar a Lista | Nombre de lista estática | [Obtener lista estática por identificador](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET) | Lista estática |
| Quitar de Lista | Nombre de lista estática | [Obtener lista estática por identificador](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET) | Lista estática |
| Completar formulario | Nombre del formulario | [Obtener formulario por identificador](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) | Formulario web |

Tenga en cuenta que debe utilizar &quot;&lt;<em>program</em>>.Notación &lt;<em>asset</em>>&quot; para especificar el nombre de los siguientes grupos de recursos: programa de marketing, lista estática, formulario web. Por ejemplo, un formulario con el nombre &quot;MPS saliente&quot; que reside debajo de un programa con el nombre &quot;GL_OP_ALL_2021&quot; se especificaría como &quot;GL_OP_ALL_2021.MPS saliente&quot;.

Ejemplo de cuerpo de la solicitud:

```json
{
  "filter": {
    "createdAt": {
      "startAt": "2021-07-01T23:59:59-00:00",
      "endAt": "2021-07-02T23:59:59-00:00"
    },
    "activityTypeIds": [
      2
    ],
    "primaryAttributeValues": [
      "GL_OP_ALL_2021.MPS Outbound"
    ]
  }
}
```

Al usar `primaryAttributeValues`, el filtro `activityTypeIds` debe estar presente y solo contener los identificadores de actividad que coincidan con el grupo de recursos correspondiente. Por ejemplo, si está filtrando recursos de formularios web, solo se permite el ID de tipo de actividad &quot;Rellenar formulario&quot; en `activityTypeIds`. `primaryAttributeValues` y `primaryAttributeValueIds` no se pueden usar juntos.

## Opciones

| Parámetro | Tipo de datos | Requerido | Notas |
|---|---|---|---|
| filter | Matriz[Objeto] | Sí | Acepta una matriz de filtros. Se debe incluir exactamente un filtro `createdAt` en la matriz. Se puede incluir un filtro `activityTypeIds` opcional. Los filtros se aplican al conjunto de actividades accesibles y el conjunto de actividades resultante se devuelve mediante el trabajo de exportación. |
| formato | Cadena | No | Acepta uno de: CSV, TSV, SSV El archivo exportado se representa como un archivo de valores separados por comas, valores separados por tabulaciones o valores separados por espacios, respectivamente, si se establece. Si no se establece, el valor predeterminado es CSV. |
| columnHeaderNames | Objeto | No | Objeto JSON que contiene pares de clave-valor de nombres de campo y encabezado de columna. La clave debe ser el nombre de un campo incluido en el trabajo de exportación. El valor es el nombre del encabezado de columna exportado para ese campo. |
| campos | Matriz[Cadena] | No | Matriz opcional de cadenas que contienen valores de campo. Los campos enumerados se incluyen en el archivo exportado. De forma predeterminada, se devuelven los campos siguientes: `marketoGUIDleadId` `activityDate` `activityTypeId` `campaignId` `primaryAttributeValueId` `primaryAttributeValueattributes`. Este parámetro se puede utilizar para reducir el número de campos que se devuelven especificando un subconjunto de la lista anterior. Ejemplo:&quot;fields&quot;: [&quot;leadId&quot;, &quot;activityDate&quot;, &quot;activityTypeId&quot;]Se puede especificar un campo adicional &quot;actionResult&quot; para incluir la acción de actividad (&quot;succeeded&quot;, &quot;skipped&quot; o &quot;failed&quot;). |


## Creación de un trabajo

Para exportar registros, primero debe definir el trabajo y el conjunto de registros que desea recuperar.  Cree el trabajo con el extremo [Crear trabajo de actividad de exportación](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/createExportActivitiesUsingPOST).  Al exportar actividades, se pueden aplicar dos filtros principales: `createdAt`, que siempre es obligatorio, y `activityTypeIds`, que es opcional.  El filtro `createdAt` se usa para definir un intervalo de fechas en el que se crearon las actividades, utilizando los parámetros `startAt` y `endAt`, los cuales son campos de fecha y hora, y representan la fecha de creación permitida más temprana y la fecha de creación permitida más reciente, respectivamente.  Opcionalmente, también puede filtrar solo ciertos tipos de actividades, usando el filtro `activityTypeIds`.  Esto resulta útil para eliminar resultados que no son relevantes para su caso de uso.

```
POST /bulk/v1/activities/export/create.json
```

```json
{ 
   "format": "CSV",
   "filter": { 
      "createdAt": { 
         "startAt": "2017-07-01T23:59:59-00:00",
         "endAt": "2017-07-31T23:59:59-00:00"
      },
      "activityTypeIds": [ 
         1,
         12,
         13
      ]
   }
}
```

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

El trabajo ahora tiene el estado &quot;Creado&quot;, pero aún no está en la cola de procesamiento.  Para ponerlo en cola para que pueda comenzar a procesarse, llame al extremo [Enqueue Export Activity Job](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/enqueueExportActivitiesUsingPOST) mediante exportId desde la respuesta de estado de creación.

```
POST /bulk/v1/activities/export/{exportId}/enqueue.json
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Queued",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "queuedAt": "2017-01-21T11:48:30-08:00",
         "format": "CSV"
      }
   ]
}
```

Ahora, el estado indica que el trabajo se ha puesto en cola.  Cuando un trabajador está disponible para este trabajo, el estado cambia a &quot;Procesando&quot; y el trabajo comienza a agregar registros desde Marketo.

## Estado del trabajo de sondeo

El estado del trabajo solo se puede recuperar para trabajos creados por el mismo usuario de API.

La extracción de actividades por lotes de Marketo es un extremo asincrónico, por lo que el estado del trabajo debe sondearse para determinar cuándo se ha completado el trabajo.  Encuesta usando el punto de conexión [Obtener estado del trabajo de actividad de exportación](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/getExportActivitiesStatusUsingGET) de la siguiente manera:

```
GET /bulk/v1/activities/export/{exportId}/status.json
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
         "numberOfRecords": 15423,
         "fileSize": 12342,
         "fileChecksum": "sha256:c16514c7e80fcac5ea055dacae9617fc3c29aff5365e3743071313ce0ed2a815"
      }
   ]
}
```

El campo de estado puede responder con uno de los siguientes valores:

- Creado
- En cola
- Procesamiento
- Cancelado
- Completado
- Error

## Recuperación de datos

Una vez completado el trabajo, recupere los datos mediante el punto de conexión [Obtener archivo de actividad de exportación](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/getExportActivitiesFileUsingGET).

```
GET /bulk/v1/activities/export/{exportId}/file.json
```

La respuesta contiene un archivo con el formato que tenía el trabajo configurado. El punto final responde con el contenido del archivo.

Si un campo de posible cliente solicitado está vacío (no contiene datos), `then null` se coloca en el campo correspondiente del archivo de exportación.  En el ejemplo siguiente, el campo campaignId de la actividad devuelta está vacío.

```json
marketoGUID,leadId,activityDate,activityTypeId,campaignId,primaryAttributeValueId,primaryAttributeValue,attributes
783957693,5414087,2022-02-13T14:06:20Z,104,8497,1670,MembershipTest1,"{""Reason"":""Changed by Smart Campaign MembershipTestCampaignStepChoice.MembershipTestCampaignStepChoiceSetUp action Change Data Value"",""Program Member ID"":3240303,""Acquired By"":true,""Old Status"":""Not in Program"",""New Status ID"":21,""Success"":false,""New Status"":""On List"",""Old Status ID"":20}"
783958220,5414094,2022-02-13T14:08:50Z,104,17240,3569,SuccessWebCPS,"{""Program Member ID"":3240305,""Acquired By"":false,""Old Status"":""Not in Program"",""New Status ID"":6,""Success"":true,""New Status"":""Attended"",""Old Status ID"":1}"
783958306,5414094,2022-02-13T14:09:16Z,104,17240,3569,SuccessWebCPS,"{""Program Member ID"":3240305,""Acquired By"":false,""Old Status"":""Attended"",""New Status ID"":6,""Success"":false,""New Status"":""Attended"",""Old Status ID"":6}"
783961924,5316669,2022-02-13T14:27:21Z,104,11614,2333,Nurture Automation,"{""Program Member ID"":3240306,""Acquired By"":false,""Old Status"":""Not in Program"",""New Status ID"":27,""Success"":false,""New Status"":""Member"",""Old Status ID"":26}"
```

Para admitir la recuperación parcial y fácil de reanudar de los datos extraídos, el extremo de archivo admite opcionalmente el encabezado HTTP `Range` del tipo `bytes`.  Si no se establece el encabezado, se devuelve todo el contenido.  Puede obtener más información sobre el uso del encabezado Range con Marketo [Bulk Extract](bulk-extract.md).

## Cancelación de un trabajo

Si un trabajo se configuró incorrectamente o se vuelve innecesario, se puede cancelar fácilmente usando el punto de conexión [Cancelar trabajo de actividad de exportación](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/cancelExportActivitiesUsingPOST):

```
POST /bulk/v1/activities/export/{exportId}/cancel.json
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

Esta respuesta tiene un estado que indica que el trabajo se ha cancelado.
