---
title: Contexto de usuario
feature: REST API
description: Aprenda a habilitar y utilizar la API de contexto de usuario de Marketo RTP para establecer variables personalizadas, leer datos de usuarios en visitas y rastrear campañas en las que se ha hecho clic y visualizado.
exl-id: b8daace2-07a5-4621-aa3a-03fa9f66ea73
TQID: https://experienceleague.adobe.com/Ph0Tw-C9jzWaR4bYyUIXyzzoa2yjHQk2gt6tNA8H2mA
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: e2290edd-b061-4880-9d79-dee306cf5aa9id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
subfeature_v2: id: a1d50dda-6d94-4e16-8c30-5eb7181c4650
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 273
ht-degree: 5%

---

# Contexto de usuario

La API de JavaScript de contexto de usuario expone los datos de nivel de usuario y de visitante en varias sesiones. Utilice datos y comportamientos históricos para crear una personalización avanzada.

La API también proporciona variables personalizadas para enviar datos y eventos al servidor RTP para la segmentación y personalización. Ver las capacidades relacionadas de [Déclencheur](../javascript-api/triggers.md) y [Coincidencia de patrones](../javascript-api/pattern-match.md).

- Debe ser cliente de Web Personalization y tener la etiqueta [RTP implementada](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) en el sitio.
- Debe solicitar al Soporte técnico de Marketo que habilite la API de contexto de usuario. Después de la habilitación, un objeto userContext se expone en el objeto global RTP.

## Atributos de contexto de usuario

| Nombre | Tipo | Descripción |
| --- | --- | --- |
| `customVar[1-5]` | Cadena | Los datos personalizados se guardan en el contexto del usuario. |
| `viewedCampaigns` | ID de campaña como cadena separada por comas | Campañas vistas en visitas actuales o anteriores. |
| `clickedCampaigns` | ID de campaña como cadena separada por comas | Se ha hecho clic en las campañas de visitas actuales o anteriores. |

## Establecer variables personalizadas

Configure variables personalizadas para agregar datos al contexto de usuario.

### Uso

`rtp('set', 'customVar'[1-5], my_custom_value);`

| Parámetro | Opcional/Requerida | Tipo | Descripción |
| --- | --- | --- | --- |
| `'set'` | Obligatorio | Cadena | Acción de método. |
| `customVar` | Obligatorio | Cadena | Nombre de variable personalizado. |
| `my_custom_value` | Obligatorio | Cadena | Valor personalizado para guardar en variable personalizada en los índices 1-5. |

Las variables personalizadas se envían a RTP solo en una llamada de vista. Establezca variables personalizadas antes de la llamada de vista. De lo contrario, las variables se envían en la siguiente llamada de vista.

Las variables personalizadas tienen las siguientes restricciones:

- Una variable personalizada no puede superar los 100 caracteres.
- Los datos de Campaign se limitan a las últimas diez visitas, con diez campañas por visita.

### Uso

`rtp('set', 'customVar', 'A');`

```javascript
// Set and get customVars
rtp('set', 'customVar1', 'foo');

// Read location
if (rtp.userContext.location.state == 'CA')  {
    // Do something
}

// Check if user viewed campaign id 45:
// The campaign id is exposed in the RTP UI when hovering over a campaign name.
if (rtp.userContext.viewedCampaign('45')) {
    // Do something
}
```
