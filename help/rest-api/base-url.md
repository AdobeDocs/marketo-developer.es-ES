---
title: "URL base"
feature: REST API
description: '"Describe cómo configurar las direcciones URL para Marketo".'
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 0%

---


# URL básica

El [Referencia de extremo](endpoint-reference.md) La documentación de cada llamada de API muestra el método, la ruta, el recurso y los parámetros REST que deben adjuntarse a la URL base para formar una solicitud.

El siguiente es un ejemplo de una URL REST bien formada:

`https://284-RPR-133.mktorest.com/rest/v1/lead/318581.json?fields=email,firstName,lastName`

que consta de las siguientes partes:

- URL básica: `https://284-RPR-133.mktorest.com/rest`
- Ruta: `/v1/lead/`
- Recurso: `318582.json`
- Parámetro de consulta: `fields=email,firstName,lastName`

La dirección URL base contiene el ID de cuenta (también conocido como ID de Munchkin) y, por lo tanto, es único para cada suscripción de Marketo. La URL base se encuentra iniciando sesión en Marketo y navegando hasta **[!UICONTROL Administrador]** > **[!UICONTROL Integración]** > **[!UICONTROL Servicios web]** menú. Está etiquetada como &quot;Extremo:&quot; debajo de la sección &quot;API de REST&quot;, como se muestra en las siguientes capturas de pantalla.

![Punto final de URL base de servicios web](assets/rest-api-base-url-web-services.png)

Una vez encontrada la dirección URL base, cópiela e inclúyala en las direcciones URL que utilice al llamar a cualquiera de las API de REST.
