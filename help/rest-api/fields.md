---
title: Campos
feature: REST API, Field Management
description: Conozca la nomenclatura de los campos de posibles clientes de REST y SOAP, los campos de lista a través de REST. Describa el posible cliente, la asignación de funciones, por qué sfdcId es nulo y utilice sfdcLeadId o sfdcContactId.
exl-id: 9033f32a-c7cb-4bbf-abcf-38ca4112139f
TQID: https://experienceleague.adobe.com/H2Bvhy-67U8JJ1V3JwYJ0O0vj4i11fwUCyYQtjxm8u0
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: b0bb9048-d951-48d8-8232-45cf248a7e27id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 194
ht-degree: 6%

---

# Campos

La API de REST y la API de SOAP utilizan diferentes convenciones de nomenclatura para los campos de posibles clientes. Utilice la convención de nombre de campo requerida por cada función de integración.

## Recuperar la lista de nombres de campo

Utilice el extremo REST &quot;Describir posible cliente&quot; para recuperar todos los nombres de campo admitidos para registros de posibles clientes.

## ¿Dónde se utiliza qué tipo de nombre de campo?

El tipo de nombre de campo requerido depende de la función de integración. La siguiente tabla identifica si cada función utiliza nombres de campo REST o SOAP.

| Función | Tipo de nombre de campo que utilizar |
| --- | --- |
| API de seguimiento de posibles clientes (Munchkin) | SOAP |
| API de Forms 2.0 | SOAP |
| Importación de listas (IU) | SOAP |
| Importación de lista (API de REST) | REST |
| Asignaciones de respuesta de webhook | SOAP |
| Scripts de correo electrónico (Velocity) | SOAP |
| SOAP API | SOAP |
| API de REST | REST |

### ¿Por qué el campo de API de REST sfdcId siempre devuelve un valor nulo?

El campo `sfdcId` es un campo de fórmula que se incluyó en el mapa de campo original para la API de REST. Los registros recuperados a través de la API de REST no calculan los valores de los campos de fórmula, por lo que `sfdcId` siempre devuelve un valor nulo.

Para recuperar el SFDC ID, utilice los campos `sfdcLeadId` y `sfdcContactId`.
