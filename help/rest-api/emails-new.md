---
title: Correos electrónicos
feature: REST API
description: Utilice la API de REST de Marketo Asset para consultar, crear, actualizar, clonar, eliminar, aprobar e inspeccionar las dependencias de los recursos de correo electrónico.
exl-id: b41a3ae5-2b25-4103-84b4-320fc2c44bd6
source-git-commit: 0e0a3e5a08e81f349044cbc327d1aba963ab30e4
workflow-type: tm+mt
source-wordcount: '497'
ht-degree: 5%

---

# Correos electrónicos

[Referencia de extremo de correo electrónico](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails_New)

Los correos electrónicos son registros de recursos que definen los metadatos del mensaje, la configuración del contenido, la configuración y el estado de aprobación.

## Acceso

Los extremos descritos en este artículo requieren un token de acceso:

```text
?access_token=<access_token>
```

Las solicitudes también requieren el encabezado `x-app-type`:

```text
x-app-type: <app-type>
```

## Consulta

Puede recuperar metadatos de correo electrónico por el recurso `id` o con el extremo de filtro.

### Por identificador

#### Solicitud

```text
GET /rest/asset/v2/email/{id}
```

#### Respuesta

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "7b3c#1900ab12cd3",
  "result": [
    {
      "id": "1017",
      "name": "Spring Launch Email",
      "description": "Main announcement email",
      "status": "draft"
    }
  ]
}
```

### Filtro

El punto final del filtro admite la búsqueda dentro de un espacio de trabajo y reduce los resultados con parámetros de consulta adicionales.

Se requiere `workspaceId`.

Parámetros de consulta admitidos:

| Parámetro | Descripción |
| - | - |
| `workspaceId` | Identificador de espacio de trabajo obligatorio utilizado para definir el ámbito de la solicitud de filtro. |
| `folderId` | Filtra los resultados a una sola carpeta. |
| `folderIds` | Parámetro repetido que filtra los resultados en varias carpetas. |
| `status` | Parámetro repetido que filtra por uno o más estados de correo electrónico. |
| `pageIndex` | Índice de página basado en cero para resultados paginados. |
| `pageSize` | Número máximo de resultados por página. |
| `createdBy` | Filtra los resultados por el usuario que crea. |
| `createdAtStart` | Devuelve los recursos creados en esta marca de tiempo o después de ella. |
| `createdAtEnd` | Devuelve los recursos creados en esta marca de tiempo o antes de ella. |
| `modifiedBy` | Filtra los resultados por el último usuario que modificó. |
| `modifiedAtStart` | Devuelve recursos modificados en esta marca de tiempo o posteriormente. |
| `modifiedAtEnd` | Devuelve recursos modificados en esta marca de tiempo o antes. |
| `name` | Filtra los resultados por nombre de correo electrónico. |
| `sortKey` | Selecciona el campo utilizado para ordenar los resultados. |
| `sortOrder` | Establece la dirección de ordenación. |
| `isCreatedByMe` | Limita los resultados a los recursos creados por el usuario actual. |
| `isModifiedByMe` | Limita los resultados a los recursos modificados por última vez por el usuario actual. |
| `templateId` | Filtra los resultados por identificador de plantilla. |
| `scriptEngine` | Filtra los resultados por la configuración del motor de secuencias de comandos. |
| `isValueNonNullable` | Filtra en función de si el valor no admite valores NULL. |
| `includeArchived` | Incluye recursos archivados en los resultados. |

#### Solicitud

```text
GET /rest/asset/v2/email/filter?workspaceId=1001&name=Spring%20Launch&status=draft&status=approved&pageIndex=0&pageSize=20
```

#### Respuesta

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "5dd1#1900ab13011",
  "result": [
    {
      "id": "1017",
      "name": "Spring Launch Email",
      "status": "draft"
    }
  ]
}
```

Utilice el parámetro `name` cuando necesite encontrar un correo electrónico con el nombre.

## Crear

Cree un correo electrónico enviando una carga útil JSON. Se requieren `name`, `appData` y `headers`. `headers.subject` es obligatorio y `appData` debe incluir al menos uno de `folderId`, `workspaceId` o `programId`.

### Solicitud

```text
POST /rest/asset/v2/email
Content-Type: application/json
```

```json
{
  "name": "Spring Launch Email",
  "description": "Main announcement email",
  "appData": {
    "workspaceId": "1001",
    "folderId": "2002",
    "editorType": "email"
  },
  "headers": {
    "subject": "Introducing the Spring Launch",
    "fromName": "Marketing Team",
    "fromEmail": "marketing@example.com",
    "replyEmail": "reply@example.com",
    "preheader": "See what changed this quarter",
    "ccEmails": [
      "owner@example.com"
    ]
  },
  "settings": {
    "isOperational": false,
    "isTextOnly": false,
    "isWebPageView": true,
    "enableUrlTracking": true
  },
  "templateId": "3003"
}
```

### Respuesta

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "238c#1900ab1313f",
  "result": [
    {
      "id": "1017",
      "name": "Spring Launch Email",
      "status": "draft"
    }
  ]
}
```

El cuerpo de la solicitud también puede incluir `data`, `editorContext`, `themeId`, `appType` y `status`.

### Crear campos

* `appData` identifica dónde se crea el correo electrónico y cómo se edita.
* `headers` contiene los valores del encabezado del mensaje, incluidos el asunto, el remitente, la dirección de respuesta, el encabezado previo y los destinatarios de CC opcionales.
* `settings` controla el comportamiento operativo y de procesamiento, como el modo de solo texto, la vista de página web y el seguimiento de URL.
* `templateId` identifica la plantilla de correo electrónico utilizada cuando se crea el correo electrónico.

## Actualización

Actualizar un correo electrónico por ID de recurso. El cuerpo de la solicitud utiliza el esquema `UpdateEmailRequest` y todas las propiedades son opcionales, de modo que solo puede enviar los campos que desee cambiar.

### Solicitud

```text
POST /rest/asset/v2/email/{id}/update
Content-Type: application/json
```

```json
{
  "description": "Updated announcement email",
  "headers": {
    "subject": "Spring Launch Is Live",
    "preheader": "Read the latest release notes"
  },
  "settings": {
    "enableUrlTracking": true,
    "isWebPageView": true
  }
}
```

### Respuesta

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "9fd3#1900ab13210",
  "result": [
    {
      "id": "1017"
    }
  ]
}
```

## Administrar estado

Los correos electrónicos utilizan un borrador y un ciclo de vida aprobado. Utilice el punto final de transición de estado para aprobar un correo electrónico, desaprobarlo, descartar un borrador o crear un nuevo borrador.

Los valores válidos de `action` son:

* `approve`
* `unapprove`
* `discard`
* `create_draft`

### Solicitud

```text
POST /rest/asset/v2/email/state/transition
Content-Type: application/json
```

### Respuesta

```json
{
  "contentId": "1017",
  "action": "approve"
}
```

## Clonar

Utilice el extremo de clonación para crear una copia de un correo electrónico existente.

### Solicitud

```text
POST /rest/asset/v2/email/clone
Content-Type: application/json
```

### Respuesta

```json
{
  "assetId": "1017",
  "newAsset": {
    "name": "Spring Launch Email Copy",
    "description": "Cloned from Spring Launch Email"
  }
}
```

## Eliminar

Eliminar un correo electrónico por ID de recurso.

### Solicitud

```text
POST /rest/asset/v2/email/{id}/delete
Content-Type: application/json
```

Este extremo toma el recurso `id` en la ruta y no define un cuerpo de solicitud.

## Utilizado por

Utilice el extremo `usedby` para recuperar recursos que hagan referencia a un correo electrónico determinado.

### Solicitud

```text
POST /rest/asset/v2/email/usedby
Content-Type: application/json
```

### Respuesta

```json
{
  "assetId": "1017",
  "pageIndex": 0,
  "pageSize": 20,
  "type": "all"
}
```

## Notas

Los extremos de consulta devuelven metadatos para el recurso. Utilice la referencia de extremo para el esquema completo de campos disponibles y cualquier propiedad específica del entorno.
