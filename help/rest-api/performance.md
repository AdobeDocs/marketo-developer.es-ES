---
title: Desempeño
feature: REST API
description: Aumente el rendimiento de la API de REST de Marketo con la compresión HTTP. Habilite gzip para reducir el ancho de banda; API masivas no admitidas y por debajo de 1024 bytes no comprimidos.
exl-id: 173a398a-9d36-4e8d-9dd3-7d0d375b085a
TQID: https://experienceleague.adobe.com/foJCTd890HZtL-UzWx2cjRXwTxqgW56A79sB7FPEWis
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 00118a89f25a23b931fac671130932bb0e0e4e4e
workflow-type: tm+mt
source-wordcount: 146
ht-degree: 1%

---

# Desempeño

Esta página contiene una lista de temas relacionados con el rendimiento que puede utilizar para aumentar el rendimiento de la integración.

## Compresión HTTP

La API de REST de Marketo admite la compresión HTTP de los cuerpos de respuesta mediante estándares definidos por la especificación HTTP 1.1. Se recomienda habilitar la compresión porque reduce el uso del ancho de banda y el tiempo empleado en recuperar datos.

>[!NOTE]
>
>Las cargas inferiores a 1024 bytes no se comprimen y las API masivas no admiten compresión.

Para habilitar la compresión, incluya el siguiente encabezado HTTP en la solicitud:

```html
Accept-Encoding: gzip
```

La API de REST de Marketo comprimirá el cuerpo de respuesta e incluirá este encabezado:

```html
Content-Encoding: gzip
```

Este es un ejemplo de uso de Curl para llamar al extremo [Obtener posibles clientes por tipo de filtro](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/getLeadsByFilterUsingGET) para recuperar 5 posibles clientes:

```bash
curl -H 'Accept-Encoding: gzip' 'https://123-ABC-456.mktorest.com/rest/v1/leads.json?filterType=id&filterValues=4,5,7,12,13'
```
