---
title: Uso
feature: REST API
description: Supervise el uso y los errores de la API de REST de Marketo con puntos finales de estado diarios y de los últimos 7 días, incluidos los recuentos por usuario y los totales de código de error.
source-git-commit: 73fa4c85ecabd4cfd24bc6591aad11dc4e75010a
workflow-type: tm+mt
source-wordcount: '393'
ht-degree: 9%

---

# Uso

[Referencia de extremo de uso](https://developer.adobe.com/marketo-apis/api/mapi#tag/Usage)

Las API de uso proporcionan un resumen del consumo de API de REST y la actividad de error de la suscripción. Estos extremos son útiles para monitorizar las integraciones, rastrear el volumen diario de llamadas e identificar las tendencias de error a lo largo del tiempo.

Los datos de uso incluyen un recuento total de llamadas de API y un desglose por usuario. Los datos de error incluyen un recuento total de errores y un desglose por código de error.

Las API Usage utilizan el mismo método de autenticación que otras API REST de Marketo. Pase el token de acceso en el encabezado `Authorization: Bearer {accessToken}`.

## Puntos de conexión

| Método | Ruta | Descripción |
| --- | --- | --- |
| GET | `/rest/v1/stats/usage.json` | Recupera el uso de la API para el día actual. |
| GET | `/rest/v1/stats/usage/last7days.json` | Recupera el uso de la API durante los últimos 7 días. |
| GET | `/rest/v1/stats/errors.json` | Recupera los errores de API del día actual. |
| GET | `/rest/v1/stats/errors/last7days.json` | Recupera errores de API de los últimos 7 días. |

## Uso diario

Recupera el uso de la API para el día actual.

```
GET /rest/v1/stats/usage.json
```

```json
{
   "requestId": "5f7f#17d7d8f2b6f",
   "success": true,
   "result": [
      {
         "date": "2015-10-17",
         "total": 1120,
         "users": [
            {
               "userId": "some.body@yahoo.com",
               "count": 200
            },
            {
               "userId": "some.body@marketo.com",
               "count": 200
            },
            {
               "userId": "some.body@gmail.com",
               "count": 720
            }
         ]
      }
   ]
}
```

Cada objeto de la matriz `result` contiene un día de totales de uso y un desglose por usuario.

## Últimos 7 días de uso

Recupera el uso de la API durante los últimos 7 días. Cada elemento de la matriz `result` representa un día.

```
GET /rest/v1/stats/usage/last7days.json
```

## Errores diarios

Recupera los errores de API del día actual.

```
GET /rest/v1/stats/errors.json
```

```json
{
   "requestId": "5f7f#17d7d8f2b6f",
   "success": true,
   "result": [
      {
         "date": "2015-10-17",
         "total": 73,
         "errors": [
            {
               "errorCode": "604",
               "count": 1
            },
            {
               "errorCode": "609",
               "count": 56
            },
            {
               "errorCode": "610",
               "count": 16
            }
         ]
      }
   ]
}
```

Cada objeto de la matriz `result` contiene un día de totales de errores y un desglose por código de error.

## Errores de los últimos 7 días

Recupera errores de API de los últimos 7 días. Cada elemento de la matriz `result` representa un día.

```
GET /rest/v1/stats/errors/last7days.json
```

## Miembros de respuesta

### Objeto de resultado de uso

| Nombre | Tipo de datos | Descripción |
| --- | --- | --- |
| `date` | Cadena | La fecha del resumen de uso en formato `YYYY-MM-DD`. |
| `total` | Número entero | Número total de llamadas de API para ese día. |
| `users` | Matriz | Lista de recuentos de uso por usuario para ese día. |

### Objeto de usuario de uso

| Nombre | Tipo de datos | Descripción |
| --- | --- | --- |
| `userId` | Cadena | Identificador de usuario de API. |
| `count` | Número entero | Número de llamadas de API realizadas por ese usuario durante el día. |

### Objeto de resultado de error

| Nombre | Tipo de datos | Descripción |
| --- | --- | --- |
| `date` | Cadena | La fecha del resumen del error en formato `YYYY-MM-DD`. |
| `total` | Número entero | Número total de errores de API para ese día. |
| `errors` | Matriz | Lista de recuentos de código de error por día. |

### Objeto de error

| Nombre | Tipo de datos | Descripción |
| --- | --- | --- |
| `errorCode` | Cadena | Código de error de Marketo. |
| `count` | Número entero | Número de veces que se produjo ese error durante el día. |

## Notas

Cada uno de los usuarios de la API se comunica individualmente en la respuesta de uso. Dividir las integraciones entre usuarios de API independientes facilita la identificación de qué servicio consume cuotas y produce errores.
