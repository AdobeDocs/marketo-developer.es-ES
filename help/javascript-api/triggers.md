---
title: Activadores
description: Utilice déclencheur RTP en Web Personalization para ejecutar funciones en estado rtp, incluido userContextReady, con sintaxis, parámetros y un ejemplo de ubicación.
feature: Javascript
exl-id: 588836fa-1e4d-41f3-aec5-5cd17eb16071
TQID: 'https://experienceleague.adobe.com/yTz9i4bnD4I0PDAmpnjdD1okYJzd40wriA-2ZzO5OMM'
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
    internal-label: Marketo Engage
feature_v2:
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
    internal-label: Implementation
  - id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
    internal-label: Personalization
  - id: dda1332a-c3e0-583f-9d9b-15f1934e0ad3
    internal-label: Javascript
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
    internal-label: Implementation
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
    internal-label: Personalization
source-git-commit: 5620f050ba834be3f6648650b5cc7d781ea394bf
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 8%
---
# Activadores

Déclencheur ejecuta funciones cuando el objeto `rtp` global alcanza un estado especificado.

Debe ser cliente de Web Personalization y tener la etiqueta [RTP implementada](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) en el sitio antes de usar la API de contexto de usuario.

## Uso

`rtp('triggerName', function_to_trigger);`

| Parámetro | Opcional/Requerida | Tipo | Descripción |
| --- | --- | --- | --- |
| &#39;triggerName&#39; | Obligatorio | Cadena | Nombre del método. |
| function_to_déclencheur | Obligatorio | Función | Función a déclencheur. |

### Déclencheur Listo para Contexto de Usuario

El déclencheur `userContextReady` llama a una función cuando el objeto `rtpUserContext` global está listo. En el siguiente ejemplo se establece una variable personalizada basada en la ubicación del usuario.

```javascript
rtp('userContextReady', function() {
    if (rtpUserContext.location.state == 'CA') {
        rtp('set', 'custom1', 'productA');
    }
});
```
