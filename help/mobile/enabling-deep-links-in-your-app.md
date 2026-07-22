---
title: Activación de vínculos profundos
feature: Mobile Marketing
description: Obtenga información sobre cómo habilitar vínculos profundos en la aplicación para mensajes push de Marketo mediante esquemas de URI personalizados, con directrices y prácticas recomendadas de iOS, Android y PhoneGap.
exl-id: c3647416-d81d-4f15-b660-bcb3e54cb9bc
TQID: https://experienceleague.adobe.com/UswOvHXGlfTrTUqr4Gsf3j2Z7Xpv2FF2luXeygT4qE0
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: f71e690b-4480-4b67-9ef5-88f42f9cdfdb
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 363
ht-degree: 1%

---

# Activación de vínculos profundos

La vinculación profunda dirige a las personas a contenido específico en la aplicación. Por ejemplo, cuando una persona selecciona un mensaje push para móvil que anuncia una camiseta morada, la aplicación puede abrir el contenido de la camiseta morada en lugar de la página de inicio.

El proceso funciona de esta manera:

1. Un usuario de Marketo coloca un URI personalizado en Acción táctil para un mensaje push.
1. Cuando una persona pulse el mensaje push en su dispositivo, Marketo ME SDK activará un evento con el URI personalizado.
1. La aplicación procesa el evento y dirige a la persona al contenido correspondiente.

Para habilitar este proceso:

1. Defina una estructura de URI personalizada para su aplicación.
1. Registre el esquema en el manifiesto de la aplicación.
1. Agregue código que procese eventos de vínculos profundos y dirija a las personas al contenido correspondiente.

Para iOS, consulte la documentación de Apple sobre [Definición de un esquema de URL personalizado para su aplicación](https://developer.apple.com/documentation/xcode/defining-a-custom-url-scheme-for-your-app).

Para Android, consulte la documentación de Google sobre [Habilitar vínculos profundos para el contenido de la aplicación](https://developer.android.com/training/app-links/deep-linking).

En el caso de las aplicaciones de PhoneGap, utilice un complemento para permitir que la aplicación híbrida responda a esquemas de URL personalizados y vínculos universales y de aplicación en iOS y Android. Ver los [complementos de vinculación profunda](https://cordova.apache.org/plugins/?q=deeplink) disponibles.

Cuando haya habilitado la vinculación profunda en la aplicación, comparta los URI personalizados con los usuarios de Marketo para que puedan insertarlos en la Acción táctil para los mensajes push.

Marketo utiliza una estructura URI predefinida al configurar dispositivos de prueba. Para obtener más información, consulte &quot;Dispositivos de prueba&quot; en la [Guía de instalación](installation.md).

## Prácticas recomendadas para definir una estructura de URI

Si la marca tiene un sitio móvil, siga su estructura URL cuando defina el URI de vínculo profundo. Por ejemplo, si la dirección URL del producto es `https://myappname.com/products/purple-shirt`, use `myappname://products/purple-shirt` como URI de vínculo profundo correspondiente.

Utilice un esquema único para su marca. Aunque ninguna regulación requiere que los esquemas sean únicos a nivel global, puede ayudar a crear un esquema único invirtiendo su nombre de dominio, como `org.companyname`.
