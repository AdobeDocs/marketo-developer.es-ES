---
title: API de REST
feature: REST API
description: Aprenda a utilizar la API de REST de Marketo, configurar usuarios de API y LaunchPoint, ver cuotas y límites, autenticarse con el encabezado Autorización y recuperar posibles clientes.
exl-id: 4b9beaf0-fc04-41d7-b93a-a1ae3147ce67
source-git-commit: 5881ab969eca3a37d19f56b6570e42828994eff3
workflow-type: tm+mt
source-wordcount: '844'
ht-degree: 1%

---

# API de REST

Marketo expone una API de REST que permite la ejecución remota de muchas de las funcionalidades del sistema. Desde la creación de programas hasta la importación masiva de posibles clientes, hay muchas opciones que permiten un control preciso de una instancia de Marketo.

Estas API generalmente se dividen en dos categorías amplias: [Base de datos de posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi/) y [Recurso](https://developer.adobe.com/marketo-apis/api/asset/). Las API de base de datos de posibles clientes permiten recuperar e interactuar con los registros de personas de Marketo y los tipos de objetos asociados, como Oportunidades y Compañías. Las API de activos permiten la interacción con material promocional y registros relacionados con el flujo de trabajo.

>[!NOTE]
>La API de SOAP se está desaprobando y dejará de estar disponible a partir del 31 de marzo de 2026. Todo el nuevo desarrollo debe realizarse con la API de Marketo [REST](./rest-api.md), y los servicios existentes deben migrarse para esa fecha a fin de evitar interrupciones en el servicio. Si tiene un servicio que usa la API de SOAP, consulte la [Guía de migración](../soap-api/migration.md) de la API de SOAP para obtener información sobre cómo migrar.
>

>[!IMPORTANT]
>Vea esta [publicación nacional](https://nation.marketo.com/t5/product-blogs/rest-api-double-slash-deprecation/ba-p/358616) sobre la obsolescencia de la doble barra en las URL de puerta de enlace de API.
>

- **Cuota diaria:** a las suscripciones se les asignan 50 000 llamadas API al día (se restablece diariamente a 12:00AM CST). Puede aumentar su cuota diaria a través de su administrador de cuentas.
- **Límite de velocidad:** El acceso a la API por instancia está limitado a 100 llamadas por 20 segundos.
- **Límite de simultaneidad:**  Máximo de diez llamadas API simultáneas.

El tamaño de las llamadas estándar está limitado a una longitud URI de 8 KB y un tamaño de cuerpo de 1 MB, aunque el cuerpo puede ser de 10 MB para nuestras API masivas. Si hay un error en con la llamada, la API generalmente devolverá un código de estado de 200, pero la respuesta JSON contendrá un miembro &quot;success&quot; con un valor de `false`, y una matriz de errores en el miembro &quot;errors&quot;. Más información sobre los errores [aquí](error-codes.md).

## Introducción

Los siguientes pasos requieren privilegios de administrador en la instancia de Marketo.

Para la primera llamada a Marketo, recupera un registro de posibles clientes. Para empezar a trabajar con Marketo, debe obtener credenciales de API para realizar llamadas autenticadas a su instancia. Inicie sesión en su instancia de y vaya a **[!UICONTROL Administrador]** -> **[!UICONTROL Usuarios y funciones]**.

![Usuarios y roles de administrador](assets/admin-users-and-roles.png)

Haga clic en la ficha **[!UICONTROL Roles]** y, a continuación, en Nuevo rol y asigne al menos el permiso &quot;Posible cliente de solo lectura&quot; (o &quot;Persona de solo lectura&quot;) al rol en el grupo de API de acceso. Asegúrese de darle un nombre descriptivo y haga clic en **[!UICONTROL Crear]**.

![Nuevo rol](assets/new-role.png)

Ahora, vuelve a la ficha [!UICONTROL Usuarios] y haz clic en **[!UICONTROL Invitar nuevo usuario]**. Asigne un nombre descriptivo al usuario que indique que es un usuario de API y una dirección de correo electrónico. Luego, haga clic en **[!UICONTROL Siguiente]**.

![Nueva información de usuario](assets/new-user-info.png)

A continuación, marque la opción [!UICONTROL Solo API], asigne al usuario la función de API que creó y haga clic en **[!UICONTROL Siguiente]**.

![Nuevos permisos de usuario](assets/new-user-permissions.png)

Para completar el proceso de creación de usuarios, haga clic en **[!UICONTROL Enviar]**.

![Nuevo mensaje de usuario](assets/new-user-message.png)

A continuación, vaya al menú [!UICONTROL Administrador] y haga clic en **[!UICONTROL LaunchPoint]**.

![Punto de inicio](assets/admin-launchpoint.png)

Haga clic en el menú **[!UICONTROL Nuevo]** y seleccione **[!UICONTROL Nuevo servicio]**. Asigne un nombre descriptivo al servicio y seleccione **[!UICONTROL Personalizado]** en el menú desplegable [!UICONTROL Servicio]. Asígnele una descripción y, a continuación, seleccione el nuevo usuario en el menú desplegable [!UICONTROL Usuario solo de API] y haga clic en **[!UICONTROL Crear]**.

![Nuevo servicio de punto de inicio](assets/admin-launchpoint-new-service.png)

Haga clic en **[!UICONTROL Ver detalles]** para que el nuevo servicio acceda al ID de cliente y al Secreto de cliente. Por ahora, puede hacer clic en el botón **[!UICONTROL Obtener token]** para generar un token de acceso válido por una hora. Guarde el token en una nota por ahora.

![Obtener token](assets/get-token.png)

A continuación, ve al menú **[!UICONTROL Administrador]** y luego a **[!UICONTROL Servicios Web]**.

![Servicios Web](assets/admin-web-services.png)

Busque el [!UICONTROL extremo] en el cuadro de la API de REST y guárdelo en una nota por ahora.

![Punto final REST](assets/admin-web-services-rest-endpoint-1.png)

Al realizar llamadas a métodos de API de REST, se debe incluir un token de acceso en cada llamada para que la llamada se realice correctamente. El token de acceso debe enviarse como un encabezado HTTP.

```
Authorization: Bearer cdf01657-110d-4155-99a7-f986b2ff13a0:int
```

>[!IMPORTANT]
>
>El 30 de junio de 2025 se eliminará la compatibilidad con la autenticación mediante el parámetro de consulta **access_token**. Si el proyecto usa un parámetro de consulta para pasar el token de acceso, debe actualizarse para usar el encabezado **Autorización** lo antes posible. El nuevo desarrollo debe usar el encabezado **Authorization** exclusivamente.

Abra una nueva ficha del explorador e introduzca lo siguiente, con la información apropiada para llamar a [Obtener posibles clientes por tipo de filtro](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadsByFilterUsingGET)

```
<Your Endpoint URL>/rest/v1/leads.json?&filterType=email&filterValues=<Your Email Address>
```

Si no tiene un registro de posibles clientes con su dirección de correo electrónico en la base de datos, cámbiela por una que sepa que está allí. Pulse Intro en la barra URL y obtenga una respuesta JSON similar a la siguiente:

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

Cada uno de los usuarios de la API se recoge de forma individual en el informe de uso de la API, por lo que la división de los servicios web por usuario permite contabilizar fácilmente el uso de cada una de las integraciones. Si el número de llamadas de API a su instancia supera el límite y provoca que las llamadas posteriores fallen, el uso de esta práctica le permite contabilizar el volumen de cada uno de sus servicios y evaluar cómo resolver el problema. Para ver su uso, vaya a **[!UICONTROL Administración]** -> **[!UICONTROL Integración]** > **[!UICONTROL Servicios web]** y haga clic en el número de llamadas en los últimos siete días.
