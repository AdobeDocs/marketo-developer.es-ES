---
title: Migración a la API de REST
feature: SOAP
description: Migración de las API de SOAP a REST
exl-id: c2956db3-defe-4163-99f3-58654ce8ee2b
source-git-commit: 8a785b0719e08544ed1a87772faf90bd9dda3077
workflow-type: tm+mt
source-wordcount: '624'
ht-degree: 1%

---

# Migración a la API de REST

La API de Marketo Engage SOAP se retirará después del 31 de octubre de 2025. Todas las integraciones existentes que usan la API de SOAP deben retirarse o migrarse a la [API de REST de Marketo Engage](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/rest/rest-api) en esta fecha para evitar interrupciones en el servicio.

## Migración

La API de SOAP admite una gama limitada de casos de uso en comparación con [REST AP](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/rest/rest-api)I. A la hora de determinar qué extremos asignar a sus casos de uso, debe seguir [Prácticas recomendadas de integración de Marketo](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/rest/marketo-integration-best-practices)

Las [arquitecturas de referencia](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/rest/reference-architectures) están disponibles para los casos de uso de [Sincronización de CRM](https://experienceleague.adobe.com/docs/marketo-developer/assets/sync-architecture-whitepaper.pdf?lang=es) y [Exportación de Data Warehouse](https://experienceleague.adobe.com/docs/marketo-developer/assets/reference_architecture.pdf?lang=es).

## Autenticación

[Documentación de autenticación](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/rest/authentication)

La API de REST de Marketo utiliza la autenticación basada en OAuth 2.0 con el tipo de concesión de Credenciales del cliente. Los tokens de acceso son válidos durante una hora después de su creación.

## Clientes potenciales

[Documentación de API de cliente potencial](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/rest/lead-database/leads)

La API de SOAP admite la sincronización de datos de posibles clientes, la [asociación de cookies de Munchkin](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/javascriptapi/leadtracking/lead-tracking) y la combinación de posibles clientes. Si la aplicación llama al método syncLead de SOAP y establece el parámetro `marketoCookie`, puede migrar mediante:

1. Usando el método REST [Sincronizar posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncLeadUsingPOST), seguido de [Posible cliente asociado](https://developer.adobe.com/marketo-apis/api/mapi/#operation/associateLeadUsingPOST)
2. Puede llamar a [Enviar formulario](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/rest/lead-database/leads), aunque para ello es necesario configurar algunas API de Marketing Assets e interactuar con [Forms](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/rest/assets/forms)

Las aplicaciones que usan el tipo de clave `foreignSysPersonId` deberían migrar a mediante un campo de posible cliente personalizado para representar este identificador externo y usar [Sincronizar posibles clientes](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/rest/lead-database/leads#create-and-update) o [Importación masiva de posibles clientes](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/rest/bulk-import/bulk-lead-import) métodos REST.

| Método SOAP | Método(s) REST |
| --- | --- |
| [getLead](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/soap/leads/getlead) | [Obtener posible cliente por id.](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET), [Obtener posibles clientes por tipo de filtro](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) |
| [getMultipleLeads](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/soap/leads/getmultipleleads) | [Obtener posible cliente por id.](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET), [Obtener posibles clientes por tipo de filtro](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET), [Obtener posibles clientes por id. de programa](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByProgramIdUsingGET), [Obtener posibles clientes por id. de lista](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByListIdUsingGET), [Exportación masiva de posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads) |
| [mergeLeads](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/soap/leads/mergeleads) | [Combinar posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi/#operation/mergeLeadsUsingPOST) |
| [syncLead](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/soap/leads/synclead) | [Sincronizar posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncLeadUsingPOST) [Enviar formulario](https://developer.adobe.com/marketo-apis/api/mapi/#operation/SubmitFormUsingPOST) [Asociar posible cliente](https://developer.adobe.com/marketo-apis/api/mapi/#operation/associateLeadUsingPOST) |
| [syncMultipleLeads](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/soap/leads/syncmultipleleads) | [Sincronizar posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncLeadUsingPOST) [Importación en lotes](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads) |

## Objetos M

M Objects era un concepto global para admitir la exportación de datos de Atribución de oportunidad para análisis externos y trabajaba con tres tipos de objetos: Oportunidades, Funciones de oportunidad y Programas.

Documentación de REST:

- [Oportunidad](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/rest/lead-database/opportunities)
- [Roles](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/rest/lead-database/opportunity-roles)
- [Programas](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/rest/assets/programs)

| Método SOAP | Método(s) REST |
| --- | --- |
| [deleteMObjects](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/soap/marketo-objects/deletemobjects) | [Eliminar oportunidades](https://developer.adobe.com/marketo-apis/api/mapi/#operation/deleteOpportunitiesUsingPOST), [Eliminar roles de oportunidad](https://developer.adobe.com/marketo-apis/api/mapi/#operation/deleteOpportunityRolesUsingPOST) |
| [describirMObjects](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/soap/marketo-objects/describemobject) | [Describir oportunidad](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeUsingGET_4), [Describir rol de oportunidad](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeOpportunityRoleUsingGET) |
| [getMObjects](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/soap/marketo-objects/getmobjects) | [Obtener oportunidades](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getOpportunitiesUsingGET), [Obtener roles de oportunidad](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeOpportunityRoleUsingGET) |
| [listMObjects](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/soap/marketo-objects/listmobjects) | N/A |
| [syncMObjects](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/soap/marketo-objects/syncmobjects) | [Oportunidades de sincronización](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncOpportunitiesUsingPOST), [Funciones de oportunidad de sincronización](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncOpportunityRolesUsingPOST) |
| [getChannels](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/soap/programs/getchannels) | [Obtener canales](https://developer.adobe.com/marketo-apis/api/asset/#operation/getAllChannelsUsingGET) |
| [getTags](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/soap/programs/gettags) | [Obtener tipos de etiqueta](https://developer.adobe.com/marketo-apis/api/asset/#operation/getTagTypesUsingGET), [Obtener etiqueta por nombre](https://developer.adobe.com/marketo-apis/api/asset/#operation/getTagByNameUsingGET) |

## Listas estáticas

Los casos de uso de listas estáticas en la API de SOAP se limitan a la ingesta de datos de pertenencia y posible cliente, y a la eliminación de la pertenencia, que se puede realizar con los métodos [Agregar a lista](https://developer.adobe.com/marketo-apis/api/mapi/#operation/addLeadsToListUsingPOST), [Importar posibles clientes en lote](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/rest/bulk-import/bulk-lead-import) o [Quitar de la lista](https://developer.adobe.com/marketo-apis/api/mapi/#operation/removeLeadsFromListUsingDELETE) REST.

| Método SOAP | Método(s) REST |
| --- | --- |
| [getImportToListStatus](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/soap/static-lists/getimporttoliststatus) | [Posibles clientes de importación en lotes](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads) |
| [importToList](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/soap/static-lists/importtolist) | [Agregar A La Lista](https://developer.adobe.com/marketo-apis/api/mapi/#operation/addLeadsToListUsingPOST) [Posibles Clientes De Importación En Lote](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads) |
| [listOperation](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/soap/static-lists/listoperation) | [Quitar De La Lista](https://developer.adobe.com/marketo-apis/api/mapi/#operation/removeLeadsFromListUsingDELETE) |

## Actividades

La API de SOAP solo admite la recuperación de actividades.

Documentación de REST:

- [Actividades sincrónicas](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/rest/lead-database/activities)
- [Extracto de actividades en lotes](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/rest/bulk-extract/bulk-activity-extract)

| Método SOAP | Método(s) REST |
| --- | --- |
| [getLeadActivity](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/soap/activities/getleadactivity) | [Actividades de exportación en lotes](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities) [Obtener actividades de posible cliente](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadActivitiesUsingGET) |
| [getLeadChanges](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/soap/activities/getleadchanges) | [Actividades de exportación en lotes](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities) [Obtener cambios de posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadChangesUsingGET) |

## Campañas

Documentación de REST:

- [Campañas inteligentes](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/rest/assets/smart-campaigns)

La API de SOAP solo admite tres casos de uso para campañas inteligentes: [Activar posibles clientes para que cumplan los requisitos para una campaña inteligente solicitada](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/rest/assets/smart-campaigns#trigger), recuperar esas campañas inteligentes solicitadas y [Programar una ejecución futura de una campaña inteligente](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/rest/assets/smart-campaigns#schedule).

| Método SOAP | Método(s) REST |
| --- | --- |
| [getCampaignsForSource](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/soap/campaigns/getcampaignsforsource) | [Obtener campañas inteligentes](https://developer.adobe.com/marketo-apis/api/asset/#operation/getAllSmartCampaignsGET) |
| [requestCampaign](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/soap/campaigns/requestcampaign) | [Solicitar campaña](https://developer.adobe.com/marketo-apis/api/mapi/#operation/triggerCampaignUsingPOST) |
| [scheduleCampaign](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/soap/campaigns/schedulecampaign) | [Programar campaña](https://developer.adobe.com/marketo-apis/api/mapi/#operation/scheduleCampaignUsingPOST) |

## Objetos personalizados

Documentación de REST:

- [Objetos personalizados](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/rest/lead-database/custom-objects)

La API de SOAP solo admitía operaciones de CRUD para objetos personalizados.

| Método SOAP | Método(s) REST |
| --- | --- |
| [deleteCustomObjects](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/soap/custom-objects/deletecustomobjects) | [Eliminar objetos personalizados](https://developer.adobe.com/marketo-apis/api/mapi/#operation/deleteCustomObjectsUsingPOST) |
| [getCustomObjects](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/soap/custom-objects/getcustomobjects) | [Obtener objetos personalizados](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getCustomObjectsUsingGET) |
| [syncCustomObjects](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/soap/custom-objects/synccustomobjects) | [Sincronizar objetos personalizados](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncCustomObjectsUsingPOST) [Importar objetos personalizados en lotes](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/rest/bulk-import/bulk-custom-object-import) |
