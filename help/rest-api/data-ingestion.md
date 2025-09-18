---
title: Ingesta de datos
feature: REST API, Dynamic Content
description: Consumir datos con las API de Marketo.
exl-id: 1d501916-53ac-42d8-a804-abb4ab01c7e8
source-git-commit: af2d0f266a271696dbf7d334f23bcd9962b9eba4
workflow-type: tm+mt
source-wordcount: '957'
ht-degree: 11%

---

# API de ingesta de datos

La API de ingesta de datos es un servicio de alto volumen, baja latencia y alta disponibilidad diseñado para gestionar de forma eficaz y con mínimos retrasos la ingesta de grandes cantidades de datos relacionados con personas y personas.

Los datos se incorporan enviando solicitudes que se ejecutan de forma asíncrona. El estado de la solicitud se puede recuperar mediante la suscripción a eventos de [Marketo Observability Data Stream](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-observability-data-stream-setup).

Las interfaces se ofrecen para dos tipos de objetos: Personas y Objetos personalizados. La operación de registro es sólo &quot;insertar o actualizar&quot;.

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
|-|-|
| Personas | Guía de solo escritura |
| Objetos personalizados | Objeto personalizado habilitado para lectura y escritura |

## Encabezados

La ingesta de datos utiliza los siguientes encabezados HTTP personalizados.

### Solicitud

| Clave | Valor | Obligatorio | Descripción |
| - | - | - | - |
| X-Correlation-Id | Cadena arbitraria (longitud máxima de 255 caracteres). | No | Se puede utilizar para rastrear solicitudes a través del sistema.  Consulte Flujo de datos de observabilidad de Marketo |
| X-Request-Source | Cadena arbitraria (longitud máxima 50 caracteres). | No | Se puede utilizar para rastrear el origen de las solicitudes a través del sistema.  Consulte Flujo de datos de observabilidad de Marketo |

### Respuesta

| Clave | Valor | Obligatorio |
| - | - | - |
| X-Request-Id | ID único de solicitud. | Sí |

## Solicitudes

Utilice el método HTTP POST para enviar datos al servidor.

La representación de datos se incluye en el cuerpo de la solicitud como application/json.

El nombre de dominio es: `mkto-ingestion-api.adobe.io`

La ruta de acceso comienza por `/subscriptions/MunchkinId`, donde MunchkinId es específico de la instancia de Marketo. Puede encontrar su Munchkin ID en la interfaz de usuario de Marketo Engage en **Administración** > **Mi cuenta** > **Información de asistencia**.  El resto de la ruta se utiliza para especificar el recurso de interés.

Ejemplo de URL para personas:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/persons`

Ejemplo de URL para objetos personalizados:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/customobjects/purchases`

### Respuestas

Todas las respuestas devuelven un identificador de solicitud único a través del encabezado `X-Request-Id`.

Ejemplo de ID de solicitud mediante encabezado:

`X-Request-Id: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Correcto

Cuando una llamada se realiza correctamente, se devuelve un estado 202.  No se devuelve ningún cuerpo de respuesta.

Ejemplo de respuesta de éxito:

```
HTTP/1.1 202 Accepted
X-Request-Id: e3d92152-0fb1-444a-8f8f-29d5a2338598
Content-Length: 0
Date: Wed, 18 Oct 2023 18:56:49 GMT
```

### Error

Cuando una llamada produce un error, se devuelve un estado que no es 202 junto con un cuerpo de respuesta con detalles de error adicionales.  El cuerpo de respuesta es application/json y contiene un único objeto con los miembros error_code y message.

A continuación se muestran códigos de error reutilizados de Adobe Developer Gateway.

| Código de estado HTTP | error_code | Mensaje |
| - | - | - |
| 401 | 401013 | El token de OAUTH no es válido |
| 403 | 403010 | Falta el token de OAuth. |
| 404 | 404040 | Recurso no encontrado |
| 429 | 429001 | Límite de uso del servicio alcanzado |

A continuación, se muestran los códigos de error que son únicos para la API de ingesta de datos y que se componen de 3 segmentos.  Los tres primeros dígitos son el estado (devuelto por Adobe Developer Gateway), seguidos de un cero &quot;0&quot;, seguido de tres dígitos.

| Código de estado HTTP | error_code | Mensaje |
| - | - | - |
| 400 | 4000801 | Solicitud incorrecta |
| 400 | 4000802 | Datos no válidos |
| 403 | 4030801 | No autorizado |
| 429 | 4290801 | Cuota diaria alcanzada |
| 500 | 5000801 | Error interno del servidor |

## Reintentos

Cuando se detecta un error transitorio, el servicio reintenta la operación tres veces.  El primer reintento se produce después de un periodo de espera de 5 minutos, el segundo después de 30 minutos más y, finalmente, el tercero después de 30 minutos más.  Los reintentos se producen por varios motivos, principalmente cuando se agota el tiempo de espera de un servicio dependiente o cuando este no está disponible temporalmente.

## Extremos

Los extremos de ingesta están disponibles para Personas y Objetos personalizados.

### Personas

Punto final utilizado para actualizar registros de persona.

| Método | Ruta |
| - | - |
| POST | /subscriptions/{munchkinId}/people |

#### Encabezados

| Clave | Valor |
| - | - |
| Content-Type | application/json |
| X-Mkto-User-Token | {accessToken} |

#### Cuerpo de solicitud

| Clave | Tipo de datos | Obligatorio | Valor | Valor predeterminado |
| - | - | - | - | - |
| prioridad | Cadena | No | Prioridad de la solicitud: normal o alta | normal |
| partitionName | Cadena | No | Nombre de la partición de persona | Predeterminado |
| deduplicarCampos | Objeto | No | Atributos en los que deduplicar. Se permiten uno o dos nombres de atributo. <br/> Se utilizan dos atributos en una operación AND. Por ejemplo, si se especifican `email` y `firstName`, ambos se usan para buscar a una persona mediante la operación AND. <br/>Los atributos admitidos son: `id`, `email`, `sfdcAccountId`, `sfdcContactId`, `sfdcLeadId` `sfdcLeadOwnerId`, atributos personalizados (&quot;cadena&quot; y tipo &quot;entero&quot; solamente), `email` |
| personas | Matriz de objeto | Sí | Lista de pares de nombre-valor de atributo de la persona | - |

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
         "lastName": "Parker"
      },
      {
         "email": "johnny.neal@yvu30.com",
         "firstName": "Johnny",
         "lastName": "Neal"
      }
   ]
}
```

#### Respuesta

`HTTP/1.1 202`
`X-Request-ID: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Objetos personalizados

Extremo utilizado para actualizar registros de objeto personalizados

| Método | Ruta |
| - | - |
| POST | `/subscriptions/{munchkinId}/customobjects/{customObjectAPIName}` |

#### Encabezados

| Clave | Valor |
| - | - |
| Content-Type | application/json |
| X-Mkto-User-Token | {accessToken} |

#### Cuerpo de solicitud

| Clave | Tipo de datos | Obligatorio | Valor | Valor predeterminado |
| - |- | - | - | - |
| prioridad | Cadena | No | Prioridad de la solicitud: normal, alta | normal |
| dedupeBy | Cadena | No | Atributos para anular la duplicación en: dedupeFields, marketoGUID | deduplicarCampos |
| customObjects | Matriz de objeto | Sí | Lista de pares de nombre-valor de atributo para el objeto. | - |

Los permisos requeridos son `Read-Write Custom Object`.

Si se especifica un campo de vínculo a una persona en la solicitud y esa persona no existe, se producen varios reintentos. Si esa persona se añade durante la ventana de reintento (65 minutos), la actualización se realiza correctamente. Por ejemplo, si el campo de vínculo es correo electrónico en Persona y la opción Persona no existe, se produce un reintento.

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

## Límites

Esta es una lista de uso de protecciones:

* Tamaño máximo de la solicitud: 1 MB
* Número máximo de objetos por solicitud y tipo de objeto: 1000
* Solicitudes máximas por segundo por ID de cliente: 5000
* Número máximo de objetos por día: 10.000.000

## API de ingesta de datos frente a API de REST

Esta es una lista de diferencias entre la API de ingesta de datos y otras API de REST de Marketo:

* Esta no es una interfaz CRUD completa, solo admite actualización
* Para autenticarse, debe pasar el token de acceso mediante el encabezado `X-Mkto-User-Token`
* El nombre de dominio de URL es `mkto-ingestion-api.adobe.io`
* La ruta de la dirección URL comienza por `/subscriptions/MunchkinId`
* No hay parámetros de consulta
* Si la llamada se realiza correctamente, devuelve un estado 202 y el cuerpo de la respuesta está vacío
* Si falla la llamada, se devuelve un estado que no es 202 y el cuerpo de la respuesta contiene `{ "error_code" : "Error Code", "message" : "Message" }`
* El identificador de solicitud se devuelve a través del encabezado `X-Request-Id`
