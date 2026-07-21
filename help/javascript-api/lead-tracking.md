---
title: Seguimiento de leads
description: Obtenga información sobre cómo incrustar Marketo Munchkin JavaScript, rastrear visitas y clics, administrar posibles clientes conocidos frente a anónimos, cookies entre dominios y desactivar campañas inteligentes.
feature: Munchkin Tracking Code, Javascript
exl-id: 7ece5133-9d32-4be3-a940-4ac0310c4d8b
TQID: https://experienceleague.adobe.com/nGUcLLgL9X7PBKf2E5IzppDj8e-SyEtxmkQaESd90mE
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: a7170d27-32ab-462b-a333-269abc654483
  - id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
  - id: d1d0a9cd-295d-4976-8c39-ddae266f240e
subfeature_v2:
  - id: d0251300-e25f-466f-9856-7e11ce8fa7aa
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 720
ht-degree: 0%

---

# API de seguimiento de posibles clientes

Munchkin JavaScript de Marketo rastrea las visitas a la página y los clics en vínculos en páginas de aterrizaje de Marketo y páginas web externas. Marketo registra estas interacciones como actividades de &quot;Visitar página web&quot; y &quot;Vínculo en el que se hizo clic en la página web&quot;.

Utilice las actividades de déclencheur y filtros para campañas inteligentes y listas inteligentes.

## Incrustación del código

La instancia de Marketo proporciona fragmentos de código preconfigurados para rastrear la actividad desde páginas externas. El uso del código incrustado se rige por este [acuerdo de licencia](../munchkin-license.pdf).

Hay tres tipos de código de seguimiento disponibles:

1. Simple: se carga sincrónicamente.
1. Asíncrona: se carga asincrónicamente.
1. jQuery asincrónico: se carga asincrónicamente y requiere que jQuery se cargue primero.

Utilice el código de seguimiento asincrónico para incrustar Munchkin en páginas externas. Para obtener la tasa de éxito de ejecución más alta posible, coloque el código en el elemento `<head>` de cada página.

Algunos sistemas de gestión de contenido pueden tener métodos o restricciones específicos al incrustar secuencias de comandos arbitrarias.

La página final debe incluir un código similar al siguiente ejemplo en el elemento `<head>` del documento de HTML:

```html
<head>
    <script type="text/javascript">
    (function() {
        var didInit = false;
        function initMunchkin() {
            if(didInit === false) {
                didInit = true;
                Munchkin.init('CHANGE-ME');
            }
        }
        var s = document.createElement('script');
        s.type = 'text/javascript';
        s.async = true;
        s.src = '//munchkin.marketo.net/munchkin.js';
        s.onreadystatechange = function() {
            if (this.readyState == 'complete' || this.readyState == 'loaded') {
                initMunchkin();
            }
        };
        s.onload = initMunchkin;
        document.getElementsByTagName('head')[0].appendChild(s);
        })();
    </script>
    ...
</head>
```

## Comportamiento de Munchkin

De forma predeterminada, Marketo Munchkin realiza las siguientes acciones cuando se carga una página:

1. Comprueba si el explorador actual tiene una cookie de Munchkin y crea una en caso necesario.
1. Envía un evento &quot;Visitar página web&quot; a la instancia de Marketo designada mediante la información de la página y el explorador actuales. Este evento registra una actividad en el registro Marketo correspondiente.
1. Envía un evento &quot;Vínculo en el que se ha hecho clic en una página web&quot; cuando el usuario selecciona un vínculo.

Use Munchkin [Ajustes de configuración](configuration.md) para cambiar este comportamiento. Por ejemplo, use `cookieAnon` para controlar si Munchkin crea una cookie para todos los posibles clientes que visitan la página o use `clickTime` para cambiar el retardo del clic.

Para deshabilitar la actividad Visita, establezca `apiOnly` como verdadero. En la versión 162 (agosto de 2022), Munchkin rastrea los clics en `tel` y `mailto` vínculos además de `http/s` vínculos.

## Posibles clientes conocidos y anónimos

Cuando un posible cliente visita por primera vez una página de su dominio, Marketo crea un registro de posible cliente anónimo. La clave principal de este registro es la cookie de Munchkin (`_mkto_trk`) creada en el explorador del usuario.

Marketo registra la actividad web posterior de ese explorador en el registro anónimo. Para asociar la actividad con un registro de Marketo conocido, debe producirse uno de los siguientes eventos:

- El posible cliente debe visitar una página con seguimiento de Munchkin con un parámetro `mkt_tok` en la cadena de consulta desde un vínculo de correo electrónico de Marketo al que se ha hecho un seguimiento.
- El posible cliente debe rellenar un formulario de Marketo.
- Se debe enviar una llamada de REST [Asociar posible cliente](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/associateLeadUsingPOST).

Cuando se produce uno de estos eventos, Marketo asocia la cookie y toda la actividad web relacionada con el posible cliente conocido.

Marketo crea un registro de actividad web anónimo para cada explorador. Si un posible cliente visita su dominio desde un nuevo equipo o explorador, la asociación debe volver a producirse.

## Dominios

Munchkin crea y rastrea cookies por dominio. Para realizar el seguimiento de un posible cliente conocido entre dominios, debe producirse un evento de asociación de posible cliente en cada dominio.

Por ejemplo, suponga que controla `marketo.com` y `example.com`. Un posible cliente envía un formulario en `marketo.com` y más tarde se dirige a `example.com`. La actividad de `marketo.com` está asociada con el posible cliente conocido, pero la actividad de `example.com` es anónima.

Los posibles clientes conocidos persisten entre subdominios. Un posible cliente conocido en `www.example.com` también es un posible cliente conocido en `info.example.com`.

Si el dominio de nivel superior consta de dos partes, como `.co.uk`, agregue el parámetro `domainLevel` al fragmento de código de Munchkin. Para obtener más información, consulte [Configuración](configuration.md#domainlevel).

## Cookie

La cookie de Munchkin utiliza la clave `_mkto_trk` y un valor que sigue uno de estos patrones:

`id:561-HYG-937&token:_mch-marketo.com-1374552656411-90718`

O

`id:561-HYG-937&token:_mch-marketo.com-97bf4361ef4433921a6da262e8df45a`

Las cookies de Munchkin son específicas de cada dominio de segundo nivel, como `example.com`. La duración predeterminada de la cookie es de 2 años (730 días).

## Beta

Para activar el canal beta de Munchkin en tus páginas de aterrizaje, ve a [Administración -> Cofre del tesoro](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/settings/enable-or-disable-treasure-chest-features) y habilita la configuración &quot;Munchkin Beta en páginas de aterrizaje&quot;.

Esta opción agrega fragmentos de código al menú **[!UICONTROL Administrador]** -> **[!UICONTROL Munchkin]**. Utilice estos fragmentos para ejecutar la versión beta en sitios externos.

## Opción de exclusión

Los visitantes pueden excluirse del seguimiento de Munchkin agregando el parámetro &quot;marketo_opt_out=true&quot; de `querystring` a la dirección URL de su explorador. Cuando Munchkin JavaScript detecta esta configuración, intenta establecer una nueva cookie &quot;mkto_opt_out&quot; con un valor de `true`.

A continuación, Munchkin elimina todas las demás cookies de seguimiento de Marketo, no establece nuevas cookies y no realiza solicitudes HTTP.
