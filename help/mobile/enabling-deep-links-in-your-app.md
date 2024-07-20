---
title: Activación de vínculos profundos
feature: Mobile Marketing
description: Instrucciones para activar los vínculos profundos
exl-id: c3647416-d81d-4f15-b660-bcb3e54cb9bc
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '376'
ht-degree: 0%

---

# Activación de vínculos profundos

La vinculación profunda le permite redirigir a las personas a contenido específico (recursos) dentro de la aplicación. Por ejemplo, cuando una persona hace clic en un mensaje push de un móvil que anuncia una camiseta morada, puede abrir la aplicación directamente al contenido de la camiseta morada (en lugar de a la página principal).

El proceso funciona de esta manera:

1. El usuario de Marketo coloca un URI personalizado en la acción táctil para su mensaje push.
1. Cuando una persona pulsa el mensaje push en su dispositivo, el SDK de Marketo MME déclencheur un evento con el URI personalizado.
1. A continuación, la aplicación procesa el evento y redirige a la persona al contenido adecuado en la aplicación.

Esto requiere definir una estructura de URI personalizada para la aplicación, registrar el esquema en el manifiesto de la aplicación y, a continuación, agregar código para procesar eventos de vínculos profundos y enrutar a la ubicación adecuada en la aplicación.

Para iOS, consulte la documentación de Apple sobre [Definición de un esquema de URL personalizado para su aplicación](https://developer.apple.com/documentation/xcode/defining-a-custom-url-scheme-for-your-app).

Para Android, consulte la documentación de Google sobre [Habilitar vínculos profundos para el contenido de la aplicación](https://developer.android.com/training/app-links/deep-linking).

En el caso de las aplicaciones de PhoneGap, la vinculación profunda no es tan directa como con las aplicaciones nativas de iOS o Android, pero hay complementos que permiten a la aplicación híbrida responder a esquemas de URL personalizados de vinculación profunda y vínculos universales y de aplicación tanto en iOS como en Android. Considera [estos complementos](https://cordova.apache.org/plugins/?q=deeplink).

Cuando haya habilitado la vinculación profunda en la aplicación, comparta los URI personalizados con los usuarios de Marketo para que puedan insertarlos en la Acción táctil para los mensajes push.

Marketo utiliza una estructura URI predefinida al configurar dispositivos de prueba. Consulte la sección &quot;Dispositivos de prueba&quot; de la [Guía de instalación](installation.md) para obtener más información.

## Prácticas recomendadas para definir una estructura de URI

Si la marca tiene un sitio móvil existente, se recomienda seguir también su estructura de URL para el URI de vínculo profundo. Por ejemplo, si `https://myappname.com/products/purple-shirt` es la dirección de su sitio web para el producto en cuestión, `myappname://products/purple-shirt` sería una buena estructura de URI de vínculo profundo para usar en su aplicación.

Por lo general, los esquemas deben ser exclusivos de la marca. Aunque actualmente no hay regulaciones para hacer que los esquemas sean únicos en todo el mundo, una manera de garantizar que los esquemas sean únicos es invertir el nombre de dominio (por ejemplo, `org.companyname`).
