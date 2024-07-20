---
title: API de REST
feature: REST API
description: Resumen de API de REST
exl-id: 4b9beaf0-fc04-41d7-b93a-a1ae3147ce67
source-git-commit: 6fc45ff98998217923e2a5b02d00d1522fe3272c
workflow-type: tm+mt
source-wordcount: '664'
ht-degree: 1%

---

# API de REST

Marketo expone una API de REST que permite la ejecución remota de muchas de las funcionalidades del sistema. Desde la creación de programas hasta la importación masiva de posibles clientes, hay muchas opciones que permiten un control preciso de una instancia de Marketo.

Estas API generalmente se dividen en dos categorías amplias: [Base de datos de posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi/) y [Recurso](https://developer.adobe.com/marketo-apis/api/asset/). Las API de base de datos de posibles clientes permiten recuperar e interactuar con registros de personas de Marketo y tipos de objetos asociados, como Oportunidades y Compañías. Las API de activos permiten la interacción con material promocional y registros relacionados con el flujo de trabajo.

- **Cuota diaria:** a las suscripciones se les asignan 50 000 llamadas API al día (se restablece diariamente a las 12:00 horas CST). Puede aumentar su cuota diaria a través de su administrador de cuentas.
- **Límite de velocidad:** Acceso a API por instancia limitado a 100 llamadas por 20 segundos.
- **Límite de simultaneidad:**  Máximo de diez llamadas API simultáneas.

El tamaño de las llamadas estándar está limitado a una longitud URI de 8 KB y un tamaño de cuerpo de 1 MB, aunque el cuerpo puede ser de 10 MB para nuestras API masivas. Si hay un error en con la llamada, la API generalmente devolverá un código de estado de 200, pero la respuesta JSON contendrá un miembro &quot;success&quot; con un valor de `false`, y una matriz de errores en el miembro &quot;errors&quot;. Más información sobre los errores [aquí](error-codes.md).

## Introducción

Los siguientes pasos requieren privilegios de administrador en la instancia de Marketo.

Para la primera llamada a Marketo, recuperará un registro de posibles clientes. Para empezar a trabajar con Marketo, debe obtener credenciales de API para realizar llamadas autenticadas a su instancia. Inicie sesión en su instancia y vaya a **[!UICONTROL Administrador]** -> **[!UICONTROL Usuarios y funciones]**.

![Usuarios y roles de administrador](assets/admin-users-and-roles.png)

Haga clic en la ficha **[!UICONTROL Roles]** y, a continuación, en Nuevo rol y asigne al menos el permiso &quot;Posible cliente de solo lectura&quot; (o &quot;Persona de solo lectura&quot;) al rol en el grupo de API de acceso. Asegúrese de darle un nombre descriptivo y haga clic en **[!UICONTROL Crear]**.

![Nuevo rol](assets/new-role.png)

Ahora vuelve a la ficha [!UICONTROL Usuarios] y haz clic en **[!UICONTROL Invitar a nuevo usuario]**. Asigne un nombre descriptivo al usuario que indique que es un usuario de API y una dirección de correo electrónico. Luego, haga clic en **[!UICONTROL Siguiente]**.

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

Abra una nueva ficha del explorador e introduzca lo siguiente, con la información apropiada para llamar a [Obtener posibles clientes por tipo de filtro](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadsByFilterUsingGET):

```
<Your Endpoint URL>/rest/v1/leads.json?access_token=<Your Access Token>&filterType=email&filterValues=<Your Email Address>
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
