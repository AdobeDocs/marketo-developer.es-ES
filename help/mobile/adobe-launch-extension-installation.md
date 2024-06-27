---
title: '[!DNL Adobe Launch] Instalación de extensión'
feature: Mobile Marketing
description: '''[!DNL Adobe Launch] resumen de instalación de extensiones'
exl-id: d71b7cd7-309b-4882-9bba-7daaaa5ef32d
source-git-commit: 2b6fd22becb0eb692dbcf48686c23f7a878cd812
workflow-type: tm+mt
source-wordcount: '726'
ht-degree: 0%

---

# [!DNL Adobe Launch] Instalación de extensiones

Instrucciones de instalación para [!DNL Adobe Launch] Extensión de Marketo. Se requieren los pasos siguientes para enviar notificaciones push o mensajes en la aplicación.

## Prerrequisitos

1. [Añadir una aplicación en el administrador de Marketo](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (obtener la clave secreta de la aplicación y el ID de Munchkin)
1. [Configure la propiedad en [!DNL Adobe Launch] portal](https://experience.adobe.com/#/@amc/data-collection/home)
1. Configure la clave secreta de la aplicación y el ID de Munchkin para la propiedad en [!DNL Adobe Launch] portal
1. [Configuración de notificaciones push](push-notifications.md) (opcional)

## Cómo instalar la extensión de Marketo en iOS

### Configurar encabezado de puente de Swift

1. Vaya a Archivo > Nuevo > Archivo y seleccione &quot;Archivo de encabezado&quot;.

1. Asigne un nombre al archivo &quot;&lt;_ProjectName_>-Bridging-Header&quot;.

1. Vaya a Proyecto > Target > Configuración de compilación > Compilador Swift > Generación de código. Añada la siguiente ruta al encabezado &quot;Objective-Bridging&quot;:

`$(PODS_ROOT)/<_ProjectName_>-Bridging-Header.h`

## Inicializar extensión

>[!BEGINTABS]

>[!TAB Objetivo C]

Actualice el `applicationDidBecomeActive` método como el siguiente

```
(void)applicationDidBecomeActive:(UIApplication*) application
{
 [[ALMarketo sharedInstance] initializeMarketo:nil];
}
```

>[!TAB Swift]

Actualice el `applicationDidBecomeActive` método como el siguiente

```
func applicationDidBecomeActive(_ application: UIApplication)
{
 ALMarketo.sharedInstance().initializeMarketo(nil)
}
```

>[!ENDTABS]

## Dispositivos de prueba iOS

1. Seleccionar [!UICONTROL Proyecto] > [!UICONTROL Target] > [!UICONTROL Información] > Tipos de URL.
1. Agregar identificador: ${PRODUCT_NAME}
1. Definir esquemas de URL: mkto-&lt;s_ecret key_=&quot;&quot;>
1. Incluir `application:openURL:sourceApplication:annotation:` hasta `AppDelegate.m file` (Objective-C)

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

## Cómo instalar el SDK de Marketo en Android

### Configuración de extensión de Android

Siga las instrucciones de [!DNL Adobe Launch] portal

### Configuración de permisos

Abrir `AndroidManifest.xml` y agregue los siguientes permisos. La aplicación debe solicitar los permisos &quot;INTERNET&quot; y &quot;ACCESS_NETWORK_STATE&quot;. Si la aplicación ya solicita estos permisos, omita este paso.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

## Inicializar extensión

Configuración de ProGuard (opcional)

Si utiliza ProGuard para su aplicación, agregue las siguientes líneas en su `proguard.cfg` archivo. El archivo se encuentra dentro de su `project` carpeta. Añadir este código excluye el SDK de Marketo del proceso de ofuscación.

```
-dontwarn com.marketo.*
-dontnote com.marketo.*
-keep class com.marketo.**{ *; }
```

## Dispositivos de prueba Android

Añadir &quot;MarketoActivity&quot; a `AndroidManifest.xml` dentro de la etiqueta de aplicación.

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

El kit de desarrollo de software (SDK) de MME para Android se ha actualizado a un marco de trabajo más moderno, estable y escalable que contiene más flexibilidad y nuevas funciones de ingeniería para el desarrollador de aplicaciones de Android.

Los desarrolladores de aplicaciones de Android Google ahora pueden utilizar directamente la [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) (FCM) con este SDK.

### Añadir FCM a la aplicación

1. Integre el SDK Android de Marketo más reciente en la aplicación Android.  Los pasos están disponibles en [GitHub](https://github.com/Marketo/android-sdk).
1. Configure la aplicación Firebase en la consola de Firebase.
   1. Crear/agregar un proyecto en [](https://accounts.google.com/ServiceLogin?passive=1209600&amp;osid=1&amp;continue=https://console.firebase.google.com/&amp;followup=https://console.firebase.google.com/)Consola Firebase.
      1. En el [Consola Firebase](https://accounts.google.com/ServiceLogin?passive=1209600&amp;osid=1&amp;continue=https://console.firebase.google.com/&amp;followup=https://console.firebase.google.com/), seleccione [!UICONTROL Agregar proyecto].
      1. Seleccione el proyecto GCM de la lista de proyectos existentes de Google Cloud y seleccione [!UICONTROL Añadir Firebase].
      1. En la pantalla de bienvenida de Firebase, seleccione &quot;Añadir Firebase a su aplicación de Android&quot;.
      1. Proporcione el nombre del paquete y SHA-1, y seleccione [!UICONTROL Agregar aplicación]. Un nuevo `google-services.json` para su aplicación Firebase descargada.
      1. Seleccionar [!UICONTROL Continuar] y siga las instrucciones detalladas para agregar el complemento Google Services en Android Studio.

   1. Vaya a &quot;Configuración del proyecto&quot; en Información general del proyecto
      1. Haga clic en la pestaña General. Descargue la `google-services.json` archivo.
      1. Haga clic en la pestaña &quot;Mensajería en la nube&quot;. Copie &quot;Clave de servidor&quot; e &quot;ID de remitente&quot;. Proporcione estas opciones &quot;Clave de servidor&quot; e &quot;ID de remitente&quot; a Marketo.
   1. Configuración de cambios de FCM en la aplicación de Android
      1. Cambie a la vista Proyecto en Android Studio para ver el directorio raíz del proyecto
         1. Mover el archivo descargado `google-services.json` en el directorio raíz del módulo de la aplicación de Android
         1. En el nivel de proyecto `build.gradle` añada lo siguiente:

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

         1. Finalmente, haga clic en &quot;[!UICONTROL Sincronizar ahora]&quot; en la barra que aparece en el ID
   1. Editar el manifiesto de la aplicación El SDK de FCM agrega automáticamente todos los permisos necesarios y la funcionalidad del receptor requerida. Asegúrese de eliminar los siguientes elementos obsoletos (y potencialmente dañinos, ya que pueden provocar la duplicación de mensajes) del manifiesto de la aplicación:

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

**P: ¿Dónde puedo encontrar las instrucciones para actualizar a la última versión del SDK de MME?** Las instrucciones se encuentran en el sitio para desarrolladores de Marketo [AQUÍ](installation.md).

**P: ¿La actualización a la versión más reciente del SDK requiere que publique una versión actualizada de la aplicación Android para mis usuarios actuales?** No.

**P: ¿Cómo afecta a los clientes de MME existentes que han publicado aplicaciones de Android integradas con el SDK de Android de Marketo?** Pueden migrar una aplicación cliente GCM existente en Android a Firebase Cloud Messaging (FCM) de la siguiente manera:

1. En el [Consola Firebase](https://accounts.google.com/ServiceLogin?passive=1209600&amp;osid=1&amp;continue=https://console.firebase.google.com/&amp;followup=https://console.firebase.google.com/), seleccione [!UICONTROL Agregar proyecto].
1. Seleccione el proyecto GCM de la lista de proyectos existentes de Google Cloud y seleccione **[!UICONTROL Añadir Firebase]**.
1. En la pantalla de bienvenida de Firebase, seleccione **[!UICONTROL Añadir Firebase a la aplicación de Android]**.
1. Proporcione el nombre del paquete y SHA-1, y seleccione **[!UICONTROL Agregar aplicación]**. Un nuevo archivo google-services.json para su
1. Aplicación Firebase descargada.
1. Seleccionar **[!UICONTROL Continuar]** y siga las instrucciones detalladas para agregar el complemento Google Services en Android Studio.

**P: ¿Podemos segmentar los posibles clientes creados con el antiguo SDK de Marketo que utilizaba la aplicación GCM?** Sí. Todos los posibles clientes creados con el SDK de Marketo se pueden segmentar para enviar las notificaciones push.
