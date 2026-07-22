---
title: Reglas de redireccionamiento de la página de destino
feature: REST API, Landing Pages
description: Utilice las API de REST de Marketo Asset para crear, consultar, actualizar y eliminar reglas de redireccionamiento de páginas de aterrizaje con filtros, paginación, opciones de nombre de host y destinos que no sean de Marketo.
exl-id: f63aa5ef-5872-4401-be75-6fb9b2977734
TQID: https://experienceleague.adobe.com/2gePbKA3xeoRdnL8mNnObN-GPTX00Ii4-zcM0lBjs-o
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: f82558ea-6af5-44eb-a424-5b3389abb0a3
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 626
ht-degree: 4%

---

# Reglas de redireccionamiento de la página de destino

[Referencia de extremo de reglas de redireccionamiento de página de aterrizaje](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Redirect-Rules)

Utilice las API de REST de reglas de redireccionamiento de páginas de aterrizaje para consultar, crear, actualizar y eliminar direcciones URL de redireccionamiento de páginas de aterrizaje.

Las reglas de redireccionamiento envían una dirección URL de página de aterrizaje a otra dirección URL de página. El origen y el destino pueden ser páginas de Marketo o de otras marcas. Para obtener documentación del producto relacionada, consulte [Documentación de Marketo Engage](https://experienceleague.adobe.com/docs/marketo/using/home.html?lang=es).

## Consulta

Reglas de redirección de la página de aterrizaje de consulta [por identificador](#by_id) o por [exploración](#browse).

### Por ID

El punto de conexión [Obtener reglas de redireccionamiento de páginas de aterrizaje por id.](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Redirect-Rules/operation/getLandingPageRedirectRuleByIdUsingGET) toma un parámetro de ruta de acceso de regla de redireccionamiento `id` y devuelve el registro correspondiente.

```http
GET /rest/asset/v1/redirectRule/{id}.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "3d0#1707b2521e4",
    "warnings": [],
    "result": [
        {
            "id": 20,
            "redirectFromUrl": "https://calqeauto.com/DefDelPro1_LandingPage1.html",
            "hostname": "calqeauto.com",
            "redirectFrom": {
                "type": "landingPageId",
                "value": 5483
            },
            "redirectTo": {
                "type": "landingPageId",
                "value": 5559
            },
            "redirectToUrl": "https://calqeauto.com/DefDelPro1_LandingPage2.html",
            "createdAt": "2020-02-25T06:56:44Z+0000",
            "updatedAt": "2020-02-25T06:56:44Z+0000"
        }
    ]
}
```

### Examinar

El extremo [Obtener reglas de redireccionamiento de páginas de aterrizaje](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Redirect-Rules/operation/getLandingPageRedirectRulesUsingGET) devuelve registros de reglas de redireccionamiento de páginas de aterrizaje.

Utilice parámetros de consulta opcionales para filtrar los resultados.

El parámetro `offset` es un entero que especifica el número máximo de entradas que se van a devolver (el valor predeterminado es 20). El máximo es 200. El parámetro `maxReturn` es un entero que especifica dónde comenzar a recuperar entradas. Se puede utilizar junto con el desplazamiento (el valor predeterminado es 0).

El parámetro `hostname` filtra por nombre de host de página de aterrizaje.

El entero `redirectToLandingPageId` filtra por el identificador de la página de aterrizaje de destino. El parámetro `redirectToPath` filtra por la ruta de la página de aterrizaje de destino.

Los parámetros `earliestUpdatedAt` y `latestUpdatedAt` establecen los límites de fecha y hora bajo y alto. El extremo devuelve las reglas creadas o actualizadas dentro del intervalo.

```http
GET /rest/asset/v1/redirectRules.json&maxReturn=3
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "12213#1707b27efb5",
    "warnings": [],
    "result": [
        {
            "id": 5,
            "redirectFromUrl": "https://www.kirtideep.contact/LandingPage2.html",
            "hostname": "www.kirtideep.contact",
            "redirectFrom": {
                "type": "landingPageId",
                "value": 5406
            },
            "redirectTo": {
                "type": "landingPageId",
                "value": 5404
            },
            "redirectToUrl": "https://www.kirtideep.contact/www.showLogs.com.html",
            "createdAt": "2019-11-14T06:26:29Z+0000",
            "updatedAt": "2019-11-14T06:26:29Z+0000"
        },
        {
            "id": 6,
            "redirectFromUrl": "https://www.kirtideep.contact/www.showLogs.com.html",
            "hostname": "www.kirtideep.contact",
            "redirectFrom": {
                "type": "landingPageId",
                "value": 5404
            },
            "redirectTo": {
                "type": "url",
                "value": "www.contactLogs.com"
            },
            "redirectToUrl": "www.contactLogs.com",
            "createdAt": "2019-11-14T06:27:10Z+0000",
            "updatedAt": "2019-11-14T06:27:10Z+0000"
        },
        {
            "id": 7,
            "redirectFromUrl": "https://www.kirtideep.contact/contact/log/check",
            "hostname": "www.kirtideep.contact",
            "redirectFrom": {
                "type": "path",
                "value": "/contact/log/check"
            },
            "redirectTo": {
                "type": "landingPageId",
                "value": 5404
            },
            "redirectToUrl": "https://www.kirtideep.contact/www.showLogs.com.html",
            "createdAt": "2019-11-14T06:27:49Z+0000",
            "updatedAt": "2019-11-14T06:27:49Z+0000"
        }
    ]
}
```

## Crear

Llame al extremo [Crear regla de redireccionamiento de página de aterrizaje](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Redirect-Rules/operation/createLandingPageRedirectRuleUsingPOST) con una solicitud POST de `application/x-www-form-urlencoded`. La solicitud tiene tres parámetros obligatorios.

El parámetro `hostname` especifica el nombre de host de la página de aterrizaje. Debe pertenecer a un dominio o alias de personalización de marca y no puede superar los 255 caracteres.

El parámetro `redirectFrom` especifica la página de aterrizaje de origen como un objeto JSON con un par tipo/valor. El atributo `type` puede ser `landingPageId` para una página de aterrizaje de Marketo o `path` para una página que no sea de Marketo.

| Parámetro | Opcional/Requerida | Tipo | Descripción |
| --- | --- | --- | --- |
| &#39;get&#39; | Obligatorio | Cadena | Acción de método. |
| &#39;visitante&#39; | Obligatorio | Cadena | Nombre del método. |
| callback | Obligatorio | Función | Función de llamada de retorno que se activará para cada campaña devuelta. |

El parámetro `redirectTo` especifica el destino como un objeto JSON con un par tipo/valor. El atributo `type` puede ser `landingPageId` para una página de aterrizaje de Marketo o `url` para una página que no sea de Marketo.

| Tipo de página de aterrizaje | tipo de redirectTo | Ejemplo |
| --- | --- | --- |
| Marketo | landingPageId | {&quot;type&quot;:&quot;landingPageId&quot;,&quot;value&quot;:&quot;1774&quot;} |
| No es de Marketo | url | {&quot;type&quot;:&quot;url&quot;,&quot;value&quot;:&quot;www.contactLogs.com&quot;} |

Para obtener más información, consulte [Redireccionar una página de aterrizaje de Marketo a otra página](https://experienceleague.adobe.com/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-actions/redirect-a-marketo-landing-page-to-another-page.html?lang=es).

```http
POST /rest/asset/v1/redirectRules.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
hostname=calqeauto.com&redirectFrom={"type":"landingPageId", "value":"5483"}&redirectTo={"type":"landingPageId", "value":"5559"}
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "d7c6#1707b223522",
    "warnings": [],
    "result": [
        {
            "id": 20,
            "redirectFromUrl": "https://calqeauto.com/DefDelPro1_LandingPage1.html",
            "hostname": "calqeauto.com",
            "redirectFrom": {
                "type": "landingPageId",
                "value": 5483
            },
            "redirectTo": {
                "type": "landingPageId",
                "value": 5559
            },
            "redirectToUrl": "https://calqeauto.com/DefDelPro1_LandingPage2.html",
            "createdAt": "2020-02-25T06:56:44Z+0000",
            "updatedAt": "2020-02-25T06:56:44Z+0000"
        }
    ]
}
```

## Actualización

El extremo [Actualizar reglas de redireccionamiento de páginas de aterrizaje](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Redirect-Rules/operation/updateLandingPageRedirectRuleUsingPOST) toma un parámetro de ruta de acceso de regla de redireccionamiento `id`. Envíe la actualización como una solicitud POST de `application/x-www-form-urlencoded`.

Pase uno o más de estos parámetros para seleccionar los atributos que desea actualizar: `hostname`, `redirectFrom` o `redirectTo`.

La respuesta devuelve el registro de regla de redirección actualizado.

```http
POST /rest/asset/v1/redirectRule/{id}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
redirectTo={"type":"landingPageId", "value":"5561"}
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "57b2#1707b3852d7",
    "warnings": [],
    "result": [
        {
            "id": 20,
            "redirectFromUrl": "https://calqeauto.com/DefDelPro1_LandingPage1.html",
            "hostname": "calqeauto.com",
            "redirectFrom": {
                "type": "landingPageId",
                "value": 5483
            },
            "redirectTo": {
                "type": "landingPageId",
                "value": 5561
            },
            "redirectToUrl": "https://calqeauto.com/DefDelPro1_LandingPage3.html",
            "createdAt": "2020-02-25T06:56:44Z+0000",
            "updatedAt": "2020-02-25T07:20:53Z+0000"
        }
    ]
}
```

## Eliminar

La regla de redireccionamiento de página de aterrizaje [Delete por ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Redirect-Rules/operation/deleteLandingPageRedirectRuleUsingPOST) toma un parámetro de ruta de acceso de regla de redireccionamiento `id`.

```http
POST /rest/asset/v1/redirectRule/{id}/delete.json
```

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "d505#154d01c8364",
  "result": [
    {
      "id": 2
    }
  ]
}
```

## Explorar dominios de página de aterrizaje

El extremo [Obtener dominios de página de aterrizaje](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Redirect-Rules/operation/getLandingPageDomainsUsingGET) devuelve registros de dominio de página de aterrizaje.

Utilice dos parámetros de consulta opcionales para filtrar los resultados.

El parámetro `offset` es un entero que especifica el número máximo de entradas que se van a devolver (el valor predeterminado es 20, el máximo es 200).

El parámetro `maxReturn` es un entero que especifica dónde comenzar a recuperar entradas. Se puede usar junto con `offset` (el valor predeterminado es 0).

```http
POST /rest/asset/v1/landingPageDomains.json?maxReturn=3
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "6eb8#1707b43d3cb",
    "warnings": [],
    "result": [
        {
            "hostname": "calqeauto.com",
            "type": "domain"
        },
        {
            "hostname": "www.google.com",
            "type": "domain-alias"
        },
        {
            "hostname": "www.kirti.com",
            "type": "domain-alias"
        }
    ]
}
```
