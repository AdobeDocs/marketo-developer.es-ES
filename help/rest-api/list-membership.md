---
title: Pertenencia a una lista (listas estáticas)
feature: REST API, Static Lists
description: Utilice las API de REST de la base de datos de posibles clientes de Marketo para añadir posibles clientes a listas estáticas, eliminar posibles clientes, recuperar miembros de listas y comprobar la pertenencia a listas.
exl-id: b8f74bcf-834a-44db-81fd-621048afeba4
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 6%

---

# Pertenencia a una lista (listas estáticas)

[Referencia de extremo de pertenencia a lista](https://developer.adobe.com/marketo-apis/api/mapi#tag/Static-Lists)

Las API de pertenencia a listas proporcionan extremos de base de datos de posibles clientes para administrar miembros de lista estáticos. Utilice estos extremos para:

- Agregar posibles clientes a una lista.
- Eliminar posibles clientes de una lista.
- Recuperar miembros de una lista.
- Determine si los posibles clientes son miembros de una lista.

## Puntos de conexión

| Extremo | Método | Ruta |
| --- | --- | --- |
| Añadir a la lista | POST | `/rest/v1/lists/{listId}/leads.json` |
| Quitar de la lista | DELETE | `/rest/v1/lists/{listId}/leads.json` |
| Obtener posibles clientes por ID de lista | GET | `/rest/v1/lists/{listId}/leads.json` |
| Miembro de la lista | GET | `/rest/v1/lists/{listId}/leads/ismember.json` |

## Añadir a la lista

Use el extremo [Add to List](https://developer.adobe.com/marketo-apis/api/mapi#tag/Static-Lists/operation/addLeadsToListUsingPOST) para agregar uno o varios miembros a una lista. Pase el parámetro de ruta de acceso `listId` necesario y uno o más parámetros de consulta `id` que contienen los ID de posibles clientes. El número máximo de ID de posibles clientes es 300.

La respuesta contiene una matriz `result` con el estado de cada ID de posible cliente de la solicitud.

```http
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

Use el extremo [Remove from List](https://developer.adobe.com/marketo-apis/api/mapi#tag/Static-Lists/operation/removeLeadsFromListUsingDELETE) para quitar uno o varios miembros de una lista. Pase el parámetro de ruta de acceso `listId` necesario y uno o más parámetros de consulta `id` que contienen los ID de posibles clientes. El número máximo de ID de posibles clientes es 300.

La respuesta contiene una matriz `result` con el estado de cada ID de posible cliente de la solicitud.

```http
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

Use el extremo [Obtener posibles clientes por id. de lista](https://developer.adobe.com/marketo-apis/api/mapi#tag/Static-Lists/operation/getLeadsByListIdUsingGET) para recuperar miembros de una lista. Pase el parámetro de ruta de acceso necesario `listId`. También puede pasar parámetros de consulta opcionales para especificar criterios de filtrado.

Los parámetros de consulta opcionales son:

- `batchSize`: especifica el número de registros de posibles clientes que se devolverán en una llamada. El valor predeterminado y máximo es 300.
- `nextPageToken`: pagina mediante grandes conjuntos de resultados. Omita este parámetro de la primera llamada e inclúyalo en las llamadas posteriores.
- `fields`: especifica una lista de nombres de campo separados por comas que se van a devolver. Si omite este parámetro, la respuesta incluye `email`, `updatedAt`, `createdAt`, `lastName`, `firstName` y `id`.

La respuesta contiene una matriz `result` con los campos de posibles clientes especificados en la solicitud.

```http
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

Use el extremo [Member of List](https://developer.adobe.com/marketo-apis/api/mapi#tag/Static-Lists/operation/areLeadsMemberOfListUsingGET) para determinar si uno o más posibles clientes son miembros de una lista. Pase el parámetro de ruta de acceso `listId` necesario y uno o más parámetros de consulta `id` que contienen los ID de posibles clientes. El número máximo de ID de posibles clientes es 300.

La respuesta contiene una matriz `result` con el estado de cada ID de posible cliente de la solicitud.

```http
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
