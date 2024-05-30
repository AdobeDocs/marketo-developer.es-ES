---
title: "Primeros pasos"
description: "Introducción a las API de Marketo"
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '1242'
ht-degree: 0%

---


# Introducción

Marketo es una plataforma de automatización de marketing que permite a los especialistas en marketing administrar programas y campañas multicanal personalizados a clientes y posibles clientes. La plataforma Marketo se puede ampliar utilizando puntos de integración. A continuación se encuentran las entidades principales y sus relaciones.

Los siguientes objetos no están disponibles mediante la API de REST cuando la sincronización nativa está habilitada: Empresa, Oportunidad, Función de oportunidad, Vendedor

![Modelo de datos](assets/data_model.png)

## Persona (posibles clientes)

Las personas son la base de cualquier plataforma de automatización de marketing. En Marketo, todos los registros de personas no vendedoras se denominan posibles clientes, independientemente de si están designados como posibles clientes, posibles clientes, sospechosos, contactos, etc., desde el punto de vista de las ventas. El objeto de posible cliente viene con un conjunto de [campos estándar](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadFieldsUsingGET) como correo electrónico, nombre y apellidos. Se pueden agregar campos adicionales al tipo de objeto de posible cliente para ampliar los tipos de información asociados con los registros del sistema. Los atributos personalizados se pueden leer y escribir en igual que los campos estándar. Puede encontrar una lista completa de campos en Marketo **[!UICONTROL Administrador]** > **[!UICONTROL Administración de campos]** menú. Los posibles clientes se identifican de forma exclusiva en Marketo mediante el campo ID. Otras claves únicas deben aplicarse externamente desde el sistema.

API relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads), [JABÓN](soap-api/leads.md), [JavaScript](javascript-api/lead-tracking.md#lead-tracking-api)

## Actividades

Los posibles clientes interactúan con su organización de varias formas. Un posible cliente puede visitar una página del sitio web de su empresa, asistir a una feria o descargar un documento técnico. Cada una de estas acciones se puede capturar dentro de Marketo para ayudar a un experto en marketing a comprender mejor qué actividades realizó un posible cliente y cuándo lo hizo para que pueda coordinar comunicaciones oportunas y relevantes. Las actividades siempre están relacionadas con los posibles clientes mediante leadId.

Puede definir sus propias actividades personalizadas. Una vez creada y publicada una actividad personalizada, puede añadir actividades personalizadas mediante la API de Marketo. Puede encontrar más información sobre las actividades personalizadas [aquí](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-activities/understanding-custom-activities).

API relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities), [JABÓN](soap-api/activities.md), [JavaScript](javascript-api/lead-tracking.md#munchkin-behavior)

## Programas y campañas

Un programa es el mecanismo mediante el cual un experto en marketing organiza todos sus diferentes tipos de esfuerzos de marketing desde una ubicación central. Un ejemplo de programa es una explosión de correo electrónico. Un posible cliente puede realizar varias acciones o actividades relacionadas con un programa determinado que se asocian al programa. Esto se conoce como progresión de plomo. Una progresión de ejemplo de un programa de ráfaga de correo electrónico registraría cuándo se envía un correo electrónico a un posible cliente, cuándo lo abre o si hace clic en un vínculo del correo electrónico.

Las campañas se crean para servir un propósito y un objetivo específicos dentro de un programa. Un ejemplo de campaña podría ser reducir un grupo de posibles clientes y enviarles la notificación por correo electrónico o notificar a un representante de ventas para que realice un seguimiento si un posible cliente hace clic en un vínculo dentro del programa de notificación por correo electrónico.

API relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns), [JABÓN](soap-api/getcampaignsforsource.md)

## Etiquetas

Las etiquetas son una forma de agrupar datos para la creación de informes. Estos identificadores proporcionan la capacidad de categorizar los datos y definir cómo desea informar sobre el programa para comprender la eficacia del programa y el ROI.

Como administrador de Marketo, tiene la capacidad de crear los tipos de etiquetas opcionales y requeridos que están disponibles para su selección cuando un usuario de Marketo crea un programa. Usted define los valores posibles para cada uno de estos tipos de etiquetas y reflejan cómo su empresa desea utilizar etiquetas personalizadas con fines de creación de informes.

Por ejemplo, es posible que desee crear un tipo de etiqueta &quot;Región&quot; personalizado con varios valores de etiqueta (por ejemplo, Noreste, Sureste) que le permitan analizar qué región genera la mayor cantidad de posibles clientes. O, por ejemplo, puede crear un tipo de etiqueta &quot;Propietario&quot;, que le permite evaluar y comprender qué propietarios de programa (por ejemplo, María, David o Juan) tienen el mayor impacto en la creación de posibles clientes y oportunidades. Encontrará más información sobre las etiquetas [aquí](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/understanding-tags).

API relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/asset/), [JABÓN](soap-api/gettags.md)

## Listas

Las listas permiten a un experto en marketing organizar una colección de posibles clientes. Existen dos tipos de listas dentro de Marketo: estáticas e inteligentes. Una lista estática es una lista fija de posibles clientes que un experto en marketing puede agregar o quitar a su elección. Una lista inteligente es una colección dinámica de posibles clientes basada en un conjunto de características designadas. Un ejemplo de lista inteligente sería &quot;Todos los posibles clientes que han visitado la página de precios de nuestro sitio web&quot;. Esta lista inteligente sigue creciendo a medida que más posibles clientes visitan la página de precios. Se puede encontrar más información sobre las listas [aquí](https://experienceleague.adobe.com/en/docs/marketo/using/home).

API relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists), [JABÓN](soap-api/getimporttoliststatus.md)

## Oportunidades

Los especialistas en marketing entregan posibles clientes a las ventas en forma de oportunidad. Una oportunidad representa un potencial acuerdo de ventas y está asociada a un posible cliente o contacto y a una organización en Marketo. Una función de oportunidad es la intersección entre un posible cliente determinado y una organización. La función de oportunidad pertenece a la función de un posible cliente dentro de la organización.

API relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities), [JABÓN](soap-api/getmobjects.md)

## Compañías

Una organización, a veces denominada cuenta en Marketo, hace referencia a la organización a la que pertenece una persona. Al utilizar los informes de ROI en Marketo o Revenue Cycle Analytics (RCA), es importante asociar a las personas con su organización y sus oportunidades para poder determinar la atribución de ROI adecuada.

API relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies), [JABÓN](soap-api/leads.md)

## Recursos

Los recursos hacen referencia a páginas de aterrizaje, correos electrónicos, formularios e imágenes que se utilizan dentro de un programa. Los recursos pueden ser locales para un programa determinado o globales. Los recursos globales están disponibles en cualquier programa.

API relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/asset/)

## Tokens

Los tokens permiten a un experto en marketing personalizar mensajes con recursos y añadir lógica dentro de las acciones de flujo. Existen tokens para el sistema general, los programas, los posibles clientes y las empresas. Un ejemplo de token de posible cliente es {{lead.First Name}}. Este token se puede colocar dentro de un correo electrónico para mostrar el nombre del posible cliente.

Los tokens definidos en el nivel de programa o carpeta se denominan &quot;Mis tokens&quot; en Marketo. Mis tokens pueden ser de uno de los tres tipos, locales, heredados o anulados.

Mis tokens creados localmente en una carpeta o programa de campaña específico están disponibles para ese programa o carpeta de campaña específica (local). Mis tokens creados en el nivel de carpeta de campaña están disponibles para su uso en todos los programas contenidos en esa carpeta de campaña (heredados). Mis tokens modificados en el nivel de programa con valores personalizados no cambian el valor Mi token principal del token en el nivel de carpeta de programa (anulado).

Mis tokens utilizan la convención de nombres {{my.My Token}}, with the word "my" added to the beginning of the token name. For example, if you create a Date type My Token with the name EventDate, the name of the token is {{my.EventDate}}. Encontrará más información sobre Mis tokens [aquí](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/programs/tokens/understanding-my-tokens-in-a-program).

API relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens), [JABÓN](soap-api/getcampaignsforsource.md)

## Objetos personalizados

Un objeto personalizado de Marketo permite crear una relación &quot;uno a varios&quot; o &quot;varios a varios&quot; (Edge-Bridge-Edge) entre los posibles clientes de Marketo y los registros de objetos personalizados. Una vez creado y publicado un objeto personalizado de Marketo, puede realizar operaciones CRUD en el objeto personalizado mediante la API de Marketo. Encontrará más información sobre la creación de objetos personalizados [aquí](https://experienceleague.adobe.com/en/docs/marketo/using/home). Cuando se agregan registros nuevos al objeto personalizado, puede utilizar un déclencheur de lista inteligente para responder. También puede utilizar datos de objeto personalizados como filtro en listas inteligentes (segmentación) o en correos electrónicos con [Scripts de correo electrónico](email-scripting.md).

API relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects), [JABÓN](soap-api/custom-objects.md)

## Vendedores

Los registros del vendedor y las relaciones con los posibles clientes se pueden administrar en Marketo cuando no hay una integración nativa de CRM habilitada. Estos registros contienen información básica sobre el vendedor, como el nombre, el correo electrónico y el cargo, que se puede utilizar para filtrar y crear tokens en Marketo cuando un posible cliente es propiedad de uno. La relación con un vendedor se administra en el nivel de cliente potencial a través del campo &quot;externalSalesPersonId&quot;, que debe actualizarse a través de la variable [Sincronizar posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/syncLeadUsingPOST) API.

API relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Sales-Persons)
