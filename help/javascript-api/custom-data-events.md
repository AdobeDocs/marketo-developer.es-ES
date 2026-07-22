---
title: Eventos de datos personalizados
description: Envíe eventos personalizados con la API de JavaScript de RTP para Web Personalization, con parámetros, datos de cadena o matriz de hasta cuatro elementos y déclencheur basados en clics.
feature: Javascript
exl-id: ef7cab9c-3bd0-450e-9247-9324b1e6f9ab
TQID: https://experienceleague.adobe.com/oWDmtMF94xG5HYXeTwkx5zF9PWo98bpwoVB6kAKLYDo
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
  - id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 241
ht-degree: 3%

---

# Eventos de datos personalizados

Utilice este método para enviar eventos personalizados para el seguimiento y la personalización en tiempo real. Puede enviar datos de terceros o almacenar en déclencheur un evento personalizado basado en el comportamiento del visitante.

Cada evento de datos personalizados se cuenta una vez durante la sesión del visitante.

Debe ser cliente de Web Personalization y tener la etiqueta [RTP implementada](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) en el sitio antes de usar la API de contexto de usuario.

| Parámetro | Opcional/Requerida | Tipo | Descripción |
| --- | --- | --- | --- |
| `send` | Obligatorio | Cadena | Acción de método. |
| `event` | Obligatorio | Cadena | Nombre del método. |
| `customData` | Obligatorio | Cadena o matriz | Datos personalizados. |

## Ejemplos

### Enviar evento con cadena para datos personalizados

```javascript
var customData = {value: 'MyEvent'};
rtp('send', 'event', customData);
```

### Enviar evento con la matriz de cadenas para los datos personalizados

La matriz de datos personalizada puede contener hasta cuatro elementos. Para enviar más de cuatro elementos, llame repetidamente a la API Send Event sin más de cuatro elementos en cada llamada.

```javascript
var customData = {value: ['MyEvent', 'download - example whitepaper']};
rtp('send', 'event', customData);
```

### Enviar evento basado en el clic del botón

En este ejemplo se envía un evento de datos personalizados cuando un visitante selecciona el botón para descargar un documento técnico específico. RTP puede utilizar el evento para segmentar a estos visitantes en tiempo real.

El sitio web puede mostrar una campaña personalizada después de dos clics más. Por ejemplo, la campaña puede presentar otro fragmento de contenido relacionado con el documento técnico descargado.

```html
<button id="download-whitepaper" onclick="rtp('send', 'event', {value :'download - example whitepaper'})">Download</button>
```
