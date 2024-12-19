---
title: Seguimiento de leads
description: API de seguimiento de posibles clientes
feature: Munchkin Tracking Code, Javascript
exl-id: 7ece5133-9d32-4be3-a940-4ac0310c4d8b
source-git-commit: 8ad3e3f0958ea705375651b1c8a75967d807ca80
workflow-type: tm+mt
source-wordcount: '766'
ht-degree: 0%

---

# API de seguimiento de posibles clientes

Munchkin JavaScript de Marketo permite rastrear las visitas y los clics de la página del usuario final en las páginas de aterrizaje de Marketo y en las páginas web externas. Se registran en Marketo como actividades &quot;Visitar página web&quot; y &quot;Vínculo en el que se hizo clic en la página web&quot;, que luego se pueden utilizar en déclencheur y filtros para campañas inteligentes y listas inteligentes.

## Incrustación del código

La instancia de Marketo proporciona automáticamente fragmentos de código de seguimiento preconfigurados para incrustar código en las páginas externas que rastrean la actividad hasta la instancia de Marketo. El uso del código incrustado se rige por este [acuerdo de licencia](../munchkin-license.pdf).

Hay tres tipos de código de seguimiento disponibles:

1. Simple: se carga sincrónicamente
1. Asíncrona: se carga asincrónicamente
1. jQuery asincrónico: se carga asincrónicamente y requiere que jQuery se cargue de antemano

Se recomienda encarecidamente que el código de seguimiento asincrónico se utilice para incrustar Munchkin en páginas externas. Para garantizar la tasa de éxito más alta posible para la ejecución, incruste el código de seguimiento asincrónico en `<head>` de cada página.

Algunos sistemas de gestión de contenido pueden tener métodos o restricciones específicos al incrustar secuencias de comandos arbitrarias.

Como referencia, la página final debe incluir código similar a este en `<head>` del documento de HTML:

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

El comportamiento predeterminado de Marketo Munchkin es hacer lo siguiente al cargar la página:

1. Compruebe si el explorador actual tiene una cookie de Munchkin y cree una si no está allí.
1. Envíe un evento &quot;Visitar página web&quot; a la instancia de Marketo designada con la información de la página y el explorador actuales. Registra una actividad en el registro correspondiente de Marketo.
1. Enviar el evento &quot;Vínculo en el que se ha hecho clic en una página web&quot; para cualquier clic del usuario en los vínculos.

El comportamiento de Munchkin se puede modificar mediante el uso de Munchkin [Configuración](configuration.md), por ejemplo, si se crea una cookie para todos los posibles clientes al visitar la página con la configuración `cookieAnon` o si se modifica el retardo de clic con la configuración `clickTime`. El envío de la actividad Visita puede deshabilitarse estableciendo la configuración de apiOnly en true. A partir de la versión 162 de (agosto de 2022), se hace un seguimiento de los clics `tel` y los vínculos `mailto`, además de los vínculos `http/s`.

## Posibles clientes conocidos y anónimos

En la primera visita de un posible cliente a una página de su dominio, se crea un nuevo registro de posible cliente anónimo en Marketo. La clave principal de este registro es la cookie de Munchkin (`_mkto_trk`) creada en el explorador del usuario. Toda la actividad web subsiguiente en ese navegador se registra en este registro anónimo. Para asociarse a un registro conocido en Marketo, debe producirse una de las siguientes acciones:

- El posible cliente debe visitar una página con seguimiento de Munchkin con un parámetro `mkt_tok` en la cadena de consulta desde un vínculo de correo electrónico de Marketo al que se ha hecho un seguimiento.
- El posible cliente debe rellenar un formulario de Marketo.
- Se debe enviar una llamada de REST [Asociar posible cliente](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/associateLeadUsingPOST).

Una vez que se cumpla una de estas condiciones, la cookie y toda la actividad web asociada se asociarán al posible cliente conocido.

Se crea un nuevo registro de actividad web anónimo para cada explorador individual, por lo que si un posible cliente visita su dominio por primera vez utilizando un nuevo equipo o explorador, esta asociación debe volver a realizarse.

## Dominios

Munchkin crea y rastrea cookies individuales por dominio, por lo que para que se produzca el seguimiento de posibles clientes conocido en todos los dominios, debe producirse un evento de asociación de posibles clientes para cada dominio. Por ejemplo, si controlo dos dominios, `marketo.com` y `example.com`, y un posible cliente rellena un formulario en `marketo.com` y luego navega a `example.com` más tarde, su actividad en `marketo.com` se rastrea en un registro de posible cliente conocido, pero su actividad en `example.com` es anónima. Sin embargo, los posibles clientes conocidos persisten entre subdominios, por lo que un posible cliente conocido en `www.example.com` también es un posible cliente conocido en `info.example.com`.

En caso de que el dominio de nivel superior esté formado por dos partes, como `.co.uk`, agregue un parámetro domainLevel al fragmento de código de Munchkin para que el código se rastree correctamente. Ver [aquí](configuration.md#domainlevel) para obtener más detalles.

## Cookie

La cookie de Munchkin utiliza la clave `_mkto_trk` y tiene un valor que sigue este patrón:

`id:561\-HYG\-937&token:_mch\-marketo.com\-1374552656411\-90718`

o

`id:561\-HYG\-937&token:_mch\-marketo.com\-97bf4361ef4433921a6da262e8df45a`

Las cookies de Munchkin son específicas de cada dominio de segundo nivel, es decir, `example.com`. La duración predeterminada de la cookie es de 2 años (730 días).

## Beta

Para activar el canal beta de Munchkin en tus páginas de aterrizaje, ve al menú [Administrador -> Cofre del tesoro](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/settings/enable-or-disable-treasure-chest-features) y activa la configuración &quot;Munchkin Beta en páginas de aterrizaje&quot;. Esto proporciona nuevos fragmentos de código en **[!UICONTROL Admin]** ->  Menú **[!UICONTROL Munchkin]** para permitirle usar la versión beta en sitios externos.

## Opción de exclusión

Los visitantes pueden excluirse por completo del seguimiento de Munchkin agregando el parámetro `querystring` &quot;marketo_opt_out=true&quot; a la dirección URL de su explorador. Cuando Munchkin JavaScript detecta esta configuración, intenta establecer una nueva cookie &quot;mkto_opt_out&quot; con un valor de `true`. Todas las demás cookies de seguimiento de Marketo se eliminan, no se establecen cookies nuevas y Munchkin no realiza solicitudes HTTP cuando se detecta esta configuración.
