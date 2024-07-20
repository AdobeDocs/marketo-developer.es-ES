---
title: Carpetas
feature: REST API
description: Manipular carpetas con la API de Marketo.
exl-id: 4b55c256-ef0a-42b4-9548-ff8a4106f064
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '1008'
ht-degree: 1%

---

# Carpetas

[Referencia de extremo de carpetas](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders)

Las carpetas son el recurso organizativo principal en Marketo y todos los demás tipos de recursos tienen al menos una carpeta como elemento principal. Esta carpeta principal puede ser una carpeta puramente organizativa o un programa, que tiene una relación funcional con otros tipos de recursos y también puede ser la carpeta principal de otros recursos. Las carpetas se pueden crear, consultar, actualizar y eliminar a través de la API, y también permiten recuperar una lista de su contenido. Aunque los programas se pueden devolver consultando la API de carpetas, la creación, actualización y eliminación de programas se deben realizar mediante la API de programas.

## Consulta

La consulta de carpetas sigue los tipos de consulta estándar para los recursos de [por id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderByIdUsingGET), [por nombre](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderByNameUsingGET) y [exploración](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderUsingGET).

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

El parámetro de tipo es obligatorio y debe ser &quot;Carpeta&quot; o &quot;Programa&quot;.  El tipo dicta si la búsqueda en la carpeta se realiza con un ID de carpeta o un ID de programa. Para este extremo, sólo se devuelve un único registro en el resultado Array. Tenga en cuenta el parámetro folderType en la respuesta. Esto puede indicar muchos tipos diferentes de carpetas. Las carpetas de actividades de Marketo tienen un tipo de carpeta de marketing o de programa, que pueden contener muchos tipos diferentes de recursos, mientras que las carpetas de Design Studio tienen un tipo correspondiente al tipo de recurso que pueden contener. Por ejemplo, una carpeta con folderType de &quot;Correo electrónico&quot; puede contener solo correos electrónicos u otras subcarpetas, que pueden tener un folderType de Correo electrónico o Plantilla de correo electrónico. Los tipos pueden incluir:

- Correo electrónico
- Plantilla de email
- Página de aterrizaje
- Plantilla de la página de destino
- Fragmento
- Archivo

### Por nombre

[También se permite la consulta por nombre](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderByNameUsingGET). La consulta por extremo de nombre tiene nombre como único parámetro requerido. Nombre realiza una coincidencia de cadena exacta con el campo de nombre de las carpetas de la instancia y devuelve los resultados de cada carpeta que coincida con ese nombre. También tiene los parámetros de consulta opcionales de &quot;tipo&quot;, que pueden ser Carpeta o Programa, &quot;raíz&quot;, el ID de la carpeta para buscar o &quot;espacio de trabajo&quot;, el nombre del espacio de trabajo en el que buscar. Si se establece el parámetro root, también se debe establecer el parámetro type.

```
GET /rest/asset/v1/folder/byName.json?name=Test%2010%20-%20deverly
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "19#14e1f2f3688",
    "result": [
        {
            "name": "Test 10 - deverly",
            "description": "This is a test",
            "createdAt": "2015-06-23T06:27:04Z+0000",
            "updatedAt": "2015-06-23T06:27:04Z+0000",
            "url": "https://app-abm.marketo.com/#MF1070A1",
            "folderId": {
                "id": 454,
                "type": "FOLDER"
            },
            "folderType": "Marketing Folder",
            "parent": {
                "id": 416,
                "type": "FOLDER"
            },
            "path": "/Marketing Activities/Default/Marketing Programs - deverly/Test 10 - deverly",
            "isArchive": false,
            "isSystem": false,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 454
        }
    ]
}
```

Al buscar por nombre, es importante tener en cuenta que tanto Marketing Activities como Design Studio son sus propias carpetas raíz, de modo que se pueden recuperar por nombre y utilizar para recorrer el resto de la jerarquía de carpetas en una instancia de destino.

### Examinar

Las carpetas también se pueden [recuperar de forma masiva](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderUsingGET). El parámetro &quot;root&quot; se puede utilizar para especificar la carpeta principal en la que se realizará la consulta y tiene el formato de un objeto JSON incrustado como el valor del parámetro de consulta. La raíz tiene dos miembros:

1. id: el ID de la carpeta o el programa.
1. type: Carpeta o Programa, según el tipo de carpeta raíz que se va a examinar.

Si no se conoce la carpeta raíz o si se desea recuperar todas las carpetas de un área determinada, la raíz se puede especificar como las áreas &quot;Actividades de marketing&quot;, &quot;Design Studio&quot; o &quot;Base de datos de posibles clientes&quot;. Los identificadores de cada uno de ellos se pueden recuperar a través de la API [Obtener carpeta por nombre](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderByNameUsingGET) y especificando el nombre del área deseada.

Al igual que otros extremos de recuperación masiva de recursos, offset y maxReturn son parámetros opcionales para la paginación.   Otros parámetros opcionales son:

- workSpace: nombre del espacio de trabajo al que filtrar.
- maxDepth: el número máximo de niveles que se atravesarán en la jerarquía de carpetas. Si se establece en 0, solo se devuelve la carpeta especificada en la raíz. Si no se especifica, el valor predeterminado es 2.

```
GET /rest/asset/v1/folders.json?root={"id":14,"type":"Folder"}
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "9bd8#14e1f49047c",
    "result": [
        {
            "name": "Marketing Activities",
            "description": "Root node for the Marketing Activities app area",
            "createdAt": "2010-03-27T18:27:45Z+0000",
            "updatedAt": "2010-03-27T18:27:45Z+0000",
            "url": null,
            "folderId": {
                "id": 14,
                "type": "Folder"
            },
            "folderType": "Zone",
            "parent": null,
            "path": "/Marketing Activities",
            "isArchive": false,
            "isSystem": true,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 14
        },
        {
            "name": "Default",
            "description": "Root node of the Marketing activities Default",
            "createdAt": "2010-03-27T18:27:45Z+0000",
            "updatedAt": "2010-03-27T18:27:45Z+0000",
            "url": null,
            "folderId": {
                "id": 15,
                "type": "Folder"
            },
            "folderType": "Zone",
            "parent": {
                "id": 14,
                "type": "Folder"
            },
            "path": "/Marketing Activities/Default",
            "isArchive": false,
            "isSystem": true,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 15
        },
        {
            "name": "Archive",
            "description": "",
            "createdAt": "2010-03-27T18:28:17Z+0000",
            "updatedAt": "2010-03-27T18:28:17Z+0000",
            "url": "https://app-abm.marketo.com/#MF157A1",
            "folderId": {
                "id": 310,
                "type": "Folder"
            },
            "folderType": "Marketing Folder",
            "parent": {
                "id": 15,
                "type": "Folder"
            },
            "path": "/Marketing Activities/Default/Archive",
            "isArchive": false,
            "isSystem": false,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 310
        }
    ]
}
```

## Estructura de respuesta

Gran parte de la estructura de respuesta de carpetas se explica por sí misma, pero algunos campos merecen la pena anotarse individualmente. Los campos `folderId` y principal son objetos JSON que incluyen el identificador explícito y el tipo de la carpeta en sí. Este tipo es el que la API utiliza en las consultas, la raíz y los parámetros principales para garantizar una delineación adecuada entre los tipos de carpetas Carpeta y Programa. `folderType` refleja el uso de la carpeta, que puede ser una de &quot;Carpeta de marketing&quot;, &quot;Programa&quot;, &quot;Correo electrónico&quot;, &quot;Plantilla de correo electrónico&quot;, &quot;Página de aterrizaje&quot;, &quot;Plantilla de página de aterrizaje&quot;, &quot;Fragmento&quot;, &quot;Imagen&quot;, &quot;Zona&quot; o &quot;Archivo&quot;.  Los tipos Carpeta y programa de marketing indican que existen en las actividades de marketing y pueden contener varios tipos de recursos. Los demás tipos indican que pueden contener solo ese tipo de recurso, subcarpetas y la versión de plantilla de ese tipo, si corresponde. El tipo Zone representa las carpetas de nivel raíz que se encuentran en las actividades de marketing.

La ruta de una carpeta muestra su jerarquía en el árbol de carpetas, de forma similar a una ruta de estilo Unix. La primera entrada en la ruta siempre será Marketing Activities o Design Studio. Si la instancia de destino tiene espacios de trabajo, la segunda entrada de la ruta será el nombre del espacio de trabajo propietario. El campo `url` muestra la dirección URL explícita del recurso en la instancia designada. Este no es un vínculo universal y debe autenticarse como usuario para funcionar correctamente. `isSystem` indica si la carpeta es una carpeta del sistema. Si se establece en true, la carpeta en sí es de solo lectura, aunque las carpetas se pueden crear como elementos secundarios.

## Crear y actualizar

[La creación de carpetas](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/createFolderUsingPOST) es sencilla y se ejecuta con un POST application/x-www-form-urlencoded que tiene dos parámetros obligatorios, &quot;name&quot;, una cadena y &quot;parent&quot;, el elemento principal en el que crear la carpeta, que es un objeto JSON incrustado con dos miembros, id y type, Folder o Program, según el tipo de la carpeta de destino. Opcionalmente, también se puede incluir una &quot;descripción&quot; o una cadena que puede tener hasta 2000 caracteres.

```
POST /rest/asset/v1/folders.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
parent={"id":416,"type":"Folder"}&name=Test 10 - deverly&description=This is a test
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "111be#14e1f193e31",
    "result": [
        {
            "name": "Test 10 - deverly",
            "description": "This is a test",
            "createdAt": "2015-06-23T06:27:04Z+0000",
            "updatedAt": "2015-06-23T06:27:04Z+0000",
            "url": "https://app-abm.marketo.com/#MF1070A1",
            "folderId": {
                "id": 454,
                "type": "FOLDER"
            },
            "folderType": "Marketing Folder",
            "parent": {
                "id": 416,
                "type": "FOLDER"
            },
            "path": "/Marketing Activities/Default/Test 10 - deverly",
            "isArchive": false,
            "isSystem": false,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 454
        }
    ]
}
```

Las actualizaciones de las carpetas se realizan mediante un extremo independiente, y la descripción, el nombre y `isArchive` son parámetros opcionales para la actualización. Si `isArchive` se cambia mediante una actualización, el resultado es que la carpeta se archiva, si se cambia a true, o se desarchiva, si se cambia a false, en la interfaz de usuario de Marketo. Los programas no se pueden actualizar con esta API.

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

### Eliminar

Las eliminaciones se pueden realizar en carpetas únicas si están vacías, lo que significa que no contienen recursos ni subcarpetas. Si una carpeta es de tipo Programa o tiene el campo isSystem establecido en true, no se puede eliminar con esta API.

```
POST /rest/asset/v1/folder/{id}/delete.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "4180#14e1f3fc017",
    "result": [
        {
            "id": 453
        }
    ]
}
```
