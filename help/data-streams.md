---
title: Flujos de datos
description: Resumen de Data Streams
exl-id: 5617b6a5-ebc8-4d97-a290-e3b87f83e360
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '1601'
ht-degree: 2%

---

# Flujos de datos

>[!NOTE]
> La información actual sobre los flujos de datos ahora se encuentra en [Usando flujos de datos](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-data-streams/).
>

Las organizaciones de marketing de nuestros clientes dependen de las campañas de marketing oportunas y centradas para mantenerse al día con su negocio y ser competitivas. Para apoyar las decisiones rápidas y permitir el cambio estratégico a gran velocidad, es importante tener datos para apoyar e impulsar esas decisiones clave que ofrecen campañas centradas y segmentadas. También hay algunos clientes que realizan esfuerzos de marketing en niveles de sus segmentos de clientes tanto dentro como fuera de Marketo Engage. Para apoyar estos diferentes esfuerzos, Marketo ha creado la capacidad de adquirir grandes volúmenes de datos en tiempo casi real mediante flujos de datos.

Además de las ventajas de los datos casi en tiempo real, existen ventajas relacionadas con el producto:

- Alivia el cuello de botella de los límites de la API porque se utiliza el streaming en su lugar.
- Reduce el escenario de límites de API y genera menos mensajes de alerta.
- Ahora debe realizar exportaciones masivas para extraer datos gracias a la capacidad de flujo de datos.

Hay flujos de datos disponibles para quienes hayan adquirido un [paquete de nivel de rendimiento de Marketo Engage](https://nation.marketo.com/t5/product-documents/marketo-engage-performance-tiers/ta-p/328835).

## Resumen del flujo de datos de actividad de clientes potenciales

El flujo de datos de actividad de posibles clientes proporciona una transmisión casi en tiempo real del seguimiento de la auditoría de las actividades de posibles clientes, donde se pueden enviar grandes volúmenes de actividades de posibles clientes al sistema externo de un cliente. Los flujos permiten a los clientes auditar de forma eficaz los eventos relacionados con los posibles clientes, los patrones de uso, proporcionar vistas de los cambios de posibles clientes y los procesos y flujos de trabajo de déclencheur en función de los distintos tipos de eventos de posibles clientes. Existen más de 144 tipos de actividades a las que se puede suscribir y enviar mediante el flujo.

Tipos de datos de posibles clientes transmitidos:

1. Cambios de posibles clientes: todos los cambios en todos los campos y nuevos posibles clientes
1. Actividades de posibles clientes: todos los tipos de actividades de posibles clientes descritos en el documento
1. Posibles clientes eliminados
1. Todos los objetos personalizados del posible cliente (si se solicita). Es todo o nada en este momento.

Al proporcionar vistas sobre los cambios de los posibles clientes, esto permite a los clientes tomar decisiones más rápidas sobre sus estrategias de marketing generales y crear campañas dirigidas más específicas. Algunos casos de uso populares serían:

- Alerta personalizada: cuando se encuentran ciertos posibles clientes con condiciones incoherentes, se pueden agregar a la lista. Los flujos de actividad pueden detectarlos e insertar la actividad &quot;Agregar a la lista&quot; para que los clientes realicen cualquier acción de seguimiento.
- Powering ML Models: Algunos clientes planean crear modelos de puntuación que utilicen las perspectivas de actividad y enviarlos de nuevo a Marketo o utilizar en sus propios modelos de puntuación internos como deseen. Al puntuar un posible cliente, los clientes pueden informar a Marketo para que añada clientes a las campañas de Nutrir y aumentar su puntuación.

Lista de actividades transmitidas:

| AlcanzarMetaEnReferencia | ClickPredictiveContent | ReceivedForwardToFriendEmail |
|--- |--- |--- |
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

Tenga en cuenta que si los objetos personalizados deben transmitirse, deben ser todos los objetos personalizados relacionados con el posible cliente. En este momento no hay forma de seleccionar cuáles son las deseadas.

## Resumen del flujo de datos de auditoría de usuarios

La secuencia de datos de auditoría de usuarios proporciona un seguimiento de auditoría casi en tiempo real de los cambios de recursos por parte de los usuarios&#x200B;. Esto permite a un cliente auditar eventos de recursos de forma eficaz, proporcionar una vista de los cambios de los usuarios y déclencheur los procesos o flujos de trabajo en función de diferentes tipos de eventos de auditoría. Los cambios de recursos casi en tiempo real se envían mediante eventos de Adobe I/O a un punto de conexión configurable. Los eventos de auditoría se desglosan por tipo de recurso y pueden suscribirse a eventos de auditoría que son importantes para ellos.

Un buen caso de uso para suscribirse a este flujo sería:

- Seguimiento de cambios al utilizar varios sistemas de marketing: hay algunos clientes que también realizan algún nivel de actividades de marketing en otro sistema, como un CRM como Salesforce, y luego pasan el posible cliente a Marketo. El posible cliente a veces se actualiza y sincroniza de un lado a otro, por lo que es importante rastrear qué sistema ha realizado cambios recientes.

Lista de eventos de auditoría de usuarios transmitidos:

| COMPONENTE | LISTA DE TIPO DE EVENTO |
|--- |--- |
| Programa predeterminado | clonar, crear, eliminar, editar canal, exportar, modificar configuración del programa, modificar token del programa, cambiar nombre |
| Correo electrónico | aprobar, clonar, crear, eliminar, editar, mover, cambiar el nombre, desaprobar |
| Programa por lotes de correos electrónicos | aprobar, childUpdate, clonar, crear, eliminar, editar, editar canal, modificar programación del programa, modificar configuración del programa, modificar token del programa, cambiar nombre, desaprobar |
| Plantilla de email | aprobar, clonar, crear, eliminar, borradorCrear, borradorDescartar, editar, cambiar nombre, desaprobar |
| Programa de participación | clonar, crear, eliminar, editar canal, modificar configuración del programa, modificar flujo del programa, modificar token del programa, cambiar nombre |
| Programa del evento | clonar, crear, eliminar, editar canal, modificar programación, modificar configuración del programa, modificar token del programa, cambiar nombre |
| Carpeta | crear, eliminar, editar, cambiar nombre |
| Form | aprobar, clonar, crear, eliminar, borradorCrear, editar, mover, cambiar nombre |
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

Actualmente, el centro de notificaciones de Marketo se puede configurar para que envíe notificaciones a una dirección de correo electrónico. La secuencia de datos de notificación permite enviar notificaciones directamente a un extremo configurable a través de eventos de Adobe I/O. Las notificaciones se proporcionan a través de la interfaz de usuario hoy en día y se puede hacer referencia a ellas mediante la campana naranja en la parte superior derecha de la pantalla. Este flujo toma esas notificaciones y las envía a través de un flujo.

Lista de eventos de notificación:

| COMPONENTE | LISTA DE TIPO DE EVENTO |
|--- |--- |
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

Esta sección proporciona instrucciones sobre lo que se necesita, cómo conectarse y recibir datos de flujo continuo para cada uno de los flujos. Hay algún nivel de codificación y configuración involucrado para cada uno.

### Flujo de datos de actividad de clientes potenciales

El flujo de actividad de posibles clientes proporciona una transmisión casi en tiempo real de los eventos de actividad de posibles clientes de Marketo y envía los cambios de tipo de actividad suscrita con atributos configurables:

- La frecuencia de los datos se inserta cada 2 segundos de forma predeterminada.
- Lotes de 100 a 500 por suscripción.
- El tiempo de espera para el servicio REST del cliente es de 20 segundos, con 3 reintentos cada 3 minutos y habilitado automáticamente si se realiza correctamente. De lo contrario, después de esto, se pausan. Una vez en pausa, el servicio se reintenta cada tres minutos en un intento de volver a habilitar, a menos que se anule el aprovisionamiento manualmente.
- Retención de datos en cola de hasta 7 días.

Para implementar el flujo de datos de actividad de clientes potenciales, estos son los pasos que deben seguir los clientes:

1. Exponga un extremo HTTP que pueda recibir solicitudes POST con un cuerpo JSON desde la red pública de Internet. El flujo de datos push de actividad envía solicitudes a:
1. Proporcione a Adobe lo siguiente:
   1. Marketo Munchkin ID para su suscripción
   1. La URL del punto de conexión en el paso 1
   1. Los tipos de actividad que desean recibir (lista completa arriba)
   1. Un medio de autenticación, para que el cliente pueda verificar que las solicitudes son legítimas. O bien:
      1. Una URL de proveedor de identidad, un ID de cliente y un secreto de cliente para la autenticación de credenciales de cliente [OAuth](https://www.oauth.com/oauth2-servers/access-tokens/client-credentials/)
      1. Un token de API, que se puede incluir en solicitudes enviadas por el flujo de datos de actividad del posible cliente en un encabezado http de autorización

A continuación, Adobe habilita el flujo de datos, momento en el que los clientes empiezan a recibir datos.

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

Se puede encontrar [aquí](https://github.com/ihgrant/activity-stream-consumer-example) un ejemplo de código para una aplicación que consume la secuencia de datos de actividad de clientes potenciales de Marketo.

### Flujo de datos de auditoría de usuarios y flujo de datos de notificaciones

Los eventos de auditoría de usuarios se envían a Adobe IO y se pueden consumir iniciando sesión con un Adobe ID. Estos son los pasos a seguir:

1. Los clientes proporcionan a Adobe lo siguiente:
   1. Adobe ID
   1. Marketo Munchkin ID para su suscripción
1. El cliente expone un extremo REST para consumir eventos normalmente en forma de webhook.
1. Una vez proporcionada, Adobe habilita el flujo para la suscripción del cliente.
1. A continuación, el cliente configura el flujo en Adobe IO (instrucciones que se proporcionarán)
   1. Este paso requiere una organización de Adobe
   1. Requiere que el usuario de organización de Adobe tenga la función Desarrollador o Administrador del sistema

Para configurar Adobe IO, consulte [Configuración de flujos de datos de auditoría de usuarios de Marketo con Adobe IO](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-user-audit-data-stream-setup/) en la sección Documentación pública.

### Configuración del flujo de datos de auditoría de usuarios en Marketo

El flujo de datos de auditoría de usuarios está disponible actualmente como parte de los paquetes de rendimiento junto con los otros 3 flujos de datos. Para obtener más información sobre los paquetes, consulte la [Página de descripción del producto](https://helpx.adobe.com/es/legal/product-descriptions/adobe-marketo-engage---product-description.html) para conocer los límites y características del producto.

### Configuración de Adobe I/O

[Consulte Introducción a Adobe I/O Events](https://developer.adobe.com/runtime/docs/guides/getting-started/)

Para obtener instrucciones básicas sobre este caso de uso, a partir de [console.adobe.io](https://developer.adobe.com/console):

Cuando se le solicite, seleccione **[!UICONTROL Crear nuevo proyecto]** o **[!UICONTROL Agregar evento]**.

### Introducción a su nuevo proyecto

Para empezar a usar los servicios de Adobe, agregue una API, eventos o tiempo de ejecución, y vea nuestra [documentación](https://developer.adobe.com/runtime/docs/).

## Documentación pública

- [Flujos de datos Marketo](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-data-streams/)
- [Introducción a los eventos y webhooks de Adobe IO](https://developer.adobe.com/events/docs/guides/)
- [Blog de flujos de datos](https://blog.developer.adobe.com/introducing-the-adobe-marketo-engage-data-streams-61198b567fbb)
