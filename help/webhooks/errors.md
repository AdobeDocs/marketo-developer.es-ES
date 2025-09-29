---
title: Errores
feature: Webhooks
description: Conozca los códigos de error de webhook de Marketo, por qué se requieren respuestas 2xx para actualizar los campos de posibles clientes y cómo detectar y gestionar errores con Webhook.
exl-id: adce40c3-87b1-4f31-8995-eb64e8a72b55
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 2%

---

# Errores

En esta página se muestran los códigos de respuesta de error para los webhooks en Marketo.

Marketo genera 1000 y 1001, y del sistema al que llama el webhook de Marketo devuelve errores de 2xx al 5xx.

Para que Marketo vuelva a asignar valores a un campo, el código de respuesta del gancho web debe ser de la variedad 2xx. Si la intención del webhook es cambiar los valores del registro de posibles clientes de Marketo a través de la respuesta, el servicio web al que se llama debe devolver 2xx, todos los demás códigos de respuesta harán que el webhook se ignore con el fin de actualizar los valores del registro de posibles clientes.

| Código de respuesta | Descripción |
| --- | --- |
| 1000 | Esto indica que la acción de flujo &quot;Llamar al webhook&quot; se está alojando dentro de una campaña por lotes. Los webhooks solo se pueden activar desde campañas de déclencheur. |
| 1001 | Esto indica que el servicio web emitió un cuerpo de respuesta vacío. |

## Error de captura de webhook

El déclencheur [!UICONTROL Se llama al webhook] puede detectar y controlar los errores de los webhooks:

Se llama al webhook ![1}](assets/webhook-called.png)

* Respuesta: la respuesta es la carga útil de respuesta literal que recibió la solicitud.
* Tipo de error: corresponde a la frase Motivo del mensaje de estado HTTP.

Se pueden utilizar para gestionar y reaccionar ante errores y excepciones predecibles. Según el servicio con el que se integre, puede ser posible recuperar ciertas clases de errores automáticamente, mientras que se pueden crear alertas para notificar a los usuarios de errores inesperados.
