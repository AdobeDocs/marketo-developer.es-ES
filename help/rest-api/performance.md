---
title: Desempeño
feature: REST API
description: Aumente el rendimiento de la API de REST de Marketo con la compresión HTTP. Habilite gzip para reducir el ancho de banda; API masivas no admitidas y por debajo de 1024 bytes no comprimidos.
exl-id: 173a398a-9d36-4e8d-9dd3-7d0d375b085a
TQID: 'https://experienceleague.adobe.com/foJCTd890HZtL-UzWx2cjRXwTxqgW56A79sB7FPEWis'
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
    internal-label: Marketo Engage
feature_v2:
  - id: dca84292-69e9-4116-a575-667d31fa060d
    internal-label: APIs
subfeature_v2:
  - id: cf1396d8-ab85-4e93-b35d-d9b573024abf
    internal-label: REST APIs
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
source-git-commit: 5620f050ba834be3f6648650b5cc7d781ea394bf
workflow-type: tm+mt
source-wordcount: '129'
ht-degree: 1%
---
# Desempeño

Utilice las opciones de rendimiento de esta página para mejorar la eficacia de la integración.

## Compresión HTTP

La API de REST de Marketo admite la compresión del cuerpo de respuesta HTTP tal como se define en la especificación HTTP 1.1. Permita la compresión para reducir el uso del ancho de banda y el tiempo de recuperación de datos.

>[!NOTE]
>
>Las cargas inferiores a 1024 bytes no se comprimen y las API masivas no admiten compresión.

Para habilitar la compresión, incluya el siguiente encabezado HTTP en la solicitud:

```html
Accept-Encoding: gzip
```

La API de REST de Marketo comprime el cuerpo de respuesta e incluye el siguiente encabezado:

```html
Content-Encoding: gzip
```

El siguiente ejemplo de cURL llama al extremo [Obtener posibles clientes por tipo de filtro](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadsByFilterUsingGET) para recuperar cinco posibles clientes:

```bash
curl -H 'Accept-Encoding: gzip' 'https://123-ABC-456.mktorest.com/rest/v1/leads.json?filterType=id&filterValues=4,5,7,12,13'
```
