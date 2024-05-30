---
title: "Eventos de datos personalizados"
description: "API de eventos de datos personalizados"
feature: Javascript
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 4%

---


# Eventos de datos personalizados

Este método envía eventos personalizados para el seguimiento y la personalización en tiempo real. Se puede utilizar para enviar datos de terceros o para almacenar en déclencheur su propio evento personalizado en función del comportamiento del visitante. Los eventos de datos personalizados se cuentan una vez en la sesión del visitante.

Debe convertirse en cliente de Web Personalization y tener la [Etiqueta RTP implementada](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) en el sitio antes de utilizar la API de contexto de usuario.

| Parámetro | Opcional/Requerida | Tipo | Descripción |
|---|---|---|---|
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

La matriz de datos personalizada puede contener un máximo de cuatro elementos.  Si debe enviar más de cuatro elementos, llame repetidamente a la API Enviar evento (con un máximo de cuatro elementos) hasta que se envíen todos los elementos.

```javascript
var customData = {value: ['MyEvent', 'download - example whitepaper']};
rtp('send', 'event', customData);
```

### Enviar evento basado en el clic del botón

Marketo personaliza el contenido de su sitio web para los visitantes que descargan un documento técnico específico. Para ello, registran el clic del visitante en el botón de descarga del documento técnico, que envía un evento de datos personalizados. RTP segmenta en tiempo real a todos los visitantes que hicieron clic en el botón del documento técnico de descarga, mostrando a cada visitante una campaña personalizada que ofrece 2 clics más tarde. Esto se logra mostrando otro fragmento de contenido relacionado con el documento técnico descargado.

```html
<button id="download-whitepaper" onclick="rtp('send', 'event', {value :'download - example whitepaper'})">Download</button>
```
