---
title: "Tokens"
feature: REST API, Tokens
description: '"Administrar tokens en Marketo".'
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '297'
ht-degree: 4%

---


# Tokens

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
| campaña de sfdc | Se utiliza en la integración de gestión de campañas de Salesforce |
| texto | Una cadena de texto |


Son los únicos tipos de datos que se pueden utilizar al crear un token mediante API.

## Consulta

[Obtener tokens por ID de carpeta](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/getTokensByFolderIdUsingGET) toma un `id` como parámetro de ruta de un tipo Program o Folder. Este tipo lo especifica el `folderType` parámetro.

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

El [Crear token](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/addTokenTOFolderUsingPOST) el punto de conexión crea tokens o, si existen, los actualiza con los valores enviados. Los tokens se crean en el contexto de una carpeta o un programa. El requerido `id` parámetro de ruta es el id de la carpeta a la que se asociará el token. El `name`, `type`, `value`, y `folderType` son todos parámetros requeridos del token. Los datos se pasan como POST x-www-form-urlencoded, no como JSON. El `name` el campo del token no puede superar los 50 caracteres.

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

[Eliminar token por nombre](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/deleteTokenByNameUsingPOST) toma un id como parámetro de ruta de un tipo de programa o de carpeta. Este tipo lo especifica el `folderType` parámetro. Los tokens se eliminan en función de su carpeta principal, la `name`, y el `type` del token, cada uno de los cuales es obligatorio. Los datos se pasan como POST x-www-form-urlencoded, no como JSON.

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
