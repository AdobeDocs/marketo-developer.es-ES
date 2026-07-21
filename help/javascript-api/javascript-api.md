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
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 267
ht-degree: 2%

---

# API de JavaScript

Las integraciones de JavaScript del lado del cliente de Marketo proporcionan seguimiento de posibles clientes, formularios, personalización web y funciones de contenido predictivo. Debe tener una cuenta de Marketo para utilizar estas funciones.

La implementación suele implicar agregar código incrustado a la propiedad web. También puede llamar a las funciones de JavaScript expuestas por el código incrustado para agregar funcionalidad.

El código incrustado es único en la instancia de Marketo porque contiene un identificador de cuenta. En la interfaz de usuario de Marketo, vaya al panel correspondiente, copie el código incrustado en el portapapeles y péguelo en la página web.

## Seguimiento de posibles clientes (Munchkin)

El [código de seguimiento JavaScript de Munchkin](lead-tracking.md) de Marketo genera posibles clientes a partir de las visitas a su sitio web. También realiza un seguimiento de los visitantes que no han proporcionado información personal y crea posibles clientes anónimos que incluyen la dirección IP del usuario y otra información.

Configure Munchkin en la página de Munchkin, en el área de Administración de Marketo.

## Formularios 2.0

[Forms 2.0](forms-api-reference.md) permite a los especialistas en mercadotecnia crear formularios web sin conocimientos de programación. Forms puede residir en páginas de aterrizaje de Marketo o estar incrustado en cualquier página del sitio web.

Utilice la API de JavaScript de Forms 2.0 para ampliar la funcionalidad principal de un formulario web de Marketo.

## Personalización web

[Marketo Web Personalization](web-personalization.md) le ayuda a atraer clientes potenciales a su sitio web en tiempo real en función de quiénes son y qué hacen.

## Contenido predictivo

[Contenido predictivo de Marketo](predictive-content.md) utiliza aprendizaje automático y análisis predictivo para presentar contenido relevante a los visitantes de la web. Añada descripciones de texto e imágenes al contenido e incruste varias recomendaciones de contenido en el sitio web.
