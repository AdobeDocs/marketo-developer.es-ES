---
title: Campos estándar
feature: REST API, Field Management
description: Examine la lista completa de campos de posibles clientes estándar de Marketo con nombres, etiquetas y descripciones REST, además de cómo recuperarlos mediante la API Describir posible cliente.
exl-id: 147dbdff-4bc9-4ab3-8918-c4de3e1aa97a
TQID: https://experienceleague.adobe.com/vu2wGk36XJ243vwavhfLE7Vc9vMIJKGx6vmVqMRgEDA
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: e64968b2-4ee5-47f9-8cae-0588f184b9ebid: f82558ea-6af5-44eb-a424-5b3389abb0a3
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: bcf56d2102f2f60eac5ad3318d348fd020391e6b
workflow-type: tm+mt
source-wordcount: 688
ht-degree: 19%

---

# Campos estándar

En la siguiente tabla se enumeran los campos estándar de Marketo disponibles a través de la API. Incluye el nombre, la etiqueta y la descripción de la API de REST de cada campo.

Use el extremo REST [Describir posible cliente](https://developer.adobe.com/marketo-apis/api/mapi) para recuperar todos los nombres de campo admitidos por sus registros de posibles clientes.

| Nombre de REST API | Etiqueta descriptiva | Descripción |
| --- | --- | --- |
| dirección | Dirección | Dirección del posible cliente |
| annualRevenue | Ingresos anuales | Ingresos anuales de la empresa del posible cliente |
| anonymousIP | IP anónima | Dirección IP de la primera visita web del posible cliente registrada |
| billingCity | Ciudad de facturación | Ciudad de la dirección de facturación del posible cliente |
| billingCountry | País de facturación | País de la dirección de facturación del posible cliente |
| billingPostalCode | Código postal de facturación | Código postal de la dirección de facturación del posible cliente |
| billingState | Estado de facturación | Estado o provincia de la dirección de facturación del posible cliente |
| billingStreet | Dirección de facturación | Dirección de la calle de facturación de la compañía del posible cliente |
| ciudad | Ciudad | Ciudad del posible cliente |
| compañía | Nombre de la empresa | Nombre de la empresa del posible cliente |
| país | País | País del posible cliente |
| dateOfBirth | Fecha de nacimiento | Fecha de nacimiento del posible cliente |
| departamento | Departamento | El departamento del jefe en su compañía |
| doNotCall | No llamar | Preferencia de no llamar del posible cliente |
| doNotCallReason | Razón por la que no se debe llamar | Explicación de la preferencia de no llamar del posible cliente |
| correo electrónico | Correo electrónico | Dirección de correo electrónico del posible cliente. Campo de clave estándar de Marketo para registros de posibles clientes |
| fax | Número de fax | Número de fax del posible cliente |
| firstName | Nombre | Nombre del posible cliente |
| industria | Industria | Sector del posible cliente |
| inferredCompany | Compañía inferida | Nombre de la compañía deducido por la búsqueda inversa de IP de la primera visita web del posible cliente registrada |
| inferredCountry | País inferido | País deducido por la búsqueda inversa de IP de la primera visita web del posible cliente registrada |
| lastName | Apellido | Apellido del posible cliente |
| leadRole | Función | El papel del líder en su compañía |
| leadScore | Puntaje del lead | Puntuación total otorgada al posible cliente mediante campañas y programas de puntuación |
| leadSource | Origen del lead | Campo que registra la fuente de la que se originó el posible cliente |
| leadStatus | Estado del lead | Campo que registra el estado actual de marketing/ventas del posible cliente |
| mainPhone | Teléfono principal: | Número de teléfono principal de la compañía del posible cliente |
| jigsawContactId | Identificación de Data.com de Marketo | ID de Data.com del posible cliente, si está disponible |
| jigsawContactStatus | Estado de Data.com de Marketo | Estado de Data.com del posible cliente si está disponible |
| middleName | Segundo nombre | Segundo nombre del posible cliente |
| mobilePhone | Número de teléfono móvil | Número de teléfono móvil del posible cliente |
| numberOfEmployees | Cantidad de empleados | Número de empleados de la compañía del posible cliente |
| teléfono | Número de teléfono | Número de teléfono del posible cliente |
| postalCode | Código postal | Código postal del posible cliente |
| clasificación | Calificación de lead | Calificación de marketing/ventas del posible cliente |
| salutación | Saludo | El saludo preferido de Lead, es decir, Señor, Señoritas... y así sucesivamente |
| sicCode | Código SIC | Código de clasificación industrial estándar de la compañía del posible cliente |
| sitio | Sitio |  |
| estado | Estado | Estado del posible cliente |
| título | Cargo | Puesto de responsable |
| cancelado | Suscripción cancelada | Estado de cancelación de suscripción del correo electrónico del posible cliente. Administrado parcialmente por el sistema. Evitará la recepción de correos electrónicos no operativos si se establece en true. |
| unsuscribedReason | Razón de la cancelación de la suscripción | Razón de la cancelación de la suscripción del posible cliente. Administrado parcialmente por el sistema. Se rellena con información de correo electrónico si el posible cliente ha cancelado la suscripción directamente desde un correo electrónico de Marketo. |
| sitio web | Sitio web | URL del sitio web de la compañía del posible cliente |
| createdAt | Creado en | Hora a la que se creó inicialmente el registro de posibles clientes. Sistema gestionado |
| updatedAt | Actualizado en | La última vez que se actualizó el registro de posibles clientes. Sistema gestionado |
| emailInvalid | Correo electrónico no válido | Estado de correo electrónico no válido. Todos los correos electrónicos a la dirección se bloquearán si se establece en true. Las devoluciones que indiquen que el correo electrónico no es válido establecen automáticamente este campo como verdadero. |
| emailInvalidCause | Causa de email no válido | Causa del estado no válido del correo electrónico. El mensaje de rechazo instigador se registrará en este campo cuando el correo electrónico no válido se establezca en verdadero. |
| inferredCity | Ciudad inferida | Ciudad del posible cliente deducida por la búsqueda inversa de IP de la primera visita web del posible cliente registrada. |
| inferredMetropolitanArea | Área metropolitana inferida | Área metropolitana del posible cliente deducida por la búsqueda inversa de IP de la primera visita web del posible cliente registrada. |
| inferredPhoneAreaCode | Código de área telefónico inferido | Código de área del teléfono del posible cliente deducido por la búsqueda inversa de IP de la primera visita web del posible cliente registrada. |
| inferredPostalCode | Código postal inferido | Código postal del posible cliente deducido por la búsqueda inversa de IP de la primera visita web del posible cliente registrada. |
| inferredStateRegion | Región del estado inferida | Región de estado del posible cliente deducida por la búsqueda inversa de IP de la primera visita web del posible cliente registrada. |
| isAnonymous | Es anónimo | Estado anónimo del registro de posibles clientes. Sistema gestionado. |
| prioridad | Prioridad | Prioridad de Insight de ventas del posible cliente. Sistema gestionado. |
| relativeScore | Puntaje relativo | Puntuación relativa de Insight de ventas del posible cliente. Sistema gestionado. |
| urgencia | Urgencia | Urgencia de Insight de ventas del posible cliente. Sistema gestionado. |
