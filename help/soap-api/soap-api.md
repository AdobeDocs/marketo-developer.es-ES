---
title: SOAP API
feature: SOAP
description: Información general sobre Marketo SOAP
exl-id: 6618cc82-15ae-4030-aa00-438e635d8369
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '244'
ht-degree: 1%

---

# SOAP API

La API de SOAP se está desaprobando y dejará de estar disponible a partir del 31 de octubre de 2025. Todo el nuevo desarrollo debe realizarse con la API de Marketo [REST](../rest-api/rest-api.md), y los servicios existentes deben migrarse para esa fecha a fin de evitar interrupciones en el servicio. Si tiene un servicio que usa la API de SOAP, consulte la [Guía de migración](./migration.md) de la API de SOAP para obtener información sobre cómo migrar.

## WSDL de SOAP

Para recuperar el documento WSDL de SOAP, obtenga su punto final de la API de SOAP en el menú **[!UICONTROL Administrador]** > **[!UICONTROL Integración]** > **[!UICONTROL Servicios web]**.

![Punto final de SOAP](assets/endpoint-soap.png)

La URL de WSDL es:

`<SOAP API Endpoint> + ?WSDL`

No utilice el punto final definido en el WSDL. Cada instancia de Marketo tiene un punto final único al que realizar llamadas.

## Límites

- **Cuota diaria:** A la mayoría de las suscripciones se les asignan 10 000 llamadas de API al día (que se restablece diariamente a 12:00AM CST). Puede aumentar su cuota diaria a través de su administrador de cuentas.
- **Límite de velocidad:** Acceso a API por instancia limitado a 100 llamadas por 20 segundos.
- **Límite de simultaneidad:**  Máximo de diez llamadas API simultáneas.

Nuestra recomendación es que los tamaños de lote no sean más grandes que 300. Los tamaños más grandes no son compatibles y pueden provocar tiempos de espera y, en casos extremos, restricciones.

## Configuración de la API de SOAP en Marketo

1. Vaya a la sección **[!UICONTROL Admin]** y haga clic en **[!UICONTROL Servicios web]**.

![admin-web-services2](assets/admin-web-services2.png)

1. Establezca una [!UICONTROL clave de cifrado] adecuada, haga clic en **[!UICONTROL Guardar cambios]** y use los valores de la API de SOAP [!UICONTROL Extremo], [!UICONTROL Id. de usuario] y [!UICONTROL Clave de cifrado] para generar la [firma de autenticación](authentication-signature.md) correcta para cada llamada a la API de SOAP.

![admin-web-services3](assets/admin-web-services3.png)
