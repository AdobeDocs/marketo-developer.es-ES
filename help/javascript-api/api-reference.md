---
title: Referencia de API de Munchkin
description: Utilice la API de JavaScript de Munchkin para rastrear visitas a la página, clics en vínculos y eventos personalizados con los métodos init, createTrackingCookie y munchkinFunction.
feature: Munchkin Tracking Code, Javascript
exl-id: e9727691-5501-4223-bc98-2b4bacc33513
TQID: https://experienceleague.adobe.com/s97x6wVZijnnxZwS7HMIkQAKlxXkcfPXuSZG4KjXGoc
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 414
ht-degree: 9%

---

# Referencia de API de Munchkin

Munchkin proporciona funciones de JavaScript para un seguimiento personalizado de los eventos del explorador. Por ejemplo, puede realizar un seguimiento de reproducciones de vídeo o clics en elementos que no son vínculos.

## Funciones

La API de Munchkin incluye las siguientes funciones:

- `init`
- `createTrackingCookie`
- `munchkinFunction`

<a name="munchkin_init"></a>

### Munchkin.init()

Se debe llamar a `Munchkin.init()` antes que a cualquier otra función. Configura Munchkin en la página actual para enviar actividades a una instancia específica y genera una actividad &quot;Visitas a página web&quot; para la página actual.

| Nombre del parámetro | Opcional/Requerida | Tipo | Descripción |
| --- | --- | --- | --- |
| Identificación de Munchkin | Obligatorio | Cadena | El ID de cuenta de Munchkin se encuentra en el menú Administración > Integración > Munchkin. Establece la instancia de destino a la que se enviarán las actividades. |
| [Ajustes de configuración](configuration.md) | Opcional | Objeto | Habilita la configuración de comportamiento alternativa para Munchkin. |

```javascript
Munchkin.init('299-BYM-827');
```

### Munchkin.createTrackingCookie()

`Munchkin.createTrackingCookie()` comprueba si existe una cookie `_mkto_trk` en el explorador. Si la cookie no existe, la función crea una.

Cuando `cookieAnon` está establecido en falso, utilice esta función para rastrear a los usuarios durante acciones específicas, como registrar o descargar un recurso.

| Nombre del parámetro | Opcional/Requerida | Tipo | Descripción |
| --- | --- | --- | --- |
| forceCreate | Obligatorio | Booleano | Crear cookie aunque `cookieAnon` esté establecido en falso. |

```javascript
Munchkin.createTrackingCookie(true);
```

### Munchkin.munchkinFunction()

Use `Munchkin.munchkinFunction()` para crear comportamientos de seguimiento personalizados. Por ejemplo, rastrea la actividad del reproductor de vídeo o las visitas a la página desde la navegación no estándar, como los cambios de hash.

| Nombre del parámetro | Opcional/Requerida | Tipo | Descripción |
| --- | --- | --- | --- |
| Tipo de función | Obligatorio | Cadena | Determina la actividad que se va a registrar. Valores permitidos: `visitWebPage`, `clickLink`, `associateLead` |
| Datos | Obligatorio | Objeto | Contiene datos de la actividad que se va a registrar. |

#### visitWebPage

Llamar a `munchkinFunction()` con `visitWebPage` envía una actividad de &quot;visita&quot; para el usuario actual a Marketo. Utilice el objeto de datos en el segundo argumento para personalizar la dirección URL y `querystring`.

| Nombre de propiedad de datos | Opcional/Requerida | Tipo | Descripción |
| --- | --- | --- | --- |
| url | Obligatorio | Cadena | Ruta de archivo URL utilizada para registrar una visita a la página.  Este valor se anexa al nombre de dominio actual para crear un nombre de página completo. Por ejemplo, si la dirección URL es `/index.html` y el nombre de dominio es `www.example.com`, la página visitada se registra como `www.example.com/index.html`. |
| params | Opcional | Cadena | Una cadena de consulta de los parámetros deseados que se van a registrar. |

Por ejemplo, `foo=bar&biz=baz`.

```javascript
Munchkin.munchkinFunction('visitWebPage', {
        'url': '/Football/Team/Seahawks',
        'params': 'defense=legion_of_boom&qb=wilson'
    }
);
```

#### clickLink

Llamar a `munchkinFunction()` con `clickLink` envía una actividad de clic para el usuario actual a Marketo. Utilice la propiedad `href` en el objeto de datos para personalizar la dirección URL del clic.

| Nombre de propiedad de datos | Opcional/Requerida | Tipo | Descripción |
| --- | --- | --- | --- |
| href | Obligatorio | Cadena | Ruta de archivo URL utilizada para registrar un clic en vínculo. Este valor se anexa al nombre de dominio actual para crear el vínculo completo. |

Por ejemplo, si href es `/index.html` y el nombre de dominio es `www.example.com`, el clic en el vínculo se registra como `www.example.com/index.html`.

```javascript
Munchkin.munchkinFunction('clickLink', {
        'href': '/Football/Team/Seahawks'
    }
);
```

#### associatedLead

Este método se ha desaprobado y ya no está disponible para su uso.
