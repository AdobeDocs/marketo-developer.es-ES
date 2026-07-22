---
title: Ingesta de datos
feature: REST API, Dynamic Content, Static Lists
description: Utilice la API de ingesta de datos de Marketo para la ingesta de gran volumen y baja latencia de Personas, Objetos personalizados, Compañías, Miembros del programa y Listas.
exl-id: 1d501916-53ac-42d8-a804-abb4ab01c7e8
TQID: https://experienceleague.adobe.com/xby7hs-CSLrVzy-FXEBi1FeU1-ca7vI4kB85BYJ9snk
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 2151
ht-degree: 16%

---

# API de ingesta de datos

La API de ingesta de datos es un servicio de alto volumen, baja latencia y alta disponibilidad. Utilícelo para introducir grandes cantidades de datos relacionados con personas y personas con un retraso mínimo.

Las solicitudes de ingesta de datos se ejecutan asincrónicamente. Para recuperar el estado de la solicitud, suscríbase a eventos de la [secuencia de datos de observabilidad de Marketo](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-observability-data-stream-setup).

La API proporciona interfaces para cinco tipos de objetos:

- Personas, Objetos personalizados y Compañías admiten operaciones de &quot;inserción o actualización&quot;.
- Los miembros del programa admiten las operaciones de &quot;insertar o actualizar&quot; y eliminación.
- Las listas (listas estáticas) admiten operaciones de adición y eliminación.

Lea la [documentación de la API de ingesta de datos](https://developer.adobe.com/marketo-apis/api/data-ingestion).

>[!NOTE]
>
>El acceso a la API de ingesta de datos requiere el derecho al paquete de [Marketo Engage Performance Tier](https://nation.marketo.com/t5/product-documents/marketo-engage-performance-tiers/ta-p/328835).

## Autenticación

La API de ingesta de datos utiliza el mismo método de autenticación OAuth 2.0 que la API de REST de Marketo para generar un token de acceso. Pase el token de acceso en el encabezado HTTP `X-Mkto-User-Token`. No se puede pasar como parámetro de consulta.

El siguiente ejemplo pasa un token de acceso en el encabezado:

`X-Mkto-User-Token: 11606815-aa7a-405a-80a1-f9683efa528b:ab`

## Permisos

La ingesta de datos utiliza el modelo de permisos de la API de REST de Marketo y no requiere permisos adicionales. Cada extremo requiere un permiso existente específico, como se muestra en la tabla siguiente.

| Extremo | Permiso |
| --- | --- |
| Personas | Guía de solo escritura |
| Objetos personalizados | Objeto personalizado habilitado para lectura y escritura |
| Compañías | Compañía habilitada para lectura y escritura |
| Miembros del programa | Guía de solo escritura |
| Listas | Guía de solo escritura |

## Tipos de objetos admitidos

| Tipo de objeto | Operaciones admitidas |
| --- | --- |
| Personas | Actualizar (insertar o actualizar) |
| Objetos personalizados | Actualizar (insertar o actualizar) |
| Compañías | Sincronizar (`createOnly`, `updateOnly`, `createOrUpdate`) |
| Miembros del programa | Sincronizar (actualizar estado), Eliminar (eliminar del programa) |
| Listas | Agregar a la lista, Quitar de la lista |

## Encabezados

La ingesta de datos admite los siguientes encabezados HTTP personalizados.

### Solicitud

| Clave | Valor | Obligatorio | Descripción |
| --- | --- | --- | --- |
| `X-Correlation-Id` | Cadena arbitraria (longitud máxima de 255 caracteres). | No | Se puede utilizar para rastrear solicitudes a través del sistema. Consulte Flujo de datos de observabilidad de Marketo |
| `X-Request-Source` | Cadena arbitraria (longitud máxima 50 caracteres). | No | Se puede utilizar para rastrear el origen de las solicitudes a través del sistema. Consulte Flujo de datos de observabilidad de Marketo |

### Respuesta

| Clave | Valor | Obligatorio |
| --- | --- | --- |
| `X-Request-Id` | ID único de solicitud. | Sí |

## Solicitudes

Enviar datos al servidor con el método HTTP POST.

Incluya los datos en el cuerpo de la solicitud como application/json.

Usar el dominio `mkto-ingestion-api.adobe.io`.

La ruta de acceso comienza por `/subscriptions/MunchkinId`, donde MunchkinId es específico de la instancia de Marketo. Encuentre su Munchkin ID en la interfaz de usuario de Marketo Engage en **Administración** > **Mi cuenta** > **Información de asistencia**. El resto de la ruta especifica el recurso.

Ejemplo de URL para personas:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/persons`

Ejemplo de URL para objetos personalizados:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/customobjects/purchases`

URL de ejemplo para compañías:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/companies`

Ejemplo de URL para miembros del programa:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/programmembers`

Ejemplo de URL para listas:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/lists`

### Respuestas

Cada respuesta devuelve un identificador de solicitud único en el encabezado `X-Request-Id`.

Ejemplo de ID de solicitud mediante encabezado:

`X-Request-Id: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Correcto

Una llamada correcta devuelve el estado 202 y ningún cuerpo de respuesta.

Ejemplo de respuesta de éxito:

```http
HTTP/1.1 202 Accepted
X-Request-Id: e3d92152-0fb1-444a-8f8f-29d5a2338598
Content-Length: 0
Date: Wed, 18 Oct 2023 18:56:49 GMT
```

### Error

Cuando falla una llamada, devuelve un estado que no es 202 y un cuerpo de respuesta con detalles de error. El cuerpo de respuesta `application/json` contiene un objeto con miembros `error_code` y `message`.

Los siguientes códigos de error se reutilizan desde Adobe Developer Gateway.

| Código de estado HTTP | error_code | Mensaje |
| --- | --- | --- |
| 401 | 401013 | El token de OAUTH no es válido |
| 403 | 403010 | Falta el token de OAuth. |
| 404 | 404040 | Recurso no encontrado |
| 429 | 429001 | Límite de uso del servicio alcanzado |

Los códigos de error específicos de la API de ingesta de datos contienen tres segmentos: el estado de tres dígitos devuelto por Adobe Developer Gateway, un cero &quot;0&quot; y tres dígitos adicionales.

| Código de estado HTTP | error_code | Mensaje |
| --- | --- | --- |
| 400 | 4000801 | Solicitud incorrecta |
| 400 | 4000802 | Datos no válidos |
| 403 | 4030801 | No autorizado |
| 429 | 4290801 | Cuota diaria alcanzada |
| 500 | 5000801 | Error interno del servidor |

## Reintentos

Cuando el servicio detecta un error transitorio, reintenta la operación. Un reintento se produce principalmente cuando un servicio dependiente agota el tiempo de espera o no está disponible temporalmente.

El servicio utiliza los siguientes intervalos de reintento:

- Operación inicial para el primer reintento: 5 minutos
- Primer reintento para el segundo: 15 minutos
- Segundo reintento al tercer reintento: 20 minutos
- Tercer reintento al cuarto: 20 minutos
- Cuarto reintento a quinto reintento: 2 horas
- Después del quinto reintento: 3 horas

## Puntos de conexión

Los extremos de ingesta están disponibles para Personas, Objetos personalizados, Compañías, Miembros del programa y Listas. Cada sección de extremo define la solicitud y proporciona un ejemplo.

### Personas

Utilice este punto final para actualizar registros de persona.

| Método | Ruta |
| --- | --- |
| POST | /subscriptions/{munchkinId}/people |

#### Encabezados

| Clave | Valor |
| --- | --- |
| `Content-Type` | application/json |
| `X-Mkto-User-Token` | {accessToken} |

#### Cuerpo de solicitud

| Clave | Tipo de datos | Obligatorio | Valor | Valor predeterminado |
| --- | --- | --- | --- | --- |
| `priority` | Cadena | No | Prioridad de la solicitud: normal o alta | normal |
| `partitionName` | Cadena | No | Nombre de la partición de persona | Predeterminado |
| `dedupeFields` | Objeto | No | Atributos en los que deduplicar. Se permiten uno o dos nombres de atributo. <br/> En una operación AND se utilizan dos atributos. Por ejemplo, si se especifican `email` y `firstName`, ambos se usan para buscar a una persona mediante la operación AND. <br/>Los atributos admitidos son: `id`, `email`, `sfdcAccountId`, `sfdcContactId`, `sfdcLeadId` `sfdcLeadOwnerId`, atributos personalizados (&quot;cadena&quot; y tipo &quot;entero&quot; solamente), `email` |  |
| `persons` | Matriz de objeto | Sí | Lista de pares de nombre-valor de atributo de la persona | – |

Los permisos requeridos son `Read-Write Lead`.

### Ejemplo de personas

#### Solicitud

`POST /subscriptions/{munchkinId}/persons`

#### Encabezados

`Content-Type: application/json`
`X-Mkto-User-Token: {accessToken}`

#### Cuerpo

```json
{
   "priority": "high",
   "partitionName": "EMEA",
   "dedupeFields": {
      "field1": "email",
      "field2": "firstName"
   },
   "persons":[
      {
         "email": "brooklyn.parker@karnv.com",
         "firstName": "Brooklyn",
         "lastName": "Parker",
         "company": "Karnv"
      },
      {
         "email": "johnny.neal@yvu30.com",
         "firstName": "Johnny",
         "lastName": "Neal",
         "company": "Acme Inc"
      }
   ]
}
```

#### Respuesta

`HTTP/1.1 202`
`X-Request-ID: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Objetos personalizados

Utilice este extremo para actualizar registros de objeto personalizados.

| Método | Ruta |
| --- | --- |
| POST | `/subscriptions/{munchkinId}/customobjects/{customObjectAPIName}` |

#### Encabezados

| Clave | Valor |
| --- | --- |
| `Content-Type` | application/json |
| `X-Mkto-User-Token` | {accessToken} |

#### Cuerpo de solicitud

| Clave | Tipo de datos | Obligatorio | Valor | Valor predeterminado |
| --- | --- | --- | --- | --- |
| `priority` | Cadena | No | Prioridad de la solicitud: normal, alta | normal |
| `dedupeBy` | Cadena | No | Atributos para anular la duplicación en: dedupeFields, marketoGUID | deduplicarCampos |
| `customObjects` | Matriz de objeto | Sí | Lista de pares de nombre-valor de atributo para el objeto. | – |

Los permisos requeridos son `Read-Write Custom Object`.

Si se especifica un campo de vínculo a una persona en la solicitud y esa persona no existe, se producen varios reintentos. Si esa persona se añade durante la ventana de reintento (65 minutos), la actualización se realiza correctamente. Por ejemplo, si el campo de vínculo es `email` en Persona y la persona no existe, se producen reintentos.

### Ejemplo de objetos personalizados

#### Solicitud

`POST /subscriptions/{munchkinId}/customobjects/{customObjectAPIName}`

#### Encabezados

`Content-Type: application/json`
`X-Mkto-User-Token: {accessToken}`

#### Cuerpo

```json
{
   "dedupeBy": "dedupeFields",
   "priority": "high",
   "customObjects": [
      {
         "email": "brooklyn.parker@karnv.com",
         "vin": "20UYA31581L000000",
         "make": "BMW",
         "model": "3-Series 330i",
         "year": 2003
      },
      {
         "email": "johnny.neal@yvu30.com",
         "vin": "19UYA31581L000000",
         "make": "BMW",
         "model": "3-Series 325i",
         "year": 1989
      }
   ]
}
```

#### Respuesta

`HTTP/1.1 202`
`X-Request-ID: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Compañías

Utilice este extremo para sincronizar registros de empresa. Admite operaciones de creación, actualización y actualización con anulación de duplicación por ID de empresa externo o ID interno de Marketo.

| Método | Ruta |
| --- | --- |
| POST | `/subscriptions/{munchkinId}/companies` |

#### Encabezados

| Clave | Valor | Obligatorio |
| --- | --- | --- |
| `Content-Type` | application/json | Sí |
| `X-Mkto-User-Token` | {accessToken} | Sí |
| `X-Correlation-Id` | Cadena arbitraria (longitud máxima de 255 caracteres) | No |
| `X-Request-Source` | Cadena arbitraria (longitud máxima 50 caracteres) | No |

#### Cuerpo de solicitud

| Clave | Tipo de datos | Obligatorio | Valor | Valor predeterminado |
| --- | --- | --- | --- | --- |
| `action` | Cadena | No | Acción de sincronización: `createOnly`, `updateOnly` o `createOrUpdate` | `createOrUpdate` |
| `dedupeBy` | Cadena | No | Campo para deduplicar en: `dedupeFields` o `idField` (sin distinción de mayúsculas y minúsculas). Para `createOnly` y `createOrUpdate`, solo se permite `dedupeFields`. Para `updateOnly`, se permiten ambos. | `dedupeFields` |
| `input` | Matriz de objeto | Sí | Lista de pares de nombre-valor de atributo de compañía. Acepta la clave JSON `input` o `companies`. | – |

Cada objeto de empresa de la matriz `input` admite los campos siguientes:

| Clave | Tipo de datos | Obligatorio | Descripción |
| --- | --- | --- | --- |
| `externalCompanyId` | Cadena | Condicional | Identificador de empresa externa. Requerido cuando `dedupeBy` es `dedupeFields`. No se permite cuando `dedupeBy` es `idField`. |
| `id` | Largo | Condicional | ID interno de la empresa de Marketo. Requerido cuando `dedupeBy` es `idField` y `action` es `updateOnly`. No se permite cuando `dedupeBy` es `dedupeFields`. |
| `company` | Cadena | No | Nombre de la empresa. |
| (cualquier campo) | Cualquiera | No | Campos de empresa estándar o personalizados adicionales según se definen en [Describir compañías](companies.md). Los nombres de campo no distinguen entre mayúsculas y minúsculas. |

Los permisos requeridos son `Read-Write Company`.

### Ejemplo de compañías

#### Solicitud

`POST /subscriptions/{munchkinId}/companies`

#### Encabezados

`Content-Type: application/json`
`X-Mkto-User-Token: {accessToken}`

#### Cuerpo

```json
{
   "action": "createOrUpdate",
   "dedupeBy": "dedupeFields",
   "input": [
      {
         "externalCompanyId": "ext-company-001",
         "company": "Acme Corporation",
         "industry": "Technology",
         "numberOfEmployees": 5000,
         "annualRevenue": 100000000
      },
      {
         "externalCompanyId": "ext-company-002",
         "company": "Globex Industries",
         "industry": "Manufacturing",
         "numberOfEmployees": 1200
      }
   ]
}
```

#### Respuesta

`HTTP/1.1 202`
`X-Request-ID: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Ejemplo de actualización de compañías por ID

```json
{
   "action": "updateOnly",
   "dedupeBy": "idField",
   "input": [
      {
         "id": 12345,
         "company": "Acme Corporation (Renamed)",
         "numberOfEmployees": 5500
      }
   ]
}
```

### Reglas de validación de compañías

| Regla | Detalles |
| --- | --- |
| acción | Debe ser uno de: `createOnly`, `updateOnly`, `createOrUpdate`. Distingue entre mayúsculas y minúsculas. |
| dedupeBy | Debe ser `dedupeFields` o `idField` (sin distinción de mayúsculas). El valor predeterminado es `dedupeFields`. |
| deduplicarPor + acción | `createOnly` y `createOrUpdate` solo permiten `dedupeFields`. `updateOnly` permite `dedupeFields` y `idField`. |
| Cuando `dedupeBy=dedupeFields` | Cada compañía debe tener `externalCompanyId`. El campo `id` no debe estar presente. |
| Cuando `dedupeBy=idField` | Cada compañía debe tener `id`. El campo `externalCompanyId` no debe estar presente. |
| `input` / `companies` | No debe ser nulo ni estar vacío. |
| Máximo de objetos por solicitud | 1,000 |

### Miembros del programa (Sincronización)

Punto final utilizado para sincronizar el estado de miembro del programa, agregar posibles clientes a los programas o actualizar su estado de programa.

| Método | Ruta |
| --- | --- |
| POST | `/subscriptions/{munchkinId}/programmembers` |

#### Encabezados

| Clave | Valor | Obligatorio |
| --- | --- | --- |
| Content-Type | application/json | Sí |
| X-Mkto-User-Token | {accessToken} | Sí |
| X-Correlation-Id | Cadena arbitraria (longitud máxima de 255 caracteres) | No |
| X-Request-Source | Cadena arbitraria (longitud máxima 50 caracteres) | No |

#### Cuerpo de solicitud

| Clave | Tipo de datos | Obligatorio | Valor | Valor predeterminado |
| --- | --- | --- | --- | --- |
| Programas | Matriz de objeto | Sí | Lista de operaciones del programa. Cada uno especifica un programa, un estado de destino y los posibles clientes que se sincronizarán. | – |

Cada objeto de la matriz `programs` contiene:

| Clave | Tipo de datos | Obligatorio | Descripción |
| --- | --- | --- | --- |
| programId | Largo | Sí | ID del programa de Marketo. Debe ser un entero positivo. |
| estado | Cadena | Sí | Estado de miembro de programa que se va a establecer, por ejemplo `"Member"` o `"Influenced"`. Acepta la clave JSON `statusName` o `status`. El valor no debe ser `"Not in Program"`; use el punto de conexión de eliminación en su lugar. |
| miembros | Matriz de objeto | Sí | Lista de referencias de posibles clientes para agregar o actualizar en el programa. Acepta la clave JSON `input` o `members`. |

Cada objeto de la matriz `members` contiene:

| Clave | Tipo de datos | Obligatorio | Descripción |
| --- | --- | --- | --- |
| leadId | Largo | Sí | El ID del posible cliente de Marketo. |
| (cualquier campo) | Cualquiera | No | Campos adicionales de miembros de programa personalizados. Los nombres de campo no distinguen entre mayúsculas y minúsculas. |

Los permisos requeridos son `Read-Write Lead`.

### Ejemplo de sincronización de miembros de programa

#### Solicitud

`POST /subscriptions/{munchkinId}/programmembers`

#### Encabezados

`Content-Type: application/json`
`X-Mkto-User-Token: {accessToken}`

#### Cuerpo

```json
{
   "programs": [
      {
         "programId": 1001,
         "status": "Member",
         "members": [
            {
               "leadId": 10001
            },
            {
               "leadId": 10002
            }
         ]
      },
      {
         "programId": 1002,
         "status": "Influenced",
         "members": [
            {
               "leadId": 10003
            }
         ]
      }
   ]
}
```

#### Respuesta

`HTTP/1.1 202`
`X-Request-ID: e3d92152-0fb1-444a-8f8f-29d5a2338598`

### Miembros del programa sincronizan reglas de validación

| Regla | Detalles |
| --- | --- |
| Programas | No debe ser nulo ni estar vacío. |
| programId | Requerido. Debe ser un entero positivo. |
| estado | Requerido. No debe estar en blanco. No debe ser `"Not in Program"` (sin distinción de mayúsculas). En su lugar, utilice el punto de conexión de eliminación. |
| miembros | No debe ser nulo ni estar vacío. |
| leadId | Necesario para cada miembro de la matriz de entrada. |
| Máximo de posibles clientes por solicitud | Un total de 1.000 miembros en todos los programas. |

### Miembros del programa (eliminar)

Extremo utilizado para eliminar posibles clientes de los programas. Esto establece el estado de pertenencia del posible cliente en `"Not in Program"` y quita el miembro de ese programa.

>[!NOTE]
>
>Este extremo utiliza POST en lugar de DELETE porque la solicitud requiere un cuerpo JSON con datos estructurados.

| Método | Ruta |
| --- | --- |
| POST | `/subscriptions/{munchkinId}/programmembers/delete` |

#### Encabezados

| Clave | Valor | Obligatorio |
| --- | --- | --- |
| Content-Type | application/json | Sí |
| X-Mkto-User-Token | {accessToken} | Sí |
| X-Correlation-Id | Cadena arbitraria (longitud máxima de 255 caracteres) | No |
| X-Request-Source | Cadena arbitraria (longitud máxima 50 caracteres) | No |

#### Cuerpo de solicitud

| Clave | Tipo de datos | Obligatorio | Valor | Valor predeterminado |
| --- | --- | --- | --- | --- |
| Programas | Matriz de objeto | Sí | Lista de operaciones de eliminación de programas. Cada uno especifica un programa y los posibles clientes que se van a eliminar. | – |

Cada objeto de la matriz `programs` contiene:

| Clave | Tipo de datos | Obligatorio | Descripción |
| --- | --- | --- | --- |
| programId | Largo | Sí | ID del programa de Marketo. Debe ser un entero positivo. |
| miembros | Matriz de objeto | Sí | Lista de referencias de posibles clientes que se van a quitar del programa. Acepta la clave JSON `input` o `members`. |

Cada objeto de la matriz `members` contiene:

| Clave | Tipo de datos | Obligatorio | Descripción |
| --- | --- | --- | --- |
| leadId | Largo | Sí | El ID del posible cliente de Marketo. |

Los permisos requeridos son `Read-Write Lead`.

### Ejemplo de eliminación de miembros del programa

#### Solicitud

`POST /subscriptions/{munchkinId}/programmembers/delete`

#### Encabezados

`Content-Type: application/json`
`X-Mkto-User-Token: {accessToken}`

#### Cuerpo

```json
{
   "programs": [
      {
         "programId": 1001,
         "members": [
            {
               "leadId": 10001
            },
            {
               "leadId": 10002
            }
         ]
      },
      {
         "programId": 1002,
         "members": [
            {
               "leadId": 10003
            }
         ]
      }
   ]
}
```

#### Respuesta

`HTTP/1.1 202`
`X-Request-ID: a1b2c3d4-e5f6-7890-abcd-ef1234567890`

### Miembros del programa eliminan reglas de validación

| Regla | Detalles |
| --- | --- |
| Programas | No debe ser nulo ni estar vacío. |
| programId | Requerido. Debe ser un entero positivo. |
| miembros | No debe ser nulo ni estar vacío. |
| leadId | Necesario para cada miembro de la matriz de entrada. |
| Máximo de posibles clientes por solicitud | Un total de 1.000 miembros en todos los programas. |

### Listas (Agregar a lista)

Punto final utilizado para agregar posibles clientes a una lista estática. Los posibles clientes se identifican con su ID de posible cliente de Marketo.

| Método | Ruta |
| --- | --- |
| POST | `/subscriptions/{munchkinId}/lists` |

#### Encabezados

| Clave | Valor | Obligatorio |
| --- | --- | --- |
| `Content-Type` | application/json | Sí |
| `X-Mkto-User-Token` | {accessToken} | Sí |
| `X-Correlation-Id` | Cadena arbitraria (longitud máxima de 255 caracteres) | No |
| `X-Request-Source` | Cadena arbitraria (longitud máxima 50 caracteres) | No |

#### Cuerpo de solicitud

| Clave | Tipo de datos | Obligatorio | Valor | Valor predeterminado |
| --- | --- | --- | --- | --- |
| `listId` | Largo | Sí | El ID de la lista estática de Marketo. Debe ser un entero positivo. | – |
| `leads` | Matriz de objeto | Sí | Lista de referencias de posibles clientes para agregar a la lista. Acepta la clave JSON `input` o `leads`. | – |

Cada objeto de la matriz de entrada contiene:

| Clave | Tipo de datos | Obligatorio | Descripción |
| --- | --- | --- | --- |
| `leadId` | Largo | Sí | El ID del posible cliente de Marketo. Acepta la clave JSON `leadId` o `id`. |

Los permisos requeridos son `Read-Write Lead`.

### Ejemplo de adición de listas a lista

#### Solicitud

`POST /subscriptions/{munchkinId}/lists`

#### Encabezados

`Content-Type: application/json`
`X-Mkto-User-Token: {accessToken}`

#### Cuerpo

```json
{
   "listId": 1001,
   "leads": [
      {
         "leadId": 10001
      },
      {
         "leadId": 10002
      },
      {
         "leadId": 10003
      }
   ]
}
```

#### Respuesta

`HTTP/1.1 202`
`X-Request-ID: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Enumera las reglas de validación de adición a lista

| Regla | Detalles |
| --- | --- |
| listId | Requerido. Debe ser un entero positivo (> 0). |
| posibles clientes | Requerido. No debe ser nulo ni estar vacío. |
| leadId | Necesario para cada posible cliente de la matriz de entrada. |
| Máximo de posibles clientes por solicitud | 1000 posibles clientes totales en la matriz de entrada. |

### Listas (Quitar de la lista)

Extremo utilizado para eliminar posibles clientes de una lista estática. Los posibles clientes se identifican con su ID de posible cliente de Marketo.

>[!NOTE]
>
>Este extremo utiliza POST en lugar de DELETE porque la solicitud requiere un cuerpo JSON con datos estructurados.

| Método | Ruta |
| --- | --- |
| POST | `/subscriptions/{munchkinId}/lists/remove` |

#### Encabezados

| Clave | Valor | Obligatorio |
| --- | --- | --- |
| `Content-Type` | application/json | Sí |
| `X-Mkto-User-Token` | {accessToken} | Sí |
| `X-Correlation-Id` | Cadena arbitraria (longitud máxima de 255 caracteres) | No |
| `X-Request-Source` | Cadena arbitraria (longitud máxima 50 caracteres) | No |

#### Cuerpo de solicitud

| Clave | Tipo de datos | Obligatorio | Valor | Valor predeterminado |
| --- | --- | --- | --- | --- |
| `listId` | Largo | Sí | El ID de la lista estática de Marketo. Debe ser un entero positivo. | – |
| `leads` | Matriz de objeto | Sí | Lista de referencias de posibles clientes que se van a quitar de la lista. Acepta la clave JSON `input` o `leads`. | – |

Cada objeto de la matriz de entrada contiene:

| Clave | Tipo de datos | Obligatorio | Descripción |
| --- | --- | --- | --- |
| `leadId` | Largo | Sí | El ID del posible cliente de Marketo. Acepta la clave JSON `leadId` o `id`. |

Los permisos requeridos son `Read-Write Lead`.

### Ejemplo de Listas eliminadas de la lista

#### Solicitud

`POST /subscriptions/{munchkinId}/lists/remove`

#### Encabezados

`Content-Type: application/json`
`X-Mkto-User-Token: {accessToken}`

#### Cuerpo

```json
{
   "listId": 1001,
   "leads": [
      {
         "leadId": 10001
      },
      {
         "leadId": 10002
      }
   ]
}
```

#### Respuesta

`HTTP/1.1 202`
`X-Request-ID: e3d92152-0fb1-444a-8f8f-29d5a2338598`

### Listas eliminadas de las reglas de validación de lista

| Regla | Detalles |
| --- | --- |
| listId | Requerido. Debe ser un entero positivo (> 0). |
| posibles clientes | Requerido. No debe ser nulo ni estar vacío. |
| leadId | Necesario para cada posible cliente de la matriz de entrada. |
| Máximo de posibles clientes por solicitud | 1000 posibles clientes totales en la matriz de entrada. |

## Límites

La API de ingesta de datos tiene las siguientes protecciones:

- Tamaño máximo de la solicitud: 1 MB
- Número máximo de objetos por solicitud para cada tipo de objeto: 1000
- Número máximo de solicitudes por segundo para cada ID de cliente: 5000
- Número máximo de objetos por día: 10.000.000

Estos límites se aplican de forma uniforme a Personas, Objetos personalizados, Compañías, Miembros del programa y Listas. Para los miembros del programa, &quot;objetos por solicitud&quot; es el número total de referencias de posibles clientes en todos los programas de una sola solicitud. En el caso de las listas, &quot;objetos por solicitud&quot; es el número de referencias de posibles clientes en la matriz de entrada.

## API de ingesta de datos frente a API de REST

La API de ingesta de datos difiere de otras API de REST de Marketo en los siguientes aspectos:

- Pase el token de acceso en el encabezado `X-Mkto-User-Token`.
- Usar el dominio `mkto-ingestion-api.adobe.io`.
- Inicie la ruta de acceso de la dirección URL con `/subscriptions/MunchkinId`.
- No utilice parámetros de consulta.
- Una llamada correcta devuelve el estado 202 y un cuerpo de respuesta vacío.
- Una llamada con error devuelve un estado distinto de 202 y un cuerpo de respuesta que contiene `{ "error_code" : "Error Code", "message" : "Message" }`.
- El encabezado `X-Request-Id` devuelve el identificador de solicitud.
