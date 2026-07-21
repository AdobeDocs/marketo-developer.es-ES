---
title: API de REST
feature: REST API
description: Aprenda a utilizar la API de REST de Marketo, configurar usuarios de API y LaunchPoint, ver cuotas y límites, autenticarse con el encabezado Autorización y recuperar posibles clientes.
exl-id: 4b9beaf0-fc04-41d7-b93a-a1ae3147ce67
TQID: https://experienceleague.adobe.com/GqhWI816wWX-2zf89wWj-GXpg9i615HRFVl2ljdYVj0
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: b13bd2ad-8e65-49e5-9691-2a0d31067b35id: c5f60233-d5ea-4453-a799-0ad258b4d399id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 803
ht-degree: 2%

---

# API de REST

La API de REST de Marketo proporciona acceso remoto a muchas funciones del sistema. Puede utilizarlo para crear programas, importar posibles clientes de forma masiva y controlar una instancia de Marketo a nivel detallado.

Las API de REST se dividen en dos grandes categorías:

- Las API [Base de datos de clientes potenciales](https://developer.adobe.com/marketo-apis/api/mapi) recuperan e interactúan con los registros de personas de Marketo y los tipos de objetos asociados, como Oportunidades y Compañías.
- Las API [Asset](https://developer.adobe.com/marketo-apis/api/asset) interactúan con los registros relacionados con los flujos de trabajo y las garantías de marketing.

>[!NOTE]
>
>La API de SOAP se está desaprobando y dejará de estar disponible a partir del 31 de julio de 2026. Todo el nuevo desarrollo debe realizarse con la API de Marketo [REST](./rest-api.md), y los servicios existentes deben migrarse para esa fecha a fin de evitar interrupciones en el servicio. Si tiene un servicio que usa la API de SOAP, consulte la [Guía de migración](../soap-api/migration.md) de la API de SOAP para obtener información sobre cómo migrar.
>

>[!IMPORTANT]
>
>Vea esta [publicación nacional](https://nation.marketo.com/t5/product-blogs/rest-api-double-slash-deprecation/ba-p/358616) sobre la obsolescencia de la doble barra en las URL de puerta de enlace de API.
>

- **Cuota diaria:** A cada suscripción se le asignan 50.000 llamadas de API al día. La cuota se restablece diariamente a las 12:00 (CST). Póngase en contacto con su administrador de cuentas para aumentar la cuota diaria.
- **Límite de velocidad:** Cada instancia está limitada a 100 llamadas de API por 20 segundos.
- **Límite de concurrencia:** Cada instancia permite un máximo de diez llamadas API simultáneas.

Las llamadas a la API estándar tienen una longitud URI máxima de 8 KB y un tamaño corporal máximo de 1 MB. Las llamadas API masivas admiten un tamaño máximo de cuerpo de 10 MB.

Cuando una llamada a contiene un error, la API suele devolver el código de estado HTTP 200. La respuesta JSON contiene un miembro `success` con un valor de `false` y una matriz de errores en el miembro `errors`. Encontrará más información sobre errores [aquí](error-codes.md).

## Introducción

Necesita privilegios de administrador en la instancia de Marketo para completar los siguientes pasos. Este flujo de trabajo crea credenciales de API y las utiliza para recuperar un registro de posibles clientes.

En primer lugar, cree un usuario de API y obtenga credenciales para las llamadas autenticadas. Inicie sesión en su instancia de y vaya a **[!UICONTROL Administración]** > **[!UICONTROL Usuarios y roles]**.

![Usuarios y roles de administrador](assets/admin-users-and-roles.png)

Seleccione la ficha **[!UICONTROL Roles]** y, a continuación, seleccione Nuevo rol. Asigne la función al menos el permiso &quot;Leer-Solo posible cliente&quot; (o &quot;Leer-Solo persona&quot;) del grupo de API de acceso. Asigne un nombre descriptivo al rol y seleccione **[!UICONTROL Crear]**.

![Nuevo rol](assets/new-role.png)

Vuelva a la ficha [!UICONTROL Usuarios] y seleccione **[!UICONTROL Invitar nuevo usuario]**. Escriba un nombre descriptivo que identifique al usuario como usuario de API, escriba una dirección de correo electrónico y seleccione **[!UICONTROL Siguiente]**.

![Nueva información de usuario](assets/new-user-info.png)

Seleccione la opción [!UICONTROL Solo API], asigne el rol de API que creó y seleccione **[!UICONTROL Siguiente]**.

![Nuevos permisos de usuario](assets/new-user-permissions.png)

Seleccione **[!UICONTROL Enviar]** para crear el usuario.

![Nuevo mensaje de usuario](assets/new-user-message.png)

A continuación, vaya al menú [!UICONTROL Administrador] y seleccione **[!UICONTROL LaunchPoint]**.

![Punto de inicio](assets/admin-launchpoint.png)

Seleccione **[!UICONTROL Nuevo]** > **[!UICONTROL Nuevo servicio]**. Escriba un nombre descriptivo y una descripción, y seleccione **[!UICONTROL Personalizado]** del menú [!UICONTROL Servicio]. Seleccione su nuevo usuario del menú [!UICONTROL Usuario solo de API] y, a continuación, seleccione **[!UICONTROL Crear]**.

![Nuevo servicio de punto de inicio](assets/admin-launchpoint-new-service.png)

Seleccione **[!UICONTROL Ver detalles]** para que el nuevo servicio acceda al ID de cliente y al Secreto de cliente. Seleccione **[!UICONTROL Obtener token]** para generar un token de acceso válido por una hora. Guarde el token para la primera llamada de API.

![Obtener token](assets/get-token.png)

Vaya a **[!UICONTROL Administración]** > **[!UICONTROL Servicios web]**.

![Servicios Web](assets/admin-web-services.png)

Busque el [!UICONTROL extremo] en el cuadro API de REST y guárdelo para la primera llamada de API.

![Punto final REST](assets/admin-web-services-rest-endpoint-1.png)

Cada llamada a la API de REST debe incluir un token de acceso en un encabezado HTTP.

```text
Authorization: Bearer cdf01657-110d-4155-99a7-f986b2ff13a0:int
```

>[!IMPORTANT]
>
>El 30 de junio de 2025 se eliminará la compatibilidad con la autenticación mediante el parámetro de consulta **access_token**. Si el proyecto usa un parámetro de consulta para pasar el token de acceso, debe actualizarse para usar el encabezado **Autorización** lo antes posible. El nuevo desarrollo debe usar el encabezado **Authorization** exclusivamente.

Abra una nueva pestaña del explorador e introduzca la siguiente URL. Reemplace los marcadores de posición por el punto de conexión y la dirección de correo electrónico de la instancia para llamar a [Obtener posibles clientes por tipo de filtro](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/getLeadsByFilterUsingGET).

```text
<Your Endpoint URL>/rest/v1/leads.json?&filterType=email&filterValues=<Your Email Address>
```

Si la base de datos no contiene un registro de posibles clientes con su dirección de correo electrónico, utilice la dirección de correo electrónico de un posible cliente existente. Envíe la dirección URL para recibir una respuesta JSON similar al siguiente ejemplo:

```json
{
    "requestId":"c493#1511ca2b184",
    "result":[
       {
           "id":1,
           "updatedAt":"2015-08-24T20:17:23Z",
           "lastName":"Elkington",
           "email":"developerfeedback@marketo.com",
           "createdAt":"2013-02-19T23:17:04Z",
           "firstName":"Kenneth"
        }
    ],
    "success":true
}
```

## Uso de API

El informe de uso de API rastrea cada usuario de API por separado. Asignar un usuario independiente a cada servicio web le ayuda a identificar el uso de API de cada integración.

Si las llamadas superan el límite de instancias y las llamadas subsiguientes fallan, utilice el informe para identificar el volumen de llamadas de cada servicio. Vaya a **[!UICONTROL Administración]** > **[!UICONTROL Integración]** > **[!UICONTROL Servicios web]** y seleccione el número de llamadas realizadas en los últimos siete días.

Para ver los extremos REST que devuelven estadísticas de uso y error diarias y de los últimos 7 días, consulte [Uso](usage.md).
