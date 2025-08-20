---
title: Objetos Marketo
feature: Email Programs
description: Información general sobre el uso de objetos de Marketo con scripts de Velocity
source-git-commit: 3ccb27a0d184e0c1314979d404022bc4e0794f7b
workflow-type: tm+mt
source-wordcount: '518'
ht-degree: 0%

---

# Objetos Marketo

La implementación de Velocity de Marketo puede funcionar con datos de varias fuentes dentro de Marketo: posibles clientes, oportunidades, objetos personalizados, aplicación móvil, instalación de aplicaciones móviles.

## Cargando campos

Para cargar un campo para utilizarlo en una secuencia de comandos, ese campo debe comprobarse en la lista correspondiente del editor de símbolos de secuencia de comandos.

Si no carga un campo y se hace referencia a él dentro del script, la ejecución del script falla durante la ejecución. Puede arrastrar y soltar campos del menú de campo en el script. Esto les permite cargarse y añade una referencia al campo en el cursor.

## Listas de oportunidades y objetos personalizados

Al recuperar a partir de oportunidades u objetos personalizados, solo se cargan los 10 objetos de un tipo actualizados más recientemente. El número de objetos personalizados disponibles se puede aumentar siguiendo los pasos descritos aquí. Se proporcionan como una lista, con el nombre de `<objectName>List` y se ordenan del registro actualizado más reciente al menos reciente. Por lo tanto, para acceder al campo Importe desde la oportunidad que se actualizó más recientemente, debe utilizar lo siguiente:

`${OpportunityList.get(0).Amount}`

En este ejemplo, se hace referencia al objeto OpportunityList, se utiliza el método get para tener acceso al registro indizado en 0 y, a continuación, se recupera la propiedad Amount del objeto devuelto. Si arrastra un campo desde una oportunidad u objeto personalizado al editor, recuperará automáticamente el campo del registro indexado en 0.

## Relaciones de objetos personalizados de SFDC

Para estar disponible para su uso, un objeto personalizado de SFDC debe tener solo una relación con el posible cliente de Marketo. Los objetos suelen vincularse a través del contacto y de la cuenta, por lo que es importante sincronizar únicamente los objetos con Marketo con la relación de posible cliente/contacto habilitada.

## Objetos de déclencheur

Cuando se activa una campaña a través de los déclencheur Agregar a oportunidad, Oportunidad se actualiza o Se agrega a `<Custom Object Name>`, una variable especial está disponible en Tokens de script ejecutados en el contexto de la campaña de déclencheur: `$TriggerObject `(no compatible con el déclencheur `<Custom Object Name>` se ha actualizado).  Si se usa un token con una referencia `$TriggerObject` en una campaña por lotes, el envío de correo electrónico fallará, ya que este objeto no está disponible en campañas por lotes de ningún tipo.  Esta es una referencia al objeto que activó la campaña. El objeto contiene todos los datos a los que se tiene acceso en el registro mediante un nombre de variable diferente.

Por ejemplo, si una campaña se activó mediante un objeto personalizado para un pedido de producto, el orden al que se agregó el posible cliente se expone en la variable `$TriggerObject`.

Este es un ejemplo de script para un correo electrónico de seguimiento de pedidos:

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

La ventaja de usar la variable `$TriggerObject` es que no necesita dedicar ningún código para determinar de cuál de los objetos disponibles desea extraer los datos locales.  El objeto se determina mediante la acción desencadenadora. Esta es la forma más explícita de elegir un objeto al que hacer referencia y debe utilizarse siempre que esté disponible y sea adecuado.

Nota: Cuando se usa `$TriggerObject`, los campos deben comprobarse en el panel de edición para que el objeto esté disponible para el script.

Nota 2: `$TriggerObject` solo funciona para déclencheur &quot;agregados&quot; y no para déclencheur &quot;actualizados&quot;.
