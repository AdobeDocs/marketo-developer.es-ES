---
title: Fragmentos
feature: REST API
description: Utilice la API de REST de Marketo Asset para consultar, crear, actualizar, clonar, eliminar, aprobar e inspeccionar las dependencias de fragmentos.
exl-id: 9dd532d1-1dd7-4581-86dd-1943fab66cbb
source-git-commit: 0e0a3e5a08e81f349044cbc327d1aba963ab30e4
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 9%

---

# Fragmentos

Los fragmentos son recursos de contenido reutilizables a los que otros recursos pueden hacer referencia, como correos electrónicos.

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

Puede recuperar los metadatos del fragmento por ID de recurso o con el punto final del filtro.

### Por identificador

#### Solicitud

```text
GET /rest/asset/v2/fragment/{id}
```

#### Respuesta

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "fa0f#1900ab15001",
  "result": [
    {
      "id": "83",
      "name": "Hero Banner Fragment",
      "description": "Reusable hero block",
      "status": "approved"
    }
  ]
}
```

### Filtro

El punto final del filtro admite la búsqueda dentro de un espacio de trabajo y reduce los resultados con parámetros de consulta adicionales. Se requiere `workspaceId`.

todo: convertir esto en una tabla
Los filtros admitidos son `folderId`, `folderIds` repetido, `status` repetido, `pageIndex`, `pageSize`, `createdBy`, `createdAtStart`, `createdAtEnd`, `modifiedBy`, `modifiedAtStart`, `modifiedAtEnd`, `name`, `fragmentType`, `sortKey`, `sortOrder`, `isCreatedByMe`, `isModifiedByMe`, `scriptEngine`, `isValueNonNullable` y `includeArchived`.

#### Solicitud

```text
GET /rest/asset/v2/fragment/filter?workspaceId=1001&fragmentType=email&pageIndex=0&pageSize=20
```

#### Respuesta

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "f9cc#1900ab1504a",
  "result": [
    {
      "id": "83",
      "name": "Hero Banner Fragment",
      "status": "approved"
    }
  ]
}
```

Utilice el parámetro `name` cuando necesite encontrar un fragmento por nombre.

## Crear

Cree un fragmento enviando una carga útil JSON. Se requieren `name`, `appData` y `settings`. `settings` debe incluir `fragmentType` y `supportedChannels`.

### Solicitud

```text
POST /rest/asset/v2/fragment
Content-Type: application/json
```

```json
{
  "name": "Hero Banner Fragment",
  "description": "Reusable hero block",
  "appData": {
    "workspaceId": "1001",
    "folderId": "395",
    "editorType": "fragment"
  },
  "settings": {
    "fragmentType": "email",
    "fragmentSubType": "html",
    "supportedChannels": [
      "email"
    ]
  }
}
```

### Respuesta

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "bd57#1900ab1509d",
  "result": [
    {
      "id": "13",
      "name": "Hero Banner Fragment",
      "status": "draft"
    }
  ]
}
```

El cuerpo de la solicitud también puede incluir `data`, `editorContext`, `themeId`, `appType` y `status`.

### Crear campos

* `appData` identifica dónde se almacena el fragmento y cómo se edita.
* `settings.fragmentType` identifica la categoría del fragmento, como un fragmento de correo electrónico.
* `settings.fragmentSubType` se puede usar para definir aún más el formato del fragmento.
* `settings.supportedChannels` enumera los canales en los que se puede utilizar el fragmento.

## Actualización

Actualizar un fragmento por ID de recurso.

### Solicitud

```text
POST /rest/asset/v2/fragment/{id}/update
Content-Type: application/json
```

```json
{
  "name": "Hero Banner Fragment v2",
  "description": "Updated reusable hero block",
  "settings": {
    "fragmentSubType": "html",
    "supportedChannels": [
      "email"
    ]
  }
}
```

### Respuesta

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "73d9#1900ab150f0",
  "result": [
    {
      "id": "13"
    }
  ]
}
```

## Administrar estado

Los fragmentos utilizan un ciclo de vida en borrador y aprobado. Utilice el punto final de transición de estado para aprobar un fragmento, desaprobarlo, descartar un borrador o crear un borrador nuevo.

Los valores válidos de `action` son:

* `approve`
* `unapprove`
* `discard`
* `create_draft`

### Solicitud

```text
POST /rest/asset/v2/fragment/state/transition
Content-Type: application/json
```

### Respuesta

```json
{
  "contentId": "13",
  "action": "approve"
}
```

## Clonar

Utilice el extremo de clonación para crear una copia de un fragmento existente.

### Solicitud

```text
POST /rest/asset/v2/fragment/clone
Content-Type: application/json
```

### Respuesta

```json
{
  "assetId": "13",
  "newAsset": {
    "name": "Hero Banner Fragment Copy",
    "description": "Cloned fragment"
  }
}
```

## Eliminar

Eliminar un fragmento por ID de recurso.

### Solicitud

```text
POST /rest/asset/v2/fragment/{id}/delete
Content-Type: application/json
```

Este extremo toma el ID de fragmento en la ruta y no define un cuerpo de solicitud.

## Utilizado por

Utilice el extremo `usedby` para recuperar recursos que hacen referencia a un fragmento determinado.

### Solicitud

```text
POST /rest/asset/v2/fragment/usedby
Content-Type: application/json
```

### Respuesta

```json
{
  "assetId": "13",
  "pageIndex": 0,
  "pageSize": 20,
  "type": "all"
}
```
