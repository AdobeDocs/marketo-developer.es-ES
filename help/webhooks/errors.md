---
title: Errores
feature: Webhooks
description: Conozca los códigos de error de webhook de Marketo, por qué se requieren respuestas 2xx para actualizar los campos de posibles clientes y cómo detectar y gestionar errores con Webhook.
exl-id: adce40c3-87b1-4f31-8995-eb64e8a72b55
TQID: https://experienceleague.adobe.com/N2jNA4EUMMTUFL9uJHZhOor6Tlz4-EXWciwoXrPml48
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 213
ht-degree: 3%

---

# Errores

En esta página se describen los códigos de respuesta de error para los webhooks de Marketo y se explica cómo gestionar los errores de webhook.

Marketo genera los códigos de error 1000 y 1001. El sistema al que llama el webhook de Marketo devuelve códigos de respuesta de 2xx a 5xx.

Marketo asigna los valores de respuesta a un campo solo cuando el servicio web devuelve un código de respuesta 2xx. Si la respuesta de un gancho web está diseñada para cambiar los valores de un registro de posibles clientes de Marketo, todos los demás códigos de respuesta hacen que Marketo ignore la respuesta de las actualizaciones de campo.

| Código de respuesta | Descripción |
| --- | --- |
| 1000 | Esto indica que la acción de flujo &quot;Llamar al webhook&quot; se está alojando dentro de una campaña por lotes. Los webhooks solo se pueden activar desde campañas de déclencheur. |
| 1001 | Esto indica que el servicio web emitió un cuerpo de respuesta vacío. |

## Error de captura de webhook

Utilice el déclencheur **[!UICONTROL Se llama al webhook]** para detectar y controlar los errores de webhook:

Se llama al webhook ![1&rbrace;](assets/webhook-called.png)

* **Respuesta** - La carga de respuesta literal recibida por la solicitud.
* **Tipo de error** - La frase de razón del mensaje de estado HTTP.

Utilice estos valores para responder a errores y excepciones predecibles. Según el servicio integrado, puede recuperarse automáticamente de algunas clases de error y crear alertas para errores inesperados.
