---
title: Servicios personalizados
feature: REST API
description: Cree Marketo Custom Services, establezca funciones y permisos solo de API, obtenga el ID de cliente y el secreto de cliente en LaunchPoint y obtenga tokens de acceso.
exl-id: 38b05c4c-4404-4c30-a7cb-d31b28a3a72e
TQID: https://experienceleague.adobe.com/lvT-8bYucf-K5LYxb5jQ7BHc137W71SvsGg7cWJlxEs
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b13bd2ad-8e65-49e5-9691-2a0d31067b35
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 894
ht-degree: 9%

---

# Servicios personalizados

Un servicio personalizado proporciona las credenciales utilizadas para autenticarse con Marketo y obtener un token de acceso del servicio de identidad de Marketo [Identity](https://developer.adobe.com/marketo-apis/api/identity/#tag/Identity/operation/identityUsingGET). Cada servicio personalizado tiene un ámbito de un usuario solo de API y deriva sus permisos de ese usuario.

## Funciones

Antes de crear un servicio personalizado, cree una función para asignarla al usuario de solo API correspondiente. Vaya a **[!UICONTROL Administración]** > **[!UICONTROL Usuarios y roles]** > **[!UICONTROL Roles]**.

Las funciones contienen permisos individuales que permiten o restringen el acceso a funciones específicas. En las suscripciones con espacios de trabajo y particiones habilitados, los permisos se asignan por espacio de trabajo. Un usuario solo puede realizar acciones permitidas en los espacios de trabajo en los que tiene esos permisos.

Para crear una función, seleccione **[!UICONTROL Nueva función]**.

![Usuarios y roles](assets/admin-users-and-roles-roles.png)

Asigne un nombre descriptivo a la función. Los usuarios solo de API tienen un conjunto específico de permisos que son independientes de los permisos de usuario estándar. Los permisos de API aparecen en su propia jerarquía bajo el árbol &quot;API de acceso&quot;.

![Nuevos permisos de roles](assets/new-role-access-api-permissions.png)

### Permisos de roles

Solo se aplican permisos en el grupo &quot;API de acceso&quot; a los usuarios de API. La asignación de todos los permisos de administración no concede permisos de API a un usuario.

Cuando cree una función, identifique las acciones que debe realizar la aplicación. Asigne únicamente los permisos mínimos requeridos para esas acciones. Los permisos innecesarios pueden permitir que las integraciones realicen acciones no deseadas en la suscripción.

Use la [herramienta de permisos](endpoint-reference.md) para determinar el conjunto mínimo de permisos. Ver la lista completa de [permisos](#permission_list).

## Usuarios

Después de crear una función, cree un usuario &quot;Solo API&quot;. Otros usuarios administran usuarios solo de API y los usuarios solo de API no pueden iniciar sesión en Marketo. Pueden:

- Crear servicios personalizados
- Permisos de ámbito para esos servicios
- Acceso a API de REST

>[!MORELIKETHIS]
>
>Para crear un usuario solo de API, ve al menú **[!UICONTROL Administración]** > **[!UICONTROL Usuarios y roles]** > **[!UICONTROL Usuarios]** y selecciona **[!UICONTROL Invitar nuevo usuario]**.

![Nueva información de usuario](assets/new-user-info.png)

Asigne al usuario un nombre descriptivo y una dirección de correo electrónico basados en el servicio y la aplicación que utilizarán la cuenta. No es necesario que la dirección de correo electrónico sea válida. Rellene los campos obligatorios, seleccione la casilla de verificación **[!UICONTROL Solo API]** y asigne uno de sus roles de API al usuario. Esta acción asigna el conjunto de permisos de la función al usuario.

![Nuevos permisos de usuario](assets/new-user-permissions.png)

Seleccione **[!UICONTROL Enviar]** para crear el usuario solo de API.

Cuando proporcione credenciales para una nueva aplicación, considere la posibilidad de crear un usuario independiente para el servicio, incluso si otra integración utiliza el mismo conjunto de permisos. Las estadísticas y los errores de uso de llamadas de API se rastrean por usuario.

Un usuario para cada aplicación ayuda a aislar el uso y los problemas en aplicaciones específicas. Esta separación es útil cuando las integraciones alcanzan los límites diarios de llamadas de API o generan errores de API.

## Servicios personalizados

Los servicios personalizados proporcionan el ID de cliente y el Secreto de cliente necesarios para autenticarse con una instancia de Marketo. Para aprovisionar un servicio, ve a **[!UICONTROL Administración]** > **[!UICONTROL Integraciones]** > **[!UICONTROL LaunchPoint]** y selecciona **[!UICONTROL Nuevo servicio]**.

Asigne un nombre descriptivo al servicio. En la lista &quot;Servicio&quot;, seleccione &quot;Personalizar&quot;. Escriba una descripción detallada, seleccione un usuario apropiado en la lista Usuario solo de API y, a continuación, seleccione **[!UICONTROL Crear]**.

![Nuevo servicio personalizado](assets/admin-launchpoint-new-service.png)

El servicio aparece en la lista de servicios de LaunchPoint con la opción &quot;Ver detalles&quot;. Seleccione &quot;Ver detalles&quot; para acceder a las opciones ID de cliente, Secreto de cliente, Usuario propietario y Obtener token.

Utilice Obtener token para realizar pruebas a corto plazo. El token tiene la misma duración que los tokens obtenidos del [servicio de identidad](https://developer.adobe.com/marketo-apis/api/identity/#tag/Identity/operation/identityUsingGET) y es válido durante 3.600 segundos después de la creación.

![Obtener token](assets/get-token.png)

## Espacios de trabajo y particiones

En las suscripciones con espacios de trabajo y particiones, los permisos de función de un usuario en un espacio de trabajo determinan el acceso a los registros y recursos. Cada espacio de trabajo tiene acceso a una o varias particiones y cada posible cliente pertenece a una partición.

Si un usuario solo de API puede leer o escribir registros de posibles clientes en un espacio de trabajo, el usuario puede acceder a todos los registros de las particiones disponibles en ese espacio de trabajo.

Assets pertenece a espacios de trabajo. Un usuario puede leer o escribir un recurso cuando el usuario tiene una función con el permiso necesario en el espacio de trabajo del recurso.

## Lista de permisos

En la tabla siguiente se enumeran los permisos disponibles para los usuarios solo de API y el acceso que concede cada permiso.

| Permiso de rol | Concede acceso a... |
| --- | --- |
| Aprobar recursos | Aprobar recursos |
| Ejecutar campaña | Solicitud o programación de una campaña |
| Actividad de solo lectura | Recuperar actividades de posibles clientes |
| Metadatos de la actividad de solo lectura | Recuperar metadatos de actividad de posible cliente |
| Recursos de solo lectura | Recuperar detalles del recurso |
| Campaña de solo lectura | Recuperar detalles de campaña |
| Compañía de solo lectura | Recuperar detalles de empresa |
| Objeto personalizado de solo lectura | Recuperar detalles de objeto personalizados |
| Guía de solo lectura | Recuperar detalles del posible cliente |
| Cuenta nombrada de solo lectura | Recuperar detalles de la cuenta con nombre |
| Lista de cuentas nombradas de solo lectura | Recuperar detalles de lista de cuentas con nombre |
| Oportunidad de solo lectura | Recuperar detalles de la oportunidad |
| Persona de ventas de solo lectura | Recuperar detalles del vendedor |
| Actividad de solo escritura | Recuperación y creación de actividades de posibles clientes |
| Metadatos de la actividad de escritura y lectura | Recuperar y crear metadatos de actividades de posibles clientes |
| Recursos habilitados para lectura y escritura | Recuperación, creación y actualización de recursos |
| Campaña habilitada para lectura y escritura | Recuperación, creación y actualización de campañas |
| Compañía habilitada para lectura y escritura | Recuperar, crear y actualizar compañías |
| Objeto personalizado habilitado para lectura y escritura | Recuperar, crear y actualizar objetos personalizados |
| Guía de solo escritura | Recuperar, crear y actualizar detalles del posible cliente |
| Cuenta nombrada de lectura y escritura | Recuperar, crear y actualizar cuentas con nombre |
| Lista de cuentas nombradas habilitadas para lectura y escritura | Recuperar, crear y actualizar listas de cuentas con nombre |
| Oportunidad habilitada para lectura y escritura | Recuperar, crear y actualizar oportunidades |
| Persona de ventas habilitada para lectura y escritura | Recuperación, creación y actualización de vendedores |
