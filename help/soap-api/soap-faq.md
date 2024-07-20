---
title: SOAP PREGUNTAS MÁS FRECUENTES
feature: SOAP
description: SOAP PREGUNTAS MÁS FRECUENTES
exl-id: a2d8f144-cd5f-41bc-8231-29c42af935b8
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---

# SOAP PREGUNTAS MÁS FRECUENTES

**Q:** ¿Cómo puedo obtener una lista de todos los programas de Marketo junto con sus metadatos?

**A:** Para recuperar una lista de todos los programas, puede usar [getMObjects](./getmobjects.md) pasando el tipo igual a &quot;Programa&quot; y estableciendo includeDetails en true.

**Q:** ¿Hay alguna manera de acelerar el rendimiento de getMultipleLeads?

**A:** Hay algunas opciones para acelerar el rendimiento de la llamada a getMultipleLeads. El primero es reducir el batchSize que está solicitando para cada llamada. 200 es el tamaño de lote recomendado. La segunda opción es especificar los campos que le interesan utilizando el filtro includeAttributes. Esto acelera la consulta y reduce la carga útil de la respuesta que recibe. El método final consiste en utilizar LastUpdateAtSelector y especificar oldUpdatedAt y latestUpdatedAt. Puede especificar diferentes intervalos de fechas y, a continuación, enhebrar varias solicitudes simultáneamente. SOAP Si utiliza el método de subprocesos, asegúrese de que su cliente de/WSDL admite [conexiones persistentes](https://www.w3.org/Protocols/rfc2616/rfc2616-sec8.html).

SOAP **Q:** ¿Cómo puedo crear oportunidades a través de la API de la cuando no estoy integrado con un CRM como SalesForce o Microsoft Dynamics?

SOAP **A:** Puede crear oportunidades usando la API de la usando la llamada de [syncMObjects](syncmobjects.md) que escribe en los tipos OpportunityPersonRole y Opportunity [MObject](marketo-objects.md).

**Q:** ¿Puedo enviar un correo electrónico desde Marketo mediante programación? Si es así, ¿cómo puedo enviar contenido personalizado para cada destinatario de correo electrónico?

**A:** Absolutamente. Puede solicitar que Marketo SOAP le envíe un mensaje de correo electrónico utilizando [requestCampaign](requestcampaign.md) o una combinación de las API [importToList](importtolist.md) y [scheduleCampaign](schedulecampaign.md) para la. Para enviar inmediatamente un correo electrónico a una o más personas, debe usar [requestCampaign](requestcampaign.md). Si desea programar el envío de un correo electrónico en una fecha y hora especificadas, debe usar [importToList](importtolist.md) para especificar los destinatarios del correo electrónico y [scheduleCampaign](schedulecampaign.md) para especificar cuándo se enviará ese correo electrónico a esos destinatarios.

Si desea personalizar el contenido para cada destinatario de correo electrónico, puede hacerlo anulando los valores de [tokens de programa](../rest-api/tokens.md) establecidos en la plantilla de correo electrónico.
