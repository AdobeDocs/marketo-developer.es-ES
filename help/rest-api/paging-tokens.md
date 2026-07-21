---
title: Tokens de paginación
feature: REST API
description: Utilice tokens de paginación de la API de REST de Marketo para recuperar actividades y posibles clientes, lo que abarca tokens basados en fechas y posiciones, errores ISO 8601 sinceDatetime y 414.
exl-id: 63fbbf03-8daf-4add-85b0-a8546c825e5b
TQID: https://experienceleague.adobe.com/Ut05n-Y-qPJnvcNRs9liwE3NVBMbJlvaGyv-nExRsek
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 387
ht-degree: 1%

---

# Tokens de paginación

Marketo proporciona tokens de paginación para recorrer página a página los resultados o recuperar datos actualizados en relación con una fecha específica.

Algunas respuestas devuelven cadenas de token de paginación largas, lo que puede provocar un error HTTP 414. Ver información sobre la administración de estos [errores](error-codes.md).

Consulte la documentación de [API de token de paginación](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getActivitiesPagingTokenUsingGET).

## Tipos de token

Marketo proporciona dos tipos de tokens de paginación relacionados pero distintos:

- Los tokens basados en fechas recuperan registros que se producen después de una fecha y hora especificadas.
- Los tokens basados en posiciones atraviesan registros en un conjunto de resultados.

## Basado en fecha

Un token de paginación basado en fecha representa una fecha y hora. Utilícelo para recuperar actividades, cambios en el valor de los datos y posibles clientes eliminados que se produzcan después de esa fecha y hora.

Genere un token basado en fecha llamando al extremo [Obtener token de paginación](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getActivitiesPagingTokenUsingGET) con una fecha y hora:

```http
GET /rest/v1/activities/pagingtoken.json?sinceDatetime=2014-10-06T13:22:17-08:00
```

```json
{
    "requestId": "1607c#14884f3e74e",
    "success": true,
    "nextPageToken": "GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ"
}
```

El parámetro `sinceDateTime` debe usar la notación de fecha estándar [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601). Para obtener los mejores resultados, proporcione una fecha y hora completa con una zona horaria.

Represente la zona horaria como un desplazamiento con respecto a GMT con el siguiente formato:

`yyyy-mm-ddThh:mm:ss+|-hh:mm`

Alternativamente, utilice una &quot;Z&quot; mayúscula para representar UTC:

`yyyy-mm-ddThh:mm:ssZ`

Por ejemplo:

`2016-09-15T15:53:00+05:00`

`2016-09-15T10:53:00Z`

Dado que `sinceDateTime` es un parámetro de consulta, codifique su valor con URL.

Pase la cadena `nextPageToken` devuelta a una llamada de [Obtener actividades de posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getLeadActivitiesUsingGET), [Obtener cambios de posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getLeadChangesUsingGET) o [Obtener posibles clientes eliminados](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getDeletedLeadsUsingGET). La llamada recupera registros que se producen después de la fecha y hora proporcionadas para la API de obtención de token de paginación.

```http
GET /rest/v1/activities.json?nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ&activityTypeIds=1&activityTypeIds=12
```

## Basado en puestos

Se puede devolver un token de paginación basado en posiciones mediante cualquier llamada de recuperación por lotes a una API de base de datos de posibles clientes. El token funciona como un cursor de base de datos y permite el recorrido de registros.

Por ejemplo, una llamada Obtener posibles clientes por tipo de filtro puede devolver un conjunto de resultados mayor que el tamaño de lote solicitado, que normalmente tiene un valor máximo y predeterminado de 300. Cuando hay más resultados disponibles, la respuesta establece el campo moreResult en true y devuelve un `nextPageToken`.

Para recuperar la página siguiente, realice otra llamada y pase el valor `nextPageToken` de la respuesta anterior. La respuesta devuelve la página siguiente del conjunto de resultados.
