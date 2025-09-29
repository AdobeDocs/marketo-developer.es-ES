---
title: Instalación
feature: Mobile Marketing
description: Guía para instalar e inicializar Marketo Mobile SDK en iOS y Android mediante CocoaPods, Swift Package Manager o Gradle, lo que permite enviar mensajes push y en la aplicación.
exl-id: e0b79d85-3509-46d2-a77d-cee211c5ec7f
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '779'
ht-degree: 0%

---

# Instalación

Instrucciones de instalación de Marketo Mobile SDK. Se requieren los pasos siguientes para enviar notificaciones push o mensajes en la aplicación.

## Instalación de Marketo SDK en iOS

### Requisitos previos

1. [Agregar una aplicación al administrador de Marketo](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (obtener la clave secreta de la aplicación y el identificador de Munchkin)
1. [Configurar notificaciones push](push-notifications.md) (opcional)

### Instalar Framework mediante CocoaPods

1. Instale CocoaPods. `$ sudo gem install cocoapods`
1. Cambie el directorio al directorio del proyecto y cree un Podfile con valores predeterminados inteligentes. `$ pod init`
1. Abra el Podfile. `$ open -a Xcode Podfile`
1. Añada la siguiente línea a su Podfile. `$ pod 'Marketo-iOS-SDK'`
1. Guarde y cierre el Podfile.
1. Descargue e instale Marketo iOS SDK. `$ pod install`
1. Abra el espacio de trabajo en Xcode. `$ open App.xcworkspace`

### Instalación de Framework mediante el Administrador de paquetes Swift

1. Seleccione el proyecto desde el Navegador de proyectos y en &quot;Añadir dependencia del paquete&quot;, haga clic en &quot;+&quot; como se muestra a continuación:

   ![Agregar dependencia](assets/dependency-manager-add.png)

1. Agregue el paquete Marketo de este repositorio. Agregar esta dirección URL para este repositorio: <https://github.com/Marketo/ios-sdk>.

   ![URL del repositorio](assets/dependency-manager-url.png)

1. Ahora agregue el paquete de recursos como se muestra: Busque `MarketoFramework.XCframework` en el navegador de proyectos y ábralo en el buscador. Arrastre y suelte `MKTResources.bundle` para copiar recursos del paquete.

### Configurar encabezado de puente de Swift

1. Vaya a Archivo > Nuevo > Archivo y seleccione &quot;Archivo de encabezado&quot;.

   ![Seleccionar &quot;Archivo de encabezado&quot;](assets/choose-header-file.png)

1. Asigne un nombre al archivo &quot;&lt;_ProjectName_>-Bridging-Header&quot;.

1. Vaya a Proyecto > Target > Fases de compilación > Compilador Swift > Generación de código. Añada la siguiente ruta al encabezado Objective-Bridging:

   `$(PODS_ROOT)/<_ProjectName_>-Bridging-Header.h`

   ![Fases de compilación](assets/build-phases.png)

## Inicializar SDK

Para poder usar Marketo iOS SDK, debe inicializarlo con el ID de cuenta de Munchkin y la clave secreta de la aplicación. Puede encontrar cada uno de estos elementos en el área de Administración de Marketo debajo de &quot;Aplicaciones y dispositivos móviles&quot;.

1. Abra el archivo AppDelegate.m (Objective-C) o el archivo puente (Swift) e importe el archivo de encabezado Marketo.h.

   ```
   #import <MarketoFramework/MarketoFramework.h>
   ```

1. Pegue el siguiente código dentro de la función `application:didFinishLaunchingWithOptions`:.

   Tenga en cuenta que debemos pasar &quot;native&quot; como tipo de marco de trabajo para las aplicaciones nativas.

>[!BEGINTABS]

>[!TAB Objetivo C]

```
Marketo *sharedInstance = [Marketo sharedInstance];

[sharedInstance initializeWithMunchkinID:@"munchkinAccountId" appSecret:@"secretKey" mobileFrameworkType:@"native" launchOptions:launchOptions];
```

>[!TAB Swift]

```
let sharedInstance: Marketo = Marketo.sharedInstance()

sharedInstance.initialize(withMunchkinID: "munchkinAccountId", appSecret: "secretKey", mobileFrameworkType: "native", launchOptions: launchOptions)
```

>[!ENDTABS]

1. Reemplace `munkinAccountId` y `secretKey` por su &quot;ID de cuenta de Munchkin&quot; y &quot;clave secreta&quot; que se encuentran en la sección **[!UICONTROL Administración]** > **[!UICONTROL Aplicaciones y dispositivos móviles]** de Marketo.

## Dispositivos de prueba iOS

1. Seleccione Proyecto > Target > Información > Tipos de URL.
1. Agregar identificador: ${PRODUCT_NAME}
1. Establecer esquemas de URL: `mkto-<Secret Key_>`
1. Incluir aplicación:openURL:sourceApplication:annotation: en el archivo AppDelegate.m (Objective-C)

## Administrar el tipo de URL personalizada en AppDelegate

>[!BEGINTABS]

>[!TAB Objetivo C]

```
- (BOOL)application:(UIApplication *)app
            openURL:(NSURL *)url
            options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options{

    return [[Marketo sharedInstance] application:app
                                         openURL:url
                                         options:options];
}
```

>[!TAB Swift]

```
private func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool
    {
        return Marketo.sharedInstance().application(app, open: url, options: options)
    }
```

>[!ENDTABS]

## Cómo instalar Marketo SDK en Android

### Requisitos previos

1. [Agregar una aplicación al administrador de Marketo](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (obtener la clave secreta de la aplicación y el identificador de Munchkin)
1. [Configurar notificaciones push](push-notifications.md#android_setup_push) (opcional)
1. [Descargar Marketo SDK para Android](https://codeload.github.com/Marketo/android-sdk/zip/refs/heads/master)

### Configuración de Android SDK con Gradle

1. En el archivo build.gradle del nivel de aplicación, en la sección de dependencias, agregue

`implementation 'com.marketo:MarketoSDK:0.8.9'`

1. El archivo raíz `build.gradle` debe tener

   ```
   buildscript {
       repositories {
           google()
           mavenCentral()
       }
   ```

1. Sincronizar el proyecto con los archivos de Gradle

### Configuración de permisos

Abra `AndroidManifest.xml` y agregue los siguientes permisos. La aplicación debe solicitar los permisos &quot;INTERNET&quot; y &quot;ACCESS_NETWORK_STATE&quot;. Si la aplicación ya solicita estos permisos, omita este paso.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

### Inicializar SDK

1. Abra la clase Application o Activity en la aplicación e importe Marketo SDK en la actividad antes de setContentView o en el contexto de la aplicación.

   ```java
   // Initialize Marketo
   Marketo marketoSdk = Marketo.getInstance(getApplicationContext());
   marketoSdk.initializeSDK("native","munchkinAccountId","secretKey");
   ```

1. Configuración de ProGuard (opcional)

   Si usa ProGuard para su aplicación, agregue las líneas siguientes al archivo `proguard.cfg`. El archivo se encuentra en la carpeta del proyecto. Añadir este código excluye el SDK de Marketo del proceso de ofuscación.

   ```
   -dontwarn com.marketo.*
   -dontnote com.marketo.*
   -keep class com.marketo.`{ *; }
   ```

## Dispositivos de prueba Android

Agregue &quot;MarketoActivity&quot; al archivo `AndroidManifest.xml` dentro de la etiqueta de la aplicación.

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

El kit de desarrollo de software de MME (SDK) para Android se ha actualizado a un marco de trabajo más moderno, estable y escalable que contiene más flexibilidad y nuevas funciones de ingeniería para el desarrollador de aplicaciones de Android.

Los desarrolladores de aplicaciones de Android ahora pueden usar directamente [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) (FCM) de Google con este SDK.

### Añadir FCM a la aplicación

1. Integre el último SDK de Marketo Android en la aplicación de Android.  Los pasos están disponibles en [GitHub](https://github.com/Marketo/android-sdk).
1. Configure la aplicación Firebase en la consola de Firebase.
   1. Crear/agregar un proyecto en [](https://accounts.google.com/ServiceLogin?passive=1209600&osid=1&continue=https://console.firebase.google.com/&followup=https://console.firebase.google.com/)consola Firebase.
      1. En la [consola de Firebase](https://accounts.google.com/ServiceLogin?passive=1209600&osid=1&continue=https://console.firebase.google.com/&followup=https://console.firebase.google.com/), seleccione `Add Project`.
      1. Seleccione el proyecto GCM de la lista de proyectos existentes de Google Cloud y seleccione `Add Firebase`.
      1. En la pantalla de bienvenida de Firebase, seleccione `Add Firebase to your Android App`.
      1. Proporcione el nombre del paquete y SHA-1, y seleccione `Add App`. Se ha descargado un nuevo archivo de `google-services.json` para la aplicación Firebase.
      1. Seleccione `Continue` y siga las instrucciones detalladas para agregar el complemento de Google Services en Android Studio.

   1. Vaya a &quot;Configuración del proyecto&quot; en Información general del proyecto
      1. Haga clic en la pestaña General. Descargue el archivo &quot;google-services.json&quot;.
      1. Haga clic en la pestaña &quot;Mensajería en la nube&quot;. Copie &quot;Clave de servidor&quot; e &quot;ID de remitente&quot;. Proporcione estas opciones &quot;Clave de servidor&quot; e &quot;ID de remitente&quot; a Marketo.
   1. Configuración de cambios de FCM en la aplicación de Android
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

         1. Finalmente, haga clic en &quot;Sincronizar ahora&quot; en la barra que aparece en el ID
   1. Edite el manifiesto de su aplicación: FCM SDK añade automáticamente todos los permisos necesarios y la funcionalidad del receptor requerida. Asegúrese de eliminar los siguientes elementos obsoletos (y potencialmente dañinos, ya que pueden provocar la duplicación de mensajes) del manifiesto de la aplicación:

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
