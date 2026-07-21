---
title: Carpetas
feature: REST API
description: Guía de API de REST de Marketo para carpetas que abarcan crear, actualizar, eliminar, consultar por id y nombre, examinar por lotes con raíz, espacio de trabajo, maxDepth y paginación.
exl-id: 4b55c256-ef0a-42b4-9548-ff8a4106f064
TQID: https://experienceleague.adobe.com/OxCNdy8qW6jwq8u57RF9mqVKPVvH99UmuiOBjFprHCM
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
  - id: d65b4a73-87a3-4d56-b638-74e74d9939ce
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 806
ht-degree: 1%

---

# Carpetas

[Referencia de extremo de carpetas](https://developer.adobe.com/marketo-apis/api/asset#tag/Folders)

Las carpetas son los recursos organizativos principales de Marketo. Todos los demás tipos de recursos tienen al menos un elemento principal que es una carpeta o un programa. Una carpeta es puramente organizativa, mientras que un programa tiene una relación funcional con otros tipos de recursos y también puede contener recursos.

Utilice la API Folders para crear, consultar, actualizar y eliminar carpetas o recuperar su contenido. Las consultas de carpeta pueden devolver Programas, pero debe utilizar la API de Programas para crear, actualizar o eliminar un Programa.

## Consulta

Las carpetas admiten los patrones de consulta de recursos estándar: [por id.](https://developer.adobe.com/marketo-apis/api/asset#tag/Folders/operation/getFolderByIdUsingGET), [por nombre](https://developer.adobe.com/marketo-apis/api/asset#tag/Folders/operation/getFolderByNameUsingGET) y por [exploración](https://developer.adobe.com/marketo-apis/api/asset#tag/Folders/operation/getFolderUsingGET).

### Por ID

```http
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

El parámetro `type` es obligatorio y debe ser `Folder` o `Program`. Determina si el extremo busca un ID de carpeta o un ID de programa. El extremo devuelve un registro en la matriz de resultados.

La respuesta `folderType` identifica lo que la carpeta puede contener. Las carpetas de Actividades de marketing tienen un tipo de Carpeta de marketing o Programa y pueden contener varios tipos de recursos. Las carpetas de Design Studio tienen un tipo que corresponde a los recursos que pueden contener. Por ejemplo, una carpeta Correo electrónico puede contener correos electrónicos y subcarpetas con un tipo de carpeta Correo electrónico o Plantilla de correo electrónico.

Los tipos de carpeta incluyen:

- Correo electrónico
- Plantilla de correo electrónico
- Página de destino
- Plantilla de la página de destino
- Fragmento
- Archivo

### Por nombre

El extremo [query by name](https://developer.adobe.com/marketo-apis/api/asset#tag/Folders/operation/getFolderByNameUsingGET) requiere `name`, que realiza una coincidencia exacta con los nombres de carpeta y devuelve todas las carpetas coincidentes.

El extremo también acepta estos parámetros opcionales:

- `type`: el tipo de carpeta, `Folder` o `Program`.
- `root`: el identificador de la carpeta que se va a buscar. Si establece `root`, también debe establecer `type`.
- `workspace`: nombre del área de trabajo que se va a buscar.

```http
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

Las actividades de marketing y Design Studio son carpetas raíz. Recupere la raíz por su nombre y, a continuación, utilícela para recorrer la jerarquía de carpetas en la instancia de destino.

### Examinar

También puede [recuperar carpetas de forma masiva](https://developer.adobe.com/marketo-apis/api/asset#tag/Folders/operation/getFolderUsingGET). Utilice el parámetro `root` para especificar la carpeta principal en la que desea realizar la consulta. Pase `root` como un objeto JSON incrustado con dos miembros:

1. `id`: ID de la carpeta o programa.
1. `type`: `Folder` o `Program`, según el tipo de carpeta raíz.

Si no conoce la carpeta raíz o desea recuperar todas las carpetas de un área, utilice la raíz Marketing Activities, Design Studio o la base de datos de posibles clientes. Recupere el ID raíz pasando el nombre del área a la API [Obtener carpeta por nombre](https://developer.adobe.com/marketo-apis/api/asset#tag/Folders/operation/getFolderByNameUsingGET).

Al igual que con otros extremos de recuperación masiva de recursos, utilice los parámetros opcionales `offset` y `maxReturn` para la paginación. Otros parámetros opcionales son:

- `workSpace`: nombre del área de trabajo por el que filtrar.
- `maxDepth`: número máximo de niveles que se atravesarán en la jerarquía de carpetas. El valor 0 devuelve solamente la carpeta especificada por `root`. El valor predeterminado es 2.

```http
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

Los campos `folderId` y `parent` son objetos JSON que contienen el tipo y el identificador de la carpeta. La API usa este tipo en los parámetros de consulta `root` y `parent` para distinguir los tipos de carpeta Carpeta y Programa.

El campo `folderType` describe cómo se usa la carpeta. Su valor puede ser Carpeta de marketing, Programa, Correo electrónico, Plantilla de correo electrónico, Página de aterrizaje, Plantilla de página de aterrizaje, Fragmento, Imagen, Zona o Archivo. La carpeta y el programa de marketing existen en las actividades de marketing y pueden contener varios tipos de recursos. Los demás tipos de carpeta solo contienen el tipo de recurso, las subcarpetas y la versión de plantilla correspondientes de ese tipo de recurso, cuando corresponda. Zone representa una carpeta de nivel raíz en las actividades de marketing.

La carpeta `path` muestra su jerarquía como una ruta de estilo Unix. La primera entrada es siempre Marketing Activities o Design Studio. Si la instancia tiene espacios de trabajo, la segunda entrada es el nombre del espacio de trabajo propietario.

El campo `url` contiene la dirección URL del recurso para la instancia designada. No es un vínculo universal y requiere la autenticación del usuario. El campo `isSystem` indica si la carpeta es una carpeta del sistema de solo lectura. Puede crear carpetas secundarias en una carpeta del sistema.

## Crear y actualizar

Para [crear una carpeta](https://developer.adobe.com/marketo-apis/api/asset#tag/Folders/operation/createFolderUsingPOST), envíe una petición POST de `application/x-www-form-urlencoded` con estos parámetros:

- `name`: cadena requerida que contiene el nombre de la carpeta.
- `parent`: objeto JSON incrustado requerido que contiene `id` y `type`. El tipo es `Folder` o `Program`, según el elemento principal.
- `description`: cadena opcional de hasta 2000 caracteres.

```http
POST /rest/asset/v1/folders.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
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

Use el extremo de actualización para cambiar los parámetros `description`, `name` o `isArchive` opcionales. Al establecer `isArchive` en `true`, se archiva la carpeta en la interfaz de usuario de Marketo. Si se establece en `false`, se eliminará la carpeta del archivo.

No puede actualizar programas con esta API.

```http
POST /rest/asset/v1/folder/{id}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```sql
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

Solo puede eliminar una carpeta si no contiene recursos ni subcarpetas. No puede usar esta API para eliminar un programa o una carpeta cuyo campo `isSystem` sea `true`.

```http
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
