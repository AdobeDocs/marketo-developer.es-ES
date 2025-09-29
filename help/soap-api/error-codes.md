---
title: Códigos de error
feature: SOAP
description: Guía de referencia sobre los códigos de error de API de Marketo SOAP con mensajes y notas, que cubre errores de autenticación, límites de tasa y concurrencia y problemas de solicitud.
exl-id: 71796520-7bd6-4a37-94e7-b073d17df06f
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '407'
ht-degree: 10%

---

# Códigos de error

Al desarrollar para Marketo, es muy importante que las solicitudes y respuestas se registren cuando se encuentra una excepción inesperada.  Aunque algunos tipos de excepciones, como la autenticación caducada, se pueden controlar de forma segura mediante la reautenticación, otros pueden requerir interacciones de soporte, y las solicitudes y respuestas siempre se solicitarán en este escenario.

A continuación se muestra una lista de códigos de error de API de SOAP.

| Código | Mensaje | Notas |
|--- |--- |--- |
| 10001 | Error interno | Fallo grave del sistema |
| 20011 | Error interno | Error del servicio API |
| 20012 | Solicitud no entendida | Mensaje de SOAP inesperado |
| 20013 | Acceso denegado | El cliente está bloqueado para acceder a la API |
| 20014 | Error de autenticación | El cliente no ha proporcionado credenciales válidas |
| 20015 | Límite de solicitudes superado | El número de llamadas hoy ha superado la cuota de la suscripción. La cuota de suscripción predeterminada es de 10 000 al día. |
| 20016 | Solicitud caducada | La firma de la solicitud es demasiado antigua. La marca de tiempo y la firma de solicitud proporcionadas se encuentran en el pasado y ya no son válidas. La solicitud se puede volver a intentar con una marca de tiempo y una firma recién generadas. |
| 20017 | Solicitud no válida | A la solicitud le falta un parámetro esperado |
| 20019 | Operación no admitida | La operación invocada no está definida en el WSDL de la API de Marketo |
| 20022 | El intervalo de tiempo especificado en el filtro de consulta ha superado el límite | El número de días transcurridos entre los campos más antiguosUpdatedAt y más recienteUpdatedAt era superior a 30 |
| 20023 | Límite de velocidad superado | El número de llamadas en los últimos 20 segundos fue superior a 100 |
| 20024 | Límite de concurrencia superado | El número de llamadas simultáneas fue superior a 10 |
| 20101 | Clave de posible cliente requerida | LeadKey es obligatorio, pero no se ha proporcionado |
| 20102 | Clave de posible cliente incorrecta | LeadKeyType no es válido |
| 20103 | Posible cliente no encontrado | El valor de LeadKey no coincide con ningún posible cliente |
| 20104 | Detalle del posible cliente obligatorio | Se requiere LeadRecord, pero no se ha proporcionado |
| 20105 | Atributo de posible cliente incorrecto | LeadRecord contiene un atributo con un nombre incorrecto |
| 20106 | Error de sincronización de posibles clientes | No se pudo actualizar o crear el LeadRecord |
| 20107 | Clave de actividad incorrecta | LeadActivityFilter contiene un tipo de actividad incorrecto |
| 20108 | Propietario de posible cliente no encontrado | LeadKey especifica un propietario de posible cliente que no existe |
| 20109 | Parámetro requerido | Falta el valor del parámetro o es nulo |
| 20110 | Parámetro incorrecto | Un valor de parámetro es incorrecto |
| 20111 | No se encontró la lista | ListKey especifica una lista que no existe |
| 20113 | Campaña no encontrada | La campaña no existe |
| 20114 | Parámetro incorrecto | El valor del parámetro es incorrecto |
| 20122 | Posición de flujo incorrecta | La posición del flujo es incorrecta |
| 20123 | Transmitir al final | La posición del flujo indica que no hay más registros disponibles |
