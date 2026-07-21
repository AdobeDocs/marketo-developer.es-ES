---
title: Archivos
feature: REST API
description: Guía de los archivos de API de REST de Marketo consulta por ID o nombre, exploración con carpeta y desplazamiento, creación o actualización mediante carga de varias partes, insertOnly, tipos MIME, sin flujo continuo
exl-id: 17361cdc-2309-442c-803c-34ce187aee1a
TQID: https://experienceleague.adobe.com/qH8zFwjJkTWHlCj1VHNiTiLK3mNOJFS83cnjEj2qjpA
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: f82558ea-6af5-44eb-a424-5b3389abb0a3
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 274
ht-degree: 1%

---

# Archivos

[Referencia de extremo de archivos](https://developer.adobe.com/marketo-apis/api/asset#tag/Files)

Utilice la API de REST de archivos para administrar imágenes, scripts, documentos, hojas de estilo y otros archivos almacenados en una suscripción de Marketo.

El almacenamiento de archivos de Marketo no está optimizado para aplicaciones que requieren mucho ancho de banda. Utilice un servicio de streaming dedicado para audio y vídeo.

## Consulta

Archivos de consulta [por id.](https://developer.adobe.com/marketo-apis/api/asset#tag/Files/operation/getFileByIdUsingGET), [por nombre](https://developer.adobe.com/marketo-apis/api/asset#tag/Files/operation/getFileByNameUsingGET) o por [exploración](https://developer.adobe.com/marketo-apis/api/asset#tag/Files/operation/getFilesUsingGET).

### Por ID

```http
GET /rest/asset/v1/file/{id}.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":null,
   "result":[
      {
         "id":147,
         "size":61346,
         "mimeType":"image/jpeg",
         "url":"http://mlm.devlocal.marketo.com/rs/test/assets/rYXNeQFFVu",
         "folder":{
            "type":"Email",
            "id":10613
         },
         "name":"rYXNeQFFVu",
         "description":null,
         "createdAt":"2014-12-09T22:33:57Z+0000",
         "updatedAt":"2014-12-09T22:33:57Z+0000"
      }
   ]
}
```

### Por nombre

Especifique el nombre del archivo utilizando el parámetro `name` requerido.

```http
GET /rest/asset/v1/file/byName.json?name=foo.png
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "9049#15918a76619",
    "result": [
        {
            "id": 46488,
            "size": 13987,
            "mimeType": "image/png",
            "url": "http://na-abm.marketo.com/rs/mktodemoaccount88/assets/foo.png",
            "folder": {
                "type": "Image",
                "id": 436,
                "name": "My Images"
            },
            "name": "foo.png",
            "description": "This is a test file.",
            "createdAt": "2015-05-06T22:16:58Z+0000",
            "updatedAt": "2015-05-06T22:19:29Z+0000"
        }
    ]
}
```

### Examinar

El extremo de exploración acepta tres parámetros opcionales:

- `folder`: la carpeta principal como objeto JSON que contiene los atributos `id` y `type`.
- `offset`: posición en la que empezar a recuperar entradas. El valor predeterminado es 0. Usar con `maxReturn`.
- `maxReturn`: número máximo de entradas que se devolverán. El valor predeterminado es 20 y el máximo es 200.

```http
GET /rest/asset/v1/files.json?folder={"id":436, "type": "Folder"}&maxReturn=3
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "17e4e#14e23372d80",
    "result": [
        {
            "id": 46484,
            "size": 1454,
            "mimeType": "text/plain",
            "url": "http://na-abm.marketo.com/rs/mktodemoaccount88/assets/websites.png",
            "folder": {
                "type": "Image",
                "id": 436,
                "name": "My Images - deverly"
            },
            "name": "websites.png",
            "description": "This is a test file.",
            "createdAt": "2015-05-06T20:15:58Z+0000",
            "updatedAt": "2015-06-22T02:12:36Z+0000"
        },
        {
            "id": 46486,
            "size": 4169,
            "mimeType": "image/png",
            "url": "http://na-abm.marketo.com/rs/mktodemoaccount88/assets/mobile.png",
            "folder": {
                "type": "Image",
                "id": 436,
                "name": "My Images - deverly"
            },
            "name": "mobile.png",
            "description": null,
            "createdAt": "2015-05-06T22:13:33Z+0000",
            "updatedAt": "2015-05-06T22:13:33Z+0000"
        },
        {
            "id": 46488,
            "size": 13987,
            "mimeType": "image/png",
            "url": "http://na-abm.marketo.com/rs/mktodemoaccount88/assets/foo.png",
            "folder": {
                "type": "Image",
                "id": 436,
                "name": "My Images - deverly"
            },
            "name": "foo.png",
            "description": "This is a test file.",
            "createdAt": "2015-05-06T22:16:58Z+0000",
            "updatedAt": "2015-05-06T22:19:29Z+0000"
        }
    ]
}
```

## Crear y actualizar

Usar una solicitud `multipart/form-data` para [crear un archivo](https://developer.adobe.com/marketo-apis/api/asset#tag/Files/operation/createFileUsingPOST). Se requieren los parámetros `name`, `folder` y `file`. Los parámetros `description` y `insertOnly` son opcionales. Si es true, `insertOnly` impide que la solicitud actualice un archivo existente con el mismo nombre.

Para el parámetro `file`, incluya un `filename` en el encabezado `Content-Disposition`. Incluya también el encabezado `Content-Type` del archivo. Marketo utiliza este tipo MIME al servir el archivo.

```http
POST /rest/asset/v1/files.json
```

```html
------WebKitFormBoundary2VyWOacQSupl4gUL
Content-Disposition: form-data; name="file"; filename="marketo.html"
Content-Type: text/html
<html>
<body>
<h1>Test Page - marketo.html</h1>
</body>
</html>
------WebKitFormBoundary2VyWOacQSupl4gUL
Content-Disposition: form-data; name="name"
marketo.html
------WebKitFormBoundary2VyWOacQSupl4gUL
Content-Disposition: form-data; name="folder"
{"id":436,"type":"Folder"}
------WebKitFormBoundary2VyWOacQSupl4gUL
Content-Disposition: form-data; name="description"
This is a test file
------WebKitFormBoundary2VyWOacQSupl4gUL—
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "278d#14e23316f63",
    "result": [
        {
            "id": 46960,
            "size": 69,
            "mimeType": "text/html",
            "url": "http://na-abm.marketo.com/rs/mktodemoaccount88/assets/marketo.html",
            "folder": {
                "type": "Image",
                "id": 436,
                "name": "My Images - deverly"
            },
            "name": "marketo.html",
            "description": "This is a test file",
            "createdAt": "2015-06-24T01:31:59Z+0000",
            "updatedAt": "2015-06-24T01:31:59Z+0000"
        }
    ]
}
```

Para [actualizar un archivo](https://developer.adobe.com/marketo-apis/api/asset#tag/File-Contents/operation/updateContentUsingPOST), especifique su ID. El parámetro `file` tiene los mismos requisitos que la creación de archivos.

```http
POST /rest/asset/v1/file/{id}/content.json
```

```html
------WebKitFormBoundary2VyWOacQSupl4gUL
Content-Disposition: form-data; name="file"; filename="marketo.html"
Content-Type: text/html
<html>
<body>
<h1>Test Page - marketo.html</h1>
</body>
</html>
------WebKitFormBoundary2VyWOacQSupl4gUL--
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": null,
    "result": [
        {
            "id": 67,
            "size": 512000,
            "mimeType": "image/png",
            "url": "http://pages.devlocal.marketo.com/rs/test/assets/aLZiwCkXor",
            "folder": {
                "type": "Email",
                "id": 10391
            },
            "name": "aLZiwCkXor",
            "description": null,
            "createdAt": "2014-12-18T09:03:43Z+0000",
            "updatedAt": "2015-01-07T04:40:20Z+0000"
        }
    ]
}
```
