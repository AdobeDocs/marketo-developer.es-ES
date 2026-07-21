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
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 188
ht-degree: 4%

---

# Coincidencia de motivo

RTP proporciona una función de utilidad que comprueba si un patrón coincide con una cadena. La utilidad devuelve un resultado de coincidencia sincrónicamente y no se puede utilizar asincrónicamente.

Debe ser cliente de Web Personalization y tener la etiqueta [RTP implementada](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) en el sitio antes de usar la API de contexto de usuario.

## Uso

> rtp.checkPattern(check_against, pattern);

| Parámetro | Opcional/Requerida | Tipo | Descripción |
| --- | --- | --- | --- |
| check_against | Obligatorio | Cadena | Cadena con la que debe coincidir el patrón, como la dirección URL de la página actual o un nombre de producto. |
| pattern | Obligatorio | Cadena | Patrón para coincidir. Agregue `%` como comodín para que coincida con el inicio, el final o el contenido de una cadena. Omitir `%` para una coincidencia completa. |

## Ejemplos

Este ejemplo establece una variable personalizada en el índice 1 cuando la dirección URL de la página actual termina con productA.

```javascript
if (rtp.checkPattern(window.location.href, '%productA')) {
    rtp('set', 'custom1', 'productA');
}
```

En el ejemplo siguiente, la ruta de URL actual es &quot;/products/productB&quot;. En el ejemplo se comprueba si la ruta de acceso contiene &quot;products&quot; y, a continuación, se establece una variable personalizada.

```javascript
var currentURLPath = '/products/productB';
if (rtp.checkPattern(currentURLPath, '%products%')) {
    rtp('set', 'custom1', 'products');
}
```
