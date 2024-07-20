---
title: '[!DNL Ionic]'
feature: Mobile Marketing
description: Usando  [!DNL Ionic] con Marketo para dispositivos móviles
exl-id: 204e5fb4-c9d6-43a6-9d77-0b2a67ddbed3
source-git-commit: 6fc45ff98998217923e2a5b02d00d1522fe3272c
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 1%

---

# Iónico

En este tema se describe cómo integrar el complemento Marketo Cordova. Actualmente no se admite el condensador [!DNL Ionic].

## Prerrequisitos

1. [Agregue una aplicación al administrador de Marketo](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (obtenga la clave secreta de su aplicación y el identificador de Munchkin).
1. Configurar notificaciones push ([iOS](push-notifications.md) | [Android](push-notifications.md) ).
1. Instalar [[!DNL Ionic]](https://ionicframework.com/getting-started/) y [Cordova CLI](https://cordova.apache.org/docs/en/latest/guide/cli/).

## Instrucciones de instalación

### Configurar el complemento de Marketo [!DNL Ionic]

1. Si la CLI de Cordova está instalada, vaya al directorio de aplicaciones [!DNL Ionic] y ejecute el siguiente comando para agregar el complemento de Marketo a la aplicación:

   `$ ionic plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

1. Para confirmar que el complemento se ha agregado a la aplicación, ejecute el siguiente comando:

   `$ ionic plugin list com.marketo.plugin 0.X.0 "MarketoPlugin"`

### Migrar a una versión más reciente (opcional)

1. Para eliminar un complemento existente, ejecute el siguiente comando:

   `$ ionic plugin remove com.marketo.plugin`

1. Para leer el complemento, ejecute el siguiente comando:

   `$ ionic plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

### Habilitar notificaciones push en xCode

1. Active la capacidad de notificación push en el proyecto de xCode.![Capacidad de notificación](assets/notification-capability.png)

### Seguimiento de notificaciones push

Pegue el siguiente código dentro de la función `application:didFinishLaunchingWithOptions:`.

>[!BEGINTABS]

>[!TAB Objetivo C]

```
Marketo *sharedInstance = [Marketo sharedInstance];

[sharedInstance trackPushNotification:launchOptions];
```

>[!TAB Swift]

```
let sharedInstance: Marketo = Marketo.sharedInstance()

sharedInstance.trackPushNotfication(launchOptions)
```

>[!ENDTABS]

### Inicializar Marketo Framework

Para asegurarse de que el marco de trabajo de Marketo se inicia al iniciar la aplicación, agregue el siguiente código en la función `onDeviceReady` de su archivo JavaScript principal.

Debe pasar `ionicCordova` como tipo de módulo para [!DNL Ionic] aplicaciones Cordova.

#### Sintaxis

```javascript
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

#### Parámetros

- Llamada de retorno de éxito : función que se ejecuta si el marco de trabajo de Marketo se inicializa correctamente.
- Callback Failure : función que se ejecuta si el marco de trabajo de Marketo no se puede inicializar.
- MUNCHKIN ID : Munchkin ID recibido de Marketo en el momento del registro.
- CLAVE SECRETA: Clave secreta recibida de Marketo en el momento del registro.

### Inicializar notificación push de Marketo

Para asegurarse de que se inicia la notificación push de Marketo, agregue el siguiente código después de la función inicializada en el archivo JavaScript principal.

#### Sintaxis

```javascript
// This function will Enable user notifications (prompts the user to accept push notifications in iOS)
marketo.initializeMarketoPush(
    function() { console.log("Marketo push successfully initialized."); },
    function(error) { console.log("an error occurred:" + error); },
    'YOUR_GCM_PROJECT_ID' // This is required for Android and will be ignored in iOS
);
```

#### Parámetros

- Llamada de retorno de éxito : función que se ejecuta si la notificación push de Marketo se inicializa correctamente.
- Función de devolución de llamada de error : que se ejecuta si la notificación push de Marketo no se inicializa.
- GCM_PROJECT_ID : Se encontró el ID del proyecto GCM en [Google Developers Console](https://accounts.google.com/ServiceLogin?service=cloudconsole&amp;passive=1209600&amp;osid=1&amp;continue=https://console.cloud.google.com/apis/dashboard&amp;followup=https://console.cloud.google.com/apis/dashboard) después de crear la aplicación.

También se puede anular el registro del token al cerrar la sesión.

```javascript
marketo.uninitializeMarketoPush(
  function() { console.log("Marketo push successfully uninitialized."); } ,
  function(error) { console.log("an error occurred:" + error); }
);
```

## Lead asociado

Puede crear un posible cliente de Marketo llamando a la función associatedLead.

### Sintaxis

```javascript
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

```javascript
// First create a lead as shown below
var lead = {};
lead[marketo.KEY_FIRST_NAME] = "Ionic";
lead[marketo.KEY_LAST_NAME] = "App";
lead[marketo.KEY_EMAIL] = email;
lead[marketo.KEY_ADDRESS] = "demo address";
lead[marketo.KEY_CITY] = "city";
lead[marketo.KEY_STATE] = "state";
lead[marketo.KEY_COUNTRY] = "country";
lead[marketo.KEY_POSTAL_CODE] = "postalCode";
lead[marketo.KEY_GENDER] = "gender";

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

```javascript
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

```javascript
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

Enlace los tipos de evento de &quot;pausa&quot; y &quot;reanudación&quot; como se muestra a continuación a los eventos de inicio y parada del informe. Se utiliza para rastrear el tiempo empleado en la aplicación móvil. Nota: Esto es obligatorio en Android.

```javascript
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

1. SDK DE MARKETO MME
1. API DE REST DE MARKETO
1. Envío de formulario

Según el método utilizado, distintos déclencheur y filtros reconocen un posible cliente recién creado. Los posibles clientes creados con el SDK de MME o la API de REST aparecen en los déclencheur y filtros &quot;Posible cliente creado&quot;. Los posibles clientes creados por los envíos de formularios aparecen en los déclencheur y filtros &quot;Rellena el formulario&quot;.

La práctica recomendada es mantener la coherencia con el método utilizado por la aplicación web al crear posibles clientes. Si ya tiene una aplicación web que utiliza el envío de formularios como mecanismo para crear posibles clientes, utilice el mismo mecanismo al crear posibles clientes en la aplicación híbrida. Si ya tiene una aplicación web que utiliza nuestra API de REST como mecanismo para crear posibles clientes, utilice el mismo mecanismo al crear posibles clientes en la aplicación híbrida. En los casos en los que no utilice el envío de formularios ni la API de REST como mecanismo para crear posibles clientes en la aplicación web, puede considerar la posibilidad de utilizar el SDK de MME para crear posibles clientes en Marketo.
