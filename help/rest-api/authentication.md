---
title: "Autenticación"
feature: REST API
description: '"Autenticación de usuarios de Marketo para el uso de API".'
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '540'
ht-degree: 0%

---


# Autenticación

Las API de REST de Marketo están autenticadas con OAuth 2.0 de 2 patas. Los ID y los secretos de cliente los proporcionan servicios personalizados definidos por el usuario. Cada servicio personalizado es propiedad de un usuario solo de API que tiene un conjunto de funciones y permisos que autorizan al servicio a realizar acciones específicas. Un token de acceso está asociado a un único servicio personalizado. La caducidad del token de acceso es independiente de los tokens asociados a otros servicios personalizados que pueden estar presentes en una instancia.

## Creación de un token de acceso

El `Client ID` y `Client Secret` se encuentran en la **[!UICONTROL Administrador]** > **[!UICONTROL Integración]** > **[!UICONTROL LaunchPoint]** seleccionando el servicio personalizado y haciendo clic en **[!UICONTROL Ver detalles]**.

![Obtener detalles del servicio REST](assets/authentication-service-view-details.png)

![Credenciales de Launchpoint](assets/admin-launchpoint-credentials.png)

El `Identity URL` se encuentra en **[!UICONTROL Administrador]** > **[!UICONTROL Integración]** > **[!UICONTROL Servicios web]** en la sección API de REST.

Cree un token de acceso mediante una solicitud de GET HTTP (o POST) como se indica a continuación:

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

- `access_token` : el token que pasa con las llamadas subsiguientes para autenticarse con la instancia de destino.
- `token_type` - El método de autenticación OAuth.
- `expires_in` : la duración restante del token actual en segundos (después de la cual no es válido). Cuando se crea un token de acceso originalmente, su duración es de 3600 segundos o una hora.
- `scope` : el usuario propietario del servicio personalizado que se utilizó para autenticarse.

## Uso de un token de acceso

Al realizar llamadas a métodos de API de REST, se debe incluir un token de acceso en cada llamada para que la llamada se realice correctamente.

Existen dos métodos que puede utilizar para incluir un token en las llamadas, como encabezado HTTP o como parámetro de cadena de consulta:

1. Encabezado HTTP

   `Authorization: Bearer cdf01657-110d-4155-99a7-f986b2ff13a0:int`

1. Parámetro de consulta

   `access_token=cdf01657-110d-4155-99a7-f986b2ff13a0:int`

## Sugerencias y prácticas recomendadas

La administración de la caducidad del token de acceso es importante para garantizar que la integración funcione correctamente y evitar que se produzcan errores de autenticación inesperados durante el funcionamiento normal. Al diseñar la autenticación para la integración, asegúrese de almacenar el token y el periodo de caducidad contenidos en la respuesta de identidad.

Antes de realizar cualquier llamada a REST, debe comprobar la validez del token en función de su vida útil restante. Si el token ha caducado, renuévelo llamando a [Identidad](https://developer.adobe.com/marketo-apis/api/identity/#tag/Identity/operation/identityUsingGET)punto final. Esto ayuda a garantizar que la llamada de REST nunca falle debido a un token caducado. Esto le ayuda a administrar la latencia de sus llamadas REST de una manera predecible, lo que es crucial para las aplicaciones del usuario final.

Si se utiliza un token caducado para autenticar una llamada REST, la llamada REST fallará y devolverá un código de error 602. Si se utiliza un token no válido para autenticar una llamada de REST, se devuelve un código de error 601. Si se recibe cualquiera de estos códigos, el cliente debe renovar el token llamando al extremo de identidad.

Si llama al extremo de identidad antes de que caduque el token, en la respuesta se devolverá el mismo token y la duración restante.

Recuerde que los tokens de acceso son propiedad de cada Servicio personalizado y no de cada usuario. Aunque dos respuestas de identidad pueden tener ámbitos para el mismo usuario, los tokens de acceso y los períodos de caducidad son independientes entre sí si se realizaron con credenciales de dos servicios diferentes. Tenga esto en cuenta cuando tenga varios conjuntos de credenciales en la misma aplicación; el ID de cliente puede ser una clave útil para administrarlos de forma independiente.
