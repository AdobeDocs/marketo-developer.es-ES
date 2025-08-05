---
title: PhoneGap
feature: Mobile Marketing
description: Uso de PhoneGap con Marketo en dispositivos móviles
exl-id: 99f14c76-9438-4942-9309-643bca434d07
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '818'
ht-degree: 1%

---

# PhoneGap

Integración del complemento PhoneGap de Marketo

## Requisitos previos

1. [Agregue una aplicación al administrador de Marketo](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (obtenga la clave secreta de su aplicación y el ID de Munchkin).
1. Configurar notificaciones push ([iOS](push-notifications.md) | [Android](push-notifications.md)).
1. [Instalar PhoneGap/Cordova CLI](https://cordova.apache.org/docs/en/latest/guide/cli/).

## Instrucciones de instalación

1. Configurar el complemento PhoneGap de Marketo

   Si la CLI de Cordova está instalada, vaya al directorio de aplicaciones de PhoneGap y ejecute el siguiente comando para agregar el complemento de Marketo a la aplicación:

   `$ cordova plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

1. Instalación del complemento FCM

   `$ cordova plugin add cordova-plugin-fcm`

   Para confirmar que el complemento se ha agregado a la aplicación, ejecute el siguiente comando y verifique

   `$ cordova plugin ls com.marketo.plugin 0.X.0 "MarketoPlugin" cordova-plugin-fcm 2.1.2 "FCMPlugin"`

**Migrar a una versión más reciente (opcional)**

Para eliminar un complemento existente, ejecute el siguiente comando:

`$ cordova plugin remove com.marketo.plugin`

Para volver a añadir el complemento, ejecute el siguiente comando:

`$ cordova plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

**Cordova versión 8.0.0 (Cordova@Android7.0.0) y superior**

Una vez creada la plataforma Cordova Android, abra la aplicación con Android Studio y actualice el valor `dirs` del archivo `Marketo.gradle` que se encuentra en la carpeta `com.marketo.plugin`.

```
repositories{
  jcenter()
  flatDir{
      dirs '../app/src/main/aar'
   }
}
```

Agregar las plataformas de destino para la aplicación `$cordova platform add android` `$ cordova platform add ios`

Comprobar la lista de plataformas agregadas `$cordova platform ls`

1. Compatibilidad con Firebase Cloud Messaging

1. Configure la aplicación Firebase en la consola de Firebase.
   1. Crear/agregar un proyecto en [&#128279;](https://console.firebase.google.com/)consola Firebase.
      1. En la [consola de Firebase](https://console.firebase.google.com/), seleccione **[!UICONTROL Agregar proyecto]**.
      1. Seleccione el proyecto GCM de la lista de proyectos existentes de Google Cloud y seleccione **[!UICONTROL Agregar Firebase]**.
      1. En la pantalla de bienvenida de Firebase, seleccione &quot;Añadir Firebase a su aplicación de Android&quot;.
      1. Proporcione su nombre de paquete y SHA-1, y seleccione **[!UICONTROL Agregar aplicación]**. Se ha descargado un nuevo archivo de `google-services.json` para la aplicación Firebase.
   1. Vaya a **[!UICONTROL Configuración del proyecto]** en [!UICONTROL Información general del proyecto]
      1. Haga clic en la ficha **[!UICONTROL General]**. Descargue el archivo &quot;google-services.json&quot;.
      1. Haga clic en la ficha **[!UICONTROL Mensajería de nube]**. Copiar [!UICONTROL clave de servidor] y [!UICONTROL identificador de remitente]. Proporcione estas [!UICONTROL clave de servidor] y [!UICONTROL ID de remitente] a Marketo.
   1. Configuración de cambios de FCM en la aplicación Phonegap
      1. Mueva el archivo &quot;google-services.json&quot; descargado al directorio raíz del módulo de la aplicación de PhoneGap
      1. Quite el archivo &#39;MyFirebaseInstanceIDService&#39; de la ubicación `platforms/android/app/src/main/java/com/gae/scaffolder/plugin` (obsoleto)
      1. Modifique el archivo &#39;MyFirebaseMessagingService&#39; en la ubicación `platforms/android/app/src/main/java/com/gae/scaffolder/plugin` de la siguiente manera:

         ```
         import com.marketo.Marketo;
         
         public class MyFirebaseMessagingService extends FirebaseMessagingService{
         
         @Override
         public void onNewToken(String s){
           super.onNewToken(s);
           MarketoExtension.setPushNotificaitonTokens(s);
           //Add your code here
         }
         
         @Override
         public void onMessageReceived(RemoteMessage remoteMessage) {
           MarketoExtension.showPushNotificaiton(remoteMessage);
           //Add your code here
         }
         }
         ```

         1. Modifique el archivo &quot;fcm_config_files_process.js&quot; en plugins de ubicación/cordova-plugin-fcm/scripts de la siguiente manera

            ```
            //change
            var strings = fs.readFileSync("platforms/android/res/values/strings.xml").toString();
            //to
            var strings = fs.readFileSync("platforms/android/app/src/main/res/values/strings.xml").toString();
            
            //AND change
            fs.writeFileSync("platforms/android/res/values/strings.xml", strings);
            //to
            fs.writeFileSync("platforms/android/app/src/main/res/values/strings.xml", strings);
            ```

### &#x200B;3. Activar notificaciones push en xCode

Active la capacidad de notificación push en el proyecto de xCode.

### &#x200B;4. Rastrear notificaciones push

Pegue el siguiente código dentro de la función `application:didFinishLaunchingWithOptions:`.

>[!BEGINTABS]

>[!TAB Objetivo C]

Actualice el método `applicationDidBecomeActive` como se muestra a continuación

```
Marketo *sharedInstance = [Marketo sharedInstance];

[sharedInstance trackPushNotification:launchOptions];
```

>[!TAB Swift]

Actualice el método `applicationDidBecomeActive` como se muestra a continuación

```
let sharedInstance: Marketo = Marketo.sharedInstance()

sharedInstance.trackPushNotification(launchOptions)
```

>[!ENDTABS]

### &#x200B;5. Inicializar Marketo Framework

Para asegurarse de que el marco de trabajo de Marketo se inicia al iniciar la aplicación, agregue el siguiente código en la función `onDeviceReady` de su archivo JavaScript principal.

Tenga en cuenta que debemos pasar `phonegap` como tipo de marco de trabajo para aplicaciones PhoneGap.

### Sintaxis

```
// This method will Initialize the Marketo Framework using Your MunchkinId and Secret Key
marketo.initialize(
  function() { console.log("MarketoSDK Init done."); },
  function(error) { console.log("an error occurred:" + error); },
  'YOUR_MUNCHKIN_ID',
  'YOUR_SECRET_KEY',
  'FRAMEWORK_TYPE'
);

// For session tracking, add following.
marketo.onStart(
  function(){ console.log("onStart."); },
  function(error){ console.log("Failed to report onStart." + error); }
);
```

### Parámetros

- Llamada de retorno de éxito : función que se ejecuta si el marco de trabajo de Marketo se inicializa correctamente.
- Callback Failure : función que se ejecuta si el marco de trabajo de Marketo no se puede inicializar.
- MUNCHKIN ID : Munchkin ID recibido desde Marketo en el momento del registro.
- CLAVE SECRETA: Clave secreta recibida de Marketo en el momento del registro.

### &#x200B;6. Inicializar la notificación push de Marketo

Para asegurarse de que se inicia la notificación push de Marketo, agregue el siguiente código después de la función initialize en el archivo JavaScript principal.

### Sintaxis

```
// This function will Enable user notifications (prompts the user to accept push notifications in iOS)
marketo.initializeMarketoPush(
    function() { console.log("Marketo push successfully initialized."); },
    function(error) { console.log("an error occurred:" + error); },
    'YOUR_GCM_PROJECT_ID' // This is required for Android and will be ignored in iOS
);
```

### Parámetros

- Llamada de retorno de éxito : función que se ejecuta si la notificación push de Marketo se inicializa correctamente.
- Función de devolución de llamada de error : que se ejecuta si la notificación push de Marketo no se inicializa.
- GCM_PROJECT_ID : Se encontró el ID del proyecto GCM en [Google Developers Console](https://console.developers.google.com/) después de crear la aplicación.

También se puede anular el registro del token al cerrar la sesión.

```
marketo. uninitializeMarketoPush(
  function() { console.log("Marketo push successfully uninitialized."); } ,
  function(error) { console.log("an error occurred:" + error); }
);
```

## Asociar posible cliente

Puede crear un posible cliente de Marketo llamando a la función associatedLead.

### Sintaxis

```
marketo.associateLead(
  function(){ console.log("MarketoSDK : Lead Added"); },
  function(error){ console.log("an error occurred:" + error); },
  'Lead_Data_JSON_String'
);
```

### Parámetros

- Llamada de retorno de éxito: función que se ejecuta si el marco de trabajo de Marketo asocia correctamente al posible cliente.
- Función de devolución de llamada de error : que se ejecuta si el marco de trabajo de Marketo no puede asociar el posible cliente.
- Datos de posibles clientes: datos de posibles clientes en formato de cadena JSON.

### Ejemplo

```
// First create a lead as shown below
var lead = {};
lead[marketo.KEY_FIRST_NAME] = "Phone";
lead[marketo.KEY_LAST_NAME] = "Gap";
lead[marketo.KEY_EMAIL] = email;
lead[marketo.KEY_ADDRESS] = "demo address";
lead[marketo.KEY_CITY] = "city";
lead[marketo.KEY_STATE] = "state";
lead[marketo.KEY_COUNTRY] = "country";
lead[marketo.KEY_POSTAL_CODE] = "postalCode";
lead[marketo.KEY_GENDER] = "gender";
// To use lead custom field, use the REST API NAME as key
lead["REST API NAME"] = "value";

// Use associateLead function to associate it.
marketo.associateLead(
  function() { console.log("MarketoSDK : Lead Associated"); },
  function(error) { console.log("an error occurred:" + error); },
  JSON.stringify(lead)
);
```

## Acción de informe

Puede informar de cualquier acción realizada por un usuario llamando a la función `reportaction`.

### Sintaxis

```
marketo.reportaction(
  function(){ console.log("MarketoSDK : New event sent "); },
  function(error){ console.log("an error occurred:" + error); },
  'Action_Name',
  'Action_Data_JSON_String'
);
```

### Parámetros

- Llamada de retorno de éxito : función que se ejecuta si el marco de trabajo de Marketo informa de la acción correctamente.
- Callback Failure : función que se ejecuta si el marco de trabajo de Marketo no informa de la acción.
- Nombre de la acción: nombre de la acción.
- Datos de acción: datos de acción en formato de cadena JSON.

### Ejemplo

```
// First create an event as below
var event = {
    "Action Type":"Add To Cart",
    "Action Details":"Adding Product in cart",
    "Action Metric":"10",
    "Action Length":"1"
}

marketo.reportaction(
    function(){ console.log("Reported action successfully."); },
    function(error){ console.log("Failed to report action." + error); },
    "Add To Cart",
    JSON.stringify(event)
);
```

## Informes de sesión

Enlace los tipos de evento de &quot;pausa&quot; y &quot;reanudación&quot; como se muestra a continuación a los eventos de inicio y parada del informe.  Se utiliza para rastrear el tiempo empleado en la aplicación móvil. Nota: Esto es obligatorio en Android.

```
//Add the following code in your www/js/index.js

bindEvents: function() {
   document.addEventListener('pause', this.onStop, false);
   document.addEventListener('resume', this.onStart, false);
},
onStop: function() {
   marketo.onStop(
       function(){ console.log("onStop"); },
       function(error){ console.log("Failed to report onStop." + error); }
   );
},
onStart: function() {
   marketo.onStart(
       function(){ console.log("onStart."); },
       function(error){console.log( "Failed to report onStart." + error); }
   );
},
```

## Creación de posibles clientes

Existen tres formas de crear posibles clientes a partir de una aplicación híbrida:

1. MARKETO MME SDK
1. API DE REST DE MARKETO
1. Envío de formulario

Según el método utilizado, distintos déclencheur y filtros reconocerán un posible cliente recién creado. Los posibles clientes creados con la API de MME SDK o REST aparecen en los déclencheur y filtros &quot;Posible cliente creado&quot;. Los posibles clientes creados por los envíos de formularios aparecen en los déclencheur y filtros &quot;Rellena el formulario&quot;.

La práctica recomendada es mantener la coherencia con el método utilizado por la aplicación web al crear posibles clientes. Si ya tiene una aplicación web que utiliza el envío de formularios como mecanismo para crear posibles clientes, utilice el mismo mecanismo al crear posibles clientes en la aplicación híbrida. Si ya tiene una aplicación web que utiliza nuestra API de REST como mecanismo para crear posibles clientes, utilice el mismo mecanismo al crear posibles clientes en la aplicación híbrida. En los casos en los que no utilice ni el envío de formularios ni la API de REST como mecanismo para crear posibles clientes en la aplicación web, puede considerar la posibilidad de utilizar MME SDK para crear posibles clientes en Marketo.
