---
title: Introducción
description: Empiece con las API de Marketo Engage y el modelo de datos, que incluye posibles clientes, actividades, programas, etiquetas, listas, directrices de REST y aviso de obsolescencia de SOAP.
exl-id: 78c44c32-4e59-4d55-a45c-ef0d7dac814d
TQID: https://experienceleague.adobe.com/0lfzor5EQJ0VqIh4fqlK29OiPmRCy6fnEtncJ38r-OM
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: b0bb9048-d951-48d8-8232-45cf248a7e27id: c954475c-8548-4e33-a0b8-6b550d956115id: d1d0a9cd-295d-4976-8c39-ddae266f240eid: e64968b2-4ee5-47f9-8cae-0588f184b9ebid: ea90ebee-5c84-42d9-8b21-006bdabc95a3
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: aa2f3246-cb95-4b30-8899-fdf7d73550ccid: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1228
ht-degree: 2%

---

# Introducción

Marketo Engage es una plataforma de automatización de marketing para administrar programas y campañas multicanal personalizados para clientes y posibles clientes. Puede ampliar la plataforma a través de sus puntos de integración.

Esta página presenta las entidades principales de Marketo Engage y sus relaciones.

>[!NOTE]
>
>La API de SOAP se está desaprobando y dejará de estar disponible a partir del 31 de julio de 2026. Use la API de Marketo [REST](./rest-api/rest-api.md) para todo el desarrollo nuevo. Migrar los servicios existentes en esa fecha para evitar interrupciones del servicio. Si un servicio usa la API de SOAP, consulte la API de SOAP [Guía de migración](./soap-api/migration.md).
>

Cuando la conexión nativa de SFDC o MS Dynamics CRM está habilitada en una instancia de Marketo Engage, estos objetos son de solo lectura:

- Compañía
- Oportunidad
- Rol de la oportunidad
- Vendedor

![Modelo de datos](assets/data_model.png)

## Persona (posibles clientes)

Las personas son la base de la automatización del marketing. Marketo se refiere a todos los registros de personas que no son vendedores como posibles clientes, independientemente de si las ventas los consideran posibles clientes, posibles clientes, sospechosos o contactos.

El objeto de posible cliente incluye campos estándar como correo electrónico, nombre y apellidos. Puede agregar campos para almacenar otra información, así como leer y escribir atributos personalizados del mismo modo que los campos estándar. Encuentre la lista completa de campos en **[!UICONTROL Administración]** > **[!UICONTROL Administración de campos]** en Marketo.

Marketo identifica de forma exclusiva a los posibles clientes mediante el campo id. Debe aplicar otras claves únicas fuera del sistema.

API relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads), [JavaScript](javascript-api/lead-tracking.md#lead-tracking-api)

## Actividades

Los posibles clientes pueden interactuar con su organización de varias formas, como visitar una página web, asistir a una feria o descargar un documento técnico. Marketo captura estas acciones como actividades para que los especialistas en marketing puedan comprender qué hizo un posible cliente y cuándo se produjo.

Las actividades siempre están relacionadas con los posibles clientes mediante leadId.

También puede definir actividades personalizadas. Después de crear y publicar una actividad personalizada, puede agregar instancias de ella a través de la API de Marketo. Para obtener más información, consulte [Explicación de las actividades personalizadas](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-activities/understanding-custom-activities).

API relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities), [JavaScript](javascript-api/lead-tracking.md#munchkin-behavior)

## Programas y campañas

Un programa organiza los esfuerzos de marketing relacionados con un experto en marketing en una ubicación. Por ejemplo, una explosión de correo electrónico puede ser un programa.

Un posible cliente puede realizar varias acciones o actividades asociadas a un programa. Este proceso se conoce como progresión del posible cliente. Para un programa de explosión de correo electrónico, la progresión puede registrar cuándo Marketo envía el correo electrónico, cuándo la persona lo abre y si hace clic en un vínculo.

Una campaña tiene un propósito y un objetivo específicos dentro de un programa. Por ejemplo, una campaña puede seleccionar un grupo de posibles clientes y enviar una notificación por correo electrónico. Otra campaña puede notificar a un representante de ventas cuando un posible cliente hace clic en un vínculo en la explosión del correo electrónico.

API relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/mapi#tag/Campaigns)

## Etiquetas

Las etiquetas agrupan y categorizan los datos del programa para la creación de informes. Utilice etiquetas para medir la eficacia del programa y el ROI.

Como administrador de Marketo, puede crear los tipos de etiquetas opcionales y requeridos que los usuarios seleccionan cuando crean un programa. Puede definir los valores posibles para cada tipo de etiqueta en función de los requisitos de informes de su empresa.

Por ejemplo, cree un tipo de etiqueta &quot;Región&quot; personalizado con valores como Noreste y Sureste para analizar qué región genera la mayor cantidad de posibles clientes. Cree un tipo de etiqueta &quot;Propietario&quot; para comparar qué propietarios de programa, como María, David o Juan, tienen el mayor impacto en la creación de posibles clientes y oportunidades. Para obtener más información, consulte [Explicación de las etiquetas](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/understanding-tags).

API relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/asset)

## Listas

Las listas organizan colecciones de posibles clientes. Marketo ofrece dos tipos:

- Una lista estática es una colección fija desde la que un experto en marketing puede agregar o quitar posibles clientes.
- Una lista inteligente es una colección dinámica basada en características definidas.

Por ejemplo, una lista inteligente llamada &quot;Todos los posibles clientes que han visitado la página de precios en nuestro sitio web&quot; sigue creciendo a medida que más posibles clientes visitan esa página. Para obtener más información, consulte la [documentación de Marketo Engage](https://experienceleague.adobe.com/es/docs/marketo/using/home).

API relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists)

## Oportunidades

Una oportunidad representa un potencial acuerdo de ventas que los especialistas en marketing ofrecen a las ventas. En Marketo, una oportunidad está asociada a un posible cliente o contacto y a una organización.

Una función de oportunidad conecta un posible cliente con una organización y describe la función del posible cliente en esa organización.

API relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/mapi#tag/Opportunities)

## Compañías

Una organización, a veces denominada cuenta en Marketo, es la organización a la que pertenece una persona.

Para obtener una atribución precisa del retorno de la inversión en los informes de retorno de la inversión de Marketo o en Revenue Cycle Analytics (RCA), asocie a las personas con sus organizaciones y oportunidades.

API relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/mapi#tag/Companies)

## Recursos

Assets incluye páginas de aterrizaje, correos electrónicos, formularios e imágenes utilizados en un programa. Un recurso puede ser local para un programa específico o global. Los recursos globales están disponibles para cada programa.

API relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/asset)

## Tókenes

Los tokens permiten a los especialistas en marketing personalizar los mensajes con los recursos y añadir lógica a las acciones de flujo. Marketo proporciona tokens para el sistema general, los programas, los posibles clientes y las empresas.

Por ejemplo, coloque el token de posible cliente `{{lead.First Name}}` en un mensaje de correo electrónico para mostrar el nombre del posible cliente.

Los tokens definidos en el nivel de programa o carpeta se denominan &quot;Mis tokens&quot; en Marketo. Mis tokens tienen tres tipos:

- Local: se crea en una carpeta de campaña o programa específico y solo está disponible en esa carpeta o programa.
- Heredado: se crea en el nivel de carpeta de campaña y está disponible para todos los programas de esa carpeta.
- Anulado: se modifica con un valor personalizado en el nivel de programa sin cambiar el valor principal de Mi token en el nivel de carpeta de programa.

Mis tokens utilizan la convención de nombres `{{my.My Token}}`, con la palabra &quot;my&quot; al principio del nombre del token. Por ejemplo, un tipo de fecha My Token denominado EventDate tiene el nombre de token `{{my.EventDate}}`. Para obtener más información, consulte [Explicación de mis tokens en un programa](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/programs/tokens/understanding-my-tokens-in-a-program).

API relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/asset#tag/Tokens)

## Objetos personalizados

Un objeto personalizado de Marketo crea una relación &quot;uno a varios&quot; o &quot;varios a varios&quot; (Edge-Bridge-Edge) entre Marketo Leads y los registros de objetos personalizados.

Después de crear y publicar un objeto personalizado de Marketo, puede realizar operaciones CRUD en él a través de la API de Marketo. Cuando se agregan registros nuevos, puede utilizar un déclencheur de lista inteligente para responder. También puede usar datos de objeto personalizados como filtro de lista inteligente para la segmentación o en correos electrónicos a través de [Scripts de correo electrónico](email-scripting.md). Para obtener más información sobre cómo crear objetos personalizados, consulte la [documentación de Marketo Engage](https://experienceleague.adobe.com/es/docs/marketo/using/home).

API relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects)

## Vendedores

Puede administrar los registros del vendedor y sus relaciones con los posibles clientes en Marketo cuando no esté habilitada la integración nativa de CRM. Estos registros contienen información como Nombre, Correo electrónico y Puesto. Cuando un vendedor es propietario de un posible cliente, puede utilizar esta información para filtrar y crear tokens.

Administre la relación con un vendedor en el nivel de cliente potencial a través del campo &quot;externalSalesPersonId&quot;. Actualice este campo mediante la API [Sincronizar posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/syncLeadUsingPOST).

API relacionadas: [REST](https://developer.adobe.com/marketo-apis/api/mapi#tag/Sales-Persons)
