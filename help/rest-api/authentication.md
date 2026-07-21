---
title: Autenticación
feature: REST API
description: Autentique las API de REST de Marketo con 2 OAuth 2.0 legged, cree y utilice tokens de acceso, cambie al encabezado Autorización, administre la caducidad y gestione los errores 601 y 602.
exl-id: f89a8389-b50c-4e86-a9e4-6f6acfa98e7e
TQID: https://experienceleague.adobe.com/cIeI0m61CyIWq4HEosZ-QAsxzZb0WcrQRpCud2qysfY
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 528
ht-degree: 0%

---

# Autenticación

Las API de REST de Marketo utilizan OAuth 2.0 de 2 patas para la autenticación. Un servicio personalizado proporciona el ID de cliente y el Secreto de cliente utilizados para obtener un token de acceso.

Cada servicio personalizado pertenece a un usuario solo de API. Las funciones y los permisos del usuario autorizan al servicio a realizar acciones específicas. Un token de acceso pertenece a un único servicio personalizado y su caducidad es independiente de los tokens de otros servicios personalizados de la instancia.

## Creación de un token de acceso

Para encontrar `Client ID` y `Client Secret`, vaya a **[!UICONTROL Administración]** > **[!UICONTROL Integración]** > **[!UICONTROL LaunchPoint]**. Seleccione el servicio personalizado y, a continuación, seleccione **[!UICONTROL Ver detalles]**.

![Obtener detalles del servicio REST](assets/authentication-service-view-details.png)

![Credenciales de Launchpoint](assets/admin-launchpoint-credentials.png)

Para encontrar `Identity URL`, vaya a **[!UICONTROL Administración]** > **[!UICONTROL Integración]** > **[!UICONTROL Servicios web]**. La dirección URL aparece en la sección API de REST.

Cree un token de acceso con una petición HTTP GET o POST:

```http
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

La respuesta contiene los campos siguientes:

- `access_token`: el token que pasa con las llamadas subsiguientes para autenticarse con la instancia de destino.
- `token_type`: método de autenticación de OAuth.
- `expires_in`: la duración restante del token actual, en segundos. Un nuevo token de acceso tiene una duración de 3600 segundos o una hora.
- `scope`: el usuario propietario del servicio personalizado usado para autenticarse.

## Uso de un token de acceso

Cada llamada a la API de REST debe incluir un token de acceso en un encabezado HTTP.

>[!IMPORTANT]
>
>La compatibilidad con la autenticación mediante el parámetro de consulta `access_token` se eliminará el 31 de agosto de 2026. Si su proyecto usa un parámetro de consulta para pasar el token de acceso, debe actualizarse para usar el [encabezado de autorización](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/rest/authentication#using-an-access-token) lo antes posible. El nuevo desarrollo debe utilizar exclusivamente el encabezado `Authorization`.

### Cambio al encabezado Autorización

Para reemplazar el parámetro de consulta `access_token` por un encabezado de Autorización, actualice la forma en que la solicitud envía el token.

El siguiente ejemplo de cURL envía el valor `access_token` como parámetro de formulario con el indicador `-F`:

```bash
curl ...  -F access_token=<Access Token> <REST API Endpoint Base URL>/bulk/v1/apiCall.json
```

El ejemplo siguiente envía el mismo valor en el encabezado HTTP `Authorization: Bearer` con el indicador `-H`:

```bash
curl ... -H 'Authorization: Bearer <Access Token>' <REST API Endpoint Base URL>/bulk/v1/apiCall.json
```

## Sugerencias y prácticas recomendadas

Almacene el token de acceso y el periodo de caducidad de la respuesta de identidad. La administración de la caducidad del token ayuda a evitar errores de autenticación inesperados durante el funcionamiento normal.

Antes de realizar una llamada de REST, compruebe la vida útil restante del token. Si el token ha caducado, renuévelo llamando al extremo [Identity](https://developer.adobe.com/marketo-apis/api/identity/#tag/Identity/operation/identityUsingGET). La renovación proactiva evita errores causados por tokens caducados y hace que la latencia de las llamadas a REST sea más predecible, lo que es importante para las aplicaciones del usuario final.

Los errores de autenticación devuelven los códigos siguientes:

- `602`: el token de acceso ha caducado.
- `601`: token de acceso no válido.

Si el cliente recibe cualquiera de los códigos, renueve el token llamando al punto de conexión de identidad.

Si llama al extremo de identidad antes de que caduque el token, la respuesta devolverá el mismo token y la duración restante.

Los tokens de acceso pertenecen a servicios personalizados, no a usuarios. Si las credenciales de dos servicios diferentes producen respuestas de identidad con ámbito para el mismo usuario, sus tokens de acceso y períodos de caducidad siguen siendo independientes.

Cuando una aplicación utiliza varios conjuntos de credenciales, utilice el ID de cliente como clave para administrar cada token de forma independiente.
