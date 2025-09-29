---
title: Contexto de usuario
feature: REST API
description: Aprenda a habilitar y utilizar la API de contexto de usuario de Marketo RTP para establecer variables personalizadas, leer datos de usuarios en visitas y rastrear campañas en las que se ha hecho clic y visualizado.
exl-id: b8daace2-07a5-4621-aa3a-03fa9f66ea73
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 5%

---

# Contexto de usuario

La API de JavaScript de contexto de usuario expone los datos de nivel de usuario y visitante en varias sesiones para habilitar la capacidad de personalización avanzada mediante el comportamiento y los datos históricos del usuario. La API va más allá de la lectura de datos y expone variables personalizadas que le permiten insertar datos y eventos significativos en el servidor RTP para fines de segmentación y personalización avanzadas. Funciones adicionales: [Déclencheur](../javascript-api/triggers.md), [Coincidencia de patrones](../javascript-api/pattern-match.md).

- Debe convertirse en cliente de Web Personalization y tener la etiqueta [RTP implementada](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) en su sitio antes de usar la API de contexto de usuario.
- La API de contexto de usuario es una función que el Soporte técnico de Marketo debe habilitar si se solicita. Cuando la API está habilitada, se expone un objeto userContext bajo el objeto global RTP.

## Atributos de contexto de usuario

| Nombre | Tipo | Descripción |
|------------------|-------------|------|
| customVar[1-5] | Cadena | Los datos personalizados se guardan en el contexto del usuario. |
| viewcampaigns | ID de campaña como cadena separada por comas | Campañas vistas en visitas actuales o anteriores. |
| clickedCampaigns | ID de campaña como cadena separada por comas | Se ha hecho clic en las campañas de visitas actuales o anteriores. |

## Establecer variables personalizadas

Añadir datos personalizados al contexto de usuario.

### Uso

`rtp('set', 'customVar'[1-5], my_custom_value);`

| Parámetro | Opcional/Requerida | Tipo | Descripción |
|-----------------|-------------------|--------|-----------------|
| &#39;set&#39; | Obligatorio | Cadena | Acción de método. |
| customVar | Obligatorio | Cadena | Nombre de variable personalizado. |
| my_custom_value | Obligatorio | Cadena | Valor personalizado para guardar en variable personalizada en los índices 1-5. |

Nota: Las variables personalizadas se envían a RTP solo en la llamada de vista, por lo que se recomienda establecer variables personalizadas antes de llamar a la vista. De lo contrario, solo se enviará en la siguiente llamada de vista.

Restricciones de variables personalizadas

- La longitud de la variable personalizada no puede superar los 100 caracteres.
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
