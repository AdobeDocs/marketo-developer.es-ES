---
title: Webhooks
feature: Webhooks
description: Obtenga información sobre cómo configurar los webhooks de Marketo para que llamen a servicios de terceros, establecer plantillas de carga útil, codificación, asignaciones de respuestas, tokens, encabezados personalizados y sugerencias.
exl-id: fd283c66-05a1-4aa4-8412-0d41b8d1e3c8
TQID: https://experienceleague.adobe.com/r-GpAqhYPKvlDtMw5l23jeJWzlSqycP65eYJPA3m9EM
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: a7170d27-32ab-462b-a333-269abc654483
  - id: b13bd2ad-8e65-49e5-9691-2a0d31067b35
  - id: d1d0a9cd-295d-4976-8c39-ddae266f240e
  - id: f82558ea-6af5-44eb-a424-5b3389abb0a3
subfeature_v2:
  - id: ad89fb33-8541-4339-afe7-bb13d1633714
  - id: fc9b09fe-b844-4544-887b-e420c3b82065
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 614
ht-degree: 4%

---

# Webhooks

Los webhooks de Marketo se comunican con servicios web de terceros. Un webhook utiliza el verbo GET o POST HTTP para enviar o recuperar datos de una URL específica.

Para obtener instrucciones sobre cómo crear un webhook y agregarlo a una campaña inteligente, consulte:

- [Creación de un webhook](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/administration/additional-integrations/create-a-webhook)
- [Llamar a un Webhook](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/call-webhook)
- [Uso de un webhook en una campaña inteligente](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/use-a-webhook-in-a-smart-campaign)

Configure cada webhook con estas propiedades:

- **[!UICONTROL URL]**: URL a la que envía la solicitud de servicio web.
- **[!UICONTROL Tipo de solicitud]**: método HTTP.
- **[!UICONTROL Plantilla de carga útil]**: la plantilla para la información enviada en el cuerpo de POST. Utilice cualquier formato de datos que admita HTTP POST, incluidos XML, JSON o SOAP. El formato de serialización debe permitir comillas dobles alrededor de las cadenas. Para insertar un token, seleccione **[!UICONTROL Insertar token]**. Marketo incluye automáticamente tokens de tipo cadena entre comillas dobles.
- **[!UICONTROL Codificación de token de solicitud]**: el formato de solicitud, JSON o Formulario/URL, que se usa para codificar valores de token que incluyen caracteres especiales como un signo &amp;. Seleccione la codificación de cuerpo correcta para que el webhook se comunique correctamente con el servicio web.
- **[!UICONTROL Tipo de respuesta]**: el formato de respuesta, JSON o XML. Seleccione el tipo correcto para asignar propiedades de respuesta a campos de posibles clientes en Marketo.
- **[!UICONTROL Encabezados personalizados]**: pares de clave-valor agregados como encabezados HTTP mediante **[!UICONTROL Acciones de webhooks]** > **[!UICONTROL Establecer encabezado personalizado]**. Puede agregar cualquier número de encabezados personalizados.

Use [Asignaciones de respuestas](response-mappings.md) para escribir datos de las respuestas del servicio web en posibles clientes.

## Tókenes

Todos los campos de gancho web salientes, incluidos la URL, la plantilla y los encabezados personalizados, rellenan el contenido de token en el mismo contexto que el paso de flujo.

Los tokens de cliente potencial y de sistema siempre están disponibles. Los tokens de déclencheur, Campaña y Programa están disponibles en sus respectivos ámbitos. Para obtener más información, consulte:

- [Información general sobre tókenes](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/demand-generation/landing-pages/personalizing-landing-pages/tokens-overview)
- [Glosario de tókenes del sistema](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/email-marketing/general/using-tokens/system-tokens-glossary)
- [Tokens para momentos interesantes](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/marketo-sales-insight/msi-for-salesforce/features/tabs-in-the-msi-panel/interesting-moments/trigger-tokens-for-interesting-moments)

Por ejemplo, cuando un programa o una campaña se asigna a un recurso de terceros, establezca un ID de nivel de programa como `My Token`. A continuación, pase el ID a la solicitud de webhook como token.

## Personalizar encabezado

Los webhooks pueden enviar cualquier número de campos de encabezado personalizados con una solicitud saliente. Agregar encabezados mediante **[!UICONTROL Acciones de webhooks]** > **[!UICONTROL Establecer encabezado personalizado]**.

Cada encabezado es un par clave-valor y puede contener tokens.

![Encabezados personalizados](assets/custom-headers.png)

## Sugerencias

- Utilice el paso de flujo Llamar al webhook solo en campañas de Déclencheur.
- Las asignaciones de respuesta actualizan un registro solo cuando el servicio web devuelve un código de respuesta HTTP 2xx.
- Puede utilizar servicios web para realizar procesos personalizados de enriquecimiento, validación o normalización de datos desde servicios internos o externos.
- El tiempo de ejecución del webhook depende del tiempo de respuesta del servicio y puede causar retrasos largos en la ejecución de la campaña. Incluso si un servicio tarda solo 50 ms en ejecutarse, 100 000 ejecuciones tardan 1,5 horas.
- Marketo espera hasta 30 segundos una llamada de servicio determinada antes de finalizar la llamada (también conocido como tiempo de espera agotado).
- Marketo pasa los caracteres del campo URL tal y como están escritos. Por ejemplo, &quot;&amp;&quot; se envía como &quot;&amp;&quot; y &quot;%26&quot; se envía como &quot;%26&quot;.
  - Para enviar un carácter con codificación de porcentaje al servidor de destinatarios, pase explícitamente la cadena que representa ese carácter.
