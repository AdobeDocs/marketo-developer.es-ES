---
title: Vendedores
feature: REST API
description: Guía de la API de REST de Marketo para los registros del vendedor con sincronización de SFDC o Dynamics, que utiliza externalSalesPersonId para relacionarse con posibles clientes y realizar consultas, actualizaciones y eliminaciones.
exl-id: f8ed5aa5-63c1-4c5b-8683-bf47eed1ea18
TQID: https://experienceleague.adobe.com/JwLNgM0zgztyoYJotCiSdGxMixnzA0kvkFbvq8kEkzE
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 369
ht-degree: 0%

---

# Vendedores

[Referencia de extremo de vendedor](https://developer.adobe.com/marketo-apis/api/mapi#tag/Sales-Persons)

Las API del vendedor proporcionan acceso de solo lectura para las suscripciones que tienen [SFDC Sync](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync) o [Microsoft Dynamics Sync](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync) habilitado.

Los vendedores son registros de personas que representan a los propietarios de ventas de los registros de posibles clientes. El campo externalSalesPersonId de cada registro de posibles clientes relaciona un posible cliente con un vendedor. Cuando se rellena este campo, Marketo rellena los campos de búsqueda correspondientes Propietario del posible cliente en el registro de posible cliente. A continuación, puede utilizar los filtros y tokens asociados.

Relacione los vendedores con otros registros pasando el atributo externalSalesPersonId al punto de conexión correspondiente:

- Registros de posibles clientes: [Sincronizar posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/syncLeadUsingPOST).
- Registros de oportunidad: [Sincronizar oportunidades](https://developer.adobe.com/marketo-apis/api/mapi#tag/Opportunities/operation/syncOpportunitiesUsingPOST).
- Registros de compañía: [Sincronizar compañías](https://developer.adobe.com/marketo-apis/api/mapi#tag/Companies/operation/syncCompaniesUsingPOST).

Los registros del vendedor solo se pueden editar mediante la API.

## Describir

Describir los registros del vendedor utilizando el patrón estándar para los objetos de la base de datos de posibles clientes.

```http
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

De manera predeterminada, el vendedor `idField` es &quot;id&quot; y `dedupeFields` es &quot;externalSalesPersonId&quot;.

## Consulta

Consulte los vendedores utilizando el patrón de consulta estándar para claves simples. El ejemplo siguiente utiliza el correo electrónico del usuario como externalSalesPersonId.

De forma predeterminada, la consulta devuelve todos los campos rellenados para los registros coincidentes.

```http
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

Cree o actualice vendedores utilizando el patrón de actualización estándar.

```http
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

Suprimir Vendedores utilizando el patrón de supresión estándar.

No puede eliminar un vendedor que esté &quot;en uso&quot;. La solicitud omite el vendedor en los siguientes casos:

- El vendedor está asociado con posibles clientes activos.
- El vendedor está asociado con una compañía que se ha eliminado.

```http
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

- Los extremos del vendedor tienen un tiempo de espera de 30 segundos a menos que se indique lo contrario.
- El tiempo de espera de las personas de ventas de sincronización es de 60 segundos.
- Eliminar vendedores tiene un tiempo de espera de 60 segundos.
