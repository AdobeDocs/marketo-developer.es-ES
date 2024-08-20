---
title: Gestión de usuarios
feature: REST API
description: Realizar operaciones de CRUD en registros de usuario.
exl-id: 2a58f496-0fe6-4f7e-98ef-e9e5a017c2de
source-git-commit: 13a567be067a8a1272e981fad4e03b0a8519f132
workflow-type: tm+mt
source-wordcount: '1185'
ht-degree: 0%

---

# Gestión de usuarios

[Referencia de extremo de administración de usuarios](https://developer.adobe.com/marketo-apis/api/user/)

Marketo proporciona un conjunto de puntos finales de User Management que le permiten realizar operaciones de CRUD en registros de usuarios en Marketo. Los usuarios se crean enviando una invitación a un usuario, que luego establece una contraseña y obtiene acceso a Marketo por primera vez.

A diferencia de otras API de REST de Marketo, al utilizar las API de administración de usuarios:

- Debe utilizar el método de encabezado HTTP para enviar el token de acceso para autenticarse. No se puede pasar el token de acceso como parámetro de cadena de consulta. Más información sobre la autenticación [aquí](authentication.md).
- Debe seleccionar un permiso de rol de dos grupos diferentes al crear el rol de usuario para [Servicio personalizado](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/additional-integrations/create-a-custom-service-for-use-with-rest-api) para la API de REST:
   1. Permiso &quot;Acceder a usuarios&quot; del grupo [Administrador de acceso](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/descriptions-of-role-permissions)
   1. &quot;Acceder a la API de administración de usuarios&quot; desde el grupo [API de acceso](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/descriptions-of-role-permissions)
- Los cuerpos de respuesta no contienen el atributo booleano &quot;success&quot; que indica el éxito o el error de una llamada. En su lugar, debe evaluar el código de estado de respuesta HTTP. Si una llamada se realiza correctamente, se devuelve un código de estado 200. Si falla una llamada, se devuelve un código de estado de nivel distinto de 200 y el cuerpo de la respuesta contiene la matriz de &quot;errores&quot; estándar con un código de error y un mensaje de error descriptivo.
- El formato de las cadenas de fecha y hora es &quot;aaaaMMdd&#39;T&#39;HH:mm:ss.SS&#39;t&#39;+|-hhmm&quot;. Esto se aplica a los siguientes atributos: createdAt, updatedAt, expiresAt.
- Los extremos de la API de administración de usuarios no tienen el prefijo &quot;/rest&quot; como otros extremos.

## Consulta

La compatibilidad de consultas para la administración de usuarios incluye la capacidad de recuperar todos los usuarios, funciones y espacios de trabajo. Además, puede recuperar un único registro de usuario por ID de usuario o un registro de rol/espacio de trabajo por ID de usuario.

### Usuario por identificador

El punto de conexión [Obtener usuario por identificador](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getUserUsingGET) toma un único parámetro de ruta de acceso `userid` y devuelve un único registro de usuario para un usuario que ha aceptado su invitación.

```
GET /userservice/management/v1/users/{userid}/user.json
```

```json
{
  "userid": "jamie@houselannister.com",
  "firstName": "Jamie",
  "lastName": "Lannister",
  "emailAddress": "jamie@lannister.com",
  "optedIn": false,
  "failedLogins": 0,
  "failedDeviceCode": 0,
  "isLocked": false,
  "lockedReason": null,
  "id": 0,
  "apiOnly": false,
  "userRoleWorkspaces": [
    {
      "accessRoleId": 1,
      "accessRoleName": "Admin",
      "workspaceId": 0,
      "workspaceName": "AllZones"
    },
    {
      "accessRoleId": 2,
      "accessRoleName":
      "Standard User",
      "workspaceId": 1008,
      "workspaceName": "World"
    }
  ],
  "expiresAt": "2020-12-31T08:00:00.000t+0000",
  "lastLoginAt": "2020-02-05T01:02:23.000t+0000"
}
```

### Usuario invitado por identificador

El punto de conexión [Obtener usuario invitado por el identificador](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getInvitedUserUsingGET) toma un único parámetro de ruta de acceso `userid` y devuelve un único registro de usuario para un usuario &quot;pendiente&quot; (aún no ha aceptado su invitación).

```
GET /userservice/management/v1/users/{userid}/invite.json
```

```json
{
    "id": 25112,
    "firstName": "Jamie",
    "lastName": "Lannister",
    "emailAddress": "jamie@lannister.com",
    "userId": "jamie@lannister.com",
    "subscriptionId": 3381,
    "status": "pending",
    "expiresAt": "20200807T20:49:54.0t+0000",
    "createdAt": "20200731T20:49:54.0t+0000",
    "updatedAt": "20200731T20:49:54.0t+0000"
}
```

### Roles y espacios de trabajo por ID

El punto de conexión [Obtener roles y espacios de trabajo por identificador](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getUserRolesAndWorkspacesUsingGET) toma un único parámetro de ruta de acceso `userid` y devuelve una lista de registros de espacio de trabajo y rol de usuario. La respuesta contiene una matriz con un objeto que contiene la función, el identificador de área de trabajo y el nombre del usuario especificado.

```
GET /userservice/management/v1/users/{userid}/roles.json
```

```json
[
  {
    "accessRoleId": 1,
    "accessRoleName": "Admin",
    "workspaceId": 0,
    "workspaceName": "AllZones"
  },
  {
    "accessRoleId": 2,
    "accessRoleName": "Standard User",
    "workspaceId": 1008,
    "workspaceName": "World"
  }
]
```

### Examinar usuarios

El extremo [Obtener usuarios](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getUsersUsingGET) devuelve una lista de todos los registros de usuario. El parámetro `pageSize` opcional es un entero que especifica el número máximo de entradas que se van a devolver. El valor predeterminado es 20. El máximo es 200. El parámetro `pageOffset` opcional es un entero que especifica dónde comenzar a recuperar entradas. Se puede usar con `pageSize`. El valor predeterminado es 0.

```
GET /userservice/management/v1/users/allusers.json
```

```json
[
  {
    "userid": "02226aae-9f54-45d1-bc26-8305c8f55ec7@adobe.com",
    "firstName": "Aparna",
    "lastName": "Ghosh",
    "emailAddress": "aparna.ghosh@ericsson.com",
    "id": 5222,
    "apiOnly": false
    },
    {
    "userid": "038e1cac-3f3e-4c05-b0b3-6265fd2abcd3@adobe.com",
    "firstName": "Timm",
    "lastName": "Rehse",
    "emailAddress": "timm.rehse@ericsson.com",
    "id": 7075,
    "apiOnly": false
    },
    {
    "userid": "0a855522-06c9-4a9e-93de-91a0d2cc2987@adobe.com",
    "firstName": "Dhinagaran",
    "lastName": "Swaminathan",
    "emailAddress": "dhinagaran.swaminathan@ericsson.com",
    "id": 6439,
    "apiOnly": false
    }
]
```

>[!NOTE]
>
>En el ejemplo de código anterior, el `userid` mostrado es para un cliente que se ha migrado a Adobe IMS. Los clientes que aún no migrarán verán una dirección de correo electrónico normal en el campo `userid`.

### Examinar funciones

El extremo [Get Roles](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getRolesUsingGET) devuelve una lista de todos los registros de roles.

```
GET /userservice/management/v1/users/roles.json
```

```json
[
    {
        "id": 1,
        "name": "Admin",
        "description": "All permissions",
        "type": "system",
        "hidden": false,
        "onlyAllZones": true,
        "createdAt": "20100327T18:27:42.0t+0000",
        "updatedAt": "20100327T18:27:42.0t+0000"
    },
    {
        "id": 2,
        "name": "Standard User",
        "description": "All permissions except Admin",
        "type": "system",
        "hidden": false,
        "onlyAllZones": false,
        "createdAt": "20100327T18:27:42.0t+0000",
        "updatedAt": "20180423T02:33:29.0t+0000"
    },
    {
        "id": 24,
        "name": "RTP Launcher",
        "description": "Role required for launcher in RTP",
        "type": "system",
        "hidden": false,
        "onlyAllZones": false,
        "createdAt": "20151024T01:45:40.0t+0000",
        "updatedAt": "20171024T23:41:24.0t+0000"
    },
    {
        "id": 25,
        "name": "RTP Editor",
        "description": "Role required for editor in RTP",
        "type": "system",
        "hidden": false,
        "onlyAllZones": false,
        "createdAt": "20151024T01:45:40.0t+0000",
        "updatedAt": "20171024T23:41:24.0t+0000"
    },
    {
        "id": 101,
        "name": "Analytics User",
        "description": "Has access to Analytics",
        "type": "custom",
        "hidden": false,
        "onlyAllZones": false,
        "createdAt": "20100327T18:27:42.0t+0000",
        "updatedAt": "20180423T02:33:29.0t+0000"
    },
    {
        "id": 102,
        "name": "Marketing User",
        "description": "All permissions except Admin",
        "type": "custom",
        "hidden": false,
        "onlyAllZones": false,
        "createdAt": "20100327T18:27:42.0t+0000",
        "updatedAt": "20100327T18:27:42.0t+0000"
    },
    {
        "id": 103,
        "name": "Web Designer",
        "description": "Has access to Design Studio except approval permission",
        "type": "custom",
        "hidden": false,
        "onlyAllZones": false,
        "createdAt": "20100327T18:27:42.0t+0000",
        "updatedAt": "20180423T02:33:29.0t+0000"
    }
]
```

### Examinar espacios de trabajo

El extremo [Get Workspaces](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getWorkspacesUsingGET) devuelve una lista de todos los registros de área de trabajo.

```
GET /userservice/management/v1/users/workspaces.json
```

```json
[
  {
    "id": 1,
    "name": "Default",
    "description": "Initial workspace for Marketing Activities, Design Studio, and so on.",
    "globalViz": 0,
    "status": "active",
    "currencyInfo": null,
    "createdAt": "20160910T23:08:05.0t+0000",
    "updatedAt": "20160910T23:08:05.0t+0000"
  },
  {
    "id": 1008,
    "name": "World",
    "description": "",
    "globalViz": 0,
    "status": "active",
    "currencyInfo": null,
    "createdAt": "20181119T21:59:36.0t+0000",
    "updatedAt": "20181119T21:59:36.0t+0000"
  },
  {
    "id": 1009,
    "name": "Reproduction - US English - All Leads",
    "description": "A Workspace for recreating customer-reported problems.",
    "globalViz": 1,
    "status": "active",
    "currencyInfo": null,
    "createdAt": "20190129T23:36:37.0t+0000",
    "updatedAt": "20190129T23:36:37.0t+0000"
  },
  {
    "id": 1010,
    "name": "US",
    "description": "United States - Qualified Leads",
    "globalViz": 0,
    "status": "active",
    "currencyInfo": null,
    "createdAt": "20190322T15:55:40.0t+0000",
    "updatedAt": "20190322T15:55:40.0t+0000"
  }
]
```

## Invitar usuario

En [suscripciones integradas en Adobe IMS](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview), este extremo admite la invitación de [usuarios solo de API](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user) solamente. Para invitar a [usuarios estándar](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users), usa la [API de administración de usuarios de Adobe](https://developer.adobe.com/umapi/).

El extremo [Invitar al usuario](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/inviteUserUsingPOST) a enviar una invitación por correo electrónico de &quot;Bienvenido a Marketo&quot; al nuevo usuario. El cuerpo del correo electrónico contiene un vínculo &quot;Iniciar sesión en Marketo&quot; que permite al usuario acceder a Marketo por primera vez. Para aceptar la invitación, el destinatario del correo electrónico hace clic en el vínculo &quot;Iniciar sesión en Marketo&quot;, crea su contraseña y obtiene acceso a Marketo. Hasta que se complete el proceso de aceptación, la invitación está &quot;pendiente&quot; y no se puede editar el registro de usuario. Una invitación pendiente caduca siete días después de enviarse. Encontrará más información sobre la administración de usuarios [aquí](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users).

Los parámetros se pasan en el cuerpo de la solicitud en formato `application/json`

Se requieren los siguientes parámetros:  `emailAddress`, `firstName`, `lastName, userRoleWorkspaces`. El parámetro `userRoleWorkspaces` es una matriz de objetos que contienen los atributos `accessRoleId` y `workspaceId`.

El parámetro `userid` es un valor de cadena de identificador de usuario único que se utiliza con fines de inicio de sesión del usuario y debe tener el formato de dirección de correo electrónico. Si no se proporciona en la solicitud, el valor de `userid` toma como valor predeterminado el valor proporcionado en el parámetro `emailAddress`.

El parámetro booleano `apiOnly` especifica si el usuario es un [usuario solo de API](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user). El parámetro `expiresAt` especifica cuándo caduca el inicio de sesión del usuario y se le aplica formato ISO-8601 de W3C (sin milisegundos). Si no se proporciona en la solicitud, el usuario nunca caducará. El parámetro `reason` es una cadena que describe el motivo de la invitación del usuario.

El extremo devuelve un valor de &quot;true&quot; si se realiza correctamente; de lo contrario, se devuelve un mensaje de error.

```
POST /userservice/management/v1/users/invite.json
```

```
Content-Type: application/json
```

```json
{
  "emailAddress": "daenerys@housetargaryen.com",
  "firstName": "Daenerys",
  "lastName": "Targaryen",
  "expiresAt": "2020-12-31T23:59:59-05:00",
  "reason": "Keeper of dragons",
  "userRoleWorkspaces": [
    {
      "accessRoleId": 1,
      "workspaceId": 0
    }
  ]
}
```

```
true
```

A continuación se muestra un ejemplo de la invitación por correo electrónico &quot;Bienvenido a Marketo&quot; que se envía al nuevo usuario. La línea de asunto del correo electrónico es &quot;Información de inicio de sesión de Marketo&quot;, el remitente es la dirección de correo electrónico del usuario solo de API asociado con el [servicio personalizado de API de REST](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/additional-integrations/create-a-custom-service-for-use-with-rest-api) y el destinatario es el especificado mediante los parámetros firstName, lastName y emailAddress.

![Invitar correo electrónico al usuario](assets/invite-user-email.png)

El usuario acepta la invitación por correo electrónico introduciendo su contraseña dos veces y haciendo clic en el botón &quot;CREAR CONTRASEÑA&quot;. A continuación, se le concede acceso a Marketo por primera vez.

## Actualizar usuario

La compatibilidad de la actualización para usuarios incluye la capacidad de actualizar atributos de usuario o eliminar un usuario. Solo se pueden actualizar los usuarios que hayan aceptado su invitación. Los atributos se pasan como parámetros al cuerpo de la solicitud en formato application/json

### Actualizar atributos de usuario

En [suscripciones integradas en Adobe IMS](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview), este punto de conexión solo admite la actualización de atributos de [usuarios solo de API](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user). Para actualizar los atributos de [usuarios estándar](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users), use la [API de administración de usuarios de Adobe](https://developer.adobe.com/umapi/).

El extremo [Actualizar atributos de usuario](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/updateUserAttributeUsingPOST) toma un único parámetro de ruta de acceso `userid` y devuelve un único registro de usuario. El cuerpo de la solicitud contiene uno o más atributos de usuario para actualizar: `emailAddress`, `firstName`, `lastName`, `expiresAt`.

```
POST /userservice/management/v1/users/{userid}/update.json
```

```
Content-Type: application/json
```

```json
{
  "firstName": "JAMIE",
  "lastName": "LANISTER",
  "expiresAt": "20211231T08:00:00.000t+0000"
}
```

```json
{
  "userid": "jamie@houselannister.com",
  "firstName": "JAMIE",
  "lastName": "LANISTER",
  "emailAddress": "jamie@houselannister.com",
  "optedIn": false,
  "failedLogins": 0,
  "failedDeviceCode": 0,
  "isLocked": false,
  "lockedReason": null,
  "id": 0,
  "apiOnly": false,
  "userRoleWorkspaces": [
    {
      "accessRoleId": 1,
      "accessRoleName": "Admin",
      "workspaceId": 0,
      "workspaceName": "AllZones"
    },
    {
      "accessRoleId": 2,
      "accessRoleName":
      "Standard User",
      "workspaceId": 1008,
      "workspaceName": "World"
    }
  ],
  "expiresAt": "2021-12-31T08:00:00.000t+0000"
  "lastLoginAt": "2020-02-05T01:02:23.000t+0000"
}
```

#### Eliminar usuario

En [suscripciones integradas en Adobe IMS](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview), este extremo admite la eliminación de [usuarios solo de API](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user) solamente. Para eliminar [usuarios estándar](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users), use la [API de administración de usuarios de Adobe](https://developer.adobe.com/umapi/).

El extremo [Delete User](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/deleteUserUsingPOST) toma un único parámetro de ruta de acceso `userid` y elimina el usuario correspondiente de la instancia. Se trata de una eliminación destructiva y no se puede deshacer. Si se ejecuta correctamente, se devuelve un código de estado 200; de lo contrario, se devuelve un mensaje de error.

```
POST /userservice/management/v1/users/{userid}/delete.json
```

#### Eliminar usuario invitado

El punto de conexión [Eliminar usuario invitado](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/deleteInvitedUserUsingPOST) toma un único parámetro de ruta de acceso `userid` y elimina el usuario &quot;pendiente&quot; correspondiente de la instancia (el usuario aún no ha aceptado su invitación). Se trata de una eliminación destructiva y no se puede deshacer. Si se ejecuta correctamente, se devuelve un código de estado 200; de lo contrario, se devuelve un mensaje de error.

```
POST /userservice/management/v1/users/{userid}/invite/delete.json
```

## Actualizar roles

La compatibilidad de la actualización de funciones incluye la posibilidad de agregar y eliminar funciones. Los atributos se pasan como parámetros al cuerpo de la solicitud en formato application/json.

## Agregar roles

El extremo [Add Roles](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/addRolesUsingPOST) toma un único parámetro de ruta de acceso `userid` y agrega uno o más roles de usuario al usuario correspondiente. El cuerpo de la solicitud contiene una lista de uno o más objetos, cada uno de los cuales contiene un  `accessRoleId` y un atributo `workspaceId`. Si se ejecuta correctamente, se devuelve la lista completa de `accessRoleId/workspaceId` pares para el usuario especificado.

```
POST /userservice/management/v1/users/{userid}/roles/create.json
```

```
Content-Type: application/json
```

```json
[
  {
    "accessRoleId": 2,
    "workspaceId": 1008
  }
]
```

```json
[
  {
    "accessRoleId": 1,
    "accessRoleName": "Admin",
    "workspaceId": 0,
    "workspaceName": "AllZones"
  },
  {
    "accessRoleId": 2,
    "accessRoleName": "Standard User",
    "workspaceId": 1008,
    "workspaceName": "World"
  }
]
```

## Eliminar roles

El extremo [Delete Roles](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/deleteRolesUsingPOST) toma un único parámetro de ruta de acceso `userid` y elimina uno o más roles de usuario del usuario correspondiente. El cuerpo de la solicitud contiene una lista de uno o más objetos, cada uno de los cuales contiene un  `accessRoleId` y un atributo `workspaceId`. Si se realiza correctamente, se devuelve la lista restante de pares accessRoleId/workspaceId del usuario especificado.

```
POST /userservice/management/v1/users/{userid}/roles/delete.json
```

```
Content-Type: application/json
```

```json
[
  {
    "accessRoleId": 2,
    "workspaceId": 1008
  }
]
```

```json
[
  {
    "accessRoleId": 1,
    "accessRoleName": "Admin",
    "workspaceId": 0,
    "workspaceName": "AllZones"
  }
]
```
