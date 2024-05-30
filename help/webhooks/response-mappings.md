---
title: "Asignaciones de respuestas"
feature: Webhooks
description: "Asignaciones de respuestas para Marketo"
source-git-commit: bcc0c0c8e8209cf9fb962a85c8e7da354d95a8fe
workflow-type: tm+mt
source-wordcount: '461'
ht-degree: 0%

---


# Asignaciones de respuesta

Marketo puede traducir los datos recibidos por un webhook de dos tipos de contenido y devolver estos valores a un campo de posible cliente: JSON y XML. El parámetro Marketo Field siempre utiliza el [Nombre de API de SOAP](../rest-api/fields.md) del campo. Cada webhook puede tener un número ilimitado de asignaciones de respuestas, que se añaden y editan haciendo clic en [!UICONTROL Editar] en el panel Asignaciones de Respuesta de su webhook:

![Asignación de respuestas](assets/response-mapping.png)

Las asignaciones de respuesta se crean mediante un emparejamiento de un &quot;Atributo de respuesta&quot;, la ruta a la propiedad deseada en el documento XML o JSON y el &quot;Campo de Marketo&quot;, que especifica el campo de posible cliente que tiene el valor escrito en él desde el Atributo de respuesta.

Las claves de las propiedades deben constar de caracteres alfanuméricos, guiones (-), guiones bajos (_), dos puntos (:) y espacios en blanco a los que se accede mediante asignaciones de respuesta de Marketo.

## Asignaciones de JSON

Se accede a las propiedades JSON con notación de puntos y notación de matrices. La notación de matrices en Marketo no aceptará cadenas como entrada y solo aceptará enteros. Para recuperar datos de un documento JSON, el tipo de respuesta debe estar establecido en JSON:

```json
{ "foo":"bar"}
```

Para acceder a `foo` en una asignación de respuesta, utilice la propiedad `name` de la propiedad porque está en el primer nivel del objeto JSON, `foo`. Este es el aspecto que tiene Marketo:

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

Queremos acceder a orderDate desde el primer elemento de la matriz de pedidos. Para acceder a esta propiedad, utilice lo siguiente: `orders[0].orderDate`

## Asignaciones XML

Se puede acceder a los valores desde elementos individuales de documentos XML. Utiliza notación de puntos similar a las asignaciones JSON. Veamos este ejemplo sencillo:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<example>
    <foo>bar</foo>
</example>
```

Para acceder a la propiedad foo aquí, utilice lo siguiente: `example.foo`

Se debe hacer referencia primero al elemento de ejemplo antes de acceder a `foo`. Para acceder a una propiedad, se debe hacer referencia a todos los elementos de la jerarquía en la asignación. Los documentos XML con matrices son un poco más complicados. Utilice el siguiente ejemplo:

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

El documento consta de la matriz principal `elementList`, con elementos secundarios, elemento que contiene una propiedad: `foo`. Para los fines de las asignaciones de respuesta de Marketo, se hace referencia a la matriz como `elementList.element`, de modo que se accede a los elementos secundarios de elementList mediante `elementList.element[i]`. Para obtener el valor de foo del primer elemento secundario de elementList, se utiliza este atributo de respuesta: `elementList.element[0].foo` Esto devuelve el valor &quot;baz&quot; al campo designado. Al intentar acceder a las propiedades dentro de elementos que contienen nombres de elementos únicos y no únicos, se produce un comportamiento indefinido. Cada elemento debe ser una sola propiedad o una matriz, los tipos no se pueden mezclar.

## Tipos

Al asignar atributos a campos, debe asegurarse de que el tipo de respuesta del webhook sea compatible con el campo de destino. Por ejemplo, si el valor de la respuesta es una cadena y el campo seleccionado es del tipo entero, el valor no se escribe. Más información [Tipos de campo](../rest-api/field-types.md).
