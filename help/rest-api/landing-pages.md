---
title: Páginas de destino
feature: REST API, Landing Pages
description: Utilice la API de REST de Marketo para consultar metadatos y contenido, crear, actualizar, aprobar, eliminar y clonar páginas de aterrizaje, incluidos los tipos guiados y de forma libre.
exl-id: 2f986fb0-0a6b-469f-b199-1c526cd5a882
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '1018'
ht-degree: 2%

---

# Páginas de destino

[Referencia de extremo de página de aterrizaje](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages)

Las páginas de aterrizaje son páginas web alojadas por Marketo.

## Consulta

Al igual que la mayoría de los demás recursos, las páginas de aterrizaje se pueden consultar [por nombre](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/getLandingPageByNameUsingGET), [por identificador](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/getLandingPageByIdUsingGET) y por [exploración](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/browseLandingPagesUsingGET). Estas consultas solo devolverán metadatos, y la lista de secciones de contenido para una página de aterrizaje debe consultarse por separado mediante el ID de la página de aterrizaje.

Si se consulta el contenido de la página de aterrizaje, se devuelve una lista de las secciones de contenido disponibles en la página de aterrizaje. Debe haber una sección en la lista de contenido de una página para actualizar el contenido:

```
GET /rest/asset/v1/landingPage/{id}/content.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "6307#154ea1689d7",
    "result": [
        {
            "id": "67",
            "type": "Form",
            "index": 1,
            "content": {
                "content": "189",
                "contentType": "Form",
                "contentUrl": "https://app-devlocal1.marketo.com/#FO189A1ZN13LA1"
            },
            "formattingOptions": {
                "zIndex": 15,
                "left": "359px",
                "top": "122px"
            }
        }
    ]
}
```

Los resultados diferirán entre las plantillas de formulario guiado y las de formulario libre, ya que las páginas de aterrizaje guiadas incluyen un conjunto de secciones definidas por la plantilla de la que se derivan, mientras que las páginas de formulario libre no incluyen secciones predefinidas y su contenido debe añadirse antes de editar.  Tenga en cuenta que el formato del atributo &quot;content&quot; puede variar según el atributo &quot;type&quot; y si el campo es estático o dinámico.

## Crear y actualizar

[Se crean páginas de aterrizaje](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/createLandingPageUsingPOST) haciendo referencia a una plantilla. Los únicos campos obligatorios para la creación son nombre, plantilla (el ID de la plantilla) y la carpeta en la que colocar la página. Para ver los metadatos adicionales que se pueden rellenar, consulte la referencia del extremo.

Los tipos de contenido válidos para [contenido de página de aterrizaje](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content) son: richText, HTML, Form, Image, Rectangle, Snippet.

```
POST rest/asset/v1/landingPages.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=createLandingPage&folder={"type": "Folder", "id": 11}&template=1&description=this is a test&workspace=default&title=test create&keywords=awesome&formPrefill=false
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "7a39#154cf7922c6",
    "result": [
        {
            "id": 27,
            "name": "createLandingPage",
            "description": "this is a test",
            "createdAt": "2016-05-20T18:41:43Z+0000",
            "updatedAt": "2016-05-20T18:41:43Z+0000",
            "folder": {
                "type": "Folder",
                "value": 11,
                "folderName": "Landing Pages"
            },
            "workspace": "Default",
            "status": "draft",
            "template": 1,
            "title": "test create",
            "keywords": "awesome",
            "robots": "index, nofollow",
            "formPrefill": false,
            "mobileEnabled": false,
            "URL": "https://app-devlocal1.marketo.com/lp/622-LME-718/createLandingPage.html",
            "computedUrl": "https://app-devlocal1.marketo.com/#LP27B2"
        }
    ]
}
```

Los metadatos de la página de aterrizaje se pueden actualizar con [Actualizar extremo de metadatos de la página de aterrizaje](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/updateLandingPageUsingPOST).

## Aprobación

Las páginas de aterrizaje siguen el modelo estándar de borrador aprobado, donde puede haber una versión en borrador o una versión aprobada. Siempre que se aplican actualizaciones a una página, siempre se aplican primero a la versión de borrador y solo se verán en directo cuando se haya aprobado la página.

## Eliminar

Para eliminar una página de aterrizaje, primero debe estar fuera de uso y ningún otro recurso de Marketo debe hacer referencia a ella, así como estar desaprobada. Las páginas se eliminan individualmente con el extremo [Eliminar página de aterrizaje](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/deleteLandingPageByIdUsingPOST). Las páginas de aterrizaje con botones sociales incrustados no se pueden eliminar a través de esta API.

## Clonar

Marketo proporciona un método sencillo para clonar una página de aterrizaje. Esta es una solicitud de POST application/x-www-url-formencoded.

El parámetro de ruta de acceso `id` especifica el identificador de la página de aterrizaje de origen que se va a clonar.

El parámetro `name` se usa para especificar el nombre de la nueva página de aterrizaje.

El parámetro `folder` se usa para especificar la carpeta principal en la que se crea la nueva página de aterrizaje. Se trata de un objeto JSON incrustado que contiene `id` y `type`.

El parámetro `template` se usa para especificar el identificador de la plantilla de la página de aterrizaje de origen.

El parámetro opcional `description` se usa para describir la nueva página de aterrizaje.

```
POST /rest/asset/v1/landingPage/{id}/clone.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=MyNewLandingPage&folder={"type":"Program","id":1119}&template=57
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "1078d#1683e4881c6",
    "warnings": [],
    "result": [
        {
            "id": 3291,
            "name": "MyNewLandingPage",
            "createdAt": "2019-01-11T18:59:25Z+0000",
            "updatedAt": "2019-01-11T18:59:25Z+0000",
            "folder": {
                "type": "Program",
                "value": 1119,
                "folderName": "DefaultProgramWithGuidedLP"
            },
            "workspace": "Default",
            "status": "draft",
            "template": 57,
            "robots": "index, nofollow",
            "formPrefill": false,
            "mobileEnabled": false,
            "URL": "http://na-abm.marketo.com/lp/284-RPR-133/DefaultProgramWithGuidedLPPerkutoTestLP-Clone-1.html",
            "computedUrl": "https://app-abm.marketo.com/#LP3291A1LA1"
        }
    ]
}
```

## Sección Administrar contenido

Las secciones de contenido se ordenan por su propiedad de índice y, en última instancia, se presentan según las reglas CSS que se apliquen cuando el cliente las muestre. Las secciones de contenido se incluyen y administran con los puntos finales de sección de contenido de la página de aterrizaje [Add](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/addLandingPageContentUsingPOST), [Update](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/updateLandingPageContentUsingPOST) y [Delete](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/removeLandingPageContentUsingPOST) correspondientes, y se pueden consultar usando [Obtener contenido de la página de aterrizaje](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/getLandingPageContentUsingGET). Cada sección tiene un tipo y un parámetro de valor. El tipo determina lo que se debe colocar en el valor.  Para estos extremos, los datos se pasan como POST x-www-form-urlencoded, no como JSON.

**Tipos de sección**

| Tipo | Valor |
|--- |--- |
| DynamicContent | El ID de la segmentación. |
| Formulario | El ID del formulario. |
| HTML | Contenido de HTML de texto. |
| Imagen | El ID del recurso de imagen. |
| Rectángulo | Vacío. |
| Texto enriquecido | Contenido de HTML de texto.  Solo puede contener elementos de texto enriquecido. |
| Fragmento | El ID del fragmento. |
| SocialButton | El ID de  el botón social. |
| Vídeo | El ID del vídeo. |

Para las páginas de forma libre, se deben agregar todas las secciones de contenido que desee y se incrustarán en el elemento div con el ID `mktoContent`. Para las páginas guiadas, puede haber una lista de elementos predefinidos en la lista del extremo [Obtener contenido de la página de aterrizaje](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/getLandingPageContentUsingGET). Se pueden agregar más o [actualizar el contenido](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/updateLandingPageContentUsingPOST) a través de sus respectivos extremos.

### Contenido dinámico

Para crear una sección de contenido dinámico, debe estar presente en la lista de contenido de la página de aterrizaje. A continuación, el punto de conexión [Actualizar sección de contenido de página de aterrizaje](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/updateLandingPageContentUsingPOST) debe usarse para establecer el tipo en &#39;DynamicContent&#39;. Cuando una sección se establece en contenido dinámico, crea secciones dinámicas subyacentes dentro de la sección de contenido, que heredan el tipo base del elemento convertido. Cada sección dinámica también hereda el contenido de la sección convertida.

```
GET /rest/asset/v1/landingPage/{id}/dynamicContent/RVMtNDg=.json
```

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "46e#1560fa169d9",
  "result": [
    {
      "createdAt": "2016-07-21",
      "updatedAt": "2016-07-21",
      "segmentation": 1007,
      "segments": [
        {
          "segmentId": 1018,
          "segmentName": "Default",
          "type": "RichText",
          "content": "\n\t\t\t\t\t\t\tAlice was beginning to get very tired of sitting by her sister on the bank, and having nothing to do: once or twice she had peeped into the book her sister was reading, but it had no pictures or conversations in it.\n\t\t\t\t\t\t"
        },
        {
          "segmentId": 1017,
          "segmentName": "New Segment",
          "type": "RichText",
          "content": "\n\t\t\t\t\t\t\tAlice was beginning to get very tired of sitting by her sister on the bank, and having nothing to do: once or twice she had peeped into the book her sister was reading, but it had no pictures or conversations in it.\n\t\t\t\t\t\t"
        }
      ]
    }
  ]
}
```

[La actualización del contenido](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/updateLandingPageDynamicContentUsingPOST) para cada segmento individual se realiza según el ID del segmento.

```
POST /rest/asset/v1/landingPage/{id}/dynamicContent/{dynamicContentId}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
segment=New Segment&value=New Content
```

```json
 {
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "7516#14e08fe7cbbc",
  "result": [
    {
      "id": 1012
    }
  ]
}
```

## Variables

Una de las funciones introducidas en las páginas de aterrizaje guiadas es la de las variables editables.  Las variables contienen valores para los elementos de una página de aterrizaje.  Las variables se pueden modificar fácilmente con el editor de páginas de aterrizaje como se muestra a continuación:

![Variables de página de aterrizaje](assets/landing-page-variables.png)

Las variables se definen como metaetiquetas dentro del elemento `<head>` de una plantilla de página de aterrizaje de modo guiado. Hay tres tipos de variables disponibles: Cadena, Color y Booleano.  Este es un ejemplo de tres definiciones de variables:

```html
<head>
  <meta charset="utf-8">
  <meta class="mktoString" mktoName="My String Variable" id="stringVar" default="Hello World!">
  <meta class="mktoColor" mktoName="My Color Variable" id="colorVar" default="#ffffff">
  <meta class="mktoBoolean" mktoName="My Boolean Variable" id="boolVar" default="true">
</head>
```

Para obtener más información, consulte la sección &quot;Variable editable&quot; en la documentación de [Crear una plantilla de página de aterrizaje guiada](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-guided-landing-page-template).

### Consulta

Recupere variables para una página de aterrizaje guiada pasando el ID de la página de aterrizaje al punto de conexión Obtener variables de página de aterrizaje.

```
GET /rest/asset/v1/landingPage/{id}/variables.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "10843#15a6d7e5fa1",
    "result": [
        {
            "id": "stringVar",
            "value": "Hello World!",
            "type": "string"
        },
        {
            "id": "colorVar",
            "value": "#FFFFFF",
            "type": "color"
        },
        {
            "id": "boolVar",
            "value": "true",
            "type": "boolean"
        }
    ]
}
```

Entrada  En este ejemplo, la página de aterrizaje guiada contiene 3 variables: stringVar, colorVar y boolVar.

### Actualización

Actualice una variable para una página de aterrizaje guiada pasando el ID de página de aterrizaje, el ID de variable y el valor de variable al punto de conexión Actualizar variables de página de aterrizaje.

```
POST /rest/asset/v1/landingPage/{id}/variable/{variableId}.json?value={newValue}
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "2b07#15a6db77da3",
    "result": [
        {
            "id": "stringVar",
            "value": "Hello Brave New World!",
            "type": "String"
        }
    ]
}
```

## Previsualizar página de aterrizaje

Marketo proporciona el punto de conexión [Obtener contenido completo de la página de aterrizaje](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/getLandingPageFullContentUsingGET) para recuperar una vista previa activa de una página de aterrizaje tal como se representaría en un explorador. Hay un parámetro requerido, el parámetro de ruta de acceso `id` que es el identificador de la página de aterrizaje que desea previsualizar. Hay dos parámetros de consulta opcionales adicionales:

- segmentation: acepta una matriz de objetos JSON que contienen los atributos segmentationId y segmentId. Cuando se establece, obtiene una vista previa de la página de aterrizaje como si fuera un posible cliente que coincide con esos segmentos.
- leadId:  Acepta el ID entero de un posible cliente. Cuando se establece, obtiene una vista previa de la página de aterrizaje como si fuera vista por el posible cliente designado.

```
GET /rest/asset/v1/landingPage/{id}/fullContent.json?leadId=1001&segmentation=[{"segmentationId":1030,"segmentId":1103}]
```

```json
{
  "success": true,
  "errors": [],
  "requestId": "119ab#17692849f1e",
  "warnings": [],
  "result": [
    {
      "id": 1023,
      "content": "<!DOCTYPE html>\n<html>\n <head>\n <meta charset=\"utf-8\">\n \n \n <meta name=\"robots\" content=\"index, nofollow\">\n <title></title>\n <style>\n body {background:#FFFFFF} \n #myConditionalDisplayArea {\n display: true;\n }\n </style>\n <link rel=\"shortcut icon\" href=\"/favicon.ico\" type=\"image/x-icon\" >\n<link rel=\"icon\" href=\"/favicon.ico\" type=\"image/x-icon\" >\n\n\n<style>.mktoGen.mktoImg {display:inline-block; line-height:0;}</style>\n </head>\n <body id=\"bodyId\">\n \n Hello Brave New World!\n <div class=\"mktoText\" id=\"exampleText\"><div>This is an example editable text area.</div>\n<div>Lead Full Name = Hanna Crawford</div>\n<div><br /></div>\n <script type=\"text/javascript\" src=\"//munchkin.marketo.net//munchkin.js\"></script><script>Munchkin.init('123-ABC-456', {customName: 'Test-Landing-Page-APIs_Guided-Landing-Page---deverly', PURL_VISIT_TOKEN, wsInfo: 'j1RR'});</script>\n<div id=\"mktoClickBlockingDiv\"></div>\n </body>\n</html>\n"
    }
  ]
}
```
