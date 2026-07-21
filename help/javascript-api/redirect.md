---
title: Redirigir
description: Implemente la API de redireccionamiento de RTP para enviar visitantes segmentados a direcciones URL de destino mediante campos como ABM, organización, ubicación y segmentos, con ejemplos y sugerencias.
feature: Javascript
exl-id: bbf91245-42e5-47ae-a561-e522cc65ff49
TQID: https://experienceleague.adobe.com/frvGjN7DBJ1RJ3QFvWxo1qGiTNFmvyxi3H6FeynJHLU
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
  - id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 473
ht-degree: 8%

---

# Redirigir

Utilice la API de redireccionamiento de RTP para enviar audiencias segmentadas a una dirección URL de destino.

- Debe ser cliente de Web Personalization y tener la etiqueta [RTP implementada](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) en el sitio antes de usar la API de contexto de usuario.
- RTP no admite listas de cuentas con nombre de marketing basado en cuentas. Las listas ABM y el código solo pertenecen a las listas de cuentas cargadas (archivos CSV) administradas dentro de RTP.

## Uso

`rtp('send' , 'redirect' , 'field_name' , [ 'values_array' , '...' , '...' ] , 'www.redirect_url.com' , true/false )`

| Parámetro | Opcional/Requerida | Tipo | Descripción |
| --- | --- | --- | --- |
| &#39;enviar&#39; | Obligatorio | Cadena | Acción de método. |
| &#39;redirigir&#39; | Obligatorio | Cadena | Nombre del método. |
| field_name | Obligatorio | Cadena | Nombre de campo con el que se debe coincidir. Ejemplo: &quot;abm.name&quot; (consulte a continuación). |
| values_array | Obligatorio | Matriz | Lista de valores para comparar con el campo (no distingue mayúsculas de minúsculas). |
| redirect_url | Obligatorio | Cadena | URL de destino para redirigir a los visitantes que cumplieron la condición. |
| redirect_matched_visitors | Opcional | Booleano | Si el valor es True, se redirigirán los visitantes que cumplan la condición. Si es false, se redirigirá a los visitantes no coincidentes de la condición. Predeterminado: true. |

Las condiciones de redirección pueden utilizar la organización, el sector, las listas ABM, la ubicación, el ISP o segmentos coincidentes.

| Condición | Jerarquía de datos | Ejemplo |
| --- | --- | --- |
| Segmentos coincidentes (solo funciona después del primer clic) | matchedSegments.name | rtp( &#39;send&#39;, &#39;redirect&#39;, &#39;matchedSegments.name&#39;, [&#39;Fortune 1,000&#39;, &#39;Enterprise&#39;], &#39;<https://www.example.com>&#39;); |
| Segmentos coincidentes (solo funciona después del primer clic) | matchedSegments.id | rtp( &#39;send&#39;, &#39;redirect&#39;, &#39;matchedSegments.id&#39;, [106 , 107 , 190] , &#39;<https://www.example.com>&#39;); |
| Listas ABM | abm.name | rtp( &#39;send&#39;, &#39;redirect&#39;, &#39;abm.name&#39;, [&#39;top_key_accounts&#39;, &#39;active_customers&#39;], &#39;<https://www.example.com>&#39;); |
| Listas ABM | abm.code | rtp( &#39;send&#39;, &#39;redirect&#39;, &#39;abm.code&#39;, [13, 15], &#39;<https://www.example.com>&#39;); |
| Organizaciones | org | rtp(&#39;send&#39;, &#39;redirect&#39;, &#39;org&#39;, [&#39;ebay&#39;], &#39;<https://www.example.com>&#39;); |
| Ubicación | location.country | rtp( &#39;send&#39;, &#39;redirect&#39;, &#39;location.country&#39;, [&#39;United States&#39;], &#39;<https://www.example.com>&#39;); |
| Ubicación | location.state | rtp( &#39;send&#39;, &#39;redirect&#39;, &#39;location.state&#39;, [&#39;ca&#39;], &#39;<https://www.example.com>&#39;); |
| Ubicación | location.city | rtp( &#39;send&#39;, &#39;redirect&#39;, &#39;location.city&#39;, [&#39;San Mateo&#39;], &#39;<https://www.example.com>&#39;); |
| Industrias | industrias | rtp( &#39;enviar&#39;, &#39;redirigir&#39;, &#39;industrias&#39;, [&#39;Educación&#39;], &#39;<https://www.example.com>&#39;); |
| ISP | isp | rtp( &#39;enviar&#39;, &#39;redirigir&#39; , isp , [&#39;Falso&#39;], &#39;<https://www.example.com>&#39;); |

## Notas

- Para reducir la latencia de una redirección basada en Firmographics, como compañía, sector o ubicación, inserte el código de redirección antes de rtp(&#39;send&#39;, &#39;view&#39;) y rtp(&#39;get&#39;,&#39;campaign&#39;).
- Coloque el código de redirección inmediatamente después de la etiqueta rtp en el encabezado de la página.
- Optimice la carga del sitio web para mejorar la velocidad del redireccionamiento de JavaScript del lado del explorador.
- Evite las redirecciones automáticas. rtp incluye una protección que bloquea las llamadas de redirección cíclicas.

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

1. Anexe el parámetro a la dirección URL de destino, por ejemplo, &lt;www.marketo.com?rtp=redirect>.
1. Cree un segmento con el nombre &quot;Redirigido por RTP&quot;.
1. Utilice el parámetro &quot;Páginas específicas&quot; para segmentar visitantes que vean una página que contenga el parámetro.

![visitantes-redirigidos-de-seguimiento](assets/tracking-redirected-vistors.png)

## Definición de más de una condición con distintas direcciones URL de destino

La llamada de redireccionamiento admite varias llamadas. Utilice varias llamadas para combinar campos y crear condiciones con distintas direcciones URL y valores.

### Uso

`rtp('send', 'redirect', field_name, url_values_map);`

| Parámetro | Opcional/Requerida | Tipo | Descripción |
| --- | --- | --- | --- |
| &#39;enviar&#39; | Obligatorio | Cadena | Acción de método. |
| &#39;redirigir&#39; | Obligatorio | Cadena | Nombre del método. |
| field_name | Obligatorio | Cadena | Nombre de campo con el que se debe coincidir. Ejemplo: &quot;abm.name&quot; (consulte más arriba). |
| url_values_map | Obligatorio | Objeto | Asigne entre la URL de redireccionamiento y la lista de valores. Ejemplo:{&#39;<https://www.example.com>&#39; : [&#39;first_abm&#39;, &#39;second_abm&#39;]} |

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
