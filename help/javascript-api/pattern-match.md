---
title: Coincidencia de motivo
description: Coincidencia de motivo
feature: Javascript
exl-id: 4ebd13e3-375b-449b-850f-3b18f570ca75
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '133'
ht-degree: 6%

---

# Coincidencia de motivo

RTP expone una función de utilidad para comprobar si el patrón coincide con una cadena determinada. La utilidad no se puede utilizar de forma asíncrona porque devuelve una indicación de si hay una coincidencia o no.

Debe convertirse en cliente de Web Personalization y tener la etiqueta [RTP implementada](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) en su sitio antes de usar la API de contexto de usuario.

## Uso

> rtp.checkPattern(check_against, pattern);

| Parámetro | Opcional/Requerida | Tipo | Descripción |
|---|---|---|---|
| check_against | Obligatorio | Cadena | Cadena con la que coincide el patrón. Por ejemplo: URL de la página actual, nombre del producto. |
| pattern | Obligatorio | Cadena | Agregar % para comodín. El patrón puede ser :start con y contiene una coincidencia completa |

## Ejemplos

Establezca la variable personalizada en el índice 1 si la dirección URL de la página actual termina con &quot;productA&quot;.

```javascript
if (rtp.checkPattern(window.location.href, '%productA')) {
    rtp('set', 'custom1', 'productA');
}
```

La ruta de URL actual es &quot;/products/productB&quot;. Este ejemplo comprueba si la ruta contiene &quot;products&quot; y establece una variable personalizada.

```javascript
var currentURLPath = '/products/productB';
if (rtp.checkPattern(currentURLPath, '%products%')) {
    rtp('set', 'custom1', 'products');
}
```
