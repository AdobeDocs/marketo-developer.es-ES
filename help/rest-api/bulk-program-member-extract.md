---
title: Extracto de miembros de programa en masa
feature: REST API
description: Utilice las API de REST de extracción de miembros de programas en lote de Marketo para exportar registros de miembros grandes para ETL, almacenamiento de datos y archivado, con permisos y metadatos de campo.
exl-id: 6e0a6bab-2807-429d-9c91-245076a34680
TQID: https://experienceleague.adobe.com/w4qaVTKSe0EORaSiURB6WbJXi29JUdEgfkb2dnfuVFw
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1026
ht-degree: 5%

---

# Extracto de miembros de programa en masa

[Referencia de extremo de extracción de miembro de programa masivo](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Program-Members)

Las API de REST de extracción masiva de miembros de programas recuperan grandes conjuntos de registros de miembros de programas de Marketo. Utilice estas API para el intercambio continuo de datos entre Marketo y sistemas externos, ETL, almacenamiento de datos y archivado.

## Permisos

El usuario de API debe tener una función con el permiso de solo lectura del posible cliente, el permiso de lectura-escritura del posible cliente o ambos.

## Describir

Use [Describir miembro del programa](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/describeProgramMemberUsingGET2) para determinar qué campos están disponibles y recuperar sus metadatos. El atributo `name` contiene el nombre de campo de la API de REST.

```http
GET /rest/v1/programs/members/describe.json
```

```json
{
    "requestId": "f813#1791563c7cc",
    "result": [
        {
            "name": "API Program Membership",
            "description": "Map for API program membership fields",
            "createdAt": "2021-03-20T01:30:05Z",
            "updatedAt": "2021-03-20T01:30:05Z",
            "dedupeFields": [
                "leadId",
                "programId"
            ],
            "searchableFields": [
                [
                    "leadId"
                ],
                [
                    "myCustomField"
                ],
                [
                    "reachedSuccess"
                ],
                [
                    "statusName"
                ]
            ],
            "fields": [
                {
                    "name": "acquiredBy",
                    "displayName": "acquiredBy",
                    "dataType": "boolean",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "attendanceLikelihood",
                    "displayName": "attendanceLikelihood",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "createdAt",
                    "displayName": "createdAt",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "isExhausted",
                    "displayName": "isExhausted",
                    "dataType": "boolean",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "leadId",
                    "displayName": "leadId",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "membershipDate",
                    "displayName": "membershipDate",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "nurtureCadence",
                    "displayName": "nurtureCadence",
                    "dataType": "string",
                    "length": 4,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "program",
                    "displayName": "program",
                    "dataType": "string",
                    "length": 255,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "programId",
                    "displayName": "programId",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "reachedSuccess",
                    "displayName": "reachedSuccess",
                    "dataType": "boolean",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "reachedSuccessDate",
                    "displayName": "reachedSuccessDate",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "registrationLikelihood",
                    "displayName": "registrationLikelihood",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "statusName",
                    "displayName": "statusName",
                    "dataType": "string",
                    "length": 255,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "statusReason",
                    "displayName": "statusReason",
                    "dataType": "string",
                    "length": 255,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "trackName",
                    "displayName": "trackName",
                    "dataType": "string",
                    "length": 255,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "updatedAt",
                    "displayName": "updatedAt",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "waitlistPriority",
                    "displayName": "waitlistPriority",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "myCustomField",
                    "displayName": "myCustomField",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "registrationCode",
                    "displayName": "registrationCode",
                    "dataType": "string",
                    "length": 100,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "webinarUrl",
                    "displayName": "webinarUrl",
                    "dataType": "string",
                    "length": 2000,
                    "updateable": true,
                    "crmManaged": false
                }
            ]
        }
    ],
    "success": true
}
```

## Filtros

Las exportaciones de miembros de programa admiten varias opciones de filtro. Cuando un trabajo especifica varios tipos de filtros, la API los combina con una operación Y.

Cada trabajo debe especificar `programId` o `programIds`. Todos los demás filtros son opcionales. El filtro `updatedAt` requiere una infraestructura que no está disponible en todas las suscripciones.

<table>
  <tbody>
    <tr>
      <td>Tipo de filtro</td>
      <td>Tipo de datos</td>
      <td>Notas</td>
    </tr>
    <tr>
      <td>programId</td>
      <td>Entero</td>
      <td>Acepta el ID de un programa. Los trabajos devuelven todos los registros accesibles que son miembros del programa en el momento en que comienza a procesarse el trabajo.Recupere los identificadores de programa usando el extremo <a href="https://developer.adobe.com/marketo-apis/api/asset#tag/Programs">Obtener programas</a>.No se puede usar con el filtro programIds.</td>
    </tr>
    <tr>
      <td>programIds</td>
      <td>Matriz[Entero]</td>
      <td>Acepta una matriz de hasta 10 ID de programa. Los trabajos devuelven todos los registros accesibles que son miembros de los programas en el momento en que comienza a procesarse el trabajo.Se agrega un campo adicional "programId" al archivo de exportación como primer campo. Este campo identifica el programa del que se ha extraído un registro de pertenencia a programa.Recupere los identificadores de programa usando el extremo <a href="https://developer.adobe.com/marketo-apis/api/asset#tag/Programs">Obtener programas</a>.No se puede usar con el filtro programId.</td>
    </tr>
    <tr>
      <td>isExhausted</td>
      <td>Booleano</td>
      <td>Acepta un valor booleano usado para filtrar los registros de pertenencia a programas de <a href="https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/drip-nurturing/using-engagement-programs/people-who-have-exhausted-content">personas que han agotado el contenido</a>.</td>
    </tr>
    <tr>
      <td>nurtureCadence</td>
      <td>Cadena</td>
      <td>Acepta una cadena utilizada para filtrar los registros de pertenencia a un programa para una cadencia de nutrición determinada. Los valores permitidos son:
        <ul>
          <li>pause: la cadencia está en pausa</li>
          <li>norma: la cadencia es normal</li>
        </ul></td>
    </tr>
    <tr>
      <td>statusNames</td>
      <td>Matriz[Cadena]</td>
      <td>Acepta una matriz de nombres de estado de miembro de programa. Los nombres de varios estados se agrupan en OR. Los trabajos con este tipo de filtro devuelven todos los registros accesibles cuyo estado de miembro del programa coincida con cualquiera de los nombres de estado especificados. Se pueden utilizar nombres de estado predeterminados y definidos por el usuario. Si el filtro statusNames se utiliza con el filtro "programIds", se comprueban los registros de pertenencia de cada programa cuyo estado coincida con cualquiera de los nombres de estado. Si no se encuentra un nombre de estado en ninguno de los programas, se devuelve el error "1003, Invalid Data".
        <table>
          <tbody>
            <tr>
              <td>Asistió</td>
              <td>Asistió a petición</td>
              <td>Rechazados</td>
            </tr>
            <tr>
              <td>Hizo clic en</td>
              <td>Contactado</td>
              <td>Convertido</td>
            </tr>
            <tr>
              <td>Comprometido</td>
              <td>Formulario relleno</td>
              <td>Influenciado</td>
            </tr>
            <tr>
              <td>Invitado</td>
              <td>Miembro</td>
              <td>Sin demostración</td>
            </tr>
            <tr>
              <td>No está en el programa</td>
              <td>En la lista</td>
              <td>Abierto</td>
            </tr>
            <tr>
              <td>Registrado</td>
              <td>Registrando</td>
              <td>Error en el registro</td>
            </tr>
            <tr>
              <td>Enviado</td>
              <td>Suscrito</td>
              <td>Suscripción cancelada</td>
            </tr>
            <tr>
              <td>Visto</td>
              <td>Visitado</td>
              <td>Visitó el stand</td>
            </tr>
            <tr>
              <td>En lista de espera</td>
              <td>Contenido web</td>
              <td></td>
            </tr>
          </tbody>
        </table></td>
    </tr>
    <tr>
      <td>updatedAt*</td>
      <td>Date Range</td>
      <td>Acepta un objeto JSON con los miembros startAt y endAt. startAt acepta una fecha y hora que representa la marca de agua inferior y endAt acepta una fecha y hora que representa la marca de agua superior. El intervalo debe ser de 31 días o menos. Las horas de la fecha deben estar en formato ISO-8601, sin milisegundos.Los trabajos con este tipo de filtro devuelven todos los registros accesibles que se actualizaron más recientemente dentro del intervalo de fechas.</td>
    </tr>
  </tbody>
</table>

Algunas suscripciones no admiten este tipo de filtro. Si no está disponible, el extremo del trabajo Crear trabajo de miembro del programa de exportación devuelve `1035, Unsupported filter type for target subscription`. Póngase en contacto con el Soporte técnico de Marketo para solicitar esta funcionalidad para su suscripción.

## Opciones

El extremo del trabajo Crear trabajo de miembro de programa de exportación proporciona opciones para lo siguiente:

- Especifique los campos que desea incluir en el archivo de exportación.
- Cambie el nombre de los encabezados de columna exportados.
- Especifique el formato del archivo de exportación.

| Parámetro | Tipo de datos | Obligatorio | Notas |
| --- | --- | --- | --- |
| campos | Matriz[Cadena] | Sí | El parámetro fields acepta una matriz de cadenas JSON. Los campos enumerados se incluyen en el archivo exportado. Se pueden exportar los siguientes tipos de campo: `LeadCustom` `LeadProgram` MemberCustom `ProgramMember`. Especifique un campo utilizando su nombre de API de REST que se puede recuperar mediante Describir posible cliente2 y/o Describir extremos de miembros del programa. |
| columnHeaderNames | Objeto | No | Objeto JSON que contiene pares de clave-valor de nombres de campo y encabezado de columna. La clave debe ser el nombre de un campo incluido en el trabajo de exportación. El valor es el nombre del encabezado de columna exportado para ese campo. |
| formato | Cadena | No | Acepta uno de: CSV, TSV, SSV. El archivo exportado se representa como un archivo de valores separados por comas, valores separados por tabulaciones o valores separados por espacios, respectivamente, si se establece. Si no se establece, el valor predeterminado es CSV. |

## Creación de un trabajo

Use el extremo [Crear trabajo de miembro de programa de exportación](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Program-Members/operation/createExportProgramMembersUsingPOST) para definir el trabajo de exportación. Especifique un `filter` que contenga el identificador de programa y el `fields` que se va a exportar. También puede especificar `format` y `columnHeaderNames`.

```http
POST /bulk/v1/program/members/export/create.json
```

```json
{
   "format": "CSV",
   "fields": [
        "firstName",
        "lastName",
        "email",
        "membershipDate",
        "program",
        "statusName",
        "leadId",
        "reachedSuccess",
        "leadCustomField01",
        "leadCustomField02",
        "pMCustomField01",
        "pMCustomField02"
   ],
   "filter": {
      "programId":1044
   }
}
```

```json
{
    "requestId": "4d44#16f92734f6e",
    "result": [
        {
            "exportId": "b5ca52a9-5ecb-4966-b5a9-11659a8b4c2b",
            "format": "CSV",
            "status": "Created",
            "createdAt": "2020-01-11T02:33:48Z"
        }
    ],
    "success": true
}
```

La respuesta confirma que el trabajo se ha creado, pero la exportación no se inicia automáticamente. Pase el elemento devuelto `exportId` al extremo [Trabajo de miembro del programa de exportación en cola](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Program-Members/operation/enqueueExportProgramMembersUsingPOST) para iniciar el trabajo:

```http
POST /bulk/v1/program/members/export/{exportId}/enqueue.json
```

```json
{
    "requestId": "d70b#16f9273ae32",
    "result": [
        {
            "exportId": "b5ca52a9-5ecb-4966-b5a9-11659a8b4c2b",
            "format": "CSV",
            "status": "Queued",
            "createdAt": "2020-01-11T02:33:48Z",
            "queuedAt": "2020-01-11T02:34:13Z"
        }
    ],
    "success": true
}
```

La respuesta en cola devuelve inicialmente un estado `Queued`. Cuando una ranura de exportación está disponible, el estado cambia a `Processing`.

## Estado del trabajo de sondeo

Solo puede recuperar el estado para los trabajos creados por el mismo usuario de API.

Dado que la exportación se ejecuta de manera asincrónica, use el extremo [Obtener estado del trabajo del miembro del programa de exportación](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Leads/operation/getExportLeadsStatusUsingGET) para sondear su progreso. El estado se actualiza solo una vez cada 60 segundos, por lo que no sondee con más frecuencia.

El estado puede ser `Created`, `Queued`, `Processing`, `Canceled`, `Completed` o `Failed`.

```http
GET /bulk/v1/program/members/export/{exportId}/status.json
```

```json
{
    "requestId": "9a40#16f9274d250",
    "result": [
        {
            "exportId": "b5ca52a9-5ecb-4966-b5a9-11659a8b4c2b",
            "format": "CSV",
            "status": "Processing",
            "createdAt": "2020-01-11T02:33:48Z",
            "queuedAt": "2020-01-11T02:34:13Z",
            "startedAt": "2020-01-11T02:35:19Z"
        }
    ],
    "success": true
}
```

Esta respuesta muestra que el trabajo aún se está procesando, por lo que el archivo no está disponible. Cuando el estado del trabajo cambie a `Completed`, el archivo estará listo para descargarse.

```json
{
    "requestId": "11ad1#16f9ff6da23",
    "result": [
        {
            "exportId": "1118dc83-273b-4d44-becb-4d212fece550",
            "format": "CSV",
            "status": "Completed",
            "createdAt": "2020-01-11T02:33:48Z",
            "queuedAt": "2020-01-11T02:34:13Z",
            "startedAt": "2020-01-11T02:35:19Z"
            "finishedAt": "2020-01-11T02:36:12Z",
            "numberOfRecords": 13,
            "fileSize": 1752,
            "fileChecksum": "sha256:b3c8e70e6e501cf1025e345a66b409d4fd07364c7da773cfa68a2b68ce1a7212"
        }
    ],
    "success": true
}
```

## Recuperación de datos

Para recuperar una exportación de miembro de programa completada, pase `exportId` al extremo [Obtener archivo de miembro de programa de exportación](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Program-Members/operation/getExportProgramMembersFileUsingGET).

El punto final devuelve el archivo en el formato configurado para el trabajo. Si un campo de miembro de programa solicitado no contiene datos, el campo de exportación correspondiente contiene `null`.

```http
GET /bulk/v1/program/members/export/{exportId}/file.json
```

```text
firstName,lastName,email,Member Date,Program,Status,Lead Id,Success,leadCustomField01,leadCustomField02,pMCustomField01,pMCustomField02
Meera,Reed,mree@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1789,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Jon,Umber,jumb@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1790,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Lyanna,Mormont,lmor@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1791,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Rickon,Stark,rsta@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1792,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Hodor,null,hodor@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1793,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Osha,null,osha@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1794,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Jojen,Reed,Jree@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1795,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Rickard,Karstark,rkar@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1796,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Maester,Luwin,mluw@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1797,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Rodrik,Cassel,rcas@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1798,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Jory,Cassel,jcas@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1799,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Septa,Mordane,smor@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1800,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
```

Para la recuperación parcial o reanudable, el extremo de archivo admite el encabezado HTTP `Range` opcional con un tipo de intervalo de `bytes`. Si no establece el encabezado, el extremo devolverá todo el archivo. Para obtener más información, consulte [Extracción en lotes](bulk-extract.md).

## Cancelación de un trabajo

Para cancelar un trabajo configurado incorrectamente o que ya no se necesita, llame al extremo [Cancelar trabajo de miembro del programa de exportación](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Program-Members/operation/cancelExportProgramMembersUsingPOST):

```http
POST /bulk/v1/program/members/export/{exportId}/cancel.json
```

```json
{
    "requestId": "bb4f#16f86727f89",
    "result": [
        {
            "exportId": "f0d3520c-3a60-4568-9e71-2e619d3805a4",
            "format": "CSV",
            "status": "Cancelled",
            "createdAt": "2020-01-07T21:47:35Z"
        }
    ],
    "success": true
}
```

El estado de respuesta indica que el trabajo se ha cancelado.
