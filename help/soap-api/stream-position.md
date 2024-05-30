---
title: "Posición del flujo"
feature: SOAP
description: "Resumen de posición de vapor"
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 0%

---


# Posición del flujo

Los elementos de posición de la secuencia contienen una referencia de posición para uno o más flujos lógicos de datos secuenciados en tiempo. La referencia de posición puede ser una especificación externa aproximada como una marca de tiempo o una especificación interna opaca de posición devuelta por una llamada de API anterior. Las posiciones de las secuencias pueden definirse como un tipo complejo de varios elementos o pueden ser una cadena.

La posición de la secuencia se utiliza para recuperar datos por lotes y permite al llamador paginar a través del resultado. La posición de flujo pasada dentro de una solicitud de API es el valor de la posición de flujo devuelta en la respuesta anterior. No se recomienda modificar la posición del flujo devuelta por la llamada de API anterior, lo que puede provocar un comportamiento inesperado de la API.

## API que admiten la posición del flujo

- [getCustomObjects](getcustomobjects.md)
- [getLeadChanges](getleadchanges.md)
- [getLeadActivity](getleadactivity.md)
- [getMObjects](getmobjects.md)
- [getMultipleLeads](getmultipleleads.md)

## Posición de flujo simple

```
<streamPosition>8UJZetaMb1V6uUZl+L7DcPP2jG+PMmtpF</streamPosition>
```

## Posición de flujo complejo

```xml
<startPosition>
  <latestCreatedAt  />
  <oldestCreatedAt>2013-08-01T00:13:13+00:00</oldestCreatedAt>
  <activityCreatedAt  />
  <offset>ID:1086173</offset>
</startPosition>
```
