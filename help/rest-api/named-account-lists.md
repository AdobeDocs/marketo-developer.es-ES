---
title: Listas de cuentas con nombre
feature: REST API
description: Obtenga información sobre cómo administrar Listas de cuentas con nombre de Marketo con la API de REST, incluidos permisos, campos, filtros y extremos para consultar, crear, actualizar y eliminar.
exl-id: 98f42780-8329-42fb-9cd8-58e5dbea3809
TQID: https://experienceleague.adobe.com/18lMhheW21Gz1-3TMHwleHhmLTOqJsZSQ5aqkbbchhM
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 686
ht-degree: 3%

---

# Listas de cuentas con nombre

[Referencia de extremo de listas de cuentas con nombre](https://developer.adobe.com/marketo-apis/api/mapi#tag/Named-Account-Lists)

[Listas de cuentas con nombre](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/target-account-management/target/account-lists) son colecciones de cuentas con nombre en Marketo. Utilícelos para la categorización, el enriquecimiento de datos y el filtrado de campañas inteligentes.

Las API de lista de cuentas con nombre le permiten administrar de forma remota los recursos de la lista y su pertenencia.
`Content`

## Permisos

El permiso requerido depende de la operación:

- Listas de cuentas con nombre de consulta: Lista de cuentas con nombre de solo lectura o Lista de cuentas con nombre de lectura y escritura.
- Crear, actualizar o eliminar listas: Lista de cuentas con nombre de lectura y escritura.
- Pertenencia a la lista de consultas: Cuenta con nombre de solo lectura o Cuenta con nombre de lectura-escritura.
- Administrar pertenencia a lista: Lectura y escritura de cuenta con nombre.

## Modelo

Las listas de cuentas con nombre tienen un conjunto limitado de campos estándar y no admiten campos personalizados.
`Named Account List Field`

| Nombre | Tipo de datos | Actualizable | Notas |
| --- | --- | --- | --- |
| marketoGUID | Cadena | Falso | Identificador de cadena único de la lista de cuentas con nombre. Este campo está administrado por el sistema y no se permite como campo al crear un registro. Campo utilizado por &quot;dedupeBy&quot;:&quot;idField&quot; al realizar una creación o actualización. |
| name | Cadena | True | Nombre de la lista. Campo utilizado por &quot;dedupeBy&quot;:&quot;dedupeFields&quot; al realizar una creación o actualización. |
| createdAt | Fecha y hora | Falso | Fecha y hora de creación de la lista. Este campo está administrado por el sistema y no se permite como campo al crear o actualizar un registro. |
| updatedAt | Fecha y hora | Falso | Fecha y hora de la última actualización de la lista. Este campo está administrado por el sistema y no se permite como campo al crear o actualizar un registro. |
| tipo | Cadena | Falso | Tipo de la lista. Puede tener un valor &quot;predeterminado&quot; o &quot;externo&quot;. Las listas externas son las creadas por la Vista de cuentas de CRM. |

## Consulta

Las consultas de lista de cuentas con nombre admiten dos filterTypes: &quot;dedupeFields&quot; y &quot;idField&quot;. Establezca el campo en el parámetro de consulta `filterType` y proporcione los valores de `filterValues as` en una lista separada por comas.

Los filtros `nextPageToken` y `batchSize` son opcionales.

```http
GET /rest/v1/namedAccountLists.json?filterType=idField&filterValues=dff23271-f996-47d7-984f-f2676861b5fb,dff23271-f996-47d7-984f-f2676861b5fc
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "seq": 0,
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
         "name": "Saas List",
         "createdAt": "xxxxxxxx",
         "updatedAt": "xxxxxxxx",
         "type": "default",
         "updateable": true
      },
      {
         "seq": 1,
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fc",
         "name": "My Account List",
         "createdAt": "xxxxxxxx",
         "updatedAt": "xxxxxxxx",
         "type": "default",
         "updateable": true
      }
   ]
}
```

## Crear y actualizar

Cree y actualice registros de lista de cuentas con nombre utilizando el patrón estándar de base de datos de posibles clientes. Las listas de cuentas con nombre solo tienen un campo actualizable: `name`.

El punto de conexión admite dos tipos de acciones estándar: &quot;createOnly&quot; y &quot;updateOnly&quot;. El `action defaults` a &quot;createOnly&quot;.

Puede especificar el(la) `dedupeBy parameter` opcional(a) cuando la acción sea `updateOnly`. Los valores permitidos son &quot;dedupeFields&quot;, que corresponde a &quot;name&quot;, y &quot;idField&quot;, que corresponde a &quot;marketoGUID&quot;.

En los modos `createOnly`, solo se permite &quot;name&quot; como campo `dedupeBy`. Puede enviar hasta 300 registros a la vez.

```http
POST /rest/v1/namedAccountLists.json
```

```json
{
   "action": "createOnly",
   "dedupeBy": "dedupeFields",
   "input": [
      {
         "name": "SAAS List"
      },
      {
         "name": "Manufacturing (Domestic)"
      }
   ]
}
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "seq": 0,
         "status": "created",
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
      },
      {
         "seq": 1,
         "status": "created",
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fc"
      }
   ]
}
```

## Eliminar

Eliminar listas de cuentas con nombre utilizando `name` o `marketoGUID` de la lista. Para seleccionar la clave, pase &quot;dedupeFields&quot; para name o &quot;idField&quot; para marketoGUID en el miembro `deleteB` de la solicitud.

Si no se establece, el valor predeterminado es deduplicarCampos. Puede eliminar hasta 300 registros a la vez.

```http
POST /rest/v1/namedAccountLists/delete.json
```

```json
{
   "deleteBy": "dedupeFields",
   "input": [
      {
         "name": "Saas List"
      },
      {
         "name": "B2C List"
      },
      {
         "name": "Launchpoint Partner List"
      }
   ]
}
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "seq": 0,
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
         "status": "deleted"
      },
      {
         "seq": 1,
         "id": "dff23271-f996-47d7-984f-f2676861b5fc",
         "status": "deleted"
      },
      {
         "seq": 2,
         "status": "skipped",
         "reasons": [
            {
               "code": "1013",
               "message": "Record not found"
            }
         ]
      }
   ]
}
```

Si no se encuentra un registro para una clave, el elemento de resultado correspondiente tiene `status` de &quot;omitido&quot;. También incluye un motivo con un código y un mensaje que describen el error.

## Administración de suscripciones

### Suscripción a consulta

Consulte la pertenencia a la lista de cuentas con nombre proporcionando `i` de la lista de cuentas. Los parámetros opcionales son:

-`field` - una lista de campos separados por comas para incluir en los registros de respuesta
-`nextPageToke` - para paginar a través del conjunto de resultados
-`batchSiz` - para especificar el número de registros que se van a devolver

Si `field` no está establecido, se devolverán `marketoGUI`,`nam`, `createdA` y`updatedA`. `batchSiz` tiene un valor máximo y predeterminado de 300.

```http
GET /rest/v1/namedAccountList/{id}/namedAccounts.json
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "seq": 0,
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
         "name": "Saas List",
         "createdAt": "2017-02-01T00:00:00Z",
         "updatedAt": "2017-03-05T17:21:15Z"
      },
      {
         "seq": 1,
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fc",
         "name": "My Account List",
         "createdAt": "2017-02-01T00:00:00Z",
         "updatedAt": "2017-03-05T17:21:15Z"
      }
   ]
}
```

### Añadir miembros

Agregue cuentas con nombre a una lista de cuentas con nombre utilizando su marketoGUID. Puede agregar hasta 300 registros a la vez.

```http
POST /rest/v1/namedAccountList/{id}/namedAccounts.json
```

```json
{
    "input": [
        {
             "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
        },
        {
             "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
        }
    ]
}
```

```json
{
    "requestId": "string",
    "result": [
        {
            "seq": 0,
            "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
            "status": "added"
        },
        {
            "seq": 1,
            "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
            "status": "added"
        }
    ],
    "success": true,
}
```

### Eliminar miembros

La eliminación de registros de una lista de cuentas utiliza una ruta diferente pero la misma interfaz. Proporcione un`marketoGUI` para cada registro que desee eliminar. Puede quitar hasta 300 registros a la vez.

```http
POST /rest/v1/namedAccountList/{id}/namedAccounts/remove.json
```

```json
{
    "input": [
        {
             "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
        },
        {
             "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
        }
    ]
}
```

```json
{
    "requestId": "string",
    "result": [
        {
            "seq": 0,
            "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
            "status": "added"
        },
        {
            "seq": 1,
            "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
            "status": "added"
        }
    ],
    "success": true
}
```

## Tiempos de espera

- Los extremos de la lista de cuentas con nombre tienen un tiempo de espera de 30 segundos a menos que se indique lo contrario.
- La sincronización de listas de cuentas con nombre tiene un tiempo de espera de 60 segundos.
- Eliminar listas de cuentas con nombre tiene un tiempo de espera de 60 segundos.
- Obtener listas de cuentas con nombre tiene un tiempo de espera de 60 segundos.
- Añadir miembros de la lista de cuentas con nombre tiene un tiempo de espera de 60 segundos.
- Los miembros de la lista Quitar cuenta con nombre tienen un tiempo de espera de 60 segundos.
- Obtener miembros de la lista de cuentas con nombre tiene un tiempo de espera de 60 segundos.
