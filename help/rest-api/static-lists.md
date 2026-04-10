---
title: Listas estáticas
feature: REST API, Static Lists
description: Utilice las API de REST de Marketo para consultar, crear, actualizar y eliminar listas estáticas, con puntos finales para ID, nombre y examinar, ámbitos de carpetas, paginación y filtros de fecha.
exl-id: 20679fd2-fae2-473e-84bc-cb4fdf2f5151
source-git-commit: e2606d6cb12c572603ff069617de58417e43ca63
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 1%

---

# Listas estáticas

[Referencia de extremo de listas estáticas](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists)

Marketo ofrece un conjunto de API de REST para realizar operaciones de CRUD en listas estáticas. Estas API siguen el patrón de interfaz estándar para las API de recursos que proporcionan las opciones de Consulta, Crear, Actualizar y Eliminar.

Para las operaciones de la base de datos de posibles clientes en los miembros de la lista, vea [Pertenencia a la lista](list-membership.md).

## Consulta

La consulta de listas estáticas sigue los tipos de consulta estándar para los recursos de [por id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET), [por nombre](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByNameUsingGET) y [examinar](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListsUsingGET).

### Por ID

[La consulta del identificador &#x200B;](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET) toma una sola lista estática `id` como parámetro de ruta de acceso y devuelve un único registro de lista estática.

```http
GET /rest/asset/v1/staticList/{id}.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "843c#1641f969e96",
    "result": [
        {
            "id": 1021,
            "name": "Foundation Seed List",
            "createdAt": "2017-07-27T01:38:33Z+0000",
            "updatedAt": "2017-07-27T01:39:26Z+0000",
            "folder": {
                "id": 13,
                "type": "Folder"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1021A1"
        }
    ]
}
```

#### Por nombre

[La consulta por nombre](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByNameUsingGET) toma una lista estática `name` como parámetro y devuelve un único registro de lista estática. Se realiza una coincidencia de cadena exacta con todos los nombres de lista estática de la instancia y se devuelve un resultado para la lista estática que coincida con ese nombre.

```http
GET /rest/asset/v1/staticList/byName.json?name=Foundation Seed List
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "28ab#1641fa246b9",
    "result": [
        {
            "id": 1021,
            "name": "Foundation Seed List",
            "createdAt": "2017-07-27T01:38:33Z+0000",
            "updatedAt": "2017-07-27T01:39:26Z+0000",
            "folder": {
                "id": 13,
                "type": "Folder"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1021A1"
        }
    ]
}
```

#### Examinar

Las listas estáticas también se pueden [recuperar en lotes](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListsUsingGET). El parámetro `folder` se puede usar para especificar la carpeta principal en la que se realizará la consulta y tiene el formato de un objeto JSON que contiene `id` y `type`. Al igual que otros extremos de recuperación masiva de recursos, `offset` y `maxReturn` son parámetros opcionales que se pueden utilizar para la paginación. Los parámetros `earliestUpdatedAt` y `latestUpdatedAt` le permiten establecer marcas de agua de fecha y hora bajas y altas para devolver listas estáticas creadas o actualizadas dentro del intervalo dado. Los valores de fecha y hora deben ser cadenas ISO-8601 válidas y no deben incluir milisegundos.

```http
GET /rest/asset/v1/staticLists.json?folder={"id":13,"type":"Folder"}
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "2dc0#1641f846633",
    "result": [
        {
            "id": 1021,
            "name": "Foundation Seed List",
            "createdAt": "2017-07-27T01:38:33Z+0000",
            "updatedAt": "2017-07-27T01:39:26Z+0000",
            "folder": {
                "id": 13,
                "type": "Folder"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1021A1"
        },
        {
            "id": 1022,
            "name": "Blacklist Seed List",
            "createdAt": "2017-07-27T23:19:33Z+0000",
            "updatedAt": "2017-07-27T23:21:29Z+0000",
            "folder": {
                "id": 13,
                "type": "Folder"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1022A1"
        },
        {
            "id": 1023,
            "name": "Possible Duplicates Seed List",
            "createdAt": "2017-07-28T00:10:02Z+0000",
            "updatedAt": "2017-07-28T00:11:22Z+0000",
            "folder": {
                "id": 13,
                "type": "Folder"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1023A1"
        }
    ]
}
```

## Crear y actualizar

[La creación de una lista estática](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/createStaticListUsingPOST) se ejecuta con un POST de `application/x-www-form-urlencoded` con dos parámetros obligatorios. El parámetro `folder` se usa para especificar la carpeta principal en la que se creará la lista estática y tiene el formato de objeto JSON que contiene `id` y `type`. El parámetro `name` se usa para asignar un nombre a la lista estática y debe ser único. Opcionalmente, el parámetro `description` se puede usar para describir la lista estática.

```http
POST /rest/asset/v1/staticLists.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
folder={"id":1034,"type":"Program"}&name=My Static List
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "1269d#164209d6e1e",
    "result": [
        {
            "id": 1027,
            "name": "My Static List",
            "createdAt": "2018-06-21T04:32:25Z+0000",
            "updatedAt": "2018-06-21T04:32:25Z+0000",
            "folder": {
                "id": 1034,
                "type": "Program"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1027A1"
        }
    ]
}
```

[La actualización de una lista estática](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/updateStaticListUsingPOST) se realiza a través de un extremo independiente con dos parámetros opcionales. El parámetro `description` se puede usar para actualizar la descripción de la lista estática. El parámetro `name` se puede usar para actualizar el nombre de la lista estática y debe ser único.

```http
POST /rest/asset/v1/staticList/{id}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
description=This is a static list used for testing
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "f84f#16420b4c746",
    "result": [
        {
            "id": 1027,
            "name": "My Static List",
            "description": "This is a static list used for testing",
            "createdAt": "2018-06-21T04:32:26Z+0000",
            "updatedAt": "2018-06-21T04:57:55Z+0000",
            "folder": {
                "id": 1034,
                "type": "Program"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1027A1"
        }
    ]
}
```

## Eliminar

[Al eliminar una lista estática](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/deleteStaticListByIdUsingPOST), se toma una sola lista estática `id` como parámetro de ruta de acceso. Las eliminaciones no se pueden realizar en listas estáticas que estén siendo utilizadas por una operación de importación o exportación, o que estén siendo utilizadas por otros recursos.

```http
POST /rest/asset/v1/staticList/{id}/delete.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "2c79#16420ded0e9",
    "result": [
        {
            "id": 1027
        }
    ]
}
```
