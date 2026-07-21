---
title: Extensión de Marketo Mobile para  [!DNL Adobe Launch]
feature: Mobile Marketing
description: Instale y configure la extensión de Marketo Mobile SDK en Adobe Launch para iOS y Android, incluida la configuración de notificaciones push y mensajes en la aplicación.
exl-id: 2f8691ff-0442-45a5-aeba-c91c3af5c711
TQID: https://experienceleague.adobe.com/Bk5GTnQjm6NDosl5Iw6TS-NRjH8owNRUKoE0mZ-H3pY
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 303
ht-degree: 0%

---

# Extensión de Marketo Mobile para [!DNL Adobe Launch]

Instale la extensión Marketo Mobile SDK en [!DNL Adobe Launch] para enviar notificaciones push, mensajes en la aplicación o ambos.

## Prerrequisitos

- [Agregue una aplicación al administrador de Marketo](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) y obtenga la clave secreta y el Munchkin Id de la aplicación.
- Siga las instrucciones de instalación del portal [!DNL Adobe Launch].
- Opcional: [Configurar notificaciones push](push-notifications.md).

## iOS

### Configurar encabezado de puente de Swift

1. Vaya a Archivo > Nuevo > Archivo y seleccione &quot;Archivo de encabezado&quot;.
1. Asigne un nombre al archivo &quot;&lt;_ProjectName_>-Bridging-Header&quot;.
1. Vaya a Proyecto > Target > Fases de compilación > Compilador Swift > Generación de código.
1. Añada la siguiente ruta al encabezado Objective-Bridging:

   `$(PODS_ROOT)/<_ProjectName_>-Bridging-Header.h`

Para Swift, elimine la siguiente instrucción import porque los pasos anteriores agregan el encabezado puente.

`import Marketo/ALMarketo`

### Dispositivos de prueba iOS

Siga las instrucciones de [Agregar dispositivos de prueba iOS](installation.md#ios_test_devices).

### Administrar el tipo de URL personalizada en AppDelegate

Siga las [instrucciones de dirección URL personalizada](installation.md#ios_test_devices).

### Configuración de notificaciones push en iOS

Siga las [instrucciones de notificación push](push-notifications.md). Utilice el nombre de clase &quot;ALMarketo&quot; en lugar de &quot;Marketo&quot;.

## Android

### Configuración de permisos

Abra `AndroidManifest.xml` y agregue los siguientes permisos. La aplicación debe solicitar los permisos &quot;INTERNET&quot; y &quot;ACCESS_NETWORK_STATE&quot;. Omita este paso si la aplicación ya los solicita.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

### Configuración de ProGuard (opcional)

Si su aplicación utiliza ProGuard, agregue las siguientes líneas al archivo `proguard.cfg` de la carpeta de su proyecto. Esta configuración excluye la SDK de Marketo de la ofuscación.

```text
-dontwarn com.marketo.*
-dontnote com.marketo.*
-keep class com.marketo.**{ *; }
```

### Dispositivos de prueba Android

Siga las instrucciones de [Dispositivos de prueba Android](installation.md#android_test_devices).

## Configuración de notificaciones push en Android

Siga las [instrucciones de Android Firebase Cloud Messaging](installation.md#android_firebase_cloud_messaging_support). Utilice el nombre de clase &quot;ALMarketo&quot; en lugar de &quot;Marketo&quot;.

Para configurar perfiles de usuario, siga las [instrucciones de perfil de usuario](user-profiles.md). Para configurar acciones personalizadas, siga las [instrucciones de acciones personalizadas](custom-actions.md#android_custom_action). En ambos conjuntos de instrucciones, utilice el nombre de clase &quot;ALMarketo&quot; en lugar de &quot;Marketo&quot;.
