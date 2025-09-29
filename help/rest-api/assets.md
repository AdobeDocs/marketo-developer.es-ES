---
title: Recursos
feature: REST API
description: Información general sobre las API de REST de Marketo Asset para consultar por ID o nombre, navegar con la paginación y crear o actualizar carpetas, correos electrónicos, formularios, plantillas, archivos y tokens.
exl-id: 4273a5b1-1904-46e8-b583-fc6f46b388d2
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '894'
ht-degree: 2%

---

# Recursos

Marketo proporciona API para interactuar con la mayoría de los recursos de marketing y organizativos de Marketo.

## Recursos

Los recursos de Marketo incluyen:

- Carpetas
- Programas
- Correos electrónicos
- Plantillas de correo electrónico
- Páginas de destino
- Plantillas de la página de destino
- Fragmentos
- Formularios
- Tókenes
- Archivos

## API

Para obtener una lista completa de los extremos de Asset API, incluidos los parámetros y la información de modelado, consulte [Referencia del extremo de Asset API](endpoint-reference.md).

## Consulta

Assets suele tener tres patrones con los que se pueden recuperar: por id, por nombre y por navegación.  Por ID y por nombre recuperarán un solo recurso para un parámetro determinado, mientras que la exploración devolverá y permitirá la paginación a través de toda la lista de recursos de ese tipo.  Los tipos de recursos individuales tienen parámetros variables por los que se pueden filtrar, por lo que asegúrese de consultar sus documentos individuales para ver información específica.

En determinados casos, el extremo de exploración para algunos tipos de recursos no devolverá recursos secundarios, como los valores permitidos para una etiqueta y deben recuperarse individualmente mediante el extremo By Name o By Id para devolver el conjunto completo de metadatos.  Otros pueden tener puntos finales independientes por completo para recuperar objetos dependientes como campos de formulario.

### Por ID

```
GET /rest/asset/v1/folder/{id}.json?type=Folder
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "1241b#14e21ca814a",
    "result": [
        {
            "name": "Social Media",
            "description": null,
            "createdAt": "2011-03-04T17:01:32Z+0000",
            "updatedAt": "2011-03-04T17:01:32Z+0000",
            "url": null,
            "folderId": {
                "id": 341,
                "type": "Folder"
            },
            "folderType": "Email",
            "parent": {
                "id": 11,
                "type": "Folder"
            },
            "path": "/Design Studio/Default/Emails/Social Media",
            "isArchive": false,
            "isSystem": false,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 341
        }
    ]
}
```

### Por nombre

Por motivos técnicos, las API de recursos no pueden buscar nombres de recursos que contengan comas (,).  Se recomienda que la convención de nombres excluya las comas para todos los tipos de recursos.

```
GET /rest/asset/v1/file/byName.json?name=My File
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":null,
   "result":[
      {
         "id":148,
         "size":270313,
         "mimeType":"image/jpeg",
         "url":"http://mlm.devlocal.marketo.com/rs/test/assets/piKLbhVFvW",
         "folder":{
            "type":"Email",
            "id":10614
         },
         "name":"My File",
         "description":null,
         "createdAt":"2014-12-09T22:33:57Z+0000",
         "updatedAt":"2014-12-09T22:33:57Z+0000"
      }
   ]
}
```

### Examinar

La navegación por los recursos siempre permitirá dos parámetros de consulta:

- desplazamiento: un desplazamiento entero desde el que se devuelven resultados.
- maxReturn: Limita el número de registros devueltos.  Si no se establece, el valor predeterminado es 20 y tiene un máximo de 200.

```
GET /rest/asset/v1/emailTemplates.json?offset=10&maxReturn=50
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"33c4#14a1832b4a8",
   "result":[
      {
         "id":18,
         "name":"AAA0unit3CreateTestEmailTemplateName.2314673e-7bc2-47da-a1e8-66dfdd8a1f1d",
         "description":"AssetAPI: getTemplates test",
         "createdAt":"2014-11-03T19:52:58Z+0000",
         "updatedAt":"2014-11-03T19:52:58Z+0000",
         "folder":{
            "type":"Folder",
            "value":15
         },
         "status":"Draft",
         "workspace":"Default"
      },
      {
         "id":177,
         "name":"ABfRGutnwN",
         "description":"HMmHkdTRrGaRpPakdgGKICxfMunCEWDUWiThgAbInfaBXxGxSFfjKQIwerngCHRlGTnAJhKPmwlXLcsjGPtWEiILGyeIJTNVHoHg",
         "createdAt":"2014-11-20T19:31:06Z+0000",
         "updatedAt":"2014-11-20T19:31:06Z+0000",
         "folder":{
            "type":"Folder",
            "value":15
         },
         "status":"Draft",
         "workspace":"Default"
      },
      {
         "id":148,
         "name":"ADVHJBQLyw",
         "description":null,
         "createdAt":"2014-11-20T06:42:57Z+0000",
         "updatedAt":"2014-11-20T06:42:57Z+0000",
         "folder":{
            "type":"Folder",
            "value":15
         },
         "status":"Draft",
         "workspace":"Default"
      }
   ]
}
```

## Crear y actualizar

Para tipos de recursos simples como carpetas, tokens y archivos, normalmente solo hay un único punto de conexión para la creación y, a continuación, un punto de conexión adicional para actualizar registros por ID.  Assets se crean con un nombre que siempre es obligatorio y, a continuación, la respuesta de creación o actualización devuelve todos los metadatos y los ID.

Por ejemplo, así es como se crea un token:

```
POST /rest/asset/v1/folder/{id}/tokens.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=April Fools&value=2015-04-01&type=date&folderType=Folder
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "e3c2#14e280db5dc",
    "result": [
        {
            "folder": {
                "type": "Folder",
                "value": 416
            },
            "tokens": [
                {
                    "name": "April Fools",
                    "type": "date",
                    "value": "2015-04-01",
                    "computedUrl": "https://app-abm.marketo.com/#MF1047C3"
                }
            ]
        }
    ]
}
```

Para actualizar una carpeta, debe hacer lo siguiente:

```
POST /rest/asset/v1/folder/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
type=Folder&description=This is a test (update 01)
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "c5b2#14e1f3954bf",
    "result": [
        {
            "name": "Learning - deverly",
            "description": "This is a test (update 01)",
            "createdAt": "2015-03-17T00:17:02Z+0000",
            "updatedAt": "2015-06-23T07:02:07Z+0000",
            "url": "https://app-abm.marketo.com/#MF1044A1",
            "folderId": {
                "id": 407,
                "type": "FOLDER"
            },
            "folderType": "Marketing Folder",
            "parent": {
                "id": 15,
                "type": "FOLDER"
            },
            "path": "/Marketing Activities/Default/Learning - deverly",
            "isArchive": false,
            "isSystem": false,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 407
        }
    ]
}
```

Otros recursos tienen estructuras más complejas y requieren actualizaciones en subsecciones u objetos secundarios adicionales, y finalmente deben someterse a aprobación antes de poder utilizarse.  Estos tipos de recursos incluyen Forms, correos electrónicos, plantillas de correo electrónico, páginas de aterrizaje y plantillas de página de aterrizaje.  Cada una de ellas tendrá un único extremo para crear un registro y, a continuación, extremos adicionales para actualizar las secciones de metadatos, contenido y contenido.

Por ejemplo, para crear una página de aterrizaje, debe llamar a su punto de conexión de creación con un ID de plantilla, recuperar sus secciones de contenido y actualizar cada una de ellas individualmente para añadir contenido antes de aprobarla para que se pueda implementar en tiempo real.

### Creación compleja

Las páginas de aterrizaje primero requieren crear un recurso de página de aterrizaje mediante una plantilla principal.  Esto crea una nueva página de aterrizaje que contiene el contenido predeterminado de la plantilla para cada sección de contenido.

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

#### Obtener secciones

Para rellenar el contenido de una página de aterrizaje, debe recuperar la lista de secciones de contenido y, a continuación, realizar actualizaciones individuales para cualquier sección que se desvíe de la plantilla.

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

#### Actualizar sección

```
POST /rest/asset/v1/landingPage/{id}/content/{contentId}.json?type=Form&value=1
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "5c37#154ea32cf11",
    "result": [
        {
            "id": 174
        }
    ]
}
```

## Aprobación

Muchos tipos de recursos tienen asociado un sistema de borrador y aprobación, que incluye correos electrónicos, páginas de aterrizaje, fragmentos de código, Forms y sus plantillas correspondientes.  Si intenta aprobar un recurso, se evaluará con un conjunto específico de reglas de validación y, a continuación, se establecerá en un estado aprobado o se devolverá un motivo de error.  Para estos tipos de recursos, cada vez que se realiza una actualización del contenido de un recurso concreto, los cambios se realizan en un borrador del recurso, lo que no afecta a la versión aprobada.  Esto permite realizar cambios en el contenido de forma segura sin afectar a las versiones activas del recurso.  Los cambios se pueden aplicar a la versión activa utilizando el punto de conexión de aprobación.  Esto también borra el estado de borrador del recurso hasta que se apliquen actualizaciones adicionales.

```
POST /rest/asset/v1/emailTemplate/{id}/approveDraft.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"abe2#14a1832a97d",
   "result":[
      {
         "id":338,
         "name":"lvAVYMZqPS",
         "description":"fZLJQSJRvnYbjGTUpIHHqDOuQgQzXQcWIXoOUPwrVLdMHKcbRqwLoSLkWZTUmaMiCIJSfQiufnnrgITUIqjuAPBLpmliiKuIUFYG",
         "createdAt":"2014-12-05T02:06:21Z+0000",
         "updatedAt":"2014-12-05T02:06:21Z+0000",
         "folder":{
            "type":"Folder",
            "value":15
         },
         "status":"Approved",
         "workspace":"Default"
      }
   ]
}
```

La aprobación correcta reemplaza la versión activa anterior con la versión actualizada.

La eliminación de borradores también está disponible a través de un punto final para cada tipo de recurso válido.  Si se utiliza en un recurso que está en un estado aprobado con borrador, se descartará el borrador actual y los cambios pendientes que tenga.  Si se utiliza en un recurso que actualmente no tiene una versión aprobada, no hará nada y devolverá un error.  Los recursos solo de borrador se pueden eliminar, pero no se pueden descartar.

```
POST /rest/asset/v1/emailTemplate/{id}/discardDraft.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"17bfa#14a1832b3c4",
   "result":[
      {
         "id":344,
         "name":"LkilkvKrkp",
         "description":"yAyUEXuWMtdhpODUmnCkGjpBcyEKnYucxaSoTyYeQzyNbYanxCXWPOzwiIWmeXPUwjfGAUmgnxlhgOPluVqwNittuvxJmNTaHxYM",
         "createdAt":"2014-12-05T02:06:23Z+0000",
         "updatedAt":"2014-12-05T02:06:23Z+0000",
         "folder":{
            "type":"Folder",
            "value":15
         },
         "status":"Draft",
         "workspace":"Default"
      }
   ]
}
```

Assets también se puede desaprobar si está en un estado de solo aprobación.  Esto eliminará cualquier versión activa del recurso y devolverá el recurso a un estado de solo borrador, al tiempo que descartará cualquier borrador asociado.  Esta acción solo se puede realizar en la mayoría de los recursos si no se está utilizando en ninguna parte de Marketo, como un correo electrónico al que se hace referencia en un paso de flujo de envío de correo electrónico o un fragmento incrustado en un correo electrónico.

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

## Eliminar

Assets con estados de aprobación y borrador, excepto para formularios, no se puede eliminar mientras se aprueba y debe desaprobarse antes de eliminarse.  Por lo general, las eliminaciones solo se pueden realizar cuando un recurso no está aprobado y está fuera de uso, y en el caso de las carpetas, está vacío de recursos.  Una excepción notable son los programas, que pueden ser eliminados junto con todo su contenido secundario, siempre y cuando el programa y su contenido no estén en uso en ningún lugar fuera de los límites del programa.

```
POST /rest/asset/v1/program/{id}/delete.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "16501#14db042c6b7",
    "result": [
        {
            "id": 1109
        }
    ]
}
```

## Tiempos de espera

Las API de recursos tienen un tiempo de espera de 300 segundos
