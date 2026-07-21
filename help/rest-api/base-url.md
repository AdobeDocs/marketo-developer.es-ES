---
title: Dirección URL base
feature: REST API
description: Aprenda a crear solicitudes de API de REST de Marketo, comprenda los recursos y parámetros de la ruta de la URL base y encuentre su URL base única.
exl-id: 6c3f122c-3ace-4ed3-bed0-a6b89cedc99a
TQID: https://experienceleague.adobe.com/NZisV6V-FMPi0RHpdaFrc1kZc3nb15YomwRgohaQmEE
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 146
ht-degree: 3%

---

# Dirección URL base

Cada llamada de API en la [referencia de extremo](endpoint-reference.md) especifica el método, la ruta, el recurso y los parámetros de REST. Anexe estos componentes a la dirección URL base para formar una solicitud.

El siguiente es un ejemplo de una URL REST bien formada:

`https://284-RPR-133.mktorest.com/rest/v1/lead/318581.json?fields=email,firstName,lastName`

El ejemplo contiene los siguientes componentes:

- **URL básica:** `https://284-RPR-133.mktorest.com/rest`
- **Ruta de acceso:** `/v1/lead/`
- **Recurso:** `318582.json`
- **Parámetro de consulta:** `fields=email,firstName,lastName`

La dirección URL base contiene el ID de cuenta, también conocido como ID de Munchkin, y es única para cada suscripción a Marketo.

Para encontrar la URL base, inicia sesión en Marketo y ve a **[!UICONTROL Administración]** > **[!UICONTROL Integración]** > **[!UICONTROL Servicios web]**. La dirección URL base está etiquetada como &quot;Extremo:&quot; en la sección &quot;API REST&quot;, como se muestra en la siguiente imagen.

![Extremo de dirección URL base de servicios web](assets/rest-api-base-url-web-services.png)

Copie la dirección URL base e inclúyala en la dirección URL de cada llamada a la API de REST.
