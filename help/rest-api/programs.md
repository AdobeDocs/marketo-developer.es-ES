---
title: Programas
feature: REST API, Programs
description: Crear y editar información del programa.
exl-id: 30700de2-8f4a-4580-92f2-7036905deb80
source-git-commit: f28aa6daf53063381077b357061fe7813c64b5de
workflow-type: tm+mt
source-wordcount: '848'
ht-degree: 2%

---

# Programas

[Referencia de extremo de programas](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs)

Los programas son un componente organizativo central de las actividades de marketing de Marketo. Pueden ser el elemento principal de la mayoría de los tipos de recursos y permiten realizar un seguimiento de la pertenencia y el éxito de los posibles clientes en el contexto de iniciativas de marketing individuales. Los programas pueden ser principales para todo tipo de registros, excepto para el programa de aprendizaje, las plantillas de correo electrónico y los archivos.

## Tipos de programas

Existen cinco tipos principales de programas dentro de Marketo:

- Predeterminado
- Evento
- Evento con seminario web
- Participación
- Correo electrónico

Los programas de participación pueden ser programas principales entre sí, mientras que los programas predeterminados, los eventos y los eventos con seminario web solo pueden ser principales para los programas de correo electrónico.

Los programas siempre tienen un canal, derivan los posibles estados miembros del programa configurados del canal con el que se crearon, que se pueden recuperar con la API Obtener canales. Un programa también puede tener un conjunto de etiquetas asociadas. Las etiquetas son campos personalizables que se pueden configurar para que sean opcionales o necesarias para cualquier tipo determinado de programa, que tendrán un valor seleccionado de una lista configurada en Marketo Admin.

## Consulta

Los programas siguen el patrón estándar para las consultas de recursos con una opción adicional para consultar por tipo de etiqueta y valores. Las etiquetas y los valores disponibles se pueden recuperar con [Obtener tipos de etiquetas](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tags/operation/getTagTypesUsingGET).

### Por ID

El extremo [Get Program by Id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) requiere un parámetro de ruta de acceso `id`.

El identificador de programa se puede obtener de la dirección URL del programa en la interfaz de usuario, donde la dirección URL será similar a `https://app-\*\*\*.marketo.com/#PG1001A1`. En esta dirección URL, `id` es 1001. Siempre será entre el primer conjunto de cartas de la dirección URL y el segundo conjunto de cartas.

```
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

El extremo [Obtener programa por nombre](https://developer.adobe.com/marketo-apis/api/asset/) requiere un parámetro de consulta `name`. Los parámetros de consulta booleanos opcionales son `includeTags` y `includeCosts`, que se utilizan para devolver etiquetas de programa y costos de programa respectivamente.

```
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

El extremo [Obtener programas](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) le permite buscar programas.

El parámetro opcional `status` le permite filtrar el estado del programa. Este parámetro solo se aplica a los programas de participación y correo electrónico. Los valores posibles son &quot;activado&quot; y &quot;desactivado&quot; para los programas de participación y &quot;desbloqueado&quot; para los programas de correo electrónico.

El parámetro opcional `maxReturn` controla el número de programas que se van a devolver (el máximo es 200, el valor predeterminado es 20). El parámetro `offset` opcional usado para los resultados de paginación (el valor predeterminado es 0).

Tenga en cuenta que este extremo no devuelve las etiquetas asociadas a un programa. Las etiquetas de programa se pueden recuperar mediante [Obtener programas por identificador](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByIdUsingGET) o [Obtener programas por nombre](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByNameUsingGET).

```
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

Los parámetros `earliestUpdatedAt` y `latestUpdatedAt` de nuestro extremo [Obtener programas](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) le permiten establecer marcas de agua de fecha y hora bajas y altas para los programas que devuelven y que se actualizaron o crearon inicialmente dentro del intervalo dado.

```
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

El extremo [Obtener programas por etiqueta](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramListByTagUsingGET) recupera una lista de programas que coinciden con el tipo de etiqueta y los valores de etiqueta proporcionados.

Hay dos parámetros obligatorios, `tagType`, que es el tipo de etiqueta con la que filtrar, y `tagValue`, que es el valor de etiqueta con el que filtrar.  Hay un parámetro entero opcional `maxReturn` que controla el número de programas que se van a devolver (el máximo es 200, el valor predeterminado es 20) y un parámetro entero opcional `offset` utilizado para los resultados de paginación (el valor predeterminado es 0).  Los resultados se devuelven en orden aleatorio.

```
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

[Al crear](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/createProgramUsingPOST) y [actualizar](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/updateProgramUsingPOST) programas se sigue el patrón de recursos estándar y tiene `folder`, `name`, `type` y `channel` como parámetros necesarios, siendo `description`, `costs` y `tags` opcionales. El canal y el tipo solo se pueden configurar al crear el programa. Solo la descripción, el nombre, `tags` y `costs` se pueden actualizar después de la creación, con un parámetro `costsDestructiveUpdate` adicional permitido. Si se pasa `costsDestructiveUpdate` como verdadero, todos los costos existentes se borrarán y se reemplazarán con los costos incluidos en la llamada. Tenga en cuenta que las etiquetas pueden ser necesarias para algunos tipos de programas en algunas suscripciones, pero esto depende de la configuración y primero debe comprobarse con Obtener etiquetas para ver si hay requisitos específicos de instancias.

Al crear o actualizar un programa de correo electrónico, `startDate` y `endDate` también se pueden pasar como fecha/hora UTC:

`"startDate": "2022-10-19T15:00:00.000Z"`
`"endDate": "2022-10-19T15:00:00.000Z"`

### Crear

```
POST /rest/asset/v1/programs.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Al actualizar los costos del programa, para anexar nuevos costos, simplemente agréguelos a la matriz `costs`. Para realizar una actualización destructiva, pase los nuevos costos junto con el parámetro `costsDestructiveUpdate` establecido en `true`. Para borrar todos los costos de un programa, no pase un parámetro `costs` y simplemente pase `costsDestructiveUpdate` establecido en `true`.

```
POST /rest/asset/v1/program/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Los programas de correo electrónico pueden aprobarse o desaprobarse de forma remota, lo que hará que el programa se ejecute en la fecha de inicio determinada y finalice en la fecha de finalización determinada. Ambos deben configurarse para aprobar el programa, así como tener un correo electrónico y una lista inteligente válidos y aprobados configurados a través de la interfaz de usuario.

### Aprobar

```
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

```
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

[Los programas de clonación](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/cloneProgramUsingPOST) siguen el patrón de recursos estándar con el nuevo nombre y la nueva carpeta como parámetros necesarios y una descripción opcional.  El parámetro `name` debe ser único globalmente y no puede exceder los 255 caracteres.  El parámetro `folder` es la carpeta principal.  El atributo de tipo de parámetro `folder` debe establecerse en &quot;Folder&quot; y la carpeta de destino debe estar en el mismo espacio de trabajo que el programa que se está clonando.

Los programas que contienen determinados tipos de recursos no se pueden clonar mediante esta API, incluidas las notificaciones push, los mensajes en la aplicación, los informes y Social Assets. Los programas en la aplicación no se pueden clonar mediante esta API.

```
POST /rest/asset/v1/program/{id}/clone.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

```
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
