---
title: "PREGUNTAS FRECUENTES SOAP"
feature: SOAP
description: "PREGUNTAS FRECUENTES SOAP"
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---


# PREGUNTAS FRECUENTES SOAP

**P:** ¿Cómo puedo obtener una lista de todos los programas de Marketo junto con sus metadatos?

**A:** Para recuperar una lista de todos los programas, puede utilizar [getMObjects](./getmobjects.md) al pasar el tipo igual a &quot;Programa&quot; y establecer includeDetails en true.

**P:** ¿Hay alguna manera de acelerar el rendimiento de getMultipleLeads?

**A:** Existen varias opciones para acelerar el rendimiento de la llamada a getMultipleLeads. El primero es reducir el batchSize que está solicitando para cada llamada. 200 es el tamaño de lote recomendado. La segunda opción es especificar los campos que le interesan utilizando el filtro includeAttributes. Esto acelera la consulta y reduce la carga útil de la respuesta que recibe. El método final consiste en utilizar LastUpdateAtSelector y especificar oldUpdatedAt y latestUpdatedAt. Puede especificar diferentes intervalos de fechas y, a continuación, enhebrar varias solicitudes simultáneamente. Si utiliza el método de subprocesos, asegúrese de que su cliente SOAP/WSDL es compatible con [conexiones persistentes](https://www.w3.org/Protocols/rfc2616/rfc2616-sec8.html).

**P:** ¿Cómo puedo crear oportunidades a través de la API de SOAP cuando no estoy integrado con un CRM como SalesForce o Microsoft Dynamics?

**A:** Puede crear oportunidades mediante la API de SOAP usando [syncMObjects](syncmobjects.md) Llamada de escritura a OpportunityPersonRole y Opportunity [MObject](marketo-objects.md) tipos.

**P:** ¿Puedo enviar un correo electrónico desde Marketo mediante programación? Si es así, ¿cómo puedo enviar contenido personalizado para cada destinatario de correo electrónico?

**A:** Por supuesto. Puede solicitar que un correo electrónico se envíe desde Marketo utilizando las opciones [requestCampaign](requestcampaign.md) o combinación de [importToList](importtolist.md) y [scheduleCampaign](schedulecampaign.md) API de SOAP. Para enviar inmediatamente un correo electrónico a una o varias personas, debe utilizar [requestCampaign](requestcampaign.md). Si desea programar el envío de un correo electrónico a una fecha y hora especificadas, debe utilizar [importToList](importtolist.md) para especificar los destinatarios del correo electrónico, y [scheduleCampaign](schedulecampaign.md) para especificar cuándo se enviarán esos destinatarios ese correo electrónico.

Si desea personalizar el contenido para cada destinatario del correo electrónico, puede hacerlo anulando los valores de [tokens de programa](../rest-api/tokens.md) que se establecen dentro de la plantilla de correo electrónico.
