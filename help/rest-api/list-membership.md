---
title: Pertenencia a una lista (listas estáticas)
feature: REST API, Static Lists
description: Utilice las API de REST de la base de datos de posibles clientes de Marketo para añadir posibles clientes a listas estáticas, eliminar posibles clientes, recuperar miembros de listas y comprobar la pertenencia a listas.
exl-id: 2a91b0f3-5ba1-4b0c-b5e7-a19ab9a7fdc3
source-git-commit: 73fa4c85ecabd4cfd24bc6591aad11dc4e75010a
workflow-type: tm+mt
source-wordcount: '482'
ht-degree: 5%

---

# Pertenencia a una lista (listas estáticas)

[Referencia de extremo de pertenencia a lista](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists)

Las API de pertenencia a listas proporcionan puntos finales de base de datos de posibles clientes para trabajar con miembros de listas estáticas. Estos extremos se pueden utilizar para agregar posibles clientes a una lista, quitar posibles clientes de una lista, recuperar miembros de una lista y determinar si uno o varios posibles clientes son miembros de una lista.

## Puntos de conexión

| Extremo | Método | Ruta |
| --- | --- | --- |
| Añadir a la lista | POST | `/rest/v1/lists/{listId}/leads.json` |
| Quitar de la lista | DELETE | `/rest/v1/lists/{listId}/leads.json` |
| Obtener posibles clientes por ID de lista | GET | `/rest/v1/lists/{listId}/leads.json` |
| Miembro de la lista | GET | `/rest/v1/lists/{listId}/leads/ismember.json` |

## Añadir a la lista

El extremo [Add to List](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/addLeadsToListUsingPOST) se usa para agregar uno o varios miembros a una lista. El extremo toma un parámetro de ruta de acceso `listId` requerido y uno o más parámetros de consulta `id` que contienen identificadores de posibles clientes (el máximo permitido es 300).

La respuesta contiene una matriz `result` compuesta por objetos JSON con el estado para cada ID de posible cliente especificado en la solicitud.

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

## Quitar de la lista

El extremo [Remove from List](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/removeLeadsFromListUsingDELETE) se usa para quitar uno o varios miembros de una lista. El extremo toma un parámetro de ruta de acceso `listId` requerido y uno o más parámetros de consulta `id` que contienen identificadores de posibles clientes (el máximo permitido es 300).

La respuesta contiene una matriz `result` compuesta por objetos JSON con el estado para cada ID de posible cliente especificado en la solicitud.

```
DELETE /rest/v1/lists/{listId}/leads.json?id=318603&id=318595&id=999999
```

```json
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

## Obtener posibles clientes por ID de lista

El punto de conexión [Obtener posibles clientes por id. de lista](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/getLeadsByListIdUsingGET) se usa para recuperar miembros de una lista. El extremo toma un parámetro de ruta de acceso `listId` necesario y permite que varios parámetros de consulta opcionales especifiquen criterios de filtrado.

El parámetro `batchSize` se usa para especificar el número de registros de posibles clientes que se van a devolver en una sola llamada. El valor predeterminado y máximo es 300.

El parámetro `nextPageToken` se usa para paginar conjuntos de resultados grandes. Este parámetro no se pasa en la primera llamada, sino solo en las llamadas posteriores para la paginación.

El parámetro `fields` contiene una lista de nombres de campo separados por comas que deben devolverse en la respuesta. Si el parámetro `fields` no se incluye en esta solicitud, se devuelven los siguientes campos predeterminados: `email`, `updatedAt`, `createdAt`, `lastName`, `firstName` y `id`.

La respuesta contiene una matriz `result` compuesta por objetos JSON que contienen los campos de posible cliente especificados en la solicitud.

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

## Miembro de la lista

El extremo [Member of List](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/areLeadsMemberOfListUsingGET) se usa para ver si uno o más posibles clientes son miembros de una lista. El extremo toma un parámetro de ruta de acceso `listId` requerido y uno o más parámetros de consulta `id` que contienen identificadores de posibles clientes (el máximo permitido es 300).

La respuesta contiene una matriz `result` compuesta por objetos JSON con el estado para cada ID de posible cliente especificado en la solicitud.

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
