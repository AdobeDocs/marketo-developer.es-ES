---
title: Configuración
description: Utilice la API de JavaScript de configuración para establecer los valores de configuración al utilizar Munchkin.
feature: Munchkin Tracking Code, Javascript
exl-id: 4700ce7b-f624-4f27-871e-9a050f203973
source-git-commit: 1ad2d793832d882bb32ebf7ef1ecd4148a6ef8d5
workflow-type: tm+mt
source-wordcount: '557'
ht-degree: 3%

---

# Configuración

Munchkin puede aceptar varias opciones de configuración para personalizar el comportamiento. Las opciones de configuración son propiedades de un objeto JavaScript que se pasa como segundo parámetro al llamar a [Munchkin.init()](api-reference.md#munchkin_init)

```json
Munchkin.init("AAA-BBB-CCC", {
        "configName":"configValue",
        "configName2":"configValue2"
    }
);
```

El objeto de configuración puede contener cualquier número de propiedades de la tabla siguiente.

## Propiedades

| Nombre | Tipo de datos | Descripción |
|---|---|---|
| altIds | Matriz | Acepta una matriz de cadenas de Munchkin ID. Cuando está habilitada, esta opción duplica toda la actividad web en las suscripciones de destino, según su Munchkin ID. |
| anonymizeIP | Booleano | Anonimiza la dirección IP registrada en Marketo para los nuevos visitantes. |
| apiOnly | Booleano | Si se establece en true, la función `Munchkin.Init()` no llamará a `visitsWebPage`. Esto es útil para aplicaciones web de una sola página que necesitan control total sobre cada evento de `visitsWebPage`. |
| asyncOnly | Booleano | Si se establece en true, envía el objeto XMLHttpRequest de forma asincrónica. El valor predeterminado es false. |
| clickTime | Entero | Establece la cantidad de tiempo que se debe bloquear después de un clic para permitir la solicitud de rastreo de clics (en milisegundos). Al reducir esto, se reduce la precisión del rastreo de clics. El valor predeterminado es 350 ms. |
| cookieAnon | Booleano | Si se establece en false, impide el seguimiento y la creación de cookies de nuevos posibles clientes anónimos. Los posibles clientes tienen cookies y se rastrean después de rellenar un formulario de Marketo o haciendo clic desde un correo electrónico de Marketo. El valor predeterminado es True. |
| cookieLifeDays | Entero | Establece la fecha de caducidad de cualquier cookie de seguimiento de Munchkin recién creada en esta cantidad de días en el futuro. El valor predeterminado es de 730 días (2 años). |
| customName | Cadena | Nombre de página personalizado. Solo para uso del sistema. |
| <a name="domainlevel"></a>nivelDeDominio | Entero | Establece el número de partes del dominio de la página que se utilizarán al establecer el atributo de dominio de la cookie. Por ejemplo, supongamos que el dominio de página actual es &quot;www.example.com&quot;.domainLevel: 2 establecerá el atributo de dominio de la cookie en &quot;.example.com&quot;domainLevel: 3 establecerá el atributo de dominio de la cookie en &quot;.www.example.com&quot;Background:Munchkin administrará automáticamente ciertos dominios de nivel superior de dos letras. De forma predeterminada, esto consta de dos partes en casos normales en los que el dominio de nivel superior tiene tres letras. Por ejemplo, &quot;www.example.com&quot;, las dos partes situadas más a la derecha se utilizan para configurar la cookie, &quot;.example.com&quot;. Para los códigos de país de dos letras como &quot;.jp&quot;, &quot;.us&quot;, &quot;.cn&quot; y &quot;.uk&quot;, el código toma tres partes de forma predeterminada. Por ejemplo, &quot;www.example.co.jp&quot; utilizará tres partes del dominio situado más a la derecha, &quot;.example.co.jp&quot;. Si el patrón de dominio requiere un comportamiento diferente, debe especificarse utilizando el parámetro `domainLevel`. |
| domainSelectorV2 | Booleano | Si se establece en true, utiliza un método mejorado para determinar cómo establecer el atributo de dominio de la cookie. |
| httpsOnly | Booleano | El valor predeterminado es false. Cuando se establece en true, establece la cookie para utilizar la configuración segura cuando la página rastreada se proporcionó mediante https. |
| useBeaconAPI | Booleano | El valor predeterminado es false. Cuando se establece en true, utiliza la [API de señalización](https://developer.mozilla.org/en-US/docs/Web/API/Beacon_API) para enviar solicitudes sin bloqueo en lugar de [XMLHttpRequest](https://developer.mozilla.org/es-ES/docs/Web/API/XMLHttpRequest). Si el explorador no admite esta API, Munchkin vuelve a utilizar XMLHttpRequest. |
| wsInfo | Cadena | Toma una cadena para dirigirse a un espacio de trabajo. Este ID de espacio de trabajo se obtiene seleccionando Workspace en el menú Administración > Integración > Munchkin. Esta configuración solo se aplica a la creación inicial de un registro de posible cliente anónimo. Una vez establecido el valor de la cookie de Munchkin para ese registro de posibles clientes, el parámetro wsInfo no se puede utilizar para cambiar su partición. Dado que esta configuración solo afecta a los posibles clientes anónimos, solo es relevante para [Visitantes anónimos en los informes web](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/reporting/basic-reporting/report-activity/display-people-or-anonymous-visitors-in-web-reports) específicos de la partición. |

## Ejemplos

### Enviar actividad a varias suscripciones

Este ejemplo envía toda la actividad web a las instancias con los ID de Munchkin &quot;AAA-BBB-CCC&quot; y &quot;XXX-YYY-ZZZ&quot;.

```javascript
<script type="text/javascript">
(function() {
  var didInit = false;
  function initMunchkin() {
    if(didInit === false) {
      didInit = true;
      // Add configuration settings to the init method
      Munchkin.init('AAA-BBB-CCCC', { 'altIds': ['XXX-YYY-ZZZ'] });
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
```

### Definir seguimiento como asincrónico

En este ejemplo se fuerza el envío asincrónico de todos los objetos XMLHttpRequest desde el subproceso principal.

```javascript
<script type="text/javascript">
(function() {
  var didInit = false;
  function initMunchkin() {
    if(didInit === false) {
      didInit = true;
      // Add configuration settings to the init method
      Munchkin.init('AAA-BBB-CCC', { 'asyncOnly': true });
    }
  }
  var s = document.createElement('script');
  s.type = 'text/javascript';
  s.async = true;
  s.src = '//munchkin.marketo.net/munchkin-beta.js';
  s.onreadystatechange = function() {
    if (this.readyState == 'complete' || this.readyState == 'loaded') {
      initMunchkin();
    }
  };
  s.onload = initMunchkin;
  document.getElementsByTagName('head')[0].appendChild(s);
})();
</script>
```
