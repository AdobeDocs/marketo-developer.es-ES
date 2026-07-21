---
title: Programas
feature: REST API, Programs
description: Guía de programas de Marketo para la API de REST de recursos que cubre tipos, canales, etiquetas, estados de miembro y extremos para obtener por ID o nombre, examinar y filtrar por estado.
exl-id: 30700de2-8f4a-4580-92f2-7036905deb80
TQID: https://experienceleague.adobe.com/5ILyahSn3Pp-lF6YPogVnkXjXP-QLtEmyLm7iKMIgo0
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: a7170d27-32ab-462b-a333-269abc654483id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45id: e64968b2-4ee5-47f9-8cae-0588f184b9ebid: f82558ea-6af5-44eb-a424-5b3389abb0a3
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: ebde5b41-29c9-4f5e-9ef6-1197e85409e3
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 741
ht-degree: 2%

---

# Programas

[Referencia de extremo de programas](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs)

Los programas organizan las actividades de marketing de Marketo y realizan un seguimiento de los posibles clientes y los resultados de las iniciativas de marketing individuales. Un programa puede contener la mayoría de los tipos de recursos, excepto páginas de aterrizaje, plantillas de correo electrónico y archivos.

## Tipos de programas

Existen cinco tipos principales de programas dentro de Marketo:

- Predeterminado
- Evento
- Evento con seminario web
- Participación
- Correo electrónico

Los programas de participación pueden contener cualquier otro tipo de programa. Los programas predeterminados, de eventos y de eventos con seminarios web solo pueden contener programas de correo electrónico.

Cada programa tiene un canal. El canal define los Estados miembros de programa disponibles y se puede recuperar con la API de obtención de canales.

Un programa también puede tener etiquetas. Las etiquetas son campos personalizables que pueden ser opcionales o necesarios para un tipo de programa. Cada etiqueta utiliza un valor de una lista configurada en Marketo Admin.

## Consulta

Consultar programas por ID, nombre, exploración o tipo y valor de etiqueta. Use [Obtener tipos de etiquetas](https://developer.adobe.com/marketo-apis/api/asset#tag/Tags/operation/getTagTypesUsingGET) para recuperar las etiquetas y los valores disponibles.

### Por ID

El extremo [Get Program by Id](https://developer.adobe.com/marketo-apis/api/asset#tag/Sales-Persons/operation/describeUsingGET_5) requiere un parámetro de ruta de acceso `id`.

Puede obtener el identificador de programa de su URL de interfaz de usuario, como `https://app-\*\*\*.marketo.com/#PG1001A1`. En este ejemplo, el identificador es `1001`, entre el primer y el segundo conjunto de cartas.

```http
GET /rest/asset/v1/program/{id}.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "948f#14db037ec71",
    "result": [
        {
            "id": 1107,
            "name": "AAA2QueryProgramName",
            "description": "AssetAPI: getProgram tests",
            "createdAt": "2015-05-21T22:45:13Z+0000",
            "updatedAt": "2015-05-21T22:45:13Z+0000",
            "url": "https://app-devlocal1.marketo.com/#PG1107A1",
            "type": "Default",
            "channel": "Online Advertising",
            "folder": {
                "type": "Folder",
                "value": 1910,
                "folderName": "ProgramQueryTestFolder"
            },
            "status": "",
            "workspace": "Default",
            "tags": [
                {
                    "tagType": "AAA1 Required Tag Type",
                    "tagValue": "AAA1 RT1"
                }
            ],
            "costs": null,
            "headStart": false
        }
    ]
}
```

### Por nombre

El extremo [Obtener programa por nombre](https://developer.adobe.com/marketo-apis/api/asset) requiere un parámetro de consulta `name`. Establezca los parámetros booleanos opcionales `includeTags` y `includeCosts` para devolver etiquetas y costes, respectivamente.

```http
GET /rest/asset/v1/program/byName.json?name=TestProgramName&includeTags=true
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "16026#14db03e070c",
    "result": [
        {
            "id": 1107,
            "name": "AAA2QueryProgramName",
            "description": "AssetAPI: getProgram tests",
            "createdAt": "2015-05-21T22:45:13Z+0000",
            "updatedAt": "2015-05-21T22:45:13Z+0000",
            "url": "https://app-devlocal1.marketo.com/#PG1107A1",
            "type": "Default",
            "channel": "Online Advertising",
            "folder": {
                "type": "Folder",
                "value": 1910,
                "folderName": "ProgramQueryTestFolder"
            },
            "status": "",
            "workspace": "Default",
            "tags": [
                {
                    "tagType": "AAA1 Required Tag Type",
                    "tagValue": "AAA1 RT1"
                }
            ],
            "costs": null,
            "headStart": false
        }
    ]
}
```

### Examinar

Use el extremo [Obtener programas](https://developer.adobe.com/marketo-apis/api/asset#tag/Sales-Persons/operation/describeUsingGET_5) para examinar programas.

El parámetro opcional `status` filtra los programas de participación y correo electrónico por estado. Los valores válidos son `on` y `off` para los programas de participación y `unlocked` para los programas de correo electrónico.

El parámetro opcional `maxReturn` controla el número de programas devueltos. El valor predeterminado es 20 y el máximo es 200. Utilice el parámetro opcional `offset` para la paginación; el valor predeterminado es 0.

Este extremo no devuelve etiquetas de programa. Recupere etiquetas con [Obtener programas por identificador](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/getProgramByIdUsingGET) o [Obtener programas por nombre](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/getProgramByNameUsingGET).

```http
GET /rest/asset/v1/programs.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "7a39#1511bf8a41c",
    "result": [
        {
            "id": 1035,
            "name": "clone it",
            "description": "",
            "createdAt": "2015-11-18T15:25:35Z+0000",
            "updatedAt": "2015-11-18T15:25:46Z+0000",
            "url": "https://app-devlocal1.marketo.com/#NP1035A1",
            "type": "Engagement",
            "channel": "Nurture",
            "folder": {
                "type": "Folder",
                "value": 28,
                "folderName": "Nurturing"
            },
            "status": "on",
            "workspace": "Default",
            "headStart": false
        },
        {
            "id": 1032,
            "name": "email prog",
            "description": "",
            "createdAt": "2015-11-18T14:56:28Z+0000",
            "updatedAt": "2015-11-18T14:56:28Z+0000",
            "url": "https://app-devlocal1.marketo.com/#EBP1032A1",
            "type": "Email",
            "channel": "Email Send",
            "folder": {
                "type": "Folder",
                "value": 26,
                "folderName": "Data Management"
            },
            "status": "unlocked",
            "workspace": "Default",
            "headStart": false
        }
    ]
}
```

### Por intervalo de fechas

Use los parámetros `earliestUpdatedAt` y `latestUpdatedAt` con [Obtener programas](https://developer.adobe.com/marketo-apis/api/asset#tag/Sales-Persons/operation/describeUsingGET_5) para establecer límites de fecha y hora bajos y altos. El extremo devuelve los programas creados o actualizados dentro del intervalo.

```http
GET /rest/asset/v1/programs.json?earliestUpdatedAt=2017-01-01T00:00:00-05:00&latestUpdatedAt=2017-01-30T00:00:00-05:00
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "1225a#15f82a83875",
    "warnings": [],
    "result": [
        {
            "id": 1070,
            "name": "Bulk Import - Test",
            "description": "",
            "createdAt": "2017-01-13T19:34:17Z+0000",
            "updatedAt": "2017-01-13T19:34:18Z+0000",
            "url": "https://app-abm.marketo.com/#PG1070A1",
            "type": "Default",
            "channel": "Content",
            "folder": {
                "type": "Folder",
                "value": 637,
                "folderName": "Avention"
            },
            "status": "",
            "workspace": "Default",
            "headStart": false
        },
        {
            "id": 1069,
            "name": "Program With Email",
            "description": "",
            "createdAt": "2017-01-03T22:53:14Z+0000",
            "updatedAt": "2017-01-03T22:53:15Z+0000",
            "url": "https://app-abm.marketo.com/#EBP1069A1",
            "type": "Email",
            "channel": "Email Send",
            "folder": {
                "type": "Folder",
                "value": 621,
                "folderName": "Smartling"
            },
            "status": "unlocked",
            "workspace": "Default",
            "headStart": false
        },
        {
            "id": 1071,
            "name": "Program with Guided Landing Page Template",
            "description": "",
            "createdAt": "2017-01-24T22:59:21Z+0000",
            "updatedAt": "2017-01-24T22:59:22Z+0000",
            "url": "https://app-abm.marketo.com/#PG1071A1",
            "type": "Default",
            "channel": "Content",
            "folder": {
                "type": "Folder",
                "value": 621,
                "folderName": "Smartling"
            },
            "status": "",
            "workspace": "Default",
            "headStart": false
        },
        {
            "id": 1047,
            "name": "ReachForce List Update",
            "description": "",
            "createdAt": "2016-05-24T19:38:35Z+0000",
            "updatedAt": "2017-01-13T19:28:09Z+0000",
            "url": "https://app-abm.marketo.com/#PG1047A1",
            "type": "Default",
            "channel": "Content",
            "folder": {
                "type": "Folder",
                "value": 407,
                "folderName": "Everly Tests"
            },
            "status": "",
            "workspace": "Default",
            "headStart": false
        }
    ]
}
```

### Por tipo de etiqueta

El extremo [Obtener programas por etiqueta](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/getProgramListByTagUsingGET) devuelve programas que coinciden con el tipo y valor de etiqueta especificados.

Se requieren los parámetros `tagType` y `tagValue`. El entero opcional `maxReturn` controla el número de programas devueltos; el valor predeterminado es 20 y el máximo es 200. Use el entero opcional `offset` para la paginación; el valor predeterminado es 0. Los resultados se devuelven en orden aleatorio.

```http
GET /rest/asset/v1/program/byTag.json?tagType=Presenter&tagValue=Dennis
```

```json
{
    "success" : true,
    "warnings" : [],
    "errors" : [],
    "requestId" : "13b6d#152b38d5be4",
    "result" : [{
            "id" : 1004,
            "name" : "It's a Program",
            "description" : "",
            "createdAt" : "2013-02-26T00:37:37Z+0000",
            "updatedAt" : "2013-03-11T15:32:02Z+0000",
            "url" : "https://app-sjst.marketo.com/#PG1004A1",
            "type" : "Default",
            "channel" : "Email Blast",
            "folder" : {
                "type" : "Folder",
                "value" : 38,
                "folderName" : "Test"
            },
            "status" : "",
            "workspace" : "Default",
            "tags" : [{
                    "tagType" : "Presenter",
                    "tagValue" : "Dennis"
                }
            ],
                        "headStart": false
    ]
}
```

## Crear y actualizar

[Para crear](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/createProgramUsingPOST) un programa se requiere `folder`, `name`, `type` y `channel`. Los parámetros opcionales son `description`, `costs` y `tags`. Algunas suscripciones requieren etiquetas para tipos de programas específicos. Utilice Obtener etiquetas para comprobar los requisitos de las instancias.

Al [actualizar](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/updateProgramUsingPOST), solo puede cambiar la descripción, el nombre, `tags` y `costs`. Solo puede establecer el canal y escribir durante la creación. Al establecer `costsDestructiveUpdate` en `true` se borran todos los costos existentes y se reemplazan por los costos incluidos en la solicitud.

Al crear o actualizar un programa de correo electrónico, `startDate` y `endDate` también se pueden pasar como fecha/hora UTC:

`"startDate": "2022-10-19T15:00:00.000Z"`
`"endDate": "2022-10-19T15:00:00.000Z"`

### Crear

```http
POST /rest/asset/v1/programs.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=API Test Program&folder={"id":1035,"type":"Folder"}&description=Sample API Program&type=Default&channel=Email Blast&costs=[{"startDate":"2015-01-01","cost":2000}]
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "d505#14d9bd96352",
    "result": [
        {
            "id": 1207,
            "name": "newProgram",
            "description": "This is a test",
            "createdAt": "2015-05-28T18:47:15Z+0000",
            "updatedAt": "2015-05-28T18:47:15Z+0000",
            "url": "https://app-devlocal1.marketo.com/#ME1207A1",
            "type": "Event",
            "channel": "channelOne",
            "folder": {
                "type": "Folder",
                "value": 59,
                "folderName": "blah blah"
            },
            "status": "",
            "workspace": "Default",
            "headStart": false
            "tags": null,
            "costs": [
                {
                    "startDate":"2015-01-01",
                    "cost":2000
                }
            ]
        }
    ]
}
```

### Actualización

Para anexar costos de programa, agréguelos a la matriz `costs`. Para reemplazar los costos existentes, pase los nuevos costos y establezca `costsDestructiveUpdate` en `true`. Para borrar todos los costos, omita `costs` y establezca `costsDestructiveUpdate` en `true`.

```http
POST /rest/asset/v1/program/{id}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
description=This is an updated description&name=Updated Program Name&costs=[{"startDate":"2016-01-01","cost":200,"note":"Google Adwords"}]
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "5c37#14db05608aa",
    "result": [
        {
            "id": 1110,
            "name": "Updated Program Name",
            "description": "This is a updated description",
            "createdAt": "2015-05-21T22:45:14Z+0000",
            "updatedAt": "2015-06-01T18:13:58Z+0000",
            "url": "https://app-devlocal1.marketo.com/#NP1110A1",
            "type": "Engagement",
            "channel": "Nurture",
            "folder": {
                "type": "Folder",
                "value": 1910,
                "folderName": "ProgramQueryTestFolder"
            },
            "status": "on",
            "workspace": "Default",
            "headStart": false,
            "tags": [
                {
                    "tagType": "AAA1 Required Tag Type",
                    "tagValue": "AAA1 RT1"
                },
                {
                    "tagType": "tagTypeOne",
                    "tagValue": "tagTypeValue1"
                }
            ],
            "costs": [
                {
                    "startDate": "2016-01-01",
                    "cost": 200,
                    "note": "Google Adwords"
                }
            ]
        }
    ]
}
```

## Aprobación

Puede aprobar o desaprobar programas de correo electrónico de forma remota. Un programa aprobado se ejecuta en su `startDate` y finaliza en su `endDate`.

Antes de la aprobación, establezca ambas fechas y configure un correo electrónico y una lista inteligente válidos y aprobados en la interfaz de usuario.

### Aprobar

```http
POST /rest/asset/v1/program/{id}/approve.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "16026#150b5bf7692",
    "result": [
        {
            "id": 11062
        }
    ]
}
```

### Desaprobar

```http
POST /rest/asset/v1/program/{id}/unapprove.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "16026#150b5bf7692",
    "result": [
        {
            "id": 11062
        }
    ]
}
```

## Clonar

[Clonar programas](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/cloneProgramUsingPOST) requiere un nombre nuevo y una carpeta principal. La descripción es opcional. `name` debe ser único a nivel global y no puede exceder los 255 caracteres.

Establezca el atributo type del parámetro `folder` en `Folder`. La carpeta de destino debe estar en el mismo espacio de trabajo que el programa de origen.

No puede utilizar esta API para clonar programas en la aplicación o programas que contengan notificaciones push, mensajes en la aplicación, informes o recursos sociales.

```http
POST /rest/asset/v1/program/{id}/clone.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=Cloned Program - PHP&folder={"id":5562,"type":"Folder"}&description=Description
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "3a7f#14db06990cc",
    "result": [
        {
            "id": 1221,
            "name": "cloneProgram",
            "description": "This is a description for the cloned program",
            "createdAt": "2015-06-01T18:36:57Z+0000",
            "updatedAt": "2015-06-01T18:36:57Z+0000",
            "url": "https://app-devlocal1.marketo.com/#PG1221A1",
            "type": "Default",
            "channel": "Blog",
            "folder": {
                "type": "Folder",
                "value": 59,
                "folderName": "blah blah"
            },
            "status": "",
            "workspace": "Default",
            "headStart": false
            "tags": null,
            "costs": null
        }
    ]
}
```

## Eliminar programa

La eliminación de programas sigue el patrón de eliminación de recursos estándar.

```http
POST /rest/asset/v1/program/{id}/delete.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "16501#14db042c6b7",
    "result": [
        {
            "id": 1109
        }
    ]
}
```
