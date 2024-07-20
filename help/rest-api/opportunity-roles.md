---
title: Funciones de oportunidad
feature: REST API
description: Gestión de funciones de oportunidad en Marketo.
exl-id: 2ba84f4d-82d0-4368-94e8-1fc6d17b69ed
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 0%

---

# Funciones de oportunidad

[Referencia de extremo de roles de oportunidad](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/getOpportunityRolesUsingGET)

Los posibles clientes están vinculados a oportunidades a través del objeto `opportunityRole` intermedio.

Las API de funciones de oportunidad solo se exponen para suscripciones que no tienen habilitada una sincronización de CRM nativa.

## Describir

Al igual que las oportunidades, una llamada descrita y las operaciones de CRUD se exponen para roles de oportunidad.

```
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

Observe que tanto `dedupeFields` como `searchableFields` son un poco diferentes de las oportunidades. `dedupeFields` proporciona realmente una clave compuesta, donde se requieren los tres de `externalOpportunityId`, `leadId` y `role`. Tanto el vínculo de oportunidad como el de posible cliente por los campos de ID deben existir en la instancia de destino para que la creación del registro se realice correctamente. Para `searchableFields`, `marketoGUID`, `leadId` y `externalOpportunityId` son todos válidos para consultas por su cuenta y utilizan un patrón idéntico al de Oportunidades, pero existe una opción adicional de usar la clave compuesta para la consulta, que requiere el envío de un objeto JSON mediante el POST, con el parámetro de consulta adicional `_method=GET`.

```
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

Esto produce el mismo tipo de respuesta que una consulta de GET estándar, simplemente tiene una interfaz diferente para realizar la solicitud.

## Crear y actualizar

Las funciones de oportunidad tienen la misma interfaz para crear y actualizar registros que las oportunidades.

```
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

Puede eliminar los roles de oportunidad desduplicando campos o campos de ID. Especifique el uso del parámetro deleteBy con un valor de dedupeFields o idField. Si no se especifica, el valor predeterminado es deduplicarCampos. El cuerpo de la solicitud contiene una matriz de entrada de funciones de oportunidad que se deben eliminar. Se permite un máximo de 300 funciones de oportunidad por llamada.

```
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

- Los extremos de la función de oportunidad tienen un tiempo de espera de 30 segundos a menos que se indique a continuación
   - Funciones de oportunidad de sincronización: 60 s 
   - Eliminar roles de oportunidad: 60 s
