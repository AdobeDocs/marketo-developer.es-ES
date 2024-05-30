---
title: "Déclencheur"
description: "Déclencheur"
feature: Javascript
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '80'
ht-degree: 11%

---


# Desencadenadores

Agrega la capacidad a las funciones de déclencheur en cierto estado del objeto rtp global.

Debe ser cliente de Personalización web y tener la [Etiqueta RTP implementada](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) en el sitio antes de utilizar la API de contexto de usuario.

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
