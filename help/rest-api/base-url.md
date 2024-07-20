---
title: URL básica
feature: REST API
description: Describe cómo crear direcciones URL para Marketo.
exl-id: 6c3f122c-3ace-4ed3-bed0-a6b89cedc99a
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 0%

---

# URL básica

La documentación de [Endpoint Reference](endpoint-reference.md) para cada llamada a la API muestra el método, la ruta, el recurso y los parámetros de REST que deben adjuntarse a la dirección URL base para formar una solicitud.

El siguiente es un ejemplo de una URL REST bien formada:

`https://284-RPR-133.mktorest.com/rest/v1/lead/318581.json?fields=email,firstName,lastName`

que consta de las siguientes partes:

- URL básica: `https://284-RPR-133.mktorest.com/rest`
- Ruta: `/v1/lead/`
- Recurso: `318582.json`
- Parámetro de consulta: `fields=email,firstName,lastName`

La dirección URL base contiene el ID de cuenta (también conocido como ID de Munchkin) y, por lo tanto, es único para cada suscripción de Marketo. La dirección URL base se encuentra al iniciar sesión en Marketo y navegar al menú **[!UICONTROL Administración]** > **[!UICONTROL Integración]** > **[!UICONTROL Servicios web]**. Está etiquetada como &quot;Extremo:&quot; debajo de la sección &quot;API de REST&quot;, como se muestra en las siguientes capturas de pantalla.

![Extremo de dirección URL base de servicios web](assets/rest-api-base-url-web-services.png)

Una vez encontrada la dirección URL base, cópiela e inclúyala en las direcciones URL que utilice al llamar a cualquiera de las API de REST.
