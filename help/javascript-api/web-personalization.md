---
title: Personalización web
description: Guía de la API de JavaScript de Personalization web y la etiqueta RTP, que cubre los eventos de vista de página, la configuración de cuenta, las exclusiones de bots y los scripts principales y bajo demanda
feature: Web Personalization, Javascript
exl-id: b2c26b28-e9bf-4faf-8b6e-c102f41aeaa1
TQID: https://experienceleague.adobe.com/yplunKmgjOJ7gJTA2TDc9cfJXyXbrVWuM-NdVbDMN4A
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: e2290edd-b061-4880-9d79-dee306cf5aa9id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
subfeature_v2: id: cdd4e0f6-e87e-453f-88ee-2ee54a7de272
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 435
ht-degree: 6%

---

# Personalización web

La API de JavaScript de Web Personalization rastrea eventos y personaliza páginas web de forma dinámica. Amplía las capacidades de personalización automatizada de la plataforma.

Las funciones relacionadas incluyen [Eventos de datos personalizados](custom-data-events.md), [Contenido dinámico](web-personalization.md), [Obtener datos del visitante](get-visitor-data.md) y [Excluir etiqueta para bots específicos](#exclude_tag_for_specific_bots).

- Debe ser cliente de Web Personalization y tener la etiqueta [RTP implementada](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) en el sitio antes de usar la API de contexto de usuario.
- RTP no admite listas de cuentas con nombre de marketing basado en cuentas. Las listas ABM y el código solo pertenecen a las listas de cuentas cargadas (archivos CSV) administradas dentro de RTP.

## Configuración de etiquetas

Inserte la etiqueta RTP en el encabezado de cada página personalizada.

```javascript
<!-- RTP tag -->
<script type='text/javascript'>
(function(c,h,a,f,e,i){c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
c[a].p=e;c[a].a=i;var g=h.createElement("script");g.async=true;g.type="text/javascript";
g.src=f;var b=h.getElementsByTagName("script")[0];b.parentNode.insertBefore(g,b)})
(window,document,"rtp","[rtp-js-cdn-url]","[pod-url]","[accountId]");
</script>
<!-- End of RTP tag -->
```

## Configuración de cuenta

La etiqueta llama automáticamente a este método para establecer el ID de cuenta correspondiente. Establezca el ID de cuenta explícitamente cuando desee utilizar cuentas diferentes para dominios diferentes.

| Parámetro | Opcional/Requerida | Tipo | Descripción |
| --- | --- | --- | --- |
| &#39;setAccount&#39; | Obligatorio | Cadena | Nombre del método. |
| accountId | Obligatorio | Cadena | ID de cuenta. |

```javascript
var accountId = '561-HYG-937';
rtp('setAccount', accountId);
```

## Funciones de envío de eventos

Este método envía un evento de vista para el seguimiento de páginas. La primera llamada del siguiente ejemplo rastrea la dirección URL de la página actual como una vista de página del visitante.

Pase el parámetro opcional &quot;page&quot; para anular la página actual, como se muestra en la segunda llamada.

| Parámetro | Opcional/Requerida | Tipo | Descripción |
| --- | --- | --- | --- |
| &#39;enviar&#39; | Obligatorio | Cadena | Acción de método. |
| &#39;vista&#39; | Obligatorio | Cadena | Nombre del método. |
| Página | Opcional | Cadena | Ruta relativa o dirección URL de página completa. |

```javascript
// Example for Default Page
rtp('send', 'view');

// Example for Overriding Default Page
var page = 'my-page?param=1';
rtp('send', 'view', page);
```

## Excluir etiqueta para bots específicos (agentes de usuario)

Para evitar que bots identificados envíen datos a la plataforma Web Personalization, agregue la siguiente instrucción `if` al script de etiquetas.

Este ejemplo excluye los agentes de usuario &quot;Googlebot|msnbot&quot; de las actividades de Web Personalization.

```javascript
<!-- RTP tag -->
<script type='text/javascript'>
if(navigator.userAgent.match(/.(Googlebot|msnbot)./gi) == null){
    (function(c,h,a,f,i){c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
    c[a].a=i;var g=h.createElement("script");g.async=true;g.type="text/javascript";
    g.src=f+'?rh='+c.location.hostname+'&aid='+i;var b=h.getElementsByTagName("script")[0];b.parentNode.insertBefore(g,b);
})(window,document,"rtp","//[cdn-pod-X-url]/rtp-api/v1/rtp.js","[accountId]");

    rtp('send','view');
    rtp('get', 'campaign', true);
}
</script>
<!-- End of RTP tag -->
```

## Llamadas de JavaScript explicadas

En las tablas siguientes se describe el JavaScript agregado a un sitio web que utiliza Personalization web y contenido predictivo.

### JavaScript principal/dependiente

| Nombre | Descripción | Control |
| --- | --- | --- |
| rtp.js | - | Controlado por Marketo |
| jquery.min.js | Versión 1.8.3 | Se puede deshabilitar poniéndose en contacto con Asistencia al cliente de Marketo |
| jquery-custom-ui-min.js | Versión 1.9.2 | Se puede deshabilitar poniéndose en contacto con Asistencia al cliente de Marketo |
| query-ui-1.8.17-dialog.js | Versión 1.9.2* | Se puede deshabilitar poniéndose en contacto con Asistencia al cliente de Marketo |

*Se utiliza solo si falta el cuadro de diálogo de la interfaz de usuario de jQuery.

### On Demand JavaScript

| Nombre | Descripción | Control |
| --- | --- | --- |
| ga-integration-2.0.1.js | Se utiliza si la integración de Google Analytics/Facebook/SiteCatalyst está habilitada | Controlado por Marketo |
| insightera-bar-2.1.js | Se utiliza si la barra de recomendaciones de contenido predictivo está habilitada | Controlado por Marketo |
| froogaloop2.min.js | Se utiliza si el seguimiento de contenido está habilitado y el reproductor Vimeo existe en la página | - |
| iframe-api-v1.js | Se utiliza si el seguimiento de contenido está habilitado y el reproductor de YouTube existe en la página | - |
