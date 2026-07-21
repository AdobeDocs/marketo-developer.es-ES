---
title: Funciones de oportunidad
feature: REST API
description: Administre las funciones de oportunidad de Marketo mediante la API de REST, incluidas las consultas con campos de desduplicación compuestos, la creación de actualizaciones, la eliminación, los tiempos de espera y la ausencia de sincronización de CRM.
exl-id: 2ba84f4d-82d0-4368-94e8-1fc6d17b69ed
TQID: https://experienceleague.adobe.com/aE27mBhsrn-0SO41M-pV5NFjoMq--1Lp-L2TQGL7-8Y
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 254
ht-degree: 0%

---

# Funciones de oportunidad

[Referencia de extremo de roles de oportunidad](https://developer.adobe.com/marketo-apis/api/mapi#tag/Opportunities/operation/getOpportunityRolesUsingGET)

Los vínculos de objetos `opportunityRole` intermedios conducen a oportunidades.

Las API de funciones de oportunidad solo están disponibles para suscripciones que no tienen habilitada la sincronización nativa con CRM.

## Describir

Al igual que con las oportunidades, la API proporciona una llamada Describir y operaciones CRUD para las funciones de oportunidad.

```http
GET /rest/v1/opportunities/roles/describe.json
```

```json
{
   "requestId":"185d6#14b51985ff0",
   "success":true,
   "result":[
      {
         "name":"opportunityRole",
         "displayName":"Opportunity Role",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:24Z",
         "idField":"marketoGUID",
         "dedupeFields":[
            "externalOpportunityId",
            "leadId",
            "role"
         ],
         "searchableFields":[
            [
               "externalOpportunityId",
               "leadId",
               "role"
            ],
            [
               "marketoGUID"
            ],
            [
               "leadId"
            ],
            [
               "externalOpportunityId"
            ]
         ],
         "fields":[
            {
               "name":"marketoGUID",
               "displayName":"Marketo GUID",
               "dataType":"string",
               "length":36,
               "updateable":false
            },
            {
               "name":"externalOpportunityId",
               "displayName":"External Opportunity Id",
               "dataType":"string",
               "length":50,
               "updateable":false
            },
            {
               "name":"leadId",
               "displayName":"Lead Id",
               "dataType":"integer",
               "updateable":false
            },
            {
               "name":"role",
               "displayName":"Role",
               "dataType":"string",
               "length":50,
               "updateable":false
            },
            {
               "name":"isPrimary",
               "displayName":"Is Primary",
               "dataType":"boolean",
               "updateable":true
            },
            {
               "name":"externalCreatedDate",
               "displayName":"External Created Date",
               "dataType":"datetime",
               "updateable":true
            }
         ]
      }
   ]
}
```

## Consulta

Los valores `dedupeFields` y `searchableFields` difieren de las oportunidades. `dedupeFields` proporciona una clave compuesta que requiere `externalOpportunityId`, `leadId` y `role`. Para que la creación de registros se realice correctamente, la oportunidad y el posible cliente a los que hacen referencia los campos de ID deben existir en la instancia de destino.

Los valores `searchableFields` `marketoGUID`, `leadId` y `externalOpportunityId` son válidos para consultas individuales que utilizan el mismo patrón que Oportunidades. También puede consultar por la clave compuesta. Esta consulta requiere un objeto JSON enviado mediante POST con el parámetro de consulta `_method=GET`.

```http
POST /rest/v1/opportunities/roles.json?_method=GET
```

```json
{
   "filterType": "dedupeFields",
   "fields": [
      "marketoGuid",
      "externalOpportunityId",
      "leadId",
      "role"
   ],
   "input": [
      {
        "externalOpportunityId": "Opportunity1",
        "leadId": 1,
        "role": "Captain"
      },
      {
        "externalOpportunityId": "Opportunity2",
        "leadId": 1872,
        "role": "Commander"
      },
      {
        "externalOpportunityId": "Opportunity3",
        "leadId": 273891,
        "role": "Lieutenant Commander"
      }
   ]
}
```

Esta solicitud produce el mismo tipo de respuesta que una consulta GET estándar, pero utiliza una interfaz de solicitud diferente.

## Crear y actualizar

Cree y actualice las funciones de oportunidad utilizando la misma interfaz que las oportunidades.

```http
POST /rest/v1/opportunities/roles.json
```

```json
{
   "action": "createOrUpdate",
   "dedupeBy": "dedupeFields",
   "input": [
      {
         "externalOpportunityId": "19UYA31581L000000",
         "leadId": 456783,
         "role": "Technical Buyer",
         "isPrimary": false
      },
      {
         "externalOpportunityId": "19UYA31581L000000",
         "leadId": 456784,
         "role": "Technical Buyer",
         "isPrimary": false
      }
   ]
}
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result":[
      {
         "seq": 0,
         "status": "updated",
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
      },
      {
         "seq": 1,
         "status": "created",
         "marketoGUID": "cff23271-f996-47d7-984f-f2676861b5fb"
      }
   ]
}
```

## Eliminar

Eliminar roles de oportunidad desduplicando campos o campos de ID. Establezca el parámetro deleteBy en deduplicationFields o idField. El valor predeterminado es deduplicarCampos.

El cuerpo de la solicitud contiene una matriz de entrada de funciones de oportunidad que se deben eliminar. Cada llamada permite un máximo de 300 funciones de oportunidad.

```http
POST /rest/v1/opportunities/roles/delete.json
```

```json
{
   "deleteBy": "dedupeFields",
   "input": [
      {
        "externalOpportunityId": "19UYA31581L000000",
        "leadId": 456783,
        "role": "Technical Buyer"
      }
   ]
}
```

```json
{
    "requestId": "10f7c#173264db42d",
    "result": [
        {
            "seq": 0,
            "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
            "status": "deleted"
        }
    ]
    "success": true
}
```

## Tiempos de espera

- Los extremos de la función de oportunidad tienen un tiempo de espera de 30 segundos a menos que se indique lo contrario.
- Los roles de oportunidad de sincronización tienen un tiempo de espera de 60 segundos.
- Eliminar roles de oportunidad tiene un tiempo de espera de 60 segundos.
