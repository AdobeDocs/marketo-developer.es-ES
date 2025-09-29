---
title: Plantillas de la página de destino
feature: REST API, Landing Pages
description: Administre las plantillas de página de aterrizaje de Marketo a través de los extremos de la API de REST para obtener forma gratuita y tipos guiados, consultar por ID o nombre, crear, actualizar HTML, clonar, Munchkin.
exl-id: f9d1255e-ec13-4b75-96d5-b4cc9457a51b
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '552'
ht-degree: 1%

---

# Plantillas de la página de destino

[Referencia de extremo de plantilla de página de aterrizaje](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates)

Las plantillas de página de aterrizaje son un recurso principal y una dependencia para páginas de aterrizaje de Marketo individuales. Las páginas de aterrizaje obtienen el esqueleto de su contenido de la plantilla principal.

## Tipos de plantilla

Marketo tiene dos tipos de plantillas de página de aterrizaje, de forma libre y guiadas. Las plantillas de página de aterrizaje de forma libre proporcionan una experiencia de edición ligeramente estructurada para las páginas que se derivan de ellas. Las plantillas guiadas proporcionan una experiencia muy estructurada, en la que los tipos de elementos y las ubicaciones se pueden restringir en el nivel de plantilla. Para obtener más información sobre las diferencias, vea [este documento](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/understanding-landing-pages/understanding-free-form-vs-guided-landing-pages).

## Consulta

Las plantillas de página de aterrizaje admiten los tipos de consulta estándar para los recursos de [por id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/getLandingPageTemplateByIdUsingGET), [por nombre](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/getLandingPageTemplateByNameUsingGET) y [exploración](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/getLandingPageTemplatesUsingGET). Estos extremos devuelven metadatos para las plantillas. La recuperación del contenido de HTML de las plantillas debe realizarse por plantilla a través de su ID.

## Crear y actualizar

Las plantillas se crean como recursos vacíos con metadatos asociados. Al crear una plantilla, se debe incluir un nombre y una carpeta, junto con una descripción opcional, el parámetro templateType y enableMunchkin. templateType puede ser improvisado o guiado y su valor predeterminado es freeForm. Para ver las diferencias entre los tipos, consulte la sección Formulario guiado frente a formulario libre. enableMunchkin establece el valor predeterminado de false y, cuando está habilitada, impide que se realice el seguimiento de Munchkin en cualquier página de aterrizaje secundaria de la plantilla.

```
POST /rest/asset/v1/landingPageTemplates.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

El contenido de la plantilla debe rellenarse por separado mediante el punto de conexión [Actualizar contenido de plantilla de página de aterrizaje](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/updateLandingPageTemplateContentUsingPOST).

### Actualizar metadatos

Los metadatos de las plantillas de página de aterrizaje se pueden actualizar a través del punto de conexión [Actualizar metadatos de plantilla de página de aterrizaje](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/updateLpTemplateUsingPOST). El nombre, la descripción y la configuración de enableMunchkin pueden actualizarse de esta manera.

### Actualizar contenido

El contenido de las plantillas de página de aterrizaje se realiza como una actualización destructiva de todo el contenido de HTML. El contenido se debe pasar como multipart/form-data, con el único parámetro denominado content.

```
POST /rest/asset/v1/landingPageTemplate/286/content.json
```

```
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

```
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

Marketo proporciona un método sencillo para clonar plantillas de página de aterrizaje. Esta es una solicitud de POST application/x-www-url-formencoded.

El parámetro de ruta de acceso `id` especifica el identificador de la plantilla de página de aterrizaje de origen que se va a clonar.

El parámetro `name` se usa para especificar el nombre de la nueva plantilla de página de aterrizaje.

El parámetro `folder` se usa para especificar la carpeta principal donde residirá la nueva plantilla de página de aterrizaje. Tiene la forma de un objeto JSON incrustado que contiene  `id` y `type`.

El parámetro opcional `description` se usa para describir la nueva plantilla de página de aterrizaje.

```
POST /rest/asset/v1/landingPageTemplate/{id}/clone.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Las plantillas de página de aterrizaje siguen el modelo estándar de borrador aprobado, donde puede haber una versión en borrador o una versión aprobada. Siempre que se aplican actualizaciones a una plantilla, siempre se aplican primero a la versión de borrador y solo se verán activas cuando se haya aprobado la plantilla.

Para que una plantilla se apruebe, debe cumplir las reglas de su tipo, ya sea guiada por la forma libre. Para obtener más información sobre los requisitos para crear y aprobar plantillas de sus respectivos tipos, consulte sus respectivos documentos de creación:

- [Plantillas de página de aterrizaje de forma gratuita](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-free-form-landing-page-template)
- [Plantillas de página de aterrizaje guiada](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-guided-landing-page-template)
- [Ejemplos de plantillas guiadas](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/guided-landing-page-template-list)

## Eliminar

Para eliminar una plantilla, debe estar fuera de uso y desaprobada, lo que significa que ninguna página de aterrizaje secundaria puede hacer referencia a ella.  Las plantillas de página de aterrizaje con botones sociales incrustados no se pueden eliminar con esta API.
