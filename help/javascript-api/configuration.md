---
title: Configuración
description: Configure Marketo Munchkin con la API de JavaScript. Aprenda la configuración de Munchkin.init como altIds, anonymizeIP, asyncOnly, vida de la cookie, domainLevel, API de Beacon.
feature: Munchkin Tracking Code, Javascript
exl-id: 4700ce7b-f624-4f27-871e-9a050f203973
TQID: https://experienceleague.adobe.com/ip2cCGgoa83v8m9GYLYXe132veYxS1C6UWX1iLB6X5Q
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45id: ea90ebee-5c84-42d9-8b21-006bdabc95a3
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 541
ht-degree: 5%

---

# Configuración

Munchkin acepta ajustes de configuración que personalizan su comportamiento. Pase la configuración como propiedades de un objeto JavaScript en el segundo parámetro de [Munchkin.init()](api-reference.md#munchkin_init).

```json
Munchkin.init("AAA-BBB-CCC", {
        "configName":"configValue",
        "configName2":"configValue2"
    }
);
```

El objeto de configuración puede contener cualquier número de las propiedades de la tabla siguiente.

## Propiedades

| Nombre | Tipo de datos | Descripción |
| --- | --- | --- |
| altIds | Matriz | Acepta una matriz de cadenas de Munchkin ID. Cuando se habilita, esto duplica toda la actividad web en las suscripciones identificadas por sus Munchkin ID. |
| anonymizeIP | Booleano | Anonimiza la dirección IP registrada en Marketo para los nuevos visitantes. |
| apiOnly | Booleano | Si se establece en true, la función `Munchkin.Init()` no llamará a `visitsWebPage`. Esto es útil para aplicaciones web de una sola página que necesitan control total sobre cada evento de `visitsWebPage`. |
| asyncOnly | Booleano | Si se establece en true, envía XMLHttpRequests asincrónicamente. El valor predeterminado es false. |
| clickTime | Entero | Establece el tiempo, en milisegundos, que se debe bloquear después de un clic para que se pueda completar la solicitud de seguimiento de clics. La reducción de este valor reduce la precisión del rastreo de clics. El valor predeterminado es 350 ms. |
| cookieAnon | Booleano | Si se establece en false, impide el seguimiento y la creación de cookies para nuevos posibles clientes anónimos. Los posibles clientes reciben cookies y se rastrean después de enviar un formulario de Marketo o hacer clic desde un correo electrónico de Marketo. El valor predeterminado es True. |
| cookieLifeDays | Entero | Establece la fecha de caducidad de cualquier cookie de seguimiento de Munchkin recién creada en esta cantidad de días en el futuro. El valor predeterminado es de 730 días (2 años). |
| customName | Cadena | Nombre de página personalizado. Solo para uso del sistema. |
| <a name="domainlevel"></a>nivelDeDominio | Entero | Establece cuántas partes del dominio de página se utilizarán para el atributo de dominio de la cookie.<br><br>Para &quot;www.example.com&quot;, `domainLevel: 2` establece el dominio de la cookie en &quot;.example.com&quot; y `domainLevel: 3` lo establece en &quot;.www.example.com&quot;.<br><br>De forma predeterminada, Munchkin utiliza dos partes cuando el dominio de nivel superior tiene tres letras. Por ejemplo, &quot;www.example.com&quot; utiliza &quot;.example.com&quot;.<br><br>Para los códigos de país de dos letras como &quot;.jp&quot;, &quot;.us&quot;, &quot;.cn&quot; y &quot;.uk&quot;, Munchkin utiliza tres partes. Por ejemplo, &quot;www.example.co.jp&quot; usa &quot;.example.co.jp&quot;.<br><br>Use el parámetro `domainLevel` cuando el patrón de dominio requiera un comportamiento diferente. |
| domainSelectorV2 | Booleano | Si se establece en true, utiliza un método mejorado para determinar cómo establecer el atributo de dominio de la cookie. |
| httpsOnly | Booleano | El valor predeterminado es false. Cuando se establece en true, establece la cookie para utilizar la configuración segura cuando la página rastreada se proporcionó mediante https. |
| useBeaconAPI | Booleano | El valor predeterminado es false. Cuando se establece en true, utiliza la [API de señalización](https://developer.mozilla.org/en-US/docs/Web/API/Beacon_API) para enviar solicitudes sin bloqueo en lugar de [XMLHttpRequest](https://developer.mozilla.org/es-ES/docs/Web/API/XMLHttpRequest). Si el explorador no admite la API de Beacon, Munchkin utiliza XMLHttpRequest. |
| wsInfo | Cadena | Se dirige a un espacio de trabajo. Obtenga el ID del espacio de trabajo seleccionando el espacio de trabajo en el menú Administración > Integración > Munchkin.<br><br>Esta configuración se aplica solamente cuando se crea inicialmente un registro de cliente potencial anónimo. Una vez establecido el valor de la cookie de Munchkin para ese registro de posibles clientes, el parámetro wsInfo no podrá cambiar su partición.<br><br>Debido a que esta configuración solo afecta a los posibles clientes anónimos, solo es relevante para [Visitantes anónimos en los informes web](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/reporting/basic-reporting/report-activity/display-people-or-anonymous-visitors-in-web-reports) específicos de la partición. |

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

En este ejemplo se fuerza el envío asincrónico de todas las solicitudes XMLHttp desde el subproceso principal.

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
