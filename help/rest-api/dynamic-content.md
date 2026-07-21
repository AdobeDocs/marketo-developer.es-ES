---
title: Contenido dinámico
feature: REST API, Dynamic Content
description: Configure contenido dinámico de Marketo de nivel de sección mediante las API de REST mediante segmentaciones para personalizar correos electrónicos, páginas de aterrizaje y fragmentos de código con extremos y ejemplos
exl-id: 8ab97624-5fb5-4a41-911f-ec8616dd43c9
TQID: https://experienceleague.adobe.com/MwfPxu74qk0bPZMr6yuxQi--e3gMvP1tXQZ5iMil02o
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 329
ht-degree: 3%

---

# Contenido dinámico

Utilice segmentaciones de posibles clientes para proporcionar contenido dinámico en estos tipos de recursos:

- Correos electrónicos
- Páginas de aterrizaje
- Fragmentos

## Información general

El contenido dinámico funciona en el nivel de sección. Cada sección puede proporcionar variaciones para los segmentos de una segmentación seleccionada.

Cuando un posible cliente ve el recurso, Marketo muestra la variación del segmento del posible cliente. Si el posible cliente no cumple los requisitos para un segmento, Marketo muestra el contenido predeterminado.

## Ejemplo

Este ejemplo utiliza una segmentación Region (US) para mostrar una promoción de evento a posibles clientes en el segmento Southwest. El segmento incluye posibles clientes de California, Nevada, Utah, Colorado, Arizona y Nuevo México.

Use el punto de conexión [Actualizar sección de contenido de correo electrónico](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/updateEmailComponentContentUsingPOST) para cambiar la sección editable con id. `Q1-promotion-banner` a una sección `DynamicContent`. El parámetro `value` especifica el identificador de segmentación.

Los correos electrónicos y las páginas de aterrizaje siguen este patrón. Los fragmentos de código utilizan el patrón diferente descrito en la Documentación de la API de fragmentos de código.

El siguiente ejemplo establece que la sección sea una sección de Contenido dinámico, segmentada por la segmentación 1001.

```http
POST /rest/asset/v1/email/{id}/content/Q1-promotion-banner.json
```

```text
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

Llame al punto de conexión [Actualizar sección de contenido dinámico de correo electrónico](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/updateEmailDynamicContentUsingPOST) para agregar contenido para un segmento en una sección específica.

La siguiente solicitud muestra un banner especial en lugar del contenido predeterminado para posibles clientes en el segmento Suroeste. Para crear más variaciones, llame al punto final de cada segmento y sección.

```http
POST /rest/asset/v1/email/{id}/dynamicContent/{dynamicContentId}.json
```

```text
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

Una segmentación es una lista definida por el usuario de conjuntos de reglas que Marketo evalúa de arriba a abajo con respecto a la base de datos de posibles clientes. Un posible cliente solo puede pertenecer a un segmento en cada segmentación. El posible cliente se une al primer segmento para el que cumple los requisitos.

Si el posible cliente no cumple los requisitos para otro segmento, se une al segmento predeterminado y recibe el contenido predeterminado de la segmentación.

### Lista

Utilice el extremo de la lista para recuperar las segmentaciones disponibles.

```http
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

Utilice el extremo de segmentos para recuperar los segmentos de una segmentación principal.

```http
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
