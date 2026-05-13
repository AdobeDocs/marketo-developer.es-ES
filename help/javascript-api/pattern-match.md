---
title: Coincidencia de motivo
description: Utilice la utilidad RTP.checkPattern para probar patrones de cadena con caracteres comodín de porcentaje, consulte limitaciones de sincronización, ejemplos de uso y URL y configuración de etiquetas RTP requerida.
feature: Javascript
exl-id: 4ebd13e3-375b-449b-850f-3b18f570ca75
TQID: https://experienceleague.adobe.com/-HopUg6-2EchL9kJrPDbz62mRlrqYaXYdufILjkvP1Y
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: e2290edd-b061-4880-9d79-dee306cf5aa9id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 00118a89f25a23b931fac671130932bb0e0e4e4e
workflow-type: tm+mt
source-wordcount: 171
ht-degree: 5%

---

# Coincidencia de motivo

RTP expone una función de utilidad para comprobar si el patrón coincide con una cadena determinada. La utilidad no se puede utilizar de forma asíncrona porque devuelve una indicación de si hay una coincidencia o no.

Debe convertirse en cliente de Web Personalization y tener la etiqueta [RTP implementada](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) en su sitio antes de usar la API de contexto de usuario.

## Uso

> rtp.checkPattern(check_against, pattern);

| Parámetro | Opcional/Requerida | Tipo | Descripción |
| --- | --- | --- | --- |
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
