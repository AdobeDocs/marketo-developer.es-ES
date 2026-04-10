---
title: Ingesta de datos
feature: REST API, Dynamic Content
description: Utilice la API de ingesta de datos de Marketo para la ingesta de gran volumen y baja latencia de Personas, Objetos personalizados, Compañías y Miembros del programa.
exl-id: 1d501916-53ac-42d8-a804-abb4ab01c7e8
source-git-commit: e2606d6cb12c572603ff069617de58417e43ca63
workflow-type: tm+mt
source-wordcount: '1789'
ht-degree: 15%

---

# API de ingesta de datos

La API de ingesta de datos es un servicio de alto volumen, baja latencia y alta disponibilidad diseñado para gestionar de forma eficaz y con mínimos retrasos la ingesta de grandes cantidades de datos relacionados con personas y personas.

Los datos se incorporan enviando solicitudes que se ejecutan de forma asíncrona. El estado de la solicitud se puede recuperar mediante la suscripción a eventos de [Marketo Observability Data Stream](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-observability-data-stream-setup).

Las interfaces se ofrecen para cuatro tipos de objetos: Personas, Objetos personalizados, Compañías y Miembros del programa. La operación de registro es &quot;insertar o actualizar&quot; solamente, excepto para Miembros del programa que también admite la eliminación.

>[!NOTE]
>
>El acceso a la API de ingesta de datos requiere el derecho al paquete de [Marketo Engage Performance Tier](https://nation.marketo.com/t5/product-documents/marketo-engage-performance-tiers/ta-p/328835).

## Autenticación

La API de ingesta de datos utiliza el mismo método de autenticación OAuth 2.0 que la API de REST de Marketo para generar un token de acceso, pero este debe pasarse a través del encabezado HTTP `X-Mkto-User-Token`. No se puede pasar el token de acceso a través de un parámetro de consulta.

Ejemplo de token de acceso mediante encabezado:

`X-Mkto-User-Token: 11606815-aa7a-405a-80a1-f9683efa528b:ab`

## Permisos

La ingesta de datos utiliza el mismo modelo de permisos que la API de REST de Marketo y no requiere ningún permiso especial adicional para su uso, aunque se requieren permisos específicos para cada extremo.

| Extremo | Permiso |
| --- | --- |
| Personas | Guía de solo escritura |
| Objetos personalizados | Objeto personalizado habilitado para lectura y escritura |
| Compañías | Compañía habilitada para lectura y escritura |
| Miembros del programa | Guía de solo escritura |

## Tipos de objetos admitidos

| Tipo de objeto | Operaciones admitidas |
| --- | --- |
| Personas | Actualizar (insertar o actualizar) |
| Objetos personalizados | Actualizar (insertar o actualizar) |
| Compañías | Sincronizar (`createOnly`, `updateOnly`, `createOrUpdate`) |
| Miembros del programa | Sincronizar (actualizar estado), Eliminar (eliminar del programa) |

## Encabezados

La ingesta de datos utiliza los siguientes encabezados HTTP personalizados.

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

Utilice el método HTTP POST para enviar datos al servidor.

La representación de datos se incluye en el cuerpo de la solicitud como application/json.

El nombre de dominio es: `mkto-ingestion-api.adobe.io`

La ruta de acceso comienza por `/subscriptions/MunchkinId`, donde MunchkinId es específico de la instancia de Marketo. Puede encontrar su Munchkin ID en la interfaz de usuario de Marketo Engage en **Administración** > **Mi cuenta** > **Información de asistencia**.  El resto de la ruta se utiliza para especificar el recurso de interés.

Ejemplo de URL para personas:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/persons`

Ejemplo de URL para objetos personalizados:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/customobjects/purchases`

URL de ejemplo para compañías:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/companies`

Ejemplo de URL para miembros del programa:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/programmembers`

### Respuestas

Todas las respuestas devuelven un identificador de solicitud único a través del encabezado `X-Request-Id`.

Ejemplo de ID de solicitud mediante encabezado:

`X-Request-Id: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Correcto

Cuando una llamada se realiza correctamente, se devuelve un estado 202.  No se devuelve ningún cuerpo de respuesta.

Ejemplo de respuesta de éxito:

```http
HTTP/1.1 202 Accepted
X-Request-Id: e3d92152-0fb1-444a-8f8f-29d5a2338598
Content-Length: 0
Date: Wed, 18 Oct 2023 18:56:49 GMT
```

### Error

Cuando una llamada produce un error, se devuelve un estado que no es 202 junto con un cuerpo de respuesta con detalles de error adicionales. El cuerpo de la respuesta es `application/json` y contiene un único objeto con los miembros `error_code` y `message`.

A continuación se muestran códigos de error reutilizados de Adobe Developer Gateway.

| Código de estado HTTP | error_code | Mensaje |
| --- | --- | --- |
| 401 | 401013 | El token de OAUTH no es válido |
| 403 | 403010 | Falta el token de OAuth. |
| 404 | 404040 | Recurso no encontrado |
| 429 | 429001 | Límite de uso del servicio alcanzado |

A continuación, se muestran los códigos de error que son únicos para la API de ingesta de datos y que se componen de 3 segmentos.  Los tres primeros dígitos son el estado (devuelto por Adobe Developer Gateway), seguidos de un cero &quot;0&quot;, seguido de tres dígitos.

| Código de estado HTTP | error_code | Mensaje |
| --- | --- | --- |
| 400 | 4000801 | Solicitud incorrecta |
| 400 | 4000802 | Datos no válidos |
| 403 | 4030801 | No autorizado |
| 429 | 4290801 | Cuota diaria alcanzada |
| 500 | 5000801 | Error interno del servidor |

## Reintentos

Cuando se detecta un error transitorio, el servicio reintenta la operación. Los reintentos se producen por varios motivos, principalmente cuando se agota el tiempo de espera de un servicio dependiente o cuando este no está disponible temporalmente.

Intervalos de reintento:

* Operación inicial y primer reintento: 5 min
* 1º y 2º: 15 min
* 2º y 3º: 20 min
* 3º y 4º: 20 min
* 4º y 5º : 2 horas
* después del quinto reintento -> 3 horas

## Puntos de conexión

Los extremos de ingesta están disponibles para Personas, Objetos personalizados, Compañías y Miembros del programa.

### Personas

Punto final utilizado para actualizar registros de persona.

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
| `persons` | Matriz de objeto | Sí | Lista de pares de nombre-valor de atributo de la persona | - |

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

Punto final utilizado para actualizar registros de objeto personalizados.

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
| `customObjects` | Matriz de objeto | Sí | Lista de pares de nombre-valor de atributo para el objeto. | - |

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

Extremo utilizado para sincronizar registros de empresa. Admite operaciones de creación, actualización y actualización con anulación de duplicación por ID de empresa externo o ID interno de Marketo.

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
| `input` | Matriz de objeto | Sí | Lista de pares de nombre-valor de atributo de compañía. Acepta la clave JSON `input` o `companies`. | - |

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
| Programas | Matriz de objeto | Sí | Lista de operaciones del programa. Cada uno especifica un programa, un estado de destino y los posibles clientes que se sincronizarán. | - |

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
| Programas | Matriz de objeto | Sí | Lista de operaciones de eliminación de programas. Cada uno especifica un programa y los posibles clientes que se van a eliminar. | - |

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

## Límites

Esta es una lista actualizada de protecciones:

* Tamaño máximo de la solicitud: 1 MB
* Número máximo de objetos por solicitud y tipo de objeto: 1000
* Solicitudes máximas por segundo por ID de cliente: 5000
* Número máximo de objetos por día: 10.000.000

Estos límites se aplican de forma uniforme a todas las personas, los objetos personalizados, las empresas y los miembros del programa. Para los miembros del programa, &quot;objetos por solicitud&quot; es el número total de referencias de posibles clientes en todos los programas de una sola solicitud.

## API de ingesta de datos frente a API de REST

Esta es una lista de diferencias entre la API de ingesta de datos y otras API de REST de Marketo:

* Para autenticarse, debe pasar el token de acceso mediante el encabezado `X-Mkto-User-Token`
* El nombre de dominio de URL es `mkto-ingestion-api.adobe.io`
* La ruta de la dirección URL comienza por `/subscriptions/MunchkinId`
* No hay parámetros de consulta
* Si la llamada se realiza correctamente, devuelve un estado 202 y el cuerpo de la respuesta está vacío
* Si falla una llamada, se devuelve un estado que no es 202 y el cuerpo de la respuesta contiene `{ "error_code" : "Error Code", "message" : "Message" }`
* El identificador de solicitud se devuelve a través del encabezado `X-Request-Id`
