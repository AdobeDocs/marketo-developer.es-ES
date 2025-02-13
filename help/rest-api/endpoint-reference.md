---
title: Referencia de extremo
feature: REST API
description: Referencias de extremo de API de Marketo
exl-id: 27d16b6f-865a-4e40-ab9c-cbabe2927472
source-git-commit: 3632d2b713d97a2c895c65f144c07e62e1d369cb
workflow-type: tm+mt
source-wordcount: '4676'
ht-degree: 26%

---

# Referencia de extremo

A continuación se muestran vínculos a las referencias de la API de REST de Marketo.

- [Recurso](https://developer.adobe.com/marketo-apis/api/asset/)
- [Identidad](https://developer.adobe.com/marketo-apis/api/identity/)
- [Base de datos de clientes potenciales](https://developer.adobe.com/marketo-apis/api/mapi/)
- [Administración de usuarios](https://developer.adobe.com/marketo-apis/api/user/)

## Lista de extremos {#endpoint_list}

Esta es una lista completa de puntos finales de API de REST.

| Nombre | Grupo | Método | URI | Permiso requerido |
|---|---|---|---|---|
| Añadir actividades personalizadas | Actividades | POST | /rest/v1/activities/external.json | Actividad de solo escritura |
| Aprobar tipo de actividad personalizada | Actividades | POST | /rest/v1/activities/external/type/{apiName}/approve.json | Metadatos de la actividad de escritura y lectura |
| Crear atributos personalizados de tipo de actividad | Actividades | POST | /rest/v1/activities/external/type/{apiName}/attributes/create.json | Metadatos de la actividad de escritura y lectura |
| Crear tipos de actividades personalizados | Actividades | POST | /rest/v1/activities/external/type.json | Metadatos de la actividad de escritura y lectura |
| Eliminar tipo de actividad personalizado | Actividades | POST | /rest/v1/activities/external/type/{apiName}/delete.json | Metadatos de la actividad de escritura y lectura |
| Eliminar atributos personalizados de tipo de actividad | Actividades | POST | /rest/v1/activities/external/type/{apiName}/attributes/delete.json | Metadatos de la actividad de escritura y lectura |
| Describir tipo de actividad personalizada | Actividades | GET | /rest/v1/activities/external/type/{apiName}/describe.json | Metadatos de la actividad de solo lectura |
| Descartar borrador de tipo de actividad personalizada | Actividades | POST | /rest/v1/activities/external/type/{apiName}/discardDraft.json | Metadatos de la actividad de escritura y lectura |
| Obtener tipos de actividades | Actividades | GET | /rest/v1/activities/types.json | Actividad de solo lectura |
| Obtener tipos de actividades personalizados | Actividades | GET | /rest/v1/activities/external/types.json | Metadatos de la actividad de solo lectura |
| Obtener posibles clientes eliminados | Actividades | GET | /rest/v1/activities/deletedleads.json | Actividad de solo lectura |
| Obtener actividades de posibles clientes | Actividades | GET | /rest/v1/activities.json | Actividad de solo lectura |
| Obtener cambios de posibles clientes | Actividades | GET | /rest/v1/activities/leadchanges.json | Actividad de solo lectura |
| Obtener token de paginación | Actividades | GET | /rest/v1/activities/pagingtoken.json | Actividad de solo lectura |
| Actualizar tipo de actividad personalizada | Actividades | POST | /rest/v1/activities/external/type/{apiName}.json | Metadatos de la actividad de escritura y lectura |
| Actualizar atributos personalizados de tipo de actividad | Actividades | POST | /rest/v1/activities/external/type/{apiName}/attributes/update.json | Metadatos de la actividad de escritura y lectura |
| Identidad | Autenticación | GET o POST | /identity/oauth/token | Ninguna |
| Cancelar trabajo de actividad de exportación | Actividades de exportación masiva | POST | /bulk/v1/activities/export/{exportid}/cancel.json | Actividad de solo lectura |
| Crear trabajo de actividad de exportación | Actividades de exportación masiva | POST | /bulk/v1/activities/export/create.json | Actividad de solo lectura |
| Poner trabajo de actividad de exportación | Actividades de exportación masiva | POST | /bulk/v1/activities/export/{exportid}/enqueue.json | Actividad de solo lectura |
| Obtener archivo de actividad de exportación | Actividades de exportación masiva | GET | /bulk/v1/activities/export/{exportid}/file.json | Actividad de solo lectura |
| Obtener estado del trabajo de actividad de exportación | Actividades de exportación masiva | GET | /bulk/v1/activities/export/{exportid}/status.json | Actividad de solo lectura |
| Obtener trabajos de actividad de exportación | Actividades de exportación masiva | GET | /bulk/v1/activities/export.json | Actividad de solo lectura |
| Cancelar trabajo de exportación de objeto personalizado | Exportación masiva de objetos personalizados | POST | /bulk/v1/customobjects/export/{exportid}/cancel.json | Objeto personalizado de solo lectura |
| Crear trabajo de exportación de objeto personalizado | Exportación masiva de objetos personalizados | POST | /bulk/v1/customobjects/export/create.json | Objeto personalizado de solo lectura |
| Trabajo de exportar objetos personalizados en cola | Exportación masiva de objetos personalizados | POST | /bulk/v1/customobjects/export/{exportid}/enqueue.json | Objeto personalizado de solo lectura |
| Obtener archivo de objeto personalizado de exportación | Exportación masiva de objetos personalizados | GET | /bulk/v1/customobjects/export/{exportid}/file.json | Objeto personalizado de solo lectura |
| Obtener estado del trabajo de exportación de objetos personalizados | Exportación masiva de objetos personalizados | GET | /bulk/v1/customobjects/export/{exportid}/status.json | Objeto personalizado de solo lectura |
| Obtener trabajos de exportación de objeto personalizado | Exportación masiva de objetos personalizados | GET | /bulk/v1/customobjects/export.json | Objeto personalizado de solo lectura |
| Cancelar trabajo de posible cliente de exportación | Posibles clientes de exportación masiva | POST | /bulk/v1/leads/export/{exportid}/cancel.json | Guía de solo lectura |
| Crear trabajo de posible cliente de exportación | Posibles clientes de exportación masiva | POST | /bulk/v1/leads/export/create.json | Guía de solo lectura |
| Poner trabajo de posible cliente de exportación | Posibles clientes de exportación masiva | POST | /bulk/v1/leads/export/{exportid}/enqueue.json | Guía de solo lectura |
| Obtener archivo de posible cliente de exportación | Posibles clientes de exportación masiva | GET | /bulk/v1/leads/export/{exportid}/file.json | Guía de solo lectura |
| Obtener estado de trabajo de cliente potencial | Posibles clientes de exportación masiva | GET | /bulk/v1/leads/export/{exportid}/status.json | Guía de solo lectura |
| Obtener trabajos de posible cliente de exportación | Posibles clientes de exportación masiva | GET | /bulk/v1/leads/export.json | Guía de solo lectura |
| Cancelar trabajo de miembro del programa de exportación | Miembros del programa de exportación masiva | POST | /bulk/v1/program/members/export/{exportid}/cancel.json | Guía de solo lectura |
| Crear trabajo de miembro del programa de exportación | Miembros del programa de exportación masiva | POST | /bulk/v1/program/members/export/create.json | Guía de solo lectura |
| Trabajo de miembro del programa de exportación en cola | Miembros del programa de exportación masiva | POST | /bulk/v1/program/members/export/{exportid}/enqueue.json | Guía de solo lectura |
| Obtener archivo de miembro del programa de exportación | Miembros del programa de exportación masiva | GET | /bulk/v1/program/members/export/{exportid}/file.json | Guía de solo lectura |
| Obtener estado del trabajo del miembro del programa de exportación | Miembros del programa de exportación masiva | GET | /bulk/v1/program/members/export/{exportid}/status.json | Guía de solo lectura |
| Obtener trabajos de miembros del programa de exportación | Miembros del programa de exportación masiva | GET | /bulk/v1/program/members/export.json | Guía de solo lectura |
| Errores al obtener objeto personalizado de importación | Importación masiva de objetos personalizados | GET | /bulk/v1/customobjects/import/{id}/failures.json | Objeto personalizado habilitado para lectura y escritura |
| Obtener estado de objeto personalizado de importación | Importación masiva de objetos personalizados | GET | /bulk/v1/customobjects/import/{id}/status.json | Objeto personalizado habilitado para lectura y escritura |
| Obtener advertencias de importación de objeto personalizado | Importación masiva de objetos personalizados | GET | /bulk/v1/customobjects/import/{id}/warnings.json | Objeto personalizado habilitado para lectura y escritura |
| Importar objetos personalizados | Importación masiva de objetos personalizados | POST | /bulk/v1/customobjects/{apiName}/import.json | Objeto personalizado habilitado para lectura y escritura |
| Obtener errores de importación de posibles clientes | Importación masiva de posibles clientes | GET | /bulk/v1/leads/batch/{id}/failures.json | Guía de solo escritura |
| Obtener estado de cliente potencial de importación | Importación masiva de posibles clientes | GET | /bulk/v1/leads/batch/{id}.json | Guía de solo escritura |
| Obtener advertencias de importación de posibles clientes | Importación masiva de posibles clientes | GET | /bulk/v1/leads/batch/{id}/warnings.json | Guía de solo escritura |
| Importar posibles clientes | Importación masiva de posibles clientes | POST | /bulk/v1/leads.json | Guía de solo escritura |
| Obtener errores de miembros del programa de importación | Miembros del programa de importación masiva | GET | /bulk/v1/program/members/import/{id}/failures.json | Guía de solo escritura |
| Obtener estado de miembro del programa de importación | Miembros del programa de importación masiva | GET | /bulk/v1/program/members/import/{id}/status.json | Guía de solo escritura |
| Obtener advertencias de miembros del programa de importación | Miembros del programa de importación masiva | GET | /bulk/v1/program/members/import/{id}/warnings.json | Guía de solo escritura |
| Importar miembros del programa | Miembros del programa de importación masiva | POST | /bulk/v1/program/{programId}/members/import.json | Guía de solo escritura |
| Obtener campaña por ID | Campañas | GET | /rest/v1/campaigns/{id}.json | Campañas de solo lectura |
| Obtener campañas | Campañas | GET | /rest/v1/campaigns.json | Campañas de solo lectura |
| Solicitar campaña | Campañas | POST | /rest/v1/campaigns/{id}/trigger.json | Campañas de lectura-escritura |
| Programar campaña | Campañas | POST | /rest/v1/campaigns/{id}/schedule.json | Campañas de lectura-escritura |
| Obtener canal por nombre | Canales | GET | /rest/asset/v1/channel/byName.json | Recurso de solo lectura |
| Obtener canales | Canales | GET | /rest/asset/v1/channels.json | Recurso de solo lectura |
| Eliminar compañías | Compañías | POST | /rest/v1/companies/delete.json | Compañía habilitada para lectura y escritura |
| Describir compañías | Compañías | GET | /rest/v1/companies/describe.json | Compañía de solo lectura |
| Obtener compañías | Compañías | GET | /rest/v1/companies.json | Compañía de solo lectura |
| Sincronizar compañías | Compañías | POST | /rest/v1/companies.json | Compañía habilitada para lectura y escritura |
| Obtener campo de empresa por nombre | Compañías | GET | /rest/v1/companies/schema/fields/{fieldApiName}.json | Campo personalizado de esquema de Lectura-Escritura |
| Obtener campos de empresa | Compañías | GET | /rest/v1/companies/schema/fields.json | Campo personalizado de esquema de Lectura-Escritura |
| Agregar campos de tipo de objeto personalizados | Objetos personalizados | POST | /rest/v1/customobjects/schema/{apiName}/addField.json | Tipo de objeto personalizado de lectura y escritura |
| Aprobar tipo de objeto personalizado | Objetos personalizados | POST | /rest/v1/customobjects/schema/{apiName}/approve.json | Tipo de objeto personalizado de lectura y escritura |
| Eliminar objetos personalizados | Objetos personalizados | POST | /rest/v1/customobjects/{name}/delete.json | Objeto personalizado habilitado para lectura y escritura |
| Eliminar tipo de objeto personalizado | Objetos personalizados | POST | /rest/v1/customobjects/schema/{apiName}/delete.json | Tipo de objeto personalizado de lectura y escritura |
| Eliminar campos de tipo de objeto personalizado | Objetos personalizados | POST | /rest/v1/customobjects/schema/{apiName}/deleteField.json | Tipo de objeto personalizado de lectura y escritura |
| Describir objetos personalizados | Objetos personalizados | GET | /rest/v1/customobjects/{name}/describe.json | Objeto personalizado de solo lectura |
| Describir tipo de objeto personalizado | Objetos personalizados | GET | /rest/v1/customobjects/schema/{apiName}/describe.json | Tipo de objeto personalizado de solo lectura |
| Descartar borrador de tipo de objeto personalizado | Objetos personalizados | POST | /rest/v1/customobjects/schema/{apiName}/discardDraft.json | Tipo de objeto personalizado de lectura y escritura |
| Obtener objetos personalizados | Objetos personalizados | GET | /rest/v1/customobjects/{name}.json | Objeto personalizado de solo lectura |
| Obtener objetos enlazables de objetos personalizados | Objetos personalizados | GET | /rest/v1/customobjects/schema/linkableObjects.json | Tipo de objeto personalizado de solo lectura |
| Obtener Assets dependiente del objeto personalizado | Objetos personalizados | GET | /rest/v1/customobjects/schema/{apiName}/dependentAssets.json | Tipo de objeto personalizado de solo lectura |
| Obtener tipos de datos del campo Tipo de objeto personalizado | Objetos personalizados | GET | /rest/v1/customobjects/schema/fieldDataTypes.json | Tipo de objeto personalizado de solo lectura |
| Lista de objetos personalizados | Objetos personalizados | GET | /rest/v1/customobjects.json | Objeto personalizado de solo lectura |
| Lista de tipos de objetos personalizados | Objetos personalizados | GET | /rest/v1/customobjects/schema.json | Tipo de objeto personalizado de solo lectura |
| Sincronizar objetos personalizados | Objetos personalizados | POST | /rest/v1/customobjects/{name}.json | Objeto personalizado habilitado para lectura y escritura |
| Sincronizar tipo de objeto personalizado | Objetos personalizados | POST | /rest/v1/customobjects/schema.json | Tipo de objeto personalizado de lectura y escritura |
| Actualizar campo de tipo de objeto personalizado | Objetos personalizados | POST | /rest/v1/customobjects/schema/{apiName}/updateField.json | Tipo de objeto personalizado de lectura y escritura |
| Aprobar borrador de plantilla de correo electrónico | Plantillas de email | POST | /rest/asset/v1/emailTemplate/{id}/approveDraft.json | Recurso de lectura-escritura |
| Clonar plantilla de email | Plantillas de email | POST | /rest/asset/v1/emailTemplate/{id}/clone.json | Recurso de lectura-escritura |
| Crear plantilla de correo electrónico | Plantillas de email | POST | /rest/asset/v1/emailTemplates.json | Recurso de lectura-escritura |
| Eliminar plantilla de email | Plantillas de email | POST | /rest/asset/v1/emailTemplate/{id}/delete.json | Recurso de lectura-escritura |
| Descartar borrador de plantilla de correo electrónico | Plantillas de email | POST | /rest/asset/v1/emailTemplate/{id}/discardDraft.json | Recurso de lectura-escritura |
| Obtener plantilla de correo electrónico por identificador | Plantillas de email | GET | /rest/asset/v1/emailTemplate/{id}.json | Recurso de solo lectura |
| Obtener plantilla de correo electrónico por nombre | Plantillas de email | GET | /rest/asset/v1/emailTemplate/byName.json | Recurso de solo lectura |
| Obtener contenido de plantilla de correo electrónico por ID | Plantillas de email | GET | /rest/asset/v1/emailTemplate/{id}/content.json | Recurso de solo lectura |
| Obtener plantilla de correo electrónico utilizada por | Plantillas de email | GET | /rest/asset/v1/emailTemplates/{id}/usedBy.json | Recurso de solo lectura |
| Obtener plantillas de correo electrónico | Plantillas de email | GET | /rest/asset/v1/emailTemplates.json | Recurso de solo lectura |
| Desaprobar borrador de plantilla de correo electrónico | Plantillas de email | POST | /rest/asset/v1/emailTemplate/{id}/unapprove.json | Recurso de lectura-escritura |
| Actualizar contenido de plantilla de correo electrónico | Plantillas de email | POST | /rest/asset/v1/emailTemplate/{id}/content.json | Recurso de lectura-escritura |
| Actualizar metadatos de plantilla de correo electrónico | Plantillas de email | POST | /rest/asset/v1/emailTemplate/{id}.json | Recurso de lectura-escritura |
| Añadir módulo de correo electrónico | Correos electrónicos | POST | /rest/asset/v1/email/{id}/content/{moduleId}/add.json | Recurso de lectura-escritura |
| Aprobar borrador de correo electrónico | Correos electrónicos | POST | /rest/asset/v1/email/{id}/approveDraft.json | Recurso de lectura-escritura |
| Clonar Email | Correos electrónicos | POST | /rest/asset/v1/email/{id}/clone.json | Recurso de lectura-escritura |
| Crear correo electrónico | Correos electrónicos | POST | /rest/asset/v1/emails.json | Recurso de lectura-escritura |
| Eliminar email | Correos electrónicos | POST | /rest/asset/v1/email/{id}/delete.json | Recurso de lectura-escritura |
| Eliminar módulo | Correos electrónicos | POST | /rest/asset/v1/email/{id}/content/{moduleId}/delete.json | Recurso de lectura-escritura |
| Descartar borrador de correo electrónico | Correos electrónicos | POST | /rest/asset/v1/email/{id}/discardDraft.json | Recurso de lectura-escritura |
| Duplicar módulo de correo electrónico | Correos electrónicos | POST | /rest/asset/v1/email/{id}/content/{moduleId}/duplicate.json | Recurso de lectura-escritura |
| Recibir correo electrónico por identificador | Correos electrónicos | GET | /rest/asset/v1/email/{id}.json | Recurso de solo lectura |
| Obtener correo electrónico por nombre | Correos electrónicos | GET | /rest/asset/v1/email/byName.json | Recurso de solo lectura |
| Obtener contenido del correo electrónico | Correos electrónicos | GET | /rest/asset/v1/email/{id}/content.json | Recurso de solo lectura |
| Obtener contenido dinámico del correo electrónico | Correos electrónicos | GET | /rest/asset/v1/email/{id}/dynamicContent/{dynamicContentId}.json | Recurso de solo lectura |
| Obtener contenido completo del correo electrónico | Correos electrónicos | GET | /rest/asset/v1/email/{id}/fullContent.json | Recurso de solo lectura |
| Obtener variables de correo electrónico | Correos electrónicos | GET | /rest/asset/v1/email/{id}/variables.json | Recurso de solo lectura |
| Obtener campos CC de correo electrónico | Correos electrónicos | GET | /rest/asset/v1/email/ccFields.json | Recurso de solo lectura |
| Obtener correos electrónicos | Correos electrónicos | GET | /rest/asset/v1/emails.json | Recurso de solo lectura |
| Reorganizar módulos de correo electrónico | Correos electrónicos | POST | /rest/asset/v1/email/{id}/content/rearrange.json | Recurso de lectura-escritura |
| Cambiar nombre de módulo de correo electrónico | Correos electrónicos | POST | /rest/asset/v1/email/{id}/content/{moduleId}/rename.json | Recurso de lectura-escritura |
| Enviar muestra de email | Correos electrónicos | POST | /rest/asset/v1/email/{id}/sendSample.json | Recurso de lectura-escritura |
| Desaprobar correo electrónico | Correos electrónicos | POST | /rest/asset/v1/email/{id}/unapprove.json | Recurso de lectura-escritura |
| Actualizar contenido del correo electrónico | Correos electrónicos | POST | /rest/asset/v1/email/{id}/content.json | Recurso de lectura-escritura |
| Actualizar sección de contenido de correo electrónico | Correos electrónicos | POST | /rest/asset/v1/email/{id}/content/{htmlId}.json | Recurso de lectura-escritura |
| Actualizar sección de contenido dinámico de correo electrónico | Correos electrónicos | POST | /rest/asset/v1/email/{id}/dynamicContent/{dynamicContentId}.json | Recurso de lectura-escritura |
| Actualizar contenido completo del correo electrónico | Correos electrónicos | POST | /rest/asset/v1/emails/{id}/fullContent.json | Recurso de lectura-escritura |
| Actualizar metadatos de correo electrónico | Correos electrónicos | POST | /rest/asset/v1/email/{id}.json | Recurso de lectura-escritura |
| Actualizar variable de correo electrónico | Correos electrónicos | POST | /rest/asset/v1/email/{id}/variable/{name}.json | Recurso de lectura-escritura |
| Crear archivo | Archivos | POST | /rest/asset/v1/files.json | Recurso de lectura-escritura |
| Obtener archivo por identificador | Archivos | GET | /rest/asset/v1/file/{id}.json | Recurso de solo lectura |
| Obtener archivo por nombre | Archivos | GET | /rest/asset/v1/file/byName.json | Recurso de solo lectura |
| Obtener archivos | Archivos | GET | /rest/asset/v1/files.json | Recurso de solo lectura |
| Actualizar contenido de archivo | Archivos | POST | /rest/asset/v1/file/{id}/content.json | Recurso de lectura-escritura |
| Crear carpeta | Carpetas | POST | /rest/asset/v1/folders.json | Recurso de lectura-escritura |
| Eliminar carpeta | Carpetas | POST | /rest/asset/v1/folder/{id}/delete.json | Recurso de lectura-escritura |
| Obtener carpeta por identificador | Carpetas | GET | /rest/asset/v1/folder/{id}.json | Recurso de solo lectura |
| Obtener carpeta por nombre | Carpetas | GET | /rest/asset/v1/folder/byName.json | Recurso de solo lectura |
| Obtener contenido de la carpeta | Carpetas | GET | /rest/asset/v1/folder/{id}/content.json | Recurso de solo lectura |
| Obtener carpetas | Carpetas | GET | /rest/asset/v1/folders.json | Recurso de solo lectura |
| Actualizar metadatos de carpeta | Carpetas | POST | /rest/asset/v1/folder/{id}.json | Recurso de lectura-escritura |
| Agregar campo al formulario | Campos del formulario | POST | /rest/asset/v1/form/{id}/fields.json | Recurso de lectura-escritura |
| Agregar conjunto de campos al formulario | Campos del formulario | POST | /rest/asset/v1/form/{id}/fieldSet.json | Recurso de lectura-escritura |
| Agregar reglas de visibilidad de campo de formulario | Campos del formulario | POST | /rest/asset/v1/form/{formId}/field/{fieldId}/visibility.json | Recurso de lectura-escritura |
| Agregar campo de texto enriquecido | Campos del formulario | POST | /rest/asset/v1/form/{id}/richText.json | Recurso de lectura-escritura |
| Eliminar campo del conjunto de campos | Campos del formulario | POST | /rest/asset/v1/form/{id}/fieldSet/{fieldSetId}/field/{fieldId}/delete.json | Recurso de lectura-escritura |
| Eliminar campo de formulario | Campos del formulario | POST | /rest/asset/v1/form/{id}/field/{fieldId}/delete.json | Recurso de lectura-escritura |
| Obtener campos de formulario disponibles | Campos del formulario | GET | /rest/asset/v1/form/fields.json | Recurso de solo lectura |
| Obtener formulario disponible Campos de miembros del programa | Campos del formulario | GET | /rest/asset/v1/form/programMemberFields.json | Recurso de solo lectura |
| Obtener campos para el formulario | Campos del formulario | GET | /rest/asset/v1/form/{id}/fields.json | Recurso de solo lectura |
| Actualizar posiciones de campo | Campos del formulario | POST | /rest/asset/v1/form/{id}/reArrange.json | Recurso de lectura-escritura |
| Actualizar campo de formulario | Campos del formulario | POST | /rest/asset/v1/form/{id}/field/{fieldId}.json | Recurso de lectura-escritura |
| Aprobar borrador de formulario | Formularios | POST | /rest/asset/v1/form/{id}/approveDraft.json | Recurso de lectura-escritura |
| Clonar formulario | Formularios | POST | /rest/asset/v1/form/{id}/clone.json | Recurso de lectura-escritura |
| Crear formulario | Formularios | POST | /rest/asset/v1/forms.json | Recurso de lectura-escritura |
| Obtener formulario utilizado por | Formularios | GET | /rest/asset/v1/form/{id}/usedBy.json | Recurso de lectura-escritura |
| Eliminar formulario | Formularios | POST | /rest/asset/v1/form/{id}/delete.json | Recurso de lectura-escritura |
| Descartar borrador de formulario | Formularios | POST | /rest/asset/v1/form/{id}/discardDraft.json | Recurso de lectura-escritura |
| Obtener formulario por identificador | Formularios | GET | /rest/asset/v1/form/{id}.json | Recurso de solo lectura |
| Obtener formulario por nombre | Formularios | GET | /rest/asset/v1/form/byName.json | Recurso de solo lectura |
| Obtener Forms | Formularios | GET | /rest/asset/v1/forms.json | Recurso de solo lectura |
| Obtener página de agradecimiento por ID de formulario | Formularios | GET | /rest/asset/v1/form/{id}/thankYouPage.json | Recurso de solo lectura |
| Actualizar metadatos de formulario | Formularios | POST | /rest/asset/v1/form/{id}.json | Recurso de lectura-escritura |
| Botón Actualizar envío | Formularios | POST | /rest/asset/v1/{id}/submitButton.json | Recurso de lectura-escritura |
| Actualizar página de agradecimiento | Formularios | POST | /rest/asset/v1/form/{id}/thankYouPage.json | Recurso de lectura-escritura |
| Sección Añadir contenido de la página de aterrizaje | Contenido de página de aterrizaje | POST | /rest/asset/v1/landingPage/{id}/content.json | Recurso de lectura-escritura |
| Eliminar sección de contenido de página de aterrizaje | Contenido de página de aterrizaje | POST | /rest/asset/v1/landingPage/{id}/content/{contentId}/delete.json | Recurso de lectura-escritura |
| Obtener contenido de la página de aterrizaje | Contenido de página de aterrizaje | GET | /rest/asset/v1/landingPage/{id}/content.json | Recurso de solo lectura |
| Obtener contenido dinámico de la página de aterrizaje | Contenido de página de aterrizaje | GET | /rest/asset/v1/landingPage/{id}/dynamicContent/{dynamicContentId}.json | Recurso de solo lectura |
| Actualizar Sección De Contenido De Página De Aterrizaje | Contenido de página de aterrizaje | POST | /rest/asset/v1/landingPage/{id}/content/{contentId}.json | Recurso de lectura-escritura |
| Actualizar Sección De Contenido Dinámico De La Página De Aterrizaje | Contenido de página de aterrizaje | POST | /rest/asset/v1/landingPage/{id}/dynamicContent/{dynamicContentId}.json | Recurso de lectura-escritura |
| Aprobar borrador de plantilla de página de aterrizaje | Plantillas de la página de destino | POST | /rest/asset/v1/landingPageTemplate/{id}/approveDraft.json | Recurso de lectura-escritura |
| Clonar plantilla de la página de destino | Plantillas de la página de destino | POST | /rest/asset/v1/landingPageTemplate/{id}/clone.json | Recurso de lectura-escritura |
| Crear plantilla de la página de destino | Plantillas de la página de destino | POST | /rest/asset/v1/landingPageTemplate.json | Recurso de lectura-escritura |
| Eliminar plantilla de página de destino | Plantillas de la página de destino | POST | /rest/asset/v1/landingPageTemplate/{id}/delete.json | Recurso de lectura-escritura |
| Descartar borrador de plantilla de página de aterrizaje | Plantillas de la página de destino | POST | /rest/asset/v1/landingPageTemplate/{id}/discardDraft.json | Recurso de lectura-escritura |
| Obtener plantilla de página de aterrizaje por ID | Plantillas de la página de destino | GET | /rest/asset/v1/landingPageTemplate/{id}.json | Recurso de solo lectura |
| Obtener plantilla de página de aterrizaje por nombre | Plantillas de la página de destino | GET | /rest/asset/v1/landingPageTemplates/byName.json | Recurso de solo lectura |
| Obtener contenido de plantilla de página de aterrizaje | Plantillas de la página de destino | GET | /rest/asset/v1/landingPageTemplate/{id}/content.json | Recurso de solo lectura |
| Obtener plantillas de página de aterrizaje | Plantillas de la página de destino | GET | /rest/asset/v1/landingPageTemplates.json | Recurso de solo lectura |
| Desaprobar plantilla de la página de destino | Plantillas de la página de destino | POST | /rest/asset/v1/landingPageTemplate/{id}/unapprove.json | Recurso de lectura-escritura |
| Actualizar contenido de plantilla de página de aterrizaje | Plantillas de la página de destino | POST | /rest/asset/v1/landingPageTemplate/{id}/content.json | Recurso de lectura-escritura |
| Actualizar metadatos de plantilla de página de aterrizaje | Plantillas de la página de destino | POST | /rest/asset/v1/landingPageTemplate/{id}.json | Recurso de lectura-escritura |
| Aprobar borrador de página de aterrizaje | Páginas de destino | POST | /rest/asset/v1/landingPage/{id}/approveDraft.json | Recurso de lectura-escritura |
| Clonar página de destino | Páginas de destino | POST | /rest/asset/v1/landingPage/{id}/clone.json | Recurso de lectura-escritura |
| Crear páginas de aterrizaje | Páginas de destino | POST | /rest/asset/v1/landingPages.json | Recurso de lectura-escritura |
| Eliminar página de destino | Páginas de destino | POST | /rest/asset/v1/landingPage/{id}/delete.json | Recurso de lectura-escritura |
| Descartar borrador de página de aterrizaje | Páginas de destino | POST | /rest/asset/v1/landingPage/{id}/discardDraft.json | Recurso de lectura-escritura |
| Obtener página de aterrizaje por ID | Páginas de destino | GET | /rest/asset/v1/landingPage/{id}.json | Recurso de solo lectura |
| Obtener página de aterrizaje por nombre | Páginas de destino | GET | /rest/asset/v1/landingPage/byName.json | Recurso de solo lectura |
| Obtener variables de página de aterrizaje | Páginas de destino | GET | /rest/asset/v1/landingPage/{id}/variables.json | Recurso de solo lectura |
| Obtener páginas de aterrizaje | Páginas de destino | GET | /rest/asset/v1/landingPages.json | Recurso de solo lectura |
| Previsualizar página de aterrizaje | Páginas de destino | GET | /rest/asset/v1/landingPage/{id}/preview.json | Recurso de solo lectura |
| Desaprobar página de destino | Páginas de destino | POST | /rest/asset/v1/landingPage/{id}/unapprove.json | Recurso de lectura-escritura |
| Actualizar metadatos de página de aterrizaje | Páginas de destino | POST | /rest/asset/v1/{id}.json | Recurso de lectura-escritura |
| Actualizar variables de página de aterrizaje | Páginas de destino | POST | /rest/asset/v1/landingPage/{id}/variable/{variableId}.json | Recurso de lectura-escritura |
| Crear reglas de redireccionamiento de página de aterrizaje | Páginas de destino | POST | /rest/asset/v1/redirectRules.json | Reglas de redireccionamiento de lectura y escritura |
| Eliminar regla de redireccionamiento de página de aterrizaje | Páginas de destino | POST | /rest/asset/v1/redirectRule/{id}/delete.json | Reglas de redireccionamiento de lectura y escritura |
| Obtener reglas de redireccionamiento de página de aterrizaje | Páginas de destino | GET | /rest/asset/v1/redirectRules.json | Reglas de redireccionamiento de solo lectura |
| Obtener regla de redireccionamiento de página de aterrizaje por ID | Páginas de destino | GET | /rest/asset/v1/redirectRule/{id}.json | Reglas de redireccionamiento de solo lectura |
| Actualizar regla de redireccionamiento de página de aterrizaje | Páginas de destino | POST | /rest/asset/v1/redirectRule/{id}.json | Reglas de redireccionamiento de lectura y escritura |
| Obtener dominios de página de aterrizaje | Páginas de destino | GET | /rest/asset/v1/landingPageDomains.json | Reglas de redireccionamiento de solo lectura |
| Asociar posible cliente | Clientes potenciales | POST | /rest/v1/leads/{id}/associate.json | Guía de solo escritura |
| Cambiar estado del programa de posibles clientes | Clientes potenciales | POST | /rest/v1/leads/programs/{programId}/status.json | Guía de solo escritura |
| Eliminar posibles clientes | Clientes potenciales | POST | /rest/v1/leads.json | Guía de solo escritura |
| Describir posible cliente | Clientes potenciales | GET | /rest/v1/leads/describe.json | Guía de solo lectura |
| Describir posible cliente2 | Clientes potenciales | GET | /rest/v1/leads/describe2.json | Guía de solo lectura |
| Describir miembro del programa | Clientes potenciales | GET | /rest/v1/program/members/describe.json | Guía de solo lectura |
| Obtener posible cliente por ID | Clientes potenciales | GET | /rest/v1/lead/{id}.json | Guía de solo lectura |
| Obtener particiones de posibles clientes | Clientes potenciales | GET | /rest/v1/leads/partitions.json | Guía de solo lectura |
| Obtener posibles clientes por tipo de filtro | Clientes potenciales | GET | /rest/v1/leads.json | Guía de solo lectura |
| Obtener posibles clientes por ID de programa | Clientes potenciales | GET | /rest/v1/leads/programs/{programId}.json | Guía de solo lectura |
| Combinar leads | Clientes potenciales | POST | /rest/v1/leads/{id}/merge.json | Guía de solo escritura |
| Obtener listas por ID de posible cliente | Clientes potenciales | GET | /rest/v1/leads/{leadId}.json | Recurso de solo lectura |
| Obtener programas por ID de posible cliente | Clientes potenciales | GET | /rest/v1/leads/{leadId}programMembership.json | Recurso de solo lectura |
| Obtener campañas inteligentes por ID de posible cliente | Clientes potenciales | GET | /rest/v1/leads/{leadId}/smartCampaignMembership.json | Recurso de solo lectura |
| Insertar el cliente potencial en Marketo | Clientes potenciales | POST | /rest/v1/leads/partitions.json | Guía de solo escritura |
| Enviar formulario | Clientes potenciales | POST | /rest/v1/leads/submitForm.json | Guía de solo escritura |
| Sincronizar posibles clientes | Clientes potenciales | POST | /rest/v1/leads.json | Guía de solo escritura |
| Actualizar partición de posibles clientes | Clientes potenciales | POST | /rest/v1/leads/partitions.json | Guía de solo escritura |
| Obtener campo de posible cliente por nombre | Clientes potenciales | GET | /rest/v1/leads/schema/fields/{fieldApiName}.json | Campo personalizado de esquema de Lectura-Escritura |
| Obtener campos de posibles clientes | Clientes potenciales | GET | /rest/v1/leads/schema/fields.json | Campo personalizado de esquema de Lectura-Escritura |
| Crear campos de posibles clientes | Clientes potenciales | POST | /rest/v1/leads/schema/fields.json | Campo personalizado de esquema de Lectura-Escritura |
| Actualizar campo de posible cliente | Clientes potenciales | POST | /rest/v1/leads/schema/fields/{fieldApiName}.json | Campo personalizado de esquema de Lectura-Escritura |
| Añadir miembros de la lista de cuentas con nombre | Listas de cuentas con nombre | POST | /rest/v1/namedaccountlist/{id}/namedaccounts.json | Cuenta nombrada de lectura y escritura |
| Eliminar listas de cuentas con nombre | Listas de cuentas con nombre | POST | /rest/v1/namedaccountlists/delete.json | Lista de cuentas nombradas habilitadas para lectura y escritura |
| Obtener miembros de lista de cuentas con nombre | Listas de cuentas con nombre | GET | /rest/v1/namedaccountlist/{id}/namedaccounts.json | Cuenta nombrada de solo lectura |
| Obtener listas de cuentas con nombre | Listas de cuentas con nombre | GET | /rest/v1/namedaccountlists.json | Lista de cuentas nombradas de solo lectura |
| Quitar miembros de la lista de cuentas con nombre | Listas de cuentas con nombre | POST | /rest/v1/namedaccountlist/{id}/namedaccounts/remove.json | Cuenta nombrada de lectura y escritura |
| Sincronizar listas de cuentas con nombre | Listas de cuentas con nombre | POST | /rest/v1/namedaccountlists.json | Lista de cuentas nombradas habilitadas para lectura y escritura |
| Eliminar cuentas con nombre | Cuentas nombradas | POST | /rest/v1/namedaccounts/delete.json | Cuenta nombrada de lectura y escritura |
| Describir cuentas con nombre | Cuentas nombradas | GET | /rest/v1/namedaccounts/describe.json | Cuenta nombrada de solo lectura |
| Obtener cuentas con nombre | Cuentas nombradas | GET | /rest/v1/namedaccounts.json | Cuenta nombrada de solo lectura |
| Sincronizar cuentas con nombre | Cuentas nombradas | POST | /rest/v1/namedaccounts.json | Cuenta nombrada de lectura y escritura |
| Obtener campo de cuenta con nombre por nombre | Cuentas nombradas | GET | /rest/v1/namedaccounts/schema/fields/{fieldApiName}.json | Campo personalizado de esquema de Lectura-Escritura |
| Obtener campos de cuenta con nombre | Cuentas nombradas | GET | /rest/v1/namedaccounts/schema/fields.json | Campo personalizado de esquema de Lectura-Escritura |
| Eliminar oportunidades | Oportunidades | POST | /rest/v1/opportunities/delete.json | Oportunidad habilitada para lectura y escritura |
| Eliminar roles de oportunidad | Oportunidades | POST | /rest/v1/opportunities/roles/delete.json | Oportunidad habilitada para lectura y escritura |
| Describir oportunidad | Oportunidades | GET | /rest/v1/opportunities/describe.json | Oportunidad de solo lectura |
| Describir función de oportunidad | Oportunidades | GET | /rest/v1/opportunities/roles/describe.json | Oportunidad de solo lectura |
| Obtener oportunidades | Oportunidades | GET | /rest/v1/opportunities.json | Oportunidad de solo lectura |
| Obtener roles de oportunidad | Oportunidades | GET | /rest/v1/opportunities/roles.json | Oportunidad de solo lectura |
| Oportunidades de sincronización | Oportunidades | POST | /rest/v1/opportunities.json | Oportunidad habilitada para lectura y escritura |
| Sincronizar roles de oportunidad | Oportunidades | POST | /rest/v1/opportunities/roles.json | Oportunidad habilitada para lectura y escritura |
| Obtener campo de oportunidad por nombre | Oportunidades | GET | /rest/v1/applications/schema/fields/{fieldApiName}.json | Campo personalizado de esquema de Lectura-Escritura |
| Obtener campos de oportunidad | Oportunidades | GET | /rest/v1/opportunities/schema/fields.json | Campo personalizado de esquema de Lectura-Escritura |
| Eliminar miembros del programa | Miembros del programa | POST | /rest/v1/programs/{programId}/members/delete.json | Guía de solo escritura |
| Describir miembro del programa | Miembros del programa | GET | /rest/v1/programs/members/describe.json | Guía de solo lectura |
| Obtener miembros del programa | Miembros del programa | GET | /rest/v1/programs/{programId}/members.json | Guía de solo lectura |
| Sincronizar datos de miembros del programa | Miembros del programa | POST | /rest/v1/programs/{programId}/members.json | Guía de solo escritura |
| Estado de miembro del programa de sincronización | Miembros del programa | POST | /rest/v1/programs/{programId}/members/status.json | Guía de solo escritura |
| Obtener campo de miembro del programa por nombre | Miembros del programa | GET | /rest/v1/programs/members/schema/fields/{fieldApiName}.json | Campo personalizado de esquema de Lectura-Escritura |
| Obtener campos de miembros del programa | Miembros del programa | GET | /rest/v1/programs/members/schema/fields.json | Campo personalizado de esquema de Lectura-Escritura |
| Crear campos de miembros del programa | Miembros del programa | POST | /rest/v1/programs/members/schema/fields.json | Campo personalizado de esquema de Lectura-Escritura |
| Actualizar campo de miembro del programa | Miembros del programa | POST | /rest/v1/programs/members/schema/fields/{fieldApiName}.json | Campo personalizado de esquema de Lectura-Escritura |
| Aprobar programa | Programas | POST | /rest/asset/v1/program/{id}/approve.json | Recurso de lectura-escritura |
| Clonar programa | Programas | POST | /rest/asset/v1/program/{id}/clone.json | Recurso de lectura-escritura |
| Creación de programas | Programas | POST | /rest/asset/v1/programs.json | Recurso de lectura-escritura |
| Eliminar programa | Programas | POST | /rest/asset/v1/program/{id}/delete.json | Recurso de lectura-escritura |
| Obtener programa por identificador | Programas | GET | /rest/asset/v1/program/{id}.json | Recurso de solo lectura |
| Obtener programa por nombre | Programas | GET | /rest/asset/v1/program/byName.json | Recurso de solo lectura |
| Obtener programas | Programas | GET | /rest/asset/v1/programs.json | Recurso de solo lectura |
| Obtener programas por etiqueta | Programas | GET | /rest/asset/v1/program/byTag.json | Recurso de solo lectura |
| Obtener lista inteligente por ID de programa | Programas | GET | /rest/asset/v1/program/{id}/smartList.json | Recurso de solo lectura |
| Desaprobar programa | Programas | POST | /rest/asset/v1/program/{id}/unapprove.json | Recurso de lectura-escritura |
| Actualizar metadatos del programa | Programas | POST | /rest/asset/v1/program/{id}.json | Recurso de lectura-escritura |
| Actualizar etiqueta de programa | Programas | POST | /rest/asset/v1/program/{id}/tag/{tagType}.json | Recurso de lectura-escritura |
| Eliminar etiqueta de programa | Programas | POST | /rest/asset/v1/program/{id}/tag/{tagType}/delete.json | Recurso de lectura-escritura |
| Eliminar SalesPersons | Vendedores | POST | /rest/v1/salespersons/delete.json | Persona de ventas habilitada para lectura y escritura |
| Describir SalesPersons | Vendedores | GET | /rest/v1/salespersons/describe.json | Persona de ventas de solo lectura |
| Obtener SalesPersons | Vendedores | GET | /rest/v1/salespersons.json | Persona de ventas de solo lectura |
| Sincronizar SalesPersons | Vendedores | POST | /rest/v1/salespersons.json | Persona de ventas habilitada para lectura y escritura |
| Obtener segmentaciones | Segmentos | GET | /rest/asset/v1/segmentation.json | Recurso de solo lectura |
| Obtener Segmentos Para Segmentaciones | Segmentos | GET | /rest/asset/v1/segmentation/{id}/segments.json | Recurso de solo lectura |
| Activar Campaña inteligente | Campañas inteligentes | POST | /rest/asset/v1/smartCampaign/{id}/activate.json | Activar campaña |
| Clonar campaña inteligente | Campañas inteligentes | POST | /rest/asset/v1/smartCampaign/{id}/clone.json | Recurso de lectura-escritura |
| Crear campaña inteligente | Campañas inteligentes | POST | /rest/asset/v1/smartCampaigns.json | Recurso de lectura-escritura |
| Desactivar campaña inteligente | Campañas inteligentes | POST | /rest/asset/v1/smartCampaign/{id}/deactivate.json | Desactivar campaña |
| Eliminar campaña inteligente | Campañas inteligentes | POST | /rest/asset/v1/smartCampaign/{id}/delete.json | Recurso de lectura-escritura |
| Obtener campañas inteligentes | Campañas inteligentes | GET | /rest/asset/v1/smartCampaigns.json | Recurso de solo lectura |
| Obtener campañas inteligentes por ID | Campañas inteligentes | GET | /rest/asset/v1/smartCampaign/{id}.json | Recurso de solo lectura |
| Obtener campaña inteligente por nombre | Campañas inteligentes | GET | /rest/asset/v1/smartCampaign/byName.json | Recurso de solo lectura |
| Obtener lista inteligente por ID de campaña inteligente | Campañas inteligentes | GET | /rest/asset/v1/smartCampaign/{id}/smartList.json | Recurso de solo lectura |
| Actualizar campaña inteligente | Campañas inteligentes | POST | /rest/asset/v1/smartCampaign/{id}.json | Recurso de lectura-escritura |
| Clonar lista inteligente | Listas inteligentes | POST | /rest/asset/v1/smartList/{id}/clone.json | Recurso de lectura-escritura |
| Eliminar lista inteligente | Listas inteligentes | POST | /rest/asset/v1/smartList/{id}/delete.json | Recurso de lectura-escritura |
| Obtener lista inteligente por identificador | Listas inteligentes | GET | /rest/asset/v1/smartList/{id}.json | Recurso de solo lectura |
| Obtener lista inteligente por nombre | Listas inteligentes | GET | /rest/asset/v1/smartList/byName.json | Recurso de solo lectura |
| Obtener listas inteligentes | Listas inteligentes | GET | /rest/asset/v1/smartLists.json | Recurso de solo lectura |
| Aprobar borrador de fragmento | Fragmentos | POST | /rest/asset/v1/snippet/{id}/approveDraft.json | Recurso de lectura-escritura |
| Clonar fragmento | Fragmentos | POST | /rest/asset/v1/snippet/{id}/clone.json | Recurso de lectura-escritura |
| Crear fragmento | Fragmentos | POST | /rest/asset/v1/snippets.json | Recurso de lectura-escritura |
| Eliminar fragmento | Fragmentos | POST | /rest/asset/v1/snippet/{id}/delete.json | Recurso de lectura-escritura |
| Descartar borrador de fragmento | Fragmentos | POST | /rest/asset/v1/snippet/{id}/discardDraft.json | Recurso de lectura-escritura |
| Obtener contenido dinámico | Fragmentos | GET | /rest/asset/v1/snippet/{id}/dynamicContent.json | Recurso de solo lectura |
| Obtener fragmento por ID | Fragmentos | GET | /rest/asset/v1/snippet/{id}.json | Recurso de solo lectura |
| Obtener contenido del fragmento | Fragmentos | GET | /rest/asset/v1/snippet/{id}/content.json | Recurso de solo lectura |
| Obtener fragmentos | Fragmentos | GET | /rest/asset/v1/snippets.json | Recurso de solo lectura |
| Desaprobar fragmento | Fragmentos | POST | /rest/asset/v1/snippet/{id}/unapprove.json | Recurso de lectura-escritura |
| Actualizar contenido de fragmento | Fragmentos | POST | /rest/asset/v1/snippet/{id}/content.json | Recurso de lectura-escritura |
| Actualizar fragmento de contenido dinámico | Fragmentos | POST | /rest/asset/v1/snippet/{id}/dynamicContent/{segmentId}.json | Recurso de lectura-escritura |
| Actualizar metadatos de fragmento | Fragmentos | POST | /rest/asset/v1/snippet/{id}.json | Recurso de lectura-escritura |
| Agregar a Lista | Listas estáticas | POST | /rest/v1/lists/{listId}/leads.json | Guía de solo escritura |
| Crear lista estática | Listas estáticas | POST | /asset/v1/staticLists.json | Recurso de lectura-escritura |
| Eliminar lista estática | Listas estáticas | POST | /asset/v1/staticList/{id}/delete.json | Recurso de lectura-escritura |
| Obtener posibles clientes por ID de lista | Listas estáticas | GET | /rest/v1/lists/{listId}/leads.json | Guía de solo lectura |
| Obtener lista por identificador | Listas estáticas | GET | /rest/v1/lists/{id}.json | Guía de solo lectura |
| Obtener listas | Listas estáticas | GET | /rest/v1/lists.json | Guía de solo lectura |
| Obtener lista estática por identificador | Listas estáticas | GET | /asset/v1/staticList/{id}.json | Recurso de solo lectura |
| Obtener lista estática por nombre | Listas estáticas | GET | /asset/v1/staticList/byName.json | Recurso de solo lectura |
| Obtener listas estáticas | Listas estáticas | GET | /asset/v1/staticLists.json | Recurso de solo lectura |
| Miembro de la lista | Listas estáticas | GET | /rest/v1/lists/{listId}/leads/ismember.json | Guía de solo lectura |
| Quitar de Lista | Listas estáticas | DELETE | /rest/v1/lists/{listId}/leads.json | Guía de solo escritura |
| Actualizar metadatos de lista estática | Listas estáticas | POST | /asset/v1/staticList/{id}.json | Recurso de lectura-escritura |
| Obtener etiqueta por nombre | Etiquetas | GET | /rest/asset/v1/tagType/byName.json | Recurso de solo lectura |
| Obtener tipos de etiquetas | Etiquetas | GET | /rest/asset/v1/tagTypes.json | Recurso de solo lectura |
| Crear token | Tokens | POST | /rest/asset/v1/folder/{id}/tokens.json | Recurso de lectura-escritura |
| Eliminar token por nombre | Tokens | POST | /rest/asset/v1/folder/{id}/tokens/delete.json | Recurso de lectura-escritura |
| Obtener tokens por ID de carpeta | Tokens | GET | /rest/asset/v1/folder/{id}/tokens.json | Recurso de solo lectura |
| Agregar roles | Gestión de usuarios | POST | /userservice/management/v1/users/{userid}/roles/create.json | Acceder a la Api de administración de usuario |
| Eliminar usuario invitado | Gestión de usuarios | POST | /userservice/management/v1/users/{userId}/invite/delete.json | Acceder a la Api de administración de usuario |
| Eliminar roles | Gestión de usuarios | POST | /userservice/management/v1/users/{userid}/roles/delete.json | Acceder a la Api de administración de usuario |
| Eliminar usuario | Gestión de usuarios | POST | /userservice/management/v1/users/{userId}/delete.json | Acceder a la Api de administración de usuario |
| Obtener usuario invitado por identificador | Gestión de usuarios | GET | /userservice/management/v1/users/{userid}/invite.json | Acceder a la Api de administración de usuario |
| Obtener roles | Gestión de usuarios | GET | /userservice/management/v1/users/roles.json | Acceder a la Api de administración de usuario |
| Obtener roles y espacios de trabajo por identificador | Gestión de usuarios | GET | /userservice/management/v1/users/{userid}/roles.json | Acceder a la Api de administración de usuario |
| Obtener usuarios | Gestión de usuarios | GET | /userservice/management/v1/users/allusers.json | Acceder a la Api de administración de usuario |
| Obtener usuario por identificador | Gestión de usuarios | GET | /userservice/management/v1/users/{userid}/user.json | Acceder a la Api de administración de usuario |
| Obtener espacios de trabajo | Gestión de usuarios | GET | /userservice/management/v1/users/workspaces.json | Acceder a la Api de administración de usuario |
| Invitar usuario | Gestión de usuarios | POST | /userservice/management/v1/users/invite.json | Acceder a la Api de administración de usuario |
| Actualizar atributos de usuario | Gestión de usuarios | POST | /userservice/management/v1/users/{userId}/update.json | Acceder a la Api de administración de usuario |