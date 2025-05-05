---
title: Extensión de Marketo Mobile para  [!DNL Adobe Launch]
feature: Mobile Marketing
description: Información general sobre la extensión de Marketo Mobile para  [!DNL Adobe Launch]
exl-id: 2f8691ff-0442-45a5-aeba-c91c3af5c711
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 1%

---

# Extensión de Marketo Mobile para [!DNL Adobe Launch]

Instrucciones de instalación de la extensión del SDK de Marketo Mobile en [!DNL Adobe Launch]. Se requieren los pasos siguientes para enviar notificaciones push o mensajes en la aplicación.

## Prerrequisitos

- [Agregar una aplicación al administrador de Marketo](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (obtener la clave secreta de la aplicación y el identificador de Munchkin)
- Siga las instrucciones proporcionadas en el portal [!DNL Adobe Launch] para la instalación
- [Configurar notificaciones push](push-notifications.md) (opcional)

## iOS

### Configurar encabezado de puente de Swift

1. Vaya a Archivo > Nuevo > Archivo y seleccione &quot;Archivo de encabezado&quot;.
1. Asigne un nombre al archivo &quot;&lt;_ProjectName_>-Bridging-Header&quot;.
1. Vaya a Proyecto > Target > Fases de compilación > Compilador Swift > Generación de código. Añada la siguiente ruta al encabezado Objective-Bridging:

   `$(PODS_ROOT)/<_ProjectName_>-Bridging-Header.h`

Para usuarios de Swift: Elimine la siguiente instrucción import, ya que el encabezado puente se agrega en los pasos anteriores.

`import Marketo/ALMarketo`

### Dispositivos de prueba iOS

Siga las instrucciones en [Agregar dispositivos de prueba de iOS](installation.md#ios_test_devices)

### Administrar el tipo de URL personalizada en AppDelegate

Siga las instrucciones [aquí](installation.md#ios_test_devices)

### Configuración de notificaciones push en iOS

Siga las instrucciones [aquí](push-notifications.md) y use el nombre de clase &quot;ALMarketo&quot; en lugar de &quot;Marketo&quot;

## Android

### Configuración de permisos

Abra `AndroidManifest.xml` y agregue los siguientes permisos. La aplicación debe solicitar los permisos &quot;INTERNET&quot; y &quot;ACCESS_NETWORK_STATE&quot;. Si la aplicación ya solicita estos permisos, omita este paso.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

### Configuración de ProGuard (opcional)

Si usa ProGuard para su aplicación, agregue las líneas siguientes al archivo `proguard.cfg`. El archivo se encuentra en la carpeta del proyecto. Añadir este código excluye el SDK de Marketo del proceso de ofuscación.

```
-dontwarn com.marketo.*
-dontnote com.marketo.*
-keep class com.marketo.**{ *; }
```

### Dispositivos de prueba Android

Siga las instrucciones [aquí](installation.md#android_test_devices)

## Configuración de notificaciones push en Android

Siga las instrucciones [aquí](installation.md#android_firebase_cloud_messaging_support) y use el nombre de clase &quot;ALMarketo&quot; en lugar de &quot;Marketo&quot;

Para configurar perfiles de usuario, siga las instrucciones [aquí](user-profiles.md) y para acciones personalizadas, siga las instrucciones [aquí](custom-actions.md#android_custom_action). En las instrucciones siguientes, utilice el nombre de clase &quot;ALMarketo&quot; en lugar de &quot;Marketo&quot;
