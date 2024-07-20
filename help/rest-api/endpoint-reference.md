---
title: Referencia de extremo
feature: REST API
description: Referencias de extremo de API de Marketo
exl-id: 27d16b6f-865a-4e40-ab9c-cbabe2927472
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 1%

---

# Referencia de extremo

- [Recurso](https://developer.adobe.com/marketo-apis/api/asset/)
- [Identidad](https://developer.adobe.com/marketo-apis/api/identity/)
- [Base de datos de clientes potenciales](https://developer.adobe.com/marketo-apis/api/mapi/)
- [Administración de usuarios](https://developer.adobe.com/marketo-apis/api/user/)

## Referencia de extremo

Marketo utiliza Swagger para proporcionar una definición formal de la interfaz pública para sus API de REST. Swagger proporciona un modelo de definición enriquecido para estructuras de URL, modelos de solicitud y modelos de respuesta, y tiene un ecosistema desarrollado de herramientas para su uso con interacción de API, pruebas y generación de clientes.

La referencia de extremo usa el paquete de JavaScript [Swagger-UI](https://swagger.io/tools/swagger-ui/) para generar las páginas de referencia del lado del cliente. Cada extremo público se enumera y proporciona la estructura del modelo de respuesta, los parámetros de solicitud necesarios y el modelo de solicitud si es necesario.

## Uso de definiciones de Swagger de Marketo

El estándar Swagger requiere que se proporcione un host o que el host que sirve el archivo genere el host. Es importante corregir el host en la definición antes de utilizar cualquier herramienta existente, ya que Marketo proporciona un parámetro de host vacío con la definición. El host de API de REST para cada instancia de Marketo es único y sigue el patrón:

`{Munchkin ID}.mktorest.com`

## Recurso API

Las API de recursos de Marketo utilizan `application/x-www-url-formencoded` parámetros de estilo en solicitudes de extremos que requieren un método POST. Sin embargo, en algunos casos, como los parámetros de carpeta, el valor del parámetro puede ser una matriz u objeto JSON. Estos parámetros se registran en la referencia del punto de conexión.
