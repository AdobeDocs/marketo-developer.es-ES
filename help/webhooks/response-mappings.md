---
title: Asignaciones de respuesta
feature: Webhooks
description: Asignaciones de respuestas de Webhooks de Marketo para JSON y XML, asignación de atributos a campos de posible cliente con nombres de API de SOAP, notación de puntos y matrices y compatibilidad de tipos.
exl-id: 95c6e33e-487c-464b-b920-3c67e248d84e
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 1%

---

# Asignaciones de respuesta

Marketo puede traducir los datos recibidos por un webhook de dos tipos de contenido y devolver estos valores a un campo de posible cliente: JSON y XML. El parámetro Marketo Field siempre usará [SOAP API name](../rest-api/fields.md) del campo. Cada webhook puede tener un número ilimitado de asignaciones de respuestas, las cuales se agregan y editan haciendo clic en el botón [!UICONTROL Editar] en el panel Asignaciones de respuestas de su webhook:

![Asignación de respuestas](assets/response-mapping.png)

Las asignaciones de respuesta se crean mediante un emparejamiento de un &quot;Atributo de respuesta&quot;, la ruta a la propiedad deseada en el documento XML o JSON y el &quot;Campo de Marketo&quot;, que especifica el campo de posible cliente que tiene el valor escrito en él desde el Atributo de respuesta.

Las claves de las propiedades deben constar de caracteres alfanuméricos, guiones (-), guiones bajos (_), dos puntos (:) y espacios en blanco a los que se accede mediante asignaciones de respuesta de Marketo.

## Asignaciones de JSON

Se accede a las propiedades JSON con notación de puntos y notación de matrices. La notación de matrices en Marketo no aceptará cadenas como entrada y solo aceptará enteros. Para recuperar datos de un documento JSON, el tipo de respuesta debe estar establecido en JSON:

```json
{ "foo":"bar"}
```

Para tener acceso a la propiedad `foo` en una asignación de respuesta, utilice `name` de la propiedad, ya que se encuentra en el primer nivel del objeto JSON, `foo`. Este es el aspecto que tiene Marketo:

![Asignación de respuestas](assets/json-resp.png)

Este es un ejemplo más complicado con una matriz:

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

Queremos acceder a orderDate desde el primer elemento de la matriz de pedidos. Para tener acceso a esta propiedad, use lo siguiente: `orders[0].orderDate`

## Asignaciones XML

Se puede acceder a los valores desde elementos individuales de documentos XML. Utiliza notación de puntos similar a las asignaciones JSON. Veamos este ejemplo sencillo:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<example>
    <foo>bar</foo>
</example>
```

Para tener acceso a la propiedad foo aquí, use lo siguiente: `example.foo`

Se debe hacer referencia al elemento de ejemplo antes de obtener acceso a `foo`. Para acceder a una propiedad, se debe hacer referencia a todos los elementos de la jerarquía en la asignación. Los documentos XML con matrices son un poco más complicados. Utilice el siguiente ejemplo:

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

El documento consta de la matriz principal `elementList`, con elementos secundarios, que contiene una propiedad: `foo`. Para los propósitos de las asignaciones de respuesta de Marketo, se hace referencia a la matriz como `elementList.element`, por lo que se tiene acceso a los elementos secundarios de elementList a través de `elementList.element[i]`. Para obtener el valor de foo del primer elemento secundario de elementList, se utiliza este atributo de respuesta: `elementList.element[0].foo` Esto devuelve el valor &quot;baz&quot; al campo designado. Al intentar acceder a las propiedades dentro de elementos que contienen nombres de elementos únicos y no únicos, se produce un comportamiento indefinido. Cada elemento debe ser una sola propiedad o una matriz, los tipos no se pueden mezclar.

## Tipos

Al asignar atributos a campos, debe asegurarse de que el tipo de respuesta del webhook sea compatible con el campo de destino. Por ejemplo, si el valor de la respuesta es una cadena y el campo seleccionado es del tipo entero, el valor no se escribe. Obtenga información sobre [tipos de campo](../rest-api/field-types.md).
