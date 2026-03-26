---
title: Activadores
description: Utilice déclencheur RTP en Web Personalization para ejecutar funciones en estado rtp, incluido userContextReady, con sintaxis, parámetros y un ejemplo de ubicación.
feature: Javascript
exl-id: 588836fa-1e4d-41f3-aec5-5cd17eb16071
source-git-commit: 6145067629ce78175af3b7464807a0fa100c7b57
workflow-type: tm+mt
source-wordcount: '116'
ht-degree: 8%

---

# Activadores

Agrega la capacidad a las funciones de déclencheur en cierto estado del objeto rtp global.

Debe ser cliente de Web Personalization y tener la etiqueta [RTP implementada](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) en el sitio antes de usar la API de contexto de usuario.

## Uso

`rtp('triggerName', function_to_trigger);`

| Parámetro | Opcional/Requerida | Tipo | Descripción |
| --- | --- | --- | --- |
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
