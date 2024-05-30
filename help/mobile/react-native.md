---
title: "React Native"
feature: "Mobile Marketing"
description: "Instalación de React Native para Marketo"
source-git-commit: 416044a6cce4dac229640058a9cb0013070c9d9c
workflow-type: tm+mt
source-wordcount: '822'
ht-degree: 0%

---


# Reaccionar nativo

Este artículo proporciona información sobre cómo instalar y configurar el SDK nativo de Marketo para integrar su aplicación móvil con nuestra plataforma.

## Prerrequisitos

[Añadir una aplicación en el administrador de Marketo](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (obtener la clave secreta de la aplicación y el ID de Munchkin).

## Integración de SDK

### Integración del SDK para Android

**Configurar con Gradle**

Agregue la dependencia del SDK de Marketo con la última versión: En el nivel de aplicación `build.gradle` , en la sección de dependencias, agregue (incluida la versión adecuada del SDK de Marketo).

```
implementation 'com.marketo:MarketoSDK:0.x.x'
```

**Añadir repositorio de mavencentral**

El SDK de Marketo está disponible en [repositorio central de maven](https://mvnrepository.com/). Para sincronizar esos archivos, agregue `mavencentral` repositorio a raíz `build.gradle`

```
build script {
  repositories {
    google()
    mavencentral()
  }
}
```

A continuación, sincronice el proyecto con los archivos de Gradle.

#### Integración del SDK de iOS

Antes de crear un puente para el proyecto React Native, es importante configurar el SDK en el proyecto Xcode.

**Integración del SDK: uso de CocoaPods**

Usar el SDK de iOS en la aplicación es fácil. Realice los siguientes pasos para configurarlo en el proyecto Xcode de su aplicación mediante CocoaPods, de modo que pueda integrar su plataforma con la aplicación.

Descargar [CocoaPods](https://cocoapods.org/) : Distribuido como una joya de Ruby, es un administrador de dependencias para Objective-C y Swift que simplifica el proceso de uso de bibliotecas de terceros en su código, como el SDK de iOS.

Para descargarlo e instalarlo, inicie un terminal de línea de comandos en su Mac y ejecute el siguiente comando en él:

1. Instale CocoaPods.

`$ sudo gem install cocoapods`

1. Abra el Podfile. (Dentro de la carpeta iOS del proyecto ReactNative)

`$ open -a Xcode Podfile`

1. Añada la siguiente línea a su Podfile.

`$ pod 'Marketo-iOS-SDK'`

1. Guarde y cierre el Podfile.

1. Descargue e instale el SDK de Marketo iOS.

`$ pod install`

1. Abra el espacio de trabajo en Xcode.

`$ open App.xcworkspace`

## Instrucciones de instalación del módulo nativo

A veces, una aplicación nativa de React necesita acceder a una API de plataforma nativa que no está disponible de forma predeterminada en JavaScript, por ejemplo, las API nativas para acceder a Apple o Google Pay. Tal vez quiera reutilizar algunas bibliotecas Objective-C, Swift, Java o C++ existentes sin tener que volver a implementarlas en JavaScript o escribir código de alto rendimiento con subprocesos múltiples para tareas como el procesamiento de imágenes.

El sistema NativeModule expone instancias de clases Java/Objective-C/C++ (nativas) a JavaScript (JS) como objetos JS, lo que permite ejecutar código nativo arbitrario desde JS. Aunque no esperamos que esta característica forme parte del proceso de desarrollo habitual, es esencial que exista. Si React Native no exporta una API nativa que su aplicación JS necesita, debe poder exportarla usted mismo.

El puente nativo de React se utiliza para la comunicación entre el JSX y las capas de aplicación nativas. En nuestro caso, la aplicación host podrá escribir el código JSX que puede invocar los métodos del SDK para Marketo.

### Android

Este archivo contiene los métodos envolventes que pueden llamar internamente a los métodos del SDK para Marketo con los parámetros que proporcione.

```
public class RNMarketoModule extends ReactContextBaseJavaModule {

   final Marketo marketoSdk;
   RNMarketoModule(ReactApplicationContext context) {
       super(context);
       marketoSdk = Marketo.getInstance(context);
   }

   @NonNull
   @Override
   public String getName() {
       return "RNMarketoModule";
   }

   @ReactMethod
   public void associateLead(ReadableMap leadData) {

       MarketoLead mLead = new MarketoLead();
       try {
           mLead.setCity(leadData.getString(MarketoLead.KEY_CITY));
           mLead.setFirstName(leadData.getString(MarketoLead.KEY_FIRST_NAME));
           mLead.setLastName(leadData.getString(MarketoLead.KEY_LAST_NAME));
           mLead.setAddress(leadData.getString(MarketoLead.KEY_ADDRESS));
           mLead.setEmail(leadData.getString(MarketoLead.KEY_EMAIL));
           mLead.setBirthDay(leadData.getString(MarketoLead.KEY_BIRTHDAY));
           mLead.setCountry(leadData.getString(MarketoLead.KEY_COUNTRY));
           mLead.setFacebookId(leadData.getString(MarketoLead.KEY_FACEBOOK));
           mLead.setGender(leadData.getString(MarketoLead.KEY_GENDER));
           mLead.setState(leadData.getString(MarketoLead.KEY_STATE));
           mLead.setPostalCode(leadData.getString(MarketoLead.KEY_POSTAL_CODE));
           mLead.setTwitterId(leadData.getString(MarketoLead.KEY_TWITTER));
           marketoSdk.associateLead(mLead);
       }
       catch (MktoException e){
       }
   }
   @ReactMethod
   public void setSecureSignature(ReadableMap readableMap) {
       MarketoConfig.SecureMode secureMode = new MarketoConfig.SecureMode();
       secureMode.setAccessKey(readableMap.getString("accessKey"));
       secureMode.setEmail(readableMap.getString("email"));
       secureMode.setSignature(readableMap.getString("signature"));
       secureMode.setTimestamp(readableMap.getInt("timeStamp"));
       marketoSdk.setSecureSignature(secureMode);
   }
   @ReactMethod
   public void initializeSDK(String munchkinId, String appSecreteKey){
       marketoSdk.initializeSDK(munchkinId,appSecreteKey);
   }

   @ReactMethod
   public void initializeMarketoPush(String projectId){
       marketoSdk.initializeMarketoPush( projectId);
   }

   @ReactMethod
   public void initializeMarketoPush(String projectId, String channelName){
       marketoSdk.initializeMarketoPush( projectId, channelName);
   }

   @ReactMethod
   public void uninitializeMarketoPush(){
       marketoSdk.uninitializeMarketoPush();
   }

   @ReactMethod
   public void reportAction(String action){
       Marketo.reportAction(action, null);
   }

   @ReactMethod
   public void reportAction(String action, ReadableMap readableMap){
       MarketoActionMetaData marketoActionMetaData = new MarketoActionMetaData();
       marketoActionMetaData.setActionDetails(readableMap.getString("setMetric"));
       marketoActionMetaData.setActionMetric(readableMap.getString("setLength"));
       marketoActionMetaData.setActionLength(readableMap.getString("actionDetails"));
       marketoActionMetaData.setActionType(readableMap.getString("actionType"));
       Marketo.reportAction(action, marketoActionMetaData);
   }
}
```

**Registrar el paquete**

Informe a react-native sobre el paquete de Marketo.

```
public class MarketoPluginPackage implements ReactPackage {

   @NonNull
   @Override
   public List createNativeModules(@NonNull ReactApplicationContext reactContext) {
           List modules = new ArrayList<>();

           modules.add(new RNMarketoModule(reactContext));

           return modules;    
   }

   @NonNull
   @Override
   public List createViewManagers(@NonNull ReactApplicationContext reactContext) {
       return Collections.emptyList();
   }
}
```

Para completar el registro del paquete, agregue MarketoPluginPackage a la lista de paquetes de React de la clase de aplicación:

```
public class MainApplication extends Application implements ReactApplication {
 
  private final ReactNativeHost mReactNativeHost =
      new ReactNativeHost(this) {
        @Override
        public boolean getUseDeveloperSupport() {
          return BuildConfig.DEBUG;
        }
 
        @Override
        protected List getPackages() {
          @SuppressWarnings("UnnecessaryLocalVariable")
          List packages = new PackageList(this).getPackages();
          packages.add(new MarketoPluginPackage());     //Add the Marketo Package here.
          // Packages that cannot be autolinked yet can be added manually here, for example:
          return packages;
        }
}
```

### iOS

En la siguiente guía creará un módulo nativo, _RNMarketoModule_, que le permiten acceder a las API de Marketo desde JavaScript.

Para empezar, abra el proyecto de iOS dentro de la aplicación nativa de React en Xcode. Puede encontrar su proyecto de iOS aquí dentro de una aplicación nativa de React. Se recomienda utilizar Xcode para escribir el código nativo. Xcode está diseñado para el desarrollo de iOS y su uso le ayudará a resolver rápidamente errores más pequeños, como la sintaxis de código.

Cree el encabezado principal del módulo nativo y los archivos de implementación. Cree un nuevo archivo llamado `MktoBridge.h` y agréguele lo siguiente:

```
//
//  MktoBridge.h
//
//  Created by Marketo, An Adobe company.
//

#import <Foundation/Foundation.h>
#import <React/RCTBridgeModule.h>

NS_ASSUME_NONNULL_BEGIN

@interface MktoBridge : NSObject 

@end

NS_ASSUME_NONNULL_END
```

Cree el archivo de implementación correspondiente, MktoBridge.m, en la misma carpeta e incluya el siguiente contenido:

```
//
//  MktoBridge.m
//
//  Created by Marketo, An Adobe company.
//

#import "MktoBridge.h"
#import <MarketoFramework/MarketoFramework.h>
#import <React/RCTBridge.h>
#import "ConstantStringsHeader.h"

@implementation MktoBridge

RCT_EXPORT_MODULE(RNMarketoModule);

+(BOOL)requiresMainQueueSetup{
  return NO;
}

RCT_EXPORT_METHOD(initializeWithMunchkin:(NSString *) munchkinId Secret: (NSString *) secretKey andFrameworkType : (NSString *) frameworkType{
  [[Marketo sharedInstance] initializeWithMunchkinID:munchkinId appSecret:secretKey  mobileFrameworkType:frameworktype launchOptions:nil];
}

RCT_EXPORT_METHOD(reportAction:(NSString *)actionName withMetaData:(NSDictionary *)metaData){
  MarketoActionMetaData *meta = [[MarketoActionMetaData alloc] init];
  [meta setType:[metaData objectForKey:KEY_ACTION_TYPE]];
  [meta setDetails:[metaData objectForKey:KEY_ACTION_DETAILS]];
  [meta setLength:[metaData valueForKey:KEY_ACTION_LENGTH]];
  [meta setMetric:[metaData valueForKey:KEY_ACTION_METRIC]];
  [[Marketo sharedInstance] reportAction:actionName withMetaData:meta];
}

RCT_EXPORT_METHOD(associateLead:(NSDictionary *)leadDetails){
  MarketoLead *lead = [[MarketoLead alloc] init];
  if ([leadDetails objectForKey:KEY_EMAIL] != nil) {
    [lead setEmail:[leadDetails objectForKey:KEY_EMAIL]];
  }
  if ([leadDetails objectForKey:KEY_FIRST_NAME] != nil) {
    [lead setFirstName:[leadDetails objectForKey:KEY_FIRST_NAME]];
  }
  
  if ([leadDetails objectForKey:KEY_LAST_NAME] != nil) {
    [lead setLastName:[leadDetails objectForKey:KEY_LAST_NAME]];
  }
  
  if ([leadDetails objectForKey:KEY_CITY] != nil) {
    [lead setCity:[leadDetails objectForKey:KEY_CITY]];
  }
    [[Marketo sharedInstance] associateLead:lead];
}

RCT_EXPORT_METHOD(uninitializeMarketoPush){
  [[Marketo sharedInstance] unregisterPushDeviceToken];
}

RCT_EXPORT_METHOD(reportAll){
  [[Marketo sharedInstance] reportAll];
}

RCT_EXPORT_METHOD(setSecureSignature:(NSDictionary *)secureSignature){
  MKTSecuritySignature *secSignature = [[MKTSecuritySignature alloc]
                                        initWithAccessKey:[secureSignature objectForKey:KEY_ACCESSKEY]
                                        signature:[secureSignature objectForKey:KEY_SIGNATURE]
                                        timestamp: [secureSignature objectForKey:KEY_EMAIL]
                                        email:[secureSignature objectForKey:KEY_EMAIL]];
  
    [[Marketo sharedInstance] setSecureSignature:secSignature];
}
@end
```

#### Inicializar el SDK de Marketo

Busque en su aplicación un lugar en el que desee agregar una llamada al método createCalendarEvent() del módulo nativo. A continuación, se muestra un ejemplo de un componente, NewModuleButton, que puede añadir en la aplicación. Puede invocar el módulo nativo dentro de la función onPress() de NewModuleButton.

```
import React from 'react';
import { NativeModules, Button } from 'react-native';

const NewModuleButton = () => {
  const onPress = () => {
    console.log('We will invoke the native module here!');
  };

  return (
    
  );
  };

export default NewModuleButton;
```

Este archivo JavaScript carga el módulo nativo en la capa JavaScript.

```javascript
import React from 'react';
import {Node} from 'react';
import { NativeModules } from 'react-native';

const { RNMarketoModule } = NativeModules;
```

Una vez que los archivos anteriores se colocan correctamente, podemos importar el módulo js en cualquier clase js y llamar a sus métodos directamente. Por ejemplo:

Tenga en cuenta que debemos pasar &quot;reactNative&quot; como tipo de marco de trabajo para las aplicaciones nativas de React. 

```
// Initialize marketo SDK with Munchkin & Seretkey you have from step 1.
RNMarketoModule.initializeSDK("MunchkinID","SecreteKEY","FrameworkType")

//You can create a Marketo Lead by calling the associateLead function.
RNMarketoModule.associateLead({ email: "", firstName: "", lastName:"", city:""})

//You can report any user performed action by calling the reportaction function.
RNMarketoModule.reportAction("Bought Shirt", {actionType:"Shopping", actionDetails: "Red Shirt", setLength : 20, setMetric : 30 })

//You can set Secure Signature by calling this method.
RNMarketoModule.setSecureSignature({accessKey: "Key102", email: "testleadrk@001.com", signature : "asdfghjkloiuytds", timeStamp: "12345678987654"})

//This function will Enable user notifications (Only Android)
RNMarketoModule.initializeMarketoPush("350312872033", "MKTO")

//The token can also be unregistered on logout.
RNMarketoModule.uninitializeMarketoPush()
```

#### Configuración de notificaciones push


Inicializar push con ID de proyecto y nombre de canal

```
RNMarketoModule.initializeMarketoPush("ProjectId", "Channel_name")
```

Añadir el siguiente servicio a `AndroidManifest.xml`


```xml
<service android:exported="true" android:name=".MyFirebaseMessagingService" android:stopWithTask="true">
    <intent-filter>
        <action  android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
    </intent-filter/>
    <intent-filter> 
        <action android:name="com.google.firebase.MESSAGING_EVENT"/> 
    </intent-filter/>
</activity/>
```

Cree una clase con el nombre `FirebaseMessagingService.java` y Agregue el siguiente código

```java
import com.google.firebase.messaging.FirebaseMessagingService;
import com.google.firebase.messaging.RemoteMessage;
import com.marketo.Marketo;

public class MyFirebaseMessagingService extends FirebaseMessagingService {

   @Override
   public void onNewToken(String token) {
       super.onNewToken(token);
       Marketo marketoSdk = Marketo.getInstance(this.getApplicationContext());
       marketoSdk.setPushNotificationToken(token);
   }

   @Override
   public void onMessageReceived(RemoteMessage remoteMessage) {
       Marketo marketoSdk = Marketo.getInstance(this.getApplicationContext());
       marketoSdk.showPushNotification(remoteMessage);
   }
}
```

Los permisos deben habilitarse en el proyecto Xcode para enviar notificaciones push al dispositivo del usuario.

Para enviar notificaciones push, [añadir notificaciones push](push-notifications.md).

Ahora en su `AppDelegate.m` archivo en XCode, importar Marketo

```
#import <MarketoFramework/MarketoFramework.h> 
```

Añadir `UNUserNotificationCenterDelegate` a la interfaz AppDelegate de la siguiente manera para controlar los delegados

```
@interface AppDelegate () <UNUserNotificationCenterDelegate>

@end
```

Registro para notificaciones remotas en `didFinishLaunchingWithOptions` método.

```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
#ifdef FB_SONARKIT_ENABLED
  InitializeFlipper(application);
#endif

  RCTBridge *bridge = [[RCTBridge alloc] initWithDelegate:self launchOptions:launchOptions];
  RCTRootView *rootView = [[RCTRootView alloc] initWithBridge:bridge
                                                   moduleName:@"HelloRN"
                                            initialProperties:nil];  
  
// asking user permission to send push notifications 
  [self registerForRemoteNotifications];
  
  self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
  UIViewController *rootViewController = [UIViewController new];
  rootViewController.view = rootView;
  self.window.rootViewController = rootViewController;
  [self.window makeKeyAndVisible];

  return YES;
}

- (void)registerForRemoteNotifications {
   UNUserNotificationCenter *center = [UNUserNotificationCenter currentNotificationCenter];
        center.delegate = self;
        [center requestAuthorizationWithOptions:(UNAuthorizationOptionSound | UNAuthorizationOptionAlert | UNAuthorizationOptionBadge) completionHandler:^(BOOL granted, NSError * _Nullable error){
            if(!error){
                dispatch_async(dispatch_get_main_queue(), ^{
                    [[UIApplication sharedApplication] registerForRemoteNotifications];
                });
            }
            else{
                NSLog(@"failed");
            }
        }];
}
```

Incluir lo siguiente `UNUserNotificationCenter` delegar notificaciones necesarias en métodos delegados.

```
-(void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler{
    completionHandler(UNAuthorizationOptionSound | UNAuthorizationOptionAlert | UNAuthorizationOptionBadge);
}

- (void)userNotificationCenter:(UNUserNotificationCenter *)center
didReceiveNotificationResponse:(UNNotificationResponse *)response
         withCompletionHandler:(void(^)(void))completionHandler {
    [[Marketo sharedInstance] userNotificationCenter:center didReceiveNotificationResponse:response withCompletionHandler:completionHandler];
}

- (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {
    // Register the push token with Marketo
    [[Marketo sharedInstance] registerPushDeviceToken:deviceToken];
}

- (void)applicationWillTerminate:(UIApplication *)application {
    [[Marketo sharedInstance] unregisterPushDeviceToken];
}

-(void)application:(UIApplication *)application didFailToRegisterForRemoteNotificationsWithError:(NSError *)error{
    NSLog(@"didFailToRegisterForRemoteNotificationsWithError");
}
```

### Agregar dispositivos de prueba

**Android**

Añadir &quot;MarketoActivity&quot; a `AndroidManifest.xml` archivo dentro de etiqueta de aplicación.

```xml
<activity android:name="com.marketo.MarketoActivity" android:configChanges="orientation|screenSize" android:exported="true">
    <intent-filter android:label="MarketoActivity">
        <action  android:name="android.intent.action.VIEW"/>
        <category  android:name="android.intent.category.DEFAULT"/>
        <category  android:name="android.intent.category.BROWSABLE"/>
        <data android:host="add_test_device" android:scheme="mkto"/>
    </intent-filter/>
</activity/>
```

**iOS**

1. Seleccione Proyecto > Target > Información > Tipos de URL.

1. Agregar identificador: ${PRODUCT_NAME}

1. Definir esquemas de URL: `mkto-<S_ecret Key_>`

1. Incluir `application:openURL:sourceApplication:annotation:` hasta `AppDelegate.m` archivo (Objective-C)

**iOS: Administrar tipo de URL personalizada/vínculos profundos en AppDelegate** 

```
- (BOOL)application:(UIApplication *)app
            openURL:(NSURL *)url
            options:(NSDictionary *)options{
   
    return [[Marketo sharedInstance] application:app
                                         openURL:url
                                         options:options];    
}
```

Estas constantes se utilizan al llamar a la API desde JavaScript. Debe crear archivos constantes y agregar lo siguiente.

```
// Lead attributes.
static NSString *const KEY_FIRST_NAME = @"firstName";
static NSString *const KEY_LAST_NAME = @"lastName";
static NSString *const KEY_ADDRESS = @"address";
static NSString *const KEY_CITY = @"city";
static NSString *const KEY_STATE = @"state";
static NSString *const KEY_COUNTRY = @"country";
static NSString *const KEY_GENDER = @"gender";
static NSString *const KEY_EMAIL = @"email";
static NSString *const KEY_TWITTER = @"twitterId";
static NSString *const KEY_FACEBOOK = @"facebookId";
static NSString *const KEY_LINKEDIN = @"linkedinId";
static NSString *const KEY_LEAD_SOURCE = @"leadSource";
static NSString *const KEY_BIRTHDAY = @"dateOfBirth";
static NSString *const KEY_FACEBOOK_PROFILE_URL = @"facebookProfileURL";
static NSString *const KEY_FACEBOOK_PROFILE_PIC = @"facebookPhotoURL";

// Custom actions
static NSString *const KEY_ACTION_TYPE = @"actionType";
static NSString *const KEY_ACTION_DETAILS = @"actionDetails";
static NSString *const KEY_ACTION_LENGTH = @"setLength";
static NSString *const KEY_ACTION_METRIC = @"setMetric";

//Secure Signature
static NSString *const KEY_ACCESSKEY = @"accessKey";
static NSString *const KEY_SIGNATURE = @"signature";
static NSString *const KEY_TIMESTAMP = @"timeStamp";
```

Ejemplo de uso

```
//You can create a Marketo Lead by calling the associateLead function.
RNMarketoModule.associateLead({ email: "", firstName: "", lastName:"", city:""})
```
