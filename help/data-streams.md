---
title: Flujos de datos
description: Información general sobre los flujos de datos de Marketo Engage que permiten eventos de actividad de clientes potenciales y de auditoría de usuarios casi en tiempo real, lo que reduce los límites de API para los clientes de nivel de rendimiento
exl-id: 5617b6a5-ebc8-4d97-a290-e3b87f83e360
TQID: https://experienceleague.adobe.com/JnhN70HexjmNueZa9MAVrxjEhZ5yJatWqZiowl22quA
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: e1e0219c-f879-479f-8427-888ed2a6e9c2
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1347
ht-degree: 4%

---

# Flujos de datos

>[!NOTE]
>
>La información actual sobre los flujos de datos ahora se encuentra en [Usando flujos de datos](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-data-streams#).
>

Los flujos de datos ofrecen grandes volúmenes de datos de Marketo Engage a sistemas externos en tiempo casi real. Utilice datos transmitidos para apoyar decisiones oportunas, campañas segmentadas, procesos de marketing externo y auditorías.

Los flujos de datos ofrecen estas ventajas:

- Reduzca la dependencia en solicitudes de API limitadas a la velocidad.
- Reduzca las alertas de límite de API.
- Ofrezca datos sin ejecutar exportaciones masivas.

Hay flujos de datos disponibles para quienes hayan adquirido un [paquete de nivel de rendimiento de Marketo Engage](https://nation.marketo.com/t5/product-documents/marketo-engage-performance-tiers/ta-p/328835).

## Resumen del flujo de datos de actividad de clientes potenciales

Flujo de datos de actividad de clientes potenciales envía grandes volúmenes de datos de actividad de clientes potenciales a un sistema externo en tiempo casi real. Utilice el flujo para auditar eventos de posibles clientes y patrones de uso, ver cambios de posibles clientes y flujos de trabajo de déclencheur de eventos de posibles clientes.

Puede suscribirse a más de 144 tipos de actividades.

El flujo puede incluir lo siguiente:

1. Cambios en todos los campos de posibles clientes y en los posibles clientes recién creados.
1. Todos los tipos de actividades de posibles clientes documentados.
1. Posibles clientes eliminados.
1. Todos los objetos personalizados de posibles clientes, cuando se solicite. No se pueden seleccionar objetos personalizados individuales.

Los casos de uso comunes incluyen:

- Alerta personalizada: agregar posibles clientes con condiciones incoherentes a una lista. La secuencia envía la actividad Añadir a la lista a un proceso de seguimiento.
- Modelos de aprendizaje automático: utilice perspectivas de actividad en modelos de puntuación externos y, a continuación, envíe puntuaciones a Marketo para influir en las campañas de nutrición u otros procesos.

Lista de actividades transmitidas:

| AlcanzarMetaEnReferencia | ClickPredictiveContent | ReceivedForwardToFriendEmail |
| --- | --- | --- |
| AddToList | ClickRTPCallToAction | ReceiveSalesEmail |
| AddToNurture | ClickSalesEmail | ReferirAplicaciónSocial |
| AddToOpportunity | ClickSharedLink | RemoveFromList |
| AddToSalesCampaign | ConvertLead | RemoveFromOpportunity |
| CallWebhook | DeleteLead | RequestCampaign |
| ChangeDataValue | Descalificar sorteo | SalesEmailBounce |
| ChangeLeadPartition | EarnEntryInSocialApp | SendAlert |
| CambiarCadenciaNutrición | EmailBounce | SendEmail |
| ChangeNurtureTrack | EmailBounceSoft | SendSalesEmail |
| ChangeOwner | EmailDelivered | SentForwardToFriendEmail |
| ChangeProgramData | EnrichWithDataDotCom | SFDCActivity |
| ChangeProgramMemberData | EnterSweepstakes | ShareContent |
| ChangeRevenueStage | FillOutFacebookLeadAdsForm | SignUpForReferralOffer |
| ChangeRevenueStageManualmente | FillOutForm | SyncLeadToMicrosoft |
| ChangeScore | Momento interesante | SyncLeadToSFDC |
| Cambiar segmento | MergeLeads | Cancelar suscripción por correo electrónico |
| ChangeStatusInProgression | Nuevo posible cliente | UpdateOpportunity |
| ChangeStatusInSalesCampaign | OpenEmail | VisitWebPage |
| ClickEmail | OpenSalesEmail | VoteInPoll |
| ClickLink | PushLeadToMarketo | WinSweepstakes |

Al transmitir objetos personalizados, incluya todos los objetos personalizados relacionados con el posible cliente. No se pueden seleccionar objetos personalizados individuales.

## Resumen del flujo de datos de auditoría de usuarios

El flujo de datos de auditoría de usuarios rastrea los cambios de los usuarios en los recursos en tiempo casi real. Utilícelo para auditar eventos de recursos, ver cambios de usuario y procesos de déclencheur desde eventos de auditoría.

Adobe I/O Events envía los cambios a un punto de conexión configurable. Suscríbase a los tipos de eventos necesarios para cada tipo de recurso.

Un caso de uso es:

- Seguimiento de cambios en sistemas de marketing: Cuando un CRM u otro sistema intercambia posibles clientes con Marketo, utilice eventos de auditoría para identificar qué sistema realizó el cambio más reciente.

Lista de eventos de auditoría de usuarios transmitidos:

| COMPONENTE | LISTA DE TIPO DE EVENTO |
| --- | --- |
| Programa predeterminado | clonar, crear, eliminar, editar canal, exportar, modificar configuración del programa, modificar token del programa, cambiar nombre |
| Correo electrónico | aprobar, clonar, crear, eliminar, editar, mover, cambiar el nombre, desaprobar |
| Programa por lotes de correos electrónicos | aprobar, childUpdate, clonar, crear, eliminar, editar, editar canal, modificar programación del programa, modificar configuración del programa, modificar token del programa, cambiar nombre, desaprobar |
| Plantilla de correo electrónico | aprobar, clonar, crear, eliminar, borradorCrear, borradorDescartar, editar, cambiar nombre, desaprobar |
| Programa de participación | clonar, crear, eliminar, editar canal, modificar configuración del programa, modificar flujo del programa, modificar token del programa, cambiar nombre |
| Programa del evento | clonar, crear, eliminar, editar canal, modificar programación, modificar configuración del programa, modificar token del programa, cambiar nombre |
| Carpeta | crear, eliminar, editar, cambiar nombre |
| Formulario | aprobar, clonar, crear, eliminar, borradorCrear, editar, mover, cambiar nombre |
| Formulario -> Formulario de página de aterrizaje | crear, clonar, editar, eliminar, aprobar, cambiar nombre |
| Página de destino | aprobar, clonar, crear, eliminar, borradorDescartar, editar, cambiar el nombre, desaprobar |
| Plantilla de la página de destino | aprobar, clonar, crear, eliminar, borradorCrear, borradorDescartar, editar, cambiar nombre, desaprobar |
| Lista inteligente | clonar, crear, eliminar, editar, exportar, modificar configuración de listas inteligentes, cambiar nombre |
| Carpeta de marketing | crear, editar, eliminar |
| Programa de cultivo | clonar, crear, eliminar, editar canal, modificar configuración del programa, modificar flujo del programa, modificar token del programa, cambiar nombre |
| Segmento | crear, eliminar, editar, cambiar nombre |
| Segmentación | aprobar, crear, eliminar, borradorCreado, borradorDescartado, cambiar nombre, desaprobar |
| Campaña inteligente | cancelar, activar, clonar, crear, desactivar, eliminar, editar, modificar la programación de campañas, modificar la acción del paso de flujo, modificar la configuración de la lista inteligente, mover, cambiar nombre |
| Fragmento | aprobar, aprobar sin borrador, clonar, crear, eliminar, editar, cambiar el nombre, desaprobar |
| IU de administración -> Punto de inicio -> Integración | crear, eliminar, editar |
| IU de administración -> Usuario | crear, editar, eliminar (igual para el usuario solo de API) |
| Inicio de sesión de administrador -> Usuario | inicio de sesión correcto, error de inicio |
| Programa -> Programa por lotes de correo electrónico | editar (para cambiar la dirección de correo electrónico seleccionada) API de recursos |
| Programa -> Programa de marketing | crear, clonar |

Ejemplo de evento de auditoría de usuario:

```json
{
    "event_id": "a1b2c3d4-zyxw-9876-9z8y-a1b2c3d4e5f6",
    "event": {
        "specversion": "1.0",
        "id": "b77c743a-8e28-40f2-8aab-9541bbc85e68",
        "type": "com.adobe.platform.marketo.audit.user.email",
        "source": "https://www.marketo.com",
        "time": "2020-05-28T19:20:47.28Z",
        "datacontenttype": "application/json",
        "dataschema": "V1.0",
        "data": {
            "componentId": 232459,
            "componentType": "Email",
            "eventAction": "approve",
            "munchkinId": "123-ABC-456",
            "imsOrgId": "ADOBEORGID@AdobeOrg",
            "user": 253,
            "userId": "example@marketo.com"
        }
    }
}
```

## Resumen del flujo de datos de notificaciones

El flujo de datos de notificación está disponible como parte de las ofertas de nivel de rendimiento de Marketo Engage.

El centro de notificaciones de Marketo puede enviar notificaciones a una dirección de correo electrónico. El flujo de datos de notificación también envía esas notificaciones a un extremo configurable a través de Adobe I/O Events. Estas son las mismas notificaciones disponibles en el icono de campana de la interfaz de usuario de Marketo.

Lista de eventos de notificación:

| COMPONENTE | LISTA DE TIPO DE EVENTO |
| --- | --- |
| Notificación | anulación de la campaña, error de campaña, nutrición (programa agotado), error de sincronización de salesforce, grupo de prueba (resultado de prueba A/B), servicios web (cuota diaria) |

Ejemplo de evento de notificación:

```json
{
    "event_id": "a1b2c3d4-zyxw-9876-9z8y-a1b2c3d4e5f6",
    "event": {
        "specversion": "1.0",
        "type": "com.adobe.platform.marketo.notification.campaign_abort",
        "source": "https://www.marketo.com",
        "time": "2021-05-27T10:22:37.489-5:00",
        "datacontenttype": "application/json",
        "dataschema": "V1.0",
        "data": {
            "componentType": "campaign_abort",
            "subType": "user_campaign_abort",
            "eventAction": {
                "campaignId":1234,
                "userId":"example@marketo.com",
            }
            "imsOrgId":"ADOBEORGID@AdobeOrg",
            "munchkinId":"123-ABC-456"
        }
    }
}
```

## Detalles técnicos

Las secciones siguientes describen la configuración necesaria para recibir datos de cada flujo. Cada flujo requiere la configuración del punto de conexión y el código de integración.

### Flujo de datos de actividad de clientes potenciales

Flujo de actividad de posibles clientes envía eventos de actividad de posibles clientes suscritos con estas características de servicio:

- De forma predeterminada, los datos se insertan cada dos segundos.
- Cada suscripción utiliza lotes de 100 a 500 registros.
- El servicio REST del cliente tiene un tiempo de espera de 20 segundos y tres reintentos a intervalos de tres minutos. Un reintento correcto habilita automáticamente el servicio. Después de tres errores, el servicio se pausa y vuelve a intentar cada tres minutos a menos que se desactive manualmente.
- Los datos en cola se conservan durante un máximo de siete días.

Para implementar el flujo de datos de actividad de clientes potenciales:

1. Exponga un extremo HTTP que pueda recibir solicitudes POST con un cuerpo JSON desde la red pública de Internet. El flujo de datos push de actividad envía solicitudes a:
1. Proporcione a Adobe lo siguiente:
   1. Marketo Munchkin ID para su suscripción
   1. La URL del punto de conexión en el paso 1
   1. Los tipos de actividad que desean recibir (lista completa arriba)
   1. Un medio de autenticación, para que el cliente pueda verificar que las solicitudes son legítimas. O bien:
      1. Una URL de proveedor de identidad, un ID de cliente y un secreto de cliente para la autenticación de credenciales de cliente [OAuth](https://www.oauth.com/oauth2-servers/access-tokens/client-credentials/)
      1. Un token de API, que se puede incluir en solicitudes enviadas por el flujo de datos de actividad del posible cliente en un encabezado http de autorización

Adobe habilita el flujo de datos después de recibir la información requerida. A continuación, el extremo comienza a recibir datos.

Diagrama de UML de una llamada de flujo de datos de actividad de posibles clientes típica:

![Diagrama del flujo de datos de la actividad del posible cliente](assets/lead-activity-data-stream.png)

Ejemplo de creación de extremo de URL:

```javascript
/*
Copyright 2022 Adobe
All Rights Reserved.

NOTICE: Adobe permits you to use, modify, and distribute this file in
accordance with the terms of the Adobe license agreement accompanying
it.
*/
constexpress=require('express')
constwinston=require('winston');
constport=3000

constapp=express().use(express.json())

constlogger=winston.createLogger({
  level: 'info',
  format: winston.format.json(),
  defaultMeta: {service: 'activity-stream-consumer-example'},
  transports: [
    // - Write all logs with level `error` and below to `error.log`
    newwinston.transports.File({filename: 'error.log',level: 'error'}),
    // - Write all logs with level `info` and below to `combined.log`
    newwinston.transports.File({filename: 'combined.log'}),
    newwinston.transports.Console({format: winston.format.simple()})
  ],
});

app.get('/',(req,res)=>{
  logger.info(JSON.stringify(req.query))
  res.sendStatus(200)
})

app.post('/',(req,res)=>{
  logger.info(JSON.stringify(req.body))
  res.sendStatus(200)
})

app.listen(port,()=>{
  logger.info(`app listening on port ${port}`)
})
```

Consulte el [ejemplo del consumidor de la secuencia de datos de la actividad del posible cliente](https://github.com/ihgrant/activity-stream-consumer-example) para ver el código de aplicación de ejemplo.

### Flujo de datos de auditoría de usuarios y flujo de datos de notificaciones

Los eventos de auditoría de usuarios se envían a través de Adobe I/O. Para consumirlos con un Adobe ID:

1. Proporcione a Adobe la siguiente información:
   1. Adobe ID
   1. Marketo Munchkin ID para su suscripción
1. Exponga un extremo REST, normalmente un gancho web, para consumir eventos.
1. Después de recibir la información del punto de conexión, Adobe habilita el flujo para la suscripción.
1. Configure el flujo en Adobe I/O.
   1. Este paso requiere una organización de Adobe
   1. Requiere que el usuario de organización de Adobe tenga la función Desarrollador o Administrador del sistema

Para configurar Adobe I/O, consulte [Configuración de flujos de datos de auditoría de usuarios de Marketo con Adobe I/O](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-user-audit-data-stream-setup#).

### Configuración del flujo de datos de auditoría de usuarios en Marketo

El flujo de datos de auditoría de usuarios está disponible actualmente como parte de los paquetes de rendimiento junto con los otros 3 flujos de datos. Para obtener más información sobre los paquetes, consulte la [Página de descripción del producto](https://helpx.adobe.com/es/legal/product-descriptions/adobe-marketo-engage---product-description.html) para conocer los límites y características del producto.

### Configuración de Adobe I/O

[Consulte Introducción a Adobe I/O Events](https://developer.adobe.com/runtime/docs/guides/getting-started/)

Para obtener instrucciones básicas sobre este caso de uso, a partir de [console.adobe.io](https://developer.adobe.com/console):

Cuando se le solicite, seleccione **[!UICONTROL Crear nuevo proyecto]** o **[!UICONTROL Agregar evento]**.

### Introducción a su nuevo proyecto

Para empezar a usar los servicios de Adobe, agregue una API, eventos o tiempo de ejecución, y vea nuestra [documentación](https://developer.adobe.com/runtime/docs/).

## Documentación pública

- [Marketo Data Streams](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-data-streams/)
- [Introducción a Adobe IO Events &amp; Webhooks](https://developer.adobe.com/events/docs/guides/)
- [Blog de flujos de datos](https://blog.developer.adobe.com/introducing-the-adobe-marketo-engage-data-streams-61198b567fbb)
