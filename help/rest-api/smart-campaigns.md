---
title: Campañas inteligentes
feature: REST API, Smart Campaigns
description: Aprenda a utilizar las API de REST de Marketo para campañas inteligentes, incluida la consulta por ID o nombre, los filtros de exploración, la creación de eliminación de clonación y la programación o solicitud de déclencheur
exl-id: 540bdf59-b102-4081-a3d7-225494a19fdd
TQID: https://experienceleague.adobe.com/iysRjtqd9plkreyIMuNjAF3YVFHtDUIrc-GInB4V8mg
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: a7170d27-32ab-462b-a333-269abc654483
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
subfeature_v2:
  - id: ad89fb33-8541-4339-afe7-bb13d1633714
  - id: d0251300-e25f-466f-9856-7e11ce8fa7aa
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1009
ht-degree: 1%

---

# Campañas inteligentes

[Referencia de extremo de campañas inteligentes (recurso)](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns)

[Referencia de extremo de campañas (posibles clientes)](https://developer.adobe.com/marketo-apis/api/mapi#tag/Campaigns)

Utilice las API de REST de campaña inteligente para consultar, crear, clonar y eliminar campañas inteligentes. También puede programar campañas por lotes, solicitar campañas de déclencheur y administrar la activación de campañas.

## Consulta

Consulte las campañas inteligentes [por id.](#by_id), [por nombre](#by_name) o por [exploración](#browse).

### Por ID

El extremo [Get Smart Campaign by ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns/operation/getSmartCampaignByIdUsingGET) toma una sola campaña inteligente `id` como parámetro de ruta y devuelve un único registro de campaña inteligente.

```http
GET /rest/asset/v1/smartCampaign/{id}.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "7883#169838a32f0",
    "warnings": [],
    "result": [
        {
            "id": 1001,
            "name": "Process Bounced Emails",
            "description": "System smart campaign for processing bounced email events",
            "createdAt": "2016-09-10T23:16:19Z+0000",
            "updatedAt": "2016-09-10T23:16:19Z+0000",
            "status": "Never Run",
            "type": "batch",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": false,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 1001,
            "flowId": 1001,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1001A1"
        }
    ]
}
```

El extremo devuelve un registro en la primera posición de la matriz `result`.

### Por nombre

El extremo [Get Smart Campaign by Name](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns/operation/getSmartCampaignByNameUsingGET) toma una sola campaña inteligente `name` como parámetro y devuelve un único registro de campaña inteligente.

```http
GET /rest/asset/v1/smartCampaign/byName.json?name=Test Trigger Campaign
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "14494#16c886ffa44",
    "warnings": [],
    "result": [
        {
            "id": 1069,
            "name": "Test Trigger Campaign",
            "description": "",
            "createdAt": "2018-02-16T01:34:39Z+0000",
            "updatedAt": "2019-08-13T00:45:21Z+0000",
            "folder": {
                "id": 327,
                "type": "Folder"
            },
            "status": "Inactive",
            "type": "trigger",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": false,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 2747,
            "flowId": 1088,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1069A1"
        }
    ]
}
```

El extremo devuelve un registro en la primera posición de la matriz `result`.

### Examinar

El extremo [Get Smart Campaigns](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns/operation/getAllSmartCampaignsGET) admite parámetros de consulta opcionales para el filtrado y la paginación.

Los parámetros `earliestUpdatedAt` y `latestUpdatedAt` aceptan `datetimes` en formato ISO-8601 (sin milisegundos). Si se establecen ambos, firstUpdatedAt debe preceder a latestUpdatedAt.

El parámetro `folder` especifica la carpeta principal que se va a examinar. Páselo como un objeto JSON que contiene `id` y `type`.

El entero `maxReturn` especifica el número máximo de entradas. El valor predeterminado es 20 y el máximo es 200.

El entero `offset` especifica dónde comenzar a recuperar entradas. Úselo con `maxReturn`. El valor predeterminado es 0.

Establezca el parámetro booleano `isActive` para que devuelva solamente las campañas de déclencheur activas.

```http
GET /rest/asset/v1/smartCampaigns.json?earliestUpdatedAt=2016-09-10T23:15:00-00:00&latestUpdatedAt=2016-09-10T23:17:00-00:00
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "626#16983a92965",
    "warnings": [],
    "result": [
        {
            "id": 1001,
            "name": "Process Bounced Emails",
            "description": "System smart campaign for processing bounced email events",
            "createdAt": "2016-09-10T23:16:19Z+0000",
            "updatedAt": "2016-09-10T23:16:19Z+0000",
            "status": "Never Run",
            "type": "batch",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": false,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 1001,
            "flowId": 1001,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1001A1"
        },
        {
            "id": 1002,
            "name": "Process Unsubscribes",
            "description": "System smart campaign for processing unsubscribe events",
            "createdAt": "2016-09-10T23:16:19Z+0000",
            "updatedAt": "2016-09-10T23:16:19Z+0000",
            "status": "Never Run",
            "type": "batch",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": false,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 1002,
            "flowId": 1002,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1002A1"
        }
    ]
}
```

El extremo devuelve uno o más registros en la matriz `result`.

## Crear

Envíe una solicitud POST de `application/x-www-form-urlencoded` al extremo [Crear campaña inteligente](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns/operation/createSmartCampaignUsingPOST). Se requieren los parámetros `name` y `folder`. Pase `folder` como un objeto JSON que contiene `id` y `type`.

Opcionalmente, puede describir la campaña inteligente utilizando el parámetro `description` (máximo 2000 caracteres).

```http
POST /rest/asset/v1/smartCampaigns.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=Smart Campaign 02&folder={"type": "folder","id": 640}&description=This is a smart campaign creation test.
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "25bc#16c9138f148",
    "warnings": [],
    "result": [
        {
            "id": 1076,
            "name": "Smart Campaign 02",
            "description": "This is a smart campaign creation test.",
            "createdAt": "2019-08-14T17:42:04Z+0000",
            "updatedAt": "2019-08-14T17:42:04Z+0000",
            "folder": {
                "id": 640,
                "type": "Folder"
            },
            "status": "Never Run",
            "type": "batch",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": true,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 5132,
            "flowId": 1095,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1076A1"
        }
    ]
}
```

## Actualización

Envíe una solicitud POST de `application/x-www-form-urlencoded` al extremo [Update Smart Campaign](https://developer.adobe.com/marketo-apis/api/asset). Se requiere el parámetro de ruta de acceso de campaña inteligente `id`. Use `name` para cambiar el nombre o `description` para cambiar la descripción.

```http
POST /rest/asset/v1/smartCampaign/{id}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```sql
name=Smart Campaign 02 Update&description=This is a smart campaign update test.
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "14b6a#16c924b992f",
    "warnings": [],
    "result": [
        {
            "id": 1076,
            "name": "Smart Campaign 02 Update",
            "description": "This is a smart campaign update test.",
            "createdAt": "2019-08-14T17:42:04Z+0000",
            "updatedAt": "2019-08-14T22:42:04Z+0000",
            "folder": {
                "id": 640,
                "type": "Folder"
            },
            "status": "Never Run",
            "type": "batch",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": true,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 5132,
            "flowId": 1095,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1076A1"
        }
    ]
}
```

## Clonar

Envíe una solicitud POST de `application/x-www-form-urlencoded` al extremo [Clone Smart Campaign](https://developer.adobe.com/marketo-apis/api/asset#tag/Sales-Persons/operation/describeUsingGET_5). Se requieren los parámetros `id`, `name` y `folder`. Estos especifican la campaña de origen, el nombre de la nueva campaña y la carpeta principal. Pase `folder` como un objeto JSON que contiene `id` y `type`.

Opcionalmente, puede describir la campaña inteligente utilizando el parámetro `description` (máximo 2000 caracteres).

```http
POST /rest/asset/v1/smartCampaign/{id}/clone.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=Test Trigger Campaign Clone&folder={"type": "folder","id": 640}&description=This is a smart campaign clone test.
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "681d#16c9339499b",
    "warnings": [],
    "result": [
        {
            "id": 1077,
            "name": "Test Trigger Campaign Clone",
            "description": "This is a smart campaign clone test.",
            "createdAt": "2019-08-15T03:01:41Z+0000",
            "updatedAt": "2019-08-15T03:01:41Z+0000",
            "folder": {
                "id": 640,
                "type": "Folder"
            },
            "status": "Inactive",
            "type": "trigger",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": false,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 5135,
            "flowId": 1096,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1077A1"
        }
    ]
}
```

## Eliminar

El extremo [Delete Smart Campaign](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns/operation/deleteSmartCampaignUsingPOST) toma una sola campaña inteligente `id` como parámetro de ruta.

```http
POST /rest/asset/v1/smartCampaign/{id}/delete.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "d757#16c934216ac",
    "warnings": [],
    "result": [
        {
            "id": 1077
        }
    ]
}
```

## Lote

Las campañas inteligentes por lotes se ejecutan a una hora especificada y procesan un conjunto definido de posibles clientes juntos.

## Programar

Use [Programar campaña](https://developer.adobe.com/marketo-apis/api/mapi#tag/Campaigns/operation/scheduleCampaignUsingPOST) para programar una campaña por lotes. Se requiere el parámetro de ruta de acceso de la campaña `id`. Pase los parámetros opcionales `tokens`, `runAt` y `cloneToProgram` en el cuerpo de la solicitud JSON.

La matriz `tokens` anula el programa existente Mis tokens para esta ejecución. Marketo descarta las invalidaciones después de que se ejecute la campaña. Cada elemento contiene un par nombre/valor, y el nombre del token debe utilizar el formato `{{my.name}}`.

El parámetro de fecha y hora `runAt` especifica cuándo ejecutar la campaña. Si se omite, la campaña se ejecuta cinco minutos después de la solicitud. El valor no puede ser de más de dos años en el futuro.

Las campañas programadas a través de esta API siempre esperan un mínimo de cinco minutos antes de ejecutarse.

El parámetro de cadena `cloneToProgram` contiene el nombre de un programa resultante.  Cuando se configura, esto hace que la campaña, el programa principal y todos sus recursos se creen con el nuevo nombre resultante. El programa principal se clona y la campaña recién creada se programa. El programa resultante se crea debajo del elemento principal. Los programas con fragmentos, notificaciones push, mensajes en la aplicación, listas estáticas, informes y recursos sociales no se pueden clonar de esta manera. Cuando se utiliza, este punto de conexión está limitado a 20 llamadas al día. El extremo [clone program](https://developer.adobe.com/marketo-apis/api/asset#tag/Sales-Persons/operation/describeUsingGET_5) es la alternativa recomendada.

```http
POST /rest/v1/campaigns/{id}/schedule.json
```

```json
{
   "input":
      {
         "runAt": "2018-03-28T18:05:00+0000",
         "tokens": [
            {
               "name": "{{my.message}}",
               "value": "Updated message"
            },
            {
               "name": "{{my.other token}}",
               "value": "Value for other token"
            }
          ]
      }
}
```

```json
{
    "requestId": "52b#161d90e1743",
    "result": [
        {
            "id": 3713
        }
    ],
    "success": true
}
```

## Activador

Las campañas inteligentes de déclencheur procesan a una persona a la vez en respuesta a un evento.

### Solicitud

Use [Solicitar campaña](https://developer.adobe.com/marketo-apis/api/mapi#tag/Campaigns/operation/triggerCampaignUsingPOST) para pasar posibles clientes a través del flujo de una campaña de déclencheur. La campaña debe utilizar un déclencheur Campaign is Requested con la API de servicio web como fuente.

Se requieren el parámetro de ruta de acceso de la campaña `id` y una matriz de identificadores de posibles clientes `leads`. Cada llamada acepta un máximo de 100 posibles clientes.

Opcionalmente, el parámetro de matriz `tokens` se puede usar para anular Mis tokens locales del programa principal de la campaña. `tokens` acepta un máximo de 100 token. Cada elemento de matriz `tokens` contiene un par nombre/valor. El nombre del token debe tener el formato &quot;`{{my.name}}`&quot;. Si usas [Agregar un token de sistema como un enlace en un correo electrónico](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/email-marketing/general/using-tokens/add-a-system-token-as-a-link-in-an-email) para agregar el token de sistema &quot;viewAsWebpageLink&quot;, no puedes anularlo usando `tokens`. En su lugar, use [Agregar una vista como vínculo de página web a un correo electrónico](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/add-a-view-as-web-page-link-to-an-email), lo que le permite invalidar &quot;viewAsWebPageLink&quot; mediante `tokens`.

Pase los parámetros `leads` y `tokens` en el cuerpo de la solicitud JSON.

```http
POST /rest/v1/campaigns/{id}/trigger.json
```

```json
{
   "input":
      {
         "leads" : [
            {
               "id" : 318592
            },
            {
               "id" : 318593
            }
         ],
         "tokens" : [
            {
               "name": "{{my.message}}",
               "value": "Updated message"
            },
            {
               "name": "{{my.other token}}",
               "value": "Value for other token"
            }
         ]
      }
}
```

```json
{
    "requestId": "9e01#161d922f1aa",
    "result": [
        {
            "id": 3712
        }
    ],
    "success": true
}
```

### Activar

El punto de conexión [Activar campaña inteligente](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns/operation/activateSmartCampaignUsingPOST) es sencillo. Se requiere un parámetro de ruta de acceso `id`. Para que la activación se realice correctamente, debe cumplirse lo siguiente en la campaña:

- La campaña está desactivada.
- La campaña tiene al menos un déclencheur y un paso de flujo.
- La campaña incluye déclencheur, filtros y pasos de flujo libres de errores.

```http
POST /rest/asset/v1/smartCampaign/{id}/activate.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "a33a#161d9c0dcf3",
    "result": [
        {
            "id": 1069
        }
    ]
}
```

### Desactivar

[Desactivar campaña inteligente](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns/operation/deactivateSmartCampaignUsingPOST) es sencillo. Se requiere un parámetro de ruta de acceso `id`. Para que la desactivación se realice correctamente, debe activarse la campaña.

```http
POST /rest/asset/v1/smartCampaign/{id}/deactivate.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "6228#161d9c29fbf",
    "result": [
        {
            "id": 1069
        }
    ]
}
```
