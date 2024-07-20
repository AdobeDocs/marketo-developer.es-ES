---
title: Vendedores
feature: REST API
description: Lea datos sobre los vendedores.
exl-id: f8ed5aa5-63c1-4c5b-8683-bf47eed1ea18
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '303'
ht-degree: 0%

---

# Vendedores

[Referencia de extremo de vendedor](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Sales-Persons)

Las API del vendedor son de solo lectura para las suscripciones que tienen [Sincronización de SFDC](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync) o [Sincronización de Microsoft Dynamics](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync) habilitadas. Los vendedores son un tipo de registro de persona que son los propietarios de ventas de los registros de posibles clientes. Están relacionados con los registros de posibles clientes por el campo externalSalesPersonId de cada registro de posibles clientes. Cuando un posible cliente está relacionado con un vendedor mediante un campo de ID de persona ventas externo rellenado, los campos de búsqueda correspondientes Propietario del posible cliente se rellenan para ese registro de posible cliente en Marketo, lo que permite el uso de los filtros y tokens correspondientes.

Los vendedores están relacionados con los registros de posibles clientes mediante el punto de conexión [Sincronizar posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/syncLeadUsingPOST) y pasando el atributo externalSalesPersonId.

Los vendedores están relacionados con los registros de oportunidad al usar el punto de conexión [Sincronizar oportunidades](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/syncOpportunitiesUsingPOST) y pasar el atributo externalSalesPersonId.

Los vendedores están relacionados con los registros de la compañía mediante el punto de conexión [Sincronizar compañías](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/syncCompaniesUsingPOST) y pasando el atributo externalSalesPersonId.

Los registros del vendedor solo se pueden editar mediante la API.

## Describir

La descripción de los registros del vendedor sigue el patrón estándar para los objetos de base de datos de posibles clientes.

```
GET /rest/v1/salespersons/describe.json
```

```json
{  
   "requestId":"185d6#14b51985ff0",
   "success":true,
   "result":[  
      {  
         "name":"SalesPerson",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:24Z",
         "idField":"id",
         "dedupeFields":[  
            "externalSalesPersonId"
         ],
         "searchableFields":[  
            [  
               "email"
            ],
            [  
               "id"
            ],
            [
               "externalSalesPersonId"
            ]
         ],
         "fields":[  
            {  
               "name":"id",
               "displayName":"Marketo Id",
               "dataType":"integer",
               "updateable":false
            },
            {  
               "name":"createdAt",
               "displayName":"Created At",
               "dataType":"datetime",
               "updateable":false
            },
            {  
               "name":"updatedAt",
               "displayName":"Updated At",
               "dataType":"datetime",
               "updateable":false
            },
            {  
               "name":"email",
               "displayName":"Email",
               "dataType":"string",
               "length":255,
               "updateable":false
            },
            {  
               "name":"externalSalesPersonId",
               "displayName":"External Sales Person Id",
               "dataType":"string",
               "length":255,
               "updateable":false
            }
         ]
      }
   ]
}
```

De manera predeterminada, el `idField` de los vendedores es &quot;id&quot; y el `dedupeFields` es solo &quot;externalSalesPersonId&quot;.

## Consulta

Vendedores que utilizan el patrón de consulta estándar para claves simples. Este ejemplo muestra el correo electrónico del usuario que se está utilizando como externalSalesPersonId. De forma predeterminada, la consulta devuelve todos los campos que se rellenan para los registros devueltos.

```
GET /rest/v1/salespersons.json?filterType=dedupeFields&filterValues=david@test.com,sam@test.com
```

```json
 {  
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[  
      {  
         "seq":0,
         "id":53453,
         "externalSalesPersonId":"sam@test.com",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:23Z"
      },
      {  
         "seq":1,
         "id":53454,
         "externalSalesPersonId":"david@test.com",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:23Z"
      }
   ]
}
```

## Crear y actualizar

El patrón para las actualizaciones es estándar.

```
POST /rest/v1/salespersons.json
```

```json
{
   "action":"createOrUpdate",
   "dedupeBy":"dedupeFields",
   "input":[
      {
         "externalSalesPersonId":"sam@test.com",
         "email":"sam@test.com",
         "firstName":"Sam",
         "lastName":"Sanosin"
      },
      {
         "externalSalesPersonId":"david@test.com",
         "email":"david@test.com",
         "firstName":"David",
         "lastName":"Aulassak"
      }
   ]
}
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "status": "updated",
         "id":45232
      },
      {
         "seq":1,
         "status": "created",
         "id":45236
      }
   ]
}
```

## Eliminar

El patrón para eliminaciones es estándar.

No se permite eliminar Vendedores cuando está &quot;en uso&quot;. En este caso, se omite el vendedor. Ejemplos:

- Cuando el vendedor está asociado con posibles clientes activos
- Cuando el vendedor está asociado con una compañía que se ha eliminado

```
POST /rest/v1/salespersons/delete.json
```

```json
{  
   "deleteBy":"dedupeFields",
   "input":[  
      {  
         "externalSalesPersonId":"sam@test.com"
      },
      {  
         "externalSalesPersonId":"david@test.com"
      },
      {  
         "externalSalesPersonId":"raj@test.com"
      }
   ]
}
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "id":56343,
         "status": "deleted"
      },
      {
         "seq":1,
         "id":53453,
         "status": "deleted"
      },
      {
         "seq":2,
         "status": "skipped"
         "reasons":[
            {
               "code":"1013",
               "message":"Record not found"
            }
         ]
      }
   ]
}
```

## Tiempos de espera

- Los puntos finales del vendedor tienen un tiempo de espera de 30 segundos a menos que se indique a continuación
   - Personas de ventas de sincronización: 60s
   - Eliminar personas de ventas: 60s
