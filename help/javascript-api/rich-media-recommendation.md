---
title: Recomendación de medios enriquecidos
description: Recomendación de medios enriquecidos
feature: Javascript
exl-id: ee92e46d-e529-40a2-a0d0-ee233916f004
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '812'
ht-degree: 4%

---

# Recomendación de medios enriquecidos

Las siguientes etiquetas y llamadas de API deben configurarse en la página que desea que muestre la plantilla Recomendación de medios enriquecidos.

1. En el encabezado de la página
   1. Tener instalada la etiqueta RTP
   1. Añada la llamada de GET a la página para rellenar las recomendaciones.
   1. Añada la llamada SET para configurar la plantilla
1. En el cuerpo de la página
   1. Coloque la etiqueta de plantilla (clase div) en la ubicación donde desee que aparezca la plantilla

Encontrará más información [aquí](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/predictive-content/enabling-predictive-content/enable-predictive-content-for-web-rich-media).

## Etiqueta de plantilla

| Atributo | Opcional/Requerida | Descripción |
|---|---|---|
| clase | Obligatorio | Especifique que este elemento del HTML div es un div de recomendación RTP. |
| data-rtp-template-id | Obligatorio | ID de la plantilla. Esto determina la alineación de la recomendación. Utilice &quot;template1&quot; para la alineación horizontal, &quot;template2&quot; para la alineación vertical o &quot;template3&quot; para la alineación vertical que incluya únicamente el título y la descripción. El script inserta la plantilla coincidente en estos `div.Permissible` valores: template1, template2, template3. |

### Ejemplos

Para mostrar las recomendaciones en alineación horizontal, utilice &quot;template1&quot;.

```html
<div class="RTP_RCMD2" data-rtp-template-id="template1"></div>
```

Para mostrar las recomendaciones en alineación vertical, utilice &quot;template2&quot;.

```html
<div class="RTP_RCMD2" data-rtp-template-id="template2"></div>
```

Para mostrar las recomendaciones en alineación vertical solo con título y descripción, utilice &quot;template3&quot;.

```html
<div class="RTP_RCMD2" data-rtp-template-id="template3"></div>
```

Vea capturas de pantalla de las alineaciones de plantilla [aquí](#example_of_rich_media_recommendation_template_1).

## Rellenar recomendación

Este método rellena todos los medios enriquecidos `<divs>` de la página con recomendaciones.

### Uso

`rtp('get', 'rcmd', 'richmedia');`

| Parámetro | Opcional/Requerida | Tipo | Descripción |
|---|---|---|---|
| &#39;get&#39; | Obligatorio | Cadena | Acción de método. |
| &#39;rcmd&#39; | Obligatorio | Cadena | Nombre del método. |
| &#39;richmedia&#39; | Obligatorio | Cadena | Nombre del submétodo. |


## Cambiar configuración de plantilla

Este método cambia la configuración predeterminada de la plantilla.

Nota: Cuando se utiliza este método, se debe llamar antes de llamar a rtp(&#39;get&#39;,&#39;rcmd&#39;, &#39;richmedia&#39;);

### Uso

`rtp('set', 'rcmd', 'richmedia', 'template_id', conf_obj);`

| Parámetro | Opcional/Requerida | Tipo | Descripción |
|---|---|---|---|
| &#39;set&#39; | Obligatorio | Cadena | Acción de método. |
| &#39;rcmd&#39; | Obligatorio | Cadena | Nombre del método. |
| &#39;richmedia&#39; | Obligatorio | Cadena | Nombre del submétodo. |
| template_id | opcional | Cadena | El ID de plantilla para los cambios de configuración. Se utiliza para especificar cambios de configuración solo para una plantilla. |
| conf_obj | Obligatorio | Objeto | La nueva configuración. El objeto contiene todas las configuraciones como par clave/valor. |


### Ejemplos

Este fragmento de código cambia el texto del título de una plantilla.

```javascript
rtp("set", "rcmd", "richmedia","template1",
    {
        "rcmd.title.text": "RECOMMENDED CONTENT"
    }
);
```

Este fragmento de código muestra la definición de categorías con varias configuraciones para una plantilla.

```javascript
rtp("set", "rcmd", "richmedia",
    {
        "template1": 
        {
            "rcmd.title.text": "RECOMMENDED CONTENT",
            "rcmd.general.font.family": "arial",
            "category":
            [
                "webinar",
                "blog posts",
                "pricing_page_category",
                "product_a_category"
            ]
        }
    }
);
```

NOTA: Utilice &quot;category&quot; para filtrar el contenido que se muestra en el resultado de las recomendaciones de contenido predictivo. Para aplicar contenido predictivo a todos los fragmentos de contenido habilitados, deje vacía la &quot;categoría&quot;. Si desea recomendar solo contenido específico para la salida en la plantilla de medios enriquecidos, agregue una categoría para el contenido en la página Definir contenido y asocie esa categoría dentro del código de plantilla de recomendación. Categorizar el contenido relevante según las secciones del sitio web (productos o soluciones).

Este fragmento de código muestra la configuración de varias plantillas para una plantilla.

```javascript
rtp("set", "rcmd", "richmedia",
    {
        "template1":
        {
            "rcmd.title.text": "RECOMMENDED CONTENT",
            "rcmd.general.font.family": "arial"
        }
    }
);
```

#### Propiedades de configuración

| Configuración | Ejemplo | Descripción |
|---|---|---|
| rcmd.general.font.family | &quot;rcmd.general.font.family&quot; : &quot;arial&quot; | Cambia la familia de fuentes para todo el texto de la plantilla. Esta propiedad admite todos los valores CSS por tipo de explorador. Es posible utilizar una familia de fuentes personalizada si existe en la página. |
| rcmd.content.background.color | &quot;rcmd.content.background.color&quot; : &quot;black&quot; | Cambia el color de fondo de los cuadros interiores de la plantilla. Esta propiedad admite todos los valores CSS por tipo de explorador. |
| rcmd.title.text | &quot;rcmd.title.text&quot; : &quot;CONTENIDO RECOMENDADO&quot; | Cambia el título de la plantilla. |
| rcmd.title.background.color | &quot;rcmd.title.background.color&quot; : &quot;blue&quot; | Cambia el color de fondo del cuadro de título. Esta propiedad admite todos los valores de color css (nombre de color, rgb, ...) |
| rcmd.title.font.size | &quot;rcmd.title.font.size&quot; : &quot;26 px&quot; | Cambia el tamaño de fuente del título. La propiedad admite todos los tamaños de fuente posibles del valor CSS (px, em, ...) |
| rcmd.title.font.color | &quot;rcmd.title.font.color&quot; : &quot;blanco&quot; | Cambia el color de fuente del título. Esta propiedad admite todos los valores de color de fuente (rgb, hex, ...) |
| rcmd.description.font.color | &quot;rcmd.description.font.color&quot; : &quot;white&quot; | Cambia el color de fuente de la descripción. Esta propiedad admite todos los valores de color de fuente (rgb, hex, ...) |
| rcmd.cta.background.color | &quot;rcmd.cta.background.color&quot; : &quot;green&quot; | Cambia el color de fondo del botón. Esta propiedad admite todos los valores de color css (nombre de color, rgb, ...) |
| rcmd.cta.font.color | &quot;rcmd.cta.font.color&quot; : &quot;rgb(90, 84, 164)&quot; | Cambia el color de fuente del botón. Esta propiedad admite todos los valores de color de fuente (rgb, hex, ...) |
| rcmd.cta.text | &quot;rcmd.cta.text&quot; : &quot;Push&quot; | Cambia el texto del botón. El texto es el mismo para todos los botones. |
| categoría | &quot;categoría&quot; : [&quot;una categoría&quot;] | Cambia la categoría de recomendación que admite esta plantilla. La plantilla solo muestra las recomendaciones con una de las categorías establecidas por esta configuración. |


Nota: La compatibilidad con la configuración puede cambiar por plantilla.

#### Ejemplo básico

Este ejemplo tiene una plantilla con tres recomendaciones. Copie este ejemplo en una página de HTML y, a continuación, reemplace la etiqueta RTP por la etiqueta.

```html
<!DOCTYPE>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>RTP recommendation</title>
<!-- RTP tag --> 
<script type='text/javascript'>

// This tag needs to be replaced with your account tag
(function(c,h,a,f,i,e){c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
c[a].a=i;c[a].e=e;var g=h.createElement("script");g.async=true;g.type="text/javascript";
g.src=f+'?aid='+i;var b=h.getElementsByTagName("script")[0];b.parentNode.insertBefore(g,b);
})(window,document,"rtp","//example.rtp.com/rtp-api/v1/rtp.js","account_id");

// Send page view (required by  the recommendation)
rtp('send','view');
// Populate recommendation
rtp('get','rcmd', 'richmedia');
</script>
<!-- End of RTP tag -->
</head>
<body>
<div class="RTP_RCMD2" data-rtp-template-id="template1"></div>
</body>
</html>
```

#### Ejemplo avanzado

Este ejemplo tiene una plantilla con tres recomendaciones. El título de la plantilla es &quot;CONTENIDO RECOMENDADO&quot; y el texto del botón será &quot;Más información&quot;. Copie este ejemplo en una página de HTML y, a continuación, reemplace la etiqueta RTP por la etiqueta.

```html
<!DOCTYPE>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>RTP recommendation</title>
<!-- RTP tag --> 
<script type='text/javascript'>

// This tag needs to be replaced with your account tag
(function(c,h,a,f,i,e){c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
c[a].a=i;c[a].e=e;var g=h.createElement("script");g.async=true;g.type="text/javascript";
g.src=f+'?aid='+i;var b=h.getElementsByTagName("script")[0];b.parentNode.insertBefore(g,b);
})(window,document,"rtp","//example.rtp.com/rtp-api/v1/rtp.js","account_id");

// Send page view (required by  the recommendation)
rtp('send','view');
// Populate the recommendation zone
rtp('get', 'campaign',true);
// Change template configuration
rtp('set', 'rcmd', 'richmedia',
    {
        template1 :
        {
            "rcmd.title.text" : "RECOMMENDED CONTENT",
            "rcmd.cta.text" : "Read More"
        }
    }
);
// Populate recommendation
rtp('get','rcmd', 'richmedia');
</script>
<!-- End of RTP tag -->
</head>
<body>
<div class="RTP_RCMD2" data-rtp-template-id="template1"></div>
</body>
</html>
```

#### Ejemplo de #1 de plantilla de recomendación de medios enriquecidos

**Nombre**: plantilla1 **Descripción**: contenido horizontal que incluye imagen, título y descripción y botón de llamada a la acción.

![Plantilla de medios enriquecidos](assets/rich-media-template1.png)

#### Ejemplo de #2 de plantilla de recomendación de medios enriquecidos

**Nombre**: plantilla2 **Descripción**: contenido vertical que incluye imagen, título y descripción y botón de llamada a la acción.

![Plantilla de medios enriquecidos](assets/rich-media-template2.png)

#### Ejemplo de #3 de plantilla de recomendación de medios enriquecidos

**Nombre**: plantilla3 **Descripción**: contenido vertical que solo incluye título y descripción. Al pasar el ratón por encima, el encabezado cambia de color y aparece un hipervínculo a la dirección URL de contenido. La descripción también vincula al contenido sin cambiar el color. ![Plantilla de medios enriquecidos](assets/rich-media-template3.png)
