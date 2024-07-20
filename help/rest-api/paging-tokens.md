---
title: Tokens de paginación
feature: REST API
description: Ver datos de tokens de paginación.
exl-id: 63fbbf03-8daf-4add-85b0-a8546c825e5b
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '345'
ht-degree: 0%

---

# Tokens de paginación

Para recorrer página a página los resultados o recuperar datos actualizados en relación con un dato determinado, Marketo proporciona tokens de paginación.

En algunos casos, se pueden devolver cadenas de token de paginación largas. Esto puede hacer que se encuentre con un código de error HTTP 414. Puede encontrar más información sobre cómo manejar estos [errores](error-codes.md).

## Tipos de token

Existen dos tipos de tokens de paginación relacionados, pero distintos, que proporciona Marketo:

- Basado en fecha
- Basado en puestos

## Basado en fecha

El primero es un token de paginación que representa una fecha. Se utilizan para recuperar actividades, cambios en el valor de los datos y posibles clientes eliminados que se produjeron después de la fecha representada por el token de paginación. Este tipo de token de paginación se genera al llamar al extremo [Obtener token de paginación](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getActivitiesPagingTokenUsingGET) y al incluir una fecha y hora.

```
GET /rest/v1/activities/pagingtoken.json?sinceDatetime=2014-10-06T13:22:17-08:00
```

```json
{
    "requestId": "1607c#14884f3e74e",
    "success": true,
    "nextPageToken": "GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ"
}
```

El formato del parámetro `sinceDateTime` debe ajustarse a la notación de fecha estándar [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601). Para obtener los mejores resultados, utilice una fecha y hora completa que incluya la zona horaria. La zona horaria se puede representar como un desplazamiento con respecto a la zona GMT mediante el siguiente formato:

`yyyy-mm-ddThh:mm:ss+|-hh:mm`

O usar una &quot;Z&quot; mayúscula como abreviatura para representar UTC como esta:

`yyyy-mm-ddThh:mm:ssZ`

Ejemplos

`2016-09-15T15:53:00+05:00`

`2016-09-15T10:53:00Z`

Como `sinceDateTime` es un parámetro de consulta, debe estar codificado en la dirección URL.

A continuación, se proporciona la cadena `nextPageToken` a una llamada de [Obtener actividades de posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadActivitiesUsingGET), [Obtener cambios de posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadChangesUsingGET) o [Obtener posibles clientes eliminados](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getDeletedLeadsUsingGET), y las actividades se recuperan después de la fecha y hora proporcionadas para la API Obtener token de paginación.

```
GET /rest/v1/activities.json?nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ&activityTypeIds=1&activityTypeIds=12
```

## Basado en puestos

El segundo tipo de token de paginación se puede devolver mediante cualquier llamada de recuperación por lotes a una API de base de datos de posibles clientes. Este tipo de token de paginación es similar en concepto a un cursor de base de datos que permite el recorrido de registros. Por ejemplo, una llamada Obtener posibles clientes por tipo de filtro puede representar un conjunto mayor que el tamaño de lote dado, normalmente el máximo y el valor predeterminado de 300. Cuando hay más resultados, el campo moreResult es true en la respuesta y se devuelve `nextPageToken`. Para recuperar los registros adicionales en el conjunto de resultados, realice una llamada adicional que incluya `nextPageToken` con el valor recibido de la respuesta anterior en la nueva llamada. La respuesta resultante devolverá la siguiente página del conjunto de resultados.
