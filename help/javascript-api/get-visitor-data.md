---
title: Obtener datos del visitante
description: Obtenga identificación de visitantes en tiempo real mediante la API de contexto de usuario de RTP con parámetros, ejemplo de llamada de retorno y respuestas de muestra para segmentos, ABM y ubicación.
feature: Javascript
exl-id: 39a2446d-8a31-461e-bbe6-a7edf24b4d52
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 4%

---

# Obtener datos del visitante

Este método se utiliza para obtener datos de identificación de visitantes en tiempo real.

- Debe convertirse en cliente de Web Personalization y tener la etiqueta [RTP implementada](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) en su sitio antes de usar la API de contexto de usuario.
- RTP no admite listas de cuentas con nombre de marketing basado en cuentas. Las listas ABM y el código solo pertenecen a las listas de cuentas cargadas (archivos CSV) administradas dentro de RTP.

Si se produce un error, aparecerá un mensaje de error como parte de la respuesta JSON. Si devuelve un código 500, póngase en contacto con el servicio de asistencia técnica con la solicitud que ha realizado.

| Parámetro | Opcional/Requerida | Tipo | Descripción |
|---|---|---|---|
| `get` | Obligatorio | Cadena | Acción de método. |
| `visitor` | Obligatorio | Cadena | Nombre del método. |
| `callback` | Obligatorio | Función | Función de llamada de retorno que se activará para cada campaña devuelta. |

## Ejemplos

Obtener datos de identificación del visitante:

```javascript
function callbackFunction() {
    console.log('RTP is awesome!');
}
rtp('get', 'visitor', callbackFunction);
```

Respuesta con coincidencia de segmentos:

A continuación se muestra una respuesta de ejemplo que se devuelve en caso de que el visitante coincida con segmentos en tiempo real antes de la llamada a la API de obtención de datos del visitante.

```json
{
    "status": 200,
    "results": {
        "matchedSegments": [
            {
                "name": "first click",
                "id": 177
            }
        ],
        "abm": [
            {
                "code": 4,
                "name": "abm_saleforce_customers"
            },
            {
                "code": 5,
                "name": "abm_top_customers"
            }
        ],
        "org": "Marketo",
        "location": {
            "country": "United States",
            "city": "San Mateo",
            "state": "CA"
        },
        "industries": [
            "Software & Internet"
        ],
        "isp": false
    }
}
```

Respuesta sin coincidencia de segmentos:

A continuación se muestra una respuesta de ejemplo que se devuelve en caso de que el visitante no coincidiera con ningún segmento en tiempo real antes de la llamada a la API de obtención de datos del visitante.

```json
{
    "status": 200,
    "results": {
        "abm": [
            {
                "code": 4,
                "name": "abm_saleforce_customers"
            },
            {
                "code": 5,
                "name": "abm_top_customers"
            }
        ],
        "org": "Marketo",
        "location": {
            "country": "United States",
            "city": "San Mateo",
            "state": "CA"
        },
        "industries": [
            "Software & Internet"
        ],
        "isp": false
    }
}
```
