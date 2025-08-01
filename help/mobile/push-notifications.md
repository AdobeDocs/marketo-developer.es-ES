---
title: Insertar notificaciones
feature: Mobile Marketing
description: Activación de las notificaciones push para Marketo Mobile
exl-id: 41d657d8-9eea-4314-ab24-fd4cb2be7f61
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '1329'
ht-degree: 0%

---

# Insertar notificaciones

Cómo activar las notificaciones push.

## Configuración de notificaciones push en iOS

Para activar las notificaciones push hay que seguir tres pasos:

1. Configure las notificaciones push en la cuenta de desarrollador de Apple.
1. Habilite las notificaciones push en xCode.
1. Habilite las notificaciones push en la aplicación con Marketo SDK.

### Configuración de notificaciones push en la cuenta de desarrollador de Apple

1. Inicie sesión en el [Centro para miembros](http://developer.apple.com/membercenter) de Apple Developer.
1. Haga clic en Certificados, identificadores y perfiles.
1. Haga clic en la carpeta &quot;Certificados->Todos&quot; debajo de &quot;iOS, tvOS, watchOS&quot;.
1. Seleccione el signo + en la pantalla superior izquierda junto a los certificados ![](assets/certificates-plus.png)
1. Active la casilla de verificación &quot;SSL del servicio de notificaciones push de Apple (zona protegida y producción)&quot; y haga clic en &quot;Continuar&quot;.
1. Seleccione el identificador de aplicación que está usando para compilar la aplicación.![](assets/push-appid.png)
1. Cree y cargue CSR para generar el certificado push. ![](assets/push-ssl.png)
1. Descargue el certificado en el equipo local y haga doble clic para instalarlo. ![](assets/certificate-download.png)
1. Abra &quot;Acceso a llaveros&quot;, haga clic con el botón secundario en el certificado y exporte dos elementos al archivo `.p12`.![cadena_clave](assets/key-chain.png)
1. Cargue este archivo a través de Marketo Admin Console para configurar las notificaciones.
1. Actualice los perfiles de aprovisionamiento de aplicaciones.

### Habilitar notificaciones push en xCode

Activar la capacidad de notificación push en el proyecto xCode.![](assets/push-xcode.png)

### Habilitar notificaciones push en la aplicación con Marketo SDK

Agregue el siguiente código al archivo `AppDelegate.m` para enviar notificaciones push a los dispositivos del cliente.

**Nota** - Si usa la extensión [!DNL Adobe Launch], use `ALMarketo` como nombre de clase

Importar lo siguiente en `AppDelegate.h`.

>[!BEGINTABS]

>[!TAB Objetivo C]

```
#import <UserNotifications/UserNotifications.h>
```

>[!TAB Swift]

```
import UserNotifications
```

>[!ENDTABS]

Agregar `UNUserNotificationCenterDelegate` a `AppDelegate` como se muestra a continuación.

>[!BEGINTABS]

>[!TAB Objetivo C]

```
@interface AppDelegate : UIResponder <UIApplicationDelegate, UNUserNotificationCenterDelegate>
```

>[!TAB Swift]

```
class AppDelegate: UIResponder, UIApplicationDelegate , UNUserNotificationCenterDelegate
```

>[!ENDTABS]

Iniciar servicio de notificaciones push. Para habilitar la notificación push, agregue el siguiente código.

>[!BEGINTABS]

>[!TAB Objetivo C]

```objectivec
BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
UNUserNotificationCenter *center = [UNUserNotificationCenter currentNotificationCenter];
        center.delegate = self;
        [center requestAuthorizationWithOptions:(UNAuthorizationOptionSound | UNAuthorizationOptionAlert | UNAuthorizationOptionBadge) completionHandler:^(BOOL granted, NSError * _Nullable error){
            if(!error){
                dispatch_async(dispatch_get_main_queue(), ^{
                    [[UIApplication sharedApplication] registerForRemoteNotifications];
                });
            }
        }];

    return YES;
}
```

>[!TAB Swift]

```
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {

    UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .sound,    .badge]) { granted, error in
            if let error = error {
                print("\(error.localizedDescription)")
            } else {
                DispatchQueue.main.async {
                    application.registerForRemoteNotifications()
                }
            }
        }

        return true
}
```

>[!ENDTABS]

Llame a este método para iniciar el proceso de registro con Apple Push Service. Si el registro se realiza correctamente, la aplicación llama al método `application:didRegisterForRemoteNotificationsWithDeviceToken:` del objeto delegado de la aplicación y le pasa un token de dispositivo.

Si el registro falla, la aplicación llama al método `application:didFailToRegisterForRemoteNotificationsWithError:` de su delegado de la aplicación.

Registre el token push con Marketo. Para recibir notificaciones push de Marketo, debe registrar el token del dispositivo con Marketo.

>[!BEGINTABS]

>[!TAB Objetivo C]

```
- (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {
    // Register the push token with Marketo
    [[Marketo sharedInstance] registerPushDeviceToken:deviceToken];
}
```

>[!TAB Swift]

```
func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
    // Register the push token with Marketo
    Marketo.sharedInstance().registerPushDeviceToken(deviceToken)
}
```

>[!ENDTABS]

También se puede anular el registro del token cuando el usuario cierra la sesión.

>[!BEGINTABS]

>[!TAB Objetivo C]

```
[[Marketo sharedInstance] unregisterPushDeviceToken];
```

>[!TAB Swift]

```
Marketo.sharedInstance().unregisterPushDeviceToken
```

>[!ENDTABS]

Para volver a registrar el token push, extraiga el código del paso 3 en un método AppDelegate y llame desde el método de inicio de sesión ViewController.

Gestionar la notificación push. Para recibir notificaciones push de Marketo, debe registrar el token del dispositivo con Marketo.

>[!BEGINTABS]

>[!TAB Objetivo C]

```
- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo
{
    [[Marketo sharedInstance] handlePushNotification:userInfo];
}
```

>[!TAB Swift]

```
func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable : Any]) {
    Marketo.sharedInstance().handlePushNotification(userInfo)
}
```

>[!ENDTABS]

Agregue el método siguiente en AppDelegate

Con este método puede presentar una alerta, un sonido o aumentar el distintivo mientras la aplicación está en primer plano. En este método, debe llamar al método completionHandler que elija.

>[!BEGINTABS]

>[!TAB Objetivo C]

```
-(void)userNotificationCenter:(UNUserNotificationCenter *)center
    willPresentNotification:(UNNotification *)notification
        withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler{

    completionHandler(UNAuthorizationOptionSound | UNAuthorizationOptionAlert | UNAuthorizationOptionBadge);
}
```

>[!TAB Swift]

```
func userNotificationCenter(_ center: UNUserNotificationCenter,
            willPresent notification: UNNotification, withCompletionHandler completionHandler: @escaping (
    UNNotificationPresentationOptions) -> Void) {
    completionHandler([.alert, .sound,.badge])
}
```

>[!ENDTABS]

Controlar las notificaciones push recién recibidas en AppDelegate

Se llamará al método en el delegado cuando el usuario responda a la notificación abriendo la aplicación, descartando la notificación o eligiendo UNNotificationAction. Se debe establecer el delegado antes de que la aplicación vuelva de applicationDidFinishLaunching:.

>[!BEGINTABS]

>[!TAB Objetivo C]

```
- (void)userNotificationCenter:(UNUserNotificationCenter *)center
didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void(^)(void))completionHandler {
    [[Marketo sharedInstance] userNotificationCenter:center didReceiveNotificationResponse:response withCompletionHandler:completionHandler];
}
```

>[!TAB Swift]

```
func userNotificationCenter(_ center: UNUserNotificationCenter,
                                didReceive response: UNNotificationResponse,
                                withCompletionHandler
                                completionHandler: @escaping () -> Void) {
        Marketo.sharedInstance().userNotificationCenter(center, didReceive: response, withCompletionHandler: completionHandler)
}
```

>[!ENDTABS]

Seguimiento de notificaciones push

Si la aplicación se está ejecutando en segundo plano (o no está activa), el dispositivo recibirá una notificación push como se muestra a continuación. Marketo realizará un seguimiento cuando el usuario toque la notificación.

![móvil8](assets/mobile8.png)

Si el dispositivo recibe una notificación push, se pasará a la llamada de retorno `application:didReceiveRemoteNotification:` en el delegado de la aplicación.

A continuación se muestra un registro de actividad de Marketo desde Marketo que muestra eventos de aplicación y eventos de notificaciones push.

![móvil9](assets/mobile9.png)

## Configuración de notificaciones push en Android

1. Agregue el siguiente permiso dentro de la etiqueta de aplicación.

   Abra `AndroidManifest.xml` y agregue los siguientes permisos. La aplicación debe solicitar los permisos &quot;INTERNET&quot; y &quot;ACCESS_NETWORK_STATE&quot;. Si la aplicación ya solicita estos permisos, omita este paso.

   ```xml
   <uses‐permission android:name="android.permission.INTERNET"/>
   <uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
   
   <!‐‐Following permissions are required for push notification.‐‐>
   <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
   <!‐‐Keeps the processor from sleeping when a message is received.‐‐>
   <uses-permission android:name="android.permission.WAKE_LOCK"/>
   <permission android:name="<PACKAGE_NAME>.permission.C2D_MESSAGE" android:protectionLevel="signature" />
   <uses-permission android:name="<PACKAGE_NAME>.permission.C2D_MESSAGE" />
   <!-- This app has permission to register and receive data message. -->
   <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
   ```

1. Configuración de FCM con HTTPv1 (Google tiene [protocolo XMPP obsoleto](https://firebase.google.com/docs/cloud-messaging/xmpp-server-ref) el 12 de junio de 2023 y se eliminará en junio de 2024) 

- Habilitar MME FCM HTTPv1 en el administrador de características de Marketo ![](assets/feature-manager.png)
   - Cargar archivo JSON de cuenta de servicio para la aplicación en MLM.
   - Puede descargar el archivo Json de la cuenta de servicio desde la consola de Firebase.   ![](assets/fcm-console.png)
   - Espere una hora después de cargar el archivo JSON de la cuenta de servicio en Marketo antes de enviar notificaciones push.  

## Dispositivos de prueba Android

Agregue la actividad Marketo en el archivo de manifiesto dentro de la etiqueta de aplicación.

```xml
<activity android:name="com.marketo.MarketoActivity"  android:configChanges="orientation|screenSize">
    <intent-filter android:label="MarketoActivity">
        <action  android:name="android.intent.action.VIEW"/>
        <category  android:name="android.intent.category.DEFAULT"/>
        <category  android:name="android.intent.category.BROWSABLE"/>
        <data android:host="add_test_device" android:scheme="mkto"/>
    </intent-filter/>
</activity/>
```

## Registrar el servicio push de Marketo

1. Para recibir notificaciones push de Marketo, debes agregar el servicio de mensajería de Firebase a tu `AndroidManifest.xml`. Agregue antes de la etiqueta de cierre de la aplicación.

   ```xml
   <meta-data
       android:name="com.google.android.gms.version"
       android:value="@integer/google_play_services_version" />
   <service android:name=".MyFirebaseMessagingService">
   <intent-filter>
   <action android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
   <action android:name="com.google.firebase.MESSAGING_EVENT"/>
   </intent-filter>
   </service>
   ```

1. Agregue métodos de Marketo SDK en el archivo `MyFirebaseMessagingService` de la siguiente manera

   ```java
   import com.marketo.Marketo;
   
   public class MyFirebaseMessagingService extends FirebaseMessagingService {
   
       @Override
       public void onNewToken(String s) {
           super.onNewToken(s);
           Marketo marketoSdk = Marketo.getInstance(this.getApplicationContext());
           marketoSdk.setPushNotificaitonToken(s);
           // Add your code here...
       }
   
       @Override
       public void onMessageReceived(RemoteMessage remoteMessage) {
           Marketo marketoSdk = Marketo.getInstance(this.getApplicationContext());
           marketoSdk.showPushNotificaiton(remoteMessage);
           // Add your code here...
       }
   
   }
   ```

   **Nota** - Si usa la extensión de Adobe, agregue como se muestra a continuación

   ```java
   import com.marketo.Marketo;
   
   public class MyFirebaseMessagingService extends FirebaseMessagingService {
   
       @Override
       public void onNewToken(String token) {
           super.onNewToken(token);
           ALMarketo.setPushNotificationToken(token);
           // Add your code here...
       }
   
       @Override
       public void onMessageReceived(RemoteMessage remoteMessage) {
           ALMarketo.showPushNotification(remoteMessage);
           // Add your code here...
       }
   
   }
   ```

**NOTA**: FCM SDK agrega automáticamente todos los permisos necesarios, así como la funcionalidad del receptor requerida. Asegúrese de eliminar los siguientes elementos obsoletos (y potencialmente dañinos, ya que pueden provocar la duplicación de mensajes) del manifiesto de la aplicación si ha utilizado versiones anteriores de SDK

```xml
<receiver android:name="com.marketo.MarketoBroadcastReceiver" android:permission="com.google.android.c2dm.permission.SEND">
    <intent-filter>
        <!‐‐Receives the actual messages.‐‐>
        <action android:name="com.google.android.c2dm.intent.RECEIVE"/>
        <!‐‐Register to enable push notification‐‐>
        <action android:name="com.google.android.c2dm.intent.REGISTRATION"/>
        <!‐‐‐Replace YOUR_PACKAGE_NAME with your own package name‐‐>
        <category android:name="YOUR_PACKAGE_NAME"/>
    </intent-filter>
</receiver>

<!‐‐Marketo service to handle push registration and notification‐‐>
<service android:name="com.marketo.MarketoIntentService"/>
```

1. Inicializar Marketo Push Después de guardar la configuración anterior, debe inicializar las notificaciones push de Marketo. Cree o abra la clase Application y copie o pegue el código siguiente. Puede obtener su ID de remitente desde la consola de Firebase.


   ```java
   Marketo marketoSdk = Marketo.getInstance(getApplicationContext());
   
   // Enable push notification here. The push notification channel name can by any string
   marketoSdk.initializeMarketoPush(SENDER_ID,"ChannelName");
   ```

   Si utiliza la extensión [!DNL Adobe Launch], siga estas instrucciones

   ```java
   // Enable push notification here. The push notification channel name can by any string
   ALMarketo.initializeMarketoPush(SENDER_ID,"ChannelName");
   ```

   Si no tiene un SENDER_ID, habilite el servicio de mensajería de Google Cloud siguiendo los pasos detallados en [este tutorial](https://developers.google.com/cloud-messaging/).

   También se puede anular el registro del token cuando el usuario cierra la sesión.

   ```java
   marketoSdk.uninitializeMarketoPush();
   ```

   Si utiliza la extensión [!DNL Adobe Launch], siga las instrucciones siguientes

   ```java
   ALMarketo.uninitializeMarketoPush();
   ```

   Nota: Para volver a registrar el token push, extraiga el código del paso 3 en un método AppDelegate y llame desde el método de inicio de sesión ViewController.

1. Set Notification Icon (opcional) para configurar un icono de notificación personalizado, se debe llamar al siguiente método.

   ```java
   MarketoConfig.Notification config = new MarketoConfig.Notification();
   // Optional bitmap for honeycomb and above
   config.setNotificationLargeIcon(bitmap);
   
   // Required icon Resource ID
   config.setNotificationSmallIcon(R.drawable.notification_small_icon);
   
   // Set the configuration
   //Use the static methods on ALMarketo class when using Adobe Extension
   Marketo.getInstance(context).setNotificationConfig(config);
   
   // Get the configuration set
   Marketo.getInstance(context).getNotificationConfig();
   ```

## Resolución de problemas

La configuración de mensajes push móviles implica muchos pasos y la coordinación de desarrolladores y especialistas en marketing. Si está experimentando dificultades, hay algunas cosas simples que puede comprobar.

Después de asegurarse de que las cosas simples son correctas, puede profundizar en los detalles de programación.

### El mensaje de inserción no aparece

En primer lugar, compruebe si los mensajes push están desactivados en el auricular. Los usuarios de móviles pueden controlar si reciben mensajes para una aplicación en particular o no. A menudo, los desarrolladores (y los especialistas en marketing) desactivan estos mensajes en algún momento durante el desarrollo. Por lo tanto, lo primero que debe comprobar es si el destinatario ha deshabilitado los mensajes push para la aplicación.

En segundo lugar, ¿la aplicación ya está abierta y activa en el dispositivo? Cuando la aplicación es la aplicación activa en el dispositivo, los mensajes push móviles no aparecen en la pantalla. Alternativamente, aparecen en el área de &quot;notificaciones locales&quot; de la aplicación.

### Ver los registros de actividad en Marketo

El primer lugar que debe buscar al rastrear un error es en los registros de actividad de Marketo. Puede utilizar los registros de actividad para comprobar que se ha enviado un mensaje.

En el registro de actividad, observe los registros de actividad de una persona que se suponía que debía recibir un mensaje. Si se envió el mensaje, habrá un registro presente en el registro de actividad. Si no es así, es probable que el problema se deba a la configuración del certificado de iOS o de la clave de API de Android dentro de Marketo.

### El certificado o la clave no son válidos

Compruebe la configuración para asegurarse de que ha cargado el certificado adecuado para Zona protegida o Producción. A veces, es mejor que el desarrollador vuelva a exportar los certificados (iOS) o las claves (Android) y, a continuación, vuelva a cargarlos en Marketo para asegurarse de que son correctos.

### Falta un certificado o una clave en el archivo .p12 (iOS)

Cuando exporte el certificado, asegúrese de exportar la clave _y_ el certificado.

### Aprovisionamiento de perfiles obsoletos (iOS)

Siempre que añada un nuevo dispositivo, debe actualizar los perfiles de aprovisionamiento y generar nuevos certificados. Asegúrese de que el proyecto Xcode señale a los perfiles y certificados correctos e importe esos certificados en Marketo.

### No Se Puede Cargar El Certificado De iOS (IOS)

Asegúrese de que la contraseña utilizada al exportar el certificado no contenga espacios.  Por ejemplo, en lugar de esto:

`Hello World 123`

use esto:

`HelloWorld123`

### Solución de problemas de certificados iOS

Para las aplicaciones de zona protegida, puede utilizar un certificado &quot;desarrollador&quot; o &quot;universal&quot;. Sin embargo, para las aplicaciones de producción debe cargar un certificado válido de &quot;distribución&quot; o &quot;universal&quot;.

### Devolución de push/token no válido

Un token de registro existente puede dejar de ser válido en una serie de situaciones, entre las que se incluyen:

- Si la aplicación cliente cancela el registro con GCM.
- Si la aplicación cliente se cancela de forma automática, lo que puede ocurrir si el usuario desinstala la aplicación. Por ejemplo, en iOS, si el servicio de comentarios de APNS informaba del token de APNS como no válido.
- Si caduca el token de registro. Por ejemplo, Google puede decidir actualizar los tokens de registro o el token de APNS ha caducado para dispositivos iOS.
- Si la aplicación cliente se actualiza, pero la nueva versión no está configurada para recibir mensajes.
