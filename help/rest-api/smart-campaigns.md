---
title: "Campañas inteligentes"
feature: REST API, Smart Campaigns
description: "Descripción general de la campaña inteligente"
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '989'
ht-degree: 1%

---


# Campañas inteligentes

[Referencia de extremo de campañas inteligentes (recurso)](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns)

[Referencia de extremo de campañas (posibles clientes)](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns)

Marketo ofrece un conjunto de API de REST para realizar operaciones en campañas inteligentes. Estas API siguen el patrón de interfaz estándar para las API de recursos que proporcionan opciones de consulta, creación, clonación y eliminación. Además, puede administrar la ejecución de campañas inteligentes programando campañas por lotes o solicitando campañas de déclencheur.

## Consulta

La consulta de campañas inteligentes sigue los tipos de consulta estándar para los recursos de [por id](#by_id), [por nombre](#by_name), y [exploración](#browse).

### Por ID

El [Obtener campañas inteligentes por ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/getSmartCampaignByIdUsingGET) el extremo toma una sola campaña inteligente `id` como parámetro de ruta y devuelve un solo registro de campaña inteligente.

```
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

Con este punto final, siempre habrá un único registro en la primera posición del `result` matriz.

### Por nombre

El [Obtener campaña inteligente por nombre](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/getSmartCampaignByNameUsingGET) el extremo toma una sola campaña inteligente `name` como parámetro y devuelve un solo registro de campaña inteligente.

```
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

Con este punto final, siempre habrá un único registro en la primera posición del `result` matriz.

### Examinar

El [Obtener campañas inteligentes](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/getAllSmartCampaignsGET) El punto de conexión funciona como otros puntos finales de exploración de Asset API y permite que varios parámetros de consulta opcionales especifiquen criterios de filtrado.

El `earliestUpdatedAt` y `latestUpdatedAt` parámetros aceptar `datetimes` en formato ISO-8601 (sin milisegundos). Si se establecen ambos, firstUpdatedAt debe preceder a latestUpdatedAt.

El `folder` parámetro especifica la carpeta principal bajo la que se va a examinar. El formato es un bloque JSON que contiene `id` y `type` atributos.

El `maxReturn` El parámetro es un entero que especifica el número máximo de entradas que se van a devolver. El valor predeterminado es 20. El máximo es 200.

El `offset` El parámetro es un entero que especifica por dónde empezar a recuperar entradas. Se puede usar junto con `maxReturn`. El valor predeterminado es 0.

El `isActive` El parámetro es un booleano que especifica que solo se devuelven campañas de Déclencheur activas.

```
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

Con este extremo, habrá uno o más registros en la variable `result` matriz.

## Crear

El [Crear campaña inteligente](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/createSmartCampaignUsingPOST) El extremo se ejecuta con un POST application/x-www-form-urlencoded con dos parámetros obligatorios. El `name` parámetro especifica el nombre de la campaña inteligente que se va a crear. El `folder` parámetro especifica la carpeta principal en la que se crea la campaña inteligente. El formato es un bloque JSON que contiene `id` y `type` atributos.

De forma opcional, puede describir la campaña inteligente utilizando `description` parámetro (máximo 2000 caracteres).

```
POST /rest/asset/v1/smartCampaigns.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

El [Actualizar campaña inteligente](https://developer.adobe.com/marketo-apis/api/asset/) El extremo se ejecuta con un POST application/x-www-form-urlencoded. Se necesita una sola campaña inteligente `id` como parámetro de ruta. Puede usar el complemento `name` para actualizar el nombre de la campaña inteligente o el parámetro `description` para actualizar la descripción de la campaña inteligente.

```
POST /rest/asset/v1/smartCampaign/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

El [Clonar campaña inteligente](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) El extremo se ejecuta con un POST application/x-www-form-urlencoded con tres parámetros obligatorios. Se necesita un `id` parámetro que especifica la campaña inteligente que se va a clonar, un `name` parámetro que especifica el nombre de la nueva campaña inteligente y un `folder` para especificar la carpeta principal en la que se crea la nueva campaña inteligente. El formato es un bloque JSON que contiene `id` y `type` atributos.

De forma opcional, puede describir la campaña inteligente utilizando `description` parámetro (máximo 2000 caracteres).

```
POST /rest/asset/v1/smartCampaign/{id}/clone.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

El [Eliminar campaña inteligente](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/deleteSmartCampaignUsingPOST) el extremo toma una sola campaña inteligente `id` como parámetro de ruta.

```
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

Las campañas inteligentes por lotes se inician en un momento específico y afectan a un conjunto específico de posibles clientes a la vez.

## Programación

Utilice el [Programar campaña](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns/operation/scheduleCampaignUsingPOST) extremo para programar una campaña por lotes para que se ejecute inmediatamente o en una fecha futura. La campaña `id` es un parámetro de ruta requerido. Los parámetros opcionales son `tokens`, `runAt`, y `cloneToProgram` que se pasan en el cuerpo de la solicitud como application/json.

El parámetro de matriz de tokens es una matriz de Mis tokens que anula los tokens de programa existentes. Después de ejecutar la campaña, los tokens se descartan.  Cada elemento de matriz de token contiene pares de nombre/valor. El nombre del token debe tener el formato &quot;{{my.name}}&quot;.

El parámetro runAt de fecha y hora especifica cuándo ejecutar la campaña. Si no se especifica, la campaña se ejecutará 5 minutos después de llamar al extremo. El valor datetime no puede ser superior a dos años en el futuro.

Las campañas programadas a través de esta API siempre esperan un mínimo de cinco minutos antes de ejecutarse.

El `cloneToProgram` parámetro de cadena contiene el nombre de un programa resultante.  Cuando se configura, esto hace que la campaña, el programa principal y todos sus recursos se creen con el nuevo nombre resultante. El programa principal se clona y la campaña recién creada se programa. El programa resultante se crea debajo del elemento principal. Los programas con fragmentos, notificaciones push, mensajes en la aplicación, listas estáticas, informes y recursos sociales no se pueden clonar de esta manera. Cuando se utiliza, este punto de conexión está limitado a 20 llamadas al día. El [programa de clonación](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) la variable es la alternativa recomendada.

```
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

## Desencadenante

Las campañas inteligentes de déclencheur afectan a una persona a la vez en función de un evento activado.

### Solicitud

Utilice el [Solicitar campaña](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns/operation/triggerCampaignUsingPOST) extremo para pasar un conjunto de posibles clientes a una campaña de déclencheur para que se ejecute a través del flujo de la campaña. La campaña debe tener un déclencheur &quot;Campaña solicitada&quot; con &quot;API de servicio web&quot; como origen.

Este extremo requiere una campaña `id` como parámetro de ruta y `leads` parámetro de matriz de enteros que contiene los id de posibles clientes Se permite un máximo de 100 posibles clientes por llamada.

Opcionalmente, la variable `tokens` El parámetro de matriz se puede utilizar para anular Mis tokens locales del programa principal de la campaña. `tokens` acepta un máximo de 100 token. Cada `tokens` el elemento de matriz contiene un par nombre/valor. El nombre del token debe tener el formato &quot;{{my.name}}&quot;. Si utiliza [Añadir un token de sistema como vínculo en un correo electrónico](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/using-tokens/add-a-system-token-as-a-link-in-an-email) para añadir el token del sistema &quot;viewAsWebpageLink&quot;, no puede anularlo utilizando `tokens`. En su lugar utilice [Añadir un vínculo de Ver como página web a un correo electrónico](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/add-a-view-as-web-page-link-to-an-email) que le permite anular &quot;viewAsWebPageLink&quot; utilizando `tokens`.

El `leads` y `tokens` Los parámetros de se pasan en el cuerpo de la solicitud como application/json.

```
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

El [Activar campaña inteligente](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/activateSmartCampaignUsingPOST) el punto final es directo. Un `id` El parámetro de ruta es obligatorio. Para que la activación se realice correctamente, debe cumplirse lo siguiente en la campaña:

- Se debe desactivar
- Debe tener al menos un déclencheur y un paso de flujo
- Debe tener déclencheur, filtros y pasos de flujo libres de errores

```
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

El [Desactivar campaña inteligente](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/deactivateSmartCampaignUsingPOST) es sencillo. Un `id` El parámetro de ruta es obligatorio. Para que la desactivación se realice correctamente, debe activarse la campaña.

```
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
