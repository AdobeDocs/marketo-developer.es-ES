---
title: Plantillas de la página de destino
feature: REST API, Landing Pages
description: Administre las plantillas de página de aterrizaje de Marketo a través de los extremos de la API de REST para obtener forma gratuita y tipos guiados, consultar por ID o nombre, crear, actualizar HTML, clonar, Munchkin.
exl-id: f9d1255e-ec13-4b75-96d5-b4cc9457a51b
TQID: https://experienceleague.adobe.com/U9K1MG-q2gIgJMgfM3lt1S4olETt8ln9seOIKZUncBY
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: f82558ea-6af5-44eb-a424-5b3389abb0a3
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 519
ht-degree: 2%

---

# Plantillas de la página de destino

[Referencia de extremo de plantilla de página de aterrizaje](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Templates)

Las plantillas de página de aterrizaje son recursos principales para las páginas de aterrizaje de Marketo. Cada página de aterrizaje deriva su estructura de contenido inicial de su plantilla principal.

## Tipos de plantilla

Marketo proporciona plantillas de página de aterrizaje guiadas y de forma libre. Las plantillas de forma libre proporcionan una experiencia de edición ligeramente estructurada. Las plantillas guiadas pueden restringir los tipos de elementos y las ubicaciones en el nivel de plantilla.

Para ver una comparación detallada, consulte [Explicación de las páginas de aterrizaje de forma libre frente a las páginas de aterrizaje guiadas](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/understanding-landing-pages/understanding-free-form-vs-guided-landing-pages).

## Consulta

Plantillas de página de aterrizaje de consulta [por id.](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Templates/operation/getLandingPageTemplateByIdUsingGET), [por nombre](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Templates/operation/getLandingPageTemplateByNameUsingGET) o por [exploración](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Templates/operation/getLandingPageTemplatesUsingGET). Estos extremos devuelven metadatos de plantilla. Recupere el contenido de HTML por separado para cada plantilla por ID.

## Crear y actualizar

Las plantillas se crean como recursos vacíos con metadatos. Se requieren los parámetros `name` y `folder`. Los parámetros `description`, `templateType` y `enableMunchkin` son opcionales.

El valor `templateType` puede ser `freeform` o `guided`, y el valor predeterminado es `freeForm`. El valor predeterminado de `enableMunchkin` es `false`. Cuando se habilita, se impide el seguimiento de Munchkin en las páginas de aterrizaje secundarias de la plantilla.

```http
POST /rest/asset/v1/landingPageTemplates.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=New LPT - PHP&folder={"id":12,"type":"Folder"}
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "11b7#14dfe1e3bcf",
    "result": [
        {
            "id": 286,
            "name": "assetAPITest",
            "description": "test",
            "createdAt": "2015-06-16T20:45:03Z+0000",
            "updatedAt": "2015-06-16T20:45:03Z+0000",
            "url": "https://app-devlocal1.marketo.com/#LT286B2ZN12",
            "folder": {
                "type": "Folder",
                "value": 12,
                "folderName": "Templates"
            },
            "status": "draft",
            "workspace": "Default"
        }
    ]
}
```

Agregue contenido de plantilla por separado con el extremo [Actualizar contenido de plantilla de página de aterrizaje](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Templates/operation/updateLandingPageTemplateContentUsingPOST).

### Actualizar metadatos

Use el extremo [Actualizar metadatos de plantilla de página de aterrizaje](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Templates/operation/updateLpTemplateUsingPOST) para cambiar el nombre, la descripción o la configuración de `enableMunchkin`.

### Actualizar contenido

Al actualizar el contenido de la plantilla, se sustituye todo el contenido existente de HTML. Pase el reemplazo como `multipart/form-data` en el parámetro `content`.

```http
POST /rest/asset/v1/landingPageTemplate/286/content.json
```

```html
content-type: multipart/form-data; boundary=--------------------------435851813185237176536801
----------------------------435851813185237176536801
Content-Disposition: form-data; name="content"; filename="content.txt"
Content-Type: text/plain

<html>
<head>
</head>
<body>
<div>Placeholder Content</div>
</body>
</html>
----------------------------435851813185237176536801--
```

```json
 {
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "7516#14e0dc60bbc",
  "result": [
    {
      "id": 286
    }
  ]
}
```

## Clonar

Clonar una plantilla de página de aterrizaje con una solicitud POST de `application/x-www-url-formencoded`.

El parámetro de ruta `id` especifica la plantilla de página de aterrizaje de origen.

El parámetro `name` especifica el nombre de la nueva plantilla de página de aterrizaje.

El parámetro `folder` especifica la carpeta principal de la nueva plantilla. Páselo como un objeto JSON incrustado que contiene `id` y `type`.

El parámetro `description` opcional describe la nueva plantilla.

```http
POST /rest/asset/v1/landingPageTemplate/{id}/clone.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=Standard Template Clone&folder={"type": "Folder", "id": 732}
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "dee6#1683e9fd410",
    "warnings": [],
    "result": [
        {
            "id": 61,
            "name": "Standard Template Clone",
            "createdAt": "2019-01-11T20:34:48Z+0000",
            "updatedAt": "2019-01-11T20:34:48Z+0000",
            "url": "https://app-abm.marketo.com/#LT61B2ZN732",
            "folder": {
                "type": "Folder",
                "value": 732,
                "folderName": "Test LP Template Clone"
            },
            "status": "draft",
            "workspace": "Default",
            "templateType": "freeForm",
            "enableMunchkin": true
        }
    ]
}
```

## Aprobación

Las plantillas de página de aterrizaje utilizan el borrador estándar y el modelo aprobado. Las actualizaciones se aplican primero al borrador y solo se activan después de que se apruebe la plantilla.

Antes de la aprobación, una plantilla debe cumplir los requisitos para su tipo guiado o de forma libre. Consulte estos recursos:

- [Plantillas de página de aterrizaje de forma libre](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-free-form-landing-page-template)
- [Plantillas de la página de destino guiada](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-guided-landing-page-template)
- [Ejemplos de plantillas guiadas](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/guided-landing-page-template-list)

## Eliminar

Para eliminar una plantilla, asegúrese de que no esté aprobada y de que ninguna página de aterrizaje secundaria haga referencia a ella. No puede utilizar esta API para eliminar plantillas de página de aterrizaje con botones sociales incrustados.
