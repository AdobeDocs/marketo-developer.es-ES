---
title: Campos estándar
feature: REST API, Field Management
description: Una tabla de campos estándar de Marketo.
exl-id: 147dbdff-4bc9-4ab3-8918-c4de3e1aa97a
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '1140'
ht-degree: 28%

---

# Campos estándar

Esta es una lista de campos estándar disponibles en Marketo a los que se puede acceder mediante la API.

Puede recuperar la lista de todos los nombres de campo admitidos disponibles en los registros de posibles clientes mediante el punto de conexión REST [Describir posible cliente](https://developer.adobe.com/marketo-apis/api/mapi/).

| Nombre de REST API | Nombre de SOAP API | Etiqueta descriptiva | Descripción |
| --- | --- | --- | --- |
| dirección | Dirección | Dirección | Dirección del posible cliente |
| annualRevenue | AnnualRevenue | Ingresos anuales | Ingresos anuales de la empresa del posible cliente |
| anonymousIP | AnonymousIP | IP anónima | Dirección IP de la primera visita web del posible cliente registrada |
| billingCity | BillingCity | Ciudad de facturación | Ciudad de la dirección de facturación del posible cliente |
| billingCountry | BillingCountry | País de facturación | País de la dirección de facturación del posible cliente |
| billingPostalCode | BillingPostalCode | Código postal de facturación | Código postal de la dirección de facturación del posible cliente |
| billingState | BillingState | Estado de facturación | Estado o provincia de la dirección de facturación del posible cliente |
| billingStreet | BillingStreet | Dirección de facturación | Dirección de la calle de facturación de la compañía del posible cliente |
| ciudad | Ciudad | Ciudad | Ciudad del posible cliente |
| compañía | Compañía | Nombre de la empresa | Nombre de la empresa del posible cliente |
| país | País | País | País del posible cliente |
| dateOfBirth | DateofBirth | Fecha de nacimiento | Fecha de nacimiento del posible cliente |
| departamento | Departamento | Departamento | El departamento del jefe en su compañía |
| doNotCall | DoNotCall | No llamar | Preferencia de no llamar del posible cliente |
| doNotCallReason | DoNotCallReason | Razón por la que no se debe llamar | Explicación de la preferencia de no llamar del posible cliente |
| correo electrónico | Correo electrónico | Correo electrónico | Dirección de correo electrónico del posible cliente. Campo de clave estándar de Marketo para registros de posibles clientes |
| fax | Fax | Número de fax | Número de fax del posible cliente |
| firstName | FirstName | Nombre | Nombre del posible cliente |
| industria | Industria | Industria | Sector del posible cliente |
| inferredCompany | InferredCompany | Compañía inferida | Nombre de la compañía deducido por la búsqueda inversa de IP de la primera visita web del posible cliente registrada |
| inferredCountry | InferredCountry | País inferido | País deducido por la búsqueda inversa de IP de la primera visita web del posible cliente registrada |
| lastName | LastName | Apellido | Apellido del posible cliente |
| leadRole | LeadRole | Función | El papel del líder en su compañía |
| leadScore | LeadScore | Puntaje del lead | Puntuación total otorgada al posible cliente mediante campañas y programas de puntuación |
| leadSource | LeadSource | Origen del lead | Campo que registra la fuente de la que se originó el posible cliente |
| leadStatus | LeadStatus | Estado del lead | Campo que registra el estado actual de marketing/ventas del posible cliente |
| mainPhone | MainPhone | Teléfono principal: | Número de teléfono principal de la compañía del posible cliente |
| jigsawContactId | ID de contacto de Marketo Jigsaw | Identificación de Data.com de Marketo | ID de Data.com del posible cliente, si está disponible |
| jigsawContactStatus | Estado del contacto de Jigsaw de Marketo | Estado de Data.com de Marketo | Estado de Data.com del posible cliente si está disponible |
| facebookDisplayName | MarketoSocialFacebookDisplayName | Nombre de Marketo Social para mostrar en Facebook | Nombre para mostrar de Facebook del posible cliente. Sistema rellenado durante el inicio de sesión social |
| facebookId | MarketoSocialFacebookId | Identificación de Marketo Social en Facebook | ID de Facebook del posible cliente. Sistema rellenado durante el inicio de sesión social |
| facebookPhotoURL | MarketoSocialFacebookPhotoURL | URL de imagen de Marketo Social para Facebook | URL de la foto del perfil de Facebook del posible cliente. Sistema rellenado durante el inicio de sesión social |
| facebookProfileURL | MarketoSocialFacebookProfileURL | URL de perfil de Marketo Social en Facebook | URL del perfil de Facebook del posible cliente. Sistema rellenado durante el inicio de sesión social |
| facebookReach | MarketoSocialFacebookReach | Alcance de Marketo Social en Facebook | El alcance Facebook del posible cliente. Sistema rellenado durante el inicio de sesión social |
| facebookReferredEnrollments | MarketoSocialFacebookReferredEnrollments | Ins. ref. desde Facebook de Marketo Social | Número de inscripciones conducidas atribuidas al posible cliente mediante Facebook. Sistema gestionado |
| facebookReferredVisits | MarketoSocialFacebookReferredVisits | Visitas referidas desde Facebook de Marketo Social | Número de visitas conducidas atribuidas al posible cliente mediante Facebook. Sistema gestionado |
| género | MarketoSocialGender | Sexo de Marketo Social | Sexo del posible cliente. Sistema rellenado durante el inicio de sesión social |
| lastReferredEnrollment | MarketoSocialLastReferredEnrollment | Última inscripción referida desde Marketo Social | Fecha de la última remisión completada. Sistema gestionado |
| lastReferredVisit | MarketoSocialLastReferredVisit | Última visita referida desde  Marketo Social | Fecha de la última visita conducida. Sistema gestionado |
| linkedInDisplayName | MarketoSocialLinkedInDisplayName | Nombre de Marketo Social para mostrar en LinkedIn | Nombre para mostrar de LinkedIn del posible cliente. Sistema rellenado durante el inicio de sesión social |
| linkedInId | MarketoSocialLinkedInId | Identificación de Marketo Social en LinkedIn | ID de LinkedIn del posible cliente. Sistema rellenado durante el inicio de sesión social |
| linkedInPhotoURL | MarketoSocialLinkedInPhotoURL | URL de imagen de Marketo Social para LinkedIn | URL de la foto de LinkedIn del posible cliente. Sistema rellenado durante el inicio de sesión social |
| linkedInProfileURL | MarketoSocialLinkedInProfileURL | URL de perfil de Marketo Social en LinkedIn | Perfil de LinkedIn del posible cliente. Sistema rellenado durante el inicio de sesión social |
| linkedInReach | MarketoSocialLinkedInReach | Alcance de Marketo Social en LinkedIn | Alcance de LinkedIn del posible cliente. Sistema rellenado durante el inicio de sesión social |
| linkedInReferredEnrollments | MarketoSocialLinkedInReferredEnrollments | Ins. ref. desde  LinkedIn de Marketo Social | Número de inscripciones conducidas atribuidas al posible cliente mediante LinkedIn. Sistema gestionado |
| linkedInReferredVisits | MarketoSocialLinkedInReferredVisits | Visitas referidas desde LinkedIn de Marketo Social | Número de visitas conducidas atribuidas al posible cliente mediante LinkedIn. Sistema gestionado |
| syndicationId |  - | Identificación de distribución de Marketo Social | ID social de Marketo interno del posible cliente. Sistema gestionado |
| totalReferredEnrollments | MarketoSocialTotalReferredEnrollments | Inscripciones referidas totales de Marketo Social | Número total de inscripciones de recomendación completadas atribuidas al posible cliente |
| totalReferredVisits | MarketoSocialTotalReferredVisits | Visitas referidas totales de Marketo Social | Número total de visitas conducidas atribuidas al posible cliente |
| twitterDisplayName | MarketoSocialTwitterDisplayName | Nombre de Marketo Social para mostrar en Twitter | Nombre para mostrar del Twitter del posible cliente. Sistema rellenado durante el inicio de sesión social |
| twitterId | MarketoSocialTwitterId | Identificación de Marketo Social en Twitter | ID de Twitter del posible cliente. Sistema rellenado durante el inicio de sesión social |
| twitterPhotoURL | MarketoSocialTwitterPhotoURL | URL de imagen de Marketo Social para Twitter | URL de la foto Twitter del posible cliente. Sistema rellenado durante el inicio de sesión social |
| twitterProfileURL | MarketoSocialTwitterProfileURL | URL de perfil de Marketo Social en Twitter | URL del perfil de Twitter del posible cliente. Sistema rellenado durante el inicio de sesión social |
| twitterReach | MarketoSocialTwitterReach | Alcance de Marketo Social en Twitter | Alcance del Twitter del plomo. Sistema rellenado durante el inicio de sesión social |
| twitterReferredEnrollments | MarketoSocialTwitterReferredEnrollments | Ins. ref. desde Twitter de Marketo Social | Número de inscripciones conducidas atribuidas al posible cliente mediante el Twitter. Sistema gestionado |
| twitterReferredVisits | MarketoSocialTwitterReferredVisits | Visitas referidas desde Twitter de Marketo Social | Número de visitas conducidas atribuidas al posible cliente mediante Twitter. Sistema gestionado |
| middleName | MiddleName | Segundo nombre | Segundo nombre del posible cliente |
| mobilePhone | MobilePhone | Número de teléfono móvil | Número de teléfono móvil del posible cliente |
| numberOfEmployees | NumberOfEmployees | Cantidad de empleados | Número de empleados de la compañía del posible cliente |
| teléfono | Teléfono | Número de teléfono | Número de teléfono del posible cliente |
| postalCode | PostalCode | Código postal | Código postal del posible cliente |
| clasificación | Calificación | Calificación de lead | Calificación de marketing/ventas del posible cliente |
| salutación | Saludo | Saludo | Saludo preferido del jefe, es decir, señor, señorita... etc. |
| sicCode | SICCode | Código SIC | Código de clasificación industrial estándar de la compañía del posible cliente |
| sitio | Sitio | Sitio |  |
| state | Estado | Estado | Estado del posible cliente |
| title | Título | Cargo | Puesto de responsable |
| cancelado | Suscripción cancelada | Suscripción cancelada | Estado de cancelación de suscripción del correo electrónico del posible cliente. Administrado parcialmente por el sistema. Evitará la recepción de correos electrónicos no operativos si se establece en true. |
| unsuscribedReason | UnsubscribedReason | Razón de la cancelación de la suscripción | Razón de la cancelación de la suscripción del posible cliente. Administrado parcialmente por el sistema. Se rellena con información de correo electrónico si el posible cliente ha cancelado la suscripción directamente desde un correo electrónico de Marketo. |
| sitio web | Sitio web | Sitio web | URL del sitio web de la compañía del posible cliente |
| createdAt |  - | Creado en | Hora a la que se creó inicialmente el registro de posibles clientes. Sistema gestionado |
| updatedAt |  - | Actualizado en | La última vez que se actualizó el registro de posibles clientes. Sistema gestionado |
| emailInvalid |  - | Email no válido | Estado de correo electrónico no válido. Todos los correos electrónicos a la dirección se bloquearán si se establece en true. Las devoluciones que indiquen que el correo electrónico no es válido establecen automáticamente este campo como verdadero. |
| emailInvalidCause |  - | Causa de email no válido | Causa del estado no válido del correo electrónico. El mensaje de rechazo instigador se registrará en este campo cuando el correo electrónico no válido se establezca en verdadero. |
| inferredCity |  - | Ciudad inferida | Ciudad del posible cliente deducida por la búsqueda inversa de IP de la primera visita web del posible cliente registrada. |
| inferredMetropolitanArea |  - | Área metropolitana inferida | Área metropolitana del posible cliente deducida por la búsqueda inversa de IP de la primera visita web del posible cliente registrada. |
| inferredPhoneAreaCode |  - | Código de área telefónico inferido | Código de área del teléfono del posible cliente deducido por la búsqueda inversa de IP de la primera visita web del posible cliente registrada. |
| inferredPostalCode |  - | Código postal inferido | Código postal del posible cliente deducido por la búsqueda inversa de IP de la primera visita web del posible cliente registrada. |
| inferredStateRegion |  - | Región del estado inferida | Región de estado del posible cliente deducida por la búsqueda inversa de IP de la primera visita web del posible cliente registrada. |
| isAnonymous |  - | Es anónimo | Estado anónimo del registro de posibles clientes. Sistema gestionado. |
| prioridad |  - | Prioridad | Prioridad de perspectivas de ventas del posible cliente. Sistema gestionado. |
| relativeScore |  - | Puntaje relativo | Puntuación relativa de perspectivas de ventas del posible cliente. Sistema gestionado. |
| urgencia |  - | Urgencia | Urgencia de perspectivas de ventas del posible cliente. Sistema gestionado. |
