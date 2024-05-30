---
title: "Redirigir"
description: "Redirigir"
feature: Javascript
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '468'
ht-degree: 7%

---


# Redirigir

La API de redireccionamiento de RTP permite redirigir las audiencias segmentadas a una dirección URL de destino.

- Debe convertirse en cliente de Web Personalization y tener la [Etiqueta RTP implementada](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) en el sitio antes de utilizar la API de contexto de usuario.
- RTP no admite listas de cuentas con nombre de marketing basado en cuentas. Las listas ABM y el código solo pertenecen a las listas de cuentas cargadas (archivos CSV) administradas dentro de RTP.

## Uso

`rtp('send' , 'redirect' , 'field_name' , [ 'values_array' , '...' , '...' ] , 'www.redirect_url.com' , true/false )`

| Parámetro | Opcional/Requerida | Tipo | Descripción |
|---------------------------|-------------------|---------|-----------------------------|
| &#39;enviar&#39; | Obligatorio | Cadena | Acción de método. |
| &#39;redirigir&#39; | Obligatorio | Cadena | Nombre del método. |
| field_name | Obligatorio | Cadena | Nombre de campo con el que se debe coincidir. Ejemplo: &quot;abm.name&quot; (consulte a continuación). |
| values_array | Obligatorio | Matriz | Lista de valores para comparar con el campo (no distingue mayúsculas de minúsculas). |
| redirect_url | Obligatorio | Cadena | URL de destino para redirigir a los visitantes que cumplieron la condición. |
| redirect_matched_visitors | opcional | Booleano | Si el valor es True, se redirigirán los visitantes que cumplan la condición. Si es false, se redirigirá a los visitantes no coincidentes de la condición. Predeterminado: true. |

Organización, Sector, Listas ABM, Ubicación, ISP, Segmentos coincidentes

| Condición | Jerarquía de datos | Ejemplo |
|-------------------------------------------------|----------------------|------------------------------------------------------------------------------------------------------------------|
| Segmentos coincidentes (solo funciona después del primer clic) | matchedSegments.name | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;matchedSegments.name&#39; , [&#39;Fortune 1.000&#39; , &#39;Enterprise&#39;] , &#39;http://www.marketo.com&#39;); |
| Segmentos coincidentes (solo funciona después del primer clic) | matchedSegments.id | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;matchedSegments.id&#39; , [106 , 107 , 190] , &#39;http://www.marketo.com&#39;); |
| Listas ABM | abm.name | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;abm.name&#39; , [&#39;top_key_accounts&#39;, &#39;clientes_activos&#39;] , &#39;http://www.marketo.com&#39;); |
| Listas ABM | abm.code | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;abm.code&#39; , [13, 15] , &#39;http://www.marketo.com&#39;); |
| Organizaciones | org | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;org&#39;, [&#39;ebay&#39;], &#39;http://www.marketo.com&#39;); |
| Ubicación | location.country | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;location.country&#39; , [&#39;Estados Unidos&#39;], &#39;http://www.marketo.com&#39;); |
| Ubicación | location.state | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;location.state&#39;, [&#39;ca&#39;], &#39;http://www.marketo.com&#39;); |
| Ubicación | location.city | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;location.city&#39;, [&#39;San Mateo&#39;], &#39;http://www.marketo.com&#39;); |
| Industrias | industrias | rtp( &#39;enviar&#39;, &#39;redirigir&#39; , &#39;industrias&#39; , [&#39;Educación&#39;], &#39;http://www.marketo.com&#39;); |
| ISP | isp | rtp( &#39;enviar&#39;, &#39;redirigir&#39; , isp , [&#39;Falso&#39;], &#39;http://www.marketo.com&#39;); |


## Notas

- Si la regla/condición de redireccionamiento se basa en Firmographics (empresa, sector, ubicación), puede insertar el código de redireccionamiento antes de rtp(&#39;send&#39;, &#39;view&#39;) y de rtp(&#39;get&#39;,&#39;campaign&#39;) para reducir la latencia.
- El redireccionamiento mediante JavaScript es un redireccionamiento del lado del navegador y depende de la carga y optimización del sitio web para alcanzar la velocidad máxima.
- La práctica recomendada es establecer el código de redirección justo después de la etiqueta rtp y colocarlo en el encabezado.
- Asegúrese de que no está ejecutando una redirección automática (hay una red de seguridad en rtp para bloquear las llamadas de redirección cíclica).

```html
<!DOCTYPE html>
<html lang="en-US">
<head>
<!-- RTP tag --> 
<script type='text/javascript'>

// This tag needs to be replaced with your account tag
(function(c,h,a,f,i){c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
c[a].a=i;var g=h.createElement("script");g.async=true;g.type="text/javascript";
g.src=f+'?rh='+c.location.hostname+'&aid='+i;var b=h.getElementsByTagName("script")[0];b.parentNode.insertBefore(g,b);
})(window,document,"rtp","//xyz.marketo.com/rtp-api/v1/rtp.js","xyz");
 
// START REDIRECT EXAMPLE 
//   - Using a helper redirect function
//   - Redirect based on named account
rtp('send','redirect','org', ['microsoft'],'http://www.marketo.com');
 
// Redirect based on named account list (ABM)
rtp('send','redirect','abm.name', {
    // Redirect visitors that match 'first_abm' list to www.marketo.com
    'http://www.marketo.com' : ['first_abm'],
    // Redirect visitors that match 'second_abm' list to blog.marketo.com
    'http://blog.marketo.com' : ['second_abm'] 
});
// END REDIRECT EXAMPLE
rtp('send','view');
rtp('get','campaign');
</script>
<!-- End of RTP tag -->
```

## Cómo redirigir visitantes rastreados

1. Anexe un parámetro al final de la dirección URL de destino, por ejemplo www.marketo.com?rtp=redirect.
1. Cree un segmento llamado: &quot;Redirigido por RTP&quot;
1. Utilice el parámetro &quot;Páginas específicas&quot; para segmentar los visitantes que vean cualquier página con el parámetro que se muestra a continuación.

![tracking-redirect-visitantes](assets/tracking-redirected-vistors.png)

## Definición de más de una condición con distintas direcciones URL de destino

La llamada de redireccionamiento admite varias llamadas. Esto permite redirigir con varios campos y crear condiciones complejas con diferentes direcciones URL y valores.

### Uso

`rtp('send', 'redirect', field_name, url_values_map);`

| Parámetro | Opcional/Requerida | Tipo | Descripción |
|---|---|---|---|
| &#39;enviar&#39; | Obligatorio | Cadena | Acción de método. |
| &#39;redirigir&#39; | Obligatorio | Cadena | Nombre del método. |
| field_name | Obligatorio | Cadena | Nombre de campo con el que se debe coincidir. Ejemplo: &quot;abm.name&quot; (consulte más arriba). |
| url_values_map | Obligatorio | Objeto | Asigne entre la URL de redireccionamiento y la lista de valores. Ejemplo:{&#39;http://marketo.com&#39; : [&#39;first_abm&#39;, &#39;second_abm&#39;]} |


#### Ejemplo

```javascript
rtp('send','redirect','abm.name', {
    // Redirect visitors that match 'first_abm' list to www.marketo.com
    'http://www.marketo.com' : ['first_abm'],
    // Redirect visitors that match 'second_abm' list to blog.marketo.com
    'http://blog.marketo.com' : ['second_abm']
});
rtp('send','redirect','org', {
    // Redirect visitors from 'Microsoft' to www.marketo.com/enterprise
    'http://www.marketo.com/enterprise' : ['microsoft']
});
```
