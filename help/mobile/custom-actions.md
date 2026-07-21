---
title: Acciones personalizadas
feature: Mobile Marketing
description: Aprenda a enviar e informar sobre acciones personalizadas con Marketo Mobile SDK para iOS y Android, poner en cola sin conexión, déclencheur campañas inteligentes y cumplir los 20 caracteres...
exl-id: 8c2698ce-4e39-4b2b-9d36-0864c55be17a
TQID: https://experienceleague.adobe.com/yZKzdm-dH0cYPGGKE-Z-4KcbhGIwyFl0Z9vEqcv1QXI
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: a7170d27-32ab-462b-a333-269abc654483
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dcid: c1579802-ddd4-4214-8a91-97b2066abe11id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 259
ht-degree: 2%

---

# Acciones personalizadas

Las acciones personalizadas rastrean las interacciones del usuario en la aplicación móvil. Cuando la aplicación llama al SDK de Marketo para enviar una acción personalizada, el SDK guarda primero la acción en el dispositivo. SDK envía la acción una vez que detecta una conectividad adecuada a Internet, por lo que Marketo podría recibir la acción después de un retraso.

Las acciones personalizadas se pueden utilizar como déclencheur en campañas inteligentes. Para obtener más información, consulte [Actividad de aplicaciones móviles](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/triggers-and-filters-for-mobile-smart-campaigns).

## Envío de acciones personalizadas en iOS

Envíe una acción personalizada.

>[!BEGINTABS]

>[!TAB Objetivo C]

```objectivec
Marketo *sharedInstance = [Marketo sharedInstance];
[sharedInstance reportAction:@"Login" withMetaData:nil];
```

>[!TAB Swift]

```swift
sharedInstance.reportAction("Login", withMetaData:nil);
```

>[!ENDTABS]

Envíe una acción personalizada con metadatos.

>[!BEGINTABS]

>[!TAB Objetivo C]

```objectivec
MarketoActionMetaData *meta = [[MarketoActionMetaData alloc] init];
[meta setType:@"Shopping"];
[meta setDetails:@"RedShirt"];
[meta setLength:20];
[meta setMetric:30];

[sharedInstance reportAction:@"Bought Shirt" withMetaData:meta];
```

>[!TAB Swift]

```swift
let meta = MarketoActionMetaData()
meta.setType("Shopping");
meta.setDetails("RedShirt");
meta.setLength(20);
meta.setMetric(30);

sharedInstance.reportAction("Bought Shirt", withMetaData:meta);
```

>[!ENDTABS]

Notificar todas las acciones guardadas inmediatamente.

>[!BEGINTABS]

>[!TAB Objetivo C]

```objectivec
[sharedInstance reportAll];
```

>[!TAB Swift]

```swift
sharedInstance.reportAll();
```

>[!ENDTABS]

## Envío de acciones personalizadas en Android

1. Envíe una acción personalizada.

   ```
   Marketo.reportAction("Login", null);
   ```

1. Envíe una acción personalizada con metadatos.

   ```
   MarketoActionMetaData meta = new MarketoActionMetaData();
   meta.setActionType("Shopping");
   meta.setActionDetails("RedShirt");
   meta.setActionLength("20");
   meta.setActionMetric("30");
   
   Marketo.reportAction("Bought Shirt", meta);
   ```

1. Informar de todas las acciones personalizadas guardadas inmediatamente.

   ```
   Marketo.reportAll();
   ```

## Solución de problemas de acciones personalizadas

Los nombres de las acciones personalizadas enviadas desde Mobile SDK a Marketo deben tener menos de 20 caracteres.

**Casos de uso de varios usuarios en un dispositivo compartido:** Cuando un usuario inicia sesión en una aplicación móvil que usa Marketo SDK, la primera llamada asocia al posible cliente con la instalación de la aplicación. Una vez que la llamada se realiza correctamente, las actividades de usuario subsiguientes aparecen en el registro de actividades del posible cliente.

La llamada de asociación es asincrónica. Las acciones personalizadas registradas inmediatamente después del inicio de sesión pueden asociarse con el usuario que ha iniciado sesión anteriormente hasta que la llamada se realice correctamente.
