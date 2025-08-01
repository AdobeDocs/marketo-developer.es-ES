---
title: Listas de cuentas con nombre
feature: REST API
description: Configure listas de cuentas con nombre.
exl-id: 98f42780-8329-42fb-9cd8-58e5dbea3809
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '696'
ht-degree: 3%

---

# Listas de cuentas con nombre

[Referencia de extremo de listas de cuentas con nombre](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Named-Account-Lists)

[Listas de cuentas con nombre](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/target-account-management/target/account-lists) en Marketo representan colecciones de cuentas con nombre. Se pueden utilizar para una amplia variedad de casos, incluida la categorización, el enriquecimiento de datos y el filtrado de campañas inteligentes. Las API de lista de cuentas con nombre permiten la administración remota de estos recursos de lista y su pertenencia.
`Content`

## Permisos

Para consultar Listas de cuentas con nombre, se requiere el permiso Lista de cuentas con nombre de solo lectura o Lista de cuentas con nombre de lectura y escritura. Para crear, actualizar o eliminar listas, se requiere el permiso Leer-escribir lista de cuentas con nombre. La consulta de la pertenencia a listas requiere los permisos Cuenta con nombre de solo lectura o Cuenta con nombre de lectura y escritura, mientras que la administración de la pertenencia requiere los permisos Cuenta con nombre de lectura y escritura.

## Modelo

Las listas de cuentas con nombre tienen un número limitado de campos estándar y no se pueden ampliar con campos personalizados.
`Named Account List Field`

| Nombre | Tipo de datos | Actualizable | Notas |
|---|---|---|---|
| marketoGUID | Cadena | Falso | Identificador de cadena único de la lista de cuentas con nombre. Este campo está administrado por el sistema y no se permite como campo al crear un registro. Campo utilizado por &quot;dedupeBy&quot;:&quot;idField&quot; al realizar una creación o actualización. |
| name | Cadena | True | Nombre de la lista. Campo utilizado por &quot;dedupeBy&quot;:&quot;dedupeFields&quot; al realizar una creación o actualización. |
| createdAt | Fecha y hora | Falso | Fecha y hora de creación de la lista. Este campo está administrado por el sistema y no se permite como campo al crear o actualizar un registro. |
| updatedAt | Fecha y hora | Falso | Fecha y hora de la última actualización de la lista. Este campo está administrado por el sistema y no se permite como campo al crear o actualizar un registro. |
| tipo | Cadena | Falso | Tipo de la lista. Puede tener un valor &quot;predeterminado&quot; o &quot;externo&quot;. Las listas externas son las creadas por la Vista de cuentas de CRM. |


## Consulta

La consulta de listas de cuentas es sencilla y sencilla. Actualmente, solo hay dos filterTypes válidos para consultar listas de cuentas con nombre: &quot;dedupeFields&quot; e &quot;idField&quot;. El campo por el que filtrar se establece en el parámetro `filterType` de la consulta, y los valores se establecen en `filterValues as` en una lista separada por comas. Los filtros `nextPageToken` y `batchSize` también son parámetros opcionales.

```
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

La creación y actualización de registros de lista de cuentas con nombre sigue los patrones establecidos para otras operaciones de creación y actualización de la base de datos de posibles clientes. Tenga en cuenta que las listas de cuentas con nombre solo tienen un campo actualizable, `name`.

El punto de conexión permite los dos tipos de acción estándar: &quot;createOnly&quot; y &quot;updateOnly&quot;.  El `action defaults` a &quot;createOnly&quot;.

Se puede especificar el elemento opcional `dedupeBy parameter` si la acción es `updateOnly`.  Los valores permitidos son &quot;dedupeFields&quot; (correspondiente a &quot;name&quot;) o &quot;idField&quot; (correspondiente a &quot;marketoGUID&quot;).  En los modos `createOnly`, solo se permite &quot;name&quot; como campo `dedupeBy`. Puede enviar hasta 300 registros a la vez.

```
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

La eliminación de las listas de cuentas con nombre es sencilla y se puede realizar en función de `name` o de `marketoGUID` de la lista. Para seleccionar la clave que desea utilizar, pase &quot;dedupeFields&quot; para name o &quot;idField&quot; para marketoGUID en el miembro `deleteB` de su solicitud. Si no se configura, se desduplicará de forma predeterminada. Puede eliminar hasta 300 registros a la vez.

```
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

En caso de que no se encuentre un registro para una clave determinada, el elemento de resultado correspondiente tendrá un `status` de &quot;omitido&quot; y un motivo con un código y un mensaje que describan el error, como se muestra en el ejemplo anterior.

## Administración de suscripciones

### Suscripción a consulta

Consultar la pertenencia a una lista de cuentas con nombre es sencillo y requiere solamente el `i` de la lista de cuentas. Los parámetros opcionales son:

-`field` - una lista de campos separados por comas para incluir en los registros de respuesta
-`nextPageToke` - para paginar a través del conjunto de resultados
-`batchSiz` - para especificar el número de registros que se van a devolver

Si `field` no está establecido, se devolverán `marketoGUI`,`nam`, `createdA` y`updatedA`. `batchSiz` tiene un valor máximo y predeterminado de 300.

```
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

Las cuentas con nombre se pueden agregar fácilmente a una Lista de cuentas con nombre. Las cuentas solo se pueden agregar utilizando su marketoGUID. Puede agregar hasta 300 registros a la vez.

```
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

La eliminación de registros de una lista de cuentas tiene una ruta diferente, pero la misma interfaz, que requiere un`marketoGUI` para cada registro que desea eliminar. Puede quitar hasta 300 registros a la vez.

```
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

- Los extremos de la lista de cuentas con nombre tienen un tiempo de espera de 30 segundos a menos que se indique a continuación
   - Sincronizar listas de cuentas con nombre: 60 s 
   - Eliminar listas de cuentas con nombre: 60s 
   - Obtener listas de cuentas con nombre: 60s 
   - Añadir miembros de la lista de la cuenta con nombre: 60s 
   - Eliminar miembros de la lista de cuentas con nombre: 60s 
   - Obtener lista de cuentas con nombre Miembros: 60s
