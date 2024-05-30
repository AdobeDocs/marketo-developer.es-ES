---
title: "Acciones personalizadas"
feature: "Mobile Marketing"
description: "Resumen de acciones personalizadas"
source-git-commit: c51e1b84efdf444c13714c1a08ecc4cac677f483
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 0%

---


# Acciones personalizadas

Puede rastrear la interacción del usuario enviando acciones personalizadas. Cuando la aplicación móvil llama al SDK de Marketo para enviar una acción personalizada, esta se guarda inicialmente en el dispositivo. A continuación, el SDK de Marketo comprueba si hay una conectividad a Internet adecuada antes de enviar la acción personalizada. Como resultado, puede haber un retraso entre el momento en que se envía la acción personalizada y el momento en que Marketo la recibe.

Las acciones personalizadas se pueden utilizar como déclencheur en campañas inteligentes. Para obtener más información, consulte [Actividad de aplicación móvil](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/triggers-and-filters-for-mobile-smart-campaigns).

## Envío de acciones personalizadas en iOS

Enviar acción personalizada.

>[!BEGINTABS]

>[!TAB Objetivo C]

```
Marketo *sharedInstance = [Marketo sharedInstance];
[sharedInstance reportAction:@"Login" withMetaData:nil];
```

>[!TAB Swift]

```
sharedInstance.reportAction("Login", withMetaData:nil);
```

>[!ENDTABS]

Enviar acción personalizada con metadatos.

>[!BEGINTABS]

>[!TAB Objetivo C]

```
MarketoActionMetaData *meta = [[MarketoActionMetaData alloc] init];
[meta setType:@"Shopping"];
[meta setDetails:@"RedShirt"];
[meta setLength:20];
[meta setMetric:30];

[sharedInstance reportAction:@"Bought Shirt" withMetaData:meta];
```

>[!TAB Swift]

```
let meta = MarketoActionMetaData()
meta.setType("Shopping");
meta.setDetails("RedShirt");
meta.setLength(20);
meta.setMetric(30);

sharedInstance.reportAction("Bought Shirt", withMetaData:meta);
```

>[!ENDTABS]

Notificar todas las acciones inmediatamente (enviar todas las acciones guardadas).

>[!BEGINTABS]

>[!TAB Objetivo C]

```
[sharedInstance reportAll];
```

>[!TAB Swift]

```
sharedInstance.reportAll();
```

>[!ENDTABS]

## Envío de acciones personalizadas en Android

1. Enviar acción personalizada.

   ```
   Marketo.reportAction("Login", null);
   ```

1. Enviar acción personalizada con metadatos.

   ```
   MarketoActionMetaData meta = new MarketoActionMetaData();
   meta.setActionType("Shopping");
   meta.setActionDetails("RedShirt");
   meta.setActionLength("20");
   meta.setActionMetric("30");
   
   Marketo.reportAction("Bought Shirt", meta);
   ```

1. Notificar todas las acciones personalizadas inmediatamente (enviar todas las acciones guardadas).

   ```
   Marketo.reportAll();
   ```

## Solución de problemas de acciones personalizadas

La configuración de acciones personalizadas de dispositivos móviles es sencilla, pero existen restricciones en cuanto al número de caracteres que puede enviar desde el SDK de dispositivos móviles a Marketo. Asegúrese de que todas las acciones personalizadas que informan a Marketo a través del SDK móvil tengan menos de 20 caracteres.

**Nota sobre los casos de uso de varios usuarios en un dispositivo compartido:** Cuando un usuario inicia sesión en una aplicación móvil integrada con el SDK de Marketo, se realiza la primera llamada para asociar el posible cliente con la instalación de la aplicación. Una vez completada correctamente esta llamada, se podrán ver más actividades de usuario en la aplicación en el registro de actividades del posible cliente. Tenga en cuenta que, como se trata de una llamada asincrónica, si hay alguna acción personalizada registrada inmediatamente después del inicio de sesión, puede asociarse con el usuario que inició sesión anteriormente hasta que la llamada asociada se realice correctamente.
