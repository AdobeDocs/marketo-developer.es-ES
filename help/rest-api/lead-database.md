---
title: Base de datos de leads
feature: REST API, Database
description: Manipular la base de datos de posibles clientes principal.
exl-id: e62e381f-916b-4d56-bc3d-0046219b68d3
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '1345'
ht-degree: 1%

---

# Base de datos de leads

Las API de base de datos de posibles clientes de Marketo son las API que Marketo proporciona con mayor frecuencia, ya que permiten el intercambio de datos de personas y datos relacionados con personas de Marketo, como actividades, oportunidades y empresas.

## Objetos

Los objetos de base de datos de posibles clientes incluyen:

- Clientes potenciales
- Compañías/Cuentas
- Cuentas nombradas
- Oportunidades
- OpportunityRoles
- SalesPersons
- Objetos personalizados
- Actividades
- Pertenencia a lista y programa

La mayoría de estos objetos incluyen al menos los métodos Create, Read, Update y Delete. También se incluye un método &quot;Describe&quot; que proporciona una lista de los campos disponibles para cada tipo, y una lista de los campos utilizados para la deduplicación (para los objetos que no son de cliente potencial), y qué campos se pueden buscar para la recuperación de registros. El conjunto más completo se proporciona para clientes potenciales, ya que tienen la mayor variedad de capacidades dentro de las aplicaciones de Marketo.

## API

Para obtener una lista completa de los extremos de la API de la base de datos de posibles clientes, incluidos los parámetros y la información de modelado, consulte la [Referencia de extremo de la API de la base de datos de posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi/).

En las instancias con una integración nativa de CRM habilitada (Microsoft Dynamics o Salesforce.com), las API Compañía, Oportunidad, Rol de oportunidad y Vendedor están deshabilitadas. Los registros se administran mediante el CRM cuando están habilitados y no se puede acceder ni actualizar a través de las API de Marketo.

- Tamaño máximo del lote (estándar): 300 registros
- Tamaño máximo del lote (masivo): archivo de 10 MB
- Tamaño predeterminado del lote: 300 registros
- Encabezado de tipo de contenido (estándar): application/json
- Encabezado de tipo de contenido (masivo): multipart/form-data

## Describir

Se proporciona una descripción de la API de para posibles clientes, empresas, oportunidades, funciones, vendedores y objetos personalizados. Al llamar a esto, se recuperan los metadatos del objeto y una lista de campos disponibles para actualizar y consultar. Describir es una parte crucial del diseño de una integración adecuada con Marketo. Proporciona metadatos enriquecidos sobre cómo se puede interactuar con los objetos y cómo no, así como sobre cómo se pueden crear, actualizar y consultar. Aparte de Describir posibles clientes, cada uno de ellos devuelve una lista de claves disponibles para `deduplication` en el parámetro de respuesta `dedupeFields`. Hay disponible una lista de campos como claves para consultar en el parámetro de respuesta `searchableFields`.

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

En este ejemplo, `dedupeFields` es en realidad una clave compuesta. Esto significa que en futuras actualizaciones y creaciones, al usar el modo `dedupeFields`, debe incluir los tres de `externalOpportunityId`, `leadId` y `role` para cada rol. La matriz `searchableFields` también proporciona la lista de campos disponibles para consultar registros de roles. Esto también incluye la clave compuesta de `externalOpportunityId`, `leadId` y `role`.

También hay un parámetro de respuesta de campos, que proporcionará el nombre de cada campo, el `displayName` tal como aparece en la interfaz de usuario de Marketo, el tipo de datos del campo, si se puede actualizar después de la creación y la longitud del campo si corresponde.

## Consulta

Todos los objetos de la base de datos de posibles clientes comparten un patrón básico para consultar claves simples, donde solo se hace referencia a un campo.

```
GET /rest/v1/{type}.json?filterType={field to query}&filterValues={comma-separated list of possible values}
```

Para todos los objetos, excepto los posibles clientes, puede seleccionar su {field to query} de los searchableFields de la llamada descrita correspondiente y componer una lista separada por comas de hasta 300 valores. También existen estos parámetros de consulta opcionales:

- `batchSize`: un recuento entero del número de resultados que se van a devolver. El valor predeterminado y máximo es 300.
- `nextPageToken`: token devuelto por una llamada anterior para paginación. Consulte [Tokens de paginación](paging-tokens.md) para obtener más información.
- `fields`: lista de nombres de campo separados por comas que se devolverán para cada registro. Consulte la descripción correspondiente para obtener una lista de los campos válidos. Si se solicita un campo en particular, pero no se devuelve, el valor se entiende como nulo.
- `_method`: se usa para enviar consultas mediante el método HTTP del POST. Consulte _method=GET para ver su uso.

Para ver un ejemplo rápido, veamos las oportunidades de consulta:

```
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

El `filterType` especificado en esta llamada es &quot;idField&quot; y no &quot;marketoGUID&quot;. Tanto este como &quot;dedupeFields&quot; son casos especiales, en los que el campo correspondiente al idField o dedupeFields se puede asignar un alias de este modo. &quot;marketoGUID&quot; sigue siendo el campo de búsqueda resultante en la llamada, pero no se establece explícitamente en la llamada. Los campos o conjuntos de campos indicados por `idField` y `dedupeFields` de una descripción de objeto siempre serán válidos `filterTypes` para una consulta. Esta llamada busca los registros que coinciden con los GUID incluidos en filterValues y devuelve todos los registros coincidentes. Si no se encuentran registros utilizando este método, la respuesta indicará éxito. Sin embargo, la matriz de resultados estará vacía, ya que la búsqueda se ejecutó correctamente, pero no hubo registros que devolver.

Si el conjunto de registros de la consulta supera los 300 o el `batchSize` que se especificó, el que sea menor, la respuesta tendrá un miembro `moreResult` con un valor true y un `nextPageToken`, que se pueden incluir en una llamada posterior para recuperar más del conjunto. Consulte [Tokens de paginación](paging-tokens.md) para obtener más información.

### URI largos

A veces, como cuando se consulta por GUID, el URI puede ser largo y superar los 8 KB permitidos por el servicio REST. En este caso, debe utilizar el método POST HTTP en lugar de GET y agregar un parámetro de consulta `_method=GET`. Además, el resto de los parámetros de consulta deben pasarse en el cuerpo del POST como una cadena &quot;application/x-www-form-urlencoded&quot; y pasar el encabezado Content-type asociado.

```
POST /rest/v1/opportunities.json?_method=GET
```

```
Content-Type: application/x-www-form-urlencoded
```

```
filterType=idField&filterValues=dff23271-f996-47d7-984f-f2676861b5fa&dff23271-f996-47d7-984f-f2676861b5fc,dff23271-f996-47d7-984f-f2676861b5fb,544fb7f5-2ddf-4fca-ae32-7e6ef1415e9f,f1ba41a2-69d1-4a35-9807-0e159d66f2c9,f7521272-3331-4a89-a768-222baff2f894
```

Además de los URI largos, este parámetro también es necesario al consultar claves compuestas.

### Claves compuestas

El patrón para consultar claves compuestas es diferente de las claves simples, ya que requiere enviar un POST con un cuerpo JSON. Esto no es necesario en todos los casos, solo en aquellos en los que se usa la opción `dedupeFields` con varios campos como `filterType`. Actualmente, las claves compuestas solo se utilizan en funciones de oportunidad y en algunos objetos personalizados. Veamos un ejemplo de una consulta para roles de oportunidad con la clave compuesta de `dedupeFields`:

```
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

La estructura del objeto JSON es básicamente plana y todos los parámetros de consulta de consultas con claves simples son miembros válidos, excepto `filterValues`. En lugar de un valor de filtro, existe una matriz de &quot;entrada&quot; de objetos JSON, cada uno de los cuales debe tener un miembro para cada uno de los campos de la clave compuesta; en este caso, son `externalOpportunityId`, `leadId` y `role`. Esto ejecuta una consulta para `roles`, contra las entradas proporcionadas y devuelve los resultados coincidentes. Si la respuesta devuelve un parámetro con `moreResult=true` y un `nextPageToken`, debe incluir todas las entradas originales y el `nextPageToken` para que la consulta se ejecute correctamente.

## Crear y actualizar

Las creaciones y actualizaciones para los registros de la base de datos de posibles clientes se realizan mediante publicaciones con cuerpos JSON. La interfaz de Oportunidades, Funciones, Objetos personalizados, Compañías y Vendedores es la misma. La interfaz del posible cliente es un poco diferente y puede leer más sobre ella específicamente.

El único parámetro requerido es una matriz denominada `input` que contiene hasta 300 objetos, cada uno con los campos que desea insertar o actualizar como miembros. Opcionalmente, también puede incluir un parámetro `action` que puede ser uno de: `createOnly`, `updateOnly` o `createOrUpdate`. Si se omite la acción, el modo predeterminado es `createOrUpdate`. `dedupeBy` es otro parámetro opcional que se puede usar cuando action se establece en createOnly o `createOrUpdate`. ` dedupeBy` puede ser `idField` o `dedupeFields`. Si se selecciona `idField`, entonces el `idField` que aparece en la descripción se usa para la deduplicación y debe incluirse en cada registro. El modo `idField` no es compatible con el modo `createOnly`. Si se seleccionan `dedupeFields`, entonces se usan los `dedupeFields` enumerados en la descripción del objeto, y cada uno debe incluirse en cada registro. Si se omite el parámetro `dedupeBy`, el modo predeterminado es `dedupeFields`.

Al pasar una lista de valores de campo, se escribe un valor de `null` o una cadena vacía en la base de datos como `null`.

```
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

Aparte de la API de posibles clientes, las llamadas para crear o actualizar objetos de base de datos de posibles clientes devuelven un campo `seq` en cada objeto de la matriz `result`. El número que aparece corresponde al orden del registro actualizado en la solicitud realizada. Cada elemento devuelve el valor de `idField` para el tipo de objeto y un `status`. El campo de estado indica uno de &quot;creado&quot;, &quot;actualizado&quot; o &quot;omitido&quot;.  Si se omite el estado, también habrá una matriz correspondiente de &quot;motivos&quot; con uno o más objetos de motivo que incluya un código y un mensaje, lo que indica por qué se omitió un registro. Consulte [códigos de error](error-codes.md) para obtener más información.

### Eliminar

La interfaz para las eliminaciones es estándar para los objetos de base de datos de posibles clientes, además de los posibles clientes. Aparte de la entrada, solo hay un parámetro obligatorio `deleteBy,` que puede tener un valor de idField o dedupeFields. Veamos cómo se eliminan algunos objetos personalizados.

```
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

`seq`, `status`, `marketoGUID` y `reasons` ya deberían estar familiarizados con usted.

Para obtener más información sobre cómo trabajar con operaciones CRUD para cada tipo de objeto individual, consulte sus páginas respectivas.
