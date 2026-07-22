---
title: Tipos de campo
feature: REST API
description: Lista completa de tipos de campos de Marketo con definiciones, ejemplos y formatos, incluida la fecha y hora en formato ISO 8601, los límites de área de texto, la moneda y el valor booleano.
exl-id: a0ba9e02-ed42-4be3-9cdd-a97fee9a726e
TQID: https://experienceleague.adobe.com/Q-L1NCCS1caYip-niSrBAkp6k37ErzmsLCFvn7fRJW0
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: a7170d27-32ab-462b-a333-269abc654483
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
  - id: d1d0a9cd-295d-4976-8c39-ddae266f240e
subfeature_v2:
  - id: ad89fb33-8541-4339-afe7-bb13d1633714
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 371
ht-degree: 8%

---

# Tipos de campo

En la tabla siguiente se describen los tipos de campo disponibles en Marketo. Para obtener más información, vea el [glosario de tipo de campo personalizado](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/administration/field-management/custom-field-type-glossary) y [Límites de campo de Marketo por tipo de campo](https://nation.marketo.com/t5/knowledgebase/marketo-field-limits-by-field-type/ta-p/251613).

| Tipo de campo | Descripción | Ejemplo |
| --- | --- | --- |
| Fecha y hora | Se utiliza para introducir una fecha y una hora. Sigue el [formato W3C](https://www.w3.org/TR/NOTE-datetime) (ISO 8601). La práctica recomendada es incluir el desplazamiento de la zona horaria. Fecha de finalización más horas y minutos: AAAA-MM-DDThh:mm:ssTZD donde TZD es &quot;+hh:mm&quot; o &quot;-hh:mm&quot; Nota: Algunas API de recursos devuelven &quot;Z+0000&quot; como TZD para `updatedAt` y `createdAt`. | 2010-05-07T15:41:32-05:00 |
| Correo electrónico | Campo de cadena que acepta direcciones de correo electrónico | <example@example.com> |
| Flotante | Un campo numérico que contiene números reales y puede utilizar un decimal. | 10,4 |
| Entero | Números enteros | 10 |
| Fórmula | Campos cuyos valores se generan manipulando los datos de otros campos presentes en un registro de posibles clientes. No se exportan y no se pueden utilizar en campañas inteligentes. | Ver este [artículo](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/administration/field-management/create-and-use-a-concatenated-string-formula-field) |
| Porcentaje | Un porcentaje expresado como un entero | 30 |
| URL | Campo de texto que restringe la entrada a direcciones URL, incluido el protocolo de la dirección URL. | <http://example.com/> |
| Teléfono | Número de teléfono | 111-111-1111 |
| Área de texto | Texto más largo. | Admite hasta 30 000 bytes. Los caracteres ASCII estándar utilizan 1 byte por carácter (lo que permite hasta 30.000 caracteres). Los caracteres Unicode pueden utilizar hasta 4 bytes por carácter (lo que reduce el número de caracteres permitidos a menos de 30 000). |
| Cadena | Texto más corto | Texto de hasta 255 caracteres |
| Puntuación | Un campo entero que se puede manipular con el paso de flujo Cambiar puntuación | 10 |
| Booleano (anteriormente Casilla de verificación) | Permite a los usuarios seleccionar un valor Verdadero (marcado) o Falso (desmarcado). | True |
| Moneda | Campo flotante que representa el tipo de divisa predeterminado seleccionado para la suscripción a Marketo | 10,40 |
| Fecha | Se utiliza para la fecha. Sigue el formato W3C. | 2010-05-07 |
| Referencia | Campo de cadena que contiene una clave para otro registro (una clave externa). | Compañía del contacto |
