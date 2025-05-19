---
title: Autenticación
feature: REST API
description: Autenticación de usuarios de Marketo para el uso de API.
exl-id: f89a8389-b50c-4e86-a9e4-6f6acfa98e7e
source-git-commit: 206724e5177eb9040fa36202d91b2ed9daa734c3
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 0%

---

# Autenticación

Las API de REST de Marketo están autenticadas con OAuth 2.0 de 2 patas. Los ID y los secretos de cliente los proporcionan servicios personalizados definidos por el usuario. Cada servicio personalizado es propiedad de un usuario solo de API que tiene un conjunto de funciones y permisos que autorizan al servicio a realizar acciones específicas. Un token de acceso está asociado a un único servicio personalizado. La caducidad del token de acceso es independiente de los tokens asociados a otros servicios personalizados que pueden estar presentes en una instancia.

## Creación de un token de acceso

`Client ID` y `Client Secret` se encuentran en el menú **[!UICONTROL Administración]** > **[!UICONTROL Integración]** > **[!UICONTROL LaunchPoint]** al seleccionar el servicio personalizado y hacer clic en **[!UICONTROL Ver detalles]**.

![Obtener detalles del servicio REST](assets/authentication-service-view-details.png)

![Credenciales de Launchpoint](assets/admin-launchpoint-credentials.png)

Se encuentra `Identity URL` en el menú **[!UICONTROL Administración]** > **[!UICONTROL Integración]** > **[!UICONTROL Servicios web]** de la sección API de REST.

Cree un token de acceso utilizando una petición HTTP GET (o POST) como se indica a continuación:

```
GET <Identity URL>/oauth/token?grant_type=client_credentials&client_id=<Client Id>&client_secret=<Client Secret>
```

Si su solicitud era válida, recibe una respuesta JSON similar a la siguiente:

```json
{
    "access_token": "cdf01657-110d-4155-99a7-f986b2ff13a0:int",
    "token_type": "bearer",
    "expires_in": 3599,
    "scope": "apis@acmeinc.com"
}
```

Definición de respuesta

- `access_token`: el token que pasa con las llamadas subsiguientes para autenticarse con la instancia de destino.
- `token_type`: método de autenticación OAuth.
- `expires_in`: duración restante del token actual en segundos (después de la cual no es válido). Cuando se crea un token de acceso originalmente, su duración es de 3600 segundos o una hora.
- `scope`: el usuario propietario del servicio personalizado que se utilizó para autenticarse.

## Uso de un token de acceso

Al realizar llamadas a métodos de API de REST, se debe incluir un token de acceso en cada llamada para que la llamada se realice correctamente.
El token de acceso debe enviarse como un encabezado HTTP.

>[!IMPORTANT]
>
>La compatibilidad con la autenticación mediante el parámetro de consulta `access_token` se eliminará el 31 de octubre de 2025. Si su proyecto usa un parámetro de consulta para pasar el token de acceso, debe actualizarse para usar el [encabezado de autorización](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/rest/authentication#using-an-access-token) lo antes posible. El nuevo desarrollo debe utilizar exclusivamente el encabezado `Authorization`.

### Cambio al encabezado Autorización


Para dejar de usar el parámetro de consulta `access_token` y cambiar al encabezado Autorización, se requiere un pequeño cambio de código.

Utilizando CURL como ejemplo, este código envía el valor `access_token` como parámetro de formulario (indicador -F):

```bash
curl ...  -F access_token=<Access Token> <REST API Endpoint Base URL>/bulk/v1/apiCall.json
```

Este código envía el mismo valor que el encabezado http `Authorization: Bearer` (indicador -H):

```bash
curl ... -H 'Authorization: Bearer <Access Token>' <REST API Endpoint Base URL>/bulk/v1/apiCall.json
```

## Sugerencias y prácticas recomendadas

La administración de la caducidad del token de acceso es importante para garantizar que la integración funcione correctamente y evitar que se produzcan errores de autenticación inesperados durante el funcionamiento normal. Al diseñar la autenticación para la integración, asegúrese de almacenar el token y el periodo de caducidad contenidos en la respuesta de identidad.

Antes de realizar cualquier llamada a REST, debe comprobar la validez del token en función de su vida útil restante. Si el token ha caducado, renuévelo llamando al extremo [Identity](https://developer.adobe.com/marketo-apis/api/identity/#tag/Identity/operation/identityUsingGET). Esto ayuda a garantizar que la llamada de REST nunca falle debido a un token caducado. Esto le ayuda a administrar la latencia de sus llamadas REST de una manera predecible, lo que es crucial para las aplicaciones del usuario final.

Si se utiliza un token caducado para autenticar una llamada REST, la llamada REST fallará y devolverá un código de error 602. Si se utiliza un token no válido para autenticar una llamada de REST, se devuelve un código de error 601. Si se recibe cualquiera de estos códigos, el cliente debe renovar el token llamando al extremo de identidad.

Si llama al extremo de identidad antes de que caduque el token, en la respuesta se devolverá el mismo token y la duración restante.

Recuerde que los tokens de acceso son propiedad de cada Servicio personalizado y no de cada usuario. Aunque dos respuestas de identidad pueden tener ámbitos para el mismo usuario, los tokens de acceso y los períodos de caducidad son independientes entre sí si se realizaron con credenciales de dos servicios diferentes. Tenga esto en cuenta cuando tenga varios conjuntos de credenciales en la misma aplicación; el ID de cliente puede ser una clave útil para administrarlos de forma independiente.
