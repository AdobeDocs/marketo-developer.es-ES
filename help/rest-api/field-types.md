---
title: Tipos de campo
feature: REST API
description: Una lista de tipos de campos de Marketo
exl-id: a0ba9e02-ed42-4be3-9cdd-a97fee9a726e
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '319'
ht-degree: 8%

---

# Tipos de campo

Esta es una descripción de los tipos de campo en Marketo. Encontrará información adicional sobre los tipos de campo [aquí](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/field-management/custom-field-type-glossary). Encontrará información adicional sobre los límites de tipo de campo [aquí](https://nation.marketo.com/t5/knowledgebase/tkb-p/support_solutions-documents).

| Tipo de campo | Descripción | Ejemplo |
| --- | --- | --- |
| Fecha y hora | Se utiliza para introducir una fecha y una hora. Sigue el [formato W3C](https://www.w3.org/TR/NOTE-datetime) (ISO 8601). Se recomienda incluir siempre el desplazamiento de zona horaria. Fecha de finalización más horas y minutos: AAAA-MM-DDThh:mm:ssTZD donde TZD es &quot;+hh:mm&quot; o &quot;-hh:mm&quot; Nota: Algunas API de recursos devuelven &quot;Z+0000&quot; como TZD para updatedAt y createdAt. | 2010-05-07T15:41:32-05:00 |
| Correo electrónico | Campo de cadena que acepta direcciones de correo electrónico | example@example.com |
| Flotante | Un campo numérico que contiene números reales y puede utilizar un decimal. | 10,4 |
| Entero | Números enteros | 10 |
| Fórmula | Campos cuyos valores se generan manipulando los datos de otros campos presentes en un registro de posibles clientes. No se exportan y no se pueden utilizar en campañas inteligentes. | Ver este [artículo](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/field-management/create-and-use-a-concatenated-string-formula-field) |
| Porcentaje | Un porcentaje expresado como un entero | 30 |
| URL | Campo de texto que restringe la entrada a direcciones URL, incluido el protocolo de la dirección URL. | http://example.com/ |
| Teléfono | Número de teléfono | 111-111-1111 |
| Área de texto | Texto más largo. | Admite hasta 30 000 bytes. Los caracteres ASCII estándar utilizan 1 byte por carácter (lo que permite hasta 30.000 caracteres). Los caracteres Unicode pueden utilizar hasta 4 bytes por carácter (lo que reduce el  número de caracteres permitido (inferior a 30 000 caracteres). |
| Cadena | Texto más corto (hasta 255 caracteres) | Lorem ipsum dolor sit amet |
| Puntaje | Un campo entero que se puede manipular con el paso de flujo Cambiar puntuación | 10 |
| Booleano (anteriormente Casilla de verificación) | Permite a los usuarios seleccionar un valor Verdadero (marcado) o Falso (desmarcado). | Verdadero |
| Moneda | Campo flotante que representa el tipo de divisa predeterminado seleccionado para la suscripción a Marketo | 10,40 |
| Fecha | Se utiliza para la fecha. Sigue el formato W3C. | 07-05-2010 |
| Referencia | Campo de cadena que contiene una clave para otro registro (una clave externa). | Compañía del contacto |
