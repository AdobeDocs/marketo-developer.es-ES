---
title: Campos
feature: REST API, Field Management
description: Conozca la nomenclatura de los campos de posibles clientes de REST y SOAP, los campos de lista a través de REST. Describa el posible cliente, la asignación de funciones, por qué sfdcId es nulo y utilice sfdcLeadId o sfdcContactId.
exl-id: 9033f32a-c7cb-4bbf-abcf-38ca4112139f
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '212'
ht-degree: 6%

---

# Campos

La API de REST y la API de SOAP utilizan diferentes convenciones de nomenclatura para los campos de posibles clientes.

## Recuperar la lista de nombres de campo

Recupere la lista de todos los nombres de campo admitidos disponibles en los registros de posibles clientes mediante el punto final REST &quot;Describir posible cliente&quot;.

## ¿Dónde se utiliza qué tipo de nombre de campo?

A veces es difícil saber qué tipo de nombre de campo debe utilizar al aprovechar una función relacionada con la integración determinada. La siguiente es una referencia rápida para las que las funciones utilizan tipos de nombre de campo REST o SOAP.

| Función | Tipo de nombre de campo que utilizar |
|--- |--- |
| API de seguimiento de posibles clientes (Munchkin) | SOAP |
| API de Forms 2.0 | SOAP |
| Importación de listas (IU) | SOAP |
| Importación de lista (API de REST) | REST |
| Asignaciones de respuesta de webhook | SOAP |
| Scripts de correo electrónico (Velocity) | SOAP |
| SOAP API | SOAP |
| API de REST | REST |

### ¿Por qué el campo de API de REST sfdcId siempre devuelve un valor nulo?

El campo `sfdcId` es un campo de fórmula que se incluyó erróneamente en el mapa de campo original para la API de REST. Los registros recuperados mediante la API de REST no calculan el valor de los campos de fórmula, por lo que el valor siempre será nulo. Para capturar el ID de SFDC real, debe usar los campos llamados `sfdcLeadId` y `sfdcContactId`.
