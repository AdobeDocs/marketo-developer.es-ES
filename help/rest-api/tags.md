---
title: Etiquetas
feature: REST API, Tags
description: Administre etiquetas para programas en Marketo.
exl-id: 64731d1a-a749-4d6f-b336-16c733d002f0
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 2%

---

# Etiquetas

[Referencia de extremo de etiquetas](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tags)

Las etiquetas son campos definidos por el usuario para programas. Cada etiqueta puede aplicarse a uno o más tipos de programa y puede ser obligatoria u opcional, según cómo se haya definido la etiqueta. Las etiquetas también pueden proporcionar una lista de valores permitidos que deben seleccionarse entre para su uso.

## Consulta

Las etiquetas se consultan con el patrón de recursos estándar, pero no tienen un punto final para Por ID. La lista de valores permitidos para una etiqueta solo se devuelve cuando la etiqueta se consulta por el nombre.

### Obtener etiquetas

```
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

```
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

El punto de conexión [Actualizar etiqueta de programa](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/updateProgramUsingPOST) le permite actualizar el valor de un tipo de etiqueta determinado. El extremo toma un parámetro de ruta de acceso `id` y `tagType` que especifican el identificador de programa y el tipo de etiqueta que se va a actualizar. Se usa un parámetro de consulta `tagValue` para especificar el nuevo valor del tipo de etiqueta. Todos los parámetros son obligatorios.

```
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

Las etiquetas se pueden actualizar de forma masiva mediante el punto de conexión [Actualizar metadatos del programa](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/updateProgramUsingPOST). Se puede encontrar un ejemplo de esto [aquí](programs.md#update).

## Eliminar

El punto de conexión [Eliminar etiqueta de programa](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/deleteProgramUsingPOST) le permite eliminar un tipo de etiqueta no obligatorio. El extremo toma `id` y `tagType` parámetros de ruta que especifican el id. de programa y el tipo de etiqueta que se va a eliminar.

```
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
