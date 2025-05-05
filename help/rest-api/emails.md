---
title: Emails
feature: REST API
description: API para manipular recursos de correo electrónico.
exl-id: 6875730d-c74a-42cf-a3d2-dad7a3ac535d
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '1946'
ht-degree: 1%

---

# Emails

[Referencia de extremo de correo electrónico](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails) Se proporciona un conjunto completo de extremos REST para manipular los recursos de correo electrónico.

Nota: si usa [Contenido predictivo de Marketo](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/predictive-content/working-with-predictive-content/understanding-predictive-content), los siguientes extremos fallarán si hacen referencia a un correo electrónico que contiene contenido predictivo: [Obtener contenido de correo electrónico](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailContentByIdUsingGET), [Actualizar la sección de contenido de correo electrónico](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailComponentContentUsingPOST), [Aprobar borrador de correo electrónico](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/approveDraftUsingPOST). La llamada devuelve un código de error 709 y el mensaje de error correspondiente.

## Consulta

El patrón de consulta de los mensajes de correo electrónico es idéntico al de las plantillas, ya que permite consultas [por id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailByIdUsingGET), [por nombre](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailByNameUsingGET) y [exploración](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailUsingGET), así como filtros basados en carpetas con las API examinar y por nombre.

Nota: Si un correo electrónico es parte de un programa de correo electrónico que usa [Pruebas A/B](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/email-marketing/email-programs/email-program-actions/email-test-a-b-test/add-an-a-b-test), ese correo electrónico no está disponible para consultas mediante los siguientes extremos: [Obtener correo electrónico por identificador](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailByIdUsingGET), [Obtener correo electrónico por nombre](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailByNameUsingGET), [Obtener correos electrónicos](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailUsingGET). La llamada indica que se ha realizado correctamente, pero contiene la siguiente advertencia: &quot;No se han encontrado recursos para los criterios de búsqueda especificados&quot;.

### Por identificador

```
GET /rest/asset/v1/email/1351.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"9ad0#14a1832af8c",
   "result":[
      {
         "id":1356,
         "name":"sakZxhxkwV",
         "description":"sample description",
         "createdAt":"2014-12-05T02:06:21Z+0000",
         "updatedAt":"2014-12-05T02:06:21Z+0000",
         "subject":{
            "type":"Text",
            "value":"sample subject"
         },
         "fromName":{
            "type":"Text",
            "value":"RBxEtmdQZz"
         },
         "fromEmail":null,
         "replyEmail":{
            "type":"Text",
            "value":"Qlikf@testmail.com"
         },
         "folder":{
            "type":"folder",
            "value":10421
         },
         "operational":false,
         "textOnly":false,
         "publishToMSI":false,
         "webView":false,
         "status":false,
         "template":338,
         "workspace":"Default",
         "isOpenTrackingDisabled": false,
         "version": 2,
         "autoCopyToText": true,
         "ccFields": [
            {
              "attributeId": "157",
              "objectName": "lead",
              "displayName": "Lead Owner Email Address",
              "apiName": null
            }
          ],
         "preHeader": "My awesome preheader!"
      }
   ]
}
```

### Por nombre

Para por nombre, opcionalmente puede pasar una carpeta para buscar solamente en esa carpeta.

```
GET /rest/asset/v1/email/byName.json?name=My Email&folder={"id":1056,"type"="Folder"}
```

```json
{
   "success":true,
   "warnings":[
   ],
   "errors":[
   ],
   "requestId":"3a7f#14c484de875",
   "result":[
      {
         "id":1032,
         "name":"My Email",
         "description":"eCjxjIHmYPLtecoSphkvIXlrygOBDLhgyQKnsKMpiKWgSCKhkPMUFvFPUvEylmFiLjQGnffXGaiNLxAwiFOmIDvxEINoaSYascJw",
         "createdAt":"2015-03-23T20:23:25Z+0000",
         "updatedAt":"2015-03-23T20:23:25Z+0000",
         "subject":{
            "type":"Text",
            "value":"ezyKBmDcyCcUIrXASrLSvRuWQgWpRZxQstJoStgMSLEBASGKMpAnVeWrgJsaVFoFJUEXhEIPpDAWpzajzingUruFpiMcRRwtoBzU"
         },
         "fromName":{
            "type":"Text",
            "value":"dAiqRNJOdY"
         },
         "fromEmail":{
            "type":"Text",
            "value":"ilZxG@testmail.com"
         },
         "replyEmail":{
            "type":"Text",
            "value":"VYsCS@testmail.com"
         },
         "folder":{
            "type":"folder",
            "value":1056
         },
         "operational":false,
         "textOnly":false,
         "publishToMSI":false,
         "webView":false,
         "status":"draft",
         "template":32,
         "workspace":"Default",
         "isOpenTrackingDisabled": false,
        "version": 2,
         "autoCopyToText": true,
         "ccFields": [
            {
              "attributeId": "157",
              "objectName": "lead",
              "displayName": "Lead Owner Email Address",
              "apiName": null
            }
          ],
         "preHeader": "My awesome preheader!"
      }
   ]
}
```

### Examinar

La exploración de carpetas funciona como otros extremos de exploración de Asset API y permite el filtrado opcional en `status`, `folder`, `earliestUpdatedAt`/`latestUpdatedAt`, `maxReturn` y `offset`. `status` está aprobado o es borrador. `folder` es un objeto JSON que contiene `id` y `type`. `maxReturn` es un entero que limita el número de resultados (el valor predeterminado es 20, el máximo es 200) y `offset` es un entero que se puede usar con `maxReturn` para leer conjuntos de resultados grandes (el valor predeterminado es 0).

```
GET /rest/asset/v1/emails.json?maxReturn=3&folder={"id":341,"type":"Folder"}
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "17576#14e22eb29cb",
    "result": [
        {
            "id": 2137,
            "name": "Social Sharing in Email",
            "description": "",
            "createdAt": "2011-03-04T17:12:42Z+0000",
            "updatedAt": "2011-03-04T19:04:36Z+0000",
            "url": null,
            "subject": {
                "type": "Text",
                "value": "Republish this content to your favorite social site!"
            },
            "fromName": {
                "type": "Text",
                "value": "Demo Master Marketo"
            },
            "fromEmail": {
                "type": "Text",
                "value": "demomaster@marketo.com"
            },
            "replyEmail": {
                "type": "Text",
                "value": "demomaster@marketo.com"
            },
            "folder": {
                "type": "Folder",
                "value": 341,
                "folderName": "Social Media"
            },
            "operational": false,
            "textOnly": false,
            "publishToMSI": false,
            "webView": true,
            "status": "approved",
            "template": null,
            "workspace": "Default",
            "isOpenTrackingDisabled": false,
            "version": 2,
            "autoCopyToText": true,
            "ccFields": [
               {
                 "attributeId": "157",
                 "objectName": "lead",
                 "displayName": "Lead Owner Email Address",
                 "apiName": null
               }
             ],
            "preHeader": "My awesome preheader!"
        }
    ]
}
```

## Contenido de consulta

Puede [recuperar las secciones editables disponibles](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailContentByIdUsingGET) para un correo electrónico consultando su contenido y, opcionalmente, filtrar el estado para obtener las secciones para las versiones Aprobado o Borrador.

```
GET /rest/asset/v1/email/1356/content.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"8a44#14c484de8c8",
   "result":[
      {
         "htmlId":"edit_text_3",
         "value":[
            {
               "type":"HTML",
               "value":"Content from testCreateEmailTemplate2"
            },
            {
               "type":"Text",
               "value":"Content from testCreateEmailTemplate2"
            }
         ],
         "contentType":"Text"
      }
   ]
}
```

Las secciones pueden devolver un tipo de contenido dinámico. Consulte la sección [Contenido dinámico](dynamic-content.md) para obtener más información.

## Campos CC de consulta

Puede recuperar el conjunto de campos habilitados para CC de correo electrónico en la instancia de destino llamando al extremo [Obtener campos CC de correo electrónico](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailCCFieldsUsingGET).

```
GET /rest/asset/v1/email/ccFields.json
```

```json
{  
   "success":true,
   "errors":[ ],
   "requestId":"e54b#16796fdbd4e",
   "warnings":[ ],
   "result":[  
      {  
         "attributeId":"157",
         "objectName":"lead",
         "displayName":"Lead Owner Email Address",
         "apiName":"leadOwnerEmailAddress"
      },
      {  
         "attributeId":"396",
         "objectName":"company",
         "displayName":"Account Owner Email Address",
         "apiName":"accountOwnderEmailAddress"
      }
   ]
}
```

## Crear y actualizar

[Se crean correos electrónicos](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/createEmailUsingPOST) a partir de una plantilla de origen y tienen una lista de secciones editables derivadas de cada elemento de HTML independiente en esa plantilla con una clase &quot;mktEditable&quot; y una propiedad de ID única. Al crear un correo electrónico con la API, se crea un registro basado en la plantilla junto con los metadatos adicionales pasados. Se requieren los siguientes parámetros para una llamada de correo electrónico de creación correcta: nombre, plantilla, carpeta.

Los siguientes parámetros son opcionales para la creación: `subject`, `fromName`, `fromEmail`, `replyEmail`, `operational`, `isOpenTrackingDisabled`. Si no se establece, `subject` estará vacío, `fromName`, `fromEmail` y `replyEmail` se establecerán en valores predeterminados de instancia, y `operational` y `isOpenTrackingDisabled` serán falsos. `isOpenTrackingDisabled` determina si el píxel de seguimiento abierto se incluye en un mensaje de correo electrónico cuando se envía.

```
POST /rest/asset/v1/emails.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=My New Email 02 - deverly&folder={"id":1017,"type":"Program"}&template=24&description=This is a test email&subject=Hey There&fromName=SomeBody&fromEmail=somebody@marketo.com&replyEmail=somebody@marketo.com
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "f557#14e22db88d9",
    "result": [
        {
            "id": 2212,
            "name": "My New Email 02 - deverly",
            "description": "This is a test email",
            "createdAt": "2015-06-23T23:58:09Z+0000",
            "updatedAt": "2015-06-23T23:58:09Z+0000",
            "url": "https://app-abm.marketo.com/#EM2212A1LA1",
            "subject": {
                "type": "Text",
                "value": "Hey There"
            },
            "fromName": {
                "type": "Text",
                "value": "SomeBody"
            },
            "fromEmail": {
                "type": "Text",
                "value": "somebody@marketo.com"
            },
            "replyEmail": {
                "type": "Text",
                "value": "somebody@marketo.com"
            },
            "folder": {
                "type": "Program",
                "value": 1017,
                "folderName": "Landing Page - promotion"
            },
            "operational": false,
            "textOnly": false,
            "publishToMSI": false,
            "webView": false,
            "status": "draft",
            "template": 24,
            "workspace": "Default",
            "isOpenTrackingDisabled": false,
            "version": 2,
            "autoCopyToText": false,
            "ccFields": null,  
            "preHeader": null
        }
    ]
}
```

[La actualización de un registro de correo electrónico](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailContentUsingPOST) se puede realizar mediante el identificador. Esto permite actualizar la descripción o el nombre del correo electrónico.

```
POST /rest/asset/v1/email/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
description=This is an Email&name=Updated Email
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "f557#14e22db88d9",
    "result": [
        {
            "id": 2212,
            "name": "Updated Email",
            "description": "This is an Email",
            "createdAt": "2015-06-23T23:58:09Z+0000",
            "updatedAt": "2015-06-23T23:58:09Z+0000",
            "url": "https://app-abm.marketo.com/#EM2212A1LA1",
            "subject": {
                "type": "Text",
                "value": "Hey There"
            },
            "fromName": {
                "type": "Text",
                "value": "SomeBody"
            },
            "fromEmail": {
                "type": "Text",
                "value": "somebody@marketo.com"
            },
            "replyEmail": {
                "type": "Text",
                "value": "somebody@marketo.com"
            },
            "folder": {
                "type": "Program",
                "value": 1017,
                "folderName": "Landing Page - promotion"
            },
            "operational": false,
            "textOnly": false,
            "publishToMSI": false,
            "webView": false,
            "status": "draft",
            "template": 24,
            "workspace": "Default",
            "isOpenTrackingDisabled": false,
            "version": 2,
            "autoCopyToText": false,
            "ccFields": null,  
            "preHeader": null
        }
    ]
}
```

### Sección de contenido, tipo y actualización

El contenido de cada sección de un correo electrónico se debe actualizar de forma individual, aparte del asunto, fromName, fromEmail y replyEmail, que se actualizan con el punto de conexión [Actualizar contenido del correo electrónico](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailContentUsingPOST). Al utilizar este punto de conexión, estos valores también se pueden configurar para utilizar contenido dinámico en lugar de contenido estático. Cada parámetro es un objeto JSON de tipo/valor, donde el tipo es &quot;Text&quot; o &quot;DynamicContent&quot; y el valor es el valor de texto adecuado o el ID de la segmentación que se utilizará para el contenido dinámico. Los datos se pasan como POST x-www-form-urlencoded, no como JSON.  isOpenTrackingDisabled se puede configurar con Actualizar contenido de correo electrónico

```
POST /rest/asset/v1/email/{id}/content.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
subject={"type":"Text","value":"Gettysburg Address"}&fromEmail={"type":"Text","value":"abe@testmail.com"}&fromName={"type":"Text","value":"Abe Lincoln"}&replyTO={"type":"Text","value":"replies@testmail.com"}
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"c865#14a1832afac",
   "result":[
      {
         "id":1356
      }
   ]
}
```

Si se configura una sección para que utilice contenido dinámico, el ID de sección debe recuperarse mediante la llamada Obtener contenido de correo electrónico.

### Actualizar sección editable

Las secciones editables se actualizan mediante sus htmlIds individuales. Solo se requieren el ID del correo electrónico y el htmlId de la sección como parámetros de ruta, mientras que el tipo, el valor y el textValue son opcionales. El tipo puede ser &quot;Texto&quot;, &quot;Contenido dinámico&quot; o &quot;Fragmento de código&quot; y afectará a lo que se pase en el valor. Si el tipo es Texto, el valor es una cadena que contiene el contenido del HTML de la sección. Si es DynamicContent, es un bloque JSON, con tres miembros, de tipo, que será &quot;DynamicContent&quot;, de segmentación, que es el ID de la segmentación que se utilizará para el contenido, y de forma predeterminada, que es una cadena que contiene el contenido de HTML predeterminado de la sección. El parámetro textValue opcional es una cadena que contiene la versión de texto de la sección. Los datos se pasan como POST x-www-form-urlencoded, no como JSON.

```
POST /rest/asset/v1/email/{id}/content/{htmlId}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
type=Text&value=<h1>Hello World!</h1>&textValue=Hello World!
```

```json
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "155ac#14d58dfa9ad",
    "result": [
        {
            "id": 2179
        }
    ]
}
```

Nota: Si la copia automática a texto está desactivada para un fragmento incrustado en un correo electrónico, el valor de HTML del fragmento se actualiza y, a continuación, se actualiza la versión de texto de otra sección del correo electrónico, la versión de texto del correo electrónico tendrá texto que refleje el valor actualizado del HTML del fragmento, no la versión anterior, como cabría esperar con la copia automática desactivada.

## Módulos

En el Editor de correo electrónico 1.0, un módulo es una sección del correo electrónico que se define en la plantilla. Los módulos pueden contener cualquier combinación de elementos, variables y otro contenido de HTML como se describe [aquí](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/email-marketing/general/email-editor-2/email-template-syntax#EmailTemplateSyntax-Modules). Marketo ofrece un conjunto de API para administrar módulos dentro de un correo electrónico. Para los extremos relacionados con el módulo que requieren el método del POST HTTP, el cuerpo tiene el formato &quot;application/x-www-form-urlencoded&quot; (no como JSON).

La mayoría de los extremos relacionados con módulos requieren un &quot;moduleId&quot; como parámetro de ruta. Es una cadena que describe el módulo. El extremo [Get Email Content](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailContentByIdUsingGET) devuelve moduleIds como atributo &quot;htmlId&quot; (consulte la sección [Consulta](#modules_query) más abajo).

### Consulta

Para trabajar con módulos, debe especificar un parámetro moduleId, que identifica de forma exclusiva el módulo. También puede especificar el parámetro de índice del módulo, que es un número entero que describe el orden del módulo en el correo electrónico.

[Recupere moduleIds y sus índices](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailContentByIdUsingGET) especificando el ID de correo electrónico como parámetro de ruta.

El siguiente ejemplo consulta un correo electrónico 1.0 basado en la plantilla &quot;Esqueleto&quot; encontrada en la sección &quot;Plantillas iniciales&quot; de la interfaz de usuario del selector de plantillas.

```
GET /rest/asset/v1/email/{moduleId}/content.json
```

```json
{
  "success": true,
  "warnings": [ ],
  "errors": [ ],
  "requestId": "3d79#158da6492bd",
  "result": [
    {
      "htmlId": "free-image",
      "contentType": "Module",
      "index": 1,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "single",
      "value": {
        "width": "600",
        "altText": "",
        "style": "-ms-interpolation-mode: bicubic; outline: none; border-right-width: 0; border-bottom-width: 0; border-left-width: 0; text-decoration: none; border-top-width: 0; display: block; max-width: 100%; line-height: 100%; height: auto; width: 600px"
      },
      "contentType": "Image",
      "parentHtmlId": "free-image",
      "isLocked": false
    },
    {
      "htmlId": "video",
      "contentType": "Module",
      "index": 2,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "video2",
      "value": {
        
      },
      "contentType": "Video",
      "parentHtmlId": "video",
      "isLocked": false
    },
    {
      "htmlId": "free-text",
      "contentType": "Module",
      "index": 3,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "text",
      "value": [
        {
          "type": "HTML",
          "value": "Lorem ipsum dolor sit amet, consectetur adipisicing elit. Harum officiis dolorum, nulla, mollitia ducimus iure modi perferendis tenetur ea illum veniam aut sapiente deserunt repellendus. Excepturi illo numquam sint harum."
        },
        {
          "type": "Text",
          "value": "Lorem ipsum dolor sit amet, consectetur adipisicing elit. Harum officiis dolorum, nulla, mollitia ducimus iure modi perferendis tenetur ea illum veniam aut sapiente deserunt repellendus. Excepturi illo numquam sint harum."
        }
      ],
      "contentType": "Text",
      "parentHtmlId": "free-text",
      "isLocked": false
    },
    {
      "htmlId": "two-articles",
      "contentType": "Module",
      "index": 6,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "article3",
      "value": {
        "height": "auto",
        "width": "270",
        "style": "-ms-interpolation-mode: bicubic; outline: none; border-right-width: 0; border-bottom-width: 0; border-left-width: 0; text-decoration: none; border-top-width: 0; display: block; max-width: 100%; line-height: 100%; height: auto; width: 270px"
      },
      "contentType": "Image",
      "parentHtmlId": "two-articles",
      "isLocked": false
    },
    {
      "htmlId": "articleTitle",
      "value": [
        {
          "type": "HTML",
          "value": "LOREM IPSUM DOLOR SIT AMET"
        },
        {
          "type": "Text",
          "value": "LOREM IPSUM DOLOR SIT AMET"
        }
      ],
      "contentType": "Text",
      "parentHtmlId": "two-articles",
      "isLocked": false
    },
    {
      "htmlId": "text2",
      "value": [
        {
          "type": "HTML",
          "value": "Gumbo beet greens corn soko endive gumbo gourd. shallot courgette tatsoi pea sprouts fava bean collard greens dandelion okra wakame tomato. Dandelion cucumber earthnut pea peanut soko zucchini."
        },
        {
          "type": "Text",
          "value": "Gumbo beet greens corn soko endive gumbo gourd. shallot courgette tatsoi pea sprouts fava bean collard greens dandelion okra wakame tomato. Dandelion cucumber earthnut pea peanut soko zucchini."
        }
      ],
      "contentType": "Text",
      "parentHtmlId": "two-articles",
      "isLocked": false
    },
    {
      "htmlId": "article4",
      "value": {
        "height": "auto",
        "width": "270",
        "style": "-ms-interpolation-mode: bicubic; outline: none; border-right-width: 0; border-bottom-width: 0; border-left-width: 0; text-decoration: none; border-top-width: 0; display: block; max-width: 100%; line-height: 100%; height: auto; width: 270px"
      },
      "contentType": "Image",
      "parentHtmlId": "two-articles",
      "isLocked": false
    },
    {
      "htmlId": "articleTitle2",
      "value": [
        {
          "type": "HTML",
          "value": "LOREM IPSUM DOLOR SIT AMET"
        },
        {
          "type": "Text",
          "value": "LOREM IPSUM DOLOR SIT AMET"
        }
      ],
      "contentType": "Text",
      "parentHtmlId": "two-articles",
      "isLocked": false
    },
    {
      "htmlId": "text3",
      "value": [
        {
          "type": "HTML",
          "value": "Gumbo beet greens corn soko endive gumbo gourd. shallot courgette tatsoi pea sprouts fava bean collard greens dandelion okra wakame tomato. Dandelion cucumber earthnut pea peanut soko zucchini."
        },
        {
          "type": "Text",
          "value": "Gumbo beet greens corn soko endive gumbo gourd. shallot courgette tatsoi pea sprouts fava bean collard greens dandelion okra wakame tomato. Dandelion cucumber earthnut pea peanut soko zucchini."
        }
      ],
      "contentType": "Text",
      "parentHtmlId": "two-articles",
      "isLocked": false
    },
    {
      "htmlId": "footer",
      "contentType": "Module",
      "index": 7,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "footerText",
      "value": [
        {
          "type": "HTML",
          "value": "<p style=\"text-align: center;\"><span style=\"color: #333333;\"><strong>Acme, Inc<\/strong><\/span><\/p> \n<div style=\"text-align: center;\">\n  You received this because you've subscribed to our newsletter. Click \n <a href=\"{{system.unsubscribeLink}}\" target=\"_blank\" class=\"mktNoTrack\">here<\/a> to unsubscribe. \n <br> \n<\/div>"
        },
        {
          "type": "Text",
          "value": "Acme, Inc \n You received this because you've subscribed to our newsletter. Click here <{{system.unsubscribeLink}}> to unsubscribe."
        }
      ],
      "contentType": "Text",
      "parentHtmlId": "footer",
      "isLocked": false
    },
    {
      "htmlId": "spacer",
      "contentType": "Module",
      "index": 0,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "CTA",
      "contentType": "Module",
      "index": 4,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "hr",
      "contentType": "Module",
      "index": 5,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    }
  ]
}
```

La matriz de resultados contiene elementos que describen tanto los módulos como los elementos HTML combinados. Los elementos de módulo contienen un atributo &quot;contentType&quot;: &quot;Module&quot; y un atributo &quot;index&quot;. moduleId se almacena en el atributo &quot;htmlId&quot;.

Continuando con el ejemplo &quot;Esqueleto&quot; anterior, la siguiente tabla contiene un resumen de moduleIds y sus índices correspondientes contenidos en el correo electrónico.

| moduleId (también conocido como htmlId) | Índice |
|---|---|
| espaciador | 0 |
| imagen libre | 1 |
| video | 2 |
| texto libre | 3 |
| CTA | 4 |
| h | 5 |
| de dos artículos | 6 |
| pie de página | 7 |

#### Agregar

[Agregue un módulo](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/addModuleUsingPOST) a un correo electrónico seleccionando uno de los módulos existentes contenidos en la plantilla de correo electrónico que está en uso. Para ello, especifique el ID de correo electrónico y el moduleId como parámetros de ruta. El parámetro de consulta de índice es obligatorio y determina el orden del módulo en el correo electrónico. Si el valor del índice supera el valor de índice existente más grande, el módulo se adjunta al correo electrónico.

```
POST /rest/asset/v1/email/{id}/content/{moduleId}/add.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
index=10
```

```
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "1063e#158d6ad2c3f",
    "result": [
        {
            "id": 1028
        }
    ]
}
```

#### Eliminar

[Elimine un módulo](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/deleteModuleUsingPOST) especificando el ID de correo electrónico y el moduleId como parámetros de ruta.

```
POST /rest/asset/v1/email/{id}/content/{moduleId}/delete.json
```

```
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "2356#158d6f6104a",
    "result": [
        {
            "id":1028
        }
    ]
}
```

#### Duplicado

[Duplique un módulo](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/duplicateModuleUsingPOST) especificando el ID de correo electrónico y el moduleId como parámetros de ruta. Esta llamada duplica el módulo, lo coloca debajo del módulo original y empuja hacia abajo a los demás módulos.

```
POST /rest/asset/v1/email/{id}/content/{moduleId}/duplicate.json
```

```
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "e740#158d705d967",
    "result": [
        {
            "id":1028
        }
    ]
}
```

#### Reorganizar

[Reorganice los módulos](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/rearrangeModulesUsingPOST)matriz que contiene todos los módulos y la posición deseada dentro del correo electrónico para cada uno. Cada elemento de matriz contiene un objeto JSON del siguiente formato:  { &quot;index&quot;: &lt;_index_>, &quot;moduleId&quot;: &quot;&lt;_moduleId_>&quot; }, donde &lt;_index_> es el número de orden de módulo basado en cero y &lt;_moduleId_> es el moduleId.

```
POST /rest/asset/v1/email/{id}/content/rearrange.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
positions=[ {"index": 0, "moduleId": "free-image"}, {"index": 1, "moduleId": "title"}, {"index": 2, "moduleId": "mkvideo"}, {"index": 3, moduleId": "free-text"}, {"index": 4, "moduleId": "blankSpace"}, {"index": 5, "moduleId": "Separator"}, {"index": 6, "moduleId": "callToAction"}, {"index": 7, "moduleId": "blankSpace2"}, {"index": 8, "moduleId": "blankSpace3"} ]
```

```
{
    "success": true,
    "warnings":[ ],
    "errors":[ ],
    "requestId": "e67a#158d72d1cde",
    "result":[
        {
            "id": 1030
        }
    ]
}
```

#### Cambiar nombre

[Cambiar el nombre de un módulo](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/renameUsingPOST) en un correo electrónico al pasar el nuevo nombre a través del parámetro name. Especifique el ID de correo electrónico y el moduleId (nombre existente) como parámetros de ruta.

```
POST /rest/asset/v1/email/{id}/content/{moduleId}/rename.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=MarketoVideo
```

```
{
    "success": true,
    "warnings":[ ],
    "errors": [ ],
    "requestId":"11521#158d740abc0",
    "result": [
        {
            "id": 1030
        }
    ]
}
```

## Variables

En el Editor de correo electrónico 1.0, las variables se utilizan para almacenar valores de elementos en el correo electrónico. Cada variable se define agregando sintaxis específica de Marketo al HTML como se describe [aquí](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/email-marketing/general/email-editor-2/email-template-syntax#EmailTemplateSyntax-Variables). Marketo ofrece un conjunto de API para administrar variables dentro de un correo electrónico.

### Consulta

[Recupere variables](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailVariablesUsingGET) de un correo electrónico especificando el ID de correo electrónico como parámetro de ruta.

El siguiente ejemplo consulta un correo electrónico 1.0 basado en la plantilla &quot;Esqueleto&quot; encontrada en la sección &quot;Plantillas iniciales&quot; de la interfaz de usuario del selector de plantillas.

```
GET /rest/asset/v1/email/{id}/variables.json
```

```
{
  "success": true,
  "warnings": [ ],
  "errors": [  ],
  "requestId": "756#158dade55e8",
  "result": [
    {
      "name": "twoArticlesSpacer5",
      "value": "20",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer6",
      "value": "15",
      "moduleScope": false
    },
    {
      "name": "twoArticlesBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "footerSpacer2",
      "value": "20",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer7",
      "value": "20",
      "moduleScope": false
    },
    {
      "name": "twoArticlesLinkText2",
      "value": "CALL TO ACTION",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer8",
      "value": "40",
      "moduleScope": false
    },
    {
      "name": "twoArticlesLinkText",
      "value": "CALL TO ACTION",
      "moduleScope": false
    },
    {
      "name": "freeTextSpacer",
      "value": "40",
      "moduleScope": false
    },
    {
      "name": "freeTextSpacer2",
      "value": "40",
      "moduleScope": false
    },
    {
      "name": "ctaSpacer2",
      "value": "20",
      "moduleScope": false
    },
    {
      "name": "hrBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "freeTextBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "spacerBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "twoArticlesLink2",
      "value": "http:\/\/mylink",
      "moduleScope": false
    },
    {
      "name": "hrBorderColor",
      "value": "#e6e6e6",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer",
      "value": "40",
      "moduleScope": false
    },
    {
      "name": "twoArticlesBorderSize",
      "value": "1",
      "moduleScope": false
    },
    {
      "name": "ctaLink",
      "value": "http:\/\/mylink",
      "moduleScope": false
    },
    {
      "name": "freeImageBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "spacerSpacer",
      "value": "40",
      "moduleScope": false
    },
    {
      "name": "footerSpacer",
      "value": "10",
      "moduleScope": false
    },
    {
      "name": "ctaLinkText",
      "value": "CALL TO ACTION",
      "moduleScope": false
    },
    {
      "name": "twoArticlesButtonBackgroundColor2",
      "value": "#333333",
      "moduleScope": false
    },
    {
      "name": "ctaBorderSize",
      "value": "1",
      "moduleScope": false
    },
    {
      "name": "ctaBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "footerBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "twoArticlesLink",
      "value": "http:\/\/mylink",
      "moduleScope": false
    },
    {
      "name": "twoArticlesBorderColor",
      "value": "#333333",
      "moduleScope": false
    },
    {
      "name": "ctaBorderColor",
      "value": "#333333",
      "moduleScope": false
    },
    {
      "name": "twoArticlesBorderColor2",
      "value": "#333333",
      "moduleScope": false
    },
    {
      "name": "hrBorderSize",
      "value": "1",
      "moduleScope": false
    },
    {
      "name": "twoArticlesButtonBackgroundColor",
      "value": "#333333",
      "moduleScope": false
    },
    {
      "name": "twoArticlesBorderSize2",
      "value": "1",
      "moduleScope": false
    },
    {
      "name": "ctaButtonBackgroundColor",
      "value": "#333333",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer4",
      "value": "20",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer3",
      "value": "15",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer2",
      "value": "20",
      "moduleScope": false
    },
    {
      "name": "ctaSpacer",
      "value": "20",
      "moduleScope": false
    }
  ]
}
```

La matriz de resultados contiene elementos que describen variables, una variable por elemento.

Las variables se pueden vincular globalmente a todo el correo electrónico o localmente a un módulo específico. Las variables de cualquiera de los ámbitos contienen los atributos &quot;name&quot;, &quot;value&quot; y &quot;moduleScope&quot;. El atributo &quot;moduleScope&quot; es booleano, donde false indica global y true indica local. Las variables locales contienen un atributo &quot;moduleId&quot; adicional que especifica el módulo al que está asociada la variable.

#### Actualización

[Actualice una variable](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateVariableUsingPOST) en un mensaje de correo electrónico pasando el nuevo valor deseado a través del parámetro value. Especifique el ID de correo electrónico y el nombre de la variable como parámetros de ruta. Si actualiza una variable de módulo, también debe pasar el parámetro moduleId para especificar el módulo al que está asociada la variable.

En el siguiente ejemplo, se actualiza una variable global denominada &quot;hrBorderSize&quot; a un valor de 1.

```
POST /rest/asset/v1/email/{id}/variable/{name}.json
```

```
Content-Type: application/x-www-form-urlencoded; charset=utf-8
```

```
value=2
```

```
{
    "success":true,
    "warnings":[ ],
    "errors":[ ],
    "requestId":"feb5#158db4be57e",
    "result": [
        {
            "name":"hrBorderSize",
            "value":"2",
            "moduleScope":false
        }
    ]
}
```

En el siguiente ejemplo, actualizamos una variable local denominada &quot;ctaLinkText&quot; al valor &quot;Haga clic en este botón&quot;. en moduleId &quot;CTA&quot;.

```
POST /rest/asset/v1/email/1032/variable/ctaLinkText.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
value=Click this button!&moduleId=CTA
```

```json
{
    "success": true,
    "warnings":[ ],
    "errors":[ ],
    "requestId": "7f34#158dc28d2f7",
    "result": [
        {
            "name":"ctaLinkText",
            "value":"Click this button!",
            "moduleScope":true,
            "moduleId":"CTA"
        }
    ]
}
```

## Aprobación

Los correos electrónicos siguen el patrón estándar para las aprobaciones de registros de recursos. Puede aprobar un borrador, desaprobar una versión aprobada y descartar un borrador existente de un correo electrónico a través de cada uno de sus propios extremos.

### Aprobar

Al llamar al extremo de aprobación, el correo electrónico se validará según las reglas de los correos electrónicos de Marketo. `from name`, `from email`, `reply to email` y `subject` deben rellenarse antes de que se pueda aprobar el correo electrónico.

```
POST /rest/asset/v1/email/{id}/approveDraft.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"15dbf#14a1832ae86",
   "result":[
      {
         "id":1362
      }
   ]
}
```

#### Desaprobar

La operación `unapprove` solo se puede realizar en correos electrónicos aprobados.

```
POST /rest/asset/v1/email/{id}/unapprove.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"3514#14a1832b0fa",
   "result":[
      {
         "id":1364
      }
   ]
}
```

#### Descartar

El correo electrónico debe estar en estado de borrador para descartarlo. Un correo electrónico aprobado no se puede descartar.

```
POST /rest/asset/v1/email/{id}/discardDraft.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"182c0#14a1832af4f",
   "result":[
      {
         "id":1362
      }
   ]
}
```

#### Eliminar

```
POST /rest/asset/v1/email/{id}/delete.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"169cd#14a1832adba",
   "result":[
      {
         "id":1361
      }
   ]
}
```

## Clonar

Marketo proporciona un método sencillo para clonar un correo electrónico. Este tipo de solicitud se realiza con un POST application/x-www-url-urlencoded y toma dos parámetros, name y folder, un objeto JSON incrustado con id y type. description también es un parámetro opcional. Si no existe ninguna versión aprobada, se clona la versión en borrador.

```
POST /rest/asset/v1/email/{id}/clone.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=Clone of Social Sharing in Email&folder={"id":239,"type":"Folder"}&description=This is a test of clone email
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "bd49#15706f43d96",
    "result": [
        {
            "id": 2250,
            "name": "Clone of Social Sharing in Email",
            "description": "This is a test of clone email",
            "createdAt": "2016-09-07T23:20:52Z+0000",
            "updatedAt": "2016-09-07T23:20:52Z+0000",
            "url": "https://app-abm.marketo.com/#EM2250B2",
            "subject": {
                "type": "Text",
                "value": "Hey There"
            },
            "fromName": {
                "type": "Text",
                "value": "SomeBody"
            },
            "fromEmail": {
                "type": "Text",
                "value": "somebody@marketo.com"
            },
            "replyEmail": {
                "type": "Text",
                "value": "somebody@marketo.com"
            },
            "folder": {
                "type": "Folder",
                "value": 239,
                "folderName": "Tradeshows and Events"
            },
            "operational": false,
            "textOnly": false,
            "publishToMSI": false,
            "webView": false,
            "status": "draft",
            "template": 24,
            "workspace": "Default",
            "isOpenTrackingDisabled": false
        }
    ]
}
```

## Enviar muestra

Puede almacenar en déclencheur un correo electrónico de ejemplo a través de la API para enviarlo al parámetro de consulta emailAddress. Si lo desea, también puede agregar un parámetro leadId para suplantar un posible cliente concreto de la base de datos, y un parámetro textOnly para enviar únicamente la versión de texto del correo electrónico.

```
POST /rest/asset/v1/email/{id}/sendSample.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
emailAddress=abe@testmail.com&textOnly=true
```

```json
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "360b#14cce7d2708",
    "result": [
        {
            "id": 2179
        }
    ]
}
```

## Obtener vista previa del email

Marketo proporciona el extremo [Obtener contenido completo del correo electrónico](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailFullContentUsingGET) para recuperar una vista previa activa de un correo electrónico tal como se enviaría a un destinatario. Este extremo solo se puede utilizar en correos electrónicos de la versión 1.0. Hay un parámetro requerido, el parámetro de ruta de ID, que es el ID del recurso de correo electrónico que desea previsualizar. Hay tres parámetros de consulta opcionales adicionales:

- estado: acepta los valores &quot;borrador&quot; o &quot;aprobado&quot;, que tomarán por defecto la versión aprobada, si se aprueba, o el borrador, si no se aprueba
- Tipo: acepta &quot;Texto&quot; o &quot;HTML&quot; y el valor predeterminado es HTML.
- leadId:. Acepta el ID entero de un posible cliente. Cuando se establece, previsualiza el correo electrónico como si lo recibiera el posible cliente designado

```
GET /rest/asset/v1/email/{id}/fullContent.json
```

```json
{
   "success": true,
   "warnings": [ ],
   "errors": [ ],
   "requestId": null,
   "result": [
      {
         "id": 339,
         "status": "draft",
         "content": "<!DOCTYPE HTML PUBLIC \"-//W3C//DTD HTML 1.01 Transitional//EN\" \"http://www.w1.org/TR/html4/loose.dtd\">\n<html>\n  <head>\n    <meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\"/>\n    <title></title>\n  </head>\n  <body>\n      <div style=\"font: 14px tahoma; width: 100%\" class=\"mktEditable\" id=\"edit_text_3\">\n        Content from testCreateEmailTemplate2\n    </div>\n  </body>\n</html>"
      }
   ]
}
```

## Reemplazar HTML

Marketo proporciona el extremo [Actualizar contenido completo del correo electrónico](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/createEmailFullContentUsingPOST) para reemplazar todo el contenido de un recurso de correo electrónico. Este punto de conexión solo se puede utilizar en correos electrónicos de la versión 1.0 que tengan la función de interfaz de usuario &quot;Editar código&quot; utilizada y en los que se haya roto la relación con la plantilla principal. Esta API está diseñada principalmente para utilizarse en recursos que se han clonado como parte de un programa y no se puede modificar con los extremos de contenido estándar. No se admiten correos electrónicos con contenido dinámico. Además, si intenta reemplazar HTML en un correo electrónico en el que la relación esté intacta, se devuelve un error.

Este extremo espera un Content-Type : multipart/form-data con el parámetro id en la ruta, el ID del correo electrónico y un parámetro en el cuerpo, contenido como documento de correo electrónico completo del HTML con el Content-Type &quot;text/html&quot;. Un documento de HTML con formato incorrecto emite una advertencia, pero puede que no permita la aprobación, mientras que la inclusión de JavaScript y/o `<script>`tags en el documento provoca que la llamada falle y emita un error.

```
POST /rest/asset/v1/email/{id}/fullContent.json
```

```
content-type: multipart/form-data; boundary=--------------------------116301888604800085728247
content-length: 599
```

```html
----------------------------116301888604800085728247
Content-Disposition: form-data; name="content"; filename="email_content.html"
Content-Type: text/html

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 1.01 Transitional//EN" "http://www.w1.org/TR/html4/loose.dtd">
 <html>
 <head>
 <meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
 <title></title>
 </head>
 <body>
 <div style="font: 14px tahoma; width: 100%" class="mktEditable" id="edit_text_3">
 EMAIL TEST CONTENT
 </div>
 </body>
 </html>
----------------------------116301888604800085728247--
```

```json
{
   "success": true,
   "warnings": [ ],
   "errors": [ ],
   "requestId": "15dbf#14a1832ae86",
   "result": [
      {
         "id": 1001
      }
   ]
}
```
