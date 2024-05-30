---
title: "API DE REST"
feature: REST API
description: "Resumen de la API de REST"
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '664'
ht-degree: 0%

---


# API de REST

Marketo expone una API de REST que permite la ejecución remota de muchas de las funcionalidades del sistema. Desde la creación de programas hasta la importación masiva de posibles clientes, hay muchas opciones que permiten un control preciso de una instancia de Marketo.

Estas API generalmente se dividen en dos grandes categorías: [Base de datos de posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi/), y [Recurso](https://developer.adobe.com/marketo-apis/api/asset/). Las API de base de datos de posibles clientes permiten recuperar e interactuar con registros de personas de Marketo y tipos de objetos asociados, como Oportunidades y Compañías. Las API de activos permiten la interacción con material promocional y registros relacionados con el flujo de trabajo.

- **Cuota diaria:** A las suscripciones se les asignan 50 000 llamadas de API al día (que se restablecen todos los días a las 12:00 h CST). Puede aumentar su cuota diaria a través de su administrador de cuentas.
- **Límite de velocidad:** Acceso a la API por instancia limitado a 100 llamadas por 20 segundos.
- **Límite de concurrencia:**  Máximo de diez llamadas API simultáneas.

El tamaño de las llamadas estándar está limitado a una longitud URI de 8 KB y un tamaño de cuerpo de 1 MB, aunque el cuerpo puede ser de 10 MB para nuestras API masivas. Si hay un error en con la llamada, la API generalmente sigue devolviendo un código de estado de 200, pero la respuesta JSON contiene un miembro de &quot;éxito&quot; con un valor de `false`y una matriz de errores en el miembro &quot;errors&quot;. Más información sobre errores [aquí](error-codes.md).

## Introducción

Los siguientes pasos requieren privilegios de administrador en la instancia de Marketo.

Para la primera llamada a Marketo, recuperará un registro de posibles clientes. Para empezar a trabajar con Marketo, debe obtener credenciales de API para realizar llamadas autenticadas a su instancia. Inicie sesión en su instancia de y vaya a **[!UICONTROL Administrador]** -> **[!UICONTROL Usuarios y funciones]**.

![Usuarios y funciones de administrador](assets/admin-users-and-roles.png)

Haga clic en [!UICONTROL Funciones] y, a continuación, Nuevo rol y asigne al menos el permiso &quot;Solo lectura de posible cliente&quot; (o &quot;Persona de solo lectura&quot;) al rol en el grupo de API de acceso. Asegúrese de darle un nombre descriptivo y haga clic en [!UICONTROL Crear].

![Nuevo rol](assets/new-role.png)

A continuación, vuelva a la pestaña Usuarios y haga clic en Invitar a nuevo usuario. Asigne un nombre descriptivo al usuario que indique que es un usuario de API y una dirección de correo electrónico, y haga clic en **[!UICONTROL Siguiente]**.

![Información del nuevo usuario](assets/new-user-info.png)

A continuación, marque la opción Solo API, asigne al usuario la función de API que ha creado y haga clic en **[!UICONTROL Siguiente]**.

![Nuevos permisos de usuario](assets/new-user-permissions.png)

Para completar el proceso de creación de usuarios, haga clic en **[!UICONTROL Enviar]**.

![Mensaje de nuevo usuario](assets/new-user-message.png)

A continuación, vaya al menú Administración y haga clic en **[!UICONTROL LaunchPoint]**.

![Launchpoint](assets/admin-launchpoint.png)

Haga clic en el menú New y seleccione [!UICONTROL Nuevo servicio]. Asigne un nombre descriptivo al servicio y seleccione &quot;Personalizado&quot; en el menú desplegable Servicio. Proporcione una descripción y, a continuación, seleccione el nuevo usuario en el menú desplegable Usuario solo de API y haga clic en [!UICONTROL Crear].

![Nuevo servicio de Launchpoint](assets/admin-launchpoint-new-service.png)

Haga clic en Ver detalles del nuevo servicio para acceder al ID de cliente y al secreto de cliente. Por ahora, puede hacer clic en [!UICONTROL Obtener token] para generar un token de acceso válido durante una hora. Guarde el token en una nota por ahora.

![Obtener token](assets/get-token.png)

A continuación, vaya al menú Administración y, a continuación, a **[!UICONTROL Servicios web]**.

![Servicios web](assets/admin-web-services.png)

Busque el extremo en el cuadro API de REST y guárdelo en una nota por ahora.

![Punto final REST](assets/admin-web-services-rest-endpoint-1.png)

Abra una nueva pestaña del explorador e introduzca lo siguiente, utilizando la información adecuada para llamar a [Obtener posibles clientes por tipo de filtro](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadsByFilterUsingGET):

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

Cada uno de los usuarios de la API se recoge de forma individual en el informe de uso de la API, por lo que la división de los servicios web por usuario permite contabilizar fácilmente el uso de cada una de las integraciones. Si el número de llamadas de API a su instancia supera el límite y provoca que las llamadas posteriores fallen, el uso de esta práctica le permite contabilizar el volumen de cada uno de sus servicios y evaluar cómo resolver el problema. Consulte su uso yendo a **[!UICONTROL Administrador]** -> **[!UICONTROL Integración]** > **[!UICONTROL Servicios web]** y haciendo clic en el número de llamadas en los últimos siete días.
