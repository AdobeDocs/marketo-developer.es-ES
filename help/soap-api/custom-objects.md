---
title: "Objetos personalizados"
feature: SOAP
description: '"Creación de objetos personalizados".'
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 1%

---


# Objetos personalizados

Un objeto personalizado de Marketo permite crear una relación de uno a varios entre los posibles clientes de Marketo y los registros de objetos personalizados. Por ejemplo, es posible que desee rastrear todas las presentaciones a las que asistió un posible cliente. Dado que los posibles clientes pueden asistir a una serie de giras (a lo largo de varios años), los objetos personalizados son más adecuados para almacenar esta información.

Los objetos personalizados son compatibles con todas las ediciones de Marketo excepto con Spark. Cuando el objeto personalizado de Marketo se aprovisione correctamente en su cuenta, puede

- Crear/leer/actualizar/eliminar registros en el objeto personalizado mediante la API de SOAP de Marketo
- Utilice el déclencheur de listas inteligentes cuando se agreguen nuevos registros al objeto personalizado
- Usar los datos de objeto personalizados como filtro en listas inteligentes
- Uso de los datos de objeto personalizados en correos electrónicos con Marketo Email Scripting

## Estructura de los objetos personalizados

Los objetos personalizados constan de:

- Un pequeño conjunto de atributos fijos que son comunes a todos los objetos personalizados:
   - Nombre del objeto (también conocido como Nombre del tipo de objeto)
   - Descripción del objeto
   - Objeto personalizado a nombre de campo de vínculo de posible cliente de Marketo: es un campo del objeto de posible cliente (persona) al que hace referencia el objeto personalizado
   - Nombre del campo de clave de objeto: la clave principal utilizada por el objeto
- Uno o más campos específicos de objeto (se admite un máximo de 50 de estos campos)

## Operaciones de objeto personalizadas

Las siguientes llamadas se pueden utilizar para interactuar con CO.

- [getCustomObjects](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectsUsingGET)
- [syncCustomObjects](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST)
- [deleteCustomObjects](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/deleteCustomObjectsUsingPOST)
