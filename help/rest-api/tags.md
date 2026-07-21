---
title: Etiquetas
feature: REST API, Tags
description: Consulte los tipos de etiquetas, obtenga valores permitidos por nombre, actualice o elimine etiquetas de programa en Marketo mediante la API de recursos REST, con ejemplos de solicitudes.
exl-id: 64731d1a-a749-4d6f-b336-16c733d002f0
TQID: https://experienceleague.adobe.com/zjdyfoofVWytE0Q-K4lk598jmleTSFOD7tSRqeAHsjk
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 227
ht-degree: 2%

---

# Etiquetas

[Referencia de extremo de etiquetas](https://developer.adobe.com/marketo-apis/api/asset#tag/Tags)

Las etiquetas son campos definidos por el usuario para programas. Una etiqueta se puede aplicar a uno o varios tipos de programas y puede ser obligatoria u opcional. Una etiqueta también puede definir una lista de valores permitidos entre los que los usuarios deben seleccionar.

## Consulta

Consulte las etiquetas con el patrón de recursos estándar. Las etiquetas no tienen un extremo By Id. Para recuperar los valores permitidos para una etiqueta, consulte la etiqueta por nombre.

### Obtener etiquetas

```http
GET /rest/asset/v1/tagTypes.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "1488a#1504ecfccf8",
    "result": [
        {
            "tagType": "AAA1 Required Tag Type",
            "applicableProgramTypes": "[program,email_batch,nurture,event,webinar]",
            "required": true
        },
        {
            "tagType": "AAA2 Required Event Tag Type",
            "applicableProgramTypes": "[event]",
            "required": true
        },
        {
            "tagType": "AAA3 Not Required Tag Type",
            "applicableProgramTypes": "[program,email_batch,nurture,event,webinar]",
            "required": false
        }
    ]
}
```

### Por nombre

```http
GET /rest/asset/v1/tagType/byName.json?name=AAA1 Required Tag Type
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "8a44#1504ed0da2f",
    "result": [
        {
            "tagType": "AAA1 Required Tag Type",
            "applicableProgramTypes": "[program,email_batch,nurture,event,webinar]",
            "required": true,
            "allowableValues": "[AAA1 RT1, AAA1 RT2, AAA1 RT3, AAA1 RT4]"
        }
    ]
}
```

## Actualización

Use el extremo [Actualizar etiqueta de programa](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/updateProgramUsingPOST) para actualizar el valor de un tipo de etiqueta. Todos los parámetros son obligatorios:

- El parámetro de ruta de acceso `id` especifica el identificador de programa.
- El parámetro de ruta de acceso `tagType` especifica el tipo de etiqueta que se actualizará.
- El parámetro de consulta `tagValue` especifica el nuevo valor.

```http
POST /rest/asset/v1/program/{id}/tag/{tagType}.json?tagValue=David
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "fd84#17f84a885a6",
    "warnings": [],
    "result": [
        {
            "id": 1067
        }
    ]
}
```

Para actualizar varias etiquetas, use el extremo [Actualizar metadatos del programa](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/updateProgramUsingPOST). Vea el ejemplo en la [sección de actualización de programas](programs.md#update).

## Eliminar

Use el extremo [Eliminar etiqueta de programa](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/deleteProgramUsingPOST) para eliminar un tipo de etiqueta no obligatorio. El parámetro de ruta de acceso `id` especifica el id. de programa y el parámetro de ruta de acceso `tagType` especifica el tipo de etiqueta que se va a eliminar.

```http
POST /rest/asset/v1/program/{id}/tag/{tagType}/delete.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "d998#17f84ad36a7",
    "warnings": [],
    "result": [
        {
            "id": 1067
        }
    ]
}
```
