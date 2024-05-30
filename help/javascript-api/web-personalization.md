---
title: "Personalización web"
description: "Personalización web"
feature: Web Personalization, Javascript
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '401'
ht-degree: 5%

---


# Personalización web

La API de JavaScript de personalización web amplía la capacidad de personalización automatizada de la plataforma. Permite el seguimiento de eventos y la personalización dinámica de una página web. Funciones adicionales: [Eventos de datos personalizados](custom-data-events.md), [Contenido dinámico](web-personalization.md), [Obtener datos del visitante](get-visitor-data.md), [Excluir etiqueta para bots específicos](#exclude_tag_for_specific_bots).

- Debe convertirse en cliente de Web Personalization y tener la [Etiqueta RTP implementada](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) en el sitio antes de utilizar la API de contexto de usuario.
- RTP no admite listas de cuentas con nombre de marketing basado en cuentas. Las listas ABM y el código solo pertenecen a las listas de cuentas cargadas (archivos CSV) administradas dentro de RTP.

## Configuración de etiquetas

La etiqueta RTP debe insertarse en el encabezado de la página personalizada.

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

Se llama automáticamente a este método en el nivel de etiqueta para establecer el ID de cuenta correspondiente. Puede establecer el ID de cuenta cuando desee dividir entre dominios diferentes.

| Parámetro | Opcional/Requerida | Tipo | Descripción |
|--------------|-------------------|--------|--------------|
| &#39;setAccount&#39; | Obligatorio | Cadena | Nombre del método. |
| accountId | Obligatorio | Cadena | ID de cuenta. |


```javascript
var accountId = '561-HYG-937';
rtp('setAccount', accountId);
```

## Funciones de envío de eventos

Este método envía un evento de vista, que se utiliza para el seguimiento de páginas. En el ejemplo siguiente, la dirección URL de la página actual se rastrea como una vista de página del visitante.

Al pasar el parámetro opcional &quot;page&quot; en este método, se puede anular la página actual.

| Parámetro | Opcional/Requerida | Tipo | Descripción |
|-----------|-------------------|--------|---------------------------------|
| &#39;enviar&#39; | Obligatorio | Cadena | Acción de método. |
| &#39;vista&#39; | Obligatorio | Cadena | Nombre del método. |
| Página | opcional | Cadena | Ruta relativa o dirección URL de página completa. |


```javascript
// Example for Default Page
rtp('send', 'view');

// Example for Overriding Default Page
var page = 'my-page?param=1';
rtp('send', 'view', page);
```

## Excluir etiqueta para bots específicos (agentes de usuario)

Para excluir exploradores específicos del envío de datos a la plataforma de personalización web (en el caso de bots identificados), agregue la siguiente instrucción IF al script de la etiqueta.

En el ejemplo de código siguiente, &quot;Googlebot|msnbot&quot; se utiliza como ejemplos de bots para excluir de las actividades de personalización web.

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

Descripción de JavaScript que se añade a un sitio web al utilizar la personalización web y el contenido predictivo.

### JavaScript principal/dependiente

| Nombre | Descripción | Control |
|---------------------------|-------------|--------------------------------------------------------|
| rtp.js | - | Controlado por Marketo |
| jquery.min.js | Versión 1.8.3 | Se puede deshabilitar poniéndose en contacto con Asistencia al cliente de Marketo |
| jquery-custom-ui-min.js | Versión 1.9.2 | Se puede deshabilitar poniéndose en contacto con Asistencia al cliente de Marketo |
| query-ui-1.8.17-dialog.js | Versión 1.9.2* | Se puede deshabilitar poniéndose en contacto con Asistencia al cliente de Marketo |


*Se utiliza solo si falta el cuadro de diálogo de la IU de jQuery

### JavaScript bajo demanda

| Nombre | Descripción | Control |
|-------------------------|-----------------------------------------------------------------------|-----------------------|
| ga-integration-2.0.1.js | Se utiliza si la integración de Google Analytics/Facebook/SiteCatalyst está activada | Controlado por Marketo |
| insightera-bar-2.1.js | Se utiliza si la barra de recomendaciones de contenido predictivo está habilitada | Controlado por Marketo |
| froogaloop2.min.js | Se utiliza si el seguimiento de contenido está habilitado y el reproductor Vimeo existe en la página | - |
| iframe-api-v1.js | Se utiliza si el seguimiento de contenido está habilitado y el reproductor de YouTube existe en la página | - |

