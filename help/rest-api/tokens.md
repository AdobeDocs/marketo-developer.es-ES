---
title: Tókenes
feature: REST API, Tokens
description: Administre Mis tokens de Marketo con la API de REST de recursos. Consulte Tipos de datos admitidos, obtener por carpeta o programa, crear o actualizar mediante POST con codificación de formulario y eliminar por nombre.
exl-id: 4f8d87d7-ba2a-4c90-8b39-4d20679d404a
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '320'
ht-degree: 4%

---

# Tókenes

[Referencia de extremo de token](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens)

Los tokens de Marketo son cadenas especiales similares a los códigos abreviados, que se reemplazan por un fragmento de datos independiente en el tiempo de ejecución. Hay varios tipos de tokens disponibles en Marketo, pero solo Mis tokens se pueden editar mediante la API. Mis tokens son tokens secundarios que son locales de una carpeta o programa en particular. Los tokens se pueden leer, crear y eliminar mediante la API.

## Tipo de datos

Los tokens se pueden crear con los siguientes tipos de datos:

| Tipo | Descripción |
|---------------|----------------------------------------------------|
| fecha | Valor de fecha del formulario &quot;aaaa-MM-dd&quot; |
| número | Un número entero o de coma flotante |
| Texto enriquecido | Una cadena de HTML |
| Puntaje | Un entero de 32 bits con signo |
| campaña de sfdc | Se utiliza en la integración de administración de campañas de Salesforce |
| texto | Una cadena de texto |

Son los únicos tipos de datos que se pueden utilizar al crear un token mediante API.

## Consulta

[Obtener tokens por id. de carpeta](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/getTokensByFolderIdUsingGET) toma un `id` como parámetro de ruta de acceso de un tipo de programa o carpeta. El parámetro `folderType` especifica este tipo.

```curl
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

El extremo [Create Token](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/addTokenTOFolderUsingPOST) crea tokens o, si existen, los actualiza con los valores enviados. Los tokens se crean en el contexto de una carpeta o un programa. El parámetro de ruta de acceso `id` requerido es el identificador de la carpeta a la que se asociará el token. `name`, `type`, `value` y `folderType` son todos parámetros obligatorios del token. Los datos se pasan como POST x-www-form-urlencoded, no como JSON. El campo `name` del token no puede superar los 50 caracteres.

```
POST /rest/asset/v1/folder/{id}/tokens.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

[Eliminar token por nombre](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/deleteTokenByNameUsingPOST) toma un identificador como parámetro de ruta de acceso de un tipo de programa o carpeta. El parámetro `folderType` especifica este tipo. Los tokens se eliminan en función de su carpeta principal, `name`, y `type` del token, cada uno de los cuales es obligatorio. Los datos se pasan como POST x-www-form-urlencoded, no como JSON.

```
POST /rest/asset/v1/folder/{id}/tokens/delete.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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
