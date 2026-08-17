---
title: Operaciones de MCP de Marketo Engage
description: Descubra qué operaciones de MCP de Marketo Engage están disponibles para su uso con asistentes de IA.
autotag-review: '2026-06-02T13:31:42.084Z'
TQID: 'https://experienceleague.adobe.com/qvrWbHOCsCCHctduNDxMhkE8JAKxZk8FCYfKvzxfcYA'
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: a7170d27-32ab-462b-a333-269abc654483
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
  - id: dca84292-69e9-4116-a575-667d31fa060d
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
topic_v2:
  - id: bbbea26f-9621-49eb-9ab8-e06fb3bbce8c
source-git-commit: c631b7c3d571f29083673f9b97d22230d109abfc
workflow-type: tm+mt
source-wordcount: 1228
ht-degree: 49%

---


# [!DNL Marketo Engage] operaciones de MCP

Las siguientes operaciones están disponibles a través del servidor MCP [!DNL Marketo Engage]. El servidor proporciona extremos de solo lectura o no destructivos. El sistema de IA no puede usar `Delete` u otras operaciones destructivas.

>[!NOTE]
>
>Las herramientas de listas inteligentes y campañas inteligentes `create` y `update` están destinadas a una versión de septiembre de 2026.

Para obtener información sobre cómo se administran los datos con la IA de Marketo y el servidor MCP de Marketo Engage, consulte la página [Información de datos](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/marketo-ai/data-information).

## Exportación masiva

[Referencia de API de exportación masiva](https://developer.adobe.com/marketo-apis/api/mapi){target="_blank"}

- `bulk_export_create`
- `bulk_export_enqueue`
- `bulk_export_file`
- `bulk_export_status`
- `get_import_status`

## Canales y etiquetas

[Referencia de API de canales](https://developer.adobe.com/marketo-apis/api/asset#tag/Channels){target="_blank"} | [Referencia de API de etiquetas](https://developer.adobe.com/marketo-apis/api/asset#tag/Tags){target="_blank"}

- `browse_channels`
- `browse_tag_types`
- `get_channel_by_name`
- `get_tag_type_by_name`

## Correos electrónicos

[Referencia de API de correos electrónicos](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails){target="_blank"}

- `approve_email`
- `browse_emails`
- `create_email`
- `get_email_by_id`
- `get_email_by_name`
- `get_email_content`
- `update_email_content`

## Carpetas

[Referencia de API de carpetas](https://developer.adobe.com/marketo-apis/api/asset#tag/Folders){target="_blank"}

- `browse_folders`
- `create_folder`
- `delete_folder`
- `get_folder_by_id`
- `get_folder_by_name`
- `get_folder_content`
- `update_folder`

## Formularios

[Referencia de API de Forms](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms){target="_blank"}

- `add_field_set`
- `add_field_to_form`
- `add_field_visibility_rule`
- `add_rich_text_field`
- `approve_form`
- `browse_forms`
- `clone_form`
- `create_form`
- `delete_field_from_fieldset`
- `delete_form`
- `delete_form_field`
- `discard_form_draft`
- `get_form_by_id`
- `get_form_by_name`
- `get_form_field_metadata`
- `get_form_fields`
- `get_forms_used_by`
- `get_program_member_fields`
- `get_thank_you_page`
- `set_field_autofill`
- `update_field_positions`
- `update_form`
- `update_form_field`

## Clientes potenciales

[Referencia de API de posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads){target="_blank"}

- `add_leads_to_list`
- `describe_lead`
- `get_activity_types`
- `get_lead_activities`
- `get_leads_by_filter`
- `get_leads_by_smart_list`
- `get_paging_token`

## Programas

[Referencia de API de programas](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs){target="_blank"}

- `approve_program`
- `browse_email_batch_programs`
- `browse_nurture_programs`
- `browse_program_details`
- `browse_program_events`
- `browse_programs`
- `browse_scheduled_programs`
- `clone_program`
- `create_program`
- `delete_program_tag`
- `get_program_by_id`
- `get_program_by_name`
- `get_program_creation_options`
- `get_program_smart_list`
- `get_programs_by_tag`
- `unapprove_program`
- `update_program`
- `update_program_tag`

## Campañas inteligentes

[Referencia de API de campañas inteligentes](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns){target="_blank"}

- `activate_smart_campaign`
- `add_flow_step`
- `browse_smart_campaigns`
- `create_smart_campaign`
- `facet_smart_campaigns`
- `get_smart_campaign_auto_suggest`
- `get_smart_campaign_by_id`
- `get_smart_campaign_by_name`
- `get_smart_campaign_flow_step_by_name`
- `get_smart_campaign_flow_step_type_by_name`
- `get_smart_campaign_flow_step_types`
- `get_smart_campaign_flow_steps`
- `get_smart_campaign_rule_by_name`
- `get_smart_campaign_rules`
- `get_smart_campaign_scheduled_runs`
- `get_smart_campaign_used_by`
- `get_smart_list_by_campaign_id`
- `schedule_campaign`
- `trigger_campaign`
- `update_flow_step_choice`
- `update_smart_campaign`

## Listas inteligentes

[Referencia de API de listas inteligentes](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Lists){target="_blank"}

- `add_smart_list_rule`
- `browse_smart_lists`
- `clone_smart_list`
- `create_smart_list`
- `delete_all_smart_list_rules`
- `get_smart_list_auto_suggest`
- `get_smart_list_by_id`
- `get_smart_list_by_name`
- `get_smart_list_rule_by_name`
- `get_smart_list_rules`
- `get_smart_list_used_by`
- `remove_smart_list_rule_constraint`
- `reorder_smart_list_rules`
- `update_smart_list_filter_logic`
- `update_smart_list_rule`

## Fragmentos

[Referencia de API de fragmentos](https://developer.adobe.com/marketo-apis/api/asset#tag/Snippets){target="_blank"}

- `approve_snippet`
- `browse_snippets`
- `clone_snippet`
- `create_snippet`
- `delete_snippet`
- `discard_snippet_draft`
- `facet_snippets`
- `get_snippet_by_id`
- `get_snippet_content`
- `get_snippet_dynamic_content`
- `unapprove_snippet`
- `update_snippet`
- `update_snippet_content`
- `update_snippet_dynamic_content`

## Listas estáticas

[Referencia de API de listas estáticas](https://developer.adobe.com/marketo-apis/api/mapi#tag/Static-Lists){target="_blank"}

- `browse_lists`
- `create_list`
- `get_list_by_id`
- `get_list_by_name`
- `get_list_members`
- `remove_from_list`
- `update_list`

## Tókenes

[Referencia de API de tokens](https://developer.adobe.com/marketo-apis/api/asset#tag/Tokens){target="_blank"}

- `create_calendar_token`
- `create_token`
- `delete_token`
- `get_calendar_tokens`
- `get_tokens_by_folder`

## Herramientas de pasos de flujo MCP habilitadas

<table style="table-layout:auto">
<tr>
<th>Pasos de flujo</th>
<th>Activadores</th>
<th>Filtros (actividad)</th>
<th>Filtros (atributo)</th>
</tr>
<tr>
<td valign="top"><ul><li>Añadir a conjunto de campos</li><li>Añadir a la lista</li><li>Agregar a la campaña de Microsoft</li><li>Añadir a Nutrir</li><li>Agregar a campaña de SFDC</li><li>Llamar a un Webhook</li><li>Cambiar valor de datos</li><li>Cambiar partición de posibles clientes</li><li>Cambiar la cadencia de nutrición</li><li>Cambiar seguimiento de nutrición</li><li>Cambiar propietario</li><li>Cambiar propietario en Microsoft</li><li>Cambiar datos del programa</li><li>Cambiar datos del miembro del programa</li><li>Cambiar etapa de ingresos</li><li>Cambiar calificación</li><li>Cambiar segmento</li><li>Cambio de estado en progreso</li><li>Cambiar estado de la campaña SFDC</li><li>Convertir posible cliente</li><li>Crear tarea</li><li>Crear tarea en Microsoft</li><li>Eliminar lead</li><li>Eliminar posible cliente de Microsoft</li><li>Eliminar lead de SFDC</li><li>Ejecutar campaña</li><li>Momento interesante</li><li>Eliminar del conjunto de campos</li><li>Quitar del flujo</li><li>Quitar de la lista</li><li>Quitar de la campaña de Microsoft</li><li>Quitar de la campaña de SFDC</li><li>Solicitar campaña</li><li>Enviar alerta</li><li>Enviar correo electrónico</li><li>Sincronizar posible cliente con Microsoft</li><li>Sincronizar lead con SFDC</li><li>Espera</li></ul></td>
<td valign="top"><ul><li>La actividad está registrada</li><li>La actividad está actualizada</li><li>Se agregó a Lista</li><li>Añadido a Microsoft Campaign</li><li>Añadido a Nutrir</li><li>Se agregó a Oportunidad</li><li>Añadido a oportunidad (cuenta)</li><li>Añadido a oportunidad (contacto)</li><li>Se agregó a Campaña de SFDC</li><li>Hace preguntas durante el evento</li><li>Asiste al evento</li><li>Se solicita una campaña</li><li>Hace clic en el vínculo</li><li>Hace clic en el vínculo del email</li><li>Hace clic en el vínculo del email de ventas</li><li>Vínculo de clics en el mensaje SMS</li><li>Clics en un vínculo</li><li>Cambios en el valor de los datos</li><li>Descarga un recurso</li><li>El email se rechaza</li><li>El email se rechaza temporalmente</li><li>El email se entregó</li><li>Interactúa con un flujo de conversación</li><li>Interacción con un cuadro de diálogo</li><li>Interactúa con un agente en el flujo de conversación</li><li>Interactúa con un agente en el cuadro de diálogo</li><li>Rellena el formulario</li><li>Tiene un momento interesante</li><li>Interactúa con el documento en el flujo de conversación</li><li>Interactúa con el documento en el cuadro de diálogo</li><li>Es email de ventas enviado</li><li>Posible cliente convertido</li><li>El posible cliente se ha creado</li><li>El posible cliente se ha eliminado de Microsoft</li><li>El posible cliente se ha eliminado de SFDC</li><li>El posible cliente se envía a Marketo</li><li>El posible cliente se sincroniza con Microsoft</li><li>El posible cliente se sincroniza con SFDC</li><li>Cambios de partición de cliente potencial</li><li>Cambio de etapa manual</li><li>Nutrir los cambios de cadencia</li><li>Nutrir los cambios de seguimiento</li><li>Abre el email</li><li>Abre el email de ventas</li><li>Se ha actualizado la oportunidad (cuenta)</li><li>Se ha actualizado la oportunidad (contacto)</li><li>La oportunidad está actualizada</li><li>Cambios del propietario</li><li>Cambios de propietario en Microsoft</li><li>Se han cambiado los datos de miembros del programa</li><li>El estado de progresión ha cambiado</li><li>Alcanza la meta de diálogo</li><li>Alcanza la meta en el flujo de conversación</li><li>Se recibió un email de Enviar a un amigo</li><li>Se quitó de Lista</li><li>Eliminado de la campaña de Microsoft</li><li>Se quitó de Oportunidad</li><li>Eliminado de oportunidad (cuenta)</li><li>Eliminado de la oportunidad (contacto)</li><li>Se quitó de Campaña de SFDC</li><li>Respuestas al correo electrónico de ventas</li><li>Responde a una encuesta</li><li>Responde a una encuesta</li><li>Se cambia la etapa de ingresos</li><li>El email de ventas se rechaza</li><li>El email de ventas está recibido</li><li>Programa la reunión en el flujo de conversación</li><li>Programa la reunión en el cuadro de diálogo</li><li>Se cambia el puntaje</li><li>Se cambia el segmento</li><li>Alerta enviada</li><li>Email de Enviar a un amigo enviado</li><li>Devoluciones de mensajes SMS</li><li>Se entrega el mensaje SMS</li><li>El estado está cambiado en la campaña de SFDC</li><li>Se cancela la suscripción a email</li><li>Visita la página web</li><li>Se llamó a un Webhook</li></ul></td>
<td valign="top"><ul><li>Se registró la actividad</li><li>Se actualizó la actividad</li><li>Se envió una alerta</li><li>Se ejecutó la campaña</li><li>Se solicitó una campaña</li><li>Haga clic en Vínculo</li><li>Hizo clic en el vínculo del email</li><li>Hizo clic en el vínculo del email de ventas</li><li>Vínculo en el que se hizo clic en el mensaje SMS</li><li>Se ha hecho clic en un vínculo</li><li>Se cambió el valor de los datos</li><li>Se ha descargado un recurso</li><li>Se rechazó el email</li><li>Se rechazó temporalmente el email</li><li>Se ha participado con un Flujo de conversación</li><li>Participó en un diálogo</li><li>Interacción con un agente en el flujo de conversación</li><li>Interactuó con un agente en el diálogo</li><li>Completó el formulario</li><li>Tuvo un momento interesante</li><li>Ha formulado preguntas durante el evento</li><li>Ha asistido a un evento</li><li>Interactuó con el documento en el flujo de conversación</li><li>Interactuó con un documento en el diálogo</li><li>Partición de cliente potencial cambiada</li><li>El posible cliente se ha convertido</li><li>Se ha creado el posible cliente</li><li>El posible cliente se ha eliminado de Microsoft</li><li>El posible cliente se ha eliminado de SFDC</li><li>El posible cliente se envió a Marketo</li><li>El posible cliente se ha sincronizado con Microsoft</li><li>El posible cliente se ha sincronizado con SFDC</li><li>Cadencia de nutrición cambiada</li><li>Nutrir la pista cambiada</li><li>Abrió el correo electrónico</li><li>Abrió el email de ventas</li><li>Se ha actualizado la oportunidad (cuenta)</li><li>Se ha actualizado la oportunidad (contacto)</li><li>Se actualizó la oportunidad</li><li>Se cambió el propietario</li><li>Se ha cambiado el propietario en Microsoft</li><li>Se cambiaron los datos de los miembros del programa</li><li>Se ha cambiado el estado de progresión</li><li>Alcanzó el objetivo del diálogo</li><li>Meta alcanzada en el flujo de conversación</li><li>Se recibió un email de Enviar a un amigo</li><li>Respondió al email de ventas</li><li>Respondido a una encuesta</li><li>Respondió a una encuesta</li><li>Se cambió la etapa de ingresos</li><li>Se rechazó el email de ventas</li><li>El email de ventas fue recibido</li><li>Reunión programada en el flujo de conversación</li><li>Programó una reunión en el diálogo</li><li>Se cambió el puntaje</li><li>Se cambió el segmento</li><li>Email de Enviar a un amigo enviado</li><li>Mensaje SMS rechazado</li><li>Se canceló la suscripción a correos electrónicos</li><li>Visitó la página web</li><li>Se agregó a Lista</li><li>Se agregó a Nutrir</li><li>Se agregó a Oportunidad</li><li>Se agregó a oportunidad (cuenta)</li><li>Se agregó a oportunidad (contacto)</li><li>Se le entregó el correo electrónico</li><li>Se envió el mensaje SMS</li><li>Se quitó de Lista</li><li>Se quitó de Oportunidad</li><li>Se eliminó de la oportunidad (cuenta)</li><li>Se eliminó de la oportunidad (contacto)</li><li>Se envió correo electrónico</li><li>Se envió el email de ventas</li><li>Se llamó a un Webhook</li></ul></td>
<td valign="top"><ul><li>Email del propietario de la cuenta</li><li>Nombre del propietario de la cuenta</li><li>Apellido del propietario de la cuenta</li><li>Fecha de adquisición</li><li>Programa de adquisición</li><li>Nombre del programa de adquisición</li><li>Dirección</li><li>Ingresos anuales</li><li>IP anónima</li><li>Dirección de facturación</li><li>Ciudad de facturación</li><li>País de facturación</li><li>Código postal de facturación</li><li>Estado de facturación</li><li>En la lista de bloqueados</li><li>Ciudad</li><li>Tipo de compañía en Microsoft</li><li>Nombre de la empresa</li><li>País</li><li>Creado en</li><li>Fecha de nacimiento</li><li>Departamento</li><li>No llamar</li><li>Razón por la que no se debe llamar</li><li>Campos duplicados</li><li>Dirección de email</li><li>Correo electrónico no válido</li><li>Causa de email no válido</li><li>Correo electrónico suspendido</li><li>Email suspendido en</li><li>Causa de suspensión de email</li><li>Número de fax</li><li>Nombre</li><li>Nombre completo</li><li>Tiene oportunidad</li><li>Industria</li><li>Ciudad inferida</li><li>Compañía inferida</li><li>País inferido</li><li>Área metropolitana inferida</li><li>Código de área telefónico inferido</li><li>Código postal inferido</li><li>Región del estado inferida</li><li>Es cliente</li><li>Es socio</li><li>Cargo</li><li>Apellido</li><li>Email del propietario del lead</li><li>Nombre del propietario del posible cliente</li><li>Cargo del propietario del lead</li><li>Apellido del propietario del posible cliente</li><li>Número de teléfono del propietario del posible cliente</li><li>Nombre de partición de cliente potencial</li><li>Calificación de lead</li><li>Puntaje del lead</li><li>Origen del lead</li><li>Estado del lead</li><li>Teléfono principal:</li><li>Marketing suspendido</li><li>Miembro del conjunto de campos</li><li>Miembro de la lista</li><li>Miembro de Nutrición</li><li>Abonado del programa</li><li>Abonado del modelo de ingresos</li><li>Miembro de la fase de ingresos</li><li>Miembro de la campaña SFDC</li><li>Abonado de la campaña inteligente</li><li>Abonado de la lista inteligente</li><li>Número de cuenta de Microsoft</li><li>Microsoft - Fecha de creación</li><li>Microsoft - Eliminado</li><li>Microsoft - Tipo</li><li>Segundo nombre</li><li>Número de teléfono móvil</li><li>Notas</li><li>Cantidad de empleados</li><li>Cantidad de oportunidades</li><li>Remitente original</li><li>Motor de búsqueda original</li><li>Frase de búsqueda original</li><li>Información de origen original</li><li>Tipo de origen original</li><li>Nombre de la empresa matriz</li><li>Zona horaria de la persona</li><li>Número de teléfono</li><li>Código postal</li><li>Muestra aleatoria</li><li>Información de origen del registro</li><li>Tipo de origen del registro</li><li>Función</li><li>Saludo</li><li>Número de cuenta de SFDC</li><li>Fecha de creación de SFDC</li><li>SFDC está eliminado</li><li>Tipo de SFDC</li><li>Código SIC</li><li>Sitio</li><li>Estado</li><li>Monto total de la oportunidad</li><li>Ingreso esperado total de la oportunidad</li><li>Suscripción cancelada</li><li>Razón de la cancelación de la suscripción</li><li>Actualizado en</li><li>Sitio web</li></ul></td>
</tr>
</table>
