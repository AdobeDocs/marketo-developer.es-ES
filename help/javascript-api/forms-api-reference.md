---
title: Referencia de API de Forms
description: Referencia de API de Forms
feature: Forms, Javascript
exl-id: 0f8d242f-0b27-4087-b080-3d41ebaa25b3
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '1327'
ht-degree: 2%

---

# Referencia de API de Forms

Existen dos objetos principales con los que interactuará mediante la API de Forms 2.0. Los objetos `MktoForms2` y `Form`. El objeto `MktoForms2` es el espacio de nombres visible públicamente de nivel superior para la funcionalidad de Forms2 y contiene funciones para crear, cargar y recuperar objetos de formulario.

## Métodos MktoForms2

<table>
  <tbody>
    <tr valign="top">
      <td><strong>Método</strong></td>
      <td><strong>Descripción</strong></td>
      <td><strong>Parámetros</strong></td>
      <td><strong>Devuelve</strong></td>
    </tr>
    <tr valign="top">
      <td>.loadForm(baseUrl, munchkinId, formId, callback)</td>
      <td>Carga un descriptor de formulario desde los servidores de Marketo y crea un nuevo objeto de formulario.</td>
      <td> baseUrl(String): URL de la instancia del servidor de Marketo para su suscripción</td>
      <td>no definido</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>munchkinId (String): ID de Munchkin de la suscripción</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>formId (Cadena o Número): el ID de versión del formulario (Vid) que se va a cargar.</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>callback (opcional) (función): una función de llamada de retorno para pasar el objeto de formulario construido a una vez que se ha cargado e inicializado.</td>
      <td></td>
    </tr>
    <tr valign="top">
      <td>.lightbox(formulario, opciones)</td>
      <td>Procesa un cuadro de diálogo modal de estilo light box con el objeto Form en él.</td>
      <td>form (objeto Form): instancia de un objeto Form que desea que se procese en un lightbox.</td>
      <td>Objeto lightbox con métodos .show() y .hide().</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>opts (optional)(Object): un objeto de opciones que se pasa al objeto lightbox</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>onSuccess(Función): una llamada de retorno que se activa cuando se envía el formulario.</td>
      <td></td>
    </tr>
    <tr>
          <td></td>
      <td></td>
      <td>closeBtn(Boolean) default true: controla si se muestra un botón de cierre (X) en el cuadro de diálogo de LightBox.</td>
      <td></td>
    </tr>
    <tr valign="top">
      <td>.newForm(formData, callback)</td>
      <td>Crea un nuevo objeto Form a partir de un objeto JS de descriptor de formulario. Agrega una función de llamada de retorno que se llama una vez que se han recuperado todas las hojas de estilo e información de posibles clientes conocida y se ha creado el objeto Form.</td>
      <td>formData (objeto descriptor de formulario): un objeto descriptor de formulario, creado por el Editor de Forms V2 de Marketo</td>
      <td>no definido</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>callback (opcional)(Función): a esta llamada de retorno se le llama con un solo argumento, una instancia recién creada del objeto Form.</td>
      <td></td>
    </tr>
    <tr valign="top">
      <td>.getForm(formId)</td>
      <td>Obtiene un objeto Form creado anteriormente por el identificador de formulario</td>
      <td> formId (número o cadena): Identificador de Vid de formulario.</td>
      <td>Objeto Form</td>
    </tr>
    <tr valign="top">
      <td>.allForms()</td>
      <td>Obtiene una matriz de todos los objetos de formulario que se han construido anteriormente en la página.</td>
      <td>n/a</td>
      <td>Matriz del objeto de formulario</td>
    </tr>
    <tr valign="top">
      <td>.getPageFields()</td>
      <td>Obtiene un objeto JS que contiene datos de la dirección URL y del referente que pueden resultar interesantes para el seguimiento.</td>
      <td>n/a</td>
      <td>Objeto</td>
    </tr>
    <tr valign="top">
      <td>.whenReady(callback)</td>
      <td>Agrega una llamada de retorno que se llama exactamente una vez para cada formulario de la página que esté "listo". La preparación significa que el formulario existe, se ha procesado inicialmente y se han llamado sus llamadas de retorno iniciales. Si ya hay un formulario listo en el momento de llamar a esta función, la llamada de retorno pasada se llama inmediatamente.</td>
      <td>callback(Función): la llamada de retorno se pasa a un solo argumento, un objeto de formulario.</td>
      <td>Objeto MktoForms2</td>
    </tr>
    <tr valign="top">
      <td>.onFormRender(callback)</td>
      <td>Agrega una llamada de retorno que se llama cada vez que se procesa cualquier formulario de la página. Forms se procesa cuando se crea inicialmente y, a continuación, cada vez que las reglas de visibilidad modifican la estructura del formulario.</td>
      <td>callback (Función): la llamada de retorno se pasa a un solo argumento, el objeto de formulario del formulario que se procesó.</td>
      <td>Objeto MktoForms2</td>
    </tr>
    <tr valign="top">
      <td>.whenRendered(callback)</td>
      <td>Al igual que onFormRender, agrega una llamada de retorno que se llama cada vez que se procesa un formulario. Además, esto también llama inmediatamente a la devolución de llamada para todos los formularios que ya se han procesado.</td>
      <td>callback(Función): la llamada de retorno se pasa a un solo argumento, el objeto de formulario del formulario procesado.</td>
      <td></td>
    </tr>
</table>


## Métodos de formulario

<table>
  <tbody>
    <tr valign="top">
      <td><strong>Método</strong></td>
      <td><strong>Descripción</strong></td>
      <td><strong>Parámetros</strong></td>
      <td><strong>Devuelve</strong></td>
    </tr>
    <tr valign="top">
      <td>.render(formElem)</td>
      <td>Representa un objeto de formulario, devolviendo un objeto jQuery que ajusta un elemento de formulario que contiene el formulario. Si se pasa un formElement, se utilizará como el elemento de formulario; de lo contrario, se creará uno nuevo.</td>
      <td>formElement (opcional): un elemento de formulario ajustado en un objeto jQuery en el que se procesará.</td>
      <td> Elemento de formulario ajustado a objetos jQuery que contiene el formulario procesado.</td>
    </tr>
    <tr valign="top">
      <td>.getId()</td>
      <td>Obtiene el id. del formulario.</td>
      <td>n/a</td>
      <td>Número: el ID del objeto de formulario que representa este formulario.</td>
    </tr>
    <tr valign="top">
      <td>.getFormElement()</td>
      <td>Obtiene el elemento de formulario jQuery ajustado de un formulario procesado.</td>
      <td>n/a</td>
      <td>Un elemento de formulario jQuery ajustado en objeto o nulo si el formulario aún no se ha representado con el método render().</td>
    </tr>
    <tr valign="top">
      <td>.validate()</td>
      <td>Fuerza la validación del formulario, resaltando los errores que puedan existir y devolviendo el resultado. No envía el formulario.</td>
      <td>n/a</td>
      <td>Booleano: devuelve el valor "True" si se han superado todos los validadores del formulario y el valor "False" en caso contrario.</td>
    </tr>
    <tr valign="top">
      <td>.onValidate(callback)</td>
      <td>Agrega una llamada de retorno de validación a la que se llamará cada vez que se active la validación.</td>
      <td>callback(Función): Se activará una llamada de retorno cada vez que se produzca la validación. La llamada de retorno se pasará a un parámetro, un booleano que indica si la validación se ha realizado correctamente.</td>
      <td>Objeto de formulario: el mismo objeto de formulario en el que se llamó al método con fines de encadenamiento.</td>
    </tr>
    <tr valign="top">
      <td>.submit()</td>
      <td>Déclencheur el evento de envío del formulario. Esto iniciará el flujo de envío desde, realizando la validación, activando cualquier evento de envío, enviando el formulario y activando cualquier evento de éxito si el envío del formulario se ha realizado correctamente.</td>
      <td>n/a</td>
      <td>Objeto de formulario: el mismo objeto de formulario en el que se llamó al método con fines de encadenamiento.</td>
    </tr>
    <tr valign="top">
      <td>.onSubmit(callback)</td>
      <td>Agrega una llamada de retorno que se llamará cuando se envíe el formulario. Esto se activa cuando comienza el envío, antes de que se conozca el éxito/error de la solicitud.</td>
      <td>callback: una función a la que se llamará cuando se envíe el formulario. Esta llamada de retorno pasará un argumento, este objeto Form.</td>
      <td>Objeto de formulario: el mismo objeto de formulario en el que se llamó al método con fines de encadenamiento.</td>
    </tr>
    <tr valign="top">
      <td>.onSuccess(callback)</td>
      <td>Agrega una llamada de retorno que se llamará cuando el formulario se haya enviado correctamente pero antes de que el posible cliente se reenvíe a la página de seguimiento. Se puede utilizar para evitar que el posible cliente se reenvíe a la página de seguimiento después del envío correcto.</td>
      <td>callback: una función a la que se llamará cuando el formulario se haya enviado correctamente. Esta llamada de retorno pasará con dos argumentos. Un objeto JS que contiene los valores enviados y una dirección URL de cadena de la página de seguimiento a la que se reenviará al usuario, o una cadena nula o vacía si no hay ninguna página de seguimiento configurada. Comportamiento especial: Si esta llamada de retorno devuelve "false" (medido con ===), el visitante NO se reenvía a la página de seguimiento y la página NO se vuelve a cargar. Esto permite al implementador realizar un procesamiento adicional de la URL de seguimiento o realizar acciones en la página mediante JavaScript en lugar de abandonar la página.</td>
      <td>Objeto de formulario: el mismo objeto de formulario en el que se llamó al método con fines de encadenamiento.</td>
    </tr>
    <tr valign="top">
      <td>.submittable(canSubmit) <em>también disponible como:</em> <em>.submitable(canSubmit)</em></td>
      <td>Obtiene o establece si el formulario se puede enviar. Si se le llama sin argumentos, obtiene el valor; si se le llama con un argumento, establece el valor. Esto se puede utilizar para evitar que se envíe un formulario, mientras que se deben cumplir otros criterios fuera del formulario normal.</td>
      <td>canSubmit (opcional)(booleano): establece el formulario como enviable o no enviable.</td>
      <td>Booleano u Objeto de formulario: si se llama sin argumentos, devuelve un booleano que indica si el formulario es enviable. Si se llama con un argumento, devuelve este objeto de formulario con fines de encadenamiento. </td>
    </tr>
    <tr valign="top">
      <td>.allFieldsFilled()</td>
      <td>Devuelve verdadero si todos los campos del formulario tienen valores establecidos que no estén en blanco.</td>
      <td>n/a</td>
      <td>Boolean: True si todos los campos tienen valores no vacíos/vacíos/no establecidos/nulos; false en caso contrario.</td>
    </tr>
    <tr valign="top">
      <td>.setValues(vals)</td>
      <td>Establece valores en uno o varios campos del formulario.</td>
      <td>vals: un objeto JS. Para cada par clave/valor del objeto, el campo de formulario denominado clave se establecerá en valor.</td>
      <td>no definido</td>
    </tr>
    <tr valign="top">
      <td>.getValues()</td>
      <td>Obtiene todos los valores de todos los campos del formulario.</td>
      <td>n/a</td>
      <td>Objeto: un objeto JS que contiene pares de clave/valor que representan los nombres y valores de los campos del formulario.</td>
    </tr>
    <tr valign="top">
      <td>.addHiddenFields(values)</td>
      <td>Agrega campos input type=hidden al formulario.</td>
      <td>values: un objeto JS que contiene pares de clave/valor que representan los nombres y valores de los campos ocultos que se agregan al formulario.</td>
      <td>no definido</td>
    </tr>
    <tr valign="top">
      <td>.vals(valores)</td>
      <td>jQuery style .vals() setter/getter. Si se llama a sin argumentos, equivale a llamar a getValues(). Si se llama con un argumento, equivale a llamar a setValues()</td>
      <td>values (optional) - Object</td>
      <td>no definido</td>
    </tr>
    <tr valign="top">
      <td>.showErrorMessage(msg, elem)</td>
      <td>Muestra un mensaje de error que apunta a elem.</td>
      <td>msg (String of HTML): cadena que contiene el texto del error que desea mostrar.</td>
            <td>Objeto Form: este objeto Form, para encadenar.</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>element (opcional)(objeto jQuery): el elemento al que apunta el error. Si no se configura, se utiliza el botón de envío del formulario.</td>
<td></td>
    </tr>
  </tbody>
</table>
