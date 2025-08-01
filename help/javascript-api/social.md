---
title: Social
description: Social
feature: Social, Javascript
exl-id: 82d2b86f-5efe-4434-b617-d27f76515a79
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '776'
ht-degree: 4%

---

# Social

[Marketo Social Marketing](https://business.adobe.com/products/marketo/social-marketing.html) permite a los especialistas en mercadotecnia incrustar widgets sociales en sitios web y páginas de aterrizaje. Los widgets sociales incluyen encuestas, botones para compartir en redes sociales, vídeos, sorteos y promociones como ofertas de referencia.

## Widget de recursos compartidos incrustados de muestra

```html
<!-- Marketo Widget Loader Script -->

<script type="text/javascript" src="//b2c-mlm.marketo.com/jsloader/271d8232-1500-4305-b7ed-05d451b9ee0c/loader.php.js">
</script>

 <!-- The Location of the Social Widget -->

<divclass='cf_widgetloader cf_w_245d8f3c0955454cbd26abc39d0d874c'="" options="{&quot;outerHeight&quot;:400, &quot;outerWidth&quot;:600}">
</divclass='cf_widgetloader'>
```

![widget para compartir en redes sociales](assets/social-share-widget.png)

Existen dos métodos básicos para la personalización de un widget social:

1. Usar la interfaz de usuario normal del producto y adjuntar detectores de eventos para recibir información cuando se han producido determinadas acciones en la interfaz de usuario para realizar lógica empresarial adicional.
1. Reemplace la IU normal del producto por una personalizada y active las &quot;fases&quot; emergentes de la IU cuando lo desee.

## Adjuntar eventos a la IU normal

Existen dos formas de suscribirse a eventos en la biblioteca JavaScript de CF, a nivel global o para un solo widget. Los eventos se documentan a continuación en la tabla de eventos.

### Suscripción a evento global

```html
<script>
cf_scripts.afterload(function(){
    CF.events.listen("event_name_here",
        function(event, arg1){
            //Your code to do something on the event goes here.
            //It will be fired whenever ANY widget fires the event "event_name_here".
        }
    );
});
</script>
```

### Suscripción de evento por widget

```html
<script>
cf_scripts.afterload(function(){
    CF.widget.listen("widget_name_here", "event_name_here",
        function(event, arg1){
            //Your code to do something on the event goes here.
            //It will be fired whenever the widget named "widget_name_here" fires the event "event_name_here".
        }
    );
});
</script>
```

## Un ejemplo

Este ejemplo muestra un elemento oculto anteriormente con el ID &quot;signedUp&quot; después de que un usuario haya completado una inscripción de oferta para un widget denominado &quot;reference_SignUp&quot;.

```html
<div id='signedUp'style='display:none; color:green;'>This is a custom message to let you know that you signed up!</div>
<div class='cf_widgetLoader cf_w_referral_SignUp'></div>

<script>
    cf_scripts.afterload(function(){
        CF.widget.listen("referral_SignUp", "offer_enrolled", function(){
        cf_jq("#signedUp").show();
    });
});
</script>
```

## Tabla de eventos básicos

| Nombre del evento | Descripción | Widgets que utilizan este evento | Argumentos admitidos (pasados a la función de devolución de llamada de evento) |
| --- | --- | --- | --- |
| share_sent | Se activa cada vez que se envía una solicitud compartida al servidor para su procesamiento | Todos los widgets que tienen la capacidad de compartir | 1&quot;.share_sent&quot; (String)<br>2. Parameters sent (objeto) |
| share_success | Se activa cuando la solicitud de uso compartido se procesa correctamente. | Todos los widgets que tienen la capacidad de compartir. | 1&quot;.share_success&quot; (String)<br>2. Objeto de respuesta Compartir, que contiene el mensaje enviado y la URL abreviada (objeto) |
| vote_success | Se activa cuando un usuario ha votado correctamente en una encuesta. | Widgets de encuesta, VS y voto | &#x200B;1. &quot;vote_success&quot; (String)<br>2. Elemento votado, incluido título, descripción, identificador de entidad (objeto) |
| offer_enrolled | Se activa cuando un usuario se ha inscrito correctamente en una oferta | Todos los widgets de oferta | 1&quot;.offer_enrolled&quot; (String)<br>2. Se cambiaron las propiedades de usuario (objeto),<br>3. Atributos de usuario modificados (objeto) |
| profile_saved | Se activa cuando un usuario ha actualizado su perfil a partir de la captura de perfil | Todos los widgets que no son de oferta y que tienen la captura de perfil habilitada | 1&quot;.profile_saved&quot; (String)<br>2. Se cambiaron las propiedades de usuario (objeto)<br>3. Atributos de usuario modificados (objeto) |
| video_loaded | Se activa cuando un vídeo incrustado está completamente cargado e inicializado. | Widget de VideoShare | &#x200B;1. &quot;video_loaded&quot; (String) 2. Elemento &quot;.cf_videoshare_wrap&quot; que contiene el vídeo (objeto jQuery) |

## Reemplazar la interfaz de usuario con una interfaz de usuario personalizada

Para reemplazar la interfaz de usuario con una interfaz personalizada, primero debe desactivar la interfaz de usuario normal. Para ello, establezca la opción _popupUIOnly_ en _true_. Con esta opción establecida, la IU estándar no se representará al cargar la página; en su lugar, el widget recupera sus datos y espera a que inicie una de sus fases emergentes llamando a la función _CF.widget.activate_ y proporcionando opciones sobre lo que debe hacer.

A continuación se muestra un ejemplo de cómo crear un botón personalizado que inicie el flujo de registro de ofertas de referencia para un widget de ofertas de referencia denominado _reference_SignUp_.

```html
<button id="myNewSignUpButton">My newSign Up button</button>

<!-- Turn off the defaultreferral offer UI by setting popupUIOnly to true-->
<div class="cf_widgetLoader cf_w_referral_SignUp" options="{popupUIOnly:true}"></div>

<script>
cf_scripts.afterload(function($, CF){
    // After the cf script library has loaded, find the button with
    // id="myNewSignUpButton", and attach a click listener to it.
    $("#myNewSignUpButton").click(function(){
        // When it is clicked, activate the popup widget flow for the referral,
        // asking it to point to the clicked button.
        CF.widget.activate("referral_SignUp", {pointTo:$(this)});
    });
});
</script>
```

Dado que la adición de controladores de clics es habitual, existe un método abreviado para agregarlos. El siguiente es funcionalmente equivalente al ejemplo anterior.

```html
<button id="myNewSignUpButton">My newSign Up button</button>
<div class="cf_widgetLoader cf_w_referral_SignUp" options="{popupUIOnly:true}"></div>

<script>
cf_scripts.afterload(function($, CF){
    // Use the addClickActivate convenience method, which will
    // automatically make the popup point at the clicked item with id myNewSignUpButton.
    CF.widget.addClickActivate("#myNewSignUpButton", "referral_SignUp", {});
});
</script>
```

## Obtención de datos de la IU de widgets para colocarlos en la IU de reemplazo

Si necesita datos sobre el widget para dibujar la interfaz de usuario de reemplazo, puede obtener los datos del evento especial _ui_data_. Puede escuchar este evento con la función normal `CF.widget.listen`, pero hacerlo puede causar una posible condición de carrera en la que el oyente de eventos se agregue después de que el widget ya haya activado el evento_ui_data_, por lo que nunca recibirá datos. Para evitar esta carrera, use el evento `CF.widget.uiData_ method instead, which will give you the most recent available _ui_data_, and listen for all future updates as well. The _ui_data` que se activa siempre que se realice una acción que hubiera causado que se redibujara la interfaz de usuario estándar del widget, aunque haya deshabilitado esa interfaz con la opción `popupUIOnly`.

Ejemplo que utiliza la función `uiData` para mostrar el número de entradas que un usuario tiene para un sorteo con el nombre de widget _sweeps_Sweepstakes_.

```html
<span>You have <span id="entryCount">?</span> entries.</span>

<div class="cf_widgetLoader cf_w_sweeps_Sweepstakes"></div>

<button id='myNewSweepsButton'>New Sweeps Up Button!</button>

<script>
cf_scripts.afterload(function($, CF){
    CF.widget.uiData("sweeps_Sweepstakes", function(uiData){
        if(uiData.user && uiData.userStatus && uiData.userEntries){
            $("entryCount").html(""+ uiData.userEntries);
        }
        else{
            $("entryCount").html("0");
        }
    });
});
</script>
```

## Referencia de datos de IU de registro de oferta de referencia

| Tipo | Descripción |
|---------------|----------------------------------------------------|
| fecha | Valor de fecha del formulario &quot;aaaa-MM-dd&quot; |
| número | Un número entero o de coma flotante |
| Texto enriquecido | Una cadena de HTML |
| Puntaje | Un entero de 32 bits con signo |
| campaña de sfdc | Se utiliza en la integración de administración de campañas de Salesforce |
| texto | Una cadena de texto |

## Referencia de datos de IU de TrackProgress de ofertas de referencia

| Tipo | Descripción |
|---------------|----------------------------------------------------|
| fecha | Valor de fecha del formulario &quot;aaaa-MM-dd&quot; |
| número | Un número entero o de coma flotante |
| Texto enriquecido | Una cadena de HTML |
| Puntaje | Un entero de 32 bits con signo |
| campaña de sfdc | Se utiliza en la integración de administración de campañas de Salesforce |
| texto | Una cadena de texto |

## Referencia de datos de IU de Sorteos (para Sorteos de Social Campaign, no de LM)

| Tipo | Descripción |
|---------------|----------------------------------------------------|
| fecha | Valor de fecha del formulario &quot;aaaa-MM-dd&quot; |
| número | Un número entero o de coma flotante |
| Texto enriquecido | Una cadena de HTML |
| Puntaje | Un entero de 32 bits con signo |
| campaña de sfdc | Se utiliza en la integración de administración de campañas de Salesforce |
| texto | Una cadena de texto |

## Referencia de datos del inicio de sesión social (widget de relleno de formulario)

| Tipo | Descripción |
|---------------|----------------------------------------------------|
| fecha | Valor de fecha del formulario &quot;aaaa-MM-dd&quot; |
| número | Un número entero o de coma flotante |
| Texto enriquecido | Una cadena de HTML |
| Puntaje | Un entero de 32 bits con signo |
| campaña de sfdc | Se utiliza en la integración de administración de campañas de Salesforce |
| texto | Una cadena de texto |

```javascript
{
"alt_id": "http://www.facebook.com/profile.php?id=1526228678",
"provider_name": "facebook",
"default_photo_url": "https://graph.facebook.com/1526228678/picture?type=large",
"email": "ian.b.taylor@gmail.com",
"verified_email": "ian.b.taylor@gmail.com",
"gender": "male",
"preferred_user_name": "IanTaylor",
"display_name": "Ian Taylor",
"birth_date": 343954800000,
"first_name": "Ian",
"last_name": "Taylor",
"city": null,
"state": null,
"region": null,
"postal_code": null,
"country": null,
"time_zone": null,
"connection_count": 0,
"credentials": {
"uid": "1526228678",
"scopes": "publish_actions",
"expires": "1371994082",
"accessToken": "BAAGFJ0KUFpcBABuNMptmYY...",
"type": "Facebook"
},
"about_me": null,
"cur_pos_title": "Senior Staff Engineer",
"phone_number": null,
"company": "Marketo",
"cur_pos_start_date": 1333231200000,
"cur_pos_summary": null
}
```
