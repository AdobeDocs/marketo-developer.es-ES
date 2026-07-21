---
title: Obtener datos del visitante
description: Obtenga identificación de visitantes en tiempo real mediante la API de contexto de usuario de RTP con parámetros, ejemplo de llamada de retorno y respuestas de muestra para segmentos, ABM y ubicación.
feature: Javascript
exl-id: 39a2446d-8a31-461e-bbe6-a7edf24b4d52
TQID: https://experienceleague.adobe.com/B-JMACtMs3aRVsb1eJKaRoQGgVKB6MTbd0KBoZ7Ay6k
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
  - id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 214
ht-degree: 4%

---

# Obtener datos del visitante

Utilice este método para obtener datos de identificación del visitante en tiempo real.

- Debe ser cliente de Web Personalization y tener la etiqueta [RTP implementada](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) en el sitio antes de usar la API de contexto de usuario.
- RTP no admite listas de cuentas con nombre de marketing basado en cuentas. Las listas ABM y el código solo pertenecen a las listas de cuentas cargadas (archivos CSV) administradas dentro de RTP.

Si se produce un error, el JSON de respuesta incluye un mensaje de error. Si la API devuelve un código 500, póngase en contacto con el servicio de asistencia y proporcione la solicitud que provocó el error.

| Parámetro | Opcional/Requerida | Tipo | Descripción |
| --- | --- | --- | --- |
| `get` | Obligatorio | Cadena | Acción de método. |
| `visitor` | Obligatorio | Cadena | Nombre del método. |
| `callback` | Obligatorio | Función | Función de llamada de retorno que se activará para cada campaña devuelta. |

## Ejemplos

El ejemplo siguiente obtiene datos de identificación del visitante.

```javascript
function callbackFunction() {
    console.log('RTP is awesome!');
}
rtp('get', 'visitor', callbackFunction);
```

Respuesta con coincidencia de segmentos:

La siguiente respuesta incluye `matchedSegments` porque el visitante coincidió con los segmentos en tiempo real antes de la llamada a la API Obtener datos del visitante.

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

La siguiente respuesta no incluye `matchedSegments` porque el visitante no coincidió con ningún segmento en tiempo real antes de la llamada a la API Obtener datos del visitante.

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
