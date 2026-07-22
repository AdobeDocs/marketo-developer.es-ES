---
title: Objetos Marketo
feature: Email Programs
description: Guía para utilizar Marketo Velocity con posibles clientes, oportunidades y objetos personalizados, carga de campos, acceso a las 10 listas principales, relaciones de SFDC y $TriggerObject.
exl-id: 88c63d72-7aa5-4550-9e1a-887a479872e1
TQID: https://experienceleague.adobe.com/PvLJb-AOk6DKaNINycpzk5ojZiL8UNcanRg3vXmsGCI
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 452
ht-degree: 1%

---

# Objetos Marketo

La implementación de Velocity de Marketo puede utilizar datos de estas fuentes de Marketo:

- Clientes potenciales
- Oportunidades
- Objetos personalizados
- Aplicación móvil
- Instalación de aplicación móvil

## Cargando campos

Para utilizar un campo en una secuencia de comandos, seleccione el campo de la lista correspondiente en el editor de símbolos de secuencia de comandos.

Si una secuencia de comandos hace referencia a un campo que no está cargado, la secuencia de comandos falla durante la ejecución. Arrastre un campo desde el menú de campo hasta la secuencia de comandos para cargarlo y añadir una referencia al cursor.

## Listas de oportunidades y objetos personalizados

Para Oportunidades y Objetos personalizados, Marketo carga solo los 10 objetos de cada tipo que se han actualizado más recientemente. Puede aumentar el número de objetos personalizados disponibles siguiendo los pasos que se describen aquí.

Marketo proporciona los objetos de una lista denominada `<objectName>List`, ordenados del registro actualizado más recientemente al registro actualizado menos recientemente. Para acceder al campo Importe desde la oportunidad actualizada más recientemente, utilice:

`${OpportunityList.get(0).Amount}`

En este ejemplo se hace referencia al objeto OpportunityList, se utiliza el método get para obtener acceso al registro del índice 0 y se recupera la propiedad Amount de ese registro.

Cuando arrastra un campo de oportunidad u objeto personalizado al editor, Marketo recupera automáticamente el campo del registro en el índice 0.

## Relaciones de objetos personalizados de SFDC

Para utilizar un objeto personalizado de SFDC, el objeto solo debe tener una relación con el posible cliente de Marketo. Los objetos suelen vincularse a través del contacto y de la cuenta. Sincronizar solo los objetos que tienen la relación de posible cliente/contacto habilitada.

## Objetos de déclencheur

Cuando una campaña utiliza el déclencheur Se agregó a la oportunidad, La oportunidad se ha actualizado o Se ha agregado a `<Custom Object Name>`, la variable `$TriggerObject` está disponible para los tokens de script que se ejecutan en la campaña de déclencheur. Esta variable no es compatible con el déclencheur Se ha actualizado `<Custom Object Name>`.

Esta variable hace referencia al objeto que activó la campaña. Contiene los mismos datos de registro que están disponibles cuando se tiene acceso al objeto a través de otro nombre de variable.

No use un token que haga referencia a `$TriggerObject` en una campaña por lotes. El objeto no está disponible en campañas por lotes y se produce un error en el envío por correo electrónico.

Por ejemplo, si un objeto personalizado de un pedido de producto déclencheur una campaña, la variable `$TriggerObject` expone el orden en el que se agregó el posible cliente.

El siguiente ejemplo muestra una secuencia de comandos para un correo electrónico de seguimiento de pedido:

```html
<div>
<strong>Your order information:</strong>
##pull information from the Triggering Order and format it in a list
<ul>
<li>Product Ordered: $!{TriggerObject.ProductName}</li>
<li>Product Quantity: $!{TriggerObject.Quanitity}</li>
<li>Shipping Address: $!{TriggerObject.ShippingAddress}</li>
<li>Billing Address: $!{TriggerObject.BillingAddress}</li>
<li>Order Total: $!{TriggerObject.Amount}</li>
</ul>
<p><a href="$!{TriggerObject.OrderURL}">View Your Order Online</a></p>
</div>
```

La acción desencadenante determina el objeto. No se necesita código adicional para determinar qué objeto disponible contiene los datos locales. Utilice `$TriggerObject` cuando esté disponible y sea adecuado, ya que identifica explícitamente el objeto al que se hace referencia.

Nota: Cuando use `$TriggerObject`, seleccione los campos del objeto en el panel de edición para que estén disponibles para el script.

Nota 2: `$TriggerObject` solo funciona para déclencheur &quot;Agregados&quot;, no para déclencheur &quot;Actualizados&quot;.
