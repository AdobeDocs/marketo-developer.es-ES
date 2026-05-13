---
title: Dirección URL base
feature: REST API
description: Aprenda a crear solicitudes de API de REST de Marketo, comprenda los recursos y parámetros de la ruta de la URL base y encuentre su URL base única.
exl-id: 6c3f122c-3ace-4ed3-bed0-a6b89cedc99a
TQID: https://experienceleague.adobe.com/NZisV6V-FMPi0RHpdaFrc1kZc3nb15YomwRgohaQmEE
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 00118a89f25a23b931fac671130932bb0e0e4e4e
workflow-type: tm+mt
source-wordcount: 157
ht-degree: 2%

---

# Dirección URL base

La documentación de [Endpoint Reference](endpoint-reference.md) para cada llamada a la API muestra el método, la ruta, el recurso y los parámetros de REST que deben adjuntarse a la dirección URL base para formar una solicitud.

El siguiente es un ejemplo de una URL REST bien formada:

`https://284-RPR-133.mktorest.com/rest/v1/lead/318581.json?fields=email,firstName,lastName`

que consta de las siguientes partes:

- URL básica: `https://284-RPR-133.mktorest.com/rest`
- Ruta: `/v1/lead/`
- Recurso: `318582.json`
- Parámetro de consulta: `fields=email,firstName,lastName`

La dirección URL base contiene el ID de cuenta (también conocido como ID de Munchkin) y, por lo tanto, es única para cada suscripción de Marketo. La dirección URL base se encuentra al iniciar sesión en Marketo y navegar al menú **[!UICONTROL Administración]** > **[!UICONTROL Integración]** > **[!UICONTROL Servicios web]**. Está etiquetada como &quot;Extremo:&quot; debajo de la sección &quot;API de REST&quot;, como se muestra en las siguientes capturas de pantalla.

![Extremo de dirección URL base de servicios web](assets/rest-api-base-url-web-services.png)

Una vez encontrada la dirección URL base, cópiela e inclúyala en las direcciones URL que utilice al llamar a cualquiera de las API de REST.
