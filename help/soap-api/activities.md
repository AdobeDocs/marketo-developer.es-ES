---
title: Actividades
feature: SOAP
description: Aprenda a interactuar con actividades mediante SOAP, recuperar actividades de posible cliente y realizar un seguimiento de los cambios de los posibles clientes con getLeadActivities y getLeadChanges
exl-id: fd695ab6-e7be-4ced-89c9-c4cd2d4c2ab0
TQID: https://experienceleague.adobe.com/6zUkvoDCqlRmblFDPWzLjdwITsyWxcXrJBKbLux76WI
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: e71bcf289229867bc969345d79c8f014761aaaf9
workflow-type: tm+mt
source-wordcount: 79
ht-degree: 2%

---

# Actividades

Las siguientes llamadas de SOAP se pueden utilizar para interactuar con Actividades.

- [getLeadActivities](getleadactivity.md)
- [getLeadChanges](getleadchanges.md)

>[!CAUTION]
>
>A partir del 30 de diciembre de 2026, las llamadas a los extremos `Get Lead Activities` y `Get Lead Changes`, que incluye el parámetro `listId`, producirán un error (código de error 1003) si las listas de destino contienen 10 000 posibles clientes o más. Para evitar interrupciones en el servicio, asegúrese de que las llamadas de se dirijan correctamente a fin de evitar este límite. Consulte la [guía de migración](migration.md).
