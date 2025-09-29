---
title: Contenido dinámico
feature: REST API, Dynamic Content
description: Configure contenido dinámico de Marketo de nivel de sección mediante las API de REST mediante segmentaciones para personalizar correos electrónicos, páginas de aterrizaje y fragmentos de código con extremos y ejemplos
exl-id: 8ab97624-5fb5-4a41-911f-ec8616dd43c9
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '441'
ht-degree: 2%

---

# Contenido dinámico

Marketo facilita el uso del contenido dinámico mediante la segmentación de posibles clientes en varios tipos de recursos:

- Correos electrónicos
- Páginas de destino
- Fragmentos

## Información general

El contenido dinámico se implementa en el nivel de sección, mediante la designación de variaciones específicas de una sección que se va a servir a un posible cliente en función de su calificación en un segmento dentro de una segmentación elegida. Si un fragmento de contenido está configurado para ofrecer contenido dinámico basado en una segmentación determinada, un posible cliente que ve ese contenido recibe la variación de contenido que coincide con el segmento en el que se encuentran, o el contenido predeterminado, si no cumple los requisitos para un segmento.

## Ejemplo

Para demostrarlo, veamos un ejemplo de correo electrónico, en el que tenemos una segmentación de Región (EE. UU.) y queremos mostrar una promoción de evento solo para los posibles clientes que se incluyen en el segmento Suroeste, que incluye posibles clientes de California, Nevada, Utah, Colorado, Arizona y Nuevo México. Para ello, realizamos una sección editable en nuestro correo electrónico con el ID &quot;Q1-promotion-banner&quot; en una sección DynamicContent. Para ello, debemos usar el punto de conexión [Actualizar sección de contenido de correo electrónico](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailComponentContentUsingPOST) para nuestro correo electrónico. El parámetro `value` se usa para especificar el identificador de la segmentación.

Nota: Tanto los correos electrónicos como las páginas de aterrizaje siguen este patrón. Los fragmentos de código tienen un patrón diferente, detallado en la Documentación de la API de fragmentos de código.

El siguiente ejemplo establece que la sección sea una sección de Contenido dinámico, segmentada por la segmentación 1001.

```
POST /rest/asset/v1/email/{id}/content/Q1-promotion-banner.json
```

```
type=DynamicContent&value=1001
```

```json
{
  "success": true,
  "errors": [],
  "requestId": "891b#1729b34b9a5",
  "warnings": [],
  "result": [
    {
      "id": 1909
    }
  ]
}
```

Para agregar contenido para segmentos individuales, debemos llamar al punto de conexión [Actualizar sección de contenido dinámico de correo electrónico](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailDynamicContentUsingPOST) para la sección específica.

El siguiente ejemplo establece la sección para mostrar nuestra imagen de titular especial para posibles clientes en el segmento Suroeste en lugar del predeterminado. Si queremos crear más variaciones para más segmentos, volveríamos a llamar a este punto final para cada segmento y sección.

```
POST /rest/asset/v1/email/{id}/dynamicContent/{dynamicContentId}.json
```

```
segment=Southwest&type=HTML&value=<img src='//www.example.com/SuperSpecialBannerForAmericanSouthwestLeads.jpg'/>
```

```json
{
  "success": true,
  "errors": [],
  "requestId": "891b#1729b34b9a5",
  "warnings": [],
  "result": [
    {
      "id": 1637
    }
  ]
}
```

## Segmentación

La segmentación es el núcleo del contenido dinámico de Marketo. Una segmentación es una lista definida por el usuario de conjuntos individuales de reglas que se evalúan de arriba abajo con respecto a toda la base de datos de posibles clientes. Un posible cliente solo puede ser miembro de un segmento en cada segmentación y será miembro del primero al que corresponda en cada segmentación. Si no cumple los requisitos para un segmento, será miembro del segmento Predeterminado y recibirá el contenido predeterminado para cualquier parte determinada de contenido dinámico que utilice esa segmentación.

### Lista

Las segmentaciones tienen un extremo de lista que devuelve una respuesta con una lista de segmentaciones disponibles.

```
GET /rest/asset/v1/segmentation.json
```

```json
{
  "success": true,
  "warnings": [ ],
  "errors": [ ],
  "requestId": "78eb#14e9de95868",
  "result": [
    {
      "id": 1001,
      "name": "My Industry Segmentation",
      "description": "",
      "createdAt": "2015-04-06T18:23:32Z+0000",
      "updatedAt": "2015-04-06T18:37:10Z+0000",
      "url": "https://app-abm.marketo.com/#SG1001A1",
      "folder": {
        "type": "Program",
        "value": 396,
        "folderName": null
      },
      "status": "approved",
      "workspace": "Default"
    },
    {
      "id": 1002,
      "name": "My Country Segmentation",
      "description": "",
      "createdAt": "2015-04-06T18:28:23Z+0000",
      "updatedAt": "2015-04-06T18:37:18Z+0000",
      "url": "https://app-abm.marketo.com/#SG1002A1",
      "folder": {
        "type": "Program",
        "value": 396,
        "folderName": null
      },
      "status": "approved",
      "workspace": "Default"
    }
  ]
}
```

Las segmentaciones también tienen un extremo que devuelve una respuesta con una lista de segmentos de una segmentación principal.

```
GET /rest/asset/v1/segmentation/1001/segments.json
```

```json
{
  "success": true,
  "warnings": [ ],
  "errors": [ ],
  "requestId": "2031#14e9df08796",
  "result": [
    {
      "id": 1001,
      "name": "Manufacturing",
      "description": null,
      "createdAt": "2015-04-06T18:23:32Z+0000",
      "updatedAt": "2015-04-06T18:37:09Z+0000",
      "status": "approved",
      "segmentationId": 1001
    },
    {
      "id": 1002,
      "name": "Healthcare",
      "description": null,
      "createdAt": "2015-04-06T18:23:32Z+0000",
      "updatedAt": "2015-04-06T18:37:09Z+0000",
      "url": "https://app-abm.marketo.com/#SL769688A1",
      "status": "approved",
      "segmentationId": 1001
    },
    {
      "id": 1003,
      "name": "Financial",
      "description": null,
      "createdAt": "2015-04-06T18:23:32Z+0000",
      "updatedAt": "2015-04-06T18:37:09Z+0000",
      "url": "https://app-abm.marketo.com/#SL769690A1",
      "status": "approved",
      "segmentationId": 1001
    },
    {
      "id": 1004,
      "name": "Technology",
      "description": null,
      "createdAt": "2015-04-06T18:23:32Z+0000",
      "updatedAt": "2015-04-06T18:37:09Z+0000",
      "url": "https://app-abm.marketo.com/#SL769692A1",
      "status": "approved",
      "segmentationId": 1001
    },
    {
      "id": 1005,
      "name": "Default",
      "description": null,
      "createdAt": "2015-04-06T18:23:32Z+0000",
      "updatedAt": "2015-04-06T18:37:09Z+0000",
      "url": "https://app-abm.marketo.com/#SL769694A1",
      "status": "approved",
      "segmentationId": 1001
    }
  ]
}
```
