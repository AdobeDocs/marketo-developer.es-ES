---
title: Instalación
feature: Mobile Marketing
description: Guía para instalar e inicializar Marketo Mobile SDK en iOS y Android mediante CocoaPods, Swift Package Manager o Gradle, lo que permite enviar mensajes push y en la aplicación.
exl-id: e0b79d85-3509-46d2-a77d-cee211c5ec7f
TQID: https://experienceleague.adobe.com/zYNoGPwJTQnqmP6CH0NDbmb-b8vAKRScMmms6vy0Sb4
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
  - id: f71e690b-4480-4b67-9ef5-88f42f9cdfdb
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 772
ht-degree: 0%

---

# Instalación

Instale e inicialice Marketo Mobile SDK para enviar notificaciones push, mensajes en la aplicación o ambos.

## Instalación de Marketo SDK en iOS

### Requisitos previos

1. [Agregue una aplicación al administrador de Marketo](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) y obtenga la clave secreta y el Munchkin Id de la aplicación.
1. Opcional: [Configurar notificaciones push](push-notifications.md).

### Instalar Framework mediante CocoaPods

1. Instale CocoaPods. `$ sudo gem install cocoapods`
1. Cambie el directorio al directorio del proyecto y cree un Podfile con valores predeterminados inteligentes. `$ pod init`
1. Abra el Podfile. `$ open -a Xcode Podfile`
1. Añada la siguiente línea a su Podfile. `$ pod 'Marketo-iOS-SDK'`
1. Guarde y cierre el Podfile.
1. Descargue e instale Marketo iOS SDK. `$ pod install`
1. Abra el espacio de trabajo en Xcode. `$ open App.xcworkspace`

### Instalación de Framework mediante el Administrador de paquetes Swift

1. Seleccione el proyecto en el Navegador de proyectos. En &quot;Añadir dependencia del paquete&quot;, seleccione &quot;+&quot;.

   ![Agregar dependencia](assets/dependency-manager-add.png)

1. Agregar el paquete de Marketo desde <https://github.com/Marketo/ios-sdk>.

   ![URL del repositorio](assets/dependency-manager-url.png)

1. Añada el paquete de recursos. Busque `MarketoFramework.XCframework` en el Navegador de proyectos y ábralo en el Buscador. Arrastre `MKTResources.bundle` para copiar los recursos del paquete.

### Configurar encabezado de puente de Swift

1. Vaya a Archivo > Nuevo > Archivo y seleccione &quot;Archivo de encabezado&quot;.

   ![Seleccionar &quot;Archivo de encabezado&quot;](assets/choose-header-file.png)

1. Asigne un nombre al archivo &quot;&lt;_ProjectName_>-Bridging-Header&quot;.

1. Vaya a Proyecto > Target > Fases de compilación > Compilador Swift > Generación de código. Añada la siguiente ruta al encabezado Objective-Bridging:

   `$(PODS_ROOT)/<_ProjectName_>-Bridging-Header.h`

   ![Fases de compilación](assets/build-phases.png)

## Inicializar SDK

Inicialice Marketo iOS SDK con su ID de cuenta de Munchkin y la clave secreta de la aplicación. Busque ambos valores en &quot;Aplicaciones y dispositivos móviles&quot; en Administración de Marketo.

1. Abra el archivo AppDelegate.m para Objective-C o el archivo puente para Swift. Importe el archivo de encabezado Marketo.h.

   ```
   #import <MarketoFramework/MarketoFramework.h>
   ```

1. Pegue el siguiente código dentro de la función `application:didFinishLaunchingWithOptions`:.

   Pase &quot;native&quot; como el tipo de framework para aplicaciones nativas.

>[!BEGINTABS]

>[!TAB Objetivo C]

```objectivec
Marketo *sharedInstance = [Marketo sharedInstance];

[sharedInstance initializeWithMunchkinID:@"munchkinAccountId" appSecret:@"secretKey" mobileFrameworkType:@"native" launchOptions:launchOptions];
```

>[!TAB Swift]

```swift
let sharedInstance: Marketo = Marketo.sharedInstance()

sharedInstance.initialize(withMunchkinID: "munchkinAccountId", appSecret: "secretKey", mobileFrameworkType: "native", launchOptions: launchOptions)
```

>[!ENDTABS]

1. Reemplace `munkinAccountId` y `secretKey` por el &quot;ID de cuenta de Munchkin&quot; y la &quot;clave secreta&quot; de Marketo **[!UICONTROL Admin]** > **[!UICONTROL Aplicaciones y dispositivos móviles]**.

## Dispositivos de prueba iOS

1. Seleccione Proyecto > Target > Información > Tipos de URL.
1. Agregue el identificador ${PRODUCT_NAME}.
1. Definir esquemas de URL en `mkto-<Secret Key_>`.
1. Agregue la aplicación :openURL:sourceApplication:annotation: al archivo AppDelegate.m para Objective-C.

## Administrar el tipo de URL personalizada en AppDelegate

>[!BEGINTABS]

>[!TAB Objetivo C]

```objectivec
- (BOOL)application:(UIApplication *)app
            openURL:(NSURL *)url
            options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options{

    return [[Marketo sharedInstance] application:app
                                         openURL:url
                                         options:options];
}
```

>[!TAB Swift]

```swift
private func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool
    {
        return Marketo.sharedInstance().application(app, open: url, options: options)
    }
```

>[!ENDTABS]

## Cómo instalar Marketo SDK en Android

### Requisitos previos

1. [Agregue una aplicación al administrador de Marketo](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) y obtenga la clave secreta y el Munchkin Id de la aplicación.
1. Opcional: [Configurar notificaciones push](push-notifications.md#android_setup_push).
1. [Descargar Marketo SDK para Android](https://codeload.github.com/Marketo/android-sdk/zip/refs/heads/master)

### Configuración de Android SDK con Gradle

1. En el archivo build.gradle de nivel de aplicación, agregue la dependencia en la sección de dependencias.

   `implementation 'com.marketo:MarketoSDK:0.8.9'`

1. Agregue la siguiente configuración al archivo raíz `build.gradle`.

   ```
   buildscript {
       repositories {
           google()
           mavenCentral()
       }
   ```

1. Sincronice el proyecto con los archivos de Gradle.

### Configuración de permisos

Abra `AndroidManifest.xml` y agregue los siguientes permisos. La aplicación debe solicitar los permisos &quot;INTERNET&quot; y &quot;ACCESS_NETWORK_STATE&quot;. Omita este paso si la aplicación ya los solicita.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

### Inicializar SDK

1. Abra la clase Application o Activity. Importe Marketo SDK en la actividad antes de setContentView o en el contexto de la aplicación.

   ```java
   // Initialize Marketo
   Marketo marketoSdk = Marketo.getInstance(getApplicationContext());
   marketoSdk.initializeSDK("native","munchkinAccountId","secretKey");
   ```

1. Configuración de ProGuard (opcional)

   Si su aplicación utiliza ProGuard, agregue las siguientes líneas al archivo `proguard.cfg` en la carpeta del proyecto. Esta configuración excluye la SDK de Marketo de la ofuscación.

   ```
   -dontwarn com.marketo.*
   -dontnote com.marketo.*
   -keep class com.marketo.`{ *; }
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
   1. Crear/agregar un proyecto en [&#128279;](https://accounts.google.com/ServiceLogin?passive=1209600&osid=1&continue=https://console.firebase.google.com/&followup=https://console.firebase.google.com/)consola Firebase.
      1. En la [consola de Firebase](https://accounts.google.com/ServiceLogin?passive=1209600&osid=1&continue=https://console.firebase.google.com/&followup=https://console.firebase.google.com/), seleccione `Add Project`.
      1. Seleccione el proyecto GCM de la lista de proyectos existentes de Google Cloud y seleccione `Add Firebase`.
      1. En la pantalla de bienvenida de Firebase, seleccione `Add Firebase to your Android App`.
      1. Proporcione el nombre del paquete y SHA-1, y seleccione `Add App`. Se ha descargado un nuevo archivo de `google-services.json` para la aplicación Firebase.
      1. Seleccione `Continue` y siga las instrucciones detalladas para agregar el complemento de Google Services en Android Studio.

   1. Vaya a &quot;Configuración del proyecto&quot; en Información general del proyecto
      1. Haga clic en la pestaña General. Descargue el archivo &quot;google-services.json&quot;.
      1. Haga clic en la pestaña &quot;Mensajería en la nube&quot;. Copie &quot;Clave de servidor&quot; e &quot;ID de remitente&quot;. Proporcione estas opciones &quot;Clave de servidor&quot; e &quot;ID de remitente&quot; a Marketo.
   1. Configure FCM en la aplicación de Android.
      1. Cambie a la vista Proyecto en Android Studio para ver el directorio raíz del proyecto
         1. Mueva el archivo &quot;google-services.json&quot; descargado al directorio raíz del módulo de la aplicación de Android
         1. En el archivo build.gradle de nivel de proyecto, agregue lo siguiente:

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

         1. Finalmente, selecciona **[!UICONTROL Sincronizar ahora]** en la barra que aparece en el identificador
   1. Edite el manifiesto de la aplicación. FCM SDK agrega automáticamente los permisos necesarios y la funcionalidad del receptor. Elimine los siguientes elementos obsoletos, que podrían provocar la duplicación de mensajes:

      ```xml
      <uses-permission android:name="android.permission.WAKE_LOCK" />
      <permission android:name="<your-package-name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />
      <uses-permission android:name="<your-package-name>.permission.C2D_MESSAGE" />
      
      ...
      
      <receiver>
        android:name="com.google.android.gms.gcm.GcmReceiver"
        android:exported="true"
        android:permission="com.google.android.c2dm.permission.SEND"
        <intent-filter>
          <action android:name="com.google.android.c2dm.intent.RECEIVE" />
          <category android:name="<your-package-name> />
        </intent-filter>
      </receiver>
      ```
