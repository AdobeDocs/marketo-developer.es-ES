---
title: Operaciones de MCP de Marketo Engage
description: Descubra qué operaciones de MCP de Marketo Engage están disponibles para su uso con asistentes de IA.
autotag-review: '2026-06-02T13:31:42.084Z'
TQID: 'https://experienceleague.adobe.com/qvrWbHOCsCCHctduNDxMhkE8JAKxZk8FCYfKvzxfcYA'
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
    internal-label: Marketo Engage
feature_v2:
  - id: a7170d27-32ab-462b-a333-269abc654483
    internal-label: Smart Campaigns
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
    internal-label: Forms
  - id: dca84292-69e9-4116-a575-667d31fa060d
    internal-label: APIs
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
    internal-label: Programs
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
topic_v2:
  - id: bbbea26f-9621-49eb-9ab8-e06fb3bbce8c
    internal-label: Artificial intelligence
source-git-commit: 1cdb28422994b772a0fa430e213facaf26817952
workflow-type: tm+mt
source-wordcount: '259'
ht-degree: 0%
---

# [!DNL Marketo Engage] operaciones de MCP

El servidor MCP [!DNL Marketo Engage] admite 182 operaciones en la administración de recursos, los datos de posibles clientes y la automatización de campañas.



## Operaciones compatibles

Puede descubrir las operaciones disponibles a través de su asistente de IA conectado.

Si la conexión expone las herramientas de detección, utilice las siguientes indicaciones:

* &quot;Utilice la herramienta Marketo MCP `list_tool_categories` para enumerar las categorías de operación disponibles&quot;.
* &quot;Utilice la herramienta Marketo MCP `search_tools` para buscar operaciones para administrar campañas inteligentes&quot;.

Reemplace la tarea en la segunda solicitud por la operación que necesite. Si no se exponen las herramientas de detección, pídale al asistente de IA que enumere las herramientas disponibles en el servidor MCP de [!DNL Marketo].

Los resultados reflejan los permisos de la conexión y la disponibilidad de las funciones. Describen las herramientas de MCP disponibles, no la API de REST [!DNL Marketo] completa.

Para conexiones que usan credenciales de cliente [!DNL Marketo], no se admiten las siguientes capacidades:

* Herramientas de SMS
* Herramientas de notificación push
* Mensaje en la aplicación y herramientas de programa en la aplicación
* Programando campañas inteligentes con `schedule_smart_campaign`
* Herramientas de administración de usuarios y de servicio masivo

La operación `schedule_campaign` es compatible con conexiones de credenciales de cliente.

Las operaciones disponibles dependen de los permisos de usuario de la API, las características activadas para la instancia de [!DNL Marketo] y cualquier restricción en el cliente de MCP. Algunas operaciones que cambian o eliminan datos requieren confirmación y acceso adicional. La compatibilidad con una categoría no implica acceso a todos los extremos REST.

Para obtener instrucciones de conexión y mensajes de ejemplo, consulte la página [Servidor MCP](mcp-server.md#available-operations). Para obtener información sobre cómo se administran los datos con [!DNL Marketo] IA y el servidor MCP [!DNL Marketo Engage], consulte la página [Información de datos](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/marketo-ai/data-information).