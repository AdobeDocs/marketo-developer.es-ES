---
title: Preguntas frecuentes sobre SOAP
feature: SOAP
description: Aprenda a enumerar programas con getMObjects, optimizar getMultipleLeads, crear oportunidades y enviar o programar correos electrónicos personalizados mediante la API de SOAP de Marketo.
exl-id: a2d8f144-cd5f-41bc-8231-29c42af935b8
TQID: https://experienceleague.adobe.com/AWgJgPdDXmapXqvG-J93utvXGV8-zLnKO-DvWFzOYoI
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 00118a89f25a23b931fac671130932bb0e0e4e4e
workflow-type: tm+mt
source-wordcount: 360
ht-degree: 0%

---

# Preguntas frecuentes sobre SOAP

**Q:** ¿Cómo puedo obtener una lista de todos los programas de Marketo junto con sus metadatos?

**A:** Para recuperar una lista de todos los programas, puede usar [getMObjects](./getmobjects.md) pasando el tipo igual a &quot;Programa&quot; y estableciendo includeDetails en true.

**Q:** ¿Hay alguna manera de acelerar el rendimiento de getMultipleLeads?

**A:** Hay algunas opciones para acelerar el rendimiento de la llamada a getMultipleLeads. El primero es reducir el batchSize que está solicitando para cada llamada. 200 es el tamaño de lote recomendado. La segunda opción es especificar los campos que le interesan utilizando el filtro includeAttributes. Esto acelera la consulta y reduce la carga útil de la respuesta que recibe. El método final consiste en utilizar LastUpdateAtSelector y especificar oldUpdatedAt y latestUpdatedAt. Puede especificar diferentes intervalos de fechas y, a continuación, enhebrar varias solicitudes simultáneamente. Si utiliza el método de subprocesos, asegúrese de que su cliente de SOAP/WSDL admite [conexiones persistentes](https://www.w3.org/Protocols/rfc2616/rfc2616-sec8.html).

**Q:** ¿Cómo puedo crear oportunidades a través de la API de SOAP cuando no estoy integrado con un CRM como Salesforce o Microsoft Dynamics?

**A:** Puede crear oportunidades usando la API de SOAP usando la llamada de [syncMObjects](syncmobjects.md) que escribe en los tipos OpportunityPersonRole y Opportunity [MObject](marketo-objects.md).

**Q:** ¿Puedo enviar un correo electrónico desde Marketo mediante programación? Si es así, ¿cómo puedo enviar contenido personalizado para cada destinatario de correo electrónico?

**A:** Absolutamente. Puede solicitar que Marketo le envíe un mensaje de correo electrónico utilizando [requestCampaign](requestcampaign.md) o una combinación de las API de SOAP [importToList](importtolist.md) y [scheduleCampaign](schedulecampaign.md). Para enviar inmediatamente un correo electrónico a una o más personas, debe usar [requestCampaign](requestcampaign.md). Si desea programar el envío de un correo electrónico en una fecha y hora especificadas, debe usar [importToList](importtolist.md) para especificar los destinatarios del correo electrónico y [scheduleCampaign](schedulecampaign.md) para especificar cuándo se enviará ese correo electrónico a esos destinatarios.

Si desea personalizar el contenido para cada destinatario de correo electrónico, puede hacerlo anulando los valores de [tokens de programa](../rest-api/tokens.md) establecidos en la plantilla de correo electrónico.
