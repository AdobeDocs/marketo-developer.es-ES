---
title: Mensajes en la aplicación
feature: Mobile Marketing
description: Configure los mensajes en la aplicación de Marketo con Mobile SDK, configure los déclencheur de evento personalizados, rastree la actividad del toque y corrija los primeros problemas de inicialización de aperturas de aplicaciones.
exl-id: 73c9f862-d154-4b37-94ce-92311aa756e8
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '312'
ht-degree: 1%

---

# Mensajes en la aplicación

Para utilizar las funcionalidades de mensajería en la aplicación de Marketo, debe realizar los siguientes pasos:

1. Instale Marketo Mobile SDK como se describe en la [Instalación móvil](installation.md).
1. Agregue su aplicación móvil a Marketo como se describe en [Agregar una aplicación móvil](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app).
1. De forma opcional, agrega código a tu aplicación móvil para capturar [acciones personalizadas](custom-actions.md).

Una vez que haya instalado Marketo Mobile SDK y haya completado la adición de la aplicación en Marketo, podrá enviar mensajes en la aplicación que se mostrarán cuando un usuario abra la aplicación.

De forma predeterminada, los mensajes en la aplicación se activan al abrirse la aplicación. Si desea almacenar en déclencheur los mensajes en la aplicación para otros eventos, como cuando se visualiza una página específica o cuando se pulsa un botón específico, debe añadir acciones personalizadas al código. Consulte la sección [Acciones personalizadas](custom-actions.md) para ver ejemplos de código de este tipo.

## Resolución de problemas

**El mensaje en la aplicación no se muestra**

Marketo responde a los déclencheur de las aplicaciones solo después de inicializar Marketo Mobile SDK con la plataforma de Marketo. El proceso de inicialización se produce cuando se instala y se abre la aplicación por primera vez. Dado que la inicialización se produce después de la primera apertura de la aplicación, el evento &quot;Apertura de la aplicación&quot; no se activa hasta que la aplicación se abre por segunda vez. Cierre la aplicación y ábrala de nuevo. En el dispositivo debería aparecer un mensaje activado por la acción Abrir aplicación.

Los eventos personalizados se activan mediante la interacción del usuario una vez abierta la aplicación. Marketo reconoce los eventos personalizados durante la primera sesión.

**Seguimiento de actividad táctil en la aplicación**

Asegúrese de asignar una acción además de &quot;descartar&quot; a uno de los botones principales o secundarios para rastrear las actividades de toque y para utilizar frecuencias de visualización base basadas en el número de toques.

Para obtener más información, consulte la sección [Mensajes en la aplicación](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/in-app-messages/creating-in-app-messages/create-an-in-app-message) en nuestra documentación del producto.
