---
title: Instalación de la extensión [!DNL Adobe Launch]
feature: Mobile Marketing
description: Instale la extensión de Adobe Launch Marketo para dispositivos móviles. Siga los pasos de configuración de iOS y Android, dispositivos de prueba, permisos y FCM para notificaciones push y en la aplicación.
exl-id: d71b7cd7-309b-4882-9bba-7daaaa5ef32d
TQID: https://experienceleague.adobe.com/UZRHaRBISIZsE6E25Ee7CnnYwyZwi6w2YgOQJ-JL00U
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 753
ht-degree: 0%

---

# Instalación de la extensión [!DNL Adobe Launch]

Instale la extensión de Marketo [!DNL Adobe Launch] para enviar notificaciones push, mensajes en la aplicación o ambos.

## Prerrequisitos

1. [Agregue una aplicación al administrador de Marketo](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) y obtenga la clave secreta y el Munchkin Id de la aplicación.
1. [Configurar la propiedad en el [!DNL Adobe Launch] portal](https://experience.adobe.com/#/@amc/data-collection/home).
1. Configure la clave secreta de la aplicación y el identificador de Munchkin para la propiedad en el portal [!DNL Adobe Launch].
1. Opcional: [Configurar notificaciones push](push-notifications.md).

## Cómo instalar la extensión de Marketo en iOS

### Configurar encabezado de puente de Swift

1. Vaya a [!UICONTROL Archivo] > [!UICONTROL Nuevo] > [!UICONTROL Archivo] y seleccione **[!UICONTROL Archivo de encabezado]**.

1. Asigne un nombre al archivo &quot;&lt;_ProjectName_>-Bridging-Header&quot;.

1. Vaya a [!UICONTROL Proyecto] > [!UICONTROL Destino] > [!UICONTROL Configuración de compilación] > [!UICONTROL Compilador Swift] > [!UICONTROL Generación de código].
1. Añada la siguiente ruta al encabezado &quot;Objective-Bridging&quot;:

`$(PODS_ROOT)/<_ProjectName_>-Bridging-Header.h`

## Inicializar extensión

>[!BEGINTABS]

>[!TAB Objetivo C]

Actualice el método `applicationDidBecomeActive` de la siguiente manera.

```objectivec
(void)applicationDidBecomeActive:(UIApplication*) application
{
 [[ALMarketo sharedInstance] initializeMarketo:nil];
}
```

>[!TAB Swift]

Actualice el método `applicationDidBecomeActive` de la siguiente manera.

```objectivec
func applicationDidBecomeActive(_ application: UIApplication)
{
 ALMarketo.sharedInstance().initializeMarketo(nil)
}
```

>[!ENDTABS]

## Dispositivos de prueba iOS

1. Seleccione **[!UICONTROL Proyecto]** > **[!UICONTROL Destino]** > **[!UICONTROL Información]** > **[!UICONTROL Tipos de URL]**.
1. Agregue el identificador ${PRODUCT_NAME}.
1. Defina los esquemas de URL en mkto-&lt;S_secret_Key_>.
1. Agregar `application:openURL:sourceApplication:annotation:` a `AppDelegate.m file` para Objective-C.

### Administrar el tipo de URL personalizada en AppDelegate

>[!BEGINTABS]

>[!TAB Objetivo C]

```objectivec
#ifdef __IPHONE_10_0
-(BOOL)application:(UIApplication *)application
           openURL:(NSURL *)url
           options:(NSDictionary *)options{
    return [[ALMarketo sharedInstance] application:application
                                         openURL:url
                               sourceApplication:nil
                                      annotation:nil];
}
#endif

- (BOOL)application:(UIApplication *)application
            openURL:(NSURL *)url
  sourceApplication:(NSString *)sourceApplication
         annotation:(id)annotation {
    return [[ALMarketo sharedInstance] application:application
                                         openURL:url
                               sourceApplication:nil
                                      annotation:nil];
}
```

>[!TAB Swift]

```objectivec
func application(_ application: UIApplication, open url: URL, sourceApplication: String?, annotation: Any) -> Bool {
    return ALMarketo.sharedInstance().application(application, open: url, sourceApplication: nil, annotation: nil)
}
```

>[!ENDTABS]

## Cómo instalar Marketo SDK en Android

### Configuración de extensión de Android

Siga las instrucciones del portal [!DNL Adobe Launch].

### Configuración de permisos

Abra `AndroidManifest.xml` y agregue los siguientes permisos. La aplicación debe solicitar los permisos &quot;INTERNET&quot; y &quot;ACCESS_NETWORK_STATE&quot;. Omita este paso si la aplicación ya los solicita.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

## Inicializar extensión

Configuración de ProGuard (opcional)

Si su aplicación utiliza ProGuard, agregue las líneas siguientes al archivo `proguard.cfg` en la carpeta `project`. Esta configuración excluye la SDK de Marketo de la ofuscación.

```text
-dontwarn com.marketo.*
-dontnote com.marketo.*
-keep class com.marketo.**{ *; }
```

## Dispositivos de prueba Android

Agregue &quot;MarketoActivity&quot; a `AndroidManifest.xml` dentro de la etiqueta de la aplicación.

```xml
<activity android:name="com.marketo.MarketoActivity"  android:configChanges="orientation|screenSize" >
    <intent-filter android:label="MarketoActivity" >
        <action  android:name="android.intent.action.VIEW"/>
        <category  android:name="android.intent.category.DEFAULT"/>
        <category  android:name="android.intent.category.BROWSABLE"/>
        <data android:host="add_test_device" android:scheme="mkto" />
    </intent-filter>
</activity>
```

## Compatibilidad con Firebase Cloud Messaging

MME SDK para Android admite el uso directo de [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) (FCM) de Google.

### Añadir FCM a la aplicación

1. Integre la última versión de Marketo Android SDK en la aplicación de Android. Consulte los pasos en [GitHub](https://github.com/Marketo/android-sdk).
1. Configure la aplicación Firebase en la consola de Firebase.
   1. Cree o agregue un proyecto en [](https://accounts.google.com/ServiceLogin?passive=1209600&osid=1&continue=https://console.firebase.google.com/&followup=https://console.firebase.google.com/)Firebase Console.
      1. En la [consola de Firebase](https://accounts.google.com/ServiceLogin?passive=1209600&osid=1&continue=https://console.firebase.google.com/&followup=https://console.firebase.google.com/), seleccione **[!UICONTROL Agregar proyecto]**.
      1. Seleccione el proyecto GCM de la lista de proyectos existentes de Google Cloud y seleccione **[!UICONTROL Agregar Firebase]**.
      1. En la pantalla de bienvenida de Firebase, seleccione **[!UICONTROL Agregar Firebase a su aplicación de Android]**.
      1. Proporcione su nombre de paquete y SHA-1, y seleccione **[!UICONTROL Agregar aplicación]**. Se ha descargado un nuevo archivo de `google-services.json` para la aplicación Firebase.
      1. Seleccione **[!UICONTROL Continuar]** y siga las instrucciones detalladas para agregar el complemento de Google Services en Android Studio.

   1. Vaya a **[!UICONTROL Configuración del proyecto]** en [!UICONTROL Información general del proyecto].
      1. Seleccione la ficha **[!UICONTROL General]** y descargue `google-services.json`.
      1. Seleccione la ficha **[!UICONTROL Mensajería en la nube]**. Copie la [!UICONTROL clave del servidor] y el [!UICONTROL identificador de remitente], y proporciónelos a Marketo.
   1. Configure FCM en la aplicación de Android.
      1. Cambie a la vista Proyecto en Android Studio para mostrar el directorio raíz del proyecto.
         1. Mueva el archivo `google-services.json` descargado al directorio raíz del módulo de aplicaciones de Android.
         1. En el nivel de proyecto `build.gradle`, agregue lo siguiente:

            ```
            buildscript {
              dependencies {
                classpath 'com.google.gms:google-services:4.0.0'
              }
            }
            ```

         1. En App-level build.gradle, agregue lo siguiente:

            ```
            dependencies {
              compile 'com.google.firebase:firebase-core:17.4.0'
            }
            // Add to the bottom of the file
            apply plugin: 'com.google.gms.google-services'
            ```

         1. Seleccione **[!UICONTROL Sincronizar ahora]** en la barra que aparece en el IDE.
   1. Edite el manifiesto de la aplicación. FCM SDK agrega automáticamente los permisos necesarios y la funcionalidad del receptor. Elimine los siguientes elementos obsoletos, que podrían provocar la duplicación de mensajes:

      ```xml
      <uses-permission android:name="android.permission.WAKE_LOCK" />
      <permission android:name="<your-package-name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />
      <uses-permission android:name="<your-package-name>.permission.C2D_MESSAGE" />
      
      ...
      
      <receiver>
        android:name="com.google.android.gms.gcm.GcmReceiver"
        android:exported="true"
        android:permission="com.google.android.c2dm.permission.SEND">
        <intent-filter>
          <action android:name="com.google.android.c2dm.intent.RECEIVE" />
          <category android:name="<your-package-name> />
        </intent-filter>
      </receiver>
      ```

### Preguntas frecuentes sobre FCM

Estas preguntas abarcan la compatibilidad con Firebase Cloud Messaging.

**Q: ¿Dónde puedo encontrar instrucciones para actualizar a la última versión de MME SDK?** Consulte las [instrucciones de instalación](installation.md) en el sitio para desarrolladores de Marketo.

**Q: ¿La actualización a la versión más reciente de SDK requiere que publique una versión actualizada de mi aplicación de Android para mis usuarios existentes?** No.

**Q: ¿Cómo afecta a los clientes de MME existentes con aplicaciones de Android publicadas que usan Marketo Android SDK?** Migre una aplicación cliente GCM de Android existente a Firebase Cloud Messaging (FCM) de la siguiente manera:

1. En la [consola de Firebase](https://accounts.google.com/ServiceLogin?passive=1209600&osid=1&continue=https://console.firebase.google.com/&followup=https://console.firebase.google.com/), seleccione **[!UICONTROL Agregar proyecto]**.
1. Seleccione el proyecto GCM de la lista de proyectos existentes de Google Cloud y seleccione **[!UICONTROL Agregar Firebase]**.
1. En la pantalla de bienvenida de Firebase, seleccione **[!UICONTROL Agregar Firebase a su aplicación de Android]**.
1. Proporcione su nombre de paquete y SHA-1, y seleccione **[!UICONTROL Agregar aplicación]**. Se ha descargado un nuevo archivo google-services.json para la aplicación Firebase.
1. Seleccione **[!UICONTROL Continuar]** y siga las instrucciones detalladas para agregar el complemento de Google Services en Android Studio.

**Q: ¿Podemos segmentar clientes potenciales creados con el SDK Marketo anterior que usaba una aplicación GCM?** Sí. Puede segmentar todos los posibles clientes creados con Marketo SDK para las notificaciones push.
