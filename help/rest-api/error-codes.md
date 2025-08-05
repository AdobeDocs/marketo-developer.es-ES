---
title: Códigos de error
feature: REST API
description: Descripciones de código de error de Marketo.
exl-id: a923c4d6-2bbc-4cb7-be87-452f39b464b6
source-git-commit: 71e685efb25acffb5c1000293daaea4e936fc4f5
workflow-type: tm+mt
source-wordcount: '2320'
ht-degree: 3%

---

# Códigos de error

A continuación se muestran listas de códigos de error de API de REST y una explicación de cómo se devuelven los errores a las aplicaciones.

## Gestión y registro de excepciones

Al desarrollar para Marketo, es importante que las solicitudes y respuestas se registren cuando se encuentre una excepción inesperada. Aunque algunos tipos de excepciones, como la autenticación caducada, se pueden controlar de forma segura mediante la reautenticación, otros pueden requerir interacciones de soporte, y las solicitudes y respuestas siempre se solicitarán en este escenario.

## Tipos de error

La API de REST de Marketo puede devolver tres tipos diferentes de errores en funcionamiento normal:

* Nivel HTTP: estos errores se indican mediante un código `4xx`.
* Response-Level: estos errores se incluyen en la matriz &quot;errors&quot; de la respuesta JSON.
* Nivel de registro: estos errores se incluyen en la matriz &quot;result&quot; de la respuesta JSON y se indican en un registro individual con el campo &quot;estado&quot; y la matriz &quot;motivos&quot;.

Para los tipos de error de nivel de respuesta y nivel de registro, se devuelve un código de estado HTTP de 200. Para todos los tipos de error, la frase de motivo HTTP no debe evaluarse porque es opcional y está sujeta a cambios.

### Errores de nivel HTTP

En circunstancias normales de funcionamiento, Marketo solo debería devolver dos errores de código de estado HTTP, `413 Request Entity Too Large` y `414 Request URI Too Long`. Ambos se pueden recuperar detectando el error, modificando la solicitud y reintentando, pero con las prácticas de codificación inteligente, nunca debe encontrarlos en el comodín.

Marketo devolverá 413 si la carga de la solicitud supera 1 MB o 10 MB en el caso de la importación de posible cliente. En la mayoría de los casos, es poco probable que se alcancen estos límites, pero añadir una comprobación al tamaño de la solicitud y mover cualquier registro, lo que provoca que se supere el límite a una nueva solicitud, debe evitar cualquier circunstancia, que lleve a que este error sea devuelto por cualquier extremo.

414 se devolverá cuando el URI de una solicitud de GET exceda de 8 KB. Para evitarlo, compare la longitud de la cadena de consulta para ver si supera este límite. Si cambia la solicitud a un método POST, escriba la cadena de consulta como el cuerpo de la solicitud con el parámetro adicional `_method=GET`. Esto renuncia a la limitación de URI. Es raro alcanzar este límite en la mayoría de los casos, pero es algo común cuando se recuperan grandes lotes de registros con valores de filtro individuales largos, como un GUID.
El extremo [Identity](https://developer.adobe.com/marketo-apis/api/identity/) puede devolver un error 401 Unauthorized. Esto suele deberse a un ID de cliente no válido o a un secreto de cliente no válido. Códigos de error de nivel HTTP

<table>
  <thead>
    <tr>
      <th> Código de respuesta </th>
      <th> Descripción </th>
      <th colspan="1"> Comentario </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><a name="413"></a>413</td>
      <td>Entidad de solicitud demasiado grande</td>
      <td>La carga útil ha superado el límite de 1 MB.</td>
    </tr>
    <tr>
      <td><a name="414"></a>414</td>
      <td>URI de solicitud demasiado largo</td>
      <td>El URI de la solicitud ha superado los 8 K. La solicitud debe reintentarse como un POST con el parámetro `_method=GET` en la URL y el resto de la cadena de consulta en el cuerpo de la solicitud.</td>
    </tr>
  </tbody>
</table>

#### Errores de nivel de respuesta

Los errores de nivel de respuesta están presentes cuando el parámetro `success` de la respuesta se establece en false y están estructurados de la siguiente manera:

```json
{
    "requestId": "e42b#14272d07d78",
    "success": false,
    "errors": [
        {
            "code": "601",
            "message": "Unauthorized"
        }
    ]
}
```

Cada objeto de la matriz &quot;errors&quot; tiene dos miembros, `code`, que es un entero entre comillas comprendido entre 601 y 799 y un `message` que indica el motivo del error en texto sin formato. Los códigos 6xx siempre indican que una solicitud ha fallado completamente y no se ha ejecutado. Un ejemplo es un 601, &quot;Token de acceso no válido&quot;, que se puede recuperar volviendo a autenticar y pasando el nuevo token de acceso con la solicitud. Los errores 7xx indican que la solicitud falló, ya sea porque no se devolvieron datos o porque se parametrizó incorrectamente la solicitud, como incluir una fecha no válida o faltar un parámetro requerido.

#### Códigos de error de nivel de respuesta

Una llamada de API que devuelva este código de respuesta no se contará en su cuota diaria o en su límite de tarifa.

<table>
  <thead>
    <tr>
      <th> Código de respuesta </th>
      <th> Descripción </th>
      <th> Comentario </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><a name="502"></a>502</td>
      <td>Puerta de enlace incorrecta</td>
      <td>El servidor remoto devolvió un error. Probablemente un tiempo de espera. La solicitud debe reintentarse con un retroceso exponencial.</td>
    </tr>
    <tr>
      <td><a name="601"></a>601*</td>
      <td>Token de acceso no válido</td>
      <td>Se incluyó un parámetro de token de acceso en la solicitud, pero el valor no era un token de acceso válido.</td>
    </tr>
    <tr>
      <td><a name="602"></a>602*</td>
      <td>Token de acceso caducado</td>
      <td>El token de acceso incluido en la llamada ya no es válido debido a la caducidad.</td>
    </tr>
    <tr>
      <td><a name="603"></a>603</td>
      <td>Acceso denegado</td>
      <td>La autenticación se ha realizado correctamente, pero el usuario no tiene permisos suficientes para llamar a esta API. Es posible que se deban asignar [permisos adicionales](custom-services.md) a la función de usuario o que se habilite <a href="https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/additional-integrations/create-an-allowlist-for-ip-based-api-access">Lista de permitidos para el acceso a la API basada en IP</a>.</td>
    </tr>
    <tr>
      <td><a name="604"></a>604*</td>
      <td>Tiempo de espera de solicitud</td>
      <td>La solicitud se estaba ejecutando durante demasiado tiempo (por ejemplo, encontró contención en la base de datos) o excedió el período de tiempo de espera especificado en el encabezado de la llamada.</td>
    </tr>
    <tr>
      <td><a name="605"></a>605*</td>
      <td>Método HTTP no admitido</td>
      <td>GET no es compatible con el extremo de posibles clientes de sincronización. POST debe usarse.</td>
    </tr>
    <tr>
      <td><a name="606"></a>606</td>
      <td>Se ha superado el límite máximo de tasa "%s"; en "%s" segundos</td>
      <td>El número de llamadas en los últimos 20 segundos fue superior a 100</td>
    </tr>
    <tr>
      <td><a name="607"></a>607</td>
      <td>Cuota diaria alcanzada</td>
      <td>El número de llamadas hoy ha superado la cuota de la suscripción (se restablece diariamente a las 12:00 CST).&gt;Su cuota se encuentra en el menú Administrador-&gt;Servicios Web. Puede aumentar su cuota a través de su administrador de cuentas.</td>
    </tr>
    <tr>
      <td><a name="608"></a>608*</td>
      <td>API temporalmente no disponible</td>
      <td></td>
    </tr>
    <tr>
      <td><a name="609"></a>609</td>
      <td>JSON no válido</td>
      <td>El cuerpo incluido en la solicitud no es un archivo JSON válido.</td>
    </tr>
    <tr>
      <td><a name="610"></a>610</td>
      <td>No se ha encontrado el recurso solicitado</td>
      <td>El URI de la llamada no coincide con un tipo de recurso de API de REST. A menudo, esto se debe a que el URI de la solicitud está mal escrito o tiene un formato incorrecto</td>
    </tr>
    <tr>
      <td><a name="611"></a>611*</td>
      <td>Error del sistema</td>
      <td>Todas las excepciones no gestionadas</td>
    </tr>
    <tr>
      <td><a name="612"></a>612</td>
      <td>Tipo de contenido no válido</td>
      <td>Si ve este error, agregue un encabezado de tipo de contenido que especifique el formato JSON a su solicitud. Por ejemplo, intente utilizar "tipo de contenido: application/json". <a href="https://stackoverflow.com/questions/28181325/why-invalid-content-type">Consulte esta pregunta sobre StackOverflow</a> para obtener más información.</td>
    </tr>
    <tr>
      <td><a name="613"></a>613</td>
      <td>Solicitud multiparte no válida</td>
      <td>El contenido de varias partes de la PUBLICACIÓN no tenía el formato correcto</td>
    </tr>
    <tr>
      <td><a name="614"></a>614</td>
      <td>Suscripción no válida</td>
      <td>No se encuentra la suscripción de destino o no se puede acceder a ella. Esto suele indicar una inaccesibilidad temporal.</td>
    </tr>
    <tr>
      <td><a name="615"></a>615</td>
      <td>Límite de acceso simultáneo alcanzado</td>
      <td>Como máximo, las solicitudes se procesan mediante cualquier suscripción de 10 a la vez. Se devuelve si ya hay 10 solicitudes en curso.</td>
    </tr>
    <tr>
      <td><a name="616"></a>616</td>
      <td>Tipo de suscripción no válido</td>
      <td>Se requiere el tipo de suscripción de Marketo adecuado para acceder a la API de metadatos de objeto personalizado. Consulte con su CSM para obtener más información.</td>
    </tr>
    <tr>
      <td><a name="701"></a>701</td>
      <td>%s no puede estar en blanco</td>
      <td>El campo del informe no debe estar vacío en la solicitud</td>
    </tr>
    <tr>
      <td><a name="702"></a>702</td>
      <td>No se han encontrado datos para un escenario de búsqueda determinado</td>
      <td>Ningún registro coincide con los parámetros de búsqueda proporcionados.
        Nota: Muchas operaciones de búsqueda fallidas devuelven "success = true" y sin errores y establecen una cadena informativa de advertencias.</td>
    </tr>
    <tr>
      <td><a name="703"></a>703</td>
      <td>La función no está habilitada para la suscripción</td>
      <td>Característica beta que no se ha habilitado en la suscripción de un usuario</td>
    </tr>
    <tr>
      <td><a name="704"></a>704</td>
      <td>Formato de fecha no válido</td>
      <td><ul>
          <li>Se especificó una fecha que no tenía el formato correcto</li>
          <li>Se ha especificado un ID de contenido dinámico no válido</li>
        </ul></td>
    </tr>
    <tr>
      <td><a name="709"></a>709</td>
      <td>Infracción de regla de negocio</td>
      <td>No se puede cumplir la llamada porque infringe un requisito para crear o actualizar un recurso como, por ejemplo, intentar crear un mensaje de correo electrónico sin una plantilla. También es posible obtener este error al intentar lo siguiente:
        <ul>
          <li>Recupere contenido para páginas de aterrizaje que contengan contenido social.</li>
          <li>Clonar un programa que contiene ciertos tipos de recursos (consulte <a href="programs.md#clone">Clonar programa</a> para obtener más información).</li>
          <li>Aprobar un recurso que no tenga borrador (es decir, que ya se haya aprobado).</li>
        </ul></td>
    </tr>
    <tr>
      <td><a name="710"></a>710</td>
      <td>Carpeta principal no encontrada</td>
      <td>No se encontró la carpeta principal especificada</td>
    </tr>
    <tr>
      <td><a name="711"></a>711</td>
      <td>Tipo de carpeta incompatible</td>
      <td>La carpeta especificada no era del tipo correcto para cumplir la solicitud</td>
    </tr>
    <tr>
      <td><a name="712"></a>712</td>
      <td>La operación de cuenta de fusión a persona no es válida</td>
      <td>Error en una llamada de combinación de posibles clientes debido a un intento de combinar posibles clientes que son cuentas de persona de Salesforce.  Las cuentas personales de Salesforce deben fusionarse en Salesforce.</td>
    </tr>
    <tr>
      <td><a name="713"></a>713</td>
      <td>Error transitorio</td>
      <td>Un recurso del sistema no estaba disponible temporalmente en el momento de la llamada de API. Cuando se encuentra este error, se recomienda esperar un tiempo y luego volver a intentar la solicitud.</td>
    </tr>
    <tr>
      <td><a name="714"></a>714</td>
      <td>No se encuentra el tipo de registro predeterminado</td>
      <td>Error en una llamada de combinación de posibles clientes porque no se pudo encontrar un tipo de registro predeterminado.</td>
    </tr>
    <tr>
      <td><a name="718"></a>718</td>
      <td>No se encontró ExternalSalesPersonID</td>
      <td>Se realizó una llamada de oportunidades de sincronización con un valor ExternalSalesPersonID inexistente.</td>
    </tr>
    <tr>
      <td>719</td>
      <td>Excepción de tiempo de espera de bloqueo</td>
      <td>Se realizó una llamada del programa de clonación y se agotó el tiempo de espera esperando un bloqueo.</td>
    </tr>
  </tbody>
</table>

### Nivel de registro {#record_level_errors}

Los errores de nivel de registro indican que no se pudo completar una operación para un registro individual, pero la solicitud en sí era válida. Una respuesta con errores en el nivel de registro sigue este patrón:

#### Respuesta

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "id":50,
         "status":"created"
      },
      {
         "id":51,
         "status":"created"
      },
      {
         "status":"skipped",
         "reasons":[
            {
               "code":"1005",
               "message":"Lead already exists"
            }
         ]
      }
   ]
}
```

Los registros incluidos en la matriz de llamadas de resultado se ordenan del mismo modo que la matriz de entrada de una solicitud.
Cada registro de una solicitud correcta puede tener éxito o fallar de forma individual, lo que se indica mediante el campo de estado de cada registro incluido en la matriz de resultados de una respuesta. El campo &quot;estado&quot; de estos registros se &quot;omitirá&quot; y aparecerá una matriz &quot;motivos&quot;. Cada motivo contiene un miembro &quot;code&quot; y un miembro &quot;message&quot;. El código siempre es 1xxx y el mensaje indica por qué se omitió el registro. Un ejemplo sería cuando una solicitud de Sync Leads tiene &quot;action&quot; establecida en &quot;createOnly&quot; pero ya existe un posible cliente para una de las claves de los registros enviados. Este caso devuelve un código de 1005 y un mensaje de &quot;El posible cliente ya existe&quot; como se muestra arriba.

#### Códigos de error de nivel de registro

>[!NOTE]
>
><table>
><tbody>
>    <tr>
>      <td>Código de respuesta</td>
>      <td>Descripción</td>
>      <td>Comentario</td>
>    </tr>
>    <tr>
>      <td><a name="1001"></a>1001</td>
>      <td>Valor ‘%s’ no válido. Requerido de tipo ‘%s’</td>
>      <td>Se genera un error cada vez que un valor de parámetro tiene un tipo que no coincide. Por ejemplo, el valor de cadena especificado para un parámetro entero.</td>
>    </tr>
>    <tr>
>      <td><a name="1002"></a>1002</td>
>      <td>Falta un valor para el parámetro obligatorio ‘%s’</td>
>      <td>Se genera un error cuando falta un parámetro requerido en la solicitud</td>
>    </tr>
>    <tr>
>      <td><a name="1003"></a>1003</td>
>      <td>Datos no válidos</td>
>      <td>Cuando los datos enviados no son de un tipo válido para el punto de conexión o modo determinados; por ejemplo, cuando se envía el ID de un posible cliente con la acción designada como createOnly o cuando se utiliza Solicitar campaña en una campaña por lotes.</td>
>    </tr>
>    <tr>
>      <td><a name="1004"></a>1004</td>
>      <td>No se encontró el lead</td>
>      <td>Para syncLead, cuando la acción es "updateOnly" y si no se encuentra el posible cliente</td>
>    </tr>
>    <tr>
>      <td><a name="1005"></a>1005</td>
>      <td>El posible cliente ya existe</td>
>      <td>Para syncLead, cuando la acción es "createOnly" y si ya existe un posible cliente</td>
>    </tr>
>    <tr>
>      <td><a name="1006"></a>1006</td>
>      <td>No se encontró el campo ‘%s’</td>
>      <td>Un campo incluido en la llamada de no es un campo válido.</td>
>    </tr>
>    <tr>
>      <td><a name="1007"></a>1007</td>
>      <td>Varios posibles clientes coinciden con los criterios de búsqueda</td>
>      <td>Varios posibles clientes coinciden con los criterios de búsqueda. Las actualizaciones solo se pueden realizar cuando la clave coincida con un único registro</td>
>    </tr>
>    <tr>
>      <td><a name="1008"></a>1008</td>
>      <td>Acceso denegado a la partición ‘%s’</td>
>      <td>El usuario del servicio personalizado no tiene acceso a un espacio de trabajo con la partición donde existe el registro.</td>
>    </tr>
>    <tr>
>      <td><a name="1009"></a>1009</td>
>      <td>Se debe especificar el nombre de partición</td>
>      <td></td>
>    </tr>
>    <tr>
>      <td><a name="1010"></a>1010</td>
>      <td>Actualización de partición no permitida</td>
>      <td>El registro especificado ya existe en una partición de posibles clientes independiente.</td>
>    </tr>
>    <tr>
>      <td><a name="1011"></a>1011</td>
>      <td>No se admite el campo ‘%s’</td>
>      <td>Cuando se especifica un campo de búsqueda o filterType con campos estándar no admitidos (por ejemplo: firstName, lastName)</td>
>    </tr>
>    <tr>
>      <td><a name="1012"></a>1012</td>
>      <td>Valor de cookie '%s' no válido</td>
>      <td>Se puede producir al llamar al <a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/associateLeadUsingPOST">posible cliente asociado</a> con un valor no válido para el parámetro "cookie".
>        Esto también ocurre al llamar a <a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadsByFilterUsingGET">Obtener posibles clientes por tipo de filtro</a> con "filterType=cookies" y un valor no válido para el parámetro "filterValues".</td>
>    </tr>
>    <tr>
>      <td><a name="1013"></a>1013</td>
>      <td>Objeto no encontrado</td>
>      <td>Get object (list, campaign) by id devuelve este código de error</td>
>    </tr>
>    <tr>
>      <td><a name="1014"></a>1014</td>
>      <td>Error al crear el objeto</td>
>      <td>Error al crear el objeto (lista)</td>
>    </tr>
>    <tr>
>      <td><a name="1015"></a>1015</td>
>      <td>Posible cliente no en lista</td>
>      <td>El posible cliente designado no es miembro de la lista de destinatarios</td>
>    </tr>
>    <tr>
>      <td><a name="1016"></a>1016</td>
>      <td>Demasiadas importaciones</td>
>      <td>Hay demasiadas importaciones en cola. Se permite un máximo de 10</td>
>    </tr>
>    <tr>
>      <td><a name="1017"></a>1017</td>
>      <td>El objeto ya existe</td>
>      <td>Error de creación porque el registro ya existe</td>
>    </tr>
>    <tr>
>      <td><a name="1018"></a>1018</td>
>      <td>CRM habilitado</td>
>      <td>No se pudo realizar la acción porque la instancia tiene habilitada una integración nativa de CRM.</td>
>    </tr>
>    <tr>
>      <td><a name="1019"></a>1019</td>
>      <td>Importación en progreso</td>
>      <td>La lista de destinos ya se está importando en</td>
>    </tr>
>    <tr>
>      <td><a name="1020"></a>1020</td>
>      <td>Demasiados clones para programar</td>
>      <td>La suscripción ha alcanzado el uso asignado de cloneToProgramName en el programa programado para el día</td>
>    </tr>
>    <tr>
>      <td><a name="1021"></a>1021</td>
>      <td>Actualización de empresa no permitida</td>
>      <td>No se permite la actualización de empresa durante syncLead</td>
>    </tr>
>    <tr>
>      <td><a name="1022"></a>1022</td>
>      <td>Objeto en uso</td>
>      <td>No se permite eliminar cuando otro objeto está utilizando un objeto</td>
>    </tr>
>    <tr>
>      <td><a name="1025"></a>1025</td>
>      <td>Estado del programa no encontrado</td>
>      <td>Se ha especificado un estado para Cambiar el estado del programa de posibles clientes que no coincide con un estado disponible para el canal del programa.</td>
>    </tr>
>    <tr>
>      <td><a name="1026"></a>1026</td>
>      <td>Objeto personalizado no habilitado</td>
>      <td>No se pudo realizar la acción porque la instancia no tiene habilitada la integración de objetos personalizados.</td>
>    </tr>
>    <tr>
>      <td><a name="1027"></a>1027</td>
>      <td>Límite máximo de tipo de actividad alcanzado</td>
>      <td>La suscripción ha alcanzado el número máximo de tipos de actividades personalizadas disponibles.</td>
>    </tr>
>    <tr>
>      <td><a name="1028"></a>1028</td>
>      <td>Límite máximo de campos alcanzado</td>
>      <td>Las actividades personalizadas tienen un máximo de 20 atributos secundarios.</td>
>    </tr>
>    <tr>
>      <td><a name="1029"></a>1029</td>
>      <td><ul>
>          <li>Demasiados trabajos en cola</li>
>          <li>Exportación de cuota diaria excedida</li>
>          <li>Trabajo ya en cola</li>
>        </ul></td>
>      <td><ul>
>          <li>Las suscripciones pueden tener un máximo de 10 trabajos de extracción masiva en cola en un momento dado.</li>
>          <li>De forma predeterminada, los trabajos de extracción están limitados a 500 MB por día (se restablece diariamente a las 12:00 CST).</li>
>          <li>El ID de exportación ya se ha puesto en la cola.</li>
>        </ul></td>
>    </tr>
>    <tr>
>      <td><a name="1035"></a>1035</td>
>      <td>Tipo de filtro no compatible</td>
>      <td>En algunas suscripciones no se admiten los siguientes tipos de filtros de extracción masiva de posibles clientes: updatedAt, smartListId, smartListName.</td>
>    </tr>
>    <tr>
>      <td><a name="1036"></a>1036</td>
>      <td>Objeto duplicado encontrado en la entrada</td>
>      <td>Se realizó una llamada para actualizar dos o más registros con la misma clave externa. Por ejemplo, una llamada de empresas de sincronización que utiliza el mismo externalCompanyId para más de una empresa.</td>
>    </tr>
>    <tr>
>      <td><a name="1037"></a>1037</td>
>      <td>Se omitió el posible cliente</td>
>      <td>Se omitió el posible cliente porque ya está en este estado o más allá de él.</td>
>    </tr>
>    <tr>
>      <td><a name="1042"></a>1042</td>
>      <td>Fecha runAt no válida</td>
>      <td>La fecha runAt especificada para Schedule Campaign era demasiado lejana en el futuro (el máximo es de 2 años).</td>
>    </tr>
>    <tr>
>      <td><a name="1048"></a>1048</td>
>      <td>Error al descartar el borrador del objeto personalizado</td>
>      <td>Se realizó una llamada para descartar la versión de borrador de un objeto personalizado.</td>
>    </tr>
>    <tr>
>      <td><a name="1049"></a>1049</td>
>      <td>Error al crear la actividad</td>
>      <td>Matriz de atributos demasiado larga.
>        La matriz de atributos pasada al registro superó la longitud máxima de 65536 bytes</td>
>    </tr>
>    <tr>
>      <td><a name="1076"></a>1076</td>
>      <td>La llamada de <a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/mergeLeadsUsingPOST">Combinar posibles clientes</a> con el indicador mergeInCRM es 4.</td>
>      <td>Está creando un registro duplicado. Se recomienda utilizar un registro existente en su lugar.
>        Este es el mensaje de error que Marketo recibe al combinar en Salesforce.</td>
>    </tr>
>    <tr>
>      <td><a name="1077"></a>1077</td>
>      <td><a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/mergeLeadsUsingPOST">Error en la llamada de combinación de posibles clientes</a> debido a la longitud del campo SFDC</td>
>      <td>Error en una llamada a MergeLeads con mergeInCRM establecido en true debido a que SFDC Field superaba el límite de caracteres permitidos. Para corregirlo, reduzca la longitud de SFDC Field o establezca mergeInCRM en false.</td>
>    </tr>
>    <tr>
>      <td><a name="1078"></a>1078</td>
>      <td><a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/mergeLeadsUsingPOST">Error en la llamada de combinación de posibles clientes</a> debido a una entidad eliminada, no es un posible cliente/contacto o los criterios de filtro de campo no coinciden.</td>
>      <td>Error de combinación, no se puede realizar la operación de combinación en CRM sincronizado de forma nativa
>        Este es el mensaje de error que Marketo recibe al combinar en Salesforce.</td>
>    </tr>
>    <tr>
>      <td><a name="1079"></a>1079</td>
>      <td><a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/mergeLeadsUsingPOST">Error en la llamada de combinación de posibles clientes</a> debido a un conflicto de URL personalizado en registros duplicados</td>
>      <td>Una llamada de combinación de posibles clientes especificó muchos posibles clientes con la misma dirección URL personalizada. Para resolver, utilice la interfaz de usuario de Marketo Engage para combinar estos registros.</td>
>    </tr>
>  </tbody>
></table>
