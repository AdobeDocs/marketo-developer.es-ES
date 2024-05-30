---
title: "Reglas de redireccionamiento de páginas de aterrizaje"
feature: REST API, Landing Pages
description: '"Configure las reglas de redireccionamiento de páginas de aterrizaje a través de la API".'
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '641'
ht-degree: 3%

---


# Reglas de redireccionamiento de la página de destino

[Referencia de extremo de reglas de redireccionamiento de página de aterrizaje](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules)

Marketo ofrece un conjunto de API de REST para realizar operaciones de CRUD en direcciones URL de redireccionamiento de páginas de aterrizaje. Estas API siguen el patrón de interfaz estándar para las API de recursos que proporcionan las opciones de Consulta, Crear, Actualizar y Eliminar.

Las reglas de redireccionamiento de páginas de aterrizaje permiten redirigir la dirección URL de una página de aterrizaje a otra dirección URL de página. Puede redirigir páginas de aterrizaje de Marketo, páginas de aterrizaje que no sean de Marketo o combinaciones de ellas. Puede encontrar información adicional sobre las reglas de la página de aterrizaje de redireccionamiento [aquí](https://experienceleague.adobe.com/docs/marketo/using/home.html?lang=es).

## Consulta

La consulta de las reglas de redirección de páginas de aterrizaje sigue los tipos de consulta estándar para los recursos de [por id](#by_id), y [exploración](#browse).

### Por ID

El [Obtener reglas de redireccionamiento de páginas de aterrizaje por identificador](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/getLandingPageRedirectRuleByIdUsingGET) el punto de conexión toma un único redireccionamiento de regla de página de aterrizaje `id` parámetro de ruta y devuelve un único registro de regla de redireccionamiento de página de aterrizaje.

```
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

El [Obtener reglas de redireccionamiento de página de aterrizaje](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/getLandingPageRedirectRulesUsingGET) el extremo devuelve una lista de registros de reglas de redirección de páginas de aterrizaje.

Existen varios parámetros de consulta opcionales que se pueden pasar a los resultados del filtro.

El `offset` El parámetro es un entero que especifica el número máximo de entradas que se devolverán (el valor predeterminado es 20). El máximo es 200. El `maxReturn` El parámetro es un entero que especifica por dónde empezar a recuperar entradas. Se puede utilizar junto con el desplazamiento (el valor predeterminado es 0).

El `hostname` El parámetro se puede utilizar para filtrar por nombre de host de las páginas de aterrizaje.

El `redirectToLandingPageId` es un entero que puede utilizarse para filtrar el ID de la página de aterrizaje a la que está redirigiendo. El `redirectToPath` se puede usar para filtrar la ruta de las páginas de aterrizaje a las que está redirigiendo.

El `earliestUpdatedAt` y `latestUpdatedAt` Los parámetros de le permiten establecer marcas de agua de fecha y hora bajas y altas para las reglas de redireccionamiento de páginas de aterrizaje de retorno que se actualizaron o crearon inicialmente dentro del intervalo dado.

```
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

El [Crear regla de redireccionamiento de página de aterrizaje](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/createLandingPageRedirectRuleUsingPOST) El extremo se ejecuta con un POST application/x-www-form-urlencoded que tiene los tres parámetros obligatorios siguientes.

El `hostname` parámetro especifica el nombre de host de la página de aterrizaje. Debe pertenecer a un alias o dominio de personalización de marca. La longitud máxima es de 255 caracteres.

El `redirectFrom` parámetro especifica la página de aterrizaje de origen. Es un objeto JSON que contiene un par tipo/valor que determina si el origen es una página de aterrizaje de Marketo o una página de aterrizaje que no sea de Marketo. El `type` El atributo puede ser &quot;landingPageId&quot; o &quot;path&quot;.

| Parámetro | Opcional/Requerida | Tipo | Descripción |
|---|---|---|---|
| &#39;get&#39; | Obligatorio | Cadena | Acción de método. |
| &#39;visitante&#39; | Obligatorio | Cadena | Nombre del método. |
| callback | Obligatorio | Función | Función de llamada de retorno que se activará para cada campaña devuelta. |


El `redirectTo` parámetro especifica la página de aterrizaje de destino. Es un objeto JSON que contiene un par tipo/valor que determina si el origen es una página de aterrizaje de Marketo o una página de aterrizaje que no sea de Marketo. El `type` el atributo puede ser &quot;landingPageId&quot; o &quot;url&quot;.

| Tipo de página de aterrizaje | tipo de redirectTo | Ejemplo |
|---|---|---|
| Marketo | landingPageId | {&quot;type&quot;:&quot;landingPageId&quot;,&quot;value&quot;:&quot;1774&quot;} |
| No es de Marketo | url | {&quot;type&quot;:&quot;url&quot;,&quot;value&quot;:&quot;www.contactLogs.com&quot;} |

Se puede encontrar más información sobre la creación de reglas de redireccionamiento de páginas de aterrizaje [aquí](https://experienceleague.adobe.com/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-actions/redirect-a-marketo-landing-page-to-another-page.html).

```
POST /rest/asset/v1/redirectRules.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

El [Actualizar reglas de redireccionamiento de página de aterrizaje](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/updateLandingPageRedirectRuleUsingPOST) el punto de conexión toma una única regla de redireccionamiento de página de aterrizaje `id` parámetro de ruta. Este extremo se ejecuta con un POST application/x-www-form-urlencoded.

Al igual que con la llamada de creación descrita anteriormente, se pasan uno o más de los siguientes parámetros de consulta para especificar qué atributo de la regla se va a actualizar: `hostname`, `redirectFrom`, `redirectTo`.

El registro actualizado de la regla de redirección de página de aterrizaje se devuelve en la respuesta.

```
POST /rest/asset/v1/redirectRule/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

El [Eliminar regla de redireccionamiento de página de aterrizaje por ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/deleteLandingPageRedirectRuleUsingPOST) el punto de conexión toma un único redireccionamiento de regla de página de aterrizaje `id` parámetro de ruta.

```
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

El [Obtener dominios de página de aterrizaje](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/getLandingPageDomainsUsingGET) el extremo devuelve una lista de registros de dominio de página de aterrizaje.

Existen dos parámetros de consulta opcionales que se pueden pasar a los resultados del filtro.

El `offset` El parámetro es un entero que especifica el número máximo de entradas que se devolverán (el valor predeterminado es 20, el máximo es 200).

El `maxReturn` El parámetro es un entero que especifica por dónde empezar a recuperar entradas. Se puede usar junto con `offset` (el valor predeterminado es 0).

```
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
