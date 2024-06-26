---
title: "Rendimiento"
feature: REST API
description: '"Sugerencias de rendimiento para trabajar con la API de Marketo".'
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 0%

---


# Desempeño

Esta página contiene una lista de temas relacionados con el rendimiento que puede utilizar para aumentar el rendimiento de la integración.

## Compresión HTTP

La API de REST de Marketo admite la compresión HTTP de los cuerpos de respuesta mediante estándares definidos por la especificación HTTP 1.1.  Se recomienda habilitar la compresión porque reduce el uso del ancho de banda y el tiempo empleado en recuperar datos.

**Nota:**  Las cargas inferiores a 1024 bytes no se comprimirán.

Para habilitar la compresión, incluya el siguiente encabezado HTTP en la solicitud:

```html
Accept-Encoding: gzip
```

La API de REST de Marketo comprimirá el cuerpo de respuesta e incluirá este encabezado:

```html
Content-Encoding: gzip
```

A continuación, se muestra un ejemplo con Curl para llamar a [Obtener posibles clientes por tipo de filtro](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadsByFilterUsingGET) punto final para recuperar 5 posibles clientes:

```bash
$ curl -H 'Accept-Encoding: gzip' 'https://123-ABC-456.mktorest.com/rest/v1/leads.json?filterType=id&filterValues=4,5,7,12,13'
```
