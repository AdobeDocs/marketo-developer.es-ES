---
title: Triggers
description: Triggers
feature: Javascript
exl-id: 588836fa-1e4d-41f3-aec5-5cd17eb16071
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '80'
ht-degree: 13%

---

# Triggers

Agrega la capacidad a las funciones de déclencheur en cierto estado del objeto rtp global.

Debe ser cliente de Web Personalization y tener la etiqueta [RTP implementada](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) en el sitio antes de usar la API de contexto de usuario.

## Uso

`rtp('triggerName', function_to_trigger);`

| Parámetro | Opcional/Requerida | Tipo | Descripción |
|---------------------|-------------------|----------|----------------------|
| &#39;triggerName&#39; | Obligatorio | Cadena | Nombre del método. |
| function_to_déclencheur | Obligatorio | Función | Función a déclencheur. |

### Déclencheur Listo para Contexto de Usuario

Establece una variable personalizada basada en la ubicación del usuario. Se llama a esta función cuando el objeto global &quot;rtpUserContext&quot; está listo.

```javascript
rtp('userContextReady', function() {
    if (rtpUserContext.location.state == 'CA') {
        rtp('set', 'custom1', 'productA');
    }
});
```
