---
title: "API DE SOAP"
feature: SOAP
description: "Información general de Marketo SOAP"
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '246'
ht-degree: 0%

---


# SOAP API

La API de SOAP ya no está en desarrollo activo. Las llamadas siguen funcionando, pero nuestro desarrollo se centra en [REST](https://developer.adobe.com/marketo-apis/) en adelante.

La API de SOAP de Marketo permite crear, recuperar y eliminar entidades y datos almacenados en Marketo. Puede encontrar el [Marketo-SOAP-SDK](https://github.com/Marketo/SOAP-API-Java-Client) en GitHub. También hay [bibliotecas de cliente](https://github.com/Marketo/Community-Supported-Client-Libraries) para ahorrarte algo de tiempo.

Última versión de API: 3_1

## WSDL DE SOAP

Para recuperar el documento WSDL de SOAP, obtenga su punto final de la API de SOAP de su **[!UICONTROL Administrador]** > **[!UICONTROL Integración]** > **[!UICONTROL Servicios web]** menú.

![Punto final de SOAP](assets/endpoint-soap.png)

La URL de WSDL es:

`<SOAP API Endpoint> + ?WSDL`

No utilice el punto final definido en el WSDL. Cada instancia de Marketo tiene un punto final único al que realizar llamadas.

## Límites

- **Cuota diaria:** A la mayoría de las suscripciones se les asignan 10 000 llamadas de API al día (que se restablecen todos los días a las 12:00 h CST). Puede aumentar su cuota diaria a través de su administrador de cuentas.
- **Límite de velocidad:** Acceso a la API por instancia limitado a 100 llamadas por 20 segundos.
- **Límite de concurrencia:**  Máximo de diez llamadas API simultáneas.

Nuestra recomendación es que los tamaños de lote no sean más grandes que 300. Los tamaños más grandes no son compatibles y pueden provocar tiempos de espera y, en casos extremos, restricciones.

## Configuración de la API de SOAP en Marketo

1. Vaya a la sección Admin y haga clic en Web Services.

![admin-web-services2](assets/admin-web-services2.png)

1. Establezca una clave de cifrado adecuada, haga clic en &quot;Guardar cambios&quot; y utilice los valores Punto final de API SOAP, ID de usuario y Clave de cifrado para generar la clave correcta [firma de autenticación](authentication-signature.md) para cada llamada a la API de SOAP.

![admin-web-services3](assets/admin-web-services3.png)
