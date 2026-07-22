---
title: Obtener actualizaciones de API de clientes potenciales
feature: REST API
description: Conozca los cambios en los límites de Obtener actividades de posibles clientes y Obtener extremos de cambios de posibles clientes.
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 0%

---

# Obtener actualizaciones de API de clientes potenciales

A partir del 30 de septiembre de 2026, las llamadas a los extremos [Obtener actividades de posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadActivitiesUsingGET) u [Obtener cambios de posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadChangesUsingGET) que incluyan el parámetro `listId` generarán un error si las listas de destino contienen 10 000 posibles clientes o más. Los extremos devolverán un código de error 1003 que indica que la lista estática de destino tiene demasiados registros.

Una o más llamadas recientes a la API se verán afectadas por este cambio. Para evitar interrupciones en el servicio, es posible que tenga que actualizar la integración de sus aplicaciones con Marketo antes del 30 de septiembre de 2026.

Estas consultas suelen crear búsquedas sin resultados potenciales o con tiempo de espera antes de encontrar los resultados. Limitar el tamaño establecido mejora la respuesta a las consultas y ayuda a que las búsquedas se completen de forma oportuna.

## ¿Cómo puedo saber si estoy afectado?

Este cambio solo afecta a un pequeño número de instancias de Marketo Engage. Los administradores de las suscripciones afectadas recibirán una notificación en la aplicación antes de que se aplique el cambio.

## ¿Qué debo hacer?

Comparta este documento con las personas o el equipo responsable de sus integraciones de Marketo Engage.

Según el caso de uso, utilice una de estas opciones de migración:

* Limite las listas estáticas utilizadas para la extracción de actividades a 10 000 miembros. Divida las listas existentes en listas más pequeñas para seguir encuestando las mismas audiencias para las actividades.
* Extraer actividades o cambios en el valor de los datos utilizando Extracción de actividades en lote o flujos de datos. Únase a los resultados de la suscripción a la lista estática con [getLeadByListId](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadsByListIdUsingGET_1) o [Extracción masiva de posibles clientes](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/rest/bulk-extract/bulk-lead-extract).

## ¿Qué Pasará Si No Hago Nada?

Las integraciones de la API podrían verse interrumpidas por errores no controlados al consultar actividades desde listas estáticas con un gran número de miembros.
