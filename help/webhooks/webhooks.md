---
title: Webhooks
feature: Webhooks
description: Obtenga información sobre cómo configurar los webhooks de Marketo para que llamen a servicios de terceros, establecer plantillas de carga útil, codificación, asignaciones de respuestas, tokens, encabezados personalizados y sugerencias.
exl-id: fd283c66-05a1-4aa4-8412-0d41b8d1e3c8
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '604'
ht-degree: 0%

---

# Webhooks

Marketo permite el uso de webhooks para comunicarse con servicios web de terceros. Los webhooks admiten el uso de los verbos GET o POST HTTP para insertar o recuperar datos de una dirección URL específica. Para obtener instrucciones detalladas sobre la creación de webhooks en la aplicación y cómo añadirlos a campañas inteligentes, consulte los siguientes artículos:

- [Crear un webhook](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/additional-integrations/create-a-webhook)
- [Llamar a webhook](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/call-webhook)
- [Usar un webhook en una campaña inteligente](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/use-a-webhook-in-a-smart-campaign)

Cada webhook individual tiene las siguientes propiedades:

- [!UICONTROL URL]: escriba la URL que usa para enviar la solicitud al servicio web.
- [!UICONTROL Tipo de solicitud]: método HTTP.
- [!UICONTROL Plantilla de carga útil]: si desea transmitir información en el cuerpo de la PUBLICACIÓN, escriba la plantilla. Utilice cualquier formato de datos que admita HTTP POST, incluidos XML, JSON o SOAP. El formato de serialización debe permitir comillas dobles alrededor de las cadenas. Para insertar un token en su plantilla, haga clic en **[!UICONTROL Insertar token]**.  Los tokens de tipo cadena se encierran automáticamente entre comillas dobles.
- [!UICONTROL Codificación de token de solicitud]: si los valores de token incluyen caracteres especiales (como un signo &amp;), indique el formato de su solicitud (JSON o formulario/URL). Debe seleccionarse la codificación correcta para el cuerpo para asegurarse de que el webhook se comunica correctamente con el servicio web.
- [!UICONTROL Tipo de respuesta]: seleccione el formato de la respuesta que recibe del servicio (JSON o XML). Se debe seleccionar el tipo de respuesta correcto para asignar las propiedades de la respuesta a los campos de posible cliente en Marketo
- [!UICONTROL Encabezados personalizados] - A los que se accede a través de [!UICONTROL Acciones de Webhooks] -> [!UICONTROL Establecer encabezado personalizado], este menú permite agregar cualquier número de pares de clave-valor personalizados como encabezados HTTP.

Se pueden escribir datos en los posibles clientes a partir de las respuestas del servicio web mediante [asignaciones de respuestas](response-mappings.md)

## Tókenes

Todos los campos salientes de un webhook (URL, plantilla y encabezados personalizados) rellenan el contenido de los tokens en el mismo contexto del paso de flujo. Esto significa que los tokens de cliente potencial y de sistema siempre están disponibles, mientras que los tokens de Déclencheur, de campaña y de programa están disponibles en sus respectivos ámbitos. Consulte artículos relacionados con tokens:

- [Información general de tokens](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/personalizing-landing-pages/tokens-overview)
- [Glosario de tokens del sistema](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/using-tokens/system-tokens-glossary)
- [Tokens para momentos interesantes](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/marketo-sales-insight/msi-for-salesforce/features/tabs-in-the-msi-panel/interesting-moments/trigger-tokens-for-interesting-moments)

Un caso común es cuando un programa o campaña se asigna explícitamente a un recurso de terceros. Se puede establecer un ID en el nivel de programa como `My Token` y luego pasarlo a la solicitud de webhook como token.

## Personalizar encabezado

Los webhooks permiten el uso de cualquier número de campos de encabezado personalizados para ser enviados junto con la solicitud saliente. Se pueden agregar mediante **[!UICONTROL Acciones de webhooks]** > **[!UICONTROL Establecer encabezado personalizado]**. Cada encabezado se registra como un par clave-valor simple. Los tokens se pueden utilizar en esta área.

![Encabezados personalizados](assets/custom-headers.png)

## Sugerencias

- El paso de flujo Llamar al webhook solo es válido en campañas de Déclencheur.
- Las actualizaciones a través de asignaciones de respuesta solo se producirán si el servicio web responde con un código de respuesta HTTP 2xx. Otros tipos de códigos no producirán actualizaciones en el registro.
- Puede utilizar servicios web para realizar procesos personalizados de enriquecimiento, validación o normalización de datos desde servicios internos o externos.
- El tiempo de ejecución del webhook está a merced del tiempo de respuesta del servicio que se está utilizando y puede resultar en largos retrasos de ejecución de la campaña. Incluso si un servicio solo tarda 50 ms en ejecutarse, es decir, 1,5 horas cuando se ejecuta 100 000 veces.
- Marketo espera hasta 30 segundos una llamada de servicio determinada antes de finalizar la llamada (también conocido como tiempo de espera agotado).
- Los caracteres incrustados en el campo URL se pasan como escritos; por ejemplo, &quot;&amp;&quot; se envía como &quot;&amp;&quot;, &quot;%26&quot; se envía como &quot;%26&quot;
   - Si un carácter debe tener codificación porcentual cuando lo recibe el servidor de destinatarios, debe pasarse explícitamente como la cadena que representa ese carácter
