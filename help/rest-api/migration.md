---
title: Obtener actualizaciones de API de clientes potenciales
feature: REST API
description: Conozca los cambios en los límites de Obtener actividades de posibles clientes y Obtener extremos de cambios de posibles clientes.
source-git-commit: e71bcf289229867bc969345d79c8f014761aaaf9
workflow-type: tm+mt
source-wordcount: '356'
ht-degree: 0%

---

# Obtener actualizaciones de API de clientes potenciales

A partir del 30 de septiembre de 2026, las llamadas a los extremos [Obtener actividades de posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadActivitiesUsingGET) u [Obtener cambios de posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadChangesUsingGET) que incluyan el parámetro `listId` generarán un error si las listas de destino contienen 10 000 posibles clientes o más con un código de error 1003 que indique que la lista estática de destino tiene demasiados registros. Recientemente se han realizado una o más llamadas a la API que se verían afectadas por este cambio. Para evitar interrupciones en el servicio, es posible que tenga que actualizar la forma en que sus aplicaciones se integran con Marketo antes del 30 de septiembre de 2026.

Estos tipos de consultas a menudo crean búsquedas que no tienen resultados potenciales o agotan el tiempo de espera antes de que se encuentren resultados. Limitar el tamaño del conjunto mejora la capacidad de respuesta de estos tipos de consultas y garantiza que una búsqueda del conjunto de datos se pueda completar de manera oportuna.

## ¿Cómo puedo saber si estoy afectado?

Este cambio solo afectará a un pequeño número de instancias de Marketo Engage. Los administradores de las suscripciones afectadas recibirán una notificación dentro de la aplicación antes de que se aplique el cambio.

## ¿Qué debo hacer?

Debe compartir este documento con las personas o el equipo responsable de sus integraciones de Marketo Engage.

Según el caso de uso, hay dos opciones básicas para migrar la aplicación a:

* Limite las listas estáticas de las que extrae actividades a un máximo de 10 000 miembros. Puede dividir cualquiera de las listas existentes en listas más pequeñas para seguir encuestando las mismas audiencias para las actividades.
* Extraiga sus actividades o cambios en el valor de los datos mediante la extracción masiva de actividades o flujos de datos y una esos resultados a la pertenencia a una lista estática con [getLeadByListId](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadsByListIdUsingGET_1) o [Extracción masiva de posibles clientes](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/rest/bulk-extract/bulk-lead-extract)

## ¿Qué Pasará Si No Hago Nada?

Es posible que experimente interrupciones en el funcionamiento de las integraciones de la API debido a errores no controlados al consultar actividades desde listas estáticas con un gran número de miembros.
