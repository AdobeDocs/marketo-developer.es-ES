---
title: Referencia de API de Munchkin
description: Utilice la API de JavaScript de Munchkin para rastrear visitas a la página, clics en vínculos y eventos personalizados con los métodos init, createTrackingCookie y munchkinFunction.
feature: Munchkin Tracking Code, Javascript
exl-id: e9727691-5501-4223-bc98-2b4bacc33513
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '424'
ht-degree: 9%

---

# Referencia de API de Munchkin

Munchkin proporciona varias funciones a las que se puede llamar manualmente mediante JavaScript. Esto puede permitir un seguimiento personalizado de los eventos del explorador, como reproducciones de vídeo o clics en elementos que no son vínculos.

## Funciones

La API de Munchkin consta de las siguientes funciones: `init`, `createTrackingCookie`, `munchkinFunction`.

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

Cuando se llama a, se comprueba que existe una cookie `_mkto_trk` en el explorador y, si no es así, se crea una. Esto resulta útil para rastrear usuarios durante acciones específicas, como el registro o la descarga de un recurso, si `cookieAnon` está establecido en falso.

| Nombre del parámetro | Opcional/Requerida | Tipo | Descripción |
| --- | --- | --- | --- |
| forceCreate | Obligatorio | Booleano | Crear cookie aunque `cookieAnon` esté establecido en falso. |

```javascript
Munchkin.createTrackingCookie(true);
```

### Munchkin.munchkinFunction()

Se utiliza para generar comportamientos de seguimiento personalizados, como reproducciones y pausas del reproductor de vídeo, o visitas a la página para navegación no estándar, como códigos hash.

| Nombre del parámetro | Opcional/Requerida | Tipo | Descripción |
| --- | --- | --- | --- |
| Tipo de función | Obligatorio | Cadena | Determina la actividad que se va a registrar. Valores permitidos: `visitWebPage`, `clickLink`, `associateLead` |
| Datos | Obligatorio | Objeto | Contiene datos de la actividad que se va a registrar. |

#### visitWebPage

Llamar a `munchkinFunction()` con `visitWebPage` envía una actividad de &quot;visita&quot; para el usuario actual a Marketo. Puede personalizar la dirección URL y `querystring` que se envían con el objeto de datos en el segundo argumento.

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

Llamar a `munchkinFunction()` con `clickLink` envía una actividad de clic para el usuario actual a Marketo. Puede personalizar la dirección URL de clic con la propiedad `href` en el objeto de datos.

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
