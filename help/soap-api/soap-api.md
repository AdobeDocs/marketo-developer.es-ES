---
title: SOAP API
feature: SOAP
description: Información general sobre Marketo SOAP
exl-id: 6618cc82-15ae-4030-aa00-438e635d8369
source-git-commit: 6fc45ff98998217923e2a5b02d00d1522fe3272c
workflow-type: tm+mt
source-wordcount: '246'
ht-degree: 1%

---

# SOAP API

SOAP La API de ya no está en desarrollo activo. Las llamadas siguen funcionando, pero nuestro desarrollo se centra en [REST](https://developer.adobe.com/marketo-apis/) a partir de ahora.

La API de Marketo SOAP permite crear, recuperar y eliminar entidades y datos almacenados en Marketo. Encontrará el [SDK-Marketo SOAP-](https://github.com/Marketo/SOAP-API-Java-Client) en GitHub. También hay [bibliotecas de cliente](https://github.com/Marketo/Community-Supported-Client-Libraries) para ahorrarle tiempo.

Última versión de API: 3_1

## SOAP SDL

SOAP SOAP Para recuperar el documento de WSDL de la, obtenga su punto final de API de la en el menú **[!UICONTROL Administrador]** > **[!UICONTROL Integración]** > **[!UICONTROL Servicios web]**.

SOAP ![Punto final de](assets/endpoint-soap.png)

La URL de WSDL es:

`<SOAP API Endpoint> + ?WSDL`

No utilice el punto final definido en el WSDL. Cada instancia de Marketo tiene un punto final único al que realizar llamadas.

## Límites

- **Cuota diaria:** A la mayoría de las suscripciones se les asignan 10.000 llamadas API al día (que se restablecen diariamente a las 12:00 horas CST). Puede aumentar su cuota diaria a través de su administrador de cuentas.
- **Límite de velocidad:** Acceso a API por instancia limitado a 100 llamadas por 20 segundos.
- **Límite de simultaneidad:**  Máximo de diez llamadas API simultáneas.

Nuestra recomendación es que los tamaños de lote no sean más grandes que 300. Los tamaños más grandes no son compatibles y pueden provocar tiempos de espera y, en casos extremos, restricciones.

## SOAP Configuración de API de en Marketo

1. Vaya a la sección **[!UICONTROL Admin]** y haga clic en **[!UICONTROL Servicios web]**.

![admin-web-services2](assets/admin-web-services2.png)

1. SOAP SOAP Establezca una [!UICONTROL clave de cifrado] adecuada, haga clic en **[!UICONTROL Guardar cambios]** y use los valores de [!UICONTROL Extremo], [!UICONTROL Id. de usuario] y [!UICONTROL Clave de cifrado] de la API para generar la [firma de autenticación](authentication-signature.md) correcta para cada llamada a la API.

![admin-web-services3](assets/admin-web-services3.png)
