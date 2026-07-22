---
title: Mensajes en la aplicación
feature: Mobile Marketing
description: Configure los mensajes en la aplicación de Marketo con Mobile SDK, configure los déclencheur de evento personalizados, rastree la actividad del toque y corrija los primeros problemas de inicialización de aperturas de aplicaciones.
exl-id: 73c9f862-d154-4b37-94ce-92311aa756e8
TQID: https://experienceleague.adobe.com/RVkEUBaFb-PHd0gE9ngzYc5zOojINwSI7ic2TmcU7-8
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 321
ht-degree: 2%

---

# Mensajes en la aplicación

Complete estos pasos para utilizar la mensajería en la aplicación de Marketo:

1. Instale Marketo Mobile SDK como se describe en la [Instalación móvil](installation.md).
1. Agregue su aplicación móvil a Marketo como se describe en [Agregar una aplicación móvil](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app).
1. Opcional: agregue código a su aplicación móvil para capturar [acciones personalizadas](custom-actions.md).

Después de instalar Marketo Mobile SDK y agregar la aplicación a Marketo, puede enviar mensajes en la aplicación que aparecen cuando un usuario abre la aplicación.

De forma predeterminada, los mensajes en la aplicación se activan al abrirla. Para almacenar en déclencheur un mensaje para otro evento, como ver una página específica o seleccionar un botón específico, agregue una acción personalizada al código. Consulte [Acciones personalizadas](custom-actions.md) para ver ejemplos de código.

## Resolución de problemas

**El mensaje en la aplicación no se muestra**

Marketo solo responde a los déclencheur de la aplicación después de que Marketo Mobile SDK se inicialice con la plataforma de Marketo. La inicialización se produce cuando se instala y abre la aplicación por primera vez.

Debido a que la inicialización se produce después de la primera vez que se abre la aplicación, el evento &quot;Apertura de la aplicación&quot; no se activa hasta que se abre la aplicación por segunda vez. Cierre y vuelva a abrir la aplicación. En ese caso, debería aparecer en el dispositivo un mensaje activado por la apertura de la aplicación.

Los eventos personalizados se activan mediante la interacción del usuario una vez abierta la aplicación. Marketo reconoce los eventos personalizados durante la primera sesión.

**Seguimiento de actividad táctil en la aplicación**

Para realizar un seguimiento de las actividades de punteo y basar la frecuencia de visualización en el número de punteos, asigne una acción que no sea &quot;descartar&quot; a un botón principal o secundario.

Para obtener más información, consulte [Mensajes en la aplicación](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/mobile-marketing/in-app-messages/creating-in-app-messages/create-an-in-app-message) en la documentación del producto.
