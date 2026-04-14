---
title: Plantillas de correo electrónico
feature: REST API
description: Utilice la API de REST de Marketo Asset para consultar, crear, actualizar, clonar, eliminar, aprobar e inspeccionar las dependencias de las plantillas de correo electrónico.
exl-id: 50bb0047-d6ea-4c94-a900-18c37b17a147
source-git-commit: 59684e1c5a8082ad12f1e4bfc854c0d2dde35d2a
workflow-type: tm+mt
source-wordcount: '292'
ht-degree: 9%

---

# Plantillas de correo electrónico

[Referencia de extremo de plantilla de correo electrónico](https://developer.adobe.com/marketo-apis/api/asset#tag/Email-Templates)

Las plantillas de correo electrónico definen la estructura y el diseño reutilizables utilizados al crear correos electrónicos.

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

Puede recuperar los metadatos de las plantillas de correo electrónico por ID de recurso o con el punto final del filtro.

### Por identificador

#### Solicitud

```http
GET /rest/asset/v2/emailtemplate/{id}
```

#### Respuesta

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "14f9e#1900ab14001",
  "result": [
    {
      "id": "19",
      "name": "Base Newsletter Template",
      "description": "Core responsive template",
      "status": "draft"
    }
  ]
}
```

### Filtro

El punto final del filtro admite la búsqueda dentro de un espacio de trabajo y reduce los resultados con parámetros de consulta adicionales. Se requiere `workspaceId`.

Los filtros admitidos son `folderId`, `folderIds` repetido, `status` repetido, `pageIndex`, `pageSize`, `createdBy`, `createdAtStart`, `createdAtEnd`, `modifiedBy`, `modifiedAtStart`, `modifiedAtEnd`, `name`, `sortKey`, `sortOrder`, `isCreatedByMe`, `isModifiedByMe`, `scriptEngine`, `isValueNonNullable` y `includeArchived`.

#### Solicitud

```http
GET /rest/asset/v2/emailtemplate/filter?workspaceId=1001&name=Newsletter&pageIndex=0&pageSize=20
```

#### Respuesta

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "33c4#1900ab1402f",
  "result": [
    {
      "id": "19",
      "name": "Base Newsletter Template",
      "status": "draft"
    }
  ]
}
```

Utilice el parámetro `name` cuando necesite encontrar una plantilla por su nombre.

## Crear

Cree una plantilla de correo electrónico enviando una carga útil JSON. Se requieren `name` y `appData`. `appData` debe incluir al menos `folderId` o `workspaceId`.

### Solicitud

```http
POST /rest/asset/v2/emailtemplate
Content-Type: application/json
```

```json
{
  "name": "Base Newsletter Template",
  "description": "Core responsive template",
  "appData": {
    "workspaceId": "1001",
    "folderId": "15",
    "editorType": "emailTemplate"
  },
  "themeId": "42"
}
```

### Respuesta

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "a99f#1900ab1407e",
  "result": [
    {
      "id": "1022",
      "name": "Base Newsletter Template",
      "status": "draft"
    }
  ]
}
```

El cuerpo de la solicitud también puede incluir `data`, `editorContext`, `appType` y `status`.

### Crear campos

`appData` identifica dónde se almacena la plantilla y cómo se edita.

`themeId` se puede usar para asociar un tema con la plantilla cuando corresponda.

## Actualización

Actualizar una plantilla por ID de recurso.

### Solicitud

```http
POST /rest/asset/v2/emailtemplate/{id}/update
Content-Type: application/json
```

```json
{
  "name": "Base Newsletter Template v2",
  "description": "Updated responsive template",
  "appData": {
    "folderId": "15"
  }
}
```

### Respuesta

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "cf10#1900ab140b3",
  "result": [
    {
      "id": "1022"
    }
  ]
}
```

## Administrar estado

Las plantillas de correo electrónico utilizan un ciclo vital de borrador y aprobado. Utilice el punto final de transición de estado para aprobar una plantilla, desaprobarla, descartar un borrador o crear un borrador nuevo.

Los valores válidos de `action` son:

- `approve`
- `unapprove`
- `discard`
- `create_draft`

### Solicitud

```http
POST /rest/asset/v2/emailtemplate/state/transition
Content-Type: application/json
```

### Respuesta

```json
{
  "contentId": "1022",
  "action": "approve"
}
```

## Clonar

Utilice el extremo de clonación para crear una copia de una plantilla existente.

### Solicitud

```http
POST /rest/asset/v2/emailtemplate/clone
Content-Type: application/json
```

### Respuesta

```json
{
  "assetId": "1022",
  "newAsset": {
    "name": "Base Newsletter Template Copy",
    "description": "Cloned template"
  }
}
```

## Eliminar

Eliminar una plantilla por ID de recurso.

### Solicitud

```http
POST /rest/asset/v2/emailtemplate/{id}/delete
Content-Type: application/json
```

Este extremo toma el ID de plantilla en la ruta y no define un cuerpo de solicitud.

## Utilizado por

Utilice el extremo `usedby` para recuperar recursos que hacen referencia a una plantilla determinada.

### Solicitud

```http
POST /rest/asset/v2/emailtemplate/usedby
Content-Type: application/json
```

### Respuesta

```json
{
  "assetId": "1022",
  "pageIndex": 0,
  "pageSize": 20,
  "type": "all"
}
```

## Notas

Los extremos de consulta devuelven metadatos para el recurso. Utilice la referencia de extremo para el esquema de respuesta completo y cualquier propiedad específica del entorno.
