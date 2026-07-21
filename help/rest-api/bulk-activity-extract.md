---
title: Extracción de actividades en lotes
feature: REST API
description: La API de REST de extracción masiva de actividades de Marketo para exportar datos de actividad de gran volumen mediante filtros de intervalo de fechas, actividad y atributo principal de 31 días para ETL y CRM.
exl-id: 6bdfa78e-bc5b-4eea-bcb0-e26e36cf6e19
TQID: https://experienceleague.adobe.com/lIlXNjatN-F77Dv3xsVkQ3hAWwLZ4wlSW0zKNkFJFMA
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: b0bb9048-d951-48d8-8232-45cf248a7e27id: e64968b2-4ee5-47f9-8cae-0588f184b9ebid: ea90ebee-5c84-42d9-8b21-006bdabc95a3
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1268
ht-degree: 7%

---

# Extracción de actividades en lotes

[Referencia de extremo de extracción de actividades en lotes](https://developer.adobe.com/marketo-apis/api/mapi)

Las API de REST de extracción masiva recuperan grandes volúmenes de datos de actividad de Marketo. Utilice estas API para procesos que no requieran baja latencia, como integración CRM, ETL, almacenamiento de datos y archivado de datos.

## Permisos

El usuario de la API debe tener el permiso &quot;Actividad de solo lectura&quot; o &quot;Actividad de lectura-escritura&quot;.

## Filtros

| Tipo de filtro | Tipo de datos | Obligatorio | Notas |
| --- | --- | --- | --- |
| `createdAt` | Date Range | Sí | Objeto JSON que contiene `startAt` y `endAt`. `startAt` es la fecha y hora de nivel bajo de agua, y `endAt` es la fecha y hora de nivel alto de agua. El intervalo debe ser de 31 días o menos. El trabajo devuelve todos los registros accesibles creados dentro del intervalo de fechas. Utilice valores de fecha y hora ISO-8601 sin milisegundos. |
| `activityTypeIds` | Matriz\[Entero\] | No | Una matriz de enteros para los tipos de actividad solicitados. No se admite la actividad &quot;Eliminar posible cliente&quot;. En su lugar, use el extremo [Obtener posibles clientes eliminados](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getDeletedLeadsUsingGET). Recupere los identificadores de tipo de actividad con el [extremo Obtener tipos de actividad](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getAllActivityTypesUsingGET). |
| [`primaryAttributeValueIds`](#primaryattributevalueids-options) | Matriz\[Entero\] | No | Matriz que acepta un máximo de 50 identificadores para atributos principales. Cada ID identifica de forma exclusiva un campo o recurso de posible cliente. Recupere los ID llamando al extremo de la API de REST adecuado. Por ejemplo, para filtrar un formulario específico para la actividad &quot;Rellenar formulario&quot;, pase el nombre del formulario al extremo [Obtener formulario por nombre](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms/operation/getLpFormByNameUsingGET) para recuperar el ID del formulario. Consulte [opciones primaryAttributeValueIds](#primaryattributevalueids-options) para ver los tipos de actividades compatibles. |
| [`primaryAttributeValues`](#primaryattributevalues-options) | Matriz\[Cadena\] | No | Matriz que acepta un máximo de 50 nombres para atributos principales. Cada nombre identifica de forma exclusiva un campo o recurso de posible cliente. Recupere nombres llamando al extremo de la API de REST adecuado. Por ejemplo, para filtrar un formulario específico para la actividad &quot;Rellenar formulario&quot;, pase el ID del formulario a [Obtener formulario por ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms/operation/getLpFormByIdUsingGET) para recuperar el nombre del formulario. Consulte [opciones primaryAttributeValues](#primaryattributevalues-options) para ver los tipos de actividades compatibles. |

### primaryAttributeValueIds, opciones {#primaryattributevalueids-options}

| Tipo de actividad | Identificador de valor de atributo principal | Extremo de recuperación | Grupo de recursos |
| --- | --- | --- | --- |
| Cambiar valor de datos | ID de campo de posible cliente | [Describir posible cliente](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/describeUsingGET_2) | Nombre del atributo |
| Cambiar calificación | ID de campo de posible cliente | [Describir posible cliente](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/describeUsingGET_2) | Nombre del atributo |
| Cambio de estado en progreso | ID de programa | [Obtener programa por nombre](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/getProgramByNameUsingGET) | Programa de marketing |
| Añadir a la lista | ID de lista estática | [Obtener lista estática por nombre](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListByNameUsingGET) | Lista estática |
| Quitar de la lista | ID de lista estática | [Obtener lista estática por nombre](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListByNameUsingGET) | Lista estática |
| Completar formulario | ID de formulario | [Obtener formulario por nombre](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms/operation/getLpFormByNameUsingGET) | Formulario web |

Si usa `primaryAttributeValueIds`, también debe incluir el filtro `activityTypeIds`. Este filtro solo puede contener ID de actividad que coincidan con el grupo de recursos correspondiente. Por ejemplo, al filtrar recursos de formularios web, `activityTypeIds` solo puede contener el ID de tipo de actividad &quot;Rellenar formulario&quot;.

La siguiente solicitud incluye el filtro `primaryAttributeValueIds`:

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
| Cambiar valor de datos | DisplayName del campo de posible cliente | [Describir posible cliente](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/describeUsingGET_2) | Nombre del atributo |
| Cambiar calificación | DisplayName del campo de posible cliente | [Describir posible cliente](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/describeUsingGET_2) | Nombre del atributo |
| Cambio de estado en progreso | Nombre del programa | [Obtener programa por identificador](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/getProgramByIdUsingGET) | Programa de marketing |
| Añadir a la lista | Nombre de lista estática | [Obtener lista estática por identificador](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListByIdUsingGET) | Lista estática |
| Quitar de la lista | Nombre de lista estática | [Obtener lista estática por identificador](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListByIdUsingGET) | Lista estática |
| Completar formulario | Nombre del formulario | [Obtener formulario por identificador](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms/operation/getLpFormByIdUsingGET) | Formulario web |

Utilice la notación `&lt;program&gt;.&lt;asset&gt;` para especificar nombres para los grupos de recursos Programa de marketing, Lista estática y Formulario web. Por ejemplo, especifique el formulario &quot;MPS saliente&quot; en el programa &quot;GL_OP_ALL_2021&quot; como &quot;GL_OP_ALL_2021.MPS saliente&quot;.

La siguiente solicitud incluye el filtro `primaryAttributeValues`:

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

Si usa `primaryAttributeValues`, también debe incluir el filtro `activityTypeIds`. Este filtro solo puede contener ID de actividad que coincidan con el grupo de recursos correspondiente. Por ejemplo, al filtrar recursos de formularios web, `activityTypeIds` solo puede contener el ID de tipo de actividad &quot;Rellenar formulario&quot;.

`primaryAttributeValues` y `primaryAttributeValueIds` no se pueden usar juntos.

## Opciones

| Parámetro | Tipo de datos | Obligatorio | Notas |
| --- | --- | --- | --- |
| `filter` | Objeto | Sí | Objeto que contiene filtros que se aplican al conjunto de actividades accesibles. Incluir exactamente un filtro `createdAt`. También puede incluir un filtro `activityTypeIds`. El trabajo de exportación devuelve el conjunto resultante de actividades. |
| `format` | Cadena | No | El formato del archivo de exportación: CSV, TSV o SSV. Estos valores producen valores separados por comas, tabulaciones o espacios, respectivamente. El valor predeterminado es CSV. |
| `columnHeaderNames` | Objeto | No | Un objeto JSON de pares de clave-valor de campo y encabezado de columna. Cada clave debe asignar un nombre a un campo incluido en el trabajo de exportación. Su valor establece el encabezado de columna exportado para ese campo. |
| `fields` | Matriz\[Cadena\] | No | Matriz de campos que se incluirá en el archivo de exportación. De manera predeterminada, la respuesta incluye `marketoGUID`, `leadId`, `activityDate`, `activityTypeId`, `campaignId`, `primaryAttributeValueId`, `primaryAttributeValue` y `attributes`. Para devolver un subconjunto, especifique los campos de esta lista, como `"fields": ["leadId", "activityDate", "activityTypeId"]`. También puede especificar `actionResult` para incluir la acción de la actividad: `("succeeded", "skipped", or "failed")`. |

## Creación de un trabajo

Cree un trabajo de exportación para definir los registros que desea recuperar. Use el extremo [Crear trabajo de actividad de exportación](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Activities/operation/createExportActivitiesUsingPOST).

Cada trabajo requiere un filtro `createdAt`. Sus parámetros datetime `startAt` y `endAt` definen las fechas de creación de actividades permitidas más tempranas y más recientes. Para excluir los tipos de actividades que no son relevantes, incluya también el filtro `activityTypeIds` opcional.

La siguiente solicitud crea un trabajo de exportación de CSV para los tipos de actividad seleccionados dentro de un intervalo de fechas:

```http
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

La respuesta devuelve un `exportId` y el estado &quot;Creado&quot;. Un trabajo creado aún no está en la cola de procesamiento.

Para agregar el trabajo a la cola, llame al extremo [Trabajo de actividad de exportación en cola](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Activities/operation/enqueueExportActivitiesUsingPOST) con `exportId` desde la respuesta de creación.

```http
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

El estado de la respuesta ahora es &quot;En cola&quot;. Cuando un trabajador está disponible, el estado cambia a &quot;Procesando&quot; y el trabajo comienza a agregar registros desde Marketo.

## Estado del trabajo de sondeo

El estado del trabajo solo se puede recuperar para trabajos creados por el mismo usuario de API.

La extracción masiva de actividades procesa los trabajos de forma asíncrona. Encuesta el punto de conexión [Obtener estado del trabajo de actividad de exportación](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Activities/operation/getExportActivitiesStatusUsingGET) para determinar cuándo se ha completado un trabajo:

```http
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

El campo `status` devuelve uno de los siguientes valores:

- `Created`
- `Queued`
- `Processing`
- `Canceled`
- `Completed`
- `Failed`

## Recuperación de datos

Cuando el estado del trabajo sea &quot;Completado&quot;, recupere los datos exportados con el punto de conexión [Obtener archivo de actividad de exportación](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Activities/operation/getExportActivitiesFileUsingGET):

```http
GET /bulk/v1/activities/export/{exportId}/file.json
```

El cuerpo de respuesta contiene el archivo en el formato configurado para el trabajo.

Si un campo de actividad solicitado no contiene datos, `null` aparece en el campo de archivo de exportación correspondiente. El siguiente ejemplo muestra los datos de actividad exportados:

```json
marketoGUID,leadId,activityDate,activityTypeId,campaignId,primaryAttributeValueId,primaryAttributeValue,attributes
783957693,5414087,2022-02-13T14:06:20Z,104,8497,1670,MembershipTest1,"{""Reason"":""Changed by Smart Campaign MembershipTestCampaignStepChoice.MembershipTestCampaignStepChoiceSetUp action Change Data Value"",""Program Member ID"":3240303,""Acquired By"":true,""Old Status"":""Not in Program"",""New Status ID"":21,""Success"":false,""New Status"":""On List"",""Old Status ID"":20}"
783958220,5414094,2022-02-13T14:08:50Z,104,17240,3569,SuccessWebCPS,"{""Program Member ID"":3240305,""Acquired By"":false,""Old Status"":""Not in Program"",""New Status ID"":6,""Success"":true,""New Status"":""Attended"",""Old Status ID"":1}"
783958306,5414094,2022-02-13T14:09:16Z,104,17240,3569,SuccessWebCPS,"{""Program Member ID"":3240305,""Acquired By"":false,""Old Status"":""Attended"",""New Status ID"":6,""Success"":false,""New Status"":""Attended"",""Old Status ID"":6}"
783961924,5316669,2022-02-13T14:27:21Z,104,11614,2333,Nurture Automation,"{""Program Member ID"":3240306,""Acquired By"":false,""Old Status"":""Not in Program"",""New Status ID"":27,""Success"":false,""New Status"":""Member"",""Old Status ID"":26}"
```

Para la recuperación parcial o reanudable, el extremo del archivo admite el encabezado HTTP `Range` opcional con un intervalo de `bytes`. Si omite este encabezado, el extremo devolverá todo el archivo. Para obtener más información sobre el uso del encabezado `Range`, consulte [Extracción en lotes](bulk-extract.md).

## Cancelación de un trabajo

Para detener un trabajo innecesario o configurado incorrectamente, llame al extremo [Cancelar trabajo de actividad de exportación](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Activities/operation/cancelExportActivitiesUsingPOST):

```http
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

El estado de respuesta indica que el trabajo se ha cancelado.
