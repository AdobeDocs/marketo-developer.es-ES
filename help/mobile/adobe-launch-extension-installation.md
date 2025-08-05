---
title: Instalación de la extensión [!DNL Adobe Launch]
feature: Mobile Marketing
description: Información general sobre la instalación de la extensión [!DNL Adobe Launch]
exl-id: d71b7cd7-309b-4882-9bba-7daaaa5ef32d
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '725'
ht-degree: 0%

---

# Instalación de la extensión [!DNL Adobe Launch]

Instrucciones de instalación para la extensión de Marketo [!DNL Adobe Launch]. Se requieren los pasos siguientes para enviar notificaciones push o mensajes en la aplicación.

## Requisitos previos

1. [Agregar una aplicación al administrador de Marketo](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (obtener la clave secreta de la aplicación y el identificador de Munchkin)
1. [Configurar la propiedad en [!DNL Adobe Launch] portal](https://experience.adobe.com/#/@amc/data-collection/home)
1. Configurar la clave secreta de la aplicación y el ID de Munchkin para la propiedad en el portal [!DNL Adobe Launch]
1. [Configurar notificaciones push](push-notifications.md) (opcional)

## Cómo instalar la extensión de Marketo en iOS

### Configurar encabezado de puente de Swift

1. Vaya a [!UICONTROL Archivo] > [!UICONTROL Nuevo] > [!UICONTROL Archivo] y seleccione **[!UICONTROL Archivo de encabezado]**.

1. Asigne un nombre al archivo &quot;&lt;_ProjectName_>-Bridging-Header&quot;.

1. Vaya a [!UICONTROL Proyecto] > [!UICONTROL Destino] > [!UICONTROL Configuración de compilación] > [!UICONTROL Compilador Swift] > [!UICONTROL Generación de código]. Añada la siguiente ruta al encabezado &quot;Objective-Bridging&quot;:

`$(PODS_ROOT)/<_ProjectName_>-Bridging-Header.h`

## Inicializar extensión

>[!BEGINTABS]

>[!TAB Objetivo C]

Actualice el método `applicationDidBecomeActive` como se muestra a continuación

```
(void)applicationDidBecomeActive:(UIApplication*) application
{
 [[ALMarketo sharedInstance] initializeMarketo:nil];
}
```

>[!TAB Swift]

Actualice el método `applicationDidBecomeActive` como se muestra a continuación

```
func applicationDidBecomeActive(_ application: UIApplication)
{
 ALMarketo.sharedInstance().initializeMarketo(nil)
}
```

>[!ENDTABS]

## Dispositivos de prueba iOS

1. Seleccione **[!UICONTROL Proyecto]** > **[!UICONTROL Destino]** > **[!UICONTROL Información]** > **[!UICONTROL Tipos de URL]**.
1. Agregar identificador: ${PRODUCT_NAME}
1. Definir esquemas de URL: mkto-&lt;S_secret_Key>
1. Incluir `application:openURL:sourceApplication:annotation:` a `AppDelegate.m file` (Objective-C)

### Administrar el tipo de URL personalizada en AppDelegate

>[!BEGINTABS]

>[!TAB Objetivo C]

```
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

```
func application(_ application: UIApplication, open url: URL, sourceApplication: String?, annotation: Any) -> Bool {
    return ALMarketo.sharedInstance().application(application, open: url, sourceApplication: nil, annotation: nil)
}
```

>[!ENDTABS]

## Cómo instalar Marketo SDK en Android

### Configuración de extensión de Android

Seguir instrucciones en el portal [!DNL Adobe Launch]

### Configuración de permisos

Abra `AndroidManifest.xml` y agregue los siguientes permisos. La aplicación debe solicitar los permisos &quot;INTERNET&quot; y &quot;ACCESS_NETWORK_STATE&quot;. Si la aplicación ya solicita estos permisos, omita este paso.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

## Inicializar extensión

Configuración de ProGuard (opcional)

Si usa ProGuard para su aplicación, agregue las líneas siguientes al archivo `proguard.cfg`. El archivo se encuentra dentro de su carpeta `project`. Añadir este código excluye el SDK de Marketo del proceso de ofuscación.

```
-dontwarn com.marketo.*
-dontnote com.marketo.*
-keep class com.marketo.**{ *; }
```

## Android  Prueba  Dispositivos

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

El kit de desarrollo de software de MME (SDK) para Android se ha actualizado a un marco de trabajo más moderno, estable y escalable que contiene más flexibilidad y nuevas funciones de ingeniería para el desarrollador de aplicaciones de Android.

Los desarrolladores de aplicaciones de Android ahora pueden usar directamente [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) (FCM) de Google con este SDK.

### Añadir FCM a la aplicación

1. Integre el último SDK de Marketo Android en la aplicación de Android.  Los pasos están disponibles en [GitHub](https://github.com/Marketo/android-sdk).
1. Configure la aplicación Firebase en la consola de Firebase.
   1. Crear/agregar un proyecto en [&#128279;](https://accounts.google.com/ServiceLogin?passive=1209600&osid=1&continue=https://console.firebase.google.com/&followup=https://console.firebase.google.com/)consola Firebase.
      1. En la [consola de Firebase](https://accounts.google.com/ServiceLogin?passive=1209600&osid=1&continue=https://console.firebase.google.com/&followup=https://console.firebase.google.com/), seleccione **[!UICONTROL Agregar proyecto]**.
      1. Seleccione el proyecto GCM de la lista de proyectos existentes de Google Cloud y seleccione **[!UICONTROL Agregar Firebase]**.
      1. En la pantalla de bienvenida de Firebase, seleccione **[!UICONTROL Agregar Firebase a su aplicación de Android]**.
      1. Proporcione su nombre de paquete y SHA-1, y seleccione **[!UICONTROL Agregar aplicación]**. Se ha descargado un nuevo archivo de `google-services.json` para la aplicación Firebase.
      1. Seleccione **[!UICONTROL Continuar]** y siga las instrucciones detalladas para agregar el complemento de Google Services en Android Studio.

   1. Vaya a **[!UICONTROL Configuración del proyecto]** en [!UICONTROL Información general del proyecto]
      1. Haga clic en la ficha **[!UICONTROL General]**. Descargue el archivo `google-services.json`.
      1. Haga clic en la ficha **[!UICONTROL Mensajería de nube]**. Copiar [!UICONTROL clave de servidor] y [!UICONTROL identificador de remitente]. Proporcione estas [!UICONTROL clave de servidor] y [!UICONTROL ID de remitente] a Marketo.
   1. Configuración de cambios de FCM en la aplicación de Android
      1. Cambie a la vista Proyecto en Android Studio para ver el directorio raíz del proyecto
         1. Mueva el archivo `google-services.json` descargado al directorio raíz del módulo de la aplicación de Android
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

         1. Finalmente, haz clic en **[!UICONTROL Sincronizar ahora]** en la barra que aparece en el identificador
   1. Edite el manifiesto de su aplicación: FCM SDK añade automáticamente todos los permisos necesarios y la funcionalidad del receptor requerida. Asegúrese de eliminar los siguientes elementos obsoletos (y potencialmente dañinos, ya que pueden provocar la duplicación de mensajes) del manifiesto de la aplicación:

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

Preguntas frecuentes sobre la compatibilidad con Firebase Cloud Messaging.

**Q: ¿Dónde puedo encontrar instrucciones para actualizar a la versión más reciente de MME SDK?** Las instrucciones se encuentran en el sitio para desarrolladores de Marketo [AQUÍ](installation.md).

**Q: ¿La actualización a la versión más reciente de SDK requiere que publique una versión actualizada de mi aplicación de Android para mis usuarios existentes?N.º**

**Q: ¿Cómo afecta a los clientes de MME existentes que han publicado aplicaciones de Android integradas con Marketo Android SDK?**: pueden migrar una aplicación cliente GCM existente en Android a Firebase Cloud Messaging (FCM) de la siguiente manera:

1. En la [consola de Firebase](https://accounts.google.com/ServiceLogin?passive=1209600&osid=1&continue=https://console.firebase.google.com/&followup=https://console.firebase.google.com/), seleccione **[!UICONTROL Agregar proyecto]**.
1. Seleccione el proyecto GCM de la lista de proyectos existentes de Google Cloud y seleccione **[!UICONTROL Agregar Firebase]**.
1. En la pantalla de bienvenida de Firebase, seleccione **[!UICONTROL Agregar Firebase a su aplicación de Android]**.
1. Proporcione su nombre de paquete y SHA-1, y seleccione **[!UICONTROL Agregar aplicación]**. Un nuevo archivo google-services.json para su
1. Aplicación Firebase descargada.
1. Seleccione **[!UICONTROL Continuar]** y siga las instrucciones detalladas para agregar el complemento de Google Services en Android Studio.

**Q: ¿Podemos segmentar los posibles clientes creados con la antigua SDK de Marketo que usaba la aplicación GCM?** Sí. Todos los posibles clientes creados con Marketo SDK se pueden segmentar para enviar las notificaciones push.
