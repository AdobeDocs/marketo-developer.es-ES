---
title: Listas estáticas
feature: REST API, Static Lists
description: '"Realizar operaciones de CRUD en listas estáticas".'
source-git-commit: e8bb45a7b3bee71c3d0ab6117296a75c8959d72e
workflow-type: tm+mt
source-wordcount: '741'
ht-degree: 1%

---


# Listas estáticas

[Referencia de extremo de listas estáticas](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists)

[Referencia de extremo de pertenencia a lista](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists)

Marketo ofrece un conjunto de API de REST para realizar operaciones de CRUD en listas estáticas. Estas API siguen el patrón de interfaz estándar para las API de recursos que proporcionan las opciones de Consulta, Crear, Actualizar y Eliminar.

## Consulta

La consulta de listas estáticas sigue los tipos de consulta estándar para los recursos de [por id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET), [por nombre](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByNameUsingGET), y [examinar](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListsUsingGET).

### Por ID

[Consulta por identificador](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET) toma una sola lista estática `id` como parámetro de ruta y devuelve un único registro de lista estática.

```
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

[Consulta por nombre](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/getSmartListByNameUsingGET) toma una lista estática `name` como parámetro y devuelve un único registro de lista estática. Se realiza una coincidencia de cadena exacta con todos los nombres de lista estática de la instancia y se devuelve un resultado para la lista estática que coincida con ese nombre.

```
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

Las listas estáticas también pueden ser [recuperado por lotes](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListsUsingGET). El `folder` Este parámetro se puede utilizar para especificar la carpeta principal en la que se realizará la consulta y tiene el formato de un objeto JSON que contiene el ID y el tipo. Al igual que otros extremos de recuperación masiva de recursos, `offset` y `maxReturn` son parámetros opcionales que se pueden utilizar para la paginación. El `earliestUpdatedAt` y `latestUpdatedAt` Los parámetros de permiten establecer marcas de agua de fecha y hora bajas y altas para devolver listas estáticas creadas o actualizadas dentro del intervalo dado. Los valores de fecha y hora deben ser cadenas ISO-8601 válidas y no deben incluir milisegundos

```
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

[Creación de una lista estática](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/createStaticListUsingPOST) se ejecuta con un POST application/x-www-form-urlencoded con dos parámetros obligatorios. El `folder` se utiliza para especificar la carpeta principal en la que se creará la lista estática y tiene el formato de objeto JSON que contiene el ID y el tipo. El `name` El parámetro se utiliza para asignar un nombre a la lista estática y debe ser único. Opcionalmente, la variable `description` puede utilizarse para describir la lista estática.

```
POST /rest/asset/v1/staticLists.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

[Actualizaciones en una lista estática](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/updateStaticListUsingPOST) se realizan a través de un punto final independiente con dos parámetros opcionales. El `description` para actualizar la descripción de la lista estática. El `name` El parámetro se puede utilizar para actualizar el nombre de la lista estática y debe ser único.

```
POST /rest/asset/v1/staticList/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

### Eliminar

[Eliminación de una lista estática](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/deleteStaticListByIdUsingPOST) toma una sola lista estática `id` como parámetro de ruta. Las eliminaciones no se pueden realizar en listas estáticas que estén siendo utilizadas por una operación de importación o exportación, o que estén siendo utilizadas por otros recursos.

```
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

## Enumerar pertenencia

Los extremos de pertenencia a listas proporcionan la capacidad de agregar, quitar y consultar miembros de lista estática. Además, puede consultar la pertenencia a listas estáticas.

### Agregar a Lista

El [Añadir a lista](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/addLeadsToListUsingPOST) extremo se utiliza para agregar uno o más miembros a una lista. El extremo toma un valor obligatorio `listId` parámetro de ruta y uno o más parámetros de consulta de id que contienen id de posibles clientes (el máximo permitido es 300).

La respuesta contiene un `result` matriz compuesta por objetos JSON con el estado para cada ID de posible cliente especificado en la solicitud.

```
POST /rest/v1/lists/{listId}/leads.json?id=318594&id=318595
```

```json
{
    "requestId": "6860#1706170ba29",
    "result": [
        {
            "id": 318594,
            "status": "added"
        },
        {
            "id": 318595,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1004",
                    "message": "Lead not found"
                }
            ]
        }
    ],
    "success": true
}
```

### Quitar de Lista

El [Eliminar de la lista](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/removeLeadsFromListUsingDELETE) extremo se utiliza para quitar uno o varios miembros de una lista. El extremo toma un valor obligatorio `listId` parámetro de ruta y uno o más `id` parámetros de consulta que contienen id de posibles clientes (el máximo permitido es 300).

La respuesta contiene un `result` matriz compuesta por objetos JSON con el estado para cada ID de posible cliente especificado en la solicitud.

```
DELETE /rest/v1/lists/{listId}/leads.json?id=318603&id=318595&id=999999
```

```
{
    "requestId": "9e79#17061689ac3",
    "result": [
        {
            "id": 318603,
            "status": "removed"
        },
        {
            "id": 318595,
            "status": "removed"
        },
        {
            "id": 999999,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1004",
                    "message": "Lead not found"
                }
            ]
        }
    ],
    "success": true
}
```

### Lista de consultas

El [Obtener posibles clientes por ID de lista](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/getLeadsByListIdUsingGET) el extremo se utiliza para recuperar miembros de una lista. El extremo toma un valor obligatorio `listId` parámetro de ruta y permite que varios parámetros de consulta opcionales especifiquen criterios de filtrado.

El `batchSize` se utiliza para especificar el número de registros de posibles clientes que se devolverán en una sola llamada (el valor predeterminado y máximo es 300).

El `nextPageToken` se utiliza para paginar conjuntos de resultados grandes. Este parámetro no se pasa en la primera llamada, sino solo en las llamadas posteriores para la paginación.

El `fields` El parámetro contiene una lista de nombres de campo separados por comas que se devolverán en la respuesta. Si el parámetro fields no se incluye en esta solicitud, se devuelven los siguientes campos predeterminados: email, updatedAt, createdAt, lastName, firstName e id.

La respuesta contiene un `result` matriz compuesta por objetos JSON que contienen los campos de posible cliente especificados en la solicitud.

```
GET /rest/v1/lists/{listId}/leads.json?batchSize=3
```

```json
{
    "requestId": "ddae#170615ba0cc",
    "result": [
        {
            "id": 318594,
            "firstName": "Hanna",
            "lastName": "Crawford",
            "email": "208161Robert.L.Deacon@pookmail.com",
            "updatedAt": "2015-04-06T17:13:50Z",
            "createdAt": "2015-04-06T17:13:50Z"
        },
        {
            "id": 318595,
            "firstName": "Bertha",
            "lastName": "Fulton",
            "email": "208160Tyrone.V.Dyer@trashymail.com",
            "updatedAt": "2015-04-06T17:13:50Z",
            "createdAt": "2015-04-06T17:13:50Z"
        },
        {
            "id": 318596,
            "firstName": "Faith",
            "lastName": "England",
            "email": "208159Rex.M.Bailey@dodgit.com",
            "updatedAt": "2015-04-06T17:13:50Z",
            "createdAt": "2015-04-06T17:13:50Z"
        }
    ],
    "success": true,
    "nextPageToken": "PS5VL5WD4UOWGOUCJR6VY7JQO24LC2U5DRBU4WO4RQMPHDHTK2T3BEZOR75VLQXYB3245WW2GMDSK==="
}
```

#### Pertenencia a lista de consulta por ID de posible cliente

El [Miembro de lista](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/areLeadsMemberOfListUsingGET) el extremo se utiliza para ver si uno o más posibles clientes son miembros de una lista. El extremo toma un valor obligatorio `listId` parámetro de ruta y uno o más `id` parámetros de consulta que contienen id de posibles clientes (el máximo permitido es 300).

La respuesta contiene un `result` matriz compuesta por objetos JSON con el estado para cada ID de posible cliente especificado en la solicitud.

```
GET /rest/v1/lists/{listId}/leads/ismember.json?id=309901&id=318603&id=999999
```

```json
{
    "requestId": "693a#17061475cf9",
    "result": [
        {
            "id": 309901,
            "status": "memberof"
        },
        {
            "id": 318603,
            "status": "notmemberof"
        },
        {
            "id": 999999,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1004",
                    "message": "Lead not found"
                }
            ]
        }
    ],
    "success": true
}
```
