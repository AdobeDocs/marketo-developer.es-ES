---
title: Tókenes
feature: REST API, Tokens
description: Administre Mis tokens de Marketo con la API de REST de recursos. Consulte Tipos de datos admitidos, obtener por carpeta o programa, crear o actualizar mediante POST con codificación de formulario y eliminar por nombre.
exl-id: 4f8d87d7-ba2a-4c90-8b39-4d20679d404a
TQID: https://experienceleague.adobe.com/uqOpu2vDuiQiZhILKuxZJQGadd0K14zwIaAdmNfK1-I
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 290
ht-degree: 4%

---

# Tókenes

[Referencia de extremo de token](https://developer.adobe.com/marketo-apis/api/asset#tag/Tokens)

Los tokens son cadenas que Marketo reemplaza con otros datos en tiempo de ejecución. La API solo puede editar Mis tokens, que son tokens secundarios locales de una carpeta o programa.

Utilice la API de tokens para leer, crear, actualizar y eliminar mis tokens.

## Tipo de datos

Los tokens se pueden crear con los siguientes tipos de datos:

| Tipo | Descripción |
| --- | --- |
| fecha | Valor de fecha del formulario &quot;aaaa-MM-dd&quot; |
| número | Un número entero o de coma flotante |
| Texto enriquecido | Una cadena de HTML |
| Puntaje | Un entero de 32 bits con signo |
| campaña de sfdc | Se utiliza en la integración de administración de campañas de Salesforce |
| texto | Una cadena de texto |

La API solo admite estos tipos de datos al crear un token.

## Consulta

[Obtener tokens por identificador de carpeta](https://developer.adobe.com/marketo-apis/api/asset#tag/Tokens/operation/getTokensByFolderIdUsingGET) toma el identificador de un programa o carpeta como parámetro de ruta de acceso. Utilice el parámetro `folderType` para especificar el tipo.

```http
GET /rest/asset/v1/folder/{id}/tokens.json?folderType=Folder
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "4fbe#14e27fc9bbf",
    "result": [
        {
            "folder": {
                "type": "Folder",
                "value": 416
            },
            "tokens": [
                {
                    "name": "AprilFool - deverly",
                    "type": "date",
                    "value": "2015-04-01",
                    "computedUrl": "https://app-abm.marketo.com/#MF1047C3"
                }
            ]
        }
    ]
}
```

## Crear y actualizar

El extremo [Create Token](https://developer.adobe.com/marketo-apis/api/asset#tag/Tokens/operation/addTokenTOFolderUsingPOST) crea un token o actualiza un token existente con los valores enviados. Los tokens pertenecen a una carpeta o programa.

El parámetro de ruta de acceso `id` identifica la carpeta principal. Se requieren los parámetros `name`, `type`, `value` y `folderType`. Pase los datos como POST `x-www-form-urlencoded`, no como JSON. El token `name` no puede superar los 50 caracteres.

```http
POST /rest/asset/v1/folder/{id}/tokens.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=April Fools&type=date&value=2015-04-01&folderType=Folder
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "e3c2#14e280db5dc",
    "result": [
        {
            "folder": {
                "type": "Folder",
                "value": 416
            },
            "tokens": [
                {
                    "name": "April Fools",
                    "type": "date",
                    "value": "2015-04-01",
                    "computedUrl": "https://app-abm.marketo.com/#MF1047C3"
                }
            ]
        }
    ]
}
```

## Eliminar

[Eliminar token por nombre](https://developer.adobe.com/marketo-apis/api/asset#tag/Tokens/operation/deleteTokenByNameUsingPOST) toma el identificador de un programa o carpeta como parámetro de ruta de acceso. Use `folderType` para especificar el tipo.

Se requiere la carpeta principal, el token `name` y el token `type`. Pase los datos como POST `x-www-form-urlencoded`, no como JSON.

```http
POST /rest/asset/v1/folder/{id}/tokens/delete.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=AprilFool - deverly&type=date&folderType=Program
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "12ed2#14e2800f89c",
    "result": [
        {
            "id": 416
        }
    ]
}
```
