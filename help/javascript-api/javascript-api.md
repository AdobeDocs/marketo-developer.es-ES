---
title: API de JavaScript
description: Aprenda a utilizar la API de Marketo JavaScript con código incrustado para el seguimiento de posibles clientes de Munchkin, Forms 2.0, Web Personalization y contenido predictivo.
feature: Munchkin Tracking Code, Forms, Web Personalization, Predictive Content, Social, Javascript
exl-id: 6129a467-be44-44bd-9e02-62009070c318
TQID: https://experienceleague.adobe.com/R9kIFBiH6jc64ay85QkumV7jCsFnj9J0t5G4IJKEsJM
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: b0bb9048-d951-48d8-8232-45cf248a7e27id: e2290edd-b061-4880-9d79-dee306cf5aa9id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: e0eb8757-182f-49f3-94a4-1587d16f5094id: eb30f47f-d87a-400f-8f78-63ce7979ff56
source-git-commit: 00118a89f25a23b931fac671130932bb0e0e4e4e
workflow-type: tm+mt
source-wordcount: 311
ht-degree: 1%

---

# API de JavaScript

A continuación se ofrece una descripción general de las funciones de integración de JavaScript del lado del cliente de Marketo. Debe tener una cuenta de Marketo para poder utilizar estas funciones. Normalmente, la implementación simplemente implica agregar un &quot;código incrustado&quot; a la propiedad web. Opcionalmente, puede utilizar funcionalidad adicional llamando a funciones de JavaScript que se exponen mediante el código incrustado. Estas funciones están completamente documentadas aquí.

El código incrustado es único para la instancia de Marketo porque contiene un identificador de cuenta. Obtenga el código incrustado navegando hasta el panel correspondiente de la interfaz de usuario de Marketo, copie en el portapapeles y pegue en la página web.

## Seguimiento de posibles clientes (Munchkin)

El [código de seguimiento JavaScript de Munchkin](lead-tracking.md) de Marketo es clave para las capacidades de Marketo. Le permite generar posibles clientes a partir de las visitas a su sitio web. Incluso rastrea visitantes que aún no le han dado su información personal, creando posibles clientes anónimos que incluyen la dirección IP del usuario y otra información. La configuración de Munchkin se realiza en la página de Munchkin, en el área de Administración de Marketo.

## Formularios 2.0

[Forms 2.0](forms-api-reference.md) permite a los especialistas en mercadotecnia crear formularios web hermosos, estables y flexibles sin tener conocimientos de programación. Forms puede residir en páginas de aterrizaje de Marketo y estar incrustado en cualquier página del sitio web. La funcionalidad principal de un formulario web de Marketo se puede ampliar mediante la API de JavaScript de Forms 2.0.

## Personalización web

[Marketo Web Personalization](web-personalization.md) es una plataforma de Targeting &amp; Personalization que te ayuda a atraer a miles de clientes potenciales a tu sitio web en tiempo real en función de quiénes son y qué hacen.

## Contenido predictivo

El [contenido predictivo de Marketo](predictive-content.md) le permite atraer a los visitantes web con el contenido más relevante, impulsado por el aprendizaje automático y el análisis predictivo. Mejore su contenido con descripciones de texto e imágenes e incruste varias recomendaciones de contenido en su sitio web.

