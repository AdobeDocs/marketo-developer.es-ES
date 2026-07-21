---
title: Asignaciones de respuesta
feature: Webhooks
description: Asignaciones de respuestas de Webhooks de Marketo para JSON y XML, asignación de atributos a campos de posible cliente con nombres de API de SOAP, notación de puntos y matrices y compatibilidad de tipos.
exl-id: 95c6e33e-487c-464b-b920-3c67e248d84e
TQID: https://experienceleague.adobe.com/-OGDeKLPS1KmWGIKj6BGq5DGXoCSj5ip-dVr7-kKDro
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 374
ht-degree: 1%

---

# Asignaciones de respuesta

Marketo puede traducir datos de ganchos web de JSON o XML y escribir los valores en campos de posibles clientes. El parámetro Marketo Field siempre usa el [nombre de API de SOAP](../rest-api/fields.md) del campo.

Cada webhook puede tener un número ilimitado de asignaciones de respuestas. Para agregar o editar asignaciones, seleccione [!UICONTROL Editar] en el panel Asignaciones de respuestas del gancho web:

![Asignación de respuestas](assets/response-mapping.png)

Una asignación de respuesta empareja estos valores:

- &quot;Atributo de respuesta&quot;: la ruta a la propiedad deseada en el documento XML o JSON.
- &quot;Campo de Marketo&quot;: Campo de posible cliente en el que Marketo escribe el valor del atributo de respuesta.

Para acceder a una propiedad a través de las asignaciones de respuesta de Marketo, su clave debe contener solo caracteres alfanuméricos, guiones (-), guiones bajos (_), dos puntos (:) y espacios en blanco.

## Asignaciones de JSON

Acceda a las propiedades JSON con notación de puntos y notación de matrices. La notación de matrices de Marketo solo acepta números enteros, no cadenas.

Para recuperar datos de un documento JSON, establezca el tipo de respuesta en JSON:

```json
{ "foo":"bar"}
```

La propiedad `foo` se encuentra en el primer nivel del objeto JSON. Utilice su propiedad `name`, `foo`, en la asignación de respuestas:

![Asignación de respuestas](assets/json-resp.png)

El siguiente ejemplo contiene una matriz:

```json
{
    "profileId" : 1234,
    "firstName" : "Jane",
    "lastName" : "Doe",
    "orders" : [
        {
            "orderId" : 5678,
            "orderDate" : "2015-01-01",
            "orderProductId" : "4982"
        },
        {
            "orderId" : 5678,
            "orderDate" : "2014-05-07",
            "orderProductId" : "4982"
        }
    ]
}
```

Para obtener acceso a orderDate desde el primer elemento de la matriz de pedidos, utilice `orders[0].orderDate`.

## Asignaciones XML

Acceda a valores de elementos XML individuales utilizando notación de puntos, similar a las asignaciones JSON. Consideremos este ejemplo:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<example>
    <foo>bar</foo>
</example>
```

Para tener acceso a la propiedad foo, use `example.foo`.

Haga referencia al elemento de ejemplo antes de obtener acceso a `foo`. Una asignación debe hacer referencia a todos los elementos de la jerarquía de propiedades.

Para un documento XML con una matriz, tenga en cuenta este ejemplo:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<elementList>
    <element>
        <foo>baz</foo>
    </element>
    <element>
        <foo>bar</foo>
    </element>
    <element>
        <foo>bar</foo>
    </element>
</elementList>
```

La matriz principal es `elementList`. Cada elemento secundario contiene la propiedad `foo`. Las asignaciones de respuesta de Marketo hacen referencia a la matriz como `elementList.element` y tienen acceso a sus elementos secundarios a través de `elementList.element[i]`.

Para obtener el valor de foo del primer elemento secundario de elementList, use el atributo de respuesta `elementList.element[0].foo`. Esta asignación devuelve el valor &quot;baz&quot; al campo designado.

El acceso a propiedades dentro de elementos que contienen nombres de elemento únicos y no únicos produce un comportamiento indefinido. Cada elemento debe ser una propiedad única o una matriz. No mezcle los tipos.

## Tipos

Al asignar atributos a campos, asegúrese de que el tipo de respuesta del gancho web sea compatible con el campo de destino. Por ejemplo, Marketo no escribe un valor de respuesta de cadena en un campo de tipo entero. Para obtener más información, vea [Tipos de campo](../rest-api/field-types.md).
