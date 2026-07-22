---
title: Base de datos de posible clientes
feature: REST API, Database
description: Guía de las API de base de datos de posibles clientes de Marketo que abarcan objetos, métodos CRUD y Describe, patrones de consulta, límites de lotes y restricciones de integración de CRM.
exl-id: e62e381f-916b-4d56-bc3d-0046219b68d3
TQID: https://experienceleague.adobe.com/7lGbhE92lvIE-XkMyUIaK9GrreZVRdM-WVZTpHARhxE
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1058
ht-degree: 1%

---

# Base de datos de posible clientes

Las API de base de datos de posibles clientes de Marketo intercambian datos relacionados con personas y personas con Marketo. Estos datos incluyen Actividades, Oportunidades y Compañías.

## Objetos

La base de datos de posibles clientes incluye los siguientes objetos:

- Clientes potenciales
- Compañías/Cuentas
- Cuentas nombradas
- Oportunidades
- OpportunityRoles
- SalesPersons
- Objetos personalizados
- Actividades
- Pertenencia a lista y programa

La mayoría de los objetos de base de datos de posibles clientes admiten los métodos Create, Read, Update y Delete. El método Describe proporciona los campos disponibles para cada tipo de objeto. En el caso de los objetos que no son de posible cliente, también identifica los campos utilizados para la anulación de duplicación y los campos en los que se puede buscar al recuperar registros.

Los objetos de posibles clientes admiten el conjunto más amplio de funciones, ya que los posibles clientes tienen la mayor variedad de usos en las aplicaciones de Marketo.

## API

Para obtener una lista completa de los extremos, los parámetros y la información de modelado de la API de base de datos de posibles clientes, consulte la [Referencia de extremo de la API de base de datos de posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi).

Cuando una instancia tiene una integración nativa de Microsoft Dynamics o Salesforce.com CRM, las API Company, Opportunity, Opportunity Role y Sales Person se desactivan. El CRM administra estos registros, por lo que no puede acceder a ellos ni actualizarlos a través de las API de Marketo.

- Tamaño máximo del lote (estándar): 300 registros
- Tamaño máximo del lote (masivo): archivo de 10 MB
- Tamaño predeterminado del lote: 300 registros
- Encabezado de tipo de contenido (estándar): application/json
- Encabezado de tipo de contenido (masivo): multipart/form-data

## Describir

La API Describir está disponible para posibles clientes, empresas, oportunidades, funciones, vendedores y objetos personalizados. Utilícelo para recuperar los metadatos del objeto y los campos disponibles para las actualizaciones y consultas.

Excepto para Describir posibles clientes, cada extremo de Describir devuelve lo siguiente:

- `dedupeFields`: claves disponibles para la deduplicación.
- `searchableFields`: claves disponibles para consultas.

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

En este ejemplo, `dedupeFields` es una clave compuesta. Cuando utilice el modo `dedupeFields` para futuras creaciones y actualizaciones, incluya `externalOpportunityId`, `leadId` y `role` para cada rol.

La matriz `searchableFields` enumera los campos disponibles para consultar los registros de rol. Esta lista incluye la clave compuesta de `externalOpportunityId`, `leadId` y `role`.

El parámetro de respuesta `fields` proporciona la siguiente información para cada campo:

- Nombre.
- `displayName` como se muestra en la interfaz de usuario de Marketo.
- Tipo de datos.
- Si el campo se puede actualizar después de la creación.
- Longitud del campo, si corresponde.

## Consulta

Los objetos de base de datos de posibles clientes comparten un patrón de consulta básico para claves simples que hacen referencia a un campo.

```http
GET /rest/v1/{type}.json?filterType={field to query}&filterValues={comma-separated list of possible values}
```

Para todos los objetos excepto posibles clientes, seleccione `{field to query}` de `searchableFields` en la respuesta Describir correspondiente. Proporcione una lista separada por comas de hasta 300 valores.

También puede incluir estos parámetros de consulta opcionales:

- `batchSize`: un entero que especifica el número de resultados que se van a devolver. El valor predeterminado y máximo es 300.
- `nextPageToken`: token devuelto por una llamada anterior para paginación. Consulte [Tokens de paginación](paging-tokens.md) para obtener más información.
- `fields`: lista de nombres de campo separados por comas que se devolverán para cada registro. Consulte la descripción correspondiente para ver los campos válidos. Si se solicita un campo que no se devuelve, su valor implica que es nulo.
- `_method`: envía consultas utilizando el método HTTP POST. Consulte la sección _method=GET para ver su uso.

El siguiente ejemplo consulta oportunidades:

```http
GET /rest/v1/opportunities.json?filterType=idField&filterValues=dff23271-f996-47d7-984f-f2676861b5fa&dff23271-f996-47d7-984f-f2676861b5fc,dff23271-f996-47d7-984f-f2676861b5fb
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fa ",
         "externalOpportunityId":"19UYA31581L000000",
         "name":"Chairs",
         "description":"Chairs",
         "amount":"1604.47",
         "source":"Inbound Sales Call/Email"
      },
      {
         "seq":1,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fc ",
         "externalOpportunityId":"29UYA31581L000000",
         "name":"Big Dog Day Care-Phase12",
         "description":"Big Dog Day Care-Phase12",
         "amount":"1604.47",
         "source":"Email"
      }
   ]
}
```

El `filterType` de esta llamada es &quot;idField&quot;, no &quot;marketoGUID&quot;. Tanto &quot;idField&quot; como &quot;dedupeFields&quot; son casos especiales que permiten utilizar un alias para el campo o campos correspondientes. Aunque la llamada no establece explícitamente &quot;marketoGUID&quot;, permanece como el campo de búsqueda.

Los campos o conjuntos de campos identificados por `idField` y `dedupeFields` en una descripción de objeto siempre son válidos `filterTypes` para una consulta. Esta llamada devuelve registros que coinciden con los GUID de filterValues. Si no coincide ningún registro, la respuesta indica éxito y devuelve una matriz de resultados vacía.

Si el conjunto de registros coincidente supera los 300 o el `batchSize` especificado, el valor que sea menor, la respuesta incluye `moreResult` con un valor true y un `nextPageToken`. Incluya el token en una llamada posterior para recuperar más registros. Consulte [Tokens de paginación](paging-tokens.md) para obtener más información.

### URI largos

Un URI puede superar el límite de 8 KB del servicio REST, como cuando se realiza una consulta por GUID. En este caso, utilice el método HTTP POST en lugar de GET y agregue el parámetro de consulta `_method=GET`.

Pase los parámetros de consulta restantes del cuerpo de POST como una cadena &quot;application/x-www-form-urlencoded&quot;. Pase también el encabezado de tipo de contenido asociado.

```http
POST /rest/v1/opportunities.json?_method=GET
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
filterType=idField&filterValues=dff23271-f996-47d7-984f-f2676861b5fa&dff23271-f996-47d7-984f-f2676861b5fc,dff23271-f996-47d7-984f-f2676861b5fb,544fb7f5-2ddf-4fca-ae32-7e6ef1415e9f,f1ba41a2-69d1-4a35-9807-0e159d66f2c9,f7521272-3331-4a89-a768-222baff2f894
```

El parámetro `_method=GET` también es necesario al consultar claves compuestas.

### Claves compuestas

Para consultar una clave compuesta, envíe una solicitud de POST con un cuerpo JSON. Utilice este patrón únicamente cuando la opción `filterType` sea `dedupeFields` con varios campos.

Actualmente, las claves compuestas solo las utilizan los roles de oportunidad y algunos objetos personalizados. El ejemplo siguiente consulta los roles de oportunidad con la clave compuesta de `dedupeFields`:

```http
POST /rest/v1/opportunities/roles.json?_method=GET
```

```json
{
   "filterType":"dedupeFields",
   "fields":[
      "marketoGuid",
      "externalOpportunityId",
      "leadId",
      "role"
   ],
   "input":[
      {
        "externalOpportunityId":"Opportunity1",
        "leadId": 1,
        "role": "Captain"
      },
      {
        "externalOpportunityId":"Opportunity2",
        "leadId": 1872,
        "role": "Commander"
      },
      {
        "externalOpportunityId":"Opportunity3",
        "leadId": 273891,
        "role": "Lieutenant Commander"
      }
   ]
}
```

El objeto JSON acepta todos los parámetros de consulta utilizados para consultas de clave simple excepto `filterValues`. En lugar de `filterValues`, proporcione una matriz de &quot;entrada&quot; de objetos JSON. Cada objeto debe incluir todos los campos de la clave compuesta. En este ejemplo, los campos son `externalOpportunityId`, `leadId` y `role`.

La solicitud consulta `roles` las entradas proporcionadas y devuelve resultados coincidentes. Si la respuesta incluye `moreResult=true` y un `nextPageToken`, incluya todas las entradas originales y el `nextPageToken` en la siguiente solicitud.

## Crear y actualizar

Cree y actualice registros de la base de datos de posibles clientes enviando solicitudes POST con cuerpos JSON. Las oportunidades, los roles, los objetos personalizados, las compañías y los vendedores utilizan la misma interfaz. Los posibles clientes utilizan una interfaz diferente, que se describe en la Documentación de posibles clientes.

El único parámetro requerido es `input`, una matriz de hasta 300 objetos. Cada objeto contiene los campos que se van a insertar o actualizar.

También puede incluir estos parámetros opcionales:

- `action`: acepta `createOnly`, `updateOnly` o `createOrUpdate`. Si se omite, el valor predeterminado del modo es `createOrUpdate`.
- `dedupeBy`: acepta `idField` o `dedupeFields` cuando la acción se establece en createOnly o `createOrUpdate`. Si se omite, el valor predeterminado del modo es `dedupeFields`.

Cuando `dedupeBy` tiene `idField`, el(la) `idField` que aparece en la descripción se usa para la deduplicación y debe incluirse en cada registro. El modo `idField` no es compatible con el modo `createOnly`.

Cuando `dedupeBy` sea `dedupeFields`, incluya cada campo `dedupeFields` en la descripción del objeto en cada registro.

Cuando pasa valores de campo, la base de datos escribe un valor de `null` o una cadena vacía como `null`.

```http
POST /rest/v1/opportunities.json
```

```json
{
   "action":"createOrUpdate",
   "dedupeBy":"dedupeFields",
   "input":[
      {
         "externalOpportunityId":"19UYA31581L000000",
         "name":"Chairs",
         "description":"Chairs",
         "amount":"1604.47",
         "source":"Inbound Sales Call/Email"
      },
      {
         "externalOpportunityId":"29UYA31581L000000",
         "name":"Big Dog Day Care-Phase12",
         "description":"Big Dog Day Care-Phase12",
         "amount":"1604.47",
         "source":"Email"
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
         "status":"updated",
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb"
      },
      {
         "seq":1,
         "status":"created",
         "marketoGUID":"cff23271-f996-47d7-984f-f2676861b5fb"
      }
   ]
}
```

Excepto la API de posibles clientes, las llamadas de creación y actualización devuelven un campo `seq` en cada objeto de la matriz `result`. El número corresponde a la posición del registro actualizado en la solicitud.

Cada resultado también devuelve el valor `idField` del tipo de objeto y un `status` de &quot;creado&quot;, &quot;actualizado&quot; u &quot;omitido&quot;. Si se omite el estado, el resultado incluye una matriz &quot;reason&quot;. Cada objeto de motivo contiene un código y un mensaje que explican por qué se omitió el registro. Consulte [códigos de error](error-codes.md) para obtener más información.

### Eliminar

Excepto para los posibles clientes, los objetos de base de datos de posibles clientes utilizan una interfaz de eliminación estándar. Además de la entrada, el único parámetro requerido es `deleteBy,`, que acepta idField o dedupeFields.

El ejemplo siguiente elimina los objetos personalizados:

```http
POST /rest/v1/customobjects/{name}/delete.json
```

```json
{
   "deleteBy":"dedupeFields",
   "input":[
      {
         "vin":"19UYA31581L000000"
      },
      {
         "vin":"29UYA31581L000000"
      },
      {
         "vin":"39UYA31581L000000"
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
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb",
         "status": "deleted"
      },
      {
         "seq":1,
         "marketoGUID":"da42707c-4dc4-4fc1-9fef-f30a3017240a",
         "status": "deleted"
      },
      {
         "seq":2,
         "status": "skipped"
         "reasons":[
            {
               "code":"1013",
               "message":"Object not found"
            }
         ]
      }
   ]
}
```

La respuesta incluye `seq`, `status` y `marketoGUID`. Para los registros omitidos, también incluye `reasons`.

Para obtener más información sobre las operaciones de CRUD para un tipo de objeto específico, consulte la documentación de ese objeto.
