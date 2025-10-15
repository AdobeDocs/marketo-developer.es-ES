---
title: Archivo de blog
description: Marketo Developer Blog archive 2014-2023 que ofrece publicaciones históricas sobre Forms 2.0, Zapier, actualizaciones de la API, desaprobación de SOAP y migración a REST.
exl-id: d7ae88dd-9938-4957-9798-db43090dab4e
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '61723'
ht-degree: 0%

---

# Archivo de blog

>[!INFO]
>
>Este es un archivo del blog de Marketo, que abarca de 2014 a 2023. Se proporciona aquí solo como referencia histórica.
>&#x200B;>Parte de la información podría estar desactualizada.  Compruebe siempre la documentación actual para obtener la funcionalidad más reciente.
>

>[!IMPORTANT]
>La API de SOAP se está desaprobando y dejará de estar disponible a partir del 31 de enero de 2026. Todo el nuevo desarrollo debe realizarse con la API de REST de Marketo, y los servicios existentes deben migrarse para esa fecha a fin de evitar interrupciones en el servicio. Si tiene un servicio que usa la API de SOAP, consulte la [Guía de migración de API de SOAP](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/soap/migration) para obtener información sobre cómo migrar.
>

>[!IMPORTANT]
>La compatibilidad con la autenticación mediante el parámetro de consulta `access_token` se eliminará el 31 de enero de 2026. Si su proyecto usa un parámetro de consulta para pasar el token de acceso, debe actualizarse para usar el [encabezado de autorización](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/rest/authentication#using-an-access-token) lo antes posible. El nuevo desarrollo debe utilizar exclusivamente el encabezado Autorización.
>

## Bienvenido al blog para desarrolladores de Marketo

Bienvenido, desarrolladores de Marketo. Hemos estado bastante ocupados aquí en Marketo lanzando nuevas funciones para ayudar a los especialistas en marketing a tener más éxito que nunca. Los especialistas en marketing de hoy en día cuentan con el apoyo de una fenomenal comunidad de desarrolladores. Este blog está dedicado a aquellos desarrolladores web e ingenieros de software que apoyan las necesidades de rápida evolución del experto en marketing moderno. A medida que la plataforma de Marketo evoluciona, vamos anunciando nuevas opciones de integración y actualizaciones de las versiones de las API a medida que se van liberando. También presentaremos una nueva serie de artículos explicativos en los que compartimos muestras de código y prácticas recomendadas sobre la integración con Marketo Platform. El primer artículo de esta serie le guiará para recuperar de forma eficaz información sobre las personas (clientes/contactos/posibles clientes) almacenadas en Marketo mediante la API. Para mantenerse al día, suscríbase utilizando el formulario anterior. Le enviamos correos electrónicos con las actualizaciones a medida que se publican nuevos artículos.

Publicado el _06-03-2014_ por _David_

## Actualizaciones de la versión de enero de 2014

### Formularios 2.0

Forms 2.0 permite a los especialistas en marketing crear formularios web hermosos, estables y flexibles sin conocimientos de programación. Además del editor, la lógica condicional y el diseño sólido, que han mejorado en gran medida, ahora es más fácil que nunca incrustar formularios Marketo en cualquier página del sitio web. Los desarrolladores pueden utilizar JavaScript para ampliar la funcionalidad principal; entre los casos de uso se incluyen:

* Al ocultar un formulario después de enviarlo correctamente, el visitante permanece en la página después de haber rellenado el formulario
* Mostrar un mensaje de error personalizado al enviar basado en la lógica empresarial personalizada
* Mostrar el formulario en un cuadro de diálogo de estilo Lightbox

La documentación para desarrolladores está disponible [aquí](/help/javascript-api/forms-api-reference.md).

### Ya está disponible la versión 2_3 de la API de SOAP

* [getLeadChanges:](/help/soap-api/getleadchanges.md) introdujo el campo de solicitud `activityNameFilter`
* [ListOperation:](/help/soap-api/listoperation.md) eliminó el campo de solicitud `skipActivityLog`

**Nota:** Las revisiones de la API de SOAP son compatibles con versiones anteriores

Publicado el _2014-01-26_ por _Travis Kaufman_

## Zapier Parte II: Anuncio sobre la integración de Marketo

En un post anterior, discutimos cómo se podía usar Zapier para integrar fuentes de datos externas con Marketo. La publicación presentó un enfoque práctico para construir su propio flujo de trabajo Zapier (o &quot;Zap&quot;) desde cero para integrar Marketo y otras aplicaciones. Por lo tanto, le gusta la idea de utilizar Zapier con Marketo, pero ¿necesita ayuda para empezar? ¡Buenas noticias! Zapier acaba de lanzar una serie de ejemplos de Zaps para Marketo que le permiten ponerse en marcha rápidamente:

* Capturar nuevos posibles clientes
* Nutre a sus clientes
* Distribuir información de contacto
* Reaccionar ante nuevos clientes potenciales

Además de estos ejemplos, puede navegar por la página de [integraciones de Marketo](https://zapier.com/apps/marketo/integrations) con cientos de otras aplicaciones en Zapier y crear sus propios flujos de trabajo automatizados en minutos. No se requiere codificación. Ahorre tiempo y deje que la automatización haga su trabajo manual. Pónganse creativos. ¡El cielo es el límite!

Publicado el _01-06-2016_ por _David_

## Recuperación de información de clientes y clientes potenciales de Marketo mediante la API

Puede recuperar información sobre clientes y clientes potenciales almacenados en Marketo mediante las API de SOAP `getLead` y [`getMultipleLeads`](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads). A menudo se desea extraer esta información de forma recurrente para mantener otro sistema actualizado a medida que se actualiza la información de los clientes y clientes potenciales o se crean nuevos registros en Marketo. Le mostramos el ejemplo de código que se ejecutaría de forma recurrente para sondear Marketo y buscar actualizaciones. El diagrama siguiente muestra las llamadas de API que se realizan en un temporizador fijo periódico. Según el caso de uso, el temporizador periódico se puede configurar para ejecutar el siguiente código cada 10 minutos.

La primera llamada a [`getMultipleLeads`](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads) establecerá el intervalo de tiempo, batchSize y los campos que se devolverán. Todos los posibles clientes dentro de Marketo que se actualizaron en el intervalo de tiempo especificado se devolverán junto con una streamPosition cuando haya más registros disponibles que batchSize especificado.

**Solicitud de SOAP para la primera llamada a getMultipleLeads:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ns2:paramsGetMultipleLeads xmlns:ns2="<http://www.marketo.com/mktows/">
  <leadSelector xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="ns2:LastUpdateAtSelector">
    <latestUpdatedAt>2014-02-13T11:51:08.710-08:00</latestUpdatedAt>
    <oldestUpdatedAt>2014-02-12T11:51:08.713-08:00</oldestUpdatedAt>
  </leadSelector>
  <batchSize>2</batchSize>
  <includeAttributes>
    <stringItem>FirstName</stringItem>
    <stringItem>LastName</stringItem>
    <stringItem>Email</stringItem>
  </includeAttributes>
</ns2:paramsGetMultipleLeads>
```

**Respuesta de SOAP desde la primera llamada a getMultipleLeads:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ns2:successGetMultipleLeads xmlns:ns2="<http://www.marketo.com/mktows/">
  <result>
 <returnCount>2</returnCount><remainingCount>24</remainingCount><newStreamPosition>id:1089965:to:1392234668:tl:1392321068:os:2:rc:24</newStreamPosition><leadRecordList>
      <leadRecord>
        <Id>84105</Id>
        <Email>eimang@marketo.com</Email>
        <ForeignSysPersonId xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
        <ForeignSysType xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
        <leadAttributeList xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true">
          <attribute>
            <attrName>FirstName</attrName>
            <attrType>string</attrType>
            <attrValue>French</attrValue>
          </attribute>
          <attribute>
            <attrName>LastName</attrName>
            <attrType>string</attrType>
            <attrValue>Lead</attrValue>
          </attribute>
        </leadAttributeList>
      </leadRecord>
      <leadRecord>
        <Id>1089965</Id>
        <Email>t@t.com</Email>
        <ForeignSysPersonId xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
        <ForeignSysType xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
        <leadAttributeList xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true">
          <attribute>
            <attrName>FirstName</attrName>
            <attrType>string</attrType>
            <attrValue>George</attrValue>
          </attribute>
          <attribute>
            <attrName>LastName</attrName>
            <attrType>string</attrType>
            <attrValue>of the Jungle</attrValue>
          </attribute>
        </leadAttributeList>
      </leadRecord>
    </leadRecordList>
  </result>
</ns2:successGetMultipleLeads>
```

Siempre que el valor `<remainingCount/>` sea mayor que 0, se realizan llamadas subsiguientes a `getMultipleLeads` para paginar el resto pasando el valor `<newStreamPosition/>` devuelto en la llamada anterior al parámetro `<streamPosition/>`. **Solicitud de SOAP para la llamada subsiguiente a getMultipleLeads:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ns2:paramsGetMultipleLeads xmlns:ns2="<http://www.marketo.com/mktows/">
  <leadSelector xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="ns2:LastUpdateAtSelector">
    <latestUpdatedAt>2014-02-13T11:51:08.710-08:00</latestUpdatedAt>
    <oldestUpdatedAt>2014-02-12T11:51:08.713-08:00</oldestUpdatedAt>
  </leadSelector><streamPosition>id:1089965:to:1392234668:tl:1392321068:os:2:rc:24</streamPosition><batchSize>2</batchSize>
  <includeAttributes>
    <stringItem>FirstName</stringItem>
    <stringItem>LastName</stringItem>
    <stringItem>Email</stringItem>
  </includeAttributes>
</ns2:paramsGetMultipleLeads>
```

Esta lógica se mantiene mientras `<remainingCount/>` sea mayor que cero. **Vea a continuación un ejemplo de programa Java que ejecuta el escenario descrito anteriormente.**

```java
import com.marketo.mktows.\*;
import java.net.URL;
import javax.xml.namespace.QName;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.GregorianCalendar;
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import org.apache.commons.codec.binary.Hex;
import javax.xml.bind.JAXBContext;
import javax.xml.bind.JAXBElement;
import javax.xml.bind.Marshaller;
import javax.xml.datatype.DatatypeFactory;
import javax.xml.datatype.XMLGregorianCalendar;

public class GetMultipleLeads {
  public static void main(String[] args) {
    System.out.println("Executing GetMultipleLeads");
        try {
        URL marketoSoapEndPoint = new URL("CHANGE ME" + "?WSDL");
        String marketoUserId = "CHANGE ME";
        String marketoSecretKey = "CHANGE ME";
        QName serviceName = new QName("<http://www.marketo.com/mktows/>",
        "MktMktowsApiService");
        MktMktowsApiService service = new
        MktMktowsApiService(marketoSoapEndPoint, serviceName);
        MktowsPort port = service.getMktowsApiSoapPort();

        // Create Signature
        DateFormat df = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
        String text = df.format(new Date());
        String requestTimestamp = text.substring(0, 22) + ":" +         text.substring(22);
        String encryptString = requestTimestamp + marketoUserId ;

        SecretKeySpec secretKey = new         SecretKeySpec(marketoSecretKey.getBytes(), "HmacSHA1");

        Mac mac = Mac.getInstance("HmacSHA1");
        mac.init(secretKey);
        byte[] rawHmac = mac.doFinal(encryptString.getBytes());
        char[] hexChars = Hex.encodeHex(rawHmac);
        String signature = new String(hexChars);

        // Set Authentication Header
        AuthenticationHeader header = new AuthenticationHeader();
        header.setMktowsUserId(marketoUserId);
        header.setRequestTimestamp(requestTimestamp);
        header.setRequestSignature(signature);

        // Create Request
        ParamsGetMultipleLeads request = new ParamsGetMultipleLeads();

        // Build Request Using LastUpdateAtSelector
        LastUpdateAtSelector leadSelector = new LastUpdateAtSelector();
        GregorianCalendar gc = new GregorianCalendar();
        gc.setTimeInMillis(new Date().getTime());
        gc.add( GregorianCalendar.DAY_OF_YEAR, -20);
        DatatypeFactory factory = DatatypeFactory.newInstance();
        ObjectFactory objectFactory = new ObjectFactory();
        JAXBElement<XMLGregorianCalendar> until =         objectFactory.createLastUpdateAtSelectorLatestUpdatedAt(factory.newXMLGregorianCalendar(gc));

        GregorianCalendar since = new GregorianCalendar();
        since.setTimeInMillis(new Date().getTime());
        since.add( GregorianCalendar.DAY_OF_YEAR, -21);
        leadSelector.setOldestUpdatedAt(factory.newXMLGregorianCalendar(since));
        leadSelector.setLatestUpdatedAt(until);
        request.setLeadSelector(leadSelector);

        ArrayOfString attributes = new ArrayOfString();
        attributes.getStringItems().add("FirstName");
        attributes.getStringItems().add("LastName");
        attributes.getStringItems().add("Email");
        request.setIncludeAttributes(attributes);

        JAXBElement<Integer> batchSize = new
        ObjectFactory().createParamsGetMultipleLeadsBatchSize(2);
        request.setBatchSize(batchSize);

        SuccessGetMultipleLeads result = null;

        int count = 0;

        do {
        if (count > 0) {
        // Set the streamPosition on subsequent calls to paginate
        through large result sets
        String pos = result.getResult().getNewStreamPosition();
        JAXBElement<String> streamPos = new
        ObjectFactory().createParamsGetMultipleLeadsStreamPosition(pos);
        request.setStreamPosition(streamPos);
        }

        JAXBContext context =
        JAXBContext.newInstance(ParamsGetMultipleLeads.class);
        Marshaller m = context.createMarshaller();
        m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
        System.out.println("REQUEST:");
        m.marshal(request, System.out);
        result = port.getMultipleLeads(request, header);
        System.out.println("RESPONSE:");
        m.marshal(result, System.out);
        count = result.getResult().getReturnCount();
        } while (result.getResult().getRemainingCount() > 0);
        }

        catch(Exception e) {
        // Update to include appropriate retry/error handling
        e.printStackTrace();
        }

  }

}
```

### Sugerencias y trucos

Al extraer grandes volúmenes de contactos fuera de Marketo, se recomienda ajustar la solicitud de API con los siguientes parámetros:

* `<includeAttributes/>`: se recomienda que solo solicite los campos que le interese mantener sincronizados con el sistema. Esto reduce la carga útil de la respuesta y aumenta el rendimiento de la consulta.
* `<batchSize/>`: la API admite devolver hasta 1000 registros en una sola llamada. Ajustar este valor a 500 registros también reduce la carga útil de la respuesta.
* `<LastUpdatedAtSelector/>`: se recomienda establecer `<oldestUpdatedAt/>` junto con el parámetro `<latestUpdatedAt/>` para limitar el intervalo de fecha. Por ejemplo, en lugar de realizar una sola solicitud de datos de un año. Desglose las llamadas de API para solicitar intervalos de fechas más pequeños.

Este artículo contiene el código utilizado para implementar integraciones personalizadas. Debido a su naturaleza personalizada, el equipo de soporte técnico de Marketo no puede solucionar problemas del trabajo personalizado. No intente implementar el siguiente código de ejemplo sin tener la experiencia técnica adecuada o sin tener acceso a un desarrollador experimentado.

Publicado el _2014-03-05_ por _Travis Kaufman_

## Actualizaciones de la versión de febrero de 2014

### Actualización de API de SOAP

* [syncMObjects](/help/soap-api/syncmobjects.md): ahora puede agregar y actualizar etiquetas y canales para los programas existentes.

Las actualizaciones se incorporan al WSDL [2_3](http://app.marketo.com/soap/mktows/2_3?WSDL).

Publicado el _2014-02-26_ por _Travis Kaufman_

## Actualizaciones de la versión de marzo de 2014

### Actualización de API de SOAP

* Mejoras de rendimiento en [syncLead](/help/soap-api/synclead.md) y [syncMultipleLeads](/help/soap-api/syncmultipleleads.md)

Las actualizaciones se incorporan al WSDL [2_3](http://app.marketo.com/soap/mktows/2_3?WSDL).

Publicado el _2014-03-20_ por _Travis Kaufman_

## Combinar actividades anónimas de visitante cuando el visitante rellena el formulario

En la publicación de blog titulada &quot;Capturar la actividad de visitantes anónimos en función de la lógica empresarial&quot;, analizamos cómo crear registros de posibles clientes anónimos en Marketo basados en eventos personalizados. En esta publicación de blog, nos basaremos en esa publicación y asociaremos un registro de posible cliente anónimo con un usuario conocido después de que reciba la información de contacto del usuario. El [código de seguimiento Munchkin](/help/javascript-api/lead-tracking.md) de Marketo le ayuda a rastrear las visitas a su sitio web. La primera vez que un usuario visita una página del sitio web que tiene el código de seguimiento de Munchkin, Marketo crea un posible cliente anónimo y utiliza una cookie del explorador para rastrearlo. Una vez que se identifican, se convierten en un posible cliente conocido y el historial asociado con la cookie de su explorador se combina en su registro de posibles clientes de Marketo. **Los posibles clientes anónimos se crean cuando alguien:**

1. Visita la página de aterrizaje de Marketo por primera vez
1. Visita una página con código de seguimiento Munchkin
1. Haga clic en el vínculo Ver como página web de un correo electrónico de Marketo

**Un posible cliente anónimo se combina en uno nuevo o existente cuando alguien:**

1. Haga clic en un vínculo a un correo electrónico de Marketo
1. Rellena un formulario de Marketo
1. Utiliza una de las técnicas siguientes para asociar un posible cliente anónimo con un registro conocido.

Para enviar datos de persona o asociar una cookie de seguimiento web del explorador con el registro de persona correspondiente en Marketo, utilice una de las siguientes técnicas: **Envío del lado del explorador** Si su caso de uso requiere el envío de datos de persona desde el explorador, debe utilizar el envío del formulario en segundo plano. **Envío del lado del servidor** Si no necesita envío del lado del explorador, la API de REST ofrece muchos métodos para el envío de datos de personas y un método creado específicamente para asociar cookies con registros de personas.

Publicado el _2014-04-22_ por _Murta_

## Actualizaciones de la versión de abril de 2014

### Actualización de seguridad de Marketo Forms

Hemos introducido un límite en el número y la frecuencia de los envíos de formularios posteriores desde una sola dirección IP. Este límite ahora se aplica a 30 publicaciones por minuto para proteger a nuestros clientes del uso malintencionado de envíos de formularios programáticos. La [API syncLead](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/soap/leads/synclead) es el vehículo de integración recomendado para el envío mediante programación de nuevos contactos en Marketo.

Publicado el _2014-04-29_ por _Travis Kaufman_

## Reemplace la integración posterior del formulario con la API de Marketo

Es una práctica común que los especialistas en marketing que utilizan Marketo asocien el éxito a una campaña de marketing determinada en función del número de contactos/posibles clientes que han rellenado un formulario web específico. El especialista en marketing simplemente crearía un nuevo formulario de Marketo, lo colocaría en una página de aterrizaje y, a continuación, configuraría una campaña de déclencheur en Marketo para asociar a todos los contactos que rellenaron ese formulario específico como un éxito para ese programa de marketing. (véase la captura de pantalla siguiente). Es una extensión natural utilizar este mismo método al registrar el éxito del programa incluso cuando el éxito de la campaña reside fuera de alguien que rellena un formulario. Por ejemplo, cuando un experto en marketing ejecuta una oferta promocional y la conversión es llamar y programar una cita. El cliente potencial no está rellenando un formulario en ningún momento del proceso. En el siguiente artículo, le mostramos cómo utilizar la API syncLead de Marketo para crear un nuevo contacto si son nuevos contactos y, a continuación, solicitar la API de Campaign para registrar el éxito de un programa de marketing determinado.

Publicado el _1970-01-01_ por _Travis Kaufman_

## Eliminar un posible cliente con la API de Marketo

Supongamos que desea eliminar un posible cliente mediante la API de Marketo. Puede hacerlo llamando a la API requestCampaign en una campaña que tenga una acción de flujo predefinida para eliminar un posible cliente. En primer lugar, se muestra cómo crear una campaña inteligente, en segundo, cómo configurar un déclencheur para ejecutar una campaña a través de la API, en tercer lugar, cómo definir la eliminación de un posible cliente como parte de una acción de flujo y, en cuarto, una muestra de código que se utilizaría para ejecutar esta campaña. **Cómo crear una nueva campaña inteligente en Marketo** Las campañas inteligentes en Marketo ejecutan todas sus actividades de marketing. En una campaña inteligente, puede configurar una serie de acciones automatizadas para realizar en una lista inteligente de contactos. En el caso de eliminar un posible cliente, configura un déclencheur en la campaña como se muestra a continuación. Primero vamos a configurar la campaña inteligente.

1. En Actividades de marketing, elija un Programa y, a continuación, en la lista desplegable Nuevo, haga clic en Nuevo recurso local 1. Haga clic en Campaña inteligente
1. Introduzca el nombre de la campaña inteligente y haga clic en Crear Añadir Déclencheur a una campaña inteligente** Añadir Déclencheur a una campaña inteligente le permite hacer que una campaña inteligente se ejecute en una persona a la vez en función de un evento en directo, que en este caso es una solicitud a través de la API requestCampaign.
1. Busque el déclencheur &quot;Campaña solicitada&quot; y arrástrelo y suéltelo en el lienzo.
1. En el déclencheur, seleccione &quot;es&quot; y &quot;API de servicio web&quot;.

**Cómo crear una acción de flujo para eliminar un posible cliente en una campaña** Haga clic en Flujo en el menú superior. En el menú de la derecha, busque Eliminar posible cliente y arrástrelo al centro para agregarlo como déclencheur a la campaña. Nota: Si elimina un posible cliente solo de Marketo y lo deja en su CRM, cuando se actualicen los datos de ese posible cliente, el posible cliente se volverá a crear en Marketo.  **Ejemplo de código para llamar a la API requestCampaign** Después de configurar la campaña y los déclencheur en la interfaz de Marketo, le mostramos cómo usar la API para ejecutar esta campaña. El primer ejemplo es una solicitud XML, el segundo es una respuesta XML y el último es un ejemplo de código Java que se puede utilizar para generar la solicitud XML. También le mostramos cómo encontrar el ID de campaña que se utiliza al realizar una llamada a la API requestCampaign. La llamada de API también requiere que conozca previamente el ID de la campaña de Marketo. Puede determinar el ID de campaña mediante cualquiera de los siguientes métodos:

1. Usar la API [getCampaignsForSource](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getCampaignsUsingGET)
1. Abra la campaña de Marketo en un explorador y observe la barra de direcciones URL. El ID de campaña (representado como un entero de 4 dígitos) se puede encontrar inmediatamente después de &quot;SC&quot;. Por ejemplo, `https://app-stage.marketo.com/#SC**1025**A1`. La parte en negrita es el ID de campaña: &quot;1025&quot;. Solicitud de SOAP para requestCampaign

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809939944BFABAE58E5D27</mktowsUserId><requestSignature>48397ad47b71a1439f13a51eea3137df46441979</requestSignature><requestTimestamp>2013-08-01T12:31:14-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsRequestCampaign>
      <source>MKTOWS</source>
      <campaignId>4496</campaignId>
      <leadList>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>lead@company.com</keyValue>
        </leadKey>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>anotherlead@company.com</keyValue>
        </leadKey>
      </leadList>
    </ns1:paramsRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Respuesta de SOAP para requestCampaign

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successRequestCampaign>
      <result>
        <success>true</success>
      </result>
    </ns1:successRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Consulte a continuación un ejemplo de programa Java que ejecuta el escenario descrito anteriormente.

```java
import com.marketo.mktows.*;
import java.net.URL;
import javax.xml.namespace.QName;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import org.apache.commons.codec.binary.Hex;
import javax.xml.bind.JAXBContext;
import javax.xml.bind.JAXBElement;
import javax.xml.bind.Marshaller;


public class RequestCampaign {

    public static void main(String[] args) {
        System.out.println("Executing Request Campaign");
        try {
            URL marketoSoapEndPoint = new URL("CHANGE ME" + "?WSDL");
            String marketoUserId = "CHANGE ME";
            String marketoSecretKey = "CHANGE ME";

            QName serviceName = new QName("http://www.marketo.com/mktows/", "MktMktowsApiService");
            MktMktowsApiService service = new MktMktowsApiService(marketoSoapEndPoint, serviceName);
            MktowsPort port = service.getMktowsApiSoapPort();

            // Create Signature
            DateFormat df = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
            String text = df.format(new Date());
            String requestTimestamp = text.substring(0, 22) + ":" + text.substring(22);
            String encryptString = requestTimestamp + marketoUserId ;

            SecretKeySpec secretKey = new SecretKeySpec(marketoSecretKey.getBytes(), "HmacSHA1");
            Mac mac = Mac.getInstance("HmacSHA1");
            mac.init(secretKey);
            byte[] rawHmac = mac.doFinal(encryptString.getBytes());
            char[] hexChars = Hex.encodeHex(rawHmac);
            String signature = new String(hexChars);

            // Set Authentication Header
            AuthenticationHeader header = new AuthenticationHeader();
            header.setMktowsUserId(marketoUserId);
            header.setRequestTimestamp(requestTimestamp);
            header.setRequestSignature(signature);

            // Create Request
            ParamsRequestCampaign request = new ParamsRequestCampaign();

            request.setSource(ReqCampSourceType.MKTOWS);

            ObjectFactory objectFactory = new ObjectFactory();
            JAXBElement<Integer> campaignId = objectFactory.createParamsRequestCampaignCampaignId(4496);
            request.setCampaignId(campaignId);

            ArrayOfLeadKey leadKeyList = new ArrayOfLeadKey();
            LeadKey key = new LeadKey();
            key.setKeyType(LeadKeyRef.EMAIL);
            key.setKeyValue("lead@company.com");

            LeadKey key2 = new LeadKey();
            key2.setKeyType(LeadKeyRef.EMAIL);
            key2.setKeyValue("anotherlead@company.com");

            leadKeyList.getLeadKeies().add(key);
            leadKeyList.getLeadKeies().add(key2);

            JAXBElement<ArrayOfLeadKey> arrayOfLeadKey = objectFactory.createParamsRequestCampaignLeadList(leadKeyList);
            request.setLeadList(arrayOfLeadKey);

            SuccessRequestCampaign result = port.requestCampaign(request, header);

            JAXBContext context = JAXBContext.newInstance(SuccessRequestCampaign.class);
            Marshaller m = context.createMarshaller();
            m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
            m.marshal(result, System.out);

        }
        catch(Exception e) {
            e.printStackTrace();
        }
    }
}
```

Este artículo contiene el código utilizado para implementar integraciones personalizadas. Debido a su naturaleza personalizada, el equipo de soporte técnico de Marketo no puede solucionar problemas del trabajo personalizado. No intente implementar el siguiente código de ejemplo sin tener la experiencia técnica adecuada o sin tener acceso a un desarrollador experimentado.

Publicado el _2014-05-16_ por _Murta_

## Seguimiento de actividades personalizado con la API de Munchkin

Supongamos que desea realizar un seguimiento de la actividad personalizada en Marketo. Por ejemplo, tiene un vídeo en una página web y quiere rastrear visitantes que ven más del 50 % de un vídeo. Puede hacerlo mediante la función de seguimiento de actividades personalizada de Munchkin. Esto se implementaría escuchando un evento en la página, que es el vídeo que llega al 50 %, y luego llamando a la API de Munchkin. Para ello, se debe configurar una actividad personalizada en Marketo a la que llamar en función de este evento en la página. Usamos YouTube para el reproductor de vídeo y usamos su [API de YouTube Iframe](https://developers.google.com/youtube/iframe_api_reference) para llamar al método en la API de Munchkin.

En primer lugar, se muestra cómo generar el código de seguimiento de Munchkin en Marketo, en segundo, cómo modificar el código de muestra de Munchkin en déclencheur en función de los eventos de página, en tercer lugar, cómo configurar una campaña con una lista inteligente definida por acciones en la página con pasos de flujos y, en quinto lugar, cómo comprobar que una visita de página de un usuario anónimo se registró en Marketo. ==== Esta publicación de blog es un ejemplo en directo del código que se explica. Rellene este formulario, por lo que es un usuario conocido de Marketo. De esta manera, cuando ves el 50% del video, te envía el resto del video, y si ves el 100% del video, te envía un enlace a otra publicación de blog. === <https://developers.google.com/youtube/iframe_api_reference>

**Cómo generar el código de seguimiento de Munchkin** El código de seguimiento de Munchkin le permite hacer un seguimiento de las visitas a su sitio web. A continuación se describen tres tipos de código Munchkin, pero en este ejemplo utilizamos el código de seguimiento Munchkin asincrónico. A) Simple: tiene las menos líneas de código, pero no optimiza el tiempo de carga de la página web. Este código carga la biblioteca jQuery cada vez que se carga una página web. B) Asincrónica: reduce el tiempo de carga de la página web. Este código comprueba si la biblioteca jQuery ya existe, la carga si falta y la utiliza para ejecutar el código de seguimiento una vez que se ha cargado el resto de la página web. C) Async jQuery: reduce el tiempo de carga de la página web y también mejora el rendimiento del sistema. Este código supone que ya tiene jQuery y no marca la casilla para cargarlo.

1. Haga clic en Admin en la parte superior derecha de la aplicación.
1. Haga clic en Munchkin en el árbol de la izquierda.
1. Seleccione Asincrónica para el tipo de código de seguimiento.
1. Haga clic en y copie el código de seguimiento de JavaScript que desea colocar en el sitio web. **Código YouTube** <https://developers.google.com/youtube/js_api_reference#EventHandlers> <https://developers.google.com/youtube/iframe_api_reference> jugador.

`getCurrentTime()` Devuelve el tiempo transcurrido en segundos desde que comenzó a reproducirse el vídeo. `player.getDuration()` Devuelve la duración en segundos del vídeo que se está reproduciendo. Tenga en cuenta que `getDuration()` devolverá 0 hasta que se carguen los metadatos del vídeo, lo que normalmente sucede justo después de que comience la reproducción del vídeo. Cuando un usuario sin cookies va a una página con el código de seguimiento de Munchkin, se crea una nueva cookie en el explorador del usuario y se crea un nuevo posible cliente anónimo en Marketo. Si el usuario ya tiene cookies y ya es un posible cliente de Marketo, la visita a la página se registrará en el registro de actividad del usuario en Marketo. **Ejemplo de código para rastrear eventos y usuarios de cookies** Coloque el código de seguimiento en sus páginas web justo antes de la etiqueta `</body>`. Las páginas de aterrizaje creadas en Marketo contienen automáticamente código de seguimiento, por lo que no es necesario colocarlo. Este ejemplo de código llamaría a la API de Munchkin después de cargar el script:

```javascript
<script type="text/javascript">
(function() {
  var didInit = false;
  function initMunchkin() {
    if(didInit === false) {
      didInit = true;
      Munchkin.init('XXX-XXX-XXX');
    }
  }
  var s = document.createElement('script');
  s.type = 'text/javascript';
  s.async = true;
  s.src = '//munchkin.marketo.net/munchkin.js';
  s.onreadystatechange = function() {
    if (this.readyState == 'complete' || this.readyState == 'loaded') {
      initMunchkin();
    }
  };
  s.onload = initMunchkin;
  document.getElementsByTagName('head')[0].appendChild(s);
})();
</script>
```

Este ejemplo de código llama a la API de Munchkin después de que el usuario haya estado en la página durante 5 segundos y también haya desplazado 500 píxeles hacia abajo en la página:

```javascript
<script src="https://code.jquery.com/jquery-2.1.0.min.js"></script>
<script type="text/javascript">
$(function(){
 setTimeout(function(){
  $(window).scroll(function() {
      var y_scroll_position = window.pageYOffset;
      var scroll_position = 500; //Sets number of pixels user must scroll to be tracked

  if(y_scroll_position > scroll_position) {
  //Munchkin tracking code
   (function() {
     var didInit = false;
     function initMunchkin() {
      if(didInit === false) {
        didInit = true;
        Munchkin.init('XXX-XXX-XXX');
      }
     }

     var s = document.createElement('script');
     s.type = 'text/javascript';
     s.async = true;
    s.src = '//munchkin.marketo.net/munchkin.js';
     s.onreadystatechange = function() {
      if (this.readyState == 'complete' || this.readyState == 'loaded') {
          initMunchkin();
      }
     };
     s.onload = initMunchkin;
     document.getElementsByTagName('head')[0].appendChild(s);
   })();
   }
 },5000); //Sets time delay before tracking user
});
</script>
```

**Cómo verificar que la visita a la página de un usuario anónimo se haya registrado en Marketo**

1. Haga clic en Analytics en el menú superior y, a continuación, haga clic en Nuevo informe. Elija Actividad de página web como tipo de informe y asigne un nombre al informe.
1. Después de crear un informe, haga clic en Lista inteligente. A continuación, seleccione el filtro Página web visitada en el cuadro de la derecha. Introduzca la página web donde colocó el código de seguimiento de Munchkin.
1. Haga clic en Configuración. Seleccione Visitantes anónimos de los ISP y cambie la opción a Mostrado.
1. Haga clic en Informe. Ahora verá la actividad rastreada en la página web que seleccionó.
1. Haga doble clic en el registro de posibles clientes, que luego mostrará el registro de actividad donde puede ver la página específica que visitó el usuario anónimo.

Este artículo contiene el código utilizado para implementar integraciones personalizadas. Debido a su naturaleza personalizada, el equipo de soporte técnico de Marketo no puede solucionar problemas del trabajo personalizado. No intente implementar el siguiente código de ejemplo sin tener la experiencia técnica adecuada o sin tener acceso a un desarrollador experimentado.

Por ejemplo, en el caso de páginas con contenido multimedia, puede que desee realizar un seguimiento personalizado. Un ejemplo común es agregar el código de seguimiento de Munchkin a la página y también utilizar la API de Munchkin para generar eventos en la instancia de Marketo para actividades como reproducir un vídeo o escuchar un clip de audio. Le recomendamos que coloque el código de seguimiento de Munchkin en la mayoría o en todas las páginas web. El código de seguimiento de Munchkin se incluye automáticamente en las páginas de aterrizaje que crea con Marketo. Utilice esta llamada para registrar que el usuario ha hecho algo, como visitar una página en Ajax, Flash u otro entorno de RIA. La dirección URL no debe contener &quot; ni ningún dominio, pero puede señalar a cualquier página, incluso a páginas que no existen. Si desea agregar parámetros de URL, utilice el argumento params.
El evento se muestra como un evento de visita a página web en el registro de actividades del usuario en el dominio de la página web que realiza la llamada. Nota La primera llamada a `mktoMunchkin()` siempre crea un evento de página web de visita para la página actual. No necesita llamar a `visitWebPage` a menos que desee rastrear una visita adicional a la página web. `mktoMunchkinFunction('visitWebPage', { url: '/MyFlashMovie/Story1', params: 'x=y&2=3' });` Nota: asegúrese de que tiene acceso a un desarrollador experimentado de JavaScript. La asistencia técnica de Marketo no está configurada para ayudar a solucionar problemas de JavaScript personalizado. La API de Munchkin JavaScript le permite integrar un sistema web de terceros con su cuenta de Marketo. Con algunos desarrollos web, puede capturar nuevos posibles clientes o actualizar los actuales con las aplicaciones existentes en el sitio web. Supongamos que tiene una aplicación web para el registro de clientes que captura información nueva del cliente. Con solo un poco de programación, también puede tener información de posibles clientes para esos usuarios capturados en Marketo y una cookie de Marketo configurada para el seguimiento web futuro.

Además, otra función permite a los desarrolladores web capturar y rastrear información de actividad web de entornos web enriquecidos, como Flash o Ajax. Nota: Si tiene los recursos de desarrollo adecuados, debe considerar la posibilidad de utilizar nuestra API de SOAP para la integración en lugar de esta API. La API de SOAP es más sólida y tiene más funcionalidad que la API de Munchkin. Requisitos de la API de Marketo SOAP Debe incluir el código JavaScript de Munchkin en la página web para que esto funcione. Puede encontrar las etiquetas de script necesarias en el Tutorial de Munchkin. Habilite también la API de Munchkin, que también se describe en el tutorial.
Under the Hood Después de realizar una llamada a la API de Munchkin, se establece automáticamente la cookie del usuario si no tiene ninguna cookie. En Marketo, registra el evento (hacer clic en un vínculo, visitar una página web o un nuevo posible cliente) en el registro de actividad de la persona. Si utiliza el vínculo de clic o visita una llamada a una página web, el evento se agrega al registro de actividad de ese posible cliente (conocido o anónimo). Si se trata de un posible cliente nuevo y utiliza la llamada de posible cliente asociada, ese posible cliente se convierte en un posible cliente conocido y se conservará su historial de actividades. Si se trata de un posible cliente existente (según la coincidencia de dirección de correo electrónico), cualquier valor cambiado o nuevo se actualizará en el registro de ese posible cliente.

Este es el formato general de una llamada a `munchkinFunction`. Inclúyala como etiquetas en la página web donde quiera que la llame. Puede invocar esto como cualquier otra función de JavaScript. Sin embargo, debe llamar a la función de seguimiento de Munchkin `mktoMunchkin()` antes de realizar cualquier llamada a `mktoMunchkinFunction()`:

```javascript
<script src="http://munchkin.marketo.net/munchkin.js" type="text/javascript"> mktoMunchkin("###-###-###"); mktoMunchkinFunction('function', { key: 'value', key2: 'value'}, 'hash');
```

Donde: `###-###-###` es el identificador de la cuenta de Munchkin para la función de la cuenta es la llamada a la que desea que los parámetros sean una matriz de parámetros necesarios para el hash de la llamada que solo se necesita para `associateLead`

Publicado el _1970-01-01_ por _Murta_

## Introducción de datos en Marketo

La siguiente presentación muestra las diferentes formas de introducir datos en Marketo. Se centra en formularios, objetos personalizados e integraciones.

[Introducción de datos en Marketo](https://www.slideshare.net/MurtzaManzur/getting-data-into-marketo-35662408) desde [Murtza Manzur](https://www.slideshare.net/MurtzaManzur)

Publicado el _2014-06-06_ por _Murta_

## Creación de posibles clientes en una Workspace

Digamos que su empresa tiene dos divisiones: Norteamérica y Europa. Le gustaría segmentar sus posibles clientes según la división de la empresa en Marketo. Puede hacerlo mediante los espacios de trabajo, una función de Marketo que le permite limitar el acceso a los posibles clientes. Para ello, debe crear un espacio de trabajo para Norteamérica y otro para Europa. A continuación, puede crear un posible cliente en un área de trabajo concreta mediante la [API syncLead](/help/soap-api/synclead.md). Considere la posibilidad de utilizar espacios de trabajo y particiones de posibles clientes si su organización tiene:

1. Equipos de marketing independientes para varias líneas de productos
1. Equipos de marketing independientes para diferentes territorios o países
1. Una empresa matriz y empresas filiales
1. Una empresa matriz y distribuidores

Al utilizar particiones de posibles clientes y espacios de trabajo, puede:

1. Restringir el acceso a los posibles clientes de su organización
1. Restringir el acceso a los recursos de su organización
1. Compartir recursos entre equipos de marketing

En primer lugar, se muestra cómo crear un espacio de trabajo en Marketo a través de la interfaz de usuario y, en segundo, cómo escribir un posible cliente para ese espacio de trabajo mediante la [API syncLead](/help/soap-api/synclead.md). **Creación de un Workspace** Un espacio de trabajo es un conjunto de posibles clientes y recursos de Marketo. En un espacio de trabajo, solo puede ver posibles clientes de ese espacio de trabajo y los recursos (correos electrónicos, campañas, listas, etc.) de ese espacio de trabajo. Las campañas inteligentes de ese espacio de trabajo solo afectan a los posibles clientes de ese espacio de trabajo. Para ver los espacios de trabajo de su cuenta:

1. Vaya a la página Espacios de trabajo y particiones de posibles clientes de la sección Administración. Los espacios de trabajo aparecerán en la pestaña Espacios de trabajo. 1. Para crear un espacio de trabajo nuevo, haga clic en el botón Nuevo Workspace de la barra de menús de la pestaña Espacios de Trabajo.
1. En el cuadro de diálogo, debe agregar información sobre el nuevo espacio de trabajo:

* **Nombre de Workspace**: el nombre de este área de trabajo tal como aparece en la interfaz
* **Descripción**: una descripción de texto opcional del área de trabajo
* **Particiones de posibles clientes** - qué posibles clientes son visibles en esta partición.
* **Todas las particiones de posibles clientes**: verá posibles clientes de todas las particiones presentes y futuras
* **Seleccionar particiones individuales**: solo se ven los posibles clientes de esas particiones (y ninguna partición futura)
* **Partición principal de posibles clientes**: los posibles clientes que visitan sus páginas de aterrizaje se agregan a esta partición de forma predeterminada
* **Idioma**: el idioma comercial del área de trabajo

Le mostramos cómo utilizar los posibles clientes de escritura de API en un espacio de trabajo concreto. El primer ejemplo es una solicitud XML, el segundo es una respuesta XML y el último es un ejemplo de código Ruby que se puede utilizar para generar la solicitud XML. 1. Después de crear el posible cliente, la partición es un campo en la información del posible cliente. Solicitud de SOAP para `requestCampaign`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809939944BFABAE58E5D27</mktowsUserId><requestSignature>48397ad47b71a1439f13a51eea3137df46441979</requestSignature><requestTimestamp>2013-08-01T12:31:14-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsRequestCampaign>
      <source>MKTOWS</source>
      <campaignId>4496</campaignId>
      <leadList>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>lead@company.com</keyValue>
        </leadKey>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>anotherlead@company.com</keyValue>
        </leadKey>
      </leadList>
    </ns1:paramsRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Respuesta de SOAP para requestCampaign

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successRequestCampaign>
      <result>
        <success>true</success>
      </result>
    </ns1:successRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Consulte a continuación un ejemplo de programa Java que ejecuta el escenario descrito anteriormente.

```java
import com.marketo.mktows.*;
import java.net.URL;
import javax.xml.namespace.QName;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import org.apache.commons.codec.binary.Hex;
import javax.xml.bind.JAXBContext;
import javax.xml.bind.JAXBElement;
import javax.xml.bind.Marshaller;


public class RequestCampaign {

    public static void main(String[] args) {
        System.out.println("Executing Request Campaign");
        try {
            URL marketoSoapEndPoint = new URL("CHANGE ME" + "?WSDL");
            String marketoUserId = "CHANGE ME";
            String marketoSecretKey = "CHANGE ME";

            QName serviceName = new QName("http://www.marketo.com/mktows/", "MktMktowsApiService");
            MktMktowsApiService service = new MktMktowsApiService(marketoSoapEndPoint, serviceName);
            MktowsPort port = service.getMktowsApiSoapPort();

            // Create Signature
            DateFormat df = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
            String text = df.format(new Date());
            String requestTimestamp = text.substring(0, 22) + ":" + text.substring(22);
            String encryptString = requestTimestamp + marketoUserId ;

            SecretKeySpec secretKey = new SecretKeySpec(marketoSecretKey.getBytes(), "HmacSHA1");
            Mac mac = Mac.getInstance("HmacSHA1");
            mac.init(secretKey);
            byte[] rawHmac = mac.doFinal(encryptString.getBytes());
            char[] hexChars = Hex.encodeHex(rawHmac);
            String signature = new String(hexChars);

            // Set Authentication Header
            AuthenticationHeader header = new AuthenticationHeader();
            header.setMktowsUserId(marketoUserId);
            header.setRequestTimestamp(requestTimestamp);
            header.setRequestSignature(signature);

            // Create Request
            ParamsRequestCampaign request = new ParamsRequestCampaign();

            request.setSource(ReqCampSourceType.MKTOWS);

            ObjectFactory objectFactory = new ObjectFactory();
            JAXBElement<Integer> campaignId = objectFactory.createParamsRequestCampaignCampaignId(4496);
            request.setCampaignId(campaignId);

            ArrayOfLeadKey leadKeyList = new ArrayOfLeadKey();
            LeadKey key = new LeadKey();
            key.setKeyType(LeadKeyRef.EMAIL);
            key.setKeyValue("lead@company.com");

            LeadKey key2 = new LeadKey();
            key2.setKeyType(LeadKeyRef.EMAIL);
            key2.setKeyValue("anotherlead@company.com");

            leadKeyList.getLeadKeies().add(key);
            leadKeyList.getLeadKeies().add(key2);

            JAXBElement<ArrayOfLeadKey> arrayOfLeadKey = objectFactory.createParamsRequestCampaignLeadList(leadKeyList);
            request.setLeadList(arrayOfLeadKey);

            SuccessRequestCampaign result = port.requestCampaign(request, header);

            JAXBContext context = JAXBContext.newInstance(SuccessRequestCampaign.class);
            Marshaller m = context.createMarshaller();
            m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
            m.marshal(result, System.out);

        }
        catch(Exception e) {
            e.printStackTrace();
        }
    }
}
```

**¿Cómo me inscribo en espacios de trabajo y particiones de posibles clientes?** Para los clientes de Marketo Enterprise, tiene acceso libre a espacios de trabajo y particiones de posibles clientes. Póngase en contacto con el administrador de activación de clientes para obtener más información sobre cómo habilitarlos e implementarlos. Para otros clientes, póngase en contacto con su ejecutivo de ventas de Marketo o envíe un correo electrónico a nuestro equipo de ventas para obtener más información sobre este producto.

Este artículo contiene el código utilizado para implementar integraciones personalizadas. Debido a su naturaleza personalizada, el equipo de soporte técnico de Marketo no puede solucionar problemas del trabajo personalizado. No intente implementar el siguiente código de ejemplo sin tener la experiencia técnica adecuada o sin tener acceso a un desarrollador experimentado.

Publicado el _1970-01-01_ por _Murta_

## Actualizaciones de la versión de junio de 2014

### API de Marketo Real-Time Personalization

La API de JavaScript de Marketo Real-Time Personalization (RTP) amplía la capacidad de personalización automatizada de la plataforma. Permite el seguimiento de eventos y la personalización dinámica de una página web. Consulte la documentación completa [aquí](/help/javascript-api/web-personalization.md).

Publicado el _2014-06-20_ por _Travis Kaufman_

## Almacenamiento de una clave externa en Marketo

Al sincronizar registros de contactos y posibles clientes entre sistemas, como un CRM propietario o un almacén de datos, es un requisito común asociar un registro de posibles clientes con un identificador de sistema único. En Marketo, puede crear o actualizar un registro de posibles clientes a través de una llamada de la API [syncMultipleLeads](/help/soap-api/syncmultipleleads.md) usando su identificador único del sistema. Para ello, almacenaría el identificador único del sistema (clave principal) como una clave externa en Marketo. El nombre de este campo en Marketo para almacenar una clave externa es ForeignSysPersonId. Hay tres cosas importantes que hay que tener en cuenta:

1. ForeignSysPersonId no está visible en la interfaz de usuario de Marketo. Por lo tanto, se recomienda rellenar también un campo de atributo personalizado con este valor.
1. ForeignSysPersonId es único para un posible cliente, pero un posible cliente puede tener más de un ForeignSysPersonId.
1. ForeignSysPersonId no se puede actualizar ni eliminar, pero se puede reasignar a otro registro.

Se muestra cómo realizar una llamada a la API [syncMultipleLeads](/help/soap-api/syncmultipleleads.md) para escribir un valor ForeignSysPersonId en dos registros de posibles clientes existentes en Marketo. **Cómo escribir ForeignSysPersonId mediante la API syncMultipleLeads** Puede insertar un nuevo registro de posibles clientes y especificar ForeignSysPersonId. También puede agregarlo a un posible cliente existente especificando Marketo ID y ForeignSysPersonId. Le guiaremos a través del último caso. **Solicitar XML para la llamada a la API de SOAP syncMultipleLeads**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809934544BFABAE58E5D27</mktowsUserId>
      <requestSignature>b5e21953ae9f1b263e644da5eccce9ff33802513</requestSignature>
      <requestTimestamp>2013-08-01T18:22:30-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsSyncMultipleLeads>
      <leadRecordList>
        <leadRecord>
          <leadId>1090240</leadId>
          <foreignSysPersonId>1224191</foreignSysPersonId>
          <leadAttributeList>
            <attribute>
              <attrName>Company</attrName>
              <attrValue>Marketo1000</attrValue>
            </attribute>
            <attribute>
              <attrName>Phone</attrName>
              <attrValue>650-555-1000</attrValue>
            </attribute>
          </leadAttributeList>
        </leadRecord>
        <leadRecord>
          <leadId>1090239</leadId>
          <foreignSysPersonId>1224192</foreignSysPersonId>
          <leadAttributeList>
            <attribute>
              <attrName>Company</attrName>
              <attrValue>Marketo1001</attrValue>
            </attribute>
            <attribute>
              <attrName>Phone</attrName>
              <attrValue>650-555-1001</attrValue>
            </attribute>
          </leadAttributeList>
        </leadRecord>
      </leadRecordList>
      <dedupEnabled>true</dedupEnabled>
    </ns1:paramsSyncMultipleLeads>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

**XML de respuesta para la llamada a la API de SOAP syncMultipleLeads**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:xsi="<http://www.w3.org/2001/XMLSchema-instance>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successSyncMultipleLeads>
      <result>
        <syncStatusList>
          <syncStatus>
            <leadId>1090240</leadId>
            <status>UPDATED</status>
            <error xsi:nil="true" />
          </syncStatus>
          <syncStatus>
            <leadId>1090239</leadId>
            <status>UPDATED</status>
            <error xsi:nil="true" />
          </syncStatus>
        </syncStatusList>
      </result>
    </ns1:successSyncMultipleLeads>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

**Vea a continuación un ejemplo de programa Ruby que mostrará el XML de solicitud más arriba.**

```java
require 'savon' # Use version 2.0 Savon gem
require 'date'

mktowsUserId = "" # CHANGE ME
marketoSecretKey = "" # CHANGE ME
marketoSoapEndPoint = "" # CHANGE ME
marketoNameSpace = "<http://www.marketo.com/mktows/>"

# Create Signature
Timestamp = DateTime.now
requestTimestamp = Timestamp.to_s
encryptString = requestTimestamp + mktowsUserId
digest = OpenSSL::Digest.new('sha1')
hashedsignature = OpenSSL::HMAC.hexdigest(digest, marketoSecretKey, encryptString)
requestSignature = hashedsignature.to_s

# Create SOAP Header
headers = {
 'ns1:AuthenticationHeader' => { "mktowsUserId" => mktowsUserId, "requestSignature" => requestSignature,
 "requestTimestamp"  => requestTimestamp
 }
}

client = Savon.client(wsdl: '<http://app.marketo.com/soap/mktows/2_3?WSDL>', soap_header: headers, endpoint: marketoSoapEndPoint, open_timeout: 90, read_timeout: 90, namespace_identifier: :ns1, env_namespace: 'SOAP-ENV')

# Create Request
request = {
        :lead_record_list => {
                :lead_record => {
                        :lead_id => "1090240",
                        :foreign_sys_person_id => "1224191",
                :lead_attribute_list => {
                        :attribute => {
                                :attr_name => "Company",
                                :attr_value => "Marketo1000" },
                         :attribute! => {
                                :attr_name => "Phone",
                                :attr_value => "650-555-1000" }
                }
        },
        :lead_record! => {
                        :lead_id => "1090239",
                        :foreign_sys_person_id => "1224192",
                :lead_attribute_list => {
                        :attribute => {
                                :attr_name => "Company",
                                :attr_value => "Marketo1001" },
                         :attribute! => {
                                :attr_name => "Phone",
                                :attr_value => "650-555-1001" }
                }
        }
        },
    :dedup_enabled => "True"
}

response = client.call(:sync_multiple_leads, message: request)

puts response
```

Este artículo contiene el código utilizado para implementar integraciones personalizadas. Debido a su naturaleza personalizada, el equipo de soporte técnico de Marketo no puede solucionar problemas del trabajo personalizado. No intente implementar el siguiente código de ejemplo sin tener la experiencia técnica adecuada o sin tener acceso a un desarrollador experimentado.

Publicado el _2014-06-27_ por _Murta_

## Actualización de la dirección de correo electrónico de un posible cliente

Supongamos que un usuario rellena un formulario de Marketo en el sitio. ¿Qué pasa? Marketo cookies del usuario y las asocia con el correo electrónico que ha proporcionado. ¿Qué sucede si la próxima vez que el usuario visita su sitio web vuelve a rellenar el mismo formulario con un correo electrónico diferente? ¿Qué pasará? Marketo creará un nuevo registro de posibles clientes y sobrescribirá la primera cookie en el explorador del usuario. El usuario es ahora un posible cliente nuevo/diferente en Marketo. Le mostramos cuatro maneras de actualizar la dirección de correo electrónico de un posible cliente en Marketo, incluido el [método de API syncLead](/help/soap-api/synclead.md), el campo personalizado en un método de formulario, la interfaz de usuario de Marketo y la importación de una lista. **A través de la API syncLead**. Puedes usar la [API syncLead](/help/soap-api/synclead.md) para actualizar un registro de posible cliente usando su Marketo ID y la nueva dirección de correo electrónico. Solicitar XML para `syncMultipleLeads` llamada de API de SOAP

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>bigcorp1_461839624B16E06BA2D663</mktowsUserId>
      <requestSignature>92f05a7be4838ae1c0e5aafe814891ee72968a08</requestSignature>
      <requestTimestamp>2013-07-31T12:38:47-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsSyncLead>
      <leadRecord>
        <leadId>1090240</leadId>
        <Email>t@t.com</Email>
      </leadRecord>
      <returnLead>false</returnLead>
    </ns1:paramsSyncLead>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

XML de respuesta para la llamada a la API de SOAP syncMultipleLeads

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:xsi="<http://www.w3.org/2001/XMLSchema-instance>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successSyncLead>
      <result>
        <leadId>1090240</leadId>
        <syncStatus>
          <leadId>1090240</leadId>
          <status>UPDATED</status>
          <error xsi:nil="true" />
        </syncStatus>
        <leadRecord xsi:nil="true" />
      </result>
    </ns1:successSyncLead>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Consulte a continuación un ejemplo de programa Ruby que generará el XML de solicitud anterior.

```java
require 'savon' # Use version 2.0 Savon gem
require 'date'

mktowsUserId = "" # CHANGE ME
marketoSecretKey = "" # CHANGE ME
marketoSoapEndPoint = "" # CHANGE ME
marketoNameSpace = "<http://www.marketo.com/mktows/>"

# Create Signature
Timestamp = DateTime.now
requestTimestamp = Timestamp.to_s
encryptString = requestTimestamp + mktowsUserId
digest = OpenSSL::Digest.new('sha1')
hashedsignature = OpenSSL::HMAC.hexdigest(digest, marketoSecretKey, encryptString)
requestSignature = hashedsignature.to_s

# Create SOAP Header
headers = {
 'ns1:AuthenticationHeader' => { "mktowsUserId" => mktowsUserId, "requestSignature" => requestSignature,
 "requestTimestamp"  => requestTimestamp
 }
}

client = Savon.client(wsdl: '<http://app.marketo.com/soap/mktows/2_3?WSDL>', soap_header: headers, endpoint: marketoSoapEndPoint, open_timeout: 90, read_timeout: 90, namespace_identifier: :ns1, env_namespace: 'SOAP-ENV')

# Create Request
request = {
 :lead_record => {
  :Email => "<t@t.com>",
  :lead_id => "1090240",
 :return_lead => "false"
}

response = client.call(:sync_lead, message: request)

puts response
```

**Mediante un campo personalizado en un formulario** Puede crear un campo personalizado para la &quot;Nueva dirección de correo electrónico&quot; en Marketo. A continuación, pida al usuario que rellene un formulario que incluya este nuevo campo. A continuación, cree un programa en Marketo que cambie el valor de los datos del campo de dirección de correo electrónico del sistema con el token `{{lead.newEmailAddress}}` cuando se produzca un cambio en el nuevo campo personalizado &quot;Nueva dirección de correo electrónico&quot;. **A través de la interfaz de usuario de Marketo**. Puedes actualizar manualmente la dirección de correo electrónico de un posible cliente a través de la interfaz de usuario de Marketo. Este es un [artículo de ayuda](https://nation.marketo.com/) que describe cómo hacerlo (se requiere el inicio de sesión de Marketo para ver el artículo). **Mediante la importación de una lista** Puede actualizar la dirección de correo electrónico de un posible cliente mediante el método import a list en Marketo descrito [aquí](https://nation.marketo.com/) (se requiere el inicio de sesión de Marketo para ver el artículo).  

Este artículo contiene el código utilizado para implementar integraciones personalizadas. Debido a su naturaleza personalizada, el equipo de soporte técnico de Marketo no puede solucionar problemas del trabajo personalizado. No intente implementar el siguiente código de ejemplo sin tener la experiencia técnica adecuada o sin tener acceso a un desarrollador experimentado.

Publicado el _1970-01-01_ por _Murta_

## Almacenar una segunda dirección de correo electrónico para un posible cliente

Supongamos que desea cambiar la puntuación de un posible cliente en Marketo mediante las API. Esto es posible hacer con la API de REST mediante el punto de conexión de Crear/Actualizar posible cliente. Si desea almacenar más de un correo electrónico en un registro de posibles clientes, debe crear un campo personalizado y almacenar el segundo correo electrónico allí. Aquí tiene un artículo de ayuda con más información: A continuación se muestra un ejemplo de código en Ruby que muestra cómo realizar esta llamada.

```java
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com>"
endpoint = "/rest/v1/leads.json"
# Replace with your access token
auth_token =  "?access_token=" + "ac756f7a-d54d-41ac-8c3c-f2d2a39ee325:ab"
request_url = marketo_instance + endpoint + auth_token

# Build request body
data = { "action" => "updateOnly", "input" => [ { "email" => "<example@email.com>", "leadScore" => "30" } ] }

# Make request
response = RestClient.post request_url, data.to_json, :content_type => :json, :accept => :json

# Returns Marketo API response
puts response
```

Publicado el _2015-02-20_ por _Murta_

## Cree un campo personalizado en Marketo y actualice este campo mediante AP

Supongamos que tiene datos adicionales sobre los posibles clientes que no se ajustan a los campos estándar de Marketo. Por ejemplo, este campo personalizado podría ser una puntuación de terceros. Puede crear un campo personalizado en Marketo para la puntuación de terceros y, a continuación, actualizar el valor de este campo mediante las API de Marketo [REST](https://developer.adobe.com/marketo-apis/) o [SOAP](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/soap/activity-type-filters). En primer lugar, se muestra cómo crear un campo personalizado en Marketo y, en segundo, cómo actualizar este campo mediante la API de REST.

### Cómo crear un campo personalizado en Marketo

1. En Administración, haga clic en Administración de campos.
1. Haga clic en el botón Nuevo campo personalizado.
1. Elija el campo Type. Esto cambia la forma en que se representa en las listas inteligentes y en Forms en Marketo.
1. Introduzca el nombre tal como desea que aparezca en Marketo. Elija el Nombre y el Nombre de la API cuidadosamente, ya que cambiar el nombre de los campos puede resultar difícil y, en algunas situaciones, no es posible.
1. Ahora se puede acceder al campo personalizado que ha creado a través de las API.

### Cómo actualizar el campo personalizado mediante la API de REST

En la sección anterior, creamos un campo personalizado llamado `myCustomField` con la cadena de tipo de datos. Para actualizar el valor de ese campo, se utiliza el punto final de la API de REST denominado Crear/actualizar posibles clientes. Para poder realizar una solicitud a la API de REST, debe autenticarse. Esto está fuera del ámbito de este artículo, pero la información detallada [está disponible en el sitio de desarrolladores de Marketo](/help/rest-api/authentication.md).

**Extremo**

`/rest/v1/leads.json`

**Cuerpo de solicitud**

```json
{
   "action":"createOrUpdate",
   "input":[
      {
         "email":"<example@example.com>",
         "myCustomField":"examplestring"
      }
   ]
}
```

Este artículo contiene el código utilizado para implementar integraciones personalizadas. Debido a su naturaleza personalizada, el equipo de soporte técnico de Marketo no puede solucionar problemas del trabajo personalizado. No intente implementar el siguiente código de ejemplo sin tener la experiencia técnica adecuada o sin tener acceso a un desarrollador experimentado.

Publicado el _2014-08-19_ por _Murta_

## Integración de Unbounce y Marketo

**NOTA: Este es un post de Fab Capodicasa. Es consultor certificado por Marketo en [Hoosh Marketing](http://hooshmarketing.com.au/), un socio de agencia de Marketo LaunchPoint especializado en B2C. Ha trabajado tanto en SaaS como en marketing durante los últimos 13 años. Su experiencia es una mezcla de TI de alta calidad, marketing directo y ventas empresariales. Fab también es un antiguo empleado de Marketo.**

**Información general** En este artículo demostramos cómo integrar Unbounce, una herramienta popular de página de aterrizaje, con Marketo. En primer lugar, se muestra cómo insertar el seguimiento de Marketo en Unbounce y, en segundo, cómo modificar los formularios Unbounce para insertar datos directamente en Marketo. El desafío de integrar Unbounce con Marketo es que Unbounce no permite cambiar el nombre de los campos predeterminados (por ejemplo, el nombre no se puede cambiar a FirstName). Tampoco permite que las etiquetas de campo sean diferentes de los nombres de campo. Esta integración incluye JavaScript, que modifica los formularios existentes para hacerlos compatibles con Marketo. Le recomiendo que tenga al menos un nivel de principiante de JavaScript y un nivel intermedio de conocimientos de Marketo para completar las tareas de este artículo.
**Parte 1: Agregar código de seguimiento de Marketo para anular la devolución** Se requiere agregar el script de seguimiento de Munchkin de Marketo a las páginas de anulación de la devolución para que funcionen tanto el análisis como la integración del formulario. Siga estos pasos: Copie el código de Munchkin de Marketo: Vaya a Administración -> Munchkin y copie la versión &quot;Simple&quot; de JavaScript. Abra la página de aterrizaje de Devolución y haga clic en JavaScript-> Añadir nuevo JavaScript.  Haga clic en Añadir, llame al script &quot;Munchkin&quot;, seleccione &quot;Antes de la etiqueta de fin de cuerpo&quot; y pegue el código Munchkin. Haga clic en el botón Listo. Para futuras páginas de Unbounce, vaya a JavaScript y habilite el script de Munchkin que hemos creado. No hay necesidad de volver a crearlo.
**Parte 2: convertir el formulario de devolución a un formulario de Marketo** Ahora necesitamos modificar el formulario de devolución agregando algunos campos ocultos nuevos y JavaScript para permitir que las páginas de aterrizaje de devolución envíen información de posibles clientes directamente a Marketo. Primero crearemos un formulario de marcador de posición de Marketo. En Marketo, cree un formulario en blanco y apruébelo.

Este es el formulario proxy de Marketo que representa el formulario de devolución. Agregue campos ocultos al formulario de devolución. Marketo requiere estos campos ocultos para determinar a qué instancia de Marketo, qué formulario y qué sesión de usuario se aplicará este envío de formulario. En No devolver, abra el formulario haciendo doble clic en. Agregue un campo oculto denominado `_mkt_trk`. Agregue un segundo campo oculto denominado `formid`.  233 debe reemplazarse por el id del formulario, que se puede encontrar en el código incrustado del formulario de Marketo en Marketo. En Marketo, abra el formulario y seleccione Acciones de formulario->Código incrustado. Agregue un campo oculto denominado `returnurl`. `http://hooshmarketing.com.au/thank-you` debe reemplazarse con una dirección URL de seguimiento; es la dirección URL a la cual desea que se redirija a los usuarios después de enviar el formulario. Por ejemplo, esta podría ser su página de agradecimiento.
**Parte 3: Enviar formulario sin rechazos directamente a Marketo** La URL de seguimiento es la página a la que se redirigirá al posible cliente una vez que se envíe el posible cliente a Marketo. En Unbounce, siga estos pasos: Haga clic en su formulario. Modifique la sección Confirmación del formulario. Cambie la confirmación para publicar datos de formulario en una dirección URL. Establezca la dirección URL de la página de seguimiento que desee. `fpmarkets` debe reemplazarse con la cadena de cuenta de Marketo, que se encuentra en Marketo en Administración->Páginas de aterrizaje.
**Parte 4: Agregar JavaScript a la página de devolución** Este JavaScript convertirá el formulario para que sea compatible con Marketo y lo enviará a Marketo. En Unbounce, siga estos pasos: Abra la página de aterrizaje de Unbounce y haga clic en JavaScript-> Añadir nuevo JavaScript. Haga clic en Agregar, llame al script &#39;Marketo Form Convert&#39; y seleccione &#39;Antes de la etiqueta de fin de cuerpo&#39;. Pegue el código JavaScript a continuación:

```javascript
var MARKETO_MUNCHKIN_ID='614-CGT-700';
var MARKETO_ACCOUNT_STRING = 'fpmarkets';

var UNBOUNCE_MARKETO_FIELD_MAP = new Object();

//default field mappings
UNBOUNCE_MARKETO_FIELD_MAP['first_name'] = 'FirstName';
UNBOUNCE_MARKETO_FIELD_MAP['email'] = 'Email';
UNBOUNCE_MARKETO_FIELD_MAP['last_name'] = 'LastName';
UNBOUNCE_MARKETO_FIELD_MAP['phone_number'] = 'Phone';

function getMarketoField(k) {
    return UNBOUNCE_MARKETO_FIELD_MAP[k];
}


var formFields = [];
var hiddenClonedFields = [];
var firstForm = document.forms[0];

//Convert Unbounce form names to Marketo field names
for(i=0; i<firstForm.elements.length; i++){
 var formField = firstForm.elements[i];
 var newFieldName = getMarketoField(formField.name);

  if(newFieldName != undefined) {


    //save original field as hidden field
    var hiddenField = document.createElement("input");
    hiddenField.setAttribute("type", "hidden");
    hiddenField.setAttribute("name", formField.name);
    hiddenField.setAttribute("id", formField.id);
    hiddenClonedFields.push(hiddenField);

    //change original field
    console.log ( 'Changed form field name: ' + formField.name + '=>' + newFieldName );
    formField.name = newFieldName;
    formField.id = newFieldName;
    formFields.push(formField);


  } else {
    console.log ( 'Couldn't map:' + formField.name );
  }
}

//Add hidden cloned Unbounce fields to form
//for Unbounce validation
for(i=0; i<hiddenClonedFields.length; i++){
    firstForm.appendChild(hiddenClonedFields[i]);
    formFields[i].onchange = (function(hf) {
        return function(event) {
            hf.value = event.target.value;
          console.log('Changed hidden cloned field:' + hf.name + '=>' + hf.value);
        };
    }(hiddenClonedFields[i]));
    console.log ( 'Added cloned field: ' + hiddenClonedFields[i].name );
}


//Add MunchkinId to form
var input = document.createElement("input");
input.setAttribute("type", "hidden");
input.setAttribute("name", "munchkinId");
input.setAttribute("value", MARKETO_MUNCHKIN_ID);
firstForm.appendChild(input);
console.log('Added hidden field:' + input.name + '=' + input.value);
```

Si tiene campos personalizados con nombres de API que no son compatibles con Unbounce, puede añadirlos a la asignación en JavaScript. Por ejemplo, si uno de los campos personalizados en Marketo era `Comments_c`, pero desea que la etiqueta del campo aparezca como Comentarios, Unbounce no le permitiría cambiar el nombre del campo a `Comments_c`.

```
//default field mappings
UNBOUNCE_MARKETO_FIELD_MAP['comments'] = 'Comments_c';
UNBOUNCE_MARKETO_FIELD_MAP['first_name'] = 'FirstName';
UNBOUNCE_MARKETO_FIELD_MAP['email'] = 'Email';
```

_comments es el nombre del campo en Unbounce. _Comments_c_ es el nombre del campo en Marketo. Para futuras páginas de Unbounce, solo tiene que ir a JavaScript y activar el script de Munchkin que hemos creado. No hay necesidad de volver a crearlo.
**Parte 5: Probando** El último paso es probar que esta integración del formulario funciona. Cree un déclencheur en Marketo que se active al rellenar el formulario de Marketo y asegúrese de que los posibles clientes se inserten correctamente en Marketo. Una vez enviado el formulario, la página debe redirigirle a la dirección URL de seguimiento.

Publicado el _2014-08-04_ por _

## Actualizaciones de la versión de julio de 2014

### Actualizaciones de Munchkin

La nueva versión de Munchkin es 141. La versión 142 no es compatible y se eliminará a principios de septiembre de 2011. En la versión 142, había funciones de acceso público que no estaban documentadas en el sitio de los desarrolladores. Esas funciones indocumentadas ya no están disponibles públicamente. Las funciones documentadas en el sitio de los desarrolladores seguirán siendo compatibles a largo plazo.

### Actualizaciones de RTP

La API de RTP tiene una nueva función denominada Obtener datos del visitante. Esta llamada a la API de RTP le permite obtener datos de visitantes en tiempo real, como organización, sector, ubicación y coincidencia de código de segmento.

Publicado el _30-07-2014_ por _Murta_

## Uso de varios códigos de seguimiento de Munchkin en una sola página

Supongamos que tiene varias instancias de Marketo y que desea enviar eventos de seguimiento web como visitas a la página o vínculos en los que se hizo clic a estas instancias múltiples; es posible hacerlo con Munchkin. Marketo realiza un seguimiento de los visitantes del sitio web por dominio (por ejemplo, &quot;marketo.com&quot;). Si aloja este script de Munchkin en un dominio que es diferente al dominio principal (por ejemplo, &quot;company.com&quot;), esos visitantes aparecerán como posibles clientes anónimos hasta que rellenen un formulario en ese otro dominio. Para ello, agregue el parámetro `altIds` a la llamada `Munchkin.init`. El parámetro altIds contiene una matriz de los Munchkin ID adicionales a los que se deben enviar los eventos web. Con el ejemplo siguiente, reemplace los Munchkin ID resaltados (XXX-XXX-XXX, AAAA-AAAA y ZZZ-ZZZ-ZZZ) por los Munchkin ID de cada instancia de Marketo a la que se deba enviar la información de seguimiento.

```javascript
<script src="http://munchkin.marketo.net/munchkin.js" type="text/javascript"></script>
<script>Munchkin.init('XXX-XXX-XXX', { altIds:['YYY-YYY-YYY', 'ZZZ-ZZZ-ZZZ'] });</script>
```

Para obtener información adicional sobre los parámetros de inicialización de Munchkin, consulte [este documento](/help/javascript-api/configuration.md).

Publicado el _2014-08-08_ por _Murta_

## Integración de Munchkin con Google Tag Manager

Google Tag Manager permite añadir etiquetas a un sitio web. En lugar de agregar manualmente cada script de seguimiento como Munchkin al código fuente de tu sitio web, puedes colocar [Google Tag Manager](https://marketingplatform.google.com/about/tag-manager/) en tu sitio y luego agregar etiquetas como [Munchkin](/help/javascript-api/lead-tracking.md) a través de la interfaz de usuario de Google Tag Manager. En esta publicación, primero se muestra cómo generar el código de seguimiento de Munchkin en Marketo y, a continuación, se muestra cómo agregar este código de seguimiento de Munchkin a Google Tag Manager.

### Cómo generar un código de seguimiento de Munchkin

1. Haga clic en **Administrador** en la parte superior derecha de la aplicación.
1. Haga clic en **Munchkin** en el árbol de la izquierda.
1. Seleccione **Asincrónica** para el tipo de código de seguimiento.
1. Haga clic en y copie el código de seguimiento de JavaScript.

**Cómo agregar código de seguimiento de Munchkin al administrador de etiquetas de Google**

1. Inicie sesión en su cuenta de Google Tag Manager y **agregue una etiqueta nueva.**
1. Crear una nueva **etiqueta personalizada de HTML**.
1. Copie y pegue su código Munchkin en el campo **HTML** y haga clic en **Continuar**.
1. Seleccione **Activar todas las páginas** y haga clic en **Crear etiqueta.** Nota: si tienes un sitio web con mucho tráfico, puedes excluir secciones de tu sitio usando **Activar algunas páginas**.
1. Haga clic en Guardar y, a continuación, compruebe que el código de seguimiento de Munchkin se está cargando en el sitio web.

Este artículo contiene el código utilizado para implementar integraciones personalizadas. Debido a su naturaleza personalizada, el equipo de soporte técnico de Marketo no puede solucionar problemas del trabajo personalizado. No intente implementar el siguiente código de ejemplo sin tener la experiencia técnica adecuada o sin tener acceso a un desarrollador experimentado.

Publicado el _2014-08-05_ por _Murta_

## Ocultar Lightbox después del envío del formulario

### Información general

El código incrustado actual de lightbox generado por Marketo no desaparece cuando se envía el formulario. Se volverá a cargar la página después del envío del formulario y este volverá a aparecer. Si desea crear una light box que se oculte después del envío del formulario, siga los pasos a continuación.

### Guía de introducción

1. Después de crear el formulario en Marketo, genere el código incrustado. Para ello, haga clic en Acciones de formulario y, a continuación, haga clic en Lightbox como tipo de código. Copie y pegue este código en un editor de texto para poder editarlo en el siguiente paso.
1. Reemplace todo el código después de &quot;(formulario)&quot; en su código incrustado con el siguiente código:

```javascript
{
var lightbox = MktoForms2.lightbox(form).show();
form.onSuccess(function(){
lightbox.hide();
return false;
});
});
</script>
```

Después del paso 2, la versión completada debe ser similar al código siguiente. El código ya está listo para utilizarse en el sitio web.

```javascript
<script src="http://app-sj04.marketo.com/js/forms2/js/forms2.js"></script>
<form id="mktoForm_0000"></form>
<script>MktoForms2.loadForm("http://app-sj04.marketo.com", "AAA-BBB-CCC", 0000, function (form){
var lightbox = MktoForms2.lightbox(form).show();
form.onSuccess(function(){
lightbox.hide();
return false;
});
});
</script>
```

Este artículo contiene el código utilizado para implementar integraciones personalizadas. Debido a su naturaleza personalizada, el equipo de soporte técnico de Marketo no puede solucionar problemas del trabajo personalizado. No intente implementar el siguiente código de ejemplo sin tener la experiencia técnica adecuada o sin tener acceso a un desarrollador experimentado.

Publicado el _2014-08-21_ por _Murta_

## Guía rápida para la API de REST de Marketo

Esta guía muestra cómo realizar la primera llamada a la API de REST de Marketo en diez minutos. Le mostramos cómo recuperar un solo posible cliente mediante el punto final de la API REST [Obtener posible cliente por identificador](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET). Para ello, le guiaremos a través del proceso de autenticación para generar un token de acceso, que utilizará para realizar una petición HTTP GET a [Obtener posible cliente por id.](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET). A continuación, le proporcionamos el código para realizar la solicitud que devuelve información de posibles clientes con formato JSON.

### Cómo generar un token de autenticación

>[!NOTE]
> A partir de junio de 2025, el token de autenticación ya no es compatible. Debe utilizar el encabezado Autenticación.
>

Un servicio personalizado en Marketo le permite describir y definir a qué datos tendrá acceso su aplicación. Debe iniciar sesión como administrador de Marketo para crear un servicio personalizado y asociar ese servicio con un solo usuario de solo API.

1. Vaya al área de administración de la aplicación de Marketo.
1. Haga clic en el nodo Usuarios y funciones del panel izquierdo.
1. Cree una función nueva. Muestre la lista de permisos de funciones haciendo clic en API de acceso. Ahora, desplácese hacia abajo y seleccione solo los permisos que necesite. En este caso, simplemente seleccionamos el permiso de solo lectura como posible cliente.
1. El siguiente paso es crear un usuario solo de API y asociarlo a la función de API que creó en el paso anterior. Puede hacerlo marcando la casilla de verificación del usuario Solo API en el momento de la creación del usuario.
1. Se requiere un servicio personalizado para identificar de forma exclusiva la aplicación cliente. Para crear una aplicación personalizada, vaya a la pantalla Admin > LaunchPoint y cree un nuevo servicio.
1. Proporcione el Nombre para mostrar, elija el tipo de servicio &quot;Personalizado&quot;, proporcione la Descripción y la dirección de correo electrónico del usuario creada en el paso 1. Se recomienda utilizar un nombre para mostrar descriptivo que represente la empresa o el propósito de este servicio de API de REST personalizado.
1. Haga clic en el vínculo &quot;Ver detalles&quot; de la cuadrícula para obtener el ID de cliente y el Secreto del cliente. La aplicación cliente puede utilizar el ID de cliente y el Secreto de cliente para generar un token de acceso.
1. Copie y pegue el token de autenticación en un editor de texto. Su token de autenticación tiene un aspecto similar al ejemplo siguiente:

`cdf01657-110d-4155-99a7-f986b2ff13a0:int`

### Cómo determinar la dirección URL del extremo

Al realizar una solicitud a la API de Marketo, debe especificar la instancia de Marketo en la dirección URL del extremo. Todas las solicitudes de API no masivas a la API de REST de Marketo seguirán el formato siguiente:

`<REST API Endpoint URL>/rest/`

La URL de extremo de API de REST se puede encontrar en el panel Administración de Marketo > Servicios web. La estructura de la URL del punto de conexión de Marketo debe ser similar al ejemplo siguiente:

`https://100-AEK-913.mktorest.com/rest/v1/lead/{id}.json`

**Nota: Las direcciones URL de API en lotes no tienen el prefijo &#39;/rest/&#39;. Para la API en lote, solo debe utilizar el host y anexar la ruta de acceso adecuada como se muestra a continuación:**

`https://100-AEK-913.mktorest.com/bulk/v1/leads/export/create.json`

### Cómo utilizar el token de autenticación para llamar a Get Lead by Id AP

En las secciones anteriores, generamos un token de autenticación y encontramos la dirección URL del punto de conexión. Ahora haremos una solicitud a un extremo de API de REST llamado [Obtener posible cliente por Id.](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET). La forma más sencilla de realizar la solicitud a la API de REST de Marketo es pegar la URL en la barra de direcciones del explorador web. Siga el formato siguiente:

`https://<REST API Endpoint URL for your Marketo instance>/rest/v1/<API that you are calling>?access_token=<access_token>`

### Ejemplo

`https://100-AEK-913.mktorest.com/rest/v1/lead/318581.json?access_token=cdf01657-110d-4155-99a7-f986b2ff13a0:int`

Si la llamada se realiza correctamente, devuelve JSON con el formato siguiente:

```json
{
    "requestId": "d82a#14e26755a9c",
    "result": [
        {
            "id": 318581,
            "updatedAt": "2015-06-11T23:15:23Z",
            "lastName": "Doe",
            "email": "<jdoe@marketo.com>",
            "createdAt": "2015-03-17T00:18:40Z",
            "firstName": "John"
        }
    ],
    "success": true
}
```

Si está interesado en obtener más información sobre las API de REST de Marketo, este es un [buen lugar para comenzar](https://developer.adobe.com/marketo-apis/).

Publicado el _04-09-2015_ por _David_

## Eliminar la cookie de seguimiento de Marketo

Pregunta: &quot;Hemos configurado un formulario en nuestro sitio web para que el equipo de ventas cancele la suscripción de las personas que han solicitado verbalmente que se les elimine de cualquier mensaje de correo electrónico. Sin embargo, lo que encontramos es que cuando ingresan en el correo electrónico y envían que están cookies con esa dirección de correo electrónico y comienzan a recibir alertas por su actividad en nuestro sitio web. El formulario que tenemos actualmente es un formulario incrustado. ¿Alguien ha descubierto una manera de deshabilitar el seguimiento de cocinero en un formulario incrustado?, entiendo cómo hacerlo con una página de aterrizaje de Marketo&quot;. Podemos hacer esto. Para implementar esto, acelere la caducidad de la cookie. Una vez que el formulario crea la cookie, puede forzar su caducidad inmediata. Como la cookie ha caducado, el explorador la eliminará automáticamente. Para iniciar este proceso, vamos a usar la funcionalidad de formulario nativo para llamar a en la función llamada `deleteCookie`.

Publicado el _2014-08-26_ por _Travis Kaufman_

## Integración de una página de aterrizaje de Marketo con Wordpress

Supongamos que el sitio web se ha creado con Wordpress y que desea incrustar una página de aterrizaje de Marketo en una de las páginas. Esto es posible mediante un iframe. Un iframe le permite incrustar una página dentro de otra página. En esta publicación, se muestra cómo incrustar una página de aterrizaje de Marketo en una página de Wordpress. **Elegir un complemento de Wordpress** Wordpress Hay varios Aquí hay un complemento de Wordpress que te permite: `http://wordpress.org/plugins/iframe/` Aquí hay algunos props y estafas sobre el uso de este enfoque: `http://www.elixiter.com/marketo-landing-page-and-form-hosting-options/` Gran post Murtza! Colby, el iframe conservaría la parte en la que después de enviar el formulario se le redirige a una PDF. Usamos iframes en nuestro sitio durante mucho tiempo. Gran post Murtza! Colby, el iframe conservaría la parte en la que después de enviar el formulario se le redirige a una PDF. Usamos iframes en nuestro sitio durante mucho tiempo. Tenga en cuenta que si desea pasar parámetros de URL desde la URL principal al iframe, debe codificar. Además, asegúrese de que Google Analytics está configurado correctamente. No quiere que las vistas de página se cuenten dos veces por cada visita a la página con el iframe.

Publicado el _1970-01-01_ por _Murta_

## Integración optimizada con una página de aterrizaje de Marketo

[Optimizely](https://www.optimizely.com/) le ofrece la capacidad de realizar pruebas A/B, multipágina y multivariable en su sitio. Puede utilizar Optimizely con una página de aterrizaje de Marketo. A continuación se indica cómo hacerlo:

1. Busque y copie el fragmento de código de Optimizely.** Vaya al panel en Optimizely y haga clic en el vínculo Código de proyecto. Copie la línea de código que aparece en la ventana emergente.
1. Inicie sesión en Marketo y seleccione la plantilla de página de aterrizaje. Luego haga clic en &quot;Editar borrador&quot;.**
1. Haga clic en Acciones de página de aterrizaje. A continuación, haga clic en Editar etiquetas Meta de página**
1. Pegue el fragmento de código de Optimizely en la sección Custom HEAD HTML y haga clic en Guardar.
1. Pruebe la página de aterrizaje para confirmar que el fragmento de Optimizely funciona

Publicado el _2014-09-18_ por _Murta_

## Buscar por nombre completo mediante la API de REST de Marketo

**Pregunta:** ¿Hay alguna manera de consultar un posible cliente usando las API de Marketo con solo el nombre completo de un posible cliente?
**Respuesta:** No es directamente posible. Sin embargo, la solución que se describe a continuación le permite hacerlo.

1. Cree un campo personalizado llamado &quot;Nombre completo&quot; en Marketo.
1. Use la API de SOAP [getMultipleLeads](/help/soap-api/getmultipleleads.md) o [Obtener varios posibles clientes por tipo de filtro](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET) para consultar la base de datos de posibles clientes. Incluya su nombre y apellidos como atributos en la solicitud a las API de REST o SOAP.
1. Después de consultar la base de datos de posibles clientes, concatene &quot;Nombre&quot; y &quot;Apellidos&quot; para cada posible cliente y almacene estos datos en una columna &quot;Nombre completo&quot;. 1. Use la API de SOAP [syncMultipleLeads](/help/soap-api/syncmultipleleads.md) para insertar estos datos en el campo personalizado &quot;Fullname&quot;. También puede usar la API [Importar posible cliente](/help/rest-api/leads.md) o importar un CSV o XLS usando la interfaz de usuario de Marketo.
1. Ahora puede consultar por nombre completo usando la API [Obtener varios posibles clientes por tipo de filtro](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) para buscar este campo personalizado. Especifique &quot;Fullname&quot; como &quot;filterType&quot; y &quot;filterValue&quot; como &quot;Joe Johnson&quot; con una llamada de API Get Multiple Leads by Filter Type REST.

Publicado el _2014-09-09_ por _Murta_

## Eliminar cookie de seguimiento de Marketo

Este método solo debe utilizarse si se desea eliminar la cookie por completo.

Este ejemplo de código se puede utilizar para eliminar una cookie de seguimiento de Marketo del explorador de un usuario después del envío del formulario de Marketo. Funciona llamando al método `deleteMarketoCookie` después de que un usuario envíe un formulario. Este método caduca la cookie al establecer la fecha de caducidad en una fecha del pasado. El comportamiento predeterminado del explorador es eliminar esta cookie porque ha caducado.

Publicado el _2014-09-09_ por _Murta_

## Restringir los dominios de correo electrónico gratuitos al rellenar el formulario

Supongamos que desea restringir el envío por parte de los visitantes de un sitio de un formulario con un dominio de correo electrónico gratuito, como Gmail o Yahoo. Para ello, incluya la secuencia de comandos siguiente en el código fuente de la página con el formulario de Marketo. Comprueba si un usuario ha introducido un correo electrónico que no sea empresarial (Gmail, Hotmail, etc.) e impide el envío de formularios de Marketo si un usuario introduce uno. Puede ampliar la lista de dominios de correo electrónico bloqueados modificando la línea 9 para incluir los dominios que desea restringir.

Publicado el _2014-09-09_ por _Murta_

## Depuración de un campo al que no se puede acceder mediante la API

Si viene a este campo <https://nation.marketo.com/> Cuando intentamos actualizar el campo AnnualRevenue mediante la API de SOAP, la respuesta indica que el registro se actualiza; sin embargo, el campo AnnualRevenue no persiste en el cambio. Si intentamos actualizar el campo manualmente con la base de datos de posibles clientes, los cambios en este campo tampoco se mantienen. Compruebe si el campo está bloqueado en la administración. La otra cosa que puede suceder a veces es que puede ser un campo de cuenta sincronizado desde SFDC. Bloqueamos los cambios en esos campos de forma predeterminada porque SFDC es el sistema de registro para ellos en muchos casos. 1) Creado A Las 2) Marketo SFDC ID 3) Marketo Código Único 4) SFDC Tipo 5) Actualizado A Las 6) Urgencia 7) Prioridad 8) Fecha De Creación De Ventas

Publicado el _1970-01-01_ por _Murta_

## Añadir código personalizado a una página de aterrizaje de Marketo

Supongamos que desea agregar un script de seguimiento de terceros, como Google Analytics, a su página de aterrizaje de Marketo. Esto es posible a través de la IU de Marketo. De forma más general, puede agregar cualquier código personalizado (HTML, CSS, JavaScript) al aterrizaje de Marketo siguiendo las instrucciones que se indican a continuación.

1. Seleccione la página de aterrizaje y haga clic en Editar borrador.
1. Arrastre el cursor en el elemento HTML.
1. Introduzca el código personalizado y haga clic en Guardar.

Publicado el _2014-09-10_ por _Murta_

## Publicación de formulario del lado del servidor

`https://community.marketo.com/MarketoResource?id=kA650000000GsXXCA0`

Publicado el _2014-09-11_ por _Murta_

## Borrando la cookie de seguimiento de Marketo de los envíos de Forms 2.0

### Información general

Forms 1.0 contenía el valor de la cookie de seguimiento de Munchkin como un campo en el DOM. Esta información se presentó junto con todas las demás aportaciones. [Forms 2.0](/help/javascript-api/forms-api-reference.md) omite este campo y rellena dinámicamente el valor tras el envío en lugar de al cargar el formulario. Aunque esto es generalmente aceptable, sí crea una clase de casos de uso, que requieren que se borre la cookie de seguimiento para evitar un seguimiento y un relleno previo no deseados. Por ejemplo, esto puede ocurrir en una feria comercial en la que un cliente de Marketo utiliza el mismo formulario en el mismo dispositivo y obtiene información de contacto de varias personas. El siguiente fragmento de código le permite borrar el valor de la cookie al enviar el formulario, sin tener que eliminar la cookie en sí del explorador del usuario.

### Ejemplo de código

Este fragmento espera una sola carga de formulario en la página. Si hay varios formularios, debe usar los métodos [loadForm o getForm](/help/javascript-api/forms-api-reference.md) para implementar las llamadas de retorno.

```javascript
<script>
//add a callback to the first ready form on the page
MktoForms2.whenReady( function(form){
 //add the tracking field to be submitted
        form.addHiddenFields({"_mkt_trk":""});
        //clear the value during the onSubmit event to prevent tracking association
 form.onSubmit( function(form){
  form.vals({"_mkt_trk":""});
 })
})
</script>
```

Publicado el _2014-09-11_ por _Kenny_

## Actualizaciones de la versión de septiembre de 2014

### Actualizaciones de la API de REST

Se ha agregado un nuevo valor de campos opcionales a la API [Obtener varios posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) que devolverá los valores de cookies de Munchkin asociados con un registro de posibles clientes. Simplemente agregue &quot;?fields=cookies&quot; a la solicitud.

Publicado el _2014-09-16_ por _Murta_

## Añadir datos de ubicación de la API de RTP a Marketo Form Fill Out

**Necesita suscripciones activas a RTP y MLM para implementar el caso de uso descrito en esta publicación de blog.**
Con las [API de JavaScript RTP](/help/javascript-api/web-personalization.md) y [Marketo Forms 2.0](/help/javascript-api/forms-api-reference.md), puede extraer los datos de ubicación deducidos de RTP e insertarlos en Marketo mediante un formulario rellenado. Esto le permite ver la ubicación deducida del usuario (según la dirección IP) durante la actividad de formulario más reciente. Para empezar, debe crear tres campos de cadena personalizados en Marketo. Puede hacerlo a través de su CRM si tiene una integración nativa con Marketo, o desde el menú Administración de campos en la sección de administración de Marketo. Recomiendo nombrar estos campos &quot;País más reciente&quot;, &quot;Estado más reciente&quot; y &quot;Ciudad más reciente&quot;. Seguimos este blog usando esta convención de nomenclatura. Los nombres de API para estos campos son &#39;mostRecentCountry&#39;, &#39;mostRecentState&#39; y &#39;mostRecentCity&#39;. Para recuperar los datos de ubicación, se usa el método [RTP para obtener los datos de ubicación del visitante](/help/javascript-api/web-personalization.md) y, a continuación, pasarlos al formulario mediante los métodos [addHiddenFields y vals](/help/javascript-api/forms-api-reference.md) de Marketo Forms 2.0. En la página, agregue la etiqueta RTP JS y un formulario de Marketo. A continuación, incluya la secuencia de comandos siguiente. Debe cambiar los nombres de los campos del formulario de destino en el código de ejemplo si utiliza una convención de nombres diferente a la descrita anteriormente.

```javascript
<script>
//modify the form and grab the user
MktoForms2.whenReady( function(form) {
        //add the hidden fields to the form
 form.addHiddenFields({
  "mostRecentCountry":"",
  "mostRecentState":"",
  "mostRecentCity":""});
 //Grab the visitor data, a JS object with it is passed in the callback function of the third argument
 rtp('get','visitor',function(visitor){
  //add the desired info from the visitor object to the form fields
  form.vals({
   "mostRecentCountry":visitor.results.location.country,
   "mostRecentCity":visitor.results.location.city,
   "mostRecentState":visitor.results.location.state});
  }
  );
 });
</script>
```

Publicado el _2014-09-17_ por _Kenny_

## Bloqueo del rastreo y la indexación de búsqueda de una página de aterrizaje de Marketo

Supongamos que desea bloquear una página de aterrizaje de Marketo para que no se rastree y se muestre en los resultados por motores de búsqueda como Google. A continuación se indica cómo hacerlo:

1. Inicie sesión en Marketo y seleccione su página de aterrizaje. Luego haga clic en &quot;Editar borrador&quot;.
1. Haga clic en Acciones de página de aterrizaje. A continuación, haga clic en Editar etiquetas Meta de página
1. Cambie el campo Robots a Sin índice, Sin seguimiento. A continuación, haga clic en Guardar.

Publicado el _2014-09-19_ por _Murta_

## Preguntas frecuentes sobre Marketo REST y las API de SOAP

**Actualizado: marzo de 2016** Aquí encontrarás respuestas a las preguntas más frecuentes sobre las API de Marketo [REST](/help/rest-api/rest-api.md) y [SOAP](/help/soap-api/soap-api.md). **Q: ¿Cuáles son las principales diferencias entre las API de REST y SOAP de Marketo?** A: aunque la capacidad de insertar/extraer datos específicos mediante las API de REST y SOAP se superpone en su mayoría, existe cierta funcionalidad que solo existe en las API de REST o SOAP. En términos de rendimiento, la API de REST tiene un [rendimiento](https://en.wikipedia.org/wiki/Throughput) mejor que la API de SOAP. En cuanto al modelo de autenticación, la API de REST tiene un modelo de autenticación que utiliza un token que caduca. Nuestra API de REST también proporciona acceso a [recursos](https://developer.adobe.com/marketo-apis/api/asset/) de Marketo.   **Q: ¿Qué características están disponibles en la API de REST que no están disponibles en la API de SOAP?** A: [API de lista de listas](/help/rest-api/list-of-standard-fields.md), [quitar un posible cliente de una API de lista](/help/rest-api/lead-database.md), [API de uso](/help/rest-api/rest-api.md) y [API de error](/help/rest-api/rest-api.md) solo están disponibles con la API de REST. **Q: ¿Hay planes para aumentar el número de API disponibles para la API de SOAP?** A: No. **Q: ¿Hay planes para aumentar el número de API disponibles para la API de REST?** A: Sí. REST es el enfoque principal del desarrollo de API de Marketo en este momento.

Publicado el _2014-09-20_ por _Murta_

## Publicación de formulario del lado del servidor

**Nota: se trata de una API no pública y no admitida, no es compatible y su comportamiento puede cambiar en cualquier momento** Si utiliza sus propios formularios en el sitio web, aún puede enviar estos datos a Marketo mediante una publicación en el servidor. La ventaja de este método es que puede conservar la lógica de formulario y aplicación existente, pero puede seguir utilizando una publicación de formulario real en Marketo. Esto proporciona a los usuarios de Marketo un evento &quot;Rellena formulario&quot;, que se puede utilizar para almacenar en déclencheur procesos automatizados o para la segmentación. **NOTA: Hay un límite de 30 publicaciones de formulario del lado del servidor por minuto a partir de una sola dirección IP.** En cada lenguaje de programación o script existen diferentes opciones para realizar un envío de formulario del lado del servidor, y pueden tener diferentes objetos o métodos que se pueden utilizar para realizar la llamada posterior. Por ejemplo, en PHP mucha gente usa la biblioteca cURL. En todos los casos, está publicando pares de nombre-valor en una dirección URL especificada. Los nombres deben ser idénticos a los nombres de API de los campos de Marketo. Además, hay un par de campos del sistema que deben incluirse para capturar correctamente el envío del formulario.

1. Cree un formulario. El primer paso es crear un formulario en Marketo o utilizar un formulario existente que desee enviar. El nombre del formulario debe ser descriptivo, pero en realidad no necesita ningún campo de formulario. Si crea un nuevo formulario, simplemente introduzca un nombre, desmarque la casilla &quot;Abrir editor de formularios&quot; y habrá terminado.
1. Buscar un ID de formulario. En la interfaz de usuario de Marketo, seleccione el formulario y observe la dirección URL: debe tener el formato `https://app-x.marketo.com/#FO8B2ZN12`. Detrás del signo #, observe el número que sigue inmediatamente a &quot;FO&quot; para encontrar el ID del formulario. En este caso, el ID del formulario es 8. En algunos casos, su primer formulario puede estar numerado 1001 y contar desde allí. El ID del formulario es una variable, para que pueda almacenar en déclencheur el envío de distintos formularios.
1. Obtenga su ID de cuenta de Marketo. Vaya a Administración > Munchkin y copie el ID de cuenta de Munchkin, que tiene el formato 000-AAA-000. Lo necesita para que el formulario se envíe a la instancia de Marketo correcta.
1. Determine la URL de POST. En la interfaz de usuario de Marketo, anote el dominio en la barra de ubicación, normalmente con el formato `<http://app-x.marketo.com/>`. Descarte cualquier cosa después de la barra y luego añada &quot;index.php/leadCapture/save&quot; para obtener la URL del POST del formulario completo. Nota 1: distingue entre mayúsculas y minúsculas. Nota 2: Las zonas protegidas de Marketo pueden tener un dominio diferente al del sistema de producción de Marketo. Por lo tanto, una URL de ejemplo sería: `http://app-x.marketo.com/index.php/leadCapture/save` También puede utilizar HTTPS en lugar de HTTP (no utilice su CNAME, ya que proporciona una excepción de seguridad).
1. Busque los nombres de los campos de formulario** Vaya a Administración > Administración de campos y haga clic en el botón Exportar nombres de campos para descargar una hoja de cálculo con los nombres de los campos de API. Utilice el nombre de la API como nombre en sus pares nombre-valor.
1. Decida qué campos publicar. Puede incluir cualquier campo de posible cliente de Marketo en el envío del formulario. Tenga en cuenta que los nombres de campo distinguen entre mayúsculas y minúsculas. Además de los campos que desea enviar, hay dos campos obligatorios y dos campos recomendados: Campos obligatorios en el formulario: (1) `munchkinId` - Este campo se utiliza para el ID de cuenta de Munchkin (2) `formid` - Este campo indica qué formulario de Marketo se ha enviado Campos recomendados en el formulario: (1) Correo electrónico: este campo se utiliza como clave principal para la anulación de duplicación. Si Marketo encuentra una dirección de correo electrónico coincidente en la base de datos de Marketo, actualiza el registro existente; de lo contrario, crea un nuevo registro. Si hay varias coincidencias, se actualiza el registro actualizado más recientemente (2) `_mkt_trk`: este campo contiene la información de la cookie, por lo que puede realizar un seguimiento de las visitas a la página web de la persona. Si tiene Munchkin en la página del formulario, Munchkin escribirá automáticamente un valor en este campo de formulario oculto. Si no es así, léalo desde la cookie con el mismo nombre y páselo a Marketo en este campo. Nota: El cuerpo del POST de un formulario de Marketo debe tener codificación de dirección URL.
1. Consulte la respuesta** la respuesta a la publicación del formulario será un código de redirección HTTP 302. En algunos sistemas, esto aparece como un error. Sin embargo, en este caso significa que el posible cliente se ha creado o actualizado correctamente. Si se produce un error, recibirá un código de error 4xx o 5xx.

Aquí tiene una [publicación](https://nation.marketo.com:443/t5/product-blogs/how-to-build-an-external-subscription-center/ba-p/242185) sobre el uso de esta técnica para los escenarios de cancelación de suscripción [de Justin Cooperman, Sr. Product Manager]

Publicado el _2014-11-07_ por _Murta_

## Buscar posibles clientes actualizados en un intervalo de fechas específico

Supongamos que desea encontrar posibles clientes que se actualizaron en fechas específicas mediante la [API de Marketo](/help/soap-api/soap-api.md). Esto es posible con la [API de SOAP getMultipleLeads](/help/soap-api/getmultipleleads.md). Este método devuelve cualquier posible cliente con un cambio de valor de datos o una nueva actividad en Marketo para el intervalo de fechas que solicite. Para `leadSelector`, debe especificar `LastUpdateAtSelector`. A continuación, defina los intervalos de fechas con `oldestUpdatedAt` y `latestUpdatedAt` límites de tiempo. Consulte el ejemplo de XML de solicitud a continuación, que muestra cómo encontrar posibles clientes que se actualizaron entre las 00:00 PST del 6 de junio de 2014 y las 00:00 PST del 7 de junio de 2011. Nota: el intervalo de fechas no debe superar los 30 días.

**XML de solicitud de muestra para encontrar posibles clientes actualizado por fecha**

```xml
<soapenv:Envelope xmlns:xsi="<http://www.w3.org/2001/XMLSchema-instance>" xmlns:soapenv="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:mkt="<http://www.marketo.com/mktows/">
<soapenv:Header>
<mkt:AuthenticationHeader>
<mktowsUserId>REPLACE</mktowsUserId>
<requestSignature>REPLACE</requestSignature>
<requestTimestamp>2014-10-23T12:19:51-07:00</requestTimestamp>
</mkt:AuthenticationHeader>
</soapenv:Header>
<soapenv:Body>
<mkt:paramsGetMultipleLeads>
<leadSelector xsi:type="mkt:LastUpdateAtSelector">
<oldestUpdatedAt>2014-06-06T00:00:00-08:00</oldestUpdatedAt>
<latestUpdatedAt>2014-06-07T00:00:00-08:00</latestUpdatedAt>
</leadSelector>
</mkt:paramsGetMultipleLeads>
</soapenv:Body>
</soapenv:Envelope>
```

Publicado el _2014-09-24_ por _Murta_

## Adición de contenido dinámico a un correo electrónico

Supongamos que envía un correo electrónico diario y desea incluir automáticamente la fecha de ese día en la plantilla de correo electrónico. Para ello, se utilizan tokens y secuencias de comandos de correo electrónico en Marketo.

1. Cree un token.** Vaya al programa en el que desee utilizar el token. Haga clic en Mis token.
1. Haga doble clic en Email Script. A continuación, asigne un nombre a este token. A continuación, haga clic en Edit.
1. Pegue el script de correo electrónico siguiente en esta ventana. A continuación, haga clic en Guardar.

## Objeto de calendario de Access Velocity

`set($x = $date.calendar)`

## Formato de fecha

`set($current_date = $date.format('dd-MM-yyyy', $x.getTime()))`

## Devuelve la fecha de hoy

`$current_date`

1. Haga referencia al token en la plantilla de correo electrónico.** Tenga en cuenta el nombre del token. Vaya al borrador del correo electrónico. Incluya el token.  Cuando se envía el correo electrónico, se rellena el valor del token. Para obtener más información, consulte la [documentación para desarrolladores de scripts de correo electrónico](https://experienceleague.adobe.com/es/docs/marketo-developer/marketo/email-scripting).

Publicado el _2014-11-22_ por _Murta_

## Bash Security Advisory

Marketo ha realizado una investigación exhaustiva de la vulnerabilidad Bash, también conocida como [Shellshock (CVE-2014-6271)](https://nvd.nist.gov/view/vuln/detail?vulnId=CVE-2014-6271), y ha concluido que no somos susceptibles a estos ataques. Además, hemos tomado medidas preventivas al actualizar el software a la última versión para garantizar el cumplimiento de la [recomendación del CERT](https://www.cisa.gov/news-events/alerts/2014/09/25/gnu-bourne-again-shell-bash-shellshock-vulnerability-cve-2014-6271).

Publicado el _2014-09-26_ por _Murta_

## Actualización de las credenciales de la API de SOAP

Se recomienda actualizar con regularidad las credenciales de la [API de SOAP](/help/soap-api/soap-api.md). Actualmente, no hay forma de hacerlo mediante programación a través de la API de Marketo. Las instrucciones siguientes le mostrarán cómo actualizar las credenciales de la API de SOAP a través de la IU de Marketo.

1. Vaya a la sección Admin y haga clic en Web Services.
1. Establezca una Clave de cifrado de al menos 10 caracteres y haga clic en Guardar cambios.

Publicado el _2014-09-29_ por _Murta_

## Cómo limpiar la base de datos de Marketo

**NOTA: Esta es una publicación de blog de invitado. Josh Hill es el Marketo Practice Lead en Perkuto, una agencia de automatización de marketing. Josh trabaja en la intersección de marketing, ventas y tecnología para ofrecer sistemas de generación de ingresos. Escribe sobre automatización de mercadotecnia y generación de demanda en [MarketingRockstarGuides.com](https://www.marketingrockstarguides.com/).** Mantener limpia la base de datos de Marketo es una parte importante de la administración de este poderoso sistema. Si los datos de Marketo están sucios o los datos de CRM están sucios, su trabajo como administrador de marketing se vuelve mucho más difícil, ya que explica por qué las campañas fueron a parar a las personas equivocadas o por qué los informes son &quot;direccionales&quot;. [Un estudio de Gartner del 2011 observó](https://www.data.com/export/sites/data/common/assets/pdf/DS_Gartner.pdf) que la mala calidad de los datos redujo la productividad laboral en un 20%. Es decir, el 20 % del tiempo (8 horas/semana o 1 día por semana) que se desperdicia porque tenía que corregir los datos. Pero esta pérdida palidece en comparación con los ingresos perdidos porque la segmentación fue incorrecta. Estas son las cinco razones principales para invertir en mantener limpios los datos: se puede realizar una mejor segmentación de posibles clientes, lo que le permite enfocar el mensaje en las personas adecuadas y en el momento adecuado. Ese cupón de descuento del 20% debería ir solo a nuevos clientes potenciales, no a tus mejores clientes, ¿verdad? - Evite el envío duplicado de correos electrónicos. A pesar de todas las precauciones, es posible enviar varios correos electrónicos al mismo posible cliente, generalmente porque no se combinan registros duplicados. - Informe preciso a su jefe. Como el CMO tiene un puesto en la tabla de ingresos, debe tener informes precisos, coherentes y repetibles... o ya no puede seguir siendo experto en marketing de ingresos. - Perjudicar los acuerdos pendientes si envía por correo electrónico el material de marketing incorrecto a los posibles clientes clave durante las negociaciones de ventas.

Si la base de datos no proporciona ese detalle en la fase de ciclo vital, podría hundir un acuerdo o dos. - Elimine los posibles clientes malos o antiguos para mantenerse por debajo de los umbrales de precios. Se ha escrito mucho sobre el costo de los datos incorrectos. Lo que más le preocupa es que demasiados posibles clientes malos, duplicados o antiguos le están empujando por encima de su nivel de precios de Marketo o Salesforce, lo que puede resultar en miles de tarifas al año. Entonces, ¿cómo mantener limpia tu base de datos? Puede seguir estos principios de limpieza de los datos, pero ¿cómo puede llegar allí dentro de Marketo? Hago algunas suposiciones sobre su sistema: - Usted tiene un sistema estándar de Marketo. - Tiene una configuración típica de Salesforce de cliente potencial-contacto-cuenta. - Está sincronizando todos los registros entre sistemas. : no utiliza duplicados con fines específicos.

Encuentre los posibles clientes que se solucionarán** Primero vamos a encontrar los posibles clientes que son problemas potenciales. Con cada cliente, paso por una serie de listas inteligentes para determinar el estado de su base de datos. Le sugiero que haga lo mismo mensualmente. Una vez que las listas inteligentes funcionan, no le llevará más de 15 minutos al mes. Esta es una excelente manera de demostrar el ROI de su trabajo y de Marketo. Cree una tabla para comprender el impacto total en la base de datos. El ejemplo siguiente incluye los criterios que busco, por lo que asegúrese de incluir otros campos importantes para su negocio. Desglose cada grupo por Solo Marketo, Posibles clientes de SFDC y Contactos de SFDC.

Usar la automatización para corregir valores de datos: el uso de reglas de automatización para corregir errores ortográficos comunes o la falta de datos se remonta varias décadas atrás. En la década de 1960, la mala calidad de los datos al enviar correo postal podría resultar en miles de dólares que se desperdician. Hoy en día, los errores de calidad de los datos arruinan las reputaciones más rápido que un mensaje de correo mal dirigido. La reputación del correo electrónico, la elección del idioma y la experiencia del cliente importan y son más importantes porque los errores pueden volverse virales públicamente en cuestión de minutos. Ahorre la reputación de su empresa con la limpieza automatizada de datos. Estos flujos de administración de datos son una de las primeras cosas que configuro en Marketo. La mayoría de las empresas configuran flujos similares: - Corrector de país (aunque debería haber seguido el Principio 1 para no necesitarlo) - Corrector de estado o asignador - a menudo útil si tiene País y Estado deducido. - Recuento de empleados a Rango de empleados. - Source de posible cliente incorrecto a Source de buen cliente - Correo electrónico no válido a Correo electrónico es bueno si el correo electrónico cambió. : traduzca los valores de los campos antiguos a un nuevo campo (de completo a vacío). Por ejemplo, este flujo ajusta Rango de empleados en función del Número de empleados.

Herramientas de adición de datos El relleno de campos vacíos es fundamental para segmentar mejor la base de datos. Los posibles clientes no siempre rellenan el sector, la función o incluso el título adecuados de un formulario. A veces, tiene datos heredados de un sistema anterior. Esta falta de datos significa que debe enviar ese correo electrónico a menos personas o posiblemente a las personas equivocadas. Para solucionarlo, necesita herramientas de adición de datos.

Opción 1: Complétela usted mismo. Es posible que pueda interpolar datos para rellenar campos vacíos. Tal vez tenga un código SIC en lugar del nombre del sector o los ingresos anuales frente al rango de ingresos anuales. Marketo puede automatizar fácilmente estas correcciones.

Opción 2: buscar un proveedor de adición/enriquecimiento de datos a través de LaunchPoint Hay [varios proveedores en LaunchPoint](https://exchange.adobe.com/apps/browse/ec?product=MRKTO), como NetProspex y ReachForce, que pueden ayudarle a enriquecer sus datos de posibles clientes. Algunos le piden una hoja de sus datos, que luego limpian y envían de vuelta. La mejor opción es una herramienta automatizada en Marketo o Salesforce, que comprueba los campos que desea y, a continuación, devuelve los datos correctos. La mayoría de los proveedores usan la [API de Marketo o los Webhooks](/help/home.md) para lograr esto.

Opción 3: Usar las API de Marketo para actualizar los posibles clientes Puede usar las API de Marketo para identificar los posibles clientes que deben limpiarse y, a continuación, actualizarlas mediante la API. La API de REST [Obtener varios posibles clientes por tipo de filtro](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) es un buen punto de partida para extraer datos de Marketo que coincidan con determinados criterios. Para actualizar posibles clientes, consulte [Crear/actualizar la API de REST de posibles clientes](/help/rest-api/leads.md).

También puede configurar un [webhook de Marketo](/help/webhooks/webhooks.md) para notificar a un sistema externo que se ha producido un evento determinado, como un formulario rellenado. A continuación, puede responder con valores para actualizar el posible cliente con.

¿Qué debería automatizar? He proporcionado algunas sugerencias en el paso 1. Pero si queremos limpiar más de la base de datos, tenemos que ir más allá de fijar los valores de los datos. - Listas negras de competidores: asegúrese de que está automatizando la recopilación y la lista negra de sus competidores. Si tiene compañeros de la competencia, aún es bueno marcarlos correctamente y colocarlos en una lista para suprimirlos.  - Varias devoluciones graves: automatizar definitivamente este flujo. Si un posible cliente se rechaza más de dos veces en un periodo de 30 días, establézcalo en Suspendido o No válido. A continuación, vaya una vez al mes para revisar si el problema era un error tipográfico u otra cosa.  - Varias devoluciones leves en 30 días: configúrelas en marketing suspendido=TRUE durante 30 días.  - Suspensión/eliminación de trampas de correo no deseado: tenga cuidado si su producto significa que las personas utilizan posibles direcciones de trampas de correo no deseado. Consulte una lista de trampas de correo no deseado. La lista inteligente.

**Qué no automatizar** Nunca automatizar la eliminación de posibles clientes, porque existe demasiado riesgo en la eliminación masiva accidental de los registros incorrectos. En su lugar, revise una vez al mes los posibles clientes marcados como Papelera. Sin embargo, si desea eliminar los posibles clientes de la papelera, ejecute un flujo como este con una espera de 30 días.

Advertencias: El uso de la automatización es una buena manera de ahorrar tiempo y mantener limpia la base de datos. La automatización es una espada de doble filo porque si la configuras mal, puede causar estragos en unos minutos. Tenga cuidado al pasar por estos procesos y mantenga a todos informados. En general, aconsejo no eliminar ningún contacto de SFDC porque es más probable que sea una oportunidad activa, un cliente o un cliente anterior. Finanzas o Legal pueden solicitarle que conserve ciertos registros y los Contactos se adjuntan a esos registros. En su lugar, céntrese en los contactos que reciben un rechazo grave, que pasan a ser inválidos o que ya no están presentes. Conozca algunas de las técnicas para mantener limpia la base de datos de Marketo. Háganos saber si se le ocurrieron otras formas de automatizar estos procesos mediante la API o los Webhooks.

Publicado el _2014-10-08_ por _Josh_

## Actualizaciones de la versión de octubre de 2014

### Relleno previo de página externa

Los formularios Marketo Forms no proporcionan la funcionalidad de relleno previo nativa cuando se cargan fuera de una página de aterrizaje de Marketo. Sin embargo, aún podemos implementar esto usando las [API de Marketo](/help/rest-api/rest-api.md) y la [API de JavaScript de Forms 2.0](/help/javascript-api/forms-api-reference.md/). El primer paso es recuperar los datos de posibles clientes de Marketo a través de una llamada REST desde el servidor. Suponiendo que no tengamos una forma inmediata de cruzar ID de posibles clientes u otro identificador único del servidor, necesitamos usar la cookie de Munchkin, &#39;_mkto_trk&#39;, para recuperar datos del servidor de Marketo, usando el método [Obtener posibles clientes por tipo de filtro](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET).
Para realizar esta llamada, necesitamos sus puntos finales de autenticación y REST de su instancia. Una vez que se haya autenticado con su instancia de Marketo, tendremos que realizar una llamada a la API de posibles clientes en `https://<host>/rest/v1/leads.json`. Luego necesitamos crear una cadena de consulta para filtrar la cookie de Marketo como esta `?filterType=cookie&filterValues=`. Debe recuperar el valor específico de la clave &#39;_mkto_trk&#39; que el cliente envía al servidor. NOTA: El valor de la cookie _mkto_trk incluye un signo &amp; y debe codificarse con la dirección URL `%26` para que el extremo de Marketo lo acepte correctamente. De forma predeterminada, la API de posibles clientes devuelve cuatro campos: `id`, `email`, `firstName` y `updatedAt`. Para establecer un conjunto específico de campos, debe incluir un parámetro de consulta `fields`, con nombres de campo separados por comas como este: `&fields=email,firstName,lastName,company`. Finalmente, nuestra llamada va a tener este aspecto:

`https://<host>/rest/v1/leads.json?filterType=cookie&filterValues=<cookie>&fields=email,firstName,lastName,company&access_token=<token>`

Cuando realizamos esta llamada, devuelve un objeto JSON con el siguiente aspecto:

```json
{
    "requestId":"e42b#14272d07d78",
    "success":true,
    "result":[
        {
        "id":50,
        "firstName":"Kenny",
 "lastName":"Elkington",
        "email":"<mkto@example.com>",
 "company":"Marketo Inc."
        }]
};
```

Ahora que tenemos los detalles del posible cliente, podemos asignarlos a un formulario de Marketo mediante los métodos whenReady y vals en JavaScript. En primer lugar, debemos establecer los resultados del posible cliente como una variable en nuestra página:

```javascript
<script>
//print your JSON object dynamically as the mktoLead variable
var mktoLead = {
    "requestId":"e42b#14272d07d78",
    "success":true,
    "result":[
        {
        "id":50,
        "firstName":"Kenny",
  "lastName":"Elkington",
        "email":"mkto@example.com",
  "company":"Marketo Inc."
        }]
}
</script>
```

Ahora que los resultados están en la página, podemos asignarlos a los campos de formulario:

```javascript
<script>
MktoForms2.whenReady( function(form) {
 //set the first result as local variable
 var mktoLeadFields = mktoLead.result[0];
    //map your results from REST call to the corresponding field name on the form
 var prefillFields = {
   "Email" : mktoLeadFields.email,
   "FirstName" : mktoLeadFields.firstName,
   "LastName" : mktoLeadFields.lastName,
   "Company" : mktoLeadFields.company
   };
 //pass our prefillFields objects into the form.vals method to fill our fields
 form.vals(prefillFields);
 }
 );
</script>
```

Publicado el _2014-10-24_ por _Kenny_

## Reemplazar HTML del correo electrónico

Esta publicación muestra cómo eliminar el HTML generado por Marketo para un correo electrónico. A continuación, puede utilizar su propia HTML sin que Marketo vuelva a aplicarle formato.

1. Vaya al correo electrónico y, a continuación, haga clic en Editar borrador.
1. Haga clic en Acciones de correo electrónico, en Herramientas de HTML y, a continuación, en Reemplazar HTML.
1. Reemplace el código de este cuadro por su HTML. A continuación, haga clic en Guardar.

Publicado el _2014-10-28_ por _Murta_

## Obtener el ID de cookie de un visitante y luego consultar los datos de posibles clientes asociados

Con el extremo REST [Obtener varios posibles clientes por tipo de filtro](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET), puede consultar los datos de posibles clientes en función del ID de cookie de un usuario. Por ejemplo, puede utilizar este método para rellenar previamente un formulario en una página de aterrizaje que no sea de Marketo. Esta publicación muestra cómo capturar el valor de la cookie del usuario durante una visita a la página web, consultar [Obtener API de REST de varios posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) con ese id de cookie y, a continuación, devolver los datos de posibles clientes del usuario. En primer lugar, necesitamos el valor de la cookie de Munchkin del usuario, &#39;_mkto_trk&#39;. A continuación, se muestra un ejemplo de una función de JavaScript que puede utilizar para obtener el valor de la cookie. Consulte [esta página StackOverflow](https://stackoverflow.com/questions/10730362/get-cookie-by-name) para obtener más información sobre este método. Recomiendo configurar un retraso de 500 ms después del evento de carga de página antes de llamar a esta función. Esto proporciona a Munchkin tiempo para cargar y almacenar cookies en el usuario.

```javascript
//Function to read value of a cookie
function readCookie(name) {
    var cookiename = name + "=";
    var ca = document.cookie.split(';');
    for(var i=0;i < ca.length;i++) {
        var c = ca[i];
        while (c.charAt(0)==' ') c = c.substring(1,c.length);
        if (c.indexOf(cookiename) == 0) return c.substring(cookiename.length,c.length);
    }
    return null;
}

//Call readCookie function to get value of user's Marketo cookie
var value = readCookie('_mkto_trk');
```

A continuación, pase el valor de la cookie &quot;_mkto_trk&quot; a su servidor. Para recuperar datos de posibles clientes, desde el servidor realiza una llamada a la API de REST [Obtener varios posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) con este valor de cookie. Necesita los extremos de autenticación y REST de su instancia. La llamada debe estructurarse de la siguiente manera:

NOTA: El valor de la cookie `_mkto_trk` incluye un signo &amp; y debe ser una URL codificada para `%26` para que el extremo de Marketo la acepte correctamente.

```java
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com>"
endpoint = "/rest/v1/leads.json"
# Replace with your access token
auth_token =  "?access_token=" + "cde42eff-aca0-48cf-a1ac-576ffec65a84:ab"
# Replace with filter type and values
filter_type_and_values = "&filterType=cookies&filterValues=id:AAA-BBB-CCC%26token:_mch-marketo.com-1418418733122-51548&fields=cookies,email"
request_url = marketo_instance + endpoint + auth_token + filter_type_and_values

# Make request
response = RestClient.get request_url

# Returns Marketo API response
puts response
```

El ejemplo anterior devuelve el correo electrónico y todas las cookies asociadas al usuario. A continuación, puede utilizar estos datos para personalizar la página posterior que visita el usuario.

`{"requestId":"aa00#14a405aa786","result":[{"id":583,"email":"<testaccount@gmail.com>","cookies":"_mch-marketo.com-1418418733122-51548"}],"success":true}`

Publicado el _2014-10-30_ por _Murta_

## ¿Cuándo necesita un desarrollador que le ayude con la automatización de marketing?

NOTA: Esta es una publicación de blog de invitado. Josh Hill es el Marketo Practice Lead en Perkuto, una agencia de automatización de marketing. Josh trabaja en la intersección de marketing, ventas y tecnología para ofrecer sistemas de generación de ingresos. Escribe sobre automatización de mercadotecnia y generación de demanda en [MarketingRockstarGuides.com](https://www.marketingrockstarguides.com/).

Las plataformas de automatización de marketing son tremendamente potentes y están listas para usarse y en manos de un operador experimentado. Las plataformas, por definición, permiten el uso de aplicaciones de extensión para hacer que el sistema haga cosas aún más sorprendentes para su equipo. Es posible que piense que el motor lógico de Marketo es capaz de hacer tanto (y lo es), pero hay limitaciones. Marketo no puede hacer todo por usted, ni debería hacerlo.

Existen otras herramientas que realizan su función mejor de lo que Marketo podría crearla. La plataforma de Marketo es muy abierta, lo que permite que exista el ecosistema de aplicaciones [LaunchPoint](https://exchange.adobe.com/apps/browse/ec?product=MRKTO). También puede utilizar esta apertura para ampliar las capacidades de su sitio y Marketo de modo que se ajusten a sus necesidades comerciales. Lo bueno de una plataforma como Marketo es que permite al experto en marketing habitual crear páginas, correos electrónicos y lógica de enrutamiento sin ser un programador experto.
Un experto en marketing en estos días necesita entender la lógica, pero es mejor dejar la programación real a los expertos. Entonces, ¿cómo sabes cuándo necesitas llamar a un desarrollador? Tengo unas cuantas reglas básicas, o heurística, para decidir cuándo un programador debe involucrarse: - Cuando Marketo no tiene un filtro, déclencheur o característica obvia para la necesidad, a menudo se puede hacer con algún JavaScript o jQuery. - ¿Será esto demasiado complejo para Marketo por sí solo? - ¿Marketo puede hacer esto? - ¿Es esto una personalización del sitio web no compatible fácilmente? - ¿Marketo necesita hablar con un sitio web u otra base de datos? - ¿Suena como algo que un ordenador puede hacer, pero Marketo no tiene una función para ello? Recuerde que, aunque Marketo puede no ofrecer una función predeterminada, se conecta a muchas integraciones de terceros, así como a conexiones personalizadas.

Eche un vistazo a algunas de estas categorías en [LaunchPoint Marketplace](https://exchange.adobe.com/apps/browse/ec?product=MRKTO): - [Herramientas de Analytics](https://exchange.adobe.com/apps/browse/ec?product=MRKTO) - [Datos adjuntos](https://exchange.adobe.com/apps/browse/ec?product=MRKTO) - [Sistemas de administración de contenido](https://exchange.adobe.com/apps/browse/ec?product=MRKTO) Algunas aplicaciones de terceros proporcionan paneles de control intuitivos y herramientas de configuración dentro de la plataforma (GoToWebinar). Son integraciones &quot;nativas&quot; en las que lo más que debe hacer es configurar el inicio de sesión y luego utilizarlo en Marketo. Sin embargo, otras extensiones requieren el uso de la API más compleja que debe programarse de forma más directa.

**Opciones de integración de Marketo** - Integración con LaunchPoint - generalmente es un inicio de sesión o una configuración sencilla. - Integración de API - requiere la configuración de API y programación: (1) [API REST](/help/rest-api/rest-api.md) (2) [API de SOAP](/help/soap-api/soap-api.md) (3) [Integración de webhook](/help/webhooks/webhooks.md) - requiere la configuración de código especial, pero es bastante fácil. (4) [Scripts de correo electrónico](./email-scripting.md) (Velocity) - JavaScript y jQuery: (1) [Forms 2.0](/help/javascript-api/forms-api-reference.md) (2) [Seguimiento de posibles clientes (Munchkin)](/help/javascript-api/lead-tracking.md) (3) [RTP JS](/help/javascript-api/web-personalization.md) Estos son algunos casos de uso para usar un desarrollador con el fin de ampliar las capacidades de la plataforma Marketo. ¿Tiene alguno de estos casos de uso? Si es así, podría ser el momento de hablar con un desarrollador. [Visite la sección de socios de servicios en LaunchPoint](https://exchange.adobe.com/apps/browse/ec?product=MRKTO).

Publicado el _2014-11-06_ por _Josh_

## Integración de Slack con Marketo

[Slack es una plataforma de colaboración empresarial](https://slack.com/). Si su equipo está en Slack, puede incorporar fácilmente notificaciones de Marketo al flujo de trabajo. Esta publicación muestra cómo agregar una notificación al registro de chat cuando se produce una actividad de posible cliente específica en Marketo. Los casos de uso potenciales incluyen notificar a todo el equipo sobre un rellenado de formulario, una visita a una página de precios o un posible cliente con el que no se haya contactado en 30 días. La captura de pantalla siguiente muestra el aspecto que tendrá la notificación de Marketo en Slack después de seguir los pasos de este artículo de ayuda.

1. Inicie sesión en Slack. Haga clic en Integraciones en Slack
1. Haga clic en el botón Añadir para los webhooks entrantes
1. Elija el canal para la notificación de Marketo. A Continuación, Haga Clic En Añadir Integración De Webhook Entrante.
1. Copie y pegue la dirección URL del webhook (necesaria para el paso ocho)
1. Elija un nombre para la notificación
1. Inicie sesión en Marketo. Vaya a Administración. Haga clic en Webhooks.
1. Haga clic en Nuevo webhook
1. Introduzca un nombre para el webhook. Introduzca la URL del webhook desde el paso cuatro. Introduzca Contabilizar como tipo de solicitud. Introduzca una plantilla de carga útil.

Esta es la plantilla de carga útil de la captura de pantalla. Utiliza tokens de nombre, empresa y dirección de correo electrónico de nivel de posible cliente.

`payload={"text": "DEVELOPER SITE ALERT: {{lead.First Name:default=edit me}} {{lead.Company=edit me}}, {{lead.Email Address:default=no email address}}" }`

1. Configure la campaña de Déclencheur en Marketo. El paso de flujo es llamar al webhook a Slack. Una lista inteligente es una visita a una página web.
1. Verifique Que Funcione.

Consulte la [documentación para desarrolladores](./webhooks/webhooks.md) para obtener más información sobre los enlaces web en Marketo.

Publicado el _2014-11-10_ por _Murta_

## Integración de Litmus con Marketo

[Litmus es un servicio](https://www.litmus.com/) para probar correos electrónicos en navegadores y clientes de correo electrónico. Litmus también proporciona análisis de los correos electrónicos, incluidos clics, aperturas y eliminaciones. Este artículo muestra cómo integrar Marketo con Litmus.

1. Al configurar su programa de correo electrónico en Marketo, haga clic en &quot;Mis tokens&quot; en el panel del programa
1. Arrastre el token &quot;Script de correo electrónico&quot; al panel central para añadirlo.
1. Asigne un nombre al token y haga clic en &quot;Haga clic para editar&quot;
1. A la derecha, debajo de &quot;Objetos estándar&quot;, expanda la categoría &quot;Posible cliente&quot;. Busque el campo &quot;Dirección de correo electrónico&quot; y marque la casilla. En el espacio de script vacío a la izquierda de esta misma página, pegue el código de seguimiento proporcionado por Litmus. En el código Litmus, reemplace cada instancia de `{{lead.Email Address}}` por `${lead.Email}`. Haga clic en Guardar para cerrar la ventana de Lightbox y vuelva a hacer clic en &quot;Guardar&quot; en la página de tokens.
1. Tome nota del nombre del token `{{my.LitmusToken}}`. Abra el correo electrónico que desee rastrear. En la parte inferior del correo electrónico, coloque el nuevo token de script. También puede agregar información predeterminada para que coincida con la versión de Litmus `{{my.LitmusToken:default=editme}}`.

Cuando se envía el correo electrónico, Marketo coloca el script en el correo electrónico.

Publicado el _2014-11-18_ por _Murta_

## Especificar una imagen cuando se comparta una página de aterrizaje de Marketo en Facebook

Supongamos que desea que una imagen se muestre automáticamente al compartir una página de aterrizaje de Marketo en Facebook. Tal vez también quiera que esta imagen no esté en la propia página de aterrizaje de Marketo, sino solo en el recurso compartido de Facebook. Puede hacerlo añadiendo una metaetiqueta de gráfico abierto a la página de aterrizaje de Marketo. A continuación se indican los pasos para hacerlo.

1. Seleccione la página de aterrizaje. A continuación, haga clic en Editar borrador.
1. Haga clic en Editar etiquetas Meta de página.
1. Agregue metadatos de gráfico abierto a la sección Etiquetas de organización de Facebook. A continuación, haga clic en Guardar. Este es el formato: `<meta property="og:image" content="http://example.com/example.jpg"/>`

[Consulte la documentación para desarrolladores de Facebook](https://developers.facebook.com/docs/sharing/best-practices) sobre las metaetiquetas de gráficos abiertos para obtener más información.

Publicado el _17-11-2014_ por _Murta_

## Redirigir páginas en función del referente

Supongamos que desea evitar el tráfico directo a una página de aterrizaje de Marketo. Imagine que esta página tiene contenido descargable, como un PDF, que desea que un usuario rellene primero un formulario antes de recibirlo. Puede resolver esto comprobando si el usuario viene de una página determinada. En este caso, sería la página en la que el usuario tiene que rellenar un formulario. Si el usuario no viene de esa página, puede redirigirlo a la página de rellenado del formulario. Para ello, debe comprobar si la página de referencia a la página de aterrizaje con contenido es la página de rellenado del formulario.
Reemplace ambas instancias de `http://example.com/PageWithForm` en el siguiente fragmento con un vínculo a la página de la que desea que provenga el usuario. Podría ser la página de rellenado del formulario.**

```javascript
<script>
window.onload = function() {
  if (document.referrer !== "http://example.com/PageWithForm") {
    document.location.href = "http://example.com/PageWithForm";
  };
 };
</script>
```

Incluya el fragmento de JavaScript personalizado antes de la etiqueta de cierre en la página de aterrizaje de Marketo con contenido.** Si el usuario no se envía a la página de aterrizaje con contenido de la página de rellenado del formulario, ahora se redirige al usuario a la página de rellenado del formulario.

Publicado el _2014-11-18_ por _Murta_

## Integración de Trello con Marketo

Trello es una [aplicación de administración de proyectos popular basada en la Web](https://trello.com/). Si su equipo utiliza Trello, puede incorporar fácilmente notificaciones de Marketo al flujo de trabajo. Esta publicación muestra cómo agregar una tarjeta con una notificación de Marketo a su tablero de Trello. Esta tarjeta se añadirá cuando se produzca una actividad de posible cliente específica en Marketo. Los casos de uso potenciales incluyen notificar a todo el equipo sobre un rellenado de formulario, una visita a una página de precios o un posible cliente con el que no se haya contactado en 30 días.

1. Inicie sesión en Trello. Vaya al tablero de Trello que agregará notificaciones de Marketo en. Haga clic en Agregar una lista y, a continuación, asígnele un nombre.
1. Haga clic en Mostrar barra lateral. Haga clic en Configuración de correo electrónico a bordo. Anote el correo electrónico en el cuadro &quot;Su dirección de correo electrónico para este tablero&quot;. Este correo electrónico se utilizará en el paso seis. Elija en qué lista añadir la notificación de Marketo.
1. Inicie sesión en Marketo. Haga clic en la nueva campaña inteligente. Escriba un nombre y haga clic en Guardar.
1. Vaya a Lista inteligente. Elija un déclencheur para esta campaña inteligente. En este ejemplo, se utiliza el déclencheur de relleno de formulario. Arrastre el déclencheur Rellenar formulario al panel central. Seleccione el formulario en el que desea almacenar en déclencheur esta notificación.
1. Cree un correo electrónico. Haga clic en Nuevo. Haga clic en Recurso local. Haga clic en Nuevo correo electrónico. Nombre el correo electrónico. A continuación, haga clic en Crear.
1. Haga clic en &quot;Editar borrador&quot; para el correo electrónico que ha creado en el paso cinco. Arrastre los tokens relevantes que desee mostrar en la tarjeta Trello. La línea de asunto del correo electrónico de Marketo aparece en el título de la tarjeta Trello y el cuerpo del correo electrónico de Marketo aparece en la descripción de la tarjeta Trello. Por ejemplo, puede usar &quot;ALERTA DE POSIBLE CLIENTE: `{{lead.First Name:default=edit me}}` `{{lead.Last Name:default=edit me}}` si desea que el nombre y los apellidos del posible cliente estén en el título de la tarjeta Trello. Luego apruebe el correo electrónico.
1. Vaya a Campaña inteligente. Haga clic en Flujo. Arrastre Enviar alerta al panel central. Seleccione el correo electrónico que acaba de crear. Seleccione &quot;Enviar a&quot; como Ninguno. Seleccione &quot;Para otros correos electrónicos&quot; como el correo electrónico de Trello del paso dos.
1. Haga clic en Programación en el menú superior. Haga clic en Activar. Haga clic en Confirm.
1. Pruebe la integración. En el tablero de Trello aparecerá una carta con el nombre y apellido del posible cliente en el título. Consulte la [documentación de Trello](https://support.atlassian.com/trello/) para obtener más información.

Publicado el _2014-11-18_ por _Murta_

## Buscar posibles clientes por valor de campo personalizado

Supongamos que desea obtener posibles clientes de la API de Marketo que coincidan con determinados criterios de actividad o inactividad. Por ejemplo, quizás desee encontrar un posible cliente cuya puntuación no haya cambiado en los últimos 30 días. Al seguir los pasos de esta publicación, puede obtener esta lista de posibles clientes. Para ello, creamos una campaña inteligente en Marketo que identifica a los posibles clientes cuya puntuación no ha cambiado en los últimos 30 días y, a continuación, almacenamos un valor en estos posibles clientes para identificarlos. Luego consultaremos la API con este valor.

1. Cree un nuevo campo personalizado llamado customLeadStatus** Inicie sesión en Marketo y vaya al panel Administración. Haga clic en Administración de campos. Haga clic en &quot;Nuevo campo personalizado&quot;.  Asigne un nombre al campo A continuación, haga clic en Crear.
1. Cree una campaña inteligente con una lista inteligente que busque posibles clientes que no se hayan actualizado en 30 días.** Haga Clic En Nueva Campaña Inteligente. Asigne un nombre a la nueva campaña inteligente. Arrastrar la puntuación no cambiada del panel derecho al panel central.
1. Agregue un paso de flujo a la campaña inteligente desde el paso 3 para actualizar el campo customLeadStatus con un nuevo valor.** Arrastrar Cambiar valor de datos del panel derecho al panel central.
1. Actualice la Campaña inteligente para permitir que los posibles clientes se ejecuten varias veces.** Haga clic en Programar. A continuación, haga clic en Edit.  Seleccione cada vez. A continuación, haga clic en Guardar. La campaña empezará a ejecutarse.
1. Consulte [Obtener varios posibles clientes por tipo de filtro API de REST](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET). Proporcionar los parámetros filterType=customLeadStatus y filterValue=requirementsEnrichment.**

Esta es una solicitud de ejemplo que devuelve estos datos.

`<https://AAA-BBB-CCC.mktorest.com/rest/v1/leads.json?access_token=><yourAccessToken>&filterType=customLeadStatus&filterValues=needsEnrichment`

Una llamada de API correcta devuelve datos JSON con posibles clientes cuyo campo customLeadStatus coincide con el valor de needEnrichment. Revise [Obtener varios posibles clientes por tipo de filtro API de REST](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) para obtener más información.

Publicado el _2014-11-22_ por _Murta_

## Sincronización de oportunidades mediante la API de SOAP

En esta publicación se describe cómo insertar oportunidades en Marketo mediante la API de SOAP y cómo asociarlas a empresas y posibles clientes. Comienza con una explicación de cómo funciona este proceso y, a continuación, proporciona ejemplos de código para cada uno de los escenarios.

**Diagrama de estructura de tabla** En primer lugar, el diagrama siguiente describe la estructura de tabla. El ID se genera automáticamente al crear un registro (número secuencial). Los campos en negrita son obligatorios: solo la función de persona de oportunidad tiene campos obligatorios. Los campos entre paréntesis son opcionales, al igual que las conexiones con una línea de puntos.

**Inserción de oportunidad básica** Primero inserte la oportunidad y, a continuación, la función de persona de oportunidad, que vincula la oportunidad con los posibles clientes. Para este ejemplo, utilice el ID de posible cliente y el ID de oportunidad para especificar el rol de persona de oportunidad. Obtiene el ID de oportunidad en la respuesta de SOAP al crear la oportunidad. El ID del posible cliente es visible en todos los posibles clientes de la base de datos de Marketo.

**Vínculo de compañía** En la mayoría de los casos, desea vincular una oportunidad a una compañía, además de a un individuo. En Marketo, no se puede acceder a los registros de la compañía a través de la API de SOAP, solo a los registros de posibles clientes (los registros de posibles clientes contienen los campos de la compañía). Todavía es posible vincular oportunidades a una compañía agregando un identificador de compañía único a cada posible cliente y utilizando ese ID en su oportunidad. El paso 1 es crear un campo &quot;ID de compañía&quot; en el registro de posibles clientes y rellenarlo con un identificador único, normalmente de un sistema back-end. El paso 2 es crear un campo de &quot;ID de compañía&quot; en la oportunidad: tiene que pedir al servicio de asistencia o consultoría de Marketo que cree ese campo por usted. A continuación, rellene este campo al crear Oportunidades, que conecta la Oportunidad con la Compañía. Esto es especialmente importante si utiliza Marketo Revenue Cycle Analytics y desea utilizar el Analizador de influencia de oportunidades, que depende de la información de la compañía.

**Uso de identificadores externos** En muchos casos, es posible que tenga sus propios identificadores únicos al integrarse con un sistema back-end. Es posible utilizar estos identificadores únicos en Marketo a través de claves externas. Para los posibles clientes, normalmente utilizaría el ID de persona del sistema externo (FSPID), que se utiliza en lugar del ID de Marketo o la dirección de correo electrónico como identificador único. El FSPID es un campo oculto del sistema, que no es visible dentro de Marketo. Si aún no lo está haciendo, es necesario que la sincronización de oportunidad también guarde el FSPID en un campo personalizado, por ejemplo, &quot;ID externo&quot; (puede asignar cualquier nombre al campo). Puede crear este campo como administrador de Marketo. Para Oportunidades, haga que el Soporte de Marketo cree otro campo personalizado en Oportunidad, por ejemplo, llamado &quot;ID extranjero&quot; (puede ponerle cualquier nombre). A continuación, rellene este campo al insertar Oportunidades. Por último, al crear la función de persona de oportunidad, se utilizan claves externas para especificar el vínculo entre posibles clientes y oportunidades, en lugar de utilizar los Marketo ID. También puede utilizar las claves externas para actualizar los posibles clientes de las oportunidades. En este momento, no es posible agregar una clave externa a los registros de la función de persona de oportunidad, por lo que tendrá que utilizar el Marketo Id generado automáticamente para esto (que obtiene en la respuesta de SOAP después de crear la función de persona de oportunidad).

**Ejemplo de código** Pasos: 1. Insertar/actualizar posible cliente con clave externa e ID de compañía 1. Insertar oportunidad con clave externa 1. Insertar rol de persona de oportunidad con claves externas 1. Solicitud de SOAP: Actualización del posible cliente existente con clave externa e ID de compañía Esto actualiza un posible cliente existente (ID de Marketo &quot;6&quot;) con la clave externa &quot;12346&quot; y el ID de cuenta/compañía &quot;C123&quot;. También guardamos la clave externa en un campo personalizado, porque la necesitamos para la función de persona de oportunidad. El uso de una clave externa es opcional: también puede utilizar el ID de Marketo para vincular este posible cliente a la oportunidad. El uso del ID de compañía también es opcional, pero es necesario si desea utilizar el Analizador de influencia de oportunidades en RCA. Solicitud:

```xml
<soapenv:Envelope xmlns:soapenv="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:mkt="<http://www.marketo.com/mktows/">
   <soapenv:Header>
      <mkt:AuthenticationHeader>
         <mktowsUserId>\*\*\*</mktowsUserId>
         <requestSignature>\*\*\*</requestSignature>
         <requestTimestamp>2014-11-20T15:18:30-07:00</requestTimestamp>
      </mkt:AuthenticationHeader>
   </soapenv:Header>
   <soapenv:Body>
      <mkt:paramsSyncLead>
         <leadRecord>
            <Id>6</Id>
            <ForeignSysPersonId>12346</ForeignSysPersonId>
            <leadAttributeList>
               <attribute>
                  <attrName>FSPID</attrName>
                  <attrValue>12346</attrValue>
               </attribute>
               <attribute>
                  <attrName>cAccountFSID</attrName>
                  <attrValue>C123</attrValue>
               </attribute>
            </leadAttributeList>
         </leadRecord>
         <returnLead>false</returnLead>
      </mkt:paramsSyncLead>
   </soapenv:Body>
</soapenv:Envelope>
```

Respuesta:

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:xsi="<http://www.w3.org/2001/XMLSchema-instance>" xmlns:ns1="<http://www.marketo.com/mktows/">
   <SOAP-ENV:Body>
      <ns1:successSyncLead>
         <result>
            <leadId>6</leadId>
            <syncStatus>
               <leadId>6</leadId>
               <status>UPDATED</status>
               <error xsi:nil="true"/>
            </syncStatus>
            <leadRecord xsi:nil="true"/>
         </result>
      </ns1:successSyncLead>
   </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Solicitud de SOAP - Creación de oportunidades En este caso, se han creado 2 campos personalizados en la tabla de oportunidades: - `opportunityId` → contiene el ID único de la oportunidad - `cAccountFSID` → contiene la referencia de la empresa en lugar de especificar su propio ID de oportunidad. También puede utilizar el ID de oportunidad generado por Marketo. En ese caso, excluya el nodo Clave externa. La asociación Compañía también es opcional, pero es necesaria si desea utilizar el Analizador de influencia de oportunidades en RCA. Solicitud:

```xml
<soapenv:Envelope xmlns:soapenv="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:mkt="<http://www.marketo.com/mktows/">
   <soapenv:Header>
      <mkt:AuthenticationHeader>
         <mktowsUserId>\*\*\*</mktowsUserId>
         <requestSignature>\*\*\*</requestSignature>
         <requestTimestamp>2014-11-20T15:03:28-07:00</requestTimestamp>
      </mkt:AuthenticationHeader>
   </soapenv:Header>
   <soapenv:Body>
      <mkt:paramsSyncMObjects>
         <mObjectList>
            <!--Zero or more repetitions:-->
            <mObject>
               <type>Opportunity</type>
               <externalKey>
                  <name>opportunityId</name>
                  <value>Opportunity_4</value>
               </externalKey>
               <attribList>
                  <attrib>
                     <name>opportunityId</name>
                     <value>Opportunity_4</value>
                  </attrib>
                  <attrib>
                     <name>Name</name>
                     <value>Opportunity 4 for ACME</value>
                  </attrib>
                  <attrib>
                     <name>IsClosed</name>
                     <value>1</value>
                  </attrib>
                  <attrib>
                     <name>IsWon</name>
                     <value>1</value>
                  </attrib>
                  <attrib>
                     <name>Amount</name>
                     <value>501.00</value>
                  </attrib>
                  <attrib>
                     <name>CloseDate</name>
                     <value>2014-10-24</value>
                  </attrib>
                  <attrib>
                     <name>ExpectedRevenue</name>
                     <value>501</value>
                  </attrib>
                  <attrib>
                     <name>Probability</name>
                     <value>100</value>
                  </attrib>
               </attribList>
               <associationList>
                  <mObjAssociation>
                     <mObjType>Company</mObjType>
                     <externalKey>
                        <name>cAccountFSID</name>
                        <value>C123</value>
                     </externalKey>
                  </mObjAssociation>
               </associationList>
            </mObject>
         </mObjectList>
         <operation>UPSERT</operation>
      </mkt:paramsSyncMObjects>
   </soapenv:Body>
</soapenv:Envelope>
```

Respuesta:

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
   <SOAP-ENV:Body>
      <ns1:successSyncMObjects>
         <result>
            <mObjStatusList>
               <mObjStatus>
                  <id>40</id>
                  <externalKey>
                     <name>opportunityId</name>
                     <value>Opportunity_4</value>
                  </externalKey>
                  <status>CREATED</status>
               </mObjStatus>
            </mObjStatusList>
         </result>
      </ns1:successSyncMObjects>
   </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Solicitud de SOAP: función de persona de la oportunidad Esta solicitud vincula al posible cliente con la oportunidad. Puede especificar varios vínculos en una única solicitud de SOAP (en este ejemplo, se vincula la oportunidad a un único posible cliente). Utiliza las claves externas para especificar el vínculo, pero en los comentarios también muestra cómo utilizar los ID reales (en este caso: 6 para el ID del posible cliente y 40 para el ID de oportunidad). Los campos &quot;IsPrimary&quot; y &quot;Role&quot; son opcionales. Solicitud:

```xml
<soapenv:Envelope xmlns:soapenv="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:mkt="<http://www.marketo.com/mktows/">
   <soapenv:Header>
      <mkt:AuthenticationHeader>
         <mktowsUserId>\*\*\*</mktowsUserId>
         <requestSignature>\*\*\*</requestSignature>
         <requestTimestamp>2014-11-20T15:18:30-07:00</requestTimestamp>
      </mkt:AuthenticationHeader>
   </soapenv:Header>
   <soapenv:Body>
      <mkt:paramsSyncMObjects>
         <mObjectList>
            <!--Zero or more repetitions:-->
            <mObject>
               <type>OpportunityPersonRole</type>
               <attribList>
                  <attrib>
                     <name>IsPrimary</name>
                     <value>1</value>
                  </attrib>
                  <attrib>
                     <name>Role</name>
                     <value>Marketing Manager</value>
                  </attrib>
               </attribList>
               <associationList>
                  <mObjAssociation>
                     <mObjType>Lead</mObjType>
                     <!--id>6</id-->
                     <externalKey>
                      <name>FSPID</name>
                      <value>12346</value>
                     </externalKey>
                  </mObjAssociation>
                  <mObjAssociation>
                     <mObjType>Opportunity</mObjType>
                     <!--id>40</id-->
                     <externalKey>
                      <name>opportunityId</name>
                      <value>Opportunity_4</value>
                    </externalKey>
                  </mObjAssociation>
               </associationList>
            </mObject>
         </mObjectList>
         <operation>UPSERT</operation>
      </mkt:paramsSyncMObjects>
   </soapenv:Body>
</soapenv:Envelope>
```

Respuesta:

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
   <SOAP-ENV:Body>
      <ns1:successSyncMObjects>
         <result>
            <mObjStatusList>
               <mObjStatus>
                  <id>11</id>
                  <status>CREATED</status>
               </mObjStatus>
            </mObjStatusList>
         </result>
      </ns1:successSyncMObjects>
   </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

**Método alternativo (realice los pasos 2 y 3 en una llamada)** Aunque primero puede insertar la oportunidad y luego la función de persona de oportunidad, también es posible hacerlo en una llamada de SOAP. Sin embargo, debe utilizar la clave externa de la oportunidad (no puede utilizar el ID de oportunidad generado automáticamente en la función de persona de la oportunidad, porque la oportunidad aún no se ha generado). Por supuesto, también puede vincular varios posibles clientes a esta oportunidad en esta misma llamada de API (este ejemplo vincula la oportunidad solo a un posible cliente). Solicitud:

```xml
<soapenv:Envelope xmlns:soapenv="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:mkt="<http://www.marketo.com/mktows/">
   <soapenv:Header>
      <mkt:AuthenticationHeader>
         <mktowsUserId>\*\*\*</mktowsUserId>
         <requestSignature>\*\*\*</requestSignature>
         <requestTimestamp>2014-11-20T15:44:08-07:00</requestTimestamp>
      </mkt:AuthenticationHeader>
   </soapenv:Header>
   <soapenv:Body>
      <mkt:paramsSyncMObjects>
         <mObjectList>
            <!--Zero or more repetitions:-->
            <mObject>
               <type>Opportunity</type>
               <externalKey>
                  <name>opportunityId</name>
                  <value>Opportunity_5</value>
               </externalKey>
               <attribList>
                  <attrib>
                     <name>opportunityId</name>
                     <value>Opportunity_5</value>
                  </attrib>
                  <attrib>
                     <name>Name</name>
                     <value>Opportunity 5 for ACME</value>
                  </attrib>
                  <attrib>
                     <name>IsClosed</name>
                     <value>1</value>
                  </attrib>
                  <attrib>
                     <name>IsWon</name>
                     <value>1</value>
                  </attrib>
                  <attrib>
                     <name>Amount</name>
                     <value>1500</value>
                  </attrib>
                  <attrib>
                     <name>CloseDate</name>
                     <value>2014-10-24</value>
                  </attrib>
                  <attrib>
                     <name>ExpectedRevenue</name>
                     <value>1500</value>
                  </attrib>
                  <attrib>
                     <name>Probability</name>
                     <value>100</value>
                  </attrib>
               </attribList>
               <associationList>
                  <mObjAssociation>
                     <mObjType>Company</mObjType>
                     <externalKey>
                        <name>cAccountFSID</name>
                        <value>C123</value>
                     </externalKey>
                  </mObjAssociation>
               </associationList>
            </mObject>
             <mObject>
               <type>OpportunityPersonRole</type>
               <attribList>
                  <attrib>
                     <name>IsPrimary</name>
                     <value>1</value>
                  </attrib>
                  <attrib>
                     <name>Role</name>
                     <value>Marketing Manager</value>
                  </attrib>
               </attribList>
               <associationList>
                  <mObjAssociation>
                     <mObjType>Lead</mObjType>
                     <!--id>6</id-->
                     <externalKey>
                      <name>FSPID</name>
                      <value>12346</value>
                     </externalKey>
                  </mObjAssociation>
                  <mObjAssociation>
                     <mObjType>Opportunity</mObjType>
                     <externalKey>
                      <name>opportunityId</name>
                      <value>Opportunity_5</value>
                    </externalKey>
                  </mObjAssociation>
               </associationList>
            </mObject>
         </mObjectList>
         <operation>UPSERT</operation>
      </mkt:paramsSyncMObjects>
   </soapenv:Body>
</soapenv:Envelope>
```

Respuesta:

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
   <SOAP-ENV:Body>
      <ns1:successSyncMObjects>
         <result>
            <mObjStatusList>
               <mObjStatus>
                  <id>41</id>
                  <externalKey>
                     <name>opportunityId</name>
                     <value>Opportunity_5</value>
                  </externalKey>
                  <status>CREATED</status>
               </mObjStatus>
               <mObjStatus>
                  <id>12</id>
                  <status>CREATED</status>
               </mObjStatus>
            </mObjStatusList>
         </result>
      </ns1:successSyncMObjects>
   </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Publicado el _2014-11-26_ por _Jep_

## Solicitudes de API de REST multiproceso

Si desea mejorar el rendimiento al llamar a la API de Marketo, puede realizar solicitudes simultáneas. Este método permite obtener más datos en un período de tiempo más corto. Al realizar una solicitud de API, parte del tiempo de ida y vuelta entre el cliente y el servidor es el tiempo de transferencia en el cable. Por lo tanto, si podemos reducir el tiempo de transferencia en el cable para las solicitudes en conjunto, mejoramos el rendimiento. El siguiente código de ejemplo muestra cómo hacerlo en Ruby. Utiliza EventMachine, que es una biblioteca de procesamiento de eventos [que se usa para realizar solicitudes multiproceso](https://github.com/igrigorik/em-http-request/wiki/Parallel-Requests). El ejemplo siguiente llama a la [API de actividades de cliente potencial](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadActivitiesUsingGET) y realiza dos solicitudes simultáneas. Este método elimina el tiempo de transferencia del cliente al servidor para la segunda solicitud. Para ello, incluye la segunda solicitud al mismo tiempo que la primera. Las respuestas de la API se escriben en un archivo de texto.

```java
require 'em-http-request'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com>"
endpoint = "/rest/v1/activities.json"
# Replace with your access token
auth_token =  "?access_token=" + "ac756f7a-d54d-41ac-8c3c-f2d2a39ee325:ab"
# Specify datetime needed as nextPageToken
since_date_time = ["&nextPageToken=A5YMOYZQBOGD2OSYYBYDAQGEMGLBDGDANAABQGRAQWAAKKID", "&nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ"]
# Specify activities needed
activity_type_ids = "&activityTypeIds=1&activityTypeIds=12"
requesturl_a = marketo_instance + endpoint + auth_token + since_date_time.at(0) + activity_type_ids
requesturl_b = marketo_instance + endpoint + auth_token + since_date_time.at(1) + activity_type_ids

# Make request
EventMachine.run do
  http1 = EventMachine::HttpRequest.new(requesturl_a).get
  http2 = EventMachine::HttpRequest.new(requesturl_b).get

# When API response is received, write response to a text file
  http1.callback {
  File.open('response1.txt', 'w') do |t|
  t.puts http1.response
  end }

  http2.callback {
  File.open('response2.txt', 'w') do |t|
  t.puts http2.response
  end }
end
```

Publicado el _2014-12-03_ por _Murta_

## Solicitudes de API de ajuste de rendimiento

En este artículo se describen las estrategias para mejorar el rendimiento al solicitar datos de la API de Marketo. Sin embargo, debe sopesar los beneficios de estas estrategias con la restricción operativa de los límites diarios de la API de Marketo.
**Estrategia 1: Solicitar menos datos en cada llamada de API** En general, a medida que se solicitan más datos en una llamada de API, aumenta el tiempo que tarda el servidor de Marketo en buscar los datos en la base de datos. Si realiza una llamada de API con intervalos de fechas, como la API de SOAP [getMultipleLeads](/help/soap-api/getmultipleleads.md), reduzca el intervalo de tiempo por llamada y compense con más llamadas. Por ejemplo, en lugar de solicitar datos del 1 de junio al 1 de julio, solicite un solo día a la vez, por ejemplo, una llamada del 1 al 2 de junio y, a continuación, otra llamada del 2 al 1 de junio. Si realiza una llamada de API que devuelve datos de los campos de posible cliente de Marketo, solo debe solicitar los campos necesarios. Cada campo de posible cliente adicional aumenta gradualmente la cantidad de tiempo que tarda una llamada de API. Otro método es reducir el tamaño del lote o el número de posibles clientes solicitados por llamada.
**Estrategia 2 - Realizar solicitudes simultáneas** Para mejorar el rendimiento y extraer más datos a la vez. Puede realizar solicitudes simultáneas a la API. Este método reduce el tiempo en las solicitudes de API de cable que se invierten en acumulado. Por ejemplo, supongamos que está realizando solicitudes a la función Obtener varios posibles clientes por tipo de filtro. Puede realizar solicitudes simultáneas para una solicitud consultando posibles clientes 1 a 300 y para otra solicitud consultando posibles clientes 301 a 600.
**Estrategia 3 - Datos de caché** Algunos datos de Marketo cambian con menos frecuencia, como la lista de campos de posibles clientes, que otros datos, como los datos de actividad de posibles clientes. Si almacena en caché datos que se actualizan con menos frecuencia, reduce el número de llamadas de API que debe realizar. También obtendrá un mejor rendimiento porque buscar los datos localmente suele ser más rápido que acceder a ellos desde un servicio web remoto.

Publicado el _2014-12-05_ por _Murta_

## Envío de datos de formulario Marketo a Google Analytics

En Google Analytics, puede enviar eventos de datos personalizados y, a continuación, utilizar los datos para segmentar y analizar el rendimiento del sitio web. El siguiente fragmento de código de JavaScript le permite insertar automáticamente datos de formulario de Marketo 2.0 en Google Analytics después de que un visitante envíe un formulario web. A continuación se indica cómo configurarlo.

**Paso uno** Inserte la etiqueta JavaScript en cualquier página que incluya Marketo Forms en la parte inferior del código (antes de la etiqueta ). JavaScript solo envía campos no ocultos (sendHiddenFields : false). Esto se puede ajustar cambiando sendHiddenFields de false a true. También puede seleccionar campos que desee excluir añadiendo ID de campo adicionales en la matriz &quot;fieldsToExclude&quot;.

```javascript
function pushFormDataToGa(a){
setTimeout(function () {
document.getElementsByTagName('form')[0].getElementsByClassName(a.submitButton)[0].addEventListener('click', function() {
  allFields = document.getElementsByTagName('form')[0].getElementsByTagName('input');
  for(i=0;i<allFields.length;i++){
   if( (allFields[i].type !="hidden" && allFields[i].type !="submit" && allFields[i].value !="" && a.fieldsToExclude.indexOf(allFields[i].id) === -1  ) || (allFields[i].type === "hidden" && a.sendHiddenFields) ){
    console.log( allFields[i].name + ": "  + allFields[i].value);
    if(typeof(_gaq) != "undefined"){
    //Classic
    _trackEvent("Marketo Form Submission", allFields[i].value , allFields[i].name
{'nonInteraction': 1});
    }else if(typeof(ga) !="undefined"){
    //Universal
    ga('send', 'event',"Marketo Form Submission", allFields[i].value , allFields[i].name, {'nonInteraction': 1});
}}}}, false);
}, 3000);}
pushFormDataToGa({
 submitButton: "mktoButton",
 fieldsToExclude: ["Email","LastName", "FirstName"],
 sendHiddenFields : false
});
```

**Paso dos** Los datos de GA aparecen en la sección Informes. Vaya a Comportamiento > Eventos > Eventos principales. **Restricciones de script:**. Este ejemplo de código solo es compatible con [Marketo Forms 2.0](/help/javascript-api/forms-api-reference.md). - Debido a la política de privacidad de Google, no puede enviar ninguna información personal (correo electrónico o nombre). Además de posibles problemas de privacidad, se trata de información personal identificable y, por lo tanto, infringe los [Términos de servicio de Google Analytics](https://marketingplatform.google.com/about/analytics/terms/us/):

&quot;El Cliente no usará (y no permitirá que terceros) el Servicio para rastrear, recopilar o cargar datos que identifiquen personalmente a un individuo (como un nombre, dirección de correo electrónico o información de facturación), u otros datos que Google pueda vincular razonablemente a dicha información.&quot;

Publicado el _16-12-2014_ por _Yanir_

## Agregar un campo de nombre completo a un formulario de Marketo

Sabemos que los formularios web más cortos mejoran las tasas de conversión. El ejemplo de código JavaScript siguiente le permite reducir aún más los formularios combinando los campos Nombre y Apellido en un campo Nombre completo. Cuando los visitantes escriben su nombre completo, la secuencia de comandos divide automáticamente el texto en campos de nombre y apellido. Para los visitantes conocidos, la secuencia de comandos une el nombre y los apellidos y, a continuación, cópielos en el nuevo campo para que no tengan que rellenar el campo de nuevo. A continuación se indica cómo configurarlo.

**Paso uno**: cree un nuevo campo personalizado en Marketo llamado Nombre completo. No es necesario crearlo en la plataforma CRM, ya que el script solo utilizará este campo para mostrar el nombre completo.
**Paso dos** Agregue este campo a todos sus formularios web. Establezca los campos de nombre y apellido como ocultos. En JavaScript, cambie la configuración &quot;splitFullName&quot; para que contenga los 3 nombres de campo. Nota: Asegúrese de que estos nombres no aparezcan en ninguna otra parte de la página.
**Paso tres** Inserte JavaScript en todas las páginas de aterrizaje en la parte inferior del código, antes de la etiqueta.

```javascript
<script>
MktoForms2.whenReady(function (form){
        function splitFullName(a,b,c){
                String.prototype.capitalize = function(){
                        return this.replace( /(^|s)([a-z])/g , function(m,p1,p2){ return p1+p2.toUpperCase(); } );
                };
                document.getElementsByName[c](0).oninput=function(){
                        var fullName = document.getElementsByName[c](0).value;
                        if((fullName.match(/ /g) || []).length ===0 || fullName.substring(fullName.indexOf(" ")+1,fullName.length) === ""){
                                var first = fullName.capitalize();;
                                var last = "null";
                        }else if(fullName.substring(0,fullName.indexOf(" ")).indexOf(".")>-1){
                                var first = fullName.substring(0,fullName.indexOf(" ")).capitalize() + " " + fullName.substring(fullName.indexOf(" ")+1,fullName.length).substring(0,fullName.substring(fullName.indexOf(" ")+1,fullName.length).indexOf(" ")).capitalize();
                                var last = fullName.substring(first.length +1,fullName.length).capitalize();
                        }else{
                                var first = fullName.substring(0,fullName.indexOf(" ")).capitalize();
                                var last = fullName.substring(fullName.indexOf(" ")+1,fullName.length).capitalize();
                        }
                        document.getElementsByName[a](0).value = first;
                        document.getElementsByName[b](0).value = last;
                };
                //Initial Values
                if(document.getElementsByName[c](0).value.length < 2 && document.getElementsByName[b](0).value.length.length >2 && document.getElementsByName[a](0).value.length.length >2 ){
                        var first = document.getElementsByName[a](0).value.capitalize();
                        var last = document.getElementsByName[b](0).value.capitalize();
                        var fullName =  first + " " + last ;
                        console.log(fullName);
                        document.getElementsByName[c](0).value = fullName;
                }
        }
        splitFullName("FirstName","LastName","leadFullName");
});
</script>
```

Nota: Este código solo funciona con Marketo Forms 2.0.

Publicado el _16-12-2014_ por _Yanir_

## Utilice cURL para importar posibles clientes a través de la API de REST

¿Desea importar posibles clientes desde un archivo CSV a través de la API de REST, pero ha notado que esto es difícil de hacer con la extensión Postman Chrome? En este post, explicamos cómo hacerlo con cURL.

1. [Descargue e instale cURL](https://curl.se/download.html), una herramienta de línea de comandos que usamos para insertar datos en la API REST de Marketo.
1. Abra la línea de comandos y, a continuación, vaya a la ubicación en la que se encuentra el archivo CSV. Los encabezados de columna del archivo CSV deben coincidir con los nombres de los campos de API, no con los nombres de los campos de Marketo.
1. Necesita un token de acceso. Inicie sesión en Marketo, vaya a Administración y, a continuación, a LaunchPoint. Busque el usuario de la API de REST y haga clic en &quot;Ver detalles&quot;. Haga clic en el botón Obtener token.
1. También necesitará el punto final REST específico de su instancia de Marketo. Inicie sesión en Marketo, vaya a Administración y, a continuación, a Servicios web. En la sección marcada como &quot;API REST&quot; encontrará la dirección URL de extremo.
1. En la línea de comandos, siga este formato para la llamada cURL. Reemplace `<accesstoken>` con su token de acceso del paso tres y reemplace `<REST API Endpoint URL>` con su dirección URL de extremo de API de REST del paso cuatro. Encontrará más información [aquí](https://developer.adobe.com/marketo-apis/api/mapi/#operation/importLeadUsingPOST). &quot;/bulk&quot; sustituirá a &quot;/rest&quot; al final de la URL de punto de conexión. Si tiene el extremo establecido para /rest/bulk, devuelve un error.

`curl -i -F format=csv -F file=@leaddata.csv -F access_token=<accesstoken> <REST API Endpoint URL>/bulk/v1/leads.json`

Publicado el _16-12-2014_ por _Jordan_

## Añadir una alerta de confirmación a un formulario de Marketo

Supongamos que cuando un usuario hace clic en el botón &quot;Enviar&quot; de un formulario de Marketo, desea mostrar una notificación que pregunte al usuario si &quot;¿Está realmente bien enviar?&quot;. Esto es posible implementando unas pocas líneas de JavaScript, que mostrarán un cuadro de confirmación cuando alguien haga clic en el botón Send. A continuación se muestra un ejemplo de cómo hacerlo. Agregue la función onSubmit al formulario de Marketo como se muestra a continuación. Para obtener más información acerca de la API de Marketo Forms, [consulte la documentación para desarrolladores](/help/javascript-api/forms-api-reference.md).

```javascript
<script src="//app-e.marketo.com/js/forms2/js/forms2.js"></script>
<form id="mktoForm_19"></form>
<script>
MktoForms2.loadForm("//app-e.marketo.com", "212-RBI-463", 19,function(form){

//Add this function to your Marketo form script
form.onSubmit(function(){
alert("Do you really want to submit the form?");
});
});
</script>
```

Publicado el _17-12-2014_ por _David_

## Mostrar mensaje de agradecimiento sin una página de aterrizaje de seguimiento

Normalmente, cuando se utilizan formularios Marketo Forms, se crean dos páginas de aterrizaje: una para colocar el formulario y otra para redirigirlo una vez completado. Sin embargo, en algunos casos, es posible que no desee tener dos páginas de aterrizaje independientes, pero muy similares, que mantener. En realidad, puede utilizar la misma página de aterrizaje para el formulario y para el mensaje de agradecimiento mediante la API de JavaScript de Forms 2.0. Para ello, cree primero la página de aterrizaje de registro y el formulario, y colóquelo en la página de aterrizaje como lo haría normalmente. A continuación, añada un elemento HTML a la página. En este elemento, agregamos código que se activa en el momento en que se envía el formulario. A continuación, ocultará el formulario y mostrará una etiqueta oculta <div> que contiene el mensaje de agradecimiento. La JavaScript debería tener un aspecto similar al siguiente:

```javascript
//Edit host with your Marketo instance info
<script src="//<host>/js/forms2/js/forms2.js"></script>
<script>
MktoForms2.whenReady(function (form){
 //Add an onSuccess handler
  form.onSuccess(function(values, followUpUrl){
   //get the form's jQuery element and hide it
   form.getFormElem().hide();
   document.getElementById('confirmform').style.visibility = 'visible';
   //return false to prevent the submission handler from taking the lead to the follow up url.
   return false;
 });
});
</script>
```

Editar el texto del mensaje de agradecimiento.

`<div id="confirmform" style="visibility:hidden;"><p><strong>Thank you. Check your email for details on your request.</strong></p></div>`

En el ejemplo de código puede editar el nombre de host y el mensaje de agradecimiento. La primera debe hacer referencia a su instancia de Marketo (por ejemplo, &quot;//app-sj06.marketo.com/js/forms2/js/forms2.js&quot;) y la segunda debe contener el texto de agradecimiento que desee mostrar una vez completado el formulario. El texto se muestra en la página de aterrizaje en la posición exacta en la que coloca el elemento HTML, por lo que asegúrese de editarlo en la hoja de propiedades. También debe asegurarse de que la capa del elemento HTML sea más pequeña que la del formulario. De forma predeterminada, ambos se colocarán en la Capa 15, por lo que estará a salvo si crea su elemento HTML en la Capa 11. Si no lo hace, no podrá escribir ningún cuadro de campo de formulario que se superponga con el mensaje de agradecimiento. No es necesario cambiar el tipo de seguimiento en el formulario o en la página de aterrizaje, ya que JavaScript sobrescribirá esa configuración. Para obtener más información acerca de la API de Marketo Forms, consulte [documentación para desarrolladores](/help/javascript-api/forms-api-reference.md).

Publicado el _19_-12-2014 por _Kristin_

## Resaltar proyectos abiertos de Source creados en Marketo Platform

Esta es la primera publicación de una serie en curso que resalta los proyectos de código abierto creados en torno a la plataforma Marketo por la comunidad de desarrolladores. Mantenemos [una lista en la cuenta de GitHub de Marketo](https://github.com/Marketo/Community-Supported-Client-Libraries) donde realizamos un seguimiento de las bibliotecas de cliente y los proyectos creados por la comunidad de desarrolladores de Marketo. A continuación se muestran tres proyectos desarrollados en torno a las API de REST y SOAP de Marketo. Daniel Chesterton creó [una biblioteca de cliente en PHP para la API REST de Marketo](https://github.com/dchesterton/marketo-rest-api). Actualmente, la biblioteca de cliente tiene cobertura para 12 extremos de API de REST.El ** Kyle Halstvedt de Elixiter creó un proyecto para [extraer posibles clientes de las listas estáticas de Marketo en una hoja de cálculo de Google](https://github.com/Elixiter/mkto_google-spreadsheet). El proyecto de Kyle utiliza la API de REST de Marketo.  David Santoso creó una joya [Ruby para la API de SOAP de Marketo.](https://github.com/davidsantoso/markety) Este proyecto puede ayudarte a integrar más rápidamente la API de Marketo SOAP con la aplicación Ruby on Rails.  Estamos encantados de ver más proyectos creados por la comunidad de desarrolladores en la plataforma Marketo. Si está trabajando en un proyecto de código abierto para Marketo Platform, [envíelo a este repositorio de GitHub mediante una solicitud de extracción](https://github.com/Marketo/Community-Supported-Client-Libraries).

Publicado el _2015-01-02_ por _Murta_

## Cambio dinámico del contenido de la página según la ubicación de un usuario

Supongamos que desea cambiar dinámicamente el número de teléfono de una página de aterrizaje en función de dónde se encuentre un usuario. Por ejemplo, si la persona está en California, le gustaría mostrarle el número de teléfono de su oficina de California en la página de aterrizaje, pero si está en Japón, le gustaría mostrarle el número de teléfono de la oficina japonesa.  Una manera de implementar esto es usar JavaScript y la [API de geolocalización HTML5](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API). La ventaja de este enfoque es que podemos crear una página de aterrizaje y cambiarla dinámicamente en función de la ubicación de un usuario, en lugar de varias páginas de aterrizaje estáticas. A continuación, se muestran los detalles técnicos de la implementación. Creamos un objeto para ubicaciones de oficinas con coordenadas de latitud y longitud, y un segundo objeto con números de teléfono de oficina. En producción, sería una práctica recomendada combinar estos dos objetos en uno solo.

```json
//Coordinates for Marketo offices
var officeLocations = {
    "San Mateo": {latitude: 37.5596465, longitude: -122.2870142},
    "Atlanta": {latitude: 33.8547013, longitude: -84.35552349999999},
    "Tokyo": {latitude: 35.6895, longitude: 139.6917},
    "Dublin": {latitude: 53.3478, longitude: -6.2603097},
    "Sydney": {latitude: -33.873651, longitude: 151.2068896},
    "Portland": {latitude: 45.512089, longitude: -122.6763367},
    "Tel Aviv": {latitude: 32.0852999, longitude: 34.78176759999999}
}

//Phone numbers for Marketo offices
var officePhoneNumbers = {
    "San Mateo": "+1-650-376-2300",
    "Atlanta": "+1-877-260-6586",
    "Tokyo": "+81-03-6759-8280",
    "Dublin": "+353-1-242-3000",
    "Sydney": "+61-2-9045-2711",
    "Portland": "+1-877-260-6586",
    "Tel Aviv": "+1-877-260-6586"
}
```

Creamos un método para solicitar la ubicación de un usuario. Para gestionar errores, si no se puede acceder a la ubicación del usuario, se establece de forma predeterminada la sede central de Marketo y su número de teléfono.

```javascript
//Method to get user's current location. Returns a position object with user's geo coordinates
function getLocation() {
    if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(findNearestOffice);
    } else {
        x.innerHTML = "Marketo Location: San Mateo
Marketo Phone Number: +1-877-260-6586";
    }
}
```

Por último, creamos un método para encontrar la oficina más cercana a la ubicación del usuario y, a continuación, devolver el número de teléfono de la oficina más cercana en la página. Este método usa el método findNearest de [Geolib, que es una biblioteca JavaScript que proporciona operaciones geoespaciales](https://github.com/manuelbieh/Geolib).

```javascript
//Find nearest Marketo office to user's location
function findNearestOffice(position) {
        var nearestOffice = geolib.findNearest({latitude: position.coords.latitude, longitude: position.coords.longitude}, officeLocations);
        x.innerHTML = "Marketo Location: " + nearestOffice.key + "
Marketo Phone Number: " +  officePhoneNumbers[nearestOffice.key];
}
```

Esta es la implementación completa. Almacenamos en déclencheur el método getLocation cuando el usuario hace clic en el botón de la página. Este [repositorio de GitHub](https://github.com/MurtzaM/Find-Nearest-Marketo-Office) tiene los archivos necesarios para configurar esta demostración.

Publicado el _2014-12-20_ por _Murta_

## Seguimiento de posibles clientes y varios dominios

El código de seguimiento Munchkin de Marketo le ayuda a realizar un seguimiento de las visitas a su sitio web. Es probable que quiera usar el código de seguimiento de Munchkin para cookie los posibles clientes anónimos de la mayoría o de todas las páginas del sitio web. Vamos a ver cómo funciona Munchkin. Las visitas a la página se registran para los posibles clientes existentes, y una visita a la página de un visitante sin cookies hará que se cree y almacene una nueva cookie y se cree un nuevo posible cliente anónimo en la base de datos de Marketo. El rastreador de Munchkin cookie automáticamente a un visitante si aún no tiene una cookie para el dominio actual. En Marketo, registra el evento (hacer clic en un vínculo, visitar una página web o un nuevo posible cliente) en el registro de actividad del posible cliente. El valor almacenado en la cookie es único para un visitante determinado. El valor es una combinación del ID de seguimiento de cuenta de Munchkin único, el nombre de dominio, la marca de tiempo y el entero aleatorio.
**¿Qué sucede si tengo varios dominios?** Supongamos que tiene dos sitios que desea rastrear: `<www.apples.com>` y `<www.bananas.com>`. Puede colocar el código de seguimiento en ambos sitios, aunque debe tener en cuenta lo siguiente. Las cookies de Marketo son &quot;cookies de origen&quot; y, por lo tanto, son específicas del dominio. Esto significa que un visitante del sitio 1 se creará como posible cliente anónimo en Marketo, si ese mismo posible cliente se dirige al sitio 2, se creará un segundo posible cliente anónimo independiente en Marketo. Si el posible cliente rellena un formulario en el sitio 1, este registro se conoce, el registro anónimo del sitio 2 permanecerá y seguirá acumulando visitas posteriores a ese sitio. Si el posible cliente luego rellena un formulario en el sitio 2 con la misma dirección de correo electrónico que se utilizó en el sitio 1, entonces ambos posibles clientes conocidos se combinarán automáticamente y se rastreará todo el comportamiento pasado y futuro en un único registro en Marketo. Ambos ID de cookie están vinculados al mismo posible cliente, y toda la actividad web (de cualquier dominio) estará en ese posible cliente.
**¿Qué sucede con varios subdominios?** subdominios no son un problema. Usemos Marketo.com como ejemplo. (Filmado en inglés) Tiene varios subdominios para diferentes idiomas, como fr.marketo.com y de.marketo.com. Con los subdominios, toda la actividad se registra con el mismo registro o cookie de posibles clientes.

Publicado el _2015-01-13_ por _David_

## Cambiar el color del texto de sugerencia en un formulario Marketo Forms

Supongamos que desea cambiar el color del texto de sugerencia (también denominado texto de marcador de posición) en Forms 2.0. Esto es posible mediante CSS personalizado. Por ejemplo, en la captura de pantalla siguiente, hice que el texto de sugerencia en este formulario de Marketo fuera azul. Existen tres opciones para hacerlo en función de cómo utilice Marketo Forms.

**Opción 1: si está incrustando un formulario de Marketo, agregue la CSS siguiente directamente a su archivo CSS principal.**

```css
::-webkit-input-placeholder {
  color: blue;
}
::-moz-placeholder {
  color: blue;
}
:-ms-input-placeholder {
  color: blue;
}
:-moz-placeholder {
  color: blue;
}
```

**Opción 2: al incrustar un formulario de Marketo, puede agregar la CSS directamente en la página entre las etiquetas `<style></style>` de la sección `<head>`.**

```css
<style>
::-webkit-input-placeholder {
  color: blue;
}
::-moz-placeholder {
  color: blue;
}
:-ms-input-placeholder {
  color: blue;
}
:-moz-placeholder {
  color: blue;
}
</style>
```

**Opción 3: si utiliza un formulario Marketo en una página de aterrizaje de Marketo, puede agregar este CSS personalizado a través de la interfaz de usuario de Marketo.** Busque la página de aterrizaje en el árbol de navegación de Marketo. A continuación, haga clic en Editar borrador. Haga clic en Editar etiquetas Meta de página. Agregue el CSS siguiente a la sección HTML de HEAD personalizado. Se deben incluir las etiquetas `<style></style>`.

```css
<style>
::-webkit-input-placeholder {
  color: blue;
}
::-moz-placeholder {
  color: blue;
}
:-ms-input-placeholder {
  color: blue;
}
:-moz-placeholder {
  color: blue;
}
</style>
```

Haga clic en Aprobar borrador. Ahora, cuando visita la página de aterrizaje de Marketo, el texto de sugerencia es del color definido en CSS. Para obtener más información acerca de Marketo Forms, [visite la documentación](/help/javascript-api/forms-api-reference.md).

Publicado el _2015-01-14_ por _Murta_

## Obtener datos de actividad mediante la API de REST

Supongamos que desea obtener todos los posibles clientes que se agregaron a una lista este mes. Con la [API de REST de obtener actividades de posible cliente](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadActivitiesUsingGET), puede obtener estos datos. Antes de llamar a la API de obtener actividades de posible cliente, es necesario obtener un token de acceso de la API de autenticación y también un token de fecha de inicio de la [Obtener token de paginación API](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getActivitiesPagingTokenUsingGET). A continuación se muestra un código de ejemplo en Ruby que recorre los puntos finales de API individuales a los que tendría que llamar para devolver todos los posibles clientes agregados a una lista este mes. 1. Obtenga el token de acceso**

```ruby
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com/identity/oauth/token?grant_type=client_credentials>"
# Relace with your client id
client_id = "99985d09-22a9-3jl2-84av-f5baae7c3a45"
# Replace with your your  client secret
client_secret = "tZPVrKiEmUDezE18yZfeaPlTJ2vKn2fw"
request_url = marketo_instance + "&client_id=" + client_id + "&client_secret=" + client_secret

# Make request
response = RestClient.get request_url

# Parse reponse and return only access token
results = JSON.parse(response.body)
access_token = results["access_token"]
puts access_token
```

1. Obtener token de paginación

```ruby
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com>"
endpoint = "/rest/v1/activities/pagingtoken.json"
# Replace with your access token
auth_token =  "?access_token=" + "ac756f7a-d54d-41ac-8c3c-f2d2a39ee325:ab"
# Specify date
since_date_time = "&sinceDatetime=2015-01-01T00:00:00-08:00"
request_url = marketo_instance + endpoint + auth_token + since_date_time

# Make request
response = RestClient.get request_url

# Returns Marketo API response
puts response
```

1. Obtener datos de actividad** Para determinar el id. de tipo de actividad necesario para esta llamada, consulte la [API de tipos de actividad obtenidos](/help/rest-api/activities.md). La API de obtención de tipos de actividad devuelve un esquema con todos los tipos de actividad y los ID asociados. Por ejemplo, devuelve el ID 12 para los nuevos posibles clientes creados y el ID 1 para la visita a la página web.

```java
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com>"
endpoint = "/rest/v1/activities.json"
# Replace with your access token
auth_token =  "?access_token=" + "ac756f7a-d54d-41ac-8c3c-f2d2a39ee325:ab"
# Specify datetime needed as nextPageToken
since_date_time = "&nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ"
# Specify activities needed
activity_type_ids = "&activityTypeIds=24"
request_url = marketo_instance + endpoint + auth_token + since_date_time + activity_type_ids

# Make request
response = RestClient.get request_url

# Returns Marketo API response
puts response
```

1. La API de obtención de actividades de posible cliente devuelve un token de paginación con cada respuesta que puede utilizar para paginar por el conjunto de resultados.** Para obtener más información, consulte la [documentación de la API de REST](/help/rest-api/rest-api.md).

Publicado el _2015-01-20_ por _Murta_

## Resaltar proyectos abiertos de Source creados en la plataforma de Marketo: segunda parte

Este es el segundo post de una serie en curso que resalta los proyectos de código abierto construidos alrededor de la plataforma Marketo por la comunidad de desarrolladores. Mantenemos [una lista en la cuenta de GitHub de Marketo](https://github.com/Marketo/Community-Supported-Client-Libraries) donde realizamos un seguimiento de las bibliotecas de cliente y los proyectos creados por la comunidad de desarrolladores de Marketo. A continuación se muestran tres proyectos desarrollados en torno a las API de Marketo SOAP y Munchkin. [PunchTab](https://www.punchtab.com/) creó [una biblioteca de cliente en Python para la API de Marketo SOAP](https://github.com/PunchTab/suds-marketo). [Flickerbox](https://www.flickerbox.com/) creó [una biblioteca de cliente en PHP para la API de Marketo SOAP](https://github.com/flickerbox/marketo).* [Richard Morrison](https://x.com/mozz100) creó [un script PHP para obtener datos de clientes potenciales de la API de SOAP de Marketo y luego pasar estos datos al cliente usando JavaScript.](https://github.com/mozz100/marketo-whodat) Este proyecto puede ayudarle a modificar una página basándose en los datos de un usuario en Marketo.  Estamos encantados de ver más proyectos creados por la comunidad de desarrolladores en la plataforma Marketo. Si está trabajando en un proyecto de código abierto para Marketo Platform, [envíelo a este repositorio de GitHub mediante una solicitud de extracción](https://github.com/Marketo/Community-Supported-Client-Libraries).

Publicado el _2015-01-20_ por _Murta_

## Envío de clics en el motor de recomendaciones RTP a Google Analytics

Esta es una solución para que los usuarios de Marketo Real-Time Personalization (RTP) vean clics del motor de recomendación de contenido en Google Analytics. Una vez que un visitante hace clic en la barra de recomendaciones de contenido, se envía un evento a Google Analytics en la categoría de eventos &quot;RTP-Recommendations&quot;. En Analytics, el texto de recomendación (tal y como aparece en la barra) se anexará a la etiqueta de evento, y la dirección URL del recurso recomendado se anexará a la acción de evento. El script funciona tanto para Google Analytics clásico como para Google Universal Analytics. Esta etiqueta debe pegarse al final del código de página de HTML, de modo que sea la última etiqueta antes de la etiqueta `</body>`.

```javascript
$( document ).ready(function() {
if(document.getElementsByClassName("insightera-bar-content").length
 >0){
document.getElementsByClassName("insightera-bar-content")[0].getElementsByTagName('a')[0].addEventListener("click",
 function(){
assetName
 = document.getElementsByClassName("insightera-bar-content")[0].getElementsByTagName('a')[0].innerText;
assetURL
 = document.getElementsByClassName("insightera-bar-content")[0].getElementsByTagName('a')[0].href;
assetURL=
 assetURL.substring(assetURL.lastIndexOf("/"),assetURL.indexOf("?iesrc"));
console.log(assetName

 * " | " + assetURL);
if(typeof(_gaq)
 != "undefined"){
//Classic
_trackEvent("RTP-Recommendations",
 assetName , assetURL , {'nonInteraction': 1});
}else
 if(typeof(ga) !="undefined"){
//Universal
ga('send',
 'event',"RTP-Recommendations", assetName , assetURL, {'nonInteraction': 1});
}
});
}
});
```

Publicado el _2015-01-22_ por _Yanir_

## Uso de la API de REST de Marketo con Boomi: Obtención y almacenamiento de un token de autenticación REST

La configuración de una exportación automática de posibles clientes que cumplen un determinado criterio es un caso de uso muy común con Marketo. Aunque esto no se puede hacer actualmente en la interfaz de Marketo, es bastante sencillo de lograr con la herramienta de terceros como Dell Boomi, una lista estática con algunas campañas de administración de datos y la API de REST de Marketo. ¿La API de REST? ¡Pensé que Boomi no tenía un Conector de API REST de Marketo! Bueno, actualmente no lo hace, pero es posible lograr lo mismo utilizando el conector HTTP y definiendo manualmente las formas de respuesta jSON. El primer paso es configurar la instancia de Marketo para que use la API de REST como se describe en la [página para desarrolladores de Marketo de API de REST](/help/rest-api/rest-api.md). También asumiré que tiene acceso a una cuenta de Dell Boomi y que posee las habilidades de Boomi para crear este tipo de procesos de integración. El proceso final tiene el siguiente aspecto e incluirá llamadas a las siguientes operaciones de la API de REST de Marketo, cada una de las cuales tiene una forma de respuesta jSON asociada que se puede encontrar en el sitio para desarrolladores. Para ahorrar tiempo, los he enumerado a continuación. Ejemplo JSON para [Authentication](/help/rest-api/authentication.md)

```json
{
  "access_token": "",
  "token_type": "",
  "expires_in": 0,
  "scope": ""
}
```

Ejemplo de JSON para [Obtener varios posibles clientes por id. de lista](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists)

```json
{
  "requestId": "",
  "success": true,
  "nextPageToken": "",
  "result": [
    {
      "id": 0,
      "email": "",
      "firstName": "",
      "lastName": ""
    },
    {
      "id": 0,
      "email": "",
      "firstName": "",
      "lastName": ""
    },
    {
      "id": 0,
      "email": "",
      "firstName": "",
      "lastName": ""
    }
  ]
}
```

Ejemplo de JSON para [Quitar posibles clientes de la lista](https://developer.adobe.com/marketo-apis/api/mapi/#operation/removeLeadsFromListUsingDELETE)

```json
{
  "requestId": "",
  "success": true,
  "result": [
    {
      "id": 1,
      "status": ""
    },
    {
      "id": 2,
      "status": "",
      "reasons": [
        {
          "code": "",
          "message": ""
        }
      ]
    }
  ]
}
```

Definir propiedades: antes de empezar a llamar a REST, es importante externalizar y encapsular las variables que está utilizando. He definido lo siguiente como se muestra a continuación.

* ClientID: Obtenga esto desde su servicio de REST Launchpoint
* Secreto del cliente: obténgalo desde el servicio REST de LaunchPoint
* AccessToken: lo obtenemos de una llamada de REST
* ListID estático: el ID de LISTA de la lista estática en la que operaremos. Obtenga esto desde la dirección URL en Marketo
* Campos: Una lista separada por comas de los campos que el resto del servicio obtiene de Marketo para cada posible cliente. El mío es &quot;id, email, firstName, lastName&quot; * IDStringToDelete: contendrá finalmente el ID de todos los posibles clientes de la lista estática que se utilizarán en su eliminación de la lista
* ActivityTypes: Se utilizará en la parte 2 de este blog, donde me expando en esto!
* SinceDateTime: Se utilizará en la parte 2 de este blog, donde me expando en esto!
* PagingToken: Se utilizará en la parte 2 de este blog, donde me expando en esto!
* Carpeta: saliente: la ruta a la carpeta saliente en el servidor SFTP. En este ejemplo utilizo &quot;/data/exit&quot;. Nos permite parametrizar la operación SFTP para convertirla en genérica.

El token de autenticación: como he mencionado, colocaremos un conector en el lienzo después de crear el proceso con una forma de inicio &quot;Sin datos&quot; (esta es solo una elección personal, me gustan todos los conectores que parecen enchufes británicos).
El conector debe configurarse de la siguiente manera: - El conector es un cliente HTTP GET - La conexión utiliza la URL: `https://123-ABC-456.mktorest.com` (observe no /rest al final para que podamos utilizarlo para llamadas REST y para obtener el token de acceso de identidad. y cambie 123-ABC-456 por la correcta para la instancia de Marketo) - La operación es &quot;Obtener token de autenticación&quot; (nuevo) - Perfil de solicitud = Ninguno - Perfil de respuesta = JSON - Nuevo perfil llamado &quot;Respuesta de token de autenticación&quot; - Tipo de contenido: Sin formato - Método HTTP: GET - Ruta de recurso (añada 4 sin comillas): &quot;identity/oAuth/token?grant_type=client_credentials&amp;client_id=&quot;; &quot;ClientID (variable de reemplazo)&quot;; &quot;&amp;client_secret=&quot;; &quot;ClientSecret (variable de reemplazo)&quot; - Establezca los parámetros en Configurar -> Parámetros —>(+): Set ClientID = ID de cliente de propiedad de proceso; Set ClientSecret = Secreto de cliente de propiedad de proceso . Después de esto, almacene el token de éxito en la variable de propiedades de proceso &quot;AccessToken&quot; como se muestra, extrayéndolo de la respuesta jSON.
El patrón de este paso se repetirá para los pasos siguientes, pero se utilizarán nuevas operaciones con diferentes perfiles de retorno de jSON. De hecho, muchas de las llamadas de REST se tratarán de la misma manera con cambios menores. En la próxima entrega, ampliaremos esto y obtendremos una lista de posibles clientes de una lista estática con REST. Por ahora, ejecute el proceso, pero coloque una forma de parada después de &quot;Set Properties&quot; y ejecute en la depuración para asegurarse de que ve el mismo token que en Marketo. ¡Deberían coincidir perfectamente!

Publicado el _2015-01-26_ por _John_

## Utilice una API de fuentes de Google para agregar una fuente personalizada a una página de aterrizaje de Marketo

**Nota: Esta es una publicación de blog de [Murtza Manzur](https://www.linkedin.com/in/murtzam). Murtza es una empresa de Marketo Developer Evangelist con sede en el área de la bahía de San Francisco.**
Supongamos que está creando una página de aterrizaje en Marketo y que desea utilizar una fuente personalizada. Esto es posible mediante la API de fuentes de Google.  Agregue un método de importación al archivo CSS con referencia a Google Fonts:

`@import url(http://fonts.googleapis.com/css?family=Open+Sans:400,300,600);`

Publicado el _2015-01-26_ por _Murta_

## Poner en mayúscula el nombre de un posible cliente mediante scripts de correo electrónico

Supongamos que un posible cliente introduce su nombre en minúsculas, como &quot;John Doe&quot;. Pero cuando envía una campaña por correo electrónico, le gustaría poner en mayúsculas el nombre del posible cliente en el correo electrónico, como Juan García. Puede poner en mayúscula el nombre de un posible cliente mediante scripts de correo electrónico. Así es como se hace esto.

1. En su programa de correo electrónico, haga clic en la pestaña &quot;Mis tokens&quot;.
1. Cree un nuevo token de script de correo electrónico arrastrando &quot;Script de correo electrónico&quot; del panel derecho al panel central. Asigne un nombre al token.
1. En el cuadro de texto Editar token de script, pegue el código siguiente. En el panel derecho, debajo de Objeto de posible cliente, seleccione la casilla de verificación Nombre. A continuación, haga clic en Guardar.

```javascript
# set($name = ${lead.FirstName})
# set($formattedFirstName = $name.substring(0).toUpperCase())
$formattedFirstName
```

1. Haga referencia al token en su recurso de correo electrónico. Generará el nombre del posible cliente con la primera letra en mayúscula. Para obtener más información sobre las secuencias de comandos por correo electrónico, visite la [documentación sobre secuencias de comandos por correo electrónico](/help/email-scripting.md).

Publicado el _2015-01-26_ por _Murta_

## Obtener todos los posibles clientes de la API de REST de Marketo

Hubo una [pregunta en StackOverflow que preguntaba cómo obtener una lista de todos los posibles clientes de Marketo a través de la API de REST](https://stackoverflow.com/questions/28184900/how-do-i-get-the-list-of-all-the-leads-in-marketo). Puede consultar estos datos usando [Obtener varios posibles clientes por tipo de filtro Extremo de API de REST](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET). A los posibles clientes en Marketo se les asignan identificadores de posibles clientes en orden secuencial empezando por 1. Al usar [Obtener varios posibles clientes por tipo de filtro extremo de API de REST](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET), puede consultar 300 posibles clientes por id. de posible cliente con cada llamada. Debe especificar id como filterType y los id de posibles clientes como filterValues con cada llamada a este extremo. Para obtener todos los posibles clientes, iteraría por el número total de posibles clientes 300 a la vez. Y
Puede obtener el recuento total de posibles clientes en una instancia de Marketo a través de la interfaz de usuario de Marketo. En la IU de Marketo, vaya a la pestaña Base de datos de posibles clientes, haga clic en Listas inteligentes del sistema, haga clic en Lista inteligente de todos los posibles clientes y, finalmente, haga clic en la pestaña &quot;Posibles clientes&quot;. A continuación, haga clic en la columna Id y ordene de forma descendente. Una vez ordenados los posibles clientes, el ID del primer posible cliente será el límite superior del ID de los posibles clientes cuando consulte todos los posibles clientes. Si no tiene acceso a la interfaz de usuario de Marketo para obtener el recuento total de posibles clientes, existe un método [alternativo para obtener este valor mediante la API de REST de obtener actividades de posibles clientes](https://stackoverflow.com/questions/28419967/get-all-leads-programmatically-in-marketo-v1).

1. Primera llamada de API: reemplazar ... con todos los valores intermedios:

`/rest/v1/leads.json?filterType=Id&filterValues=1,2,3,...,298,299,300`

Este es un ejemplo de código en Ruby para la primera llamada.

```java
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com>"
endpoint = "/rest/v1/leads.json"
# Replace with your access token
auth_token =  "?access_token=" + "ac756f7a-d54d-41ac-8c3c-f2d2a39ee325:ab"
# Replace with filter type and values
ids_needed = (1..300).to_a.join(",")
filter_type_and_values = "&filterType=Id&filterValues=" + ids_needed
request_url = marketo_instance + endpoint + auth_token + filter_type_and_values

# Make request
response = RestClient.get request_url

# Returns Marketo API response
puts response
```

1. La segunda llamada de API y cada llamada posterior a la API seguirían el mismo patrón hasta que se alcance el recuento total de posibles clientes:

```
//replace ... with all the values in between
/rest/v1/leads.json?filterType=Id&filterValues=301,302,303,...,598,599,600
```

Para obtener más información, consulte la [documentación de la API de REST](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET).

Publicado el _2015-01-28_ por _Murta_

## Ejecutar acciones de envío de formulario de Iframe a página principal

Hemos visto algunos casos en los que los usuarios utilizan formularios iframe y desean dirigir a los visitantes que rellenaron el formulario a una página de agradecimiento, a un PDF, a un vídeo, etc. El problema es que, como el formulario está incrustado en una página de aterrizaje que es diferente de la principal, la acción solo se produce en la página interna en la que está el formulario. Para solucionarlo, a continuación se muestran dos etiquetas de JavaScript que hemos creado. Inserte en como un elemento de HTML en las páginas de iframe o directamente en la plantilla de página de aterrizaje que utilice para los iframes. Colóquelo antes de la última etiqueta `</body>`. La primera etiqueta realiza la acción en la página principal y la segunda etiqueta la abre en una nueva pestaña.

**Acción de formulario en una página principal**

```javascript
<script>
MktoForms2.whenReady(function (form){
form.onSuccess(function (values, url){
window.parent.location.assign(url);
return false;
           });
});
</script>
```

**Acción de formulario en una nueva ficha**

```javascript
<script>
MktoForms2.whenReady(function (form){
var newWin;
form.onSubmit(function (){
newWin = window.open('about:blank', 'myWindow');
});
form.onSuccess(function (values, url){
newWin.location.replace(url);
return
 false;
});
});
</script>
```

Publicado el _2015-02-02_ por _Yanir_

## Envío de datos de visualización de un vídeo de YouTube al mercado

Supongamos que desea segmentar los posibles clientes en Marketo en función de si han iniciado o finalizado un vídeo específico. Esto se puede hacer con Munchkin, la API de Iframe de YouTube y las listas inteligentes en Marketo. El código de ejemplo de esta publicación le permite enviar eventos de vídeo iniciados y de vídeo finalizados a Marketo a través de Munchkin. Para que esto funcione, Munchkin también debe cargarse en la página para poder empezar a enviar eventos de vista de vídeo a Marketo. El vídeo iniciado y finalizado se mostrará en el registro de actividad del posible cliente. Una vez que los datos se encuentran en Marketo, puede crear una lista inteligente y posibles clientes de segmentos que hayan iniciado o finalizado un vídeo.

1. Obtenga el ID del vídeo de YouTube que desea incrustar.** Desde la dirección URL del vídeo de YouTube que desee utilizar, anote el ID, que es la serie de caracteres aleatorios después de `v=`.
1. Coloque el ID de vídeo de YouTube del paso uno en la octava línea de este ejemplo de código. A continuación, coloque el código antes de `</body>` en el HTML de su página.

```javascript
<div id="player"></div>
<script>
var tag = document.createElement('script');
tag.src = "https://www.youtube.com/iframe_api";
document.getElementsByTagName('head')[0].appendChild(tag);

//Change 'iiqxcjxJ5Us' to video needed
var player, videoId = 'iiqxcjxJ5Us';
function onYouTubeIframeAPIReady() {
player = new YT.Player('player', {
height: '390',
width: '640',
videoId: videoId,
events: {
'onStateChange': onPlayerStateChange
}
});
}

function onPlayerStateChange(event) {
switch( event.data ) {
//Send video started event to Marketo
case YT.PlayerState.PLAYING: Munchkin.munchkinFunction('visitWebPage', {
url: '/video/'+videoId
, params: 'video=started'
}
);
break;
//Send video finished event to Marketo
case YT.PlayerState.ENDED: Munchkin.munchkinFunction('visitWebPage', {
url: '/video/'+videoId
, params: 'video=finished'
}
);
break;
}

}
</script>
```

1. Cree una lista inteligente en Marketo con la URL del vídeo y el evento de visualización que esté buscando como el valor de &quot;Querystring contains&quot;. Para obtener más información sobre la API de YouTube Iframe, [visite la documentación de la API de YouTube](https://developers.google.com/youtube/iframe_api_reference). Para obtener más información sobre Munchkin, [revise la documentación para desarrolladores de Marketo](/help/javascript-api/lead-tracking.md).

Publicado el _2015-02-02_ por _Murta_

## Sugerencias y trucos para la API de Marketo SOAP

NOTA: Esta es una publicación de blog de invitado. [Ed Blachman es Arquitecto Senior](https://www.linkedin.com/uas/login?session_redirect=https%3A%2F%2Fwww.linkedin.com%2Fprofile%2Fview%3Fid%3D2777965) en [TIBCO Software, un conocido proveedor de software empresarial](https://exchange.adobe.com/apps/browse/ec?product=MRKTO). Ed está trabajando en productos que permiten lo que Gartner llama &quot;desarrolladores ciudadanos&quot; para integrar los servicios en la nube que utilizan sin necesidad de hacer ninguna programación ellos mismos. [La API de SOAP de Marketo](/help/soap-api/soap-api.md) es una herramienta poderosa mediante la cual los desarrolladores pueden aprovechar el poder de Marketo e integrarlo con nuestras propias aplicaciones. Entre [la documentación formal](./getting-started.md) y [los recursos de la comunidad](https://nation.marketo.com/), hay mucha información disponible sobre cómo usarla. Cuando empecé, me apoyé mucho en esa información y la encontré invaluable. Sin embargo, en ese proceso, construí algunos consejos y trucos que no había visto en ninguno de esos lugares. Aquí hay algo de lo que descubrí.

**Espacio aislado para desarrolladores** El espacio aislado es, por supuesto, un recurso maravilloso para los desarrolladores de API: un lugar seguro en el que puede experimentar con las características de Marketo, agregando y eliminando objetos sin interferir con las actividades de marketing reales que llevan a cabo los usuarios de Marketo reales de su organización. Sin embargo, la zona protegida no es una panacea.
Por ejemplo, necesitaba compartir nuestro espacio aislado con otro grupo de desarrollo, y esto llevó cierto tiempo, porque se habían acostumbrado a la noción de que eran propietarios del espacio aislado. Finalmente, descubrimos un par de prácticas recomendadas para compartir: - No escriba pruebas que dependan del conocimiento completo del contenido de su espacio aislado. Como recurso compartido, los esquemas pueden estar sujetos a cambios sin previo aviso, así como a entradas completas en la base de datos de posibles clientes, en los programas u otras entidades. Si las pruebas suponen un conocimiento completo de la zona protegida, el ciclo de desarrollo crea períodos de interrupción para los grupos con los que se comparte. Dado que, por lo general, su ciclo de desarrollo no coincidirá con el suyo, esto equivale a acaparar el recurso, no es genial. Tampoco es necesario, si lo piensas bien. - Use una convención para etiquetar todas sus cosas: sus posibles clientes, sus campos de esquema de posibles clientes, sus programas, lo que sea. Si cada uno puede identificar sus propios objetos, y si puede estar de acuerdo con sus coinquilinos en que cada uno de ustedes dejará en paz los objetos de los demás, debe tener una base firme para compartir. Para los posibles clientes, puede crear un campo personalizado y una convención utilizando este campo personalizado para identificar estos posibles clientes como posibles clientes de prueba. En el caso de las listas o programas, puede iniciar los nombres de los objetos con alguna cadena que identifique los objetos como pertenecientes a usted. - Considere la posibilidad de escribir pruebas que limpien después de sí mismas, que primero creen los objetos que le interesan, luego accedan o actualicen o los eliminen selectivamente, y finalmente los eliminen. (Tenga en cuenta que esto no se puede lograr al 100 % en la API de SOAP, porque no todo en la zona protegida, o en una instancia real para el caso, se puede administrar mediante la API de SOAP. Aun así, vale la pena hacer esto tanto como sea posible).

**Instancias reales** El problema con la zona protegida es que no se está usando en producción, por lo que es difícil tener una idea de cómo se ve el uso real en una instancia de Marketo. Ahora, si tienes la suerte de tener un usuario avanzado de Marketo en tu equipo, o si estás haciendo un desarrollo a medida para usuarios internos de Marketo, no es un problema. Pero en el caso de mi equipo, fue un gran negocio. Ninguno de nosotros era experto en Marketo y, dado que se nos pedía que entendiéramos un gran número de servicios en la nube, no teníamos la plantilla para convertirnos en expertos en nada. Estas son algunas de las ideas que obtuvimos del acceso a una instancia real: - Grandes esquemas de plomo. El esquema de posible cliente de la instancia de producción a la que hemos accedido tiene más de 200 campos. Esto dejó muy claro a nuestros diseñadores de IU que la IU que estaban diseñando tenía que acomodar esquemas de ese tamaño (o más grandes). - Uso de ráfagas. Vimos dos órdenes de diferencia de magnitud entre los tiempos de mayor uso y los tiempos de bajo uso (en términos de número de posibles clientes creados o actualizados). Esto afectaba tanto al volumen de datos que recuperábamos de las llamadas a la API (obvio) como al tiempo que tardaba una llamada a la API en responder (posiblemente menos obvio).

**Tiempo de respuesta de llamada de API** Según la hora del día, los detalles de su llamada de API y el contenido de su instancia, es posible que el tiempo de respuesta de la API de SOAP tarde más de la media. En ocasiones, recibíamos llamadas API que tardaban un minuto y medio en responder. Debe tener en cuenta la posibilidad de tratar con él: - Prueba. Tal vez esto no sea un problema para su uso. Pero no asumas eso, haz algunas pruebas. - Ajusta tu uso. En nuestro caso, el problema más grande era que establecíamos el tamaño de página para nuestras llamadas en [getMultipleLeads](/help/soap-api/getmultipleleads.md) para que fueran tan grandes como lo permitiera la API. En nuestro contexto, eso tiene cierto sentido porque nuestro objetivo es ser lo más eficientes posible con la cuota de API de nuestro cliente. Sin embargo, en su contexto, es posible que no tenga que preocuparse tanto por las cuotas de llamadas de API de los usuarios, en cuyo caso obtendrá un mejor tiempo de respuesta al solicitar páginas de datos más pequeñas.

**Partición de posibles clientes** Marketo proporciona potentes herramientas, particiones y espacios de trabajo que permiten que varios grupos de marketing compartan una sola instancia de Marketo. Sin embargo, esas herramientas no se reflejan directamente en la API de SOAP. Por ejemplo, cuando utiliza getMultipleLeads para obtener todos los posibles clientes que se han actualizado o creado desde hace algún tiempo, puede recuperar todos los posibles clientes en su instancia, en cuyo caso, independientemente de qué partición o espacio de trabajo contenga un posible cliente (y sin nada que indique). La creación de posibles clientes y la adición de posibles clientes a listas son otros contextos en los que la partición de posibles clientes puede afectar a lo que realmente hacen las llamadas de API. Tenga en cuenta que esto significa que las particiones y los espacios de trabajo pueden no ser la solución que necesita para resolver el problema del uso compartido de la zona protegida que se ha descrito anteriormente. Entonces, ¿cómo se da cuenta de si esto es un problema para usted? He encontrado que todo esto es útil: los Evangelistas Desarrolladores están comprometidos con nuestro éxito en el uso de las API, y donde hay preguntas, son increíblemente buenos en trabajar para encontrar respuestas. - [Documentación de API](./getting-started.md). Los evangelistas ya han incluido este tema en parte de la documentación, y como parte de su compromiso con nuestro éxito, son muy buenos en actualizar el documento. - Sus Propios Casos De Prueba. Aunque el uso de particiones y espacios de trabajo para compartir el espacio aislado puede no ser una buena idea, el espacio aislado es un buen lugar para jugar con particiones y espacios de trabajo a fin de averiguar si plantean desafíos para el uso previsto. (También es una buena manera de reducir sus preguntas para los evangelistas, que siempre son una buena idea).

**TIMTOWTDI y prueba** &quot;Hay más de una manera de hacerlo&quot;: el lema de programación Perl se aplica en ciertos contextos a la API de SOAP de Marketo. Por ejemplo, quería combinar la actualización de un conjunto de posibles clientes con la adición de esos posibles clientes a una lista. La API de SOAP le ofrece dos formas de hacerlo: 1. [importToList](/help/soap-api/importtolist.md) + [getImportToListStatus](/help/soap-api/getimporttoliststatus.md). Al leer la documentación, esta es obviamente la manera &quot;normal&quot; de hacerlo. Sin embargo, el hecho de que tenga que sondear para ver el estado de la operación de importación me ha levantado un indicador amarillo. ¿Era esta realmente la forma en que quería implementar mi importación? 1. [syncMultipleLeads](/help/soap-api/syncmultipleleads.md) + [listOperation](/help/soap-api/listoperation.md). Esto parece mucho menos elegante que una llamada importToList unitaria, pero no depende del sondeo. ¿Era una opción viable? Casos como estos son difíciles de tratar para los evangelistas, porque realmente dependen de la naturaleza de los casos con los que estás tratando y exactamente lo que estás tratando de hacer. Por suerte, si ha configurado un entorno de prueba de unidades robusto, también debería poder utilizarlo para explorar preguntas como estas. En este caso particular, resultó que la opción 2 era mejor para mi caso de uso que la opción 1: no debido a la encuesta, sino porque me encontré con limitaciones orientadas a campos en importToList, y también porque estaba intentando escribir código que se pudiera usar en contextos e instancias sobre los cuales no tenía control. Sin embargo, su caso de uso puede ser diferente y las pruebas son la única manera de averiguarlo.

**Conclusión** No creo que nada de esto sea un gran secreto. Por otro lado, habría estado por delante del juego si hubiera sabido todo esto antes de empezar. Espero que te resulte útil.

Publicado el _2015-02-05_ por _David_

## Uso de la API de REST de Marketo con Boomi: Obtención y eliminación de posibles clientes de listas estáticas

En la parte 1 de esta serie, expliqué cómo era posible empezar a utilizar la API de REST a través de Boomi con el conector Boomi HTTP, específicamente obteniendo el token de autenticación necesario para acceder a la API de REST y almacenándolo en una variable de proceso. A continuación, empezaremos a realizar llamadas a Marketo y, en esta entrega, le muestro cómo puede [obtener varios posibles clientes por id. de lista](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists) y [eliminar posibles clientes de la lista](/help/rest-api/lead-database.md). Preste especial atención a la eliminación de posibles clientes de una lista porque hay un aspecto muy &quot;ligeramente documentado&quot; y sutil de Boomi en el trabajo allí que me expando cuando llegamos allí.

En nuestra siguiente entrega ampliamos esta funcionalidad para empezar a hacer cosas interesantes como conseguir actividad de plomo, pero eso es un blog para otro día. Para esta entrega veremos la segunda y tercera áreas resaltadas. Como revisión, he incluido las respuestas JSON que necesitamos a continuación. Recuerde que para crear un perfil JSON en Boomi, todo lo que debe hacer es crear un componente de perfil de tipo JSON, hacer clic en &quot;importar&quot; y seleccionar el archivo. Boomi hace el resto, extrapolando cosas como si debería haber múltiples ID permitidos. Ejemplo de JSON para [Obtener varios posibles clientes por id. de lista](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists)

```json
{
  "requestId": "",
  "success": true,
  "nextPageToken": "",
  "result": [
    {
      "id": 0,
      "email": "",
      "firstName": "",
      "lastName": ""
    },
    {
      "id": 0,
      "email": "",
      "firstName": "",
      "lastName": ""
    },
    {
      "id": 0,
      "email": "",
      "firstName": "",
      "lastName": ""
    }
  ]
}
```

Ejemplo de JSON para [Quitar posibles clientes de la solicitud de lista](https://developer.adobe.com/marketo-apis/api/mapi/#operation/removeLeadsFromListUsingDELETE)

```json
{
   "input":[
      {
         "id": ""
      },
      {
         "id": ""
      }
   ]
}
```

Ejemplo de JSON para [Quitar posibles clientes de la respuesta de lista](https://developer.adobe.com/marketo-apis/api/mapi/#operation/removeLeadsFromListUsingDELETE)

```json
{
  "requestId": "",
  "success": true,
  "result": [
    {
      "id": 1,
      "status": ""
    },
    {
      "id": 2,
      "status": "",
      "reasons": [
        {
          "code": "",
          "message": ""
        }
      ]
    }
  ]
}
```

Obtener varios posibles clientes por ID de lista** suelte otro conector (Get) en el proceso mediante la misma conexión definida en el artículo anterior. Cree una nueva operación llamada &quot;Obtener varios posibles clientes por ID de lista&quot; (soy un adhesivo para la coherencia). Sus atributos son los siguientes: Perfil de solicitud: Ninguno (este usa la dirección URL de solicitud): Tipo de perfil de respuesta: jSON: Perfil de respuesta: cree un nuevo perfil basado en la respuesta Obtener varios posibles clientes por ID de lista anterior. Tenga en cuenta que puede cambiarlo para que devuelva los campos que desee, no solo los enumerados. Es importante recordar que el perfil de respuesta JSON debe coincidir con la lista de campos que solicita de la API de REST y debe solicitar solo los campos que necesita. En el objeto Propiedades del proceso, definimos una propiedad denominada &quot;campos&quot; que es una lista separada por comas de los campos que desea que REST devuelva. y esa es la lista que tiene que coincidir con el perfil. Tipo de contenido: text/plain (esto es solo una solicitud de URL) Método HTTP: GET (lo busca en los documentos de la API de REST, siempre está en la lista) Ruta del recurso (añadir 5) rest/v1/list/ listID (variable de reemplazo) /leads.json?access_token= access_token (variable de reemplazo) &amp;fields= fields (variable de reemplazo). A continuación, en la pestaña Parameters del conector, puede introducir los valores de las variables, todos los cuales se rellenaron anteriormente en las propiedades de proceso. En la siguiente sección hablaré sobre cómo puede evitar rellenar estos campos manualmente. Voy a omitir la parte del proceso en la que asigno la respuesta de Obtener varios posibles clientes por ID de lista a un perfil de archivo plano y la inserto en un servidor FTP, porque esa es la funcionalidad directa de Boomi.

Eliminar posibles clientes de una lista Este es interesante, uno de mis compañeros de trabajo, [Ken Niwa](https://www.linkedin.com/uas/login?session_redirect=https%3A%2F%2Fwww.linkedin.com%2Fprofile%2Fview%3Fid%3D7429494) me enseñó esta siguiente técnica y es bastante interesante y se basa en un artículo de Boomi titulado &quot;Cómo crear una solicitud de POST para una aplicación RESTful&quot;, que se muestra a continuación.  ...pero primero lo primero. En el proceso, saliendo de &quot;Obtener varios posibles clientes por ID de lista&quot; tenemos la forma Obtener varios posibles clientes por respuesta de ID de lista, y tenemos que asignar eso en la &quot;Eliminar posibles clientes de la solicitud de lista&quot; que la asignación es bastante sencilla, solo hay que asignar el ID que obtuvimos de los posibles clientes en la lista original a la lista de ID que estamos pasando en el jSON de eliminación. A continuación, suelte otro conector con la acción &quot;Enviar&quot;, utilizando la misma conexión. Cree una nueva operación llamada &quot;Eliminar posibles clientes de la solicitud de lista&quot;. cuyos atributos son Solicitar perfil: jSON Tipo de contenido: application/json Perfil de solicitud: [Perfil JSON] Eliminar posibles clientes de la solicitud de lista (creada a partir del archivo anterior) Tipo de perfil de respuesta: jSON Perfil de respuesta: [Perfil JSON] Eliminar posibles clientes de la respuesta de lista (creada a partir del archivo anterior) Tipo de contenido: application/json Método HTTP: Ruta de recurso de DELETE (añadir 4) rest/v1/lists/ listID (variable de sustitución) /leads.json?access_token= access_token (variable de sustitución) Esto es lo interesante de este conector. NO vamos a añadir explícitamente los parámetros en la pestaña del conector. En su lugar, como se indica en el artículo, se crean propiedades de documento dinámicas que tienen los mismos nombres que las variables de reemplazo. En este caso, esas variables listID y access_token. Al hacerlo, la forma jSON fluye a la llamada REST y los parámetros aparecen en su lugar correcto en la dirección URL. No podemos hacer esto con la llamada anterior porque es un GET, no un POST. Por lo tanto, en este punto ha visto una llamada a la API de GET y POST REST y puede empezar a ver el patrón para realizar estas llamadas REST. En la próxima entrega empezaremos a ver la exportación de la actividad del posible cliente a través de la API de REST, que está un poco más involucrada.

Publicado el _2015-02-06_ por _John_

## Incrustar un vídeo de YouTube con el seguimiento de posibles clientes en una página de aterrizaje de Marketo

En una publicación de blog anterior, describí cómo segmentar posibles clientes en Marketo en función de si han iniciado o finalizado un vídeo específico de YouTube. En esta publicación de blog, explicamos cómo tomar la implementación de esa publicación y utilizarla en una página de aterrizaje de Marketo.

1. Vaya al programa de Marketo donde desea crear la nueva página de aterrizaje. Haga clic en Nuevo recurso local y, a continuación, en Página de aterrizaje.
1. Asigne un nombre a la página de aterrizaje. Asigne una URL de página. Seleccione una plantilla. A continuación, haga clic en Crear.
1. Una vez creada la página de aterrizaje, haga clic en Editar borrador.
1. Desde el panel derecho, arrastre el botón HTML al lienzo principal de la izquierda.
1. En el cuadro Editor de HTML personalizado que aparece. A continuación, haga clic en Guardar.
1. Ajuste el tamaño de un elemento de HTML arrastrando el contorno del cuadro. A continuación, haga clic en Aprobar y cerrar.
1. Pruebe la versión activa de la página de aterrizaje haciendo clic en Ver página aprobada. Se abre una página de aterrizaje con YouTube en una nueva ventana. El vídeo iniciado y finalizado se mostrará en el registro de actividad del posible cliente, como se muestra en la primera y la segunda captura de pantalla a continuación. Una vez que los datos se encuentran en Marketo, puede crear una lista inteligente y posibles clientes de segmentos que hayan iniciado o finalizado un vídeo, como se muestra en la captura de pantalla siguiente. Para obtener más información sobre la API de YouTube Iframe, [visite la documentación de la API de YouTube](https://developers.google.com/youtube/iframe_api_reference). Para obtener más información sobre Munchkin, [consulte la documentación para desarrolladores de Marketo](/help/javascript-api/lead-tracking.md).

Publicado el _2015-02-09_ por _Murta_

## Seguimiento web de aplicaciones de una sola página con Munchkin

Una aplicación de una sola página es un sitio web que carga todos los recursos necesarios para navegar por el sitio en la primera carga de página. Cuando un usuario hace clic en un vínculo, el contenido se carga desde los primeros datos de carga de página. Para el usuario, el sitio web se comporta como se espera porque la dirección URL de la barra de direcciones es como la navegación tradicional por la página. Munchkin funciona bien con sitios web tradicionales porque Munchkin se ejecuta cada vez que los usuarios cargan una nueva página. Sin embargo, con una aplicación de una sola página si no se carga una página nueva, Munchkin se ejecutará solo una vez. El método que explico en esta publicación es rastrear cuándo un usuario hace clic en un vínculo y luego enviar esta información a Munchkin. Implementamos esto mediante la función de Munchkin `clickLink`. A continuación se muestra una implementación de ejemplo en jQuery que enlaza los eventos de clic al método Munchkin `clickLink`. Al llamar al método Munchkin `clickLink`, se pasa el parámetro de la dirección URL en la que se hizo clic.

```javascript
<script>
$("a").on('click', function(event) {
    var urlThatWasClicked = $(this).attr('href');
    Munchkin.munchkinFunction('clickLink', { href: urlThatWasClicked});
});
</script>
```

Publicado el _2015-02-11_ por _Murta_

## Cambiar la puntuación de un posible cliente mediante la API de REST

Supongamos que desea cambiar la puntuación de un posible cliente en Marketo mediante las API. Esto es posible hacer con la API de REST mediante el punto de conexión de Crear/Actualizar posible cliente. A continuación se muestra un ejemplo de código en Ruby que muestra cómo realizar esta llamada.

```ruby
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "https://AAA-BBB-CCC.mktorest.com"
endpoint = "/rest/v1/leads.json"
# Replace with your access token
auth_token =  "?access_token=" + "ac756f7a-d54d-41ac-8c3c-f2d2a39ee325:ab"
request_url = marketo_instance + endpoint + auth_token

# Build request body
data = { "action" => "updateOnly", "input" => [ { "email" => "<example@email.com>", "leadScore" => "30" } ] }

# Make request
response = RestClient.post request_url, data.to_json, :content_type => :json, :accept => :json

# Returns Marketo API response
puts response
```

En el cuerpo JSON de la solicitud, especificamos `updateOnly` como la acción. Esto significa que la solicitud solo funcionará si existe el posible cliente; de lo contrario, fallará. Si desea crear un posible cliente si no existe, especifique `createOrUpdate` como acción. Utilizamos el correo electrónico del posible cliente como identificador principal para encontrar el registro de posible cliente en Marketo. Finalmente, especificamos el valor de la puntuación del posible cliente usando la clave `leadScore`. Es posible actualizar 300 posibles clientes a la vez mediante este método.

Publicado el _2015-02-19_ por _Murta_

## Resaltar proyectos abiertos de Source creados en la plataforma de Marketo: tercera parte

Esta es la tercera publicación de una serie en curso que resalta los proyectos de código abierto creados en torno a la plataforma Marketo por la comunidad de desarrolladores. Mantenemos [una lista en la cuenta de GitHub de Marketo](https://github.com/Marketo/Community-Supported-Client-Libraries) donde realizamos un seguimiento de las bibliotecas de cliente y los proyectos creados por la comunidad de desarrolladores de Marketo. A continuación se muestran tres proyectos desarrollados en torno a las API de REST de Marketo. **[Usermind](http://www.usermind.com/) creó [una biblioteca de cliente Node.js para la API REST de Marketo](https://github.com/MadKudu/node-marketo).** **[Arunim Samat](https://github.com/asamat) creó [una biblioteca de cliente en Python para la API de REST de Marketo](https://github.com/asamat/python_marketo).** **[Jacques Lemieux de Marketo](https://www.linkedin.com/in/jalemieux) creó [una biblioteca de cliente en Ruby para la API de REST de Marketo.](https://github.com/jalemieux/mkto_rest)** Nos complace ver más proyectos creados por la comunidad de desarrolladores en la plataforma Marketo. Si está trabajando en un proyecto de código abierto para Marketo Platform, [envíelo a este repositorio de GitHub mediante una solicitud de extracción](https://github.com/Marketo/Community-Supported-Client-Libraries).

Publicado el _2015-02-20_ por _Murta_

## Inserción de un formulario Marketo en una campaña de RTP

Muchos especialistas en marketing están interesados en colocar un formulario Marketo en una campaña de Marketo Real-Time Personalization (RTP). Tanto si es un tipo de campaña Dialog, In Zone o Widget RTP, puede copiar el código de Form HTML y pegarlo en el editor de campañas de RTP. He visto estos ejemplos de uso: - Conseguir que los visitantes se suscriban a su newsletter después de hacer un segundo o tercer clic en su sitio - Formulario de registro rápido y efectivo para seminarios web - Descargar un caso práctico cerrado - Ofrecer posibles clientes que cancelaron su suscripción en el pasado para volver a suscribirse Rellene un formulario en la campaña y reciba el agradecimiento o el contenido solicitado, lo que da como resultado un clic menos para llegar a sus objetivos. Así que aquí va la explicación de cómo hacerlo e incrustar un formulario Marketo 2.0 en una campaña de Marketo RTP. Vea un gran ejemplo de eMarketer, usuarios de RTP que lo llevaron un paso más allá y en lugar de dirigir a los visitantes a una página de agradecimiento - decidieron mostrar un mensaje de agradecimiento dentro de la campaña de RTP. El código de esta opción también se encuentra a continuación. ¡Disfruta y estoy feliz de escuchar sobre tu experiencia con él!

1. Haga clic con el botón derecho en un formulario aprobado. Seleccionar **código incrustado.**
1. Copie el **código.**
1. En Marketo RTP, vaya a **Campañas**.
1. Haga clic en **CREAR NUEVA CAMPAÑA**.
1. En el Editor de texto enriquecido, haga clic en el **icono de HTML**.
1. Pegue el código incrustado del formulario en el Editor de Source de HTML. Haga clic en **Actualizar**.
1. El formulario no se mostrará en la vista del editor, pero puede previsualizarlo para ver cómo se procesará en una campaña.
1. Haga clic en **Iniciar** para iniciar la campaña.

### Nota

Los cambios realizados en el formulario deben realizarse dentro de Actividades de marketing de Marketo, en Editar borrador del formulario.

### Artículos relacionados

* [Formularios 2.0](/help/javascript-api/forms-api-reference.md)

Publicado el _2015-12-20_ por _Yanir_

## Agregar un botón Restablecer a un formulario Marketo Forms

```javascript
<script src="//app-sj01.marketo.com/js/forms2/js/forms2.min.js"></script>
<form id="mktoForm_116"></form>
<script>MktoForms2.loadForm("//app-sj01.marketo.com", "410-XOR-673", 116,
function(form) { form.getFormElem()[0].querySelector('button[type="submit"]').insertAdjacentHTML('afterend','<button type="reset" class="mktoButton">Reset</button>') });
</script>
```

Publicado el _2015-03-18_ por _Murta_

## Actualizaciones de la versión de marzo de 2015

La [API de recursos REST de Marketo se publicó en la versión de marzo de 2015](https://developer.adobe.com/marketo-apis/api/asset/). Esta API permite acceder a los objetos de archivo, carpeta, token, correo electrónico y plantilla de correo electrónico de Marketo. Tenga en cuenta que se agregaron dos permisos de función para proporcionar acceso a los extremos de la API de recursos: Assets de solo lectura y Assets de lectura y escritura. Si la función de usuario de la API es anterior al lanzamiento de las API de Asset, debe crear una nueva función de usuario de la API con estos permisos para habilitar el acceso. De lo contrario, recibirá una respuesta de error 603 &quot;Acceso denegado&quot;. Además de la versión de la API de REST Asset, hubo actualizaciones en los extremos de la API de REST existentes. El extremo [Merge Lead REST API](https://developer.adobe.com/marketo-apis/api/mapi/#operation/mergeLeadsUsingPOST) se actualizó para permitir la combinación de varios posibles clientes. El extremo [Schedule Campaign REST API](https://developer.adobe.com/marketo-apis/api/mapi/#operation/scheduleCampaignUsingPOST) se actualizó para permitir la clonación de una campaña al programarla.

Publicado el _2015-03-23_ por _Murta_

## Activación de campañas RTP con un retraso

Este JavaScript personalizado permite a los usuarios de RTP mostrar campañas unos segundos después de que se cargue la página web. Esto se recomienda para campañas de cuadros de diálogo y widgets. Se puede utilizar para mostrar una campaña después de un retraso, una vez que el visitante vio el contenido normal en la página. Se recomienda implementar este código solo en páginas específicas donde se deben mostrar las campañas. No se recomienda utilizar este código en todas las páginas, ya que puede afectar al rendimiento. **Instrucciones de instalación** El código personalizado envía un evento de datos personalizados RTP (t=timeOnPage, es decir: t=60) y luego carga la campaña que coincide con este evento. De forma predeterminada, se activa después de 60 segundos. Puede personalizarlo cambiando el parámetro sendCustomRTPEvent a cualquier otro número. Coloque el código inmediatamente después del código RTP estándar:

```javascript
<script>
function sendCustomRTPEvent(a){
 var eventValue="t="+a;
 setTimeout(function(){
  rtp('send', 'event', {value: eventValue});
  rtp('get', 'campaign',true);
 }, 1000 \* a);
}
sendCustomRTPEvent(60); //Seconds
</script>
```

Para configurar una campaña RTP para que reaccione después de un retraso de tiempo: 1. Inicie sesión en su cuenta de RTP 1. Cree un nuevo segmento 1. En la sección Eventos de segmento, agregue: `t=60`. Un visitante solo puede hacer coincidir cada segmento de RTP una vez por sesión, por lo que ve cada campaña solo una vez (a menos que se establezca en &quot;adhesivo&quot;).

Publicado el _2015-03-24_ por _Yanir_

## Actualizaciones de la versión de abril de 2015

### Marketo Mobile Engagement SDK v0.3.2

Marketo ahora incluye automatización de marketing y participación del usuario para aplicaciones móviles. La instalación de [Marketo Mobile SDK](/help/mobile/mobile.md) en su aplicación de iOS o Android permite a los especialistas en mercadotecnia detectar eventos en la aplicación y enviar notificaciones push relevantes.

### Mejoras de API de REST

* Objetos personalizados

Se han introducido nuevos [extremos de objeto personalizados](/help/rest-api/custom-objects.md) que le permiten enumerar, describir y ELIMINAR mediante programación los datos que residen con un objeto personalizado de Marketo.

Tenga en cuenta que se agregaron permisos de funciones para proporcionar acceso a los extremos de la API de objetos personalizados: objeto personalizado de solo lectura, objeto personalizado de lectura y escritura. Si la función de usuario de la API es anterior al lanzamiento de las API de objetos personalizados, debe crear una nueva función de usuario de la API con estos permisos para habilitar el acceso. De lo contrario, recibirá una respuesta de error 603 &quot;Acceso denegado&quot;.

* Programar campaña: clonar programa

Se introdujo un nuevo parámetro opcional &quot;cloneToProgramName&quot; en la [API de programación de campaña](/help/rest-api/data-ingestion.md). Cuando este parámetro está presente, el programa principal de la campaña se clona y la campaña recién creada se programa. El parámetro especifica el nombre deseado para el programa resultante.

Publicado el _2015-04-28_ por _Travis Kaufman_

## Sincronización De Cancelaciones De Suscripción De Correo Electrónico Entre Instancias

¿Administra varias instancias de Marketo? Mantener la información de posibles clientes sincronizada entre instancias puede ser difícil. Esta es una forma de sincronizar las cancelaciones de suscripción de correo electrónico entre instancias mediante un webhook que llama a un servicio web externo. El servicio web externo realiza un bucle en cada instancia en busca del posible cliente conocido que activó el evento de cancelación de suscripción. Cuando se encuentra un posible cliente coincidente, se actualiza el campo &quot;sin suscribir&quot; en el registro de posibles clientes correspondiente. Aquí hay un diagrama que ilustra la idea.  Depende de usted implementar el servicio web, pero el siguiente código debe ayudarle a iniciar el proceso.

### Servicio web externo

El servicio web externo realiza los siguientes pasos para cada instancia de Marketo que debe sincronizarse:

1. Compone la API REST específica de instancia [URL de extremo](/help/rest-api/endpoint-reference.md)
1. Obtiene el token de acceso mediante [Identidad](/help/rest-api/authentication.md)
1. Obtiene la lista de registros de posibles clientes que coinciden con la dirección de correo electrónico mediante [Obtener varios posibles clientes por tipo de filtro](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET)
1. Actualiza el campo &quot;Cancelación de la suscripción&quot; de cada registro de posibles clientes mediante Crear/Actualizar posibles clientes

Este es otro diagrama que muestra en detalle la llamada al servicio web externo y las llamadas a la API REST de Marketo.  El siguiente código de ejemplo no es un servicio web predeterminado. En su lugar, es un programa de modo de consola al que puede pasar argumentos a través de la línea de comandos. La intención aquí es mostrar cómo llamar a las API de Marketo adecuadas para actualizar los registros de posibles clientes en todas las instancias. La implementación del servicio web se deja como un ejercicio para el lector.
**Código de ejemplo** Para poner en marcha el código de ejemplo, debe crear un proyecto Java en su IDE favorito. Después de esto, deberá realizar los siguientes cambios: 1. El código de ejemplo usa [json-simple](https://code.google.com/archive/p/json-simple) para analizar cadenas JSON. Agregue el jar json-simple a su proyecto Java. 1. El código de ejemplo tiene una estructura que contiene metadatos para cada instancia de Marketo. Coloque los valores reales de las instancias en la estructura de la siguiente manera:

```java
public static String instanceInfo[][] = {
{ "AccountId1", "ClientId1","ClientSecret1" },    // Instance 1 metadata
{ "AccountId2", "ClientId2","ClientSecret2" },    // Instance 2 metadata
{ "AccountId3", "ClientId3","ClientSecret3" }     // Instance 3 metadata
};
```

Puede encontrar los metadatos de la instancia en el Panel de administración de Marketo:

* ID de cuenta Administración > Integración > Munchkin > Cuenta de Munchkin
* ID de cliente y secreto de cliente > Administración > Integración > LaunchPoint > Cancelar suscripción por correo electrónico > Sincronizar > Ver detalles

El código de ejemplo toma dos argumentos de línea de comandos que simulan los parámetros de consulta &quot;id&quot; y &quot;email&quot; para el servicio web externo descrito anteriormente.

1. args[0] = Id. de cuenta
1. args[1] = Dirección de correo electrónico

Pase los valores reales de la instancia como argumentos al programa. Esta es una captura de pantalla de configuración del proyecto de Intellij IDEA.

**SyncEmailUnsubscribe.java**

```java
package com.marketo;

import org.json.simple.JSONArray;
import org.json.simple.JSONObject;
import org.json.simple.parser.JSONParser;
import org.json.simple.parser.ParseException;

import javax.net.ssl.HttpsURLConnection;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.Iterator;
import java.util.NoSuchElementException;
import java.util.Scanner;

public class SyncEmailUnsubscribe {
    // Define Marketo instance meta data here.
    // Each row contains three elements: Account Id, Client Id, Client Secret.
    // For example:
    //  public static String instanceData[][] = {
    //    {"111-AAA-222", "2f4a4435-f6fa-4bd9-3248-098754982345", "asdf6IVE9h4Jjcl59cOMAKFSk78ut12W"},
    //    {"222-BBB-333", "5f4a6657-f6fa-4cd9-4356-123083238821", "gfjgfIVE9h4Jjcl59cOMAKFSk78ut12W"},
    //    {"444-CCC-444", "9f4a4678-f6fa-4dd9-7735-908713247721", "xzcxvbVE9h4Jjcl59cOMAKFSk78ut12W"}
    //  };
    //
    public static String instanceData[][] = {
            // ADD YOUR INSTANCE META DATA HERE
    };

    public static void main(String[] args) {
        String accountId = args[0];     // Account id that processed the unsubscribe
        String emailAddress = args[1];  // Email address of lead that unsubscribed

        SyncEmailUnsubscribe seu = new SyncEmailUnsubscribe();

        // Loop through each Marketo instance
        for (int i = 0; i < instanceData.length; i++) {

            // Make sure we skip instance that triggered the webhook
            if (!accountId.equals(instanceData[i][0])) {
                String endpointUrl = String.format("https://%s.mktorest.com", instanceData[i][0]);

                // Generate access token
                String identityUrl = String.format("%s/identity/oauth/token?grant_type=client_credentials&client_id=%s&client_secret=%s", endpointUrl, instanceData[i][1], instanceData[i][2]);
                String token = seu.getToken(identityUrl);

                // Get lead records for given email address (may be duplicates)
                String getLeadsUrl = String.format("%s/rest/v1/leads.json?access_token=%s&filterType=email&filterValues=%s", endpointUrl, token, emailAddress);
                String leads = seu.getLeads(getLeadsUrl);

                // Update unsubscribed field in lead record
                String updateLeadsUrl = String.format("%s/rest/v1/leads.json?access_token=%s", endpointUrl, token);
                seu.updateLeads(updateLeadsUrl, leads, accountId);
            }
        }

        System.exit(0);
    }

    // Call Identity Service to generate access token
    public String getToken(String url) {
        // Call Identity Service
        String tokenData = getData(url);

        // Convert response into JSONObject
        JSONParser parser = new JSONParser();
        Object obj = null;
        try {
            obj = parser.parse(tokenData);
        } catch (ParseException pe) {
            System.out.println("position: " + pe.getPosition());
            System.out.println(pe);
        }

        // Retrieve access_token
        JSONObject jsonObject = (JSONObject)obj;
        return jsonObject.get("access_token").toString();
    }

    // Call Get Multiple Leads by Filter Type Service to get lead records
    public String getLeads(String url) {
        return getData(url);
    }

    // Call Create/Update Lead Service to update "unsubscribed" flag in lead record
    public void updateLeads(String url, String leads, String account) {
        JSONObject body = composeBody(leads, account);
        if (body != null) {
            postData(url, body);
        }
    }

    // Compose JSON body for Create/Update Leads Service
    private JSONObject composeBody(String leads, String account) {
        JSONObject body = new JSONObject();

        // Convert leads into JSONObject
        JSONParser parser = new JSONParser();
        Object obj = null;
        try {
            obj = parser.parse(leads);
        } catch (ParseException pe) {
            System.out.println("position: " + pe.getPosition());
            System.out.println(pe);
        }
        JSONObject leadsObj = (JSONObject)obj;

        Object success = leadsObj.get("success");
        if (success.equals(true)) {
            body.put("action", "updateOnly");
            body.put("lookupField", "id");
            body.put("asyncProcessing", "true");

            // Build array of lead objects
            JSONArray input = new JSONArray();
            JSONArray result = (JSONArray) leadsObj.get("result");
            Iterator<JSONObject> iterator = result.iterator();
            while (iterator.hasNext()) {
                JSONObject leadIn = (JSONObject)iterator.next();
                JSONObject lead = new JSONObject();
                lead.put("id", leadIn.get("id"));
                lead.put("unsubscribed", "true");
                lead.put("unsubscribedReason", "Cross instance synch triggered by webhook from: " + account);
                input.add(lead);
            }

            body.put("input", input);
        }
        return body;
    }

    // HTTP POST request
    private String postData(String endpoint, JSONObject body) {
        String data = "";
        try {
            // Make request
            URL url = new URL(endpoint);
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
            urlConn.setRequestMethod("POST");
            urlConn.setAllowUserInteraction(false);
            urlConn.setDoOutput(true);
            urlConn.setRequestProperty("Content-type", "application/json");
            urlConn.setRequestProperty("accept", "application/json");
            urlConn.connect();
            OutputStream os = urlConn.getOutputStream();
            os.write(body.toJSONString().getBytes());
            os.close();
            int responseCode = urlConn.getResponseCode();
            if (responseCode == 200) {
                System.out.println("Status: 200");
                InputStream inStream = urlConn.getInputStream();
                data = convertStreamToString(inStream);
                System.out.println(data);
            } else {
                System.out.println(responseCode);
                data = "Status:" + responseCode;
            }
        } catch (MalformedURLException e) {
            System.out.println("URL not valid.");
        } catch (IOException e) {
            System.out.println("IOException: " + e.getMessage());
            e.printStackTrace();
        }
        return data;
    }

    // HTTP GET request
    private String getData(String endpoint) {
        String data = "";
        try {
            URL url = new URL(endpoint);
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
            urlConn.setRequestMethod("GET");
            urlConn.setAllowUserInteraction(false);
            urlConn.setDoOutput(true);
            int responseCode = urlConn.getResponseCode();
            if (responseCode == 200) {
                System.out.println("Status: 200");
                InputStream inStream = urlConn.getInputStream();
                data = convertStreamToString(inStream);
                System.out.println(data);
            } else {
                System.out.println(responseCode);
                data = "Status:" + responseCode;
            }
        } catch (MalformedURLException e) {
            System.out.println("URL not valid.");
        } catch (IOException e) {
            System.out.println("IOException: " + e.getMessage());
            e.printStackTrace();
        }
        return data;
    }

    private String convertStreamToString(InputStream inputStream) {
        try {
            return new Scanner(inputStream).useDelimiter("A").next();
        } catch (NoSuchElementException e) {
            return "";
        }
    }
}
```

### Configuración de Marketo

Realice los siguientes pasos para cada instancia de Marketo que desee sincronizar.

1. Crear un servicio personalizado con permiso de función: Leer y escribir posible cliente. Si no conoce cómo crear un servicio personalizado, haga clic [aquí](/help/rest-api/custom-services.md).
1. Cree un webhook que llame al servicio web externo. Si no está familiarizado con la creación de Webhooks, haga clic [aquí](/help/webhooks/webhooks.md).
1. Agregue el webhook como paso de flujo en Smart Campaign.

La captura de pantalla siguiente muestra cómo crear un webhook para invocar el servicio especificado anteriormente mediante tokens para rellenar automáticamente los parámetros de consulta. Ahora que hemos creado nuestro webhook, podemos agregarlo a una campaña inteligente como acción de flujo.  La lista inteligente debe contener el déclencheur &quot;Cancela la suscripción al correo electrónico&quot;.

### Validación

Para probar todo esto, cree un posible cliente con la misma dirección de correo electrónico en varias instancias de Marketo. Asegúrese de que es el propietario de la dirección de correo electrónico. En un déclencheur de instancia, una acción de flujo de envío de correo electrónico, abra el correo electrónico resultante y haga clic en Cancelar suscripción. Para validar los resultados, inicie sesión en cada una de las otras instancias e inspeccione los registros de posibles clientes asociados con la dirección de correo electrónico. La casilla &quot;Cancelación de la suscripción&quot; debe estar marcada y el campo &quot;Razón de la cancelación de la suscripción&quot; debe contener una nota con el ID de la cuenta de origen que inició la sincronización.

Publicado el _2015-05-11_ por _David_

## Sincronización de cambios de datos de posibles clientes mediante la API REST

Esa publicación presentaba un ejemplo de código que se podía ejecutar de forma recurrente para sondear Marketo en busca de actualizaciones. La idea era utilizar las API de Marketo para identificar los cambios en los datos de posibles clientes y extraer los datos que habían cambiado. Estos datos se podrían insertar en un sistema externo para su sincronización. El ejemplo de código presentado utilizaba nuestra API de SOAP. Bueno, tenemos una [nueva forma de caminar](https://www.youtube.com/watch?v=G-7ZJjLy5D8&feature=youtu.be), y de esa manera se está usando la [API REST de Marketo](/help/rest-api/rest-api.md). Esta publicación muestra cómo lograr el mismo objetivo utilizando dos extremos REST: [Obtener cambios de posible cliente](/help/rest-api/rest-api.md), [Obtener posible cliente por id.](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET). El programa contiene 2 pasos principales:

1. Llame a Obtener cambios de posibles clientes para generar una lista de todos los ID de posibles clientes que han cambiado campos de posibles clientes específicos o que se han agregado durante un período de tiempo determinado.
1. Llame a Obtener ID de posible cliente para cada ID de posible cliente de la lista para recuperar los datos de campo del registro de posibles clientes.

Tomaremos los datos recuperados en el paso 2 y los formatearemos para que los consuma un sistema externo.  **Entrada de programa** De manera predeterminada, el programa &quot;regresa&quot; un día desde la fecha actual para buscar cambios. Por lo tanto, podría ejecutar este programa a la misma hora cada día, por ejemplo. Para retroceder más en el tiempo, puede especificar el número de días como argumento de línea de comandos, lo que aumenta de manera efectiva la ventana de tiempo. El programa contiene varias variables que puede modificar: CUSTOM_SERVICE_DATA: Contiene los datos del [servicio personalizado](/help/rest-api/custom-services.md) de Marketo (id. de cuenta, id. de cliente y secreto de cliente). LEAD_CHANGE_FIELD_FILTER: contiene una lista separada por comas de los campos de posibles clientes a los que inspeccionaremos para ver si hay cambios. READ_BATCH_SIZE: es el número de registros que se recuperarán a la vez. Utilice esto para ajustar la respuesta al tamaño del cuerpo. **Salida de programa** El programa recopila todos los registros de posibles clientes modificados y los formatea en JSON como una matriz de objetos de posibles clientes de la siguiente manera:

```json
{
    "result": [
        {
            "leadId": "318592",
            "updatedAt": "2015-07-22T19:19:07Z",
            "firstName": "David",
            "lastName": "Everly",
            "email": "<deverly@marketo.com>"
        },

        ...more lead objects here...
    ]
}
```

La idea es que pueda pasar este JSON como carga útil de solicitud a un servicio web externo para sincronizar los datos. **Lógica de programa** Primero establecemos nuestra ventana de tiempo, componemos nuestras URL de punto final REST y obtenemos nuestro token de acceso de autenticación. A continuación activamos un bucle Get Paging Token/Get Lead Changes en el que se ejecuta hasta que agotamos el suministro de cambios de posibles clientes. El propósito de este bucle es acumular una lista de ID de posibles clientes únicos para que podamos pasarlos a Obtener posible cliente por ID más adelante en el programa. Para este ejemplo, indicamos a Get Lead Changes que busque los cambios en los campos siguientes: firstName, lastName, email. Puede seleccionar cualquier combinación de campos según sus necesidades. Obtener cambios de posibles clientes devuelve objetos &quot;result&quot; que contienen un ID de tipo de actividad, que podemos utilizar para filtrar los resultados. Nota: puede obtener una lista de tipos de actividades llamando al extremo REST [Obtener tipos de actividades](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getAllActivityTypesUsingGET). Nos interesan 2 tipos de actividades que se devuelven: 1. Nuevo posible cliente (12)

```json
{
    "id": 12024682,
    "leadId": 318581,
    "activityDate": "2015-03-17T00:18:41Z",
    "activityTypeId": 12,
    "primaryAttributeValueId": 318581,
    "primaryAttributeValue": "David Everly",
    "attributes": [
        {
            "name": "Created Date",
            "value": "2015-03-16"
        },
        {
            "name": "Source Type",
            "value": "New lead"
        }
    ]
}
```

1. Cambiar valor de datos (13) Podemos saber qué campo cambió inspeccionando la propiedad &quot;name&quot; en la respuesta Cambiar valor de datos.

```json
{
    "id": 12024689,
    "leadId": 318581,
    "activityDate": "2015-03-17T22:58:18Z",
    "activityTypeId": 13,
    "fields": [
        {
            "id": 31,
            "name": "lastName",
            "newValue": "Evely",
            "oldValue": "Everly"
        }
    ],
    "attributes": [
        {
            "name": "Source",
            "value": "Web form fillout"
        }
    ]
}
```

Cuando se devuelve cualquiera de estos dos tipos de actividades, se almacena el ID de posible cliente asociado en una lista. Una vez que tengamos nuestra lista, podemos iterar a través de ella llamando a Obtener posible cliente por ID para cada elemento. Esto recuperará los datos de posibles clientes más recientes para cada posible cliente de la lista. Para este ejemplo recuperamos los siguientes campos de posibles clientes: `leadId`, `updatedAt`, `firstName`, `lastName` y `email`. Puede seleccionar cualquier combinación de campos según sus necesidades. Para ello, especifique el parámetro fields para Obtener posible cliente por ID. Y finalmente JSONificamos los resultados como una matriz de objetos de plomo como se describe anteriormente.

**Código de programa**

```java
package com.marketo;

// minimal-json library (<https://github.com/ralfstx/minimal-json>)
import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;
import com.eclipsesource.json.JsonValue;

import java.io.\*;
import java.net.MalformedURLException;
import java.net.URL;
import java.text.DateFormat;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.\*;

import javax.net.ssl.HttpsURLConnection;

public class LeadChanges {
    //
    // Define Marketo REST API access credentials: Account Id, Client Id, Client Secret.  For example:
    //    public static String CUSTOM_SERVICE_DATA[] =
    //      {"111-AAA-222", "2f4a4435-f6fa-4bd9-3248-098754982345", "asdf6IVE9h4Jjcl59cOMAKFSk78ut12W"};
    //
    private static final String CUSTOM_SERVICE_DATA[] =
            {INSERT YOUR CUSTOM SERVICE DATA HERE};

    // Lead fields that we are interested in
    private static final String LEAD_CHANGE_FIELD_FILTER = "firstName,lastName,email";

    // Number of lead records to read at a time
    private static final String READ_BATCH_SIZE = "200";

    // Activity type ids that we are interested in
    private static final int ACTIVITY_TYPE_ID_NEW_LEAD = 12;
    private static final int ACTIVITY_TYPE_ID_CHANGE_DATA_VALE = 13;

    public static void main(String[] args) {
        // Command line argument to set how far back to look for lead changes (number of days)
        int lookBackNumDays = 1;
        if (args.length == 1) {
            lookBackNumDays = Integer.parseInt(args[0]);
        }

        // Establish "since date" using current timestamp minus some number of days (default is 1 day)
        Calendar cal = Calendar.getInstance();
        cal.add(Calendar.DAY_OF_MONTH, -lookBackNumDays);
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
        String sinceDateTime = sdf.format(cal.getTime());

        // Compose base URL
        String baseUrl = String.format("https://%s.mktorest.com",
                CUSTOM_SERVICE_DATA[0]);

        // Compose Identity URL
        String identityUrl = String.format("%s/identity/oauth/token?grant_type=%s&client_id=%s&client_secret=%s",
                baseUrl, "client_credentials", CUSTOM_SERVICE_DATA[1], CUSTOM_SERVICE_DATA[2]);

        // Call Identity API
        JsonObject identityObj = JsonObject.readFrom(getData(identityUrl));
        String accessToken = identityObj.get("access_token").asString();

        // Compose URLs for Get Lead Changes, and Get Paging Token
        String leadChangesUrl = String.format("%s/rest/v1/activities/leadchanges.json?access_token=%s&fields=%s&batchSize=%s",
                baseUrl, accessToken, LEAD_CHANGE_FIELD_FILTER, READ_BATCH_SIZE);
        String pagingTokenUrl = String.format("%s/rest/v1/activities/pagingtoken.json?access_token=%s&sinceDatetime=%s",
                baseUrl, accessToken, sinceDateTime);

        HashSet leadIdList = new HashSet();

        // Call Get Paging Token API
        JsonObject pagingTokenObj = JsonObject.readFrom(getData(pagingTokenUrl));
        if (pagingTokenObj.get("success").asBoolean()) {

            String nextPageToken = pagingTokenObj.get("nextPageToken").asString();
            boolean moreResult;

            do {
                moreResult = false;

                // Call Get Lead Changes API
                JsonObject leadChangesObj = JsonObject.readFrom(getData(String.format("%s&nextPageToken=%s",
                        leadChangesUrl, nextPageToken)));
                if (leadChangesObj.get("success").asBoolean()) {
                    moreResult = leadChangesObj.get("moreResult").asBoolean();
                    nextPageToken = leadChangesObj.get("nextPageToken").asString();

                    if (leadChangesObj.get("result") != null) {
                        JsonArray resultAry = leadChangesObj.get("result").asArray();
                        for (JsonValue resultObj : resultAry) {
                            int activityTypeId = resultObj.asObject().get("activityTypeId").asInt();


                            // Store lead ids for later use
                            boolean storeThisId = false;
                            if (activityTypeId == ACTIVITY_TYPE_ID_NEW_LEAD) {
                                storeThisId = true;
                            } else if (activityTypeId == ACTIVITY_TYPE_ID_CHANGE_DATA_VALE) {
                                // See if any of the changed fields are of interest to us
                                JsonArray fieldsAry = resultObj.asObject().get("fields").asArray();
                                for (JsonValue fieldsObj : fieldsAry) {
                                    String name = fieldsObj.asObject().get("name").asString();
                                    if (LEAD_CHANGE_FIELD_FILTER.contains(name)) {
                                        storeThisId = true;
                                    }
                                }

                            }

                            if (storeThisId) {
                                leadIdList.add(resultObj.asObject().get("leadId").toString());
                            }
                        }
                    }
                }

            } while (moreResult);
        }

        JsonObject result = new JsonObject();
        JsonArray leads = new JsonArray();

        for (Object o : leadIdList) {
            String leadId = o.toString();

            // Compose Get Lead by Id URL
            String getLeadUrl = String.format("%s/rest/v1/lead/%s.json?access_token=%s",
                    baseUrl, leadId, accessToken);

            // Call Get Lead by Id API
            JsonObject leadObj = JsonObject.readFrom(getData(getLeadUrl));
            if (leadObj.get("success").asBoolean()) {
                if (leadObj.get("result") != null) {
                    JsonArray resultAry = leadObj.get("result").asArray();
                    for (JsonValue resultObj : resultAry) {

                        // Create lead object
                        JsonObject lead = new JsonObject();
                        lead.add("leadId", leadId);
                        lead.add("updatedAt", resultObj.asObject().get("updatedAt").asString());
                        lead.add("firstName", resultObj.asObject().get("firstName").asString());
                        lead.add("lastName", resultObj.asObject().get("lastName").asString());
                        lead.add("email", resultObj.asObject().get("email").asString());

                        // Add lead object to leads array
                        leads.add(lead);
                    }
                }
            }
        }

        // Add leads array to result object
        result.add("result", leads);

        // Print out result object
        System.out.println(result);

        System.exit(0);
    }

    // Perform HTTP GET request
    private static String getData(String endpoint) {
        String data = "";
        try {
            URL url = new URL(endpoint);
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
            urlConn.setRequestMethod("GET");
            urlConn.setAllowUserInteraction(false);
            urlConn.setDoOutput(true);
            int responseCode = urlConn.getResponseCode();
            if (responseCode == 200) {
                InputStream inStream = urlConn.getInputStream();
                data = convertStreamToString(inStream);
            } else {
                System.out.println(responseCode);
                data = "Status:" + responseCode;
            }
        } catch (MalformedURLException e) {
            System.out.println("URL not valid.");
        } catch (IOException e) {
            System.out.println("IOException: " + e.getMessage());
            e.printStackTrace();
        }
        return data;
    }

    private static String convertStreamToString(InputStream inputStream) {
        try {
            return new Scanner(inputStream).useDelimiter("A").next();
        } catch (NoSuchElementException e) {
            return "";
        }
    }
}
```

Ahí lo tienen, [la vida es una canción feliz](https://www.youtube.com/watch?v=zFaBwZDywLk). ¡Disfrútelo!

Publicado el _31-07-2015_ por _David_

## Actualizaciones de la versión de mayo de 2015

### API de REST

* API de oportunidad. Se han introducido nuevos [extremos de oportunidad](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities) que le permiten enumerar, describir y CRUD mediante programación los datos que residen dentro de un objeto de oportunidad de Marketo.

Nota: Se agregaron permisos de función para proporcionar acceso a los extremos de oportunidad: oportunidad de solo lectura, oportunidad de lectura y escritura. Si la función de usuario de la API es anterior al lanzamiento de las API de oportunidad, debe crear una nueva función de usuario de la API con estos permisos para habilitar el acceso. De lo contrario, recibirá una respuesta de error 603 &quot;Acceso denegado&quot;.

* API de recursos: fragmentos de código. Se han introducido nuevos [extremos de recurso para fragmentos](https://developer.adobe.com/marketo-apis/api/asset/#snippet_endpoints) que le permiten manipular objetos de fragmento mediante programación. Los fragmentos de código se pueden utilizar como bloques de contenido dinámico en correos electrónicos y páginas de aterrizaje.
* API de posibles clientes: Actualizar la partición de posibles clientes. Se ha agregado un nuevo extremo de posible cliente [para las particiones](https://developer.adobe.com/marketo-apis/api/mapi/#operation/updatePartitionsUsingPOST) que le permite actualizar la partición para uno o más posibles clientes.
* Se ha corregido un problema en el cual las API relacionadas con posibles clientes no tenían el desplazamiento de zona horaria en los atributos &quot;createdAt&quot; y &quot;updatedAt&quot;.
* Se ha corregido un problema en el cual Programar campaña no devolvía el código de error adecuado cuando se había superado la cantidad máxima diaria de llamadas.
* Se ha corregido un problema en el cual Obtener carpeta por ID a veces devolvía nulo para los atributos &quot;principal&quot; y &quot;descripción&quot;.
* Se ha corregido un problema en el cual Aprobar correo electrónico por ID generaba un error del sistema en algunos casos.
* Se ha corregido un problema en el cual Crear token por ID de carpeta producía un token que no se podía utilizar en determinados casos.

### Real-Time Personalization (RTP)

* API de recomendación de medios enriquecidos. Se han agregado nuevas funciones de [rich media recommendation](/help/javascript-api/web-personalization.md) a la API de JavaScript de RTP. La Recomendación de contenido multimedia enriquecido atrae a los visitantes web con el contenido más relevante basado en el aprendizaje automático y el análisis predictivo. Mejore sus recursos de contenido con descripciones de texto e imágenes e incruste varias recomendaciones de contenido en su sitio web.

### Mobile Engagement SDK

iOS v0.3.4/Android v0.3.3

* Acciones personalizadas. Se ha agregado la capacidad de rastrear la interacción del usuario enviando acciones personalizadas. Para obtener más información, consulte &quot;Envío de acciones personalizadas&quot; [aquí](/help/mobile/mobile.md).
* El método trackPushNotification ha quedado obsoleto.

Publicado el _2015-05-26_ por _David_

## Actualizaciones de la versión de junio de 2015

### API de REST

* API de empresa

Se han introducido nuevos [extremos de compañía](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies) que le permiten enumerar, describir y CRUD mediante programación los datos que residen en un objeto de compañía de Marketo.

Nota: Se han agregado permisos de función para proporcionar acceso a los extremos del programa: compañía de solo lectura, compañía de lectura y escritura. Si la función de usuario de la API es anterior al lanzamiento de las API de la compañía, debe actualizar la función de usuario de la API con estos permisos para habilitar el acceso. De lo contrario, recibirá una respuesta de error 603 &quot;Acceso denegado&quot;

* API de recursos: programas

Se ha actualizado Asset API para que admita carpetas de aplicación y carpetas de programa. Varias API de recursos habían aceptado un ID de carpeta en la solicitud en forma de entero. El ID de carpeta se puede pasar como parte del URI o del cuerpo de la solicitud, según la API.

Ahora debe especificar el tipo de carpeta junto con el ID de carpeta. El tipo de carpeta es &quot;Programa&quot; o &quot;Carpeta&quot;. &quot;Carpeta&quot; especifica una carpeta de aplicación, mientras que &quot;Programa&quot; especifica una carpeta de programa. El tipo de carpeta se especifica de dos formas diferentes, según la API a la que llame:

1. Utilice la estructura de datos FolderIdType. FolderIdType es un bloque JSON simple que contiene el ID y el par de tipos de la siguiente manera:

`{ "id" : **id**, "type" : "**type**" }`

Donde **id** es el id. de carpeta y &quot;**type**&quot; es el tipo de carpeta. Los valores permitidos para &quot;**type**&quot; son &quot;Folder&quot; o &quot;Program&quot;.

Ejemplo - Crear carpeta

`POST /asset/v1/folders.json`

`parent={"id":416,"type":"Folder"}&name=Test Folder&description=This is a test folder`

1. Utilice el parámetro de ID existente y especifique su tipo utilizando un parámetro de consulta &quot;type&quot;.

Ejemplo: obtención de carpeta por ID

`GET /rest/asset/v1/folder/1016.json?type=Program`

Todas las respuestas de la API que contenían un ID de carpeta en el objeto Result ahora también contendrán un atributo folderId cuyo valor es un FolderIdType. Se puede utilizar para determinar el tipo de carpeta para un ID de carpeta determinado.

Ejemplo - Obtener carpeta por nombre

`GET /rest/asset/v1/folder/byName.json?name=Social Media`

```json
"result": [
{
...

"folderId": {"id":341, "type": "Program"},
...

"id":"341"
}
]
```

Para determinar el tipo de carpeta de un ID determinado, puede usar la API [Examinar carpetas](https://developer.adobe.com/marketo-apis/api/asset/browse-folders/).

Se ha cambiado el nombre del atributo &quot;type&quot; en las respuestas de API a &quot;folderType&quot;. Esto sirve para evitar confusiones con el elemento &quot;type&quot; contenido en FolderIdType.

Por ejemplo, a partir de esto:

&quot;**type**&quot;:&quot;Carpeta de marketing&quot;

por esto:

&quot;**folderType**&quot;: &quot;Carpeta de marketing&quot;

### Mobile Engagement SDK

iOS 0.3.5

* Se ha corregido un problema por el cual el cuadro de diálogo Establecer dispositivo de prueba se ejecutaba en el subproceso principal. [MOB-638]
* Se ha añadido la administración de errores en caso de que no se registre el dispositivo de prueba. [MOB-639]

Android 0.3.3

* Se agregó el atributo android:configChanges al elemento AndroidManifest.xml `<activity>` para evitar que se descarte el cuadro de diálogo de progreso al agregar un dispositivo de prueba y cambiar la orientación. [MOB-687]

Publicado el _30-06-2015_ por _David_

## Personalization web en la aplicación (Beta) mediante la API de RTP

Varios de nuestros clientes proporcionan soluciones de aplicaciones web para sus usuarios y recibimos solicitudes si pueden utilizar Marketo Real-Time Personalization (RTP) en su entorno de aplicaciones web seguras. ¡La respuesta es sí! Hemos lanzado una API para la mensajería en la aplicación, para que pueda personalizar el contenido y promocionar actividades de marketing como seminarios web, nuevas versiones de funciones, ventas adicionales y atraer a los usuarios en función de los datos de su aplicación web. Por ejemplo, personalizar el contenido en la aplicación para:

* Ofertas de prueba basadas en la actividad del usuario
* Diferentes tipos de suscripción (formación para mejorar ventas, ventas cruzadas o seminarios web)
* Nuevas funciones relevantes para la actividad del usuario

**Ejemplo de caso de uso** El equipo de éxito del cliente de Marketo utiliza Web Personalization en la aplicación para comunicarse con tipos de suscripción específicos (Spark, Standard, Select o Enterprise) con contenido personalizado, asegurándose de que ven campañas progresivas y nutriendo a los usuarios en la aplicación en función de su participación. Veamos cómo se puede hacer para un usuario con un tipo de suscripción Enterprise. **Requisito previo** para comprender la [API de contexto de usuario de RTP](/help/javascript-api/web-personalization.md). **Habilite la solicitud de la API de contexto de usuario** del soporte técnico de Marketo para habilitar la API de contexto de usuario para su cuenta RTP. **Establecer la variable personalizada** Hay 5 ranuras de variables personalizadas disponibles en RTP para enviar datos a. En este ejemplo, enviaremos un tipo de suscripción de usuario Enterprise a la variable personalizada 1.

`rtp('set', 'customVar1', 'Enterprise');`

**Crear un nuevo segmento RTP** Vaya a **Segmentos**, haga clic en **CREAR NUEVO**.

1. Arrastre el filtro **API de contexto de usuario** al generador de segmentos.
1. Seleccione la **variable personalizada 1** (tipo de suscripción) que **value** es **Enterprise**.

**Mostrar campaña basada en el historial de visitas anteriores** Para mostrar al visitante otra campaña si ya hizo clic en una campaña en una visita anterior.

1. Dentro de la **API de contexto de usuario**, haga clic en **(+)** para agregar otro atributo de API de contexto de usuario
1. Agregar el operador **AND u OR).**
1. Seleccione **Campañas: Se Hizo Clic.** estableció **ID de campaña** en el ID de la campaña. (Consulte la nota siguiente sobre cómo encontrar el ID de campaña).
1. Haga clic en **GUARDAR Y DEFINIR CAMPAÑA** para crear la campaña creativa.

En general, este segmento coincidirá si un visitante está asociado con la variable personalizada (tipo de suscripción) igual a Empresa y si hizo clic en la campaña (ID: 5390) en una visita anterior. El siguiente paso es definir una campaña personalizada para este segmento. La captura de pantalla siguiente muestra una campaña de diálogo de RTP (abajo a la izquierda) en la página Mi Marketo que promociona un seminario web para usuarios de Enterprise.  **NOTA:** **Localizando el identificador de campaña** Vaya a **Campañas**, pase el ratón sobre **Nombre de campaña** para localizar el identificador de campaña.
Publicado el _17-06-2015_ por _David_

## Envío de correos electrónicos transaccionales con la API de REST de Marketo: parte 1

Hay algunos requisitos de configuración en Marketo para ejecutar la llamada necesaria con la API de REST de Marketo.

* El destinatario debe tener un registro en Marketo
* Debe haber un correo electrónico transaccional creado y aprobado en la instancia de Marketo.
* Debe haber una campaña de déclencheur activa con la API Campaign is Requested, Source: Web Service configurada para enviar el correo electrónico

Primero [crea y aprueba tu correo electrónico](https://experienceleague.adobe.com/es/docs/marketo/using/home). Si el correo electrónico es realmente transaccional, es probable que necesite ponerlo en funcionamiento, pero asegúrese de que cumpla los requisitos legales para ser operativo. Se configura desde con la pantalla Editar en Acciones de correo electrónico > Configuración de correo electrónico. Apruébelo y estaremos listos para crear nuestra campaña. Si no tienes experiencia en la creación de campañas, consulta el artículo [Crear una nueva campaña inteligente](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/creating-a-smart-campaign/create-a-new-smart-campaign) en docs.marketo.com. Una vez creada la campaña, se deben seguir estos pasos. Configure la lista inteligente con el déclencheur Campaign is Requested: Ahora necesitamos configurar el flujo para que apunte un paso Send Email a nuestro correo electrónico. Antes de la activación, debe decidir algunos ajustes en la pestaña Programación. Si este correo electrónico en particular solo debe enviarse una vez a un registro determinado, deje la configuración de calificación tal cual. Sin embargo, si es necesario que reciban el correo electrónico varias veces, debe ajustarlo a cada vez o a una de las cadencias disponibles. Ahora estamos listos para activarlos.

### Envío de llamadas de API

**Nota:** En los ejemplos de Java siguientes, estamos usando el paquete minimal-json para manejar las representaciones de JSON en nuestro código. Puede leer más sobre este proyecto aquí: [https://github.com/ralfstx/minimal-json](https://github.com/ralfstx/minimal-json) La primera parte de enviar un correo electrónico transaccional a través de la API es garantizar que exista un registro con la dirección de correo electrónico correspondiente en su instancia de Marketo y que tengamos acceso a su ID de posible cliente. A efectos de esta publicación, supondremos que las direcciones de correo electrónico ya están en Marketo y que solo necesitamos recuperar el ID del registro. Para ello, estamos usando la llamada [Obtener varios posibles clientes por tipo de filtro](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) y estamos reutilizando parte del código Java de la publicación anterior de. Veamos nuestro método Main para que solicite la campaña:

```java
package dev.marketo.blog_request_campaign;

import com.eclipsesource.json.JsonArray;

public class App
{
    public static void main( String[] args )
    {
     //Create an instance of Auth so that we can authenticate with our Marketo instance
        Leads leadsRequest = new Leads(auth).setFilterType("email").addFilterValue("<requestCampaign.test@marketo.com>");

        //Create and parameterize an instance of Leads
        //Set your email filterValue appropriately
        Leads leadsRequest = new Leads(auth).setFilterType("email").addFilterValue("test.requestCamapign@example.com");

        //Get the inner results array of the response
        JsonArray leadsResult = leadsRequest.getData().get("result").asArray();

        //Get the id of the record indexed at 0
        int lead = leadsResult.get(0).asObject().get("id").asInt();

        //Set the ID of your campaign from Marketo
        int campaignId = 0;
        RequestCampaign rc = new RequestCampaign(auth, campaignId).addLead(lead);

        //Send the request to Marketo
        rc.postData();
    }
}
```

Para obtener estos resultados de la respuesta JsonObject de leadRequest, necesitamos escribir algún código. Para recuperar el primer resultado en Array, se debe extraer Array del objeto JsonObject y indexar el objeto en 0:

`JsonArray leadsResult = leadsRequest.getData().get("result").asArray();`
`int leadId = leadsResult.get(0).asObject().get("id").asInt();`

Desde aquí, todo lo que hay que hacer es solicitar la llamada de campaña. Para ello, los parámetros requeridos son ID en la dirección URL de la solicitud y una matriz de objetos JSON que contenga un miembro, &quot;id&quot;. Vamos a echar un vistazo al código para esto:

```java
package dev.marketo.blog_request_campaign;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.io.Reader;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.ArrayList;
import javax.net.ssl.HttpsURLConnection;
import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;

public class RequestCampaign {
 private String endpoint;
 private Auth auth;
 public ArrayList leads = new ArrayList();
 public ArrayList tokens = new ArrayList();

 public RequestCampaign(Auth auth, int campaignId) {
  this.auth = auth;
  this.endpoint = this.auth.marketoInstance + "/rest/v1/campaigns/" + campaignId + "/trigger.json";
 }
 public RequestCampaign setLeads(ArrayList leads) {
  this.leads = leads;
  return this;
 }
 public RequestCampaign addLead(int lead){
  leads.add(lead);
  return this;
 }
 public RequestCampaign setTokens(ArrayList tokens) {
  this.tokens = tokens;
  return this;
 }
 public RequestCampaign addToken(String tokenKey, String val){
  JsonObject jo = new JsonObject().add("name", tokenKey);
  jo.add("value", val);
  tokens.add(jo);
  return this;
 }
 public JsonObject postData(){
  JsonObject result = null;
  try {
   JsonObject requestBody = buildRequest(); //builds the Json Request Body
   String s = endpoint + "?access_token=" + auth.getToken(); //takes the endpoint URL and appends the access_token parameter to authenticate
   System.out.println("Executing RequestCampaign calln" + "Endpoint: " + s + "nRequest Body:n"  + requestBody);
   URL url = new URL(s);
   HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection(); //Return a URL connection and cast to HttpsURLConnection
   urlConn.setRequestMethod("POST");
   urlConn.setRequestProperty("Content-type", "application/json");
            urlConn.setRequestProperty("accept", "text/json");
            urlConn.setDoOutput(true);
   OutputStreamWriter wr = new OutputStreamWriter(urlConn.getOutputStream());
   wr.write(requestBody.toString());
   wr.flush();
   InputStream inStream = urlConn.getInputStream(); //get the inputStream from the URL connection
   Reader reader = new InputStreamReader(inStream);
   result = JsonObject.readFrom(reader); //Read from the stream into a JsonObject
   System.out.println("Result:n" + result);
  } catch (MalformedURLException e) {
   e.printStackTrace();
  } catch (IOException e) {
   e.printStackTrace();
  }
  return result;
 }

 private JsonObject buildRequest(){
  JsonObject requestBody = new JsonObject(); //Create a new JsonObject for the Request Body
  JsonObject input = new JsonObject();
  JsonArray leadsArray = new JsonArray();
  for (int lead : leads) {
   JsonObject jo = new JsonObject().add("id", lead);
   leadsArray.add(jo);
  }
  input.add("leads", leadsArray);
  JsonArray tokensArray = new JsonArray();
  for (JsonObject jo : tokens) {
   tokensArray.add(jo);
  }
  input.add("tokens", tokensArray);
  requestBody.add("input", input);
  return requestBody;
 }

}
```

Esta clase tiene un constructor que toma un Auth y el ID de la campaña. Los posibles clientes se agregan al objeto pasando una ArrayList `<Integer>` que contiene los identificadores de los registros que se van a establecerLeads, o utilizando addLead, que toma un entero y lo anexa a la ArrayList existente en la propiedad lead. Para almacenar en déclencheur la llamada de API para pasar los registros de posibles clientes a la campaña, es necesario llamar a postData, que devuelve un JsonObject que contiene los datos de respuesta de la solicitud. Cuando se llama a una campaña de solicitud, cada posible cliente pasado a la llamada se procesa mediante la campaña de déclencheur de destinatario en Marketo y se envía el correo electrónico que se creó anteriormente. ¡Enhorabuena! Ha activado un correo electrónico mediante la API de REST de Marketo. Esté atento a la Parte 2, donde analizaremos la personalización dinámica del contenido de un correo electrónico a través de la Campaña de solicitud.

Publicado el _2015-07-17_ por _Kenny_

## Autenticación y recuperación de datos de posibles clientes de Marketo con la API de REST

**Nota:** En los ejemplos de Java siguientes, estamos usando el paquete minimal-json para manejar las representaciones de JSON en nuestro código. Puede leer más sobre este proyecto aquí: [https://github.com/ralfstx/minimal-json](https://github.com/ralfstx/minimal-json) Uno de los requisitos más comunes al integrarse con Marketo es la recuperación de datos de posibles clientes. La mayoría de las integraciones, si no todas, requerirán la recuperación o el envío de datos de posibles clientes desde una suscripción de Marketo, por lo que hoy daremos un vistazo a una tarea básica de recuperación de información de posibles clientes, [autenticándonos](/help/rest-api/authentication.md) con una suscripción y luego recuperando los datos de posibles clientes de la misma. Para recuperar los posibles clientes, primero debemos autenticarnos con la instancia de Marketo de destino mediante OAuth 2.0. Hay tres partes de información que necesitamos para autenticarnos con Marketo, el ID de cliente, el secreto de cliente y el host de la instancia de Marketo. Esta es la clase que utilizamos para autenticarnos:

```java
package dev.marketo.blog_leads;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.Reader;
import java.net.MalformedURLException;
import java.net.URL;
import javax.net.ssl.HttpsURLConnection;
import com.eclipsesource.json.JsonObject;

public class Auth {
 protected String marketoInstance; //Instance Host obtained from Admin -> Web Service
 private String clientId; //clientId obtained from Admin -> Launchpoint -> View Details for selected service
 private String clientSecret; //clientSecret obtained from Admin -> Launchpoint -> View Details for selected service
 private String idEndpoint; //idEndpoint constructed to authenticate with service when constructor is used
 private String token; //token is stored for reuse until expiration
 private Long expiry; //used to store time of expiration

 //Creates an instance of Auth which is used to Authenticate with a particular service on a particular instance
 public Auth(String id, String secret, String instanceUrl) {
  this.clientId = id;
  this.clientSecret = secret;
  this.marketoInstance = instanceUrl;
  this.idEndpoint = marketoInstance + "/identity/oauth/token?grant_type=client_credentials&client_id=" + clientId + "&client_secret=" + clientSecret;
 }
 //The only public method of Auth, used to check if the current value of Token is valid, and then to retrieve a new one if it is not
 public String getToken(){
  long now  = System.currentTimeMillis();
  if (expiry == null || expiry < now){
   System.out.println("Token is empty or expired. Trying new authentication");
   JsonObject jo = getData();
   System.out.println("Got Authentication Response: " + jo);
   this.token = jo.get("access_token").asString();
                        //expires_in is reported as seconds, set expiry to system time in ms + expires \* 1000
   this.expiry = System.currentTimeMillis() + (jo.get("expires_in").asLong() \* 1000);
  }
  return this.token;
 }
 //Executes the authentication request
 private JsonObject getData(){
  JsonObject jsonObject = null;
  try {
   URL url = new URL(idEndpoint);
   HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
   urlConn.setRequestMethod("GET");
            urlConn.setRequestProperty("accept", "application/json");
            System.out.println("Trying to authenticate with " + idEndpoint);
            int responseCode = urlConn.getResponseCode();
            if (responseCode == 200) {
                InputStream inStream = urlConn.getInputStream();
                Reader reader = new InputStreamReader(inStream);
                jsonObject = JsonObject.readFrom(reader);
            }else {
                throw new IOException("Status: " + responseCode);
            }
  } catch (MalformedURLException e) {
   e.printStackTrace();
  }catch (IOException e) {
            e.printStackTrace();
        }
  return jsonObject;
 }
}
```

Este código le permite crear una instancia de autenticación con su ID de cliente, secreto de cliente y host (como marketoInstance) desde Administración -> Launchpoint (ID y secreto) y Administración -> Servicios web (host). Expone el método getToken que comprueba si el token almacenado actualmente es nulo o ha caducado y, a continuación, devuelve el token existente o realiza una nueva solicitud de autenticación y, a continuación, devuelve el nuevo token del miembro &quot;access_token&quot; de la respuesta JSON. Ahora que puede autenticarse con su instancia de Marketo, el siguiente paso es recuperar sus posibles clientes. Se utiliza esta clase:

```java
package dev.marketo.blog_leads;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.Reader;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.ArrayList;
import javax.net.ssl.HttpsURLConnection;
import com.eclipsesource.json.JsonObject;

public class Leads {
 private StringBuilder endpoint;
 private Auth auth;
 public String filterType;
 public ArrayList filterValues = new ArrayList();
 public Integer batchSize;
 public String nextPageToken;
 public ArrayList fields = new ArrayList();

 public Leads(Auth a) {
  this.auth = a;
  this.endpoint = new StringBuilder(this.auth.marketoInstance + "/rest/v1/leads.json");
 }
 public Leads setFilterType(String filterType) {
  this.filterType = filterType;
  return this;
 }
 public Leads setFilterValues(ArrayList filterValues) {
  this.filterValues = filterValues;
  return this;
 }
 public Leads addFilterValue(String filterVal) {
  this.filterValues.add(filterVal);
  return this;
 }
 public Leads setBatchSize(Integer batchSize) {
  this.batchSize = batchSize;
  return this;
 }
 public Leads setNextPageToken(String nextPageToken) {
  this.nextPageToken = nextPageToken;
  return this;
 }
 public Leads setFields(ArrayList fields) {
  this.fields = fields;
  return this;
 }

 public JsonObject getData() {
        JsonObject result = null;
        try {
         endpoint.append("?access_token=" + auth.getToken() + "&filterType=" + filterType + "&filterValues=" + csvString(filterValues));
         if (batchSize != null && batchSize > 0 && batchSize <= 300){
             endpoint.append("&batchSize=" + batchSize);
            }
            if (nextPageToken != null){
             endpoint.append("&nextPageToken=" + nextPageToken);
            }
            if (fields != null){
             endpoint.append("&fields=" + csvString(fields));
            }
            URL url = new URL(endpoint.toString());
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
            urlConn.setRequestMethod("GET");
            urlConn.setRequestProperty("accept", "text/json");
            InputStream inStream = urlConn.getInputStream();
            Reader reader = new InputStreamReader(inStream);
            result = JsonObject.readFrom(reader);
        } catch (MalformedURLException e) {
            System.out.println("URL not valid.");
        } catch (IOException e) {
            System.out.println("IOException: " + e.getMessage());
            e.printStackTrace();
        }
        return result;
    }
 private String csvString(ArrayList fields) {
  StringBuilder fieldCsv = new StringBuilder();
     for (int i = 0; i < fields.size(); i++){
      fieldCsv.append(fields.get(i));
      if (i + 1 != fields.size()){
       fieldCsv.append(",");
      }
     }
  return fieldCsv.toString();
 }
}
```

Esta clase tiene un único constructor que acepta un objeto Auth y, a continuación, expone varios establecedores para los parámetros opcionales y requeridos. En este caso, solo tenemos que preocuparnos por configurar filterType y filterValues para obtener posibles clientes por dirección de correo electrónico. Por lo tanto, utilizaremos setFilterType para &quot;correo electrónico&quot; y addFilterValue para la dirección de correo electrónico para la que necesitamos recuperar un ID. Una vez configurados los parámetros, puede utilizar el método getData para recuperar un objeto JsonObject del extremo de posibles clientes, que contiene una matriz de resultados que tiene una representación JSON de los registros de posibles clientes recuperados.

### Poniéndolo todo junto

Ahora que hemos revisado el código de ejemplo que estamos utilizando, veamos un ejemplo sencillo para recuperar posibles clientes que coincidan con una dirección de correo electrónico de prueba, <testlead@marketo.com>. Para esto, necesitamos usar setFilterType para &quot;correo electrónico&quot; y addFilterValue para la dirección de correo electrónico para la que necesitamos recuperar información. Una vez configurados los parámetros, puede utilizar el método getData para recuperar un objeto JsonObject del extremo de posibles clientes, que contiene una matriz de resultados que tiene una representación JSON de los registros de posibles clientes recuperados.

```java
package dev.marketo.blog_leads;

import com.eclipsesource.json.JsonObject;

public class App
{
    public static void main( String[] args )
    {
     //Create an instance of Auth so that we can authenticate with our Marketo instance
        //Change the credentials here to your own
        Auth auth = new Auth("CHANGE ME", "CHANGE ME", "CHANGE ME");
        //Create and parameterize an instance of Leads
        Leads getLeads = new Leads(auth)
              .setFilterType("email")
              .addFilterValue("<testlead@marketo.com>");
        //get the inner results array of the response
        JsonObject leadsResult = getLeads.getData();
        System.out.println(leadsResult);
    }
}
```

En este ejemplo de método principal, creamos una instancia de Auth y, a continuación, pasamos esto a un nuevo constructor de posibles clientes. Con setFilterType y addFilterValue, configuramos nuestra instancia de posibles clientes para que recupere únicamente los posibles clientes que coincidan con la dirección de correo electrónico &quot;<testlead@marketo.com>&quot;. Este ejemplo imprime esto en la consola:

El token está vacío o caducado. Intentando nueva autenticación
Intentando autenticar con <https://299-BYM-827.mktorest.com/identity/oauth/token?grant_type=client_credentials&client_id=b417d98f-9289-47d1-a61f-db141bf0267f&client_secret=0DipOvz4h2wP1ANeVjlfwMvECJpo0ZYc>
Se obtuvo la respuesta de autenticación: {&quot;access_token&quot;:&quot;ec0f02c0-28ac-4d6c-b7d7-00e47ae85ff1:st&quot;,&quot;token_type&quot;:&quot;bearer&quot;,&quot;expires_in&quot;:538,&quot;scope&quot;:&quot;<apiuser@mktosupport.com>&quot;}
&lbrace;&quot;requestId&quot;:&quot;14fb6#14e6a7a9ad6&quot;,&quot;result&quot;:[{&quot;id&quot;:1026322,&quot;updatedAt&quot;:&quot;2015-07-07T21:43:25Z&quot;,&quot;lastName&quot;:&quot;Lead&quot;,&quot;email&quot;:&quot;<testlead@marketo.com>&quot;,&quot;createdAt&quot;:&quot;2015-07-07T21:43:25Z&quot;,&quot;firstName&quot;:&quot;Test&quot;},{&quot;id&quot;:1026323,&quot;updatedAt&quot;:&quot;2015-07-07T21:43:43Z&quot;,&quot;lastName&quot;:&quot;Lead2&quot;,&quot;email&quot;:&quot;<testlead@marketo.com>&quot;,&quot;createdAt&quot;:&quot;2015-07-07T21:43:43Z&quot;,&quot;firstName&quot;:&quot;Test&quot;}],&quot;success&quot;:true

Ahora tenemos datos de posibles clientes que podemos procesar de la manera que necesitemos. Gracias por leer, y por favor deje cualquier comentario que tenga en los comentarios.

Publicado el _2015-07-10_ por _Kenny_

## Actualizaciones de la versión de julio de 2015

API de REST

* API de vendedor

Se han introducido nuevos [extremos de vendedor](/help/rest-api/sales-persons.md) que le permiten enumerar, describir y CRUD mediante programación los datos que residen en un objeto de vendedor de Marketo. Además, se puede asignar un vendedor a un posible cliente, una oportunidad o una compañía. Para ello, especifique un atributo &quot;externalSalesPersonId&quot; al llamar al punto de conexión Create/Update/Upsert para el posible cliente, la oportunidad o la compañía.

Nota: Se han agregado permisos de funciones para proporcionar acceso a los extremos del programa: Vendedor de solo lectura, Vendedor de lectura-escritura. Si la función de usuario de la API es anterior a la publicación de las API de vendedor, debe actualizar la función de usuario de la API con estos permisos para habilitar el acceso. De lo contrario, recibirá una respuesta de error 603 &quot;Acceso denegado&quot;.

* API de recursos: plantilla de página de aterrizaje

Se han introducido nuevos [extremos de plantilla de página de aterrizaje](https://developer.adobe.com/marketo-apis/api/asset/#landing_page_templates_endpoints) que le permiten enumerar, crear y actualizar mediante programación los datos asociados con una plantilla de página de aterrizaje.

* API de recursos: Segmentos

Se han introducido dos extremos relacionados con el segmento:

[Obtener segmentos](https://developer.adobe.com/marketo-apis/api/asset/get-segments)

[Obtener segmentación por identificador](https://developer.adobe.com/marketo-apis/api/asset/get-segmentation-by-id)

* Se ha corregido un problema por el cual Obtener carpeta por nombre no respetaba el parámetro &quot;workspace&quot;. [LM-61059]
* Se han realizado varias mejoras de rendimiento en las API de objeto personalizado.

Publicado el _17-07-2015_ por _David_

## Crear y asociar posibles clientes, empresas y oportunidades con la API de REST de Marketo

Para aprovechar al máximo las ventajas del análisis de Marketo, es crucial crear asociaciones correctas y sólidas entre los registros de posibles clientes, empresas y oportunidades. Cuando no está aprovechando una sincronización de CRM nativa, la creación de estas relaciones puede plantear algunas dificultades, por lo que hoy caminamos a través de la construcción de ellos.

### Relaciones de objetos

En Marketo, hay algunas relaciones vitales para establecer completamente la creación de informes de oportunidades:

* Los posibles clientes y las oportunidades tienen una relación de varios a varios con el objeto OpportunityRole.
* OpportunityRole tiene un campo leadId y externaloportunityid para crear la relación de posible cliente a oportunidad.
* Para poder optar a un filtro de lista inteligente Tiene oportunidad, un posible cliente debe tener una función de oportunidad relacionada con una oportunidad.
* Las oportunidades tienen una relación varios a uno con el objeto Compañía a través del campo externalCompanyId.
* Los posibles clientes tienen una relación &quot;uno a varios&quot; con las compañías a través del campo externalCompanyId.
* Las oportunidades se atribuyen a un programa en función del programa de adquisición de un posible cliente o de su pertenencia y éxito en un programa (consulte [Explicación de la atribución](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/reporting/revenue-cycle-analytics/revenue-tools/attribution/understanding-attribution)).

La creación de estas relaciones en toda la base de datos de posibles clientes le permitirá aprovechar completamente el análisis de Marketo y ver la influencia que sus programas tienen en la creación de oportunidades y las tasas de ganancia.

### Compañías

La forma más sencilla de crear estas relaciones es a partir de la creación de la empresa. Esto garantiza que podamos pasar externalCompanyId a nuestras oportunidades durante la creación, en lugar de tener que realizar llamadas de API adicionales para actualizar las oportunidades después de crearlas. Según la configuración existente, este puede ser o no un paso necesario, pero los nuevos posibles clientes y contactos netos con empresas asociadas deben tener estos registros agregados a la instancia de Marketo para que se creen las relaciones, así que echemos un vistazo a algún código para crear registros de empresa.

```java
package dev.marketo.opportunities;

import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.io.Reader;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.ArrayList;
import java.util.List;

import javax.net.ssl.HttpsURLConnection;

import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;

public class UpsertCompanies {
 public List<JsonObject> input; //a list of Companies to use for input.  Each must have a member "externalCompanyId".
 public String action; //specify the action to be undertaken, createOnly, updateOnly, createOrUpdate
 public String dedupeBy; //select mode of Deduplication, dedupeFields for all dedupe parameters(externalCompanyId), idField for marketoId
 private String endpoint; //endpoint URL created with Constructor
 private Auth auth; //Marketo Auth Object


 //Constructs an UpsertOpportunities with Auth, but with no input set
 public UpsertCompanies(Auth auth){
  this.auth = auth;
  this.endpoint = this.auth.marketoInstance + "/rest/v1/companies.json";
 }
 //Constructs and UpsertOpportunities with Auth and input set
 public UpsertCompanies(Auth auth, List<JsonObject> input) {
  this(auth);
  this.input = input;
 }
 //adds input to existing list, creates arraylist if it was built without a list
 public UpsertCompanies addCompanies(JsonObject... companies){
  if (this.input == null){
   this.input = new ArrayList();
  }
  for (JsonObject jo : companies) {
   System.out.println(jo);
   this.input.add(jo);
  }
  return this;
 }

 public JsonObject postData(){
  JsonObject result = null;
  try {
   JsonObject requestBody = buildRequest(); //builds the Json Request Body
   String s = endpoint + "?access_token=" + auth.getToken(); //takes the endpoint URL and appends the access_token parameter to authenticate
   URL url = new URL(s);
   HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection(); //Return a URL connection and cast to HttpsURLConnection
   urlConn.setRequestMethod("POST");
   urlConn.setRequestProperty("Content-type", "application/json");
            urlConn.setRequestProperty("accept", "text/json");
            urlConn.setDoOutput(true);
   OutputStreamWriter wr = new OutputStreamWriter(urlConn.getOutputStream());
   wr.write(requestBody.toString());
   wr.flush();
   InputStream inStream = urlConn.getInputStream(); //get the inputStream from the URL connection
   Reader reader = new InputStreamReader(inStream);
   result = JsonObject.readFrom(reader); //Read from the stream into a JsonObject
   urlConn.disconnect();
  } catch (MalformedURLException e) {
   e.printStackTrace();
  } catch (IOException e) {
   e.printStackTrace();
  }
  return result;
 }

 private JsonObject buildRequest(){
  JsonObject requestBody = new JsonObject(); //Create a new JsonObject for the Request Body
  JsonArray in = new JsonArray(); //Create a JsonArray for the "input" member to hold Opp records
  for (JsonObject jo : input) {
   in.add(jo); //add our company records to the input array
  }
  requestBody.add("input", in);
  if (this.action != null){
   requestBody.add("action", action); //add the action member if available
  }
  if (this.dedupeBy != null){
   requestBody.add("dedupeBy", dedupeBy); //add the dedupeBy member if available
  }
  return requestBody;
 }
 //Getters and Setters
 //Setters return the UpsertCompanies instance to allow simple formatting:
 public List<JsonObject> getInput() {
  return input;
 }
 //sets or replaces existing input with list
 public UpsertCompanies setInput(List input) {
  this.input = input;
  return this;
 }
 public String getAction() {
  return action;
 }
 public UpsertCompanies setAction(String action) {
  this.action = action;
  return this;
 }
 public String getDedupeBy() {
  return dedupeBy;
 }
 public UpsertCompanies setDedupeBy(String dedupeBy) {
  this.dedupeBy = dedupeBy;
  return this;
 }

}
```

La API de compañías proporciona dos opciones de anulación de duplicación, establecidas por el parámetro dedupeBy de la solicitud, &quot;dedupeFields&quot; y &quot;idField&quot;. Se pueden recuperar explícitamente llamando a Describir compañías. Si dedupeBy no está establecido, el valor predeterminado es dedupeFields. En el caso de los registros de empresa, dedupeFields siempre corresponde a &quot;externalCompanyId&quot;, que es una cadena arbitraria establecida por un origen externo, y a idField, correspondiente al campo &quot;marketoId&quot;, que es un entero generado y devuelto por Marketo después de la creación. Según la selección de deduplicarBy, se debe incluir uno de externalCompanyId o marketoId en cualquier llamada de actualización para un registro de empresa. Estos mismos requisitos se aplican a las API de objetos de función Oportunidad y Oportunidad. Nuestro código expone dos constructores: uno que acepta un solo argumento de un objeto Auth y otro que acepta Auth y una lista de registros de empresa [JsonObject](https://github.com/ralfstx/minimal-json). Si se construye sin una List de entrada, los registros de compañía deben agregarse a través del método addCompanies, que comprobará la creación de una nueva ArrayList si la entrada es nula y, a continuación, agregará todos los argumentos JsonObject a la List de entrada. Este es un ejemplo de uso:

```java
//Create a new company to associate to
JsonObject myCompany = new JsonObject().add("externalCompanyId", "myCompany");
UpsertCompanies upsertCompanies = new UpsertCompanies(auth).addCompanies(myCompany);
JsonObject companiesResult = upsertCompanies.postData();
System.out.println(companiesResult);
```

Estamos creando un objeto JsonObject de compañía único con un solo campo, `externalCompanyId`, a continuación, construyendo una instancia de UpsertCompanies y agregando nuestra compañía a la lista de entrada con `addCompanies`.

### Oportunidades

Similar a los objetos de compañía, la API de oportunidad tiene un parámetro `dedupeBy`, que acepta &quot;dedupeFields&quot; o &quot;idField&quot;, correspondientes a &quot;externaloportunityid&quot; y &quot;marketoGUID&quot; respectivamente. Este es nuestro código, que se parece bastante a la clase UpsertCompanies:

```java
package dev.marketo.opportunities;

import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.io.Reader;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.ArrayList;
import java.util.List;

import javax.net.ssl.HttpsURLConnection;

import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;

public class UpsertOpportunities {
 public List<JsonObject> input; //a list of Opportunities to use for input.  Each must have a member "externalopportunityid".  Each can optionally include "externalCompanyId" for company association
 public String action; //specify the action to be undertaken, createOnly, updateOnly, createOrUpdate
 public String dedupeBy; //select mode of Deduplication, dedupeFields for all dedupe parameters, idField for marketoId
 private String endpoint; //endpoint URL created with Constructor
 private Auth auth; //Marketo Auth Object


 //Constructs an UpsertOpportunities with Auth, but with no input set
 public UpsertOpportunities(Auth auth){
  this.auth = auth;
  this.endpoint = this.auth.marketoInstance + "/rest/v1/opportunities.json";
 }
 //Constructs and UpsertOpportunities with Auth and input set
 public UpsertOpportunities(Auth auth, List<JsonObject> input) {
  this(auth);
  this.input = input;
 }
 public UpsertOpportunities addOpportunities(JsonObject... opp){
  if (this.input == null){
   this.input = new ArrayList();
  }
  for (JsonObject jo : opp) {
   System.out.println(jo);
   this.input.add(jo);
  }
  return this;
 }

 public JsonObject postData(){
  JsonObject result = null;
  try {
   JsonObject requestBody = buildRequest(); //builds the Json Request Body
   String s = endpoint + "?access_token=" + auth.getToken(); //takes the endpoint URL and appends the access_token parameter to authenticate
   URL url = new URL(s);
   HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection(); //Return a URL connection and cast to HttpsURLConnection
   urlConn.setRequestMethod("POST");
   urlConn.setRequestProperty("Content-type", "application/json");
            urlConn.setRequestProperty("accept", "text/json");
            urlConn.setDoOutput(true);
   OutputStreamWriter wr = new OutputStreamWriter(urlConn.getOutputStream());
   wr.write(requestBody.toString());
   wr.flush();
   InputStream inStream = urlConn.getInputStream(); //get the inputStream from the URL connection
   Reader reader = new InputStreamReader(inStream);
   result = JsonObject.readFrom(reader); //Read from the stream into a JsonObject
   urlConn.disconnect();
  } catch (MalformedURLException e) {
   e.printStackTrace();
  } catch (IOException e) {
   e.printStackTrace();
  }
  return result;
 }

 private JsonObject buildRequest(){
  JsonObject requestBody = new JsonObject(); //Create a new JsonObject for the Request Body
  JsonArray in = new JsonArray(); //Create a JsonArray for the "input" member to hold Opp records
  for (JsonObject jo : input) {
   in.add(jo); //add our Opportunity records to the input array
  }
  requestBody.add("input", in);
  if (this.action != null){
   requestBody.add("action", action); //add the action member if available
  }
  if (this.dedupeBy != null){
   requestBody.add("dedupeBy", dedupeBy); //add the dedupeBy member if available
  }
  return requestBody;
 }
 //Getters and Setters
 //Setters return the UpsertOpportunites instance to allow simple formatting:
 public List<JsonObject> getInput() {
  return input;
 }
 public UpsertOpportunities setInput(List<JsonObject> input) {
  this.input = input;
  return this;
 }
 public String getAction() {
  return action;
 }
 public UpsertOpportunities setAction(String action) {
  this.action = action;
  return this;
 }
 public String getDedupeBy() {
  return dedupeBy;
 }
 public UpsertOpportunities setDedupeBy(String dedupeBy) {
  this.dedupeBy = dedupeBy;
  return this;
 }

}
```

Se proporcionan las mismas opciones de constructor, tomando un método Auth o `Auth+List<JsonObject>`, y un método `addOpportunities` para introducir oportunidades de JsonObject. Este es un ejemplo de uso:

```java
//Create some JsonObjects for Opportunity Data
JsonObject opp1 = new JsonObject().add("name", "opportunity1")
    .add("externalopportunityid", "Opportunity1Test")
    .add("externalCompanyId", "myCompany")
    .add("externalCreatedDate", "2015-01-01T00:00:00z");
JsonObject opp2 = new JsonObject().add("name", "opportunity2")
    .add("externalopportunityid", "Opportunity2Test")
    .add("externalCompanyId", "myCompany")
    .add("externalCreatedDate", "2015-01-01T00:00:00z");

//Create an Instance of UpsertOpportunities and POST it
UpsertOpportunities upsertOpps = new UpsertOpportunities(auth)
                        .setAction("createOnly")
                        .addOpportunities(opp1, opp2);
JsonObject oppsResult = upsertOpps.postData();
System.out.println(oppsResult);
```

Aquí creamos dos oportunidades de ejemplo y les damos valores para los campos name, externaloportunityid, externalCompanyId y externalCreatedDate. Todavía no hemos hablado de externalCreatedDate, pero es importante utilizarlo, ya que se trata como el campo maestro en RCE para cuando se creó una oportunidad, lo que lo hace importante para la atribución correcta. Puede utilizar la lógica empresarial de su organización para determinar lo que introduce en este campo, en función de si va a rellenar los datos de oportunidad existentes o a crear otros nuevos sobre la marcha. Creamos nuestra instancia de UpsertOpportunity y luego agregamos nuestros JsonObjects a través de addOpportunity. Ahora que la instancia está configurada, puede insertarla en Marketo con postData e imprimir el resultado

### Funciones

Las funciones son bastante similares a los dos objetos anteriores, excepto que tienen un requisito ligeramente diferente al establecer dedupeBy en dedupeFields. Las funciones requieren que se incluyan tres campos al crear o actualizar un registro mediante este método, &quot;leadId&quot;, &quot;role&quot; y &quot;externaloportunityid&quot;. &quot;role&quot; puede ser cualquier valor de cadena, pero los otros dos deben hacer referencia a un ID válido de posible cliente y a un ID válido de una oportunidad, respectivamente.

```java
package dev.marketo.opportunities;

import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.io.Reader;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.ArrayList;
import java.util.List;

import javax.net.ssl.HttpsURLConnection;

import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;

public class UpsertOpportunityRoles {
 public List<JsonObject> input; //Array of Opportunity Roles as JsonObjects, must have "leadId", "role" and "externalopprtunityid"
 public String action; //specify the action to be undertaken, createOnly, updateOnly, createOrUpdate, defaults to createOrUpdate if unset
 public String dedupeBy;//select mode of Deduplication, dedupeFields for all dedupe parameters, idField for marketoId
 private String endpoint; //endpoint URL created with Constructor
 private Auth auth; //Marketo Auth Object

 //Constructs an UpsertOpportunityRoles with Auth, but with no input set
 public UpsertOpportunityRoles(Auth auth) {
  this.auth = auth;
  this.endpoint = this.auth.marketoInstance + "/rest/v1/opportunities/roles.json";
 }
 //Constructs and UpsertOpportunities with Auth and input set
 public UpsertOpportunityRoles(Auth auth, List<JsonObject> input) {
  this(auth);
  this.input = input;
 }
 public UpsertOpportunityRoles addRoles(JsonObject... role){
  if (this.input == null){
   this.input = new ArrayList();
  }
  for (JsonObject jo : role) {
   System.out.println(jo);
   this.input.add(jo);
  }
  return this;
 }
 //executes the request to Marketo, body will be empty if input is not set
 public JsonObject postData(){
  JsonObject result = null;
  try {
   JsonObject requestBody = buildRequest(); //builds the Json Request Body
   String s = endpoint + "?access_token=" + auth.getToken(); //takes the endpoint URL and appends the access_token parameter to authenticate
   URL url = new URL(s);
   HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection(); //Return a URL connection and cast to HttpsURLConnection
   urlConn.setRequestMethod("POST");
   urlConn.setRequestProperty("Content-type", "application/json");//"application/json" content-type is required.
            urlConn.setRequestProperty("accept", "text/json");
            urlConn.setDoOutput(true);
   OutputStreamWriter wr = new OutputStreamWriter(urlConn.getOutputStream());
   wr.write(requestBody.toString());
   wr.flush();
   InputStream inStream = urlConn.getInputStream();  //get the inputStream from the URL connection
   Reader reader = new InputStreamReader(inStream);
   result = JsonObject.readFrom(reader); //Read from the stream into a JsonObject
   urlConn.disconnect();
  } catch (MalformedURLException e) {
   e.printStackTrace();
  } catch (IOException e) {
   e.printStackTrace();
  }
  return result;
 }

 public JsonObject buildRequest(){
  JsonObject requestBody = new JsonObject();
  JsonArray in = new JsonArray();
  for (JsonObject jo : input) {
   in.add(jo);
  }
  requestBody.add("input", in);
  if (this.action != null){
   requestBody.add("action", action);
  }
  if (this.dedupeBy != null){
   requestBody.add("dedupeBy", dedupeBy);
  }
  return requestBody;
 }
 //Getters and Setters
 //Setters return the UpsertOpportunites instance to allow simple formatting:
 public List<JsonObject> getInput() {
  return input;
 }
 public UpsertOpportunityRoles setInput(List<JsonObject> input) {
  this.input = input;
  return this;
 }
 public String getAction() {
  return action;
 }
 public UpsertOpportunityRoles setAction(String action) {
  this.action = action;
  return this;
 }
 public String getDedupeBy() {
  return dedupeBy;
 }
 public UpsertOpportunityRoles setDedupeBy(String dedupeBy) {
  this.dedupeBy = dedupeBy;
  return this;
 }

}
```

Estamos siguiendo un patrón similar para los constructores y el método addRoles al de los ejemplos anteriores. A continuación se muestra un ejemplo.

```java
//Create Some opp roles now
JsonObject opp1Role = new JsonObject()
                        .add("role", "Captain")
                        .add("externalopportunityid", opp1.get("externalopportunityid").asString())
                        .add("leadId", 318794);
JsonObject opp2Role = new JsonObject()
                        .add("role", "Commander")
                        .add("externalopportunityid", opp2.get("externalopportunityid").asString())
                        .add("leadId", 318795);

//Create an Instance of UpsertOpportunityRoles and POST it
UpsertOpportunityRoles upsertRoles = new UpsertOpportunityRoles(auth)
                        .setAction("createOnly")
                        .addRoles(opp1Role, opp2Role);
JsonObject rolesResult = upsertRoles.postData();
System.out.println(rolesResult);
```

Aquí estamos creando los nuevos JsonObjects para nuestras dos funciones de ejemplo y agregando sus campos desduplicados requeridos, extrayendo el ID de oportunidad externo de las oportunidades que ya hemos creado y luego empujándolos a Marketo.

### Poniéndolo Todo Junto

Este es el ejemplo completo de nuestro método principal:

```java
package dev.marketo.opportunities;

import com.eclipsesource.json.JsonObject;

public class App
{
    public static void main( String[] args )
    {
     //create an Instance of Auth
        Auth auth = new Auth("CLIENT_ID_CHANGE_ME", "CLIENT_SECRET_CHANGE_ME", "MARKETO_HOST_CHANGE_ME");

        //Create a new company to associate to
        JsonObject myCompany = new JsonObject().add("externalCompanyId", "myCompany");
        UpsertCompanies upsertCompanies = new UpsertCompanies(auth).addCompanies(myCompany);
        JsonObject companiesResult = upsertCompanies.postData();
        System.out.println(companiesResult);

        //Create some JsonObjects for Opportunity Data
        JsonObject opp1 = new JsonObject().add("name", "opportunity1")
           .add("externalopportunityid", "Opportunity1Test")
           .add("externalCompanyId", "myCompany")
           .add("externalCreatedDate", "2015-01-01T00:00:00z");
        JsonObject opp2 = new JsonObject().add("name", "opportunity2")
           .add("externalopportunityid", "Opportunity2Test")
           .add("externalCompanyId", "myCompany")
           .add("externalCreatedDate", "2015-01-01T00:00:00z");

        //Create an Instance of UpsertOpportunities and POST it
        UpsertOpportunities upsertOpps = new UpsertOpportunities(auth)
                                .setAction("createOnly")
                                .addOpportunities(opp1, opp2);
        JsonObject oppsResult = upsertOpps.postData();
        System.out.println(oppsResult);

        //Create Some opp roles now
        JsonObject opp1Role = new JsonObject()
           .add("role", "Captain")
           .add("externalopportunityid", opp1.get("externalopportunityid").asString())
           .add("leadId", 318794);
        JsonObject opp2Role = new JsonObject()
           .add("role", "Commander")
           .add("externalopportunityid", opp2.get("externalopportunityid").asString())
           .add("leadId", 318795);

        //Create an Instance of UpsertOpportunityRoles and POST it
        UpsertOpportunityRoles upsertRoles = new UpsertOpportunityRoles(auth)
           .setAction("createOnly")
           .addRoles(opp1Role, opp2Role);
        JsonObject rolesResult = upsertRoles.postData();
        System.out.println(rolesResult);

    }
}
```

Puede ver la secuencia de creación de compañías, oportunidades y roles. Ahora, todos están listos para sincronizar los datos de su empresa y oportunidad con Marketo.

Publicado el _2015-08-07_ por _Kenny_

## Envío de correos electrónicos transaccionales con la API de REST de Marketo: parte 2, contenido personalizado

Esta semana estamos viendo cómo pasar contenido dinámico a nuestros correos electrónicos a través de la llamada de API de solicitud de campaña. Solicitar campaña no solo permite activar correos electrónicos de forma externa, sino que también puede reemplazar el contenido de [Mis tokens](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/core-marketo-concepts/programs/tokens/understanding-my-tokens-in-a-program) dentro de un correo electrónico. Mis tokens son contenido reutilizable que se puede personalizar en el nivel de programa o de carpeta de marketing. También pueden existir como marcadores de posición para reemplazarlos a través de la llamada de campaña de solicitud.

### Creación del correo electrónico

Para personalizar nuestro contenido, primero debemos configurar un [programa](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/core-marketo-concepts/programs/creating-programs/create-a-program) y un [correo electrónico](https://experienceleague.adobe.com/es/docs/marketo/using/home) en Marketo. Para generar nuestro contenido personalizado, necesitamos crear tokens dentro del programa y luego colocarlos en el correo electrónico que vamos a enviar. Para simplificar, en este ejemplo utilizamos un solo token, pero puede reemplazar cualquier número de tokens en un mensaje de correo electrónico, en el formulario de correo electrónico, en el nombre del remitente, en la respuesta a o en cualquier parte del contenido del correo electrónico. Así que vamos a crear un texto enriquecido token para el reemplazo y llamarlo &quot;bodyReplacement&quot;. El texto enriquecido nos permite reemplazar cualquier contenido del token con HTML arbitrario que queramos introducir. Los tokens no se pueden guardar cuando están vacíos, así que adelante e inserte aquí algún texto de marcador de posición. Ahora necesitamos insertar nuestro token en el correo electrónico: ahora se puede acceder a este token para su reemplazo mediante una llamada de campaña de solicitud. Este token puede ser tan simple como una sola línea de texto que debe reemplazarse por correo electrónico, o puede incluir casi todo el diseño del correo electrónico.

### El código

Estamos ampliando el código de la semana pasada para pasar tokens personalizados a nuestra llamada de campaña de solicitud.

```java
package dev.marketo.blog_request_campaign;

import com.eclipsesource.json.JsonArray;

public class App
{
    public static void main( String[] args )
    {
     //Create an instance of Auth so that we can authenticate with our Marketo instance
        Auth auth = new Auth("Client ID - CHANGE ME", "Client Secret - CHANGE ME", "Host - CHANGE ME");

        //Create and parameterize an instance of Leads
        Leads leadsRequest = new Leads(auth).setFilterType("email").addFilterValue("requestCampaign.test@marketo.com");

        //get the inner results array of the response
        JsonArray leadsResult = leadsRequest.getData().get("result").asArray();

        //get the id of the record indexed at 0
        int lead = leadsResult.get(0).asObject().get("id").asInt();

        //Set the ID of our campaign from Marketo
        int campaignId = 1578;
        RequestCampaign rc = new RequestCampaign(auth, campaignId).addLead(lead);

        //Create the content of the token here, and add it to the request
        String bodyReplacement = "<div class="replacedContent"><p>This content has been replaced</p></div>";
        rc.addToken("{{my.bodyReplacement}}", bodyReplacement);
        rc.postData();
    }
}
```

Esta vez vamos a crear el contenido de nuestro token en la variable bodyReplacement y, a continuación, utilizar el método addToken para agregarlo a la solicitud. addToken toma una clave y un valor y, a continuación, crea una representación de JsonObject y la añade a la matriz de tokens interna. A continuación, esto se serializa durante el método postData y crea un cuerpo con este aspecto:

`{"input":{"leads":[{"id":1}],"tokens":[{"name":"{{my.bodyReplacement}}","value":"<div class="replacedContent"><p>This content has been replaced</p></div>"}]}}`

En conjunto, la salida de la consola tiene este aspecto:

```bash
Token is empty or expired. Trying new authentication
Trying to authenticate with&nbsp;...
Got Authentication Response: {"access_token":"19d51b9a-ff60-4222-bbd5-be8b206f1d40:st","token_type":"bearer","expires_in":3565,"scope":"<apiuser@mktosupport.com>"}
Executing RequestCampaign call
Endpoint: .../rest/v1/campaigns/1578/trigger.json?access_token=19d51b9a-ff60-4222-bbd5-be8b206f1d40:st
Request Body:
{"input":{"leads":[{"id":1}],"tokens":[{"name":"{{my.bodyReplacement}}","value":"<div class="replacedContent"><p>This content has been replaced</p></div>"}]}}
Result:
{"requestId":"1e8d#14eadc5143d","result":[{"id":1578}],"success":true}
```

### Ajuste

Este método se puede ampliar de muchas maneras, cambiando el contenido de los correos electrónicos en secciones de diseño individuales o fuera de los correos electrónicos, lo que permite pasar valores personalizados a tareas o momentos interesantes. Se puede personalizar utilizando este método en cualquier lugar donde se pueda utilizar un token desde un programa. También hay una funcionalidad similar disponible con la llamada [Programar campaña](https://developer.adobe.com/marketo-apis/api/mapi/#operation/scheduleCampaignUsingPOST) que le permitirá procesar tokens en toda una campaña por lotes. No se pueden personalizar por posible cliente, pero son muy útiles para personalizar el contenido en un amplio conjunto de posibles clientes.

Publicado el _2015-07-24_ por _Kenny_

## Actualización de datos de posibles clientes en Marketo mediante la API

Hace más de un año publicamos Actualizando la información de clientes y clientes potenciales en Marketo mediante la API. Esa publicación presentaba un ejemplo de código que se podía ejecutar de forma recurrente para sondear un sistema externo en busca de actualizaciones. La idea era recuperar los datos externos e insertarlos en Marketo para actualizar la información de posibles clientes. El ejemplo de código presentado utilizaba nuestra API de SOAP. Punto final REST para lograr el mismo objetivo. **Entrada de programa** Es probable que necesite transformar los datos del sistema externo en un formato consumible por las API de REST de Marketo. Dado que estamos utilizando la API de creación/actualización de posibles clientes, los datos deben tener el formato JSON, que se envía en el cuerpo de la solicitud. Esto se explica mejor mediante un ejemplo. En el siguiente código de muestra de Java, hemos colocado datos de posibles clientes ficticios en una matriz de cadenas. Cada cadena con los siguientes datos para cada posible cliente: nombre, apellidos, dirección de correo electrónico, cargo.

```java
private static String externalLeadData[] = {
        "Henry, Adams, <henry@superstar.com>, Director of Demand Generation",
        "Suzie, Smith, <ssmith@gmail.com>, VP Marketing"
};
```

El código de ejemplo transforma la matriz en el bloque JSON siguiente.

```json
{
"input":
    [
        {"firstName":"Henry", "lastName":"Adams", "email":"<henry@superstar.com>", "title":"Director of Demand Generation"},
        {"firstName":"Suzie", "lastName":"Smith", "email":"<ssmith@gmail.com>", "title":"VP Marketing"}
    ]
}
```

Cada elemento de matriz de &quot;entrada&quot; corresponde a un posible cliente individual en Marketo. Los elementos de matriz son objetos JSON que contienen uno o más nombres de campo de posible cliente de Marketo y sus valores respectivos. Los nombres de campo que decida especificar (en este caso firstName, lastName, email y title) deben coincidir con el nombre de API de REST definido para la suscripción de Marketo. Puede encontrar los nombres de la API de REST en la sección de administración de campos del panel de administración de Marketo exportando los nombres de los campos. Los nombres de campo se exportan a un archivo de Excel como se muestra a continuación. También puede encontrar nombres de campo mediante programación llamando a la API [Describir posible cliente](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeUsingGET_2). Por ejemplo, este es un fragmento de respuesta Describir que contiene el nombre de la API de REST para el campo Nombre.

```json
{
    "id": 29,
    "displayName": "First Name",
    "dataType": "string",
    "length": 255,
    "soap": {
        "name": "FirstName",
        "readOnly": false
    },
    "rest": {
        "name": "firstName",
        "readOnly": false
    }
},
```

**Lógica de programa** Una vez que la carga útil tenga el formato adecuado, estaremos listos para llamar a Crear/Actualizar posibles clientes. Tenga en cuenta que en este ejemplo no se especifica ninguno de los parámetros opcionales. El comportamiento predeterminado es crear o actualizar registros de posibles clientes (actualización), utilizar el correo electrónico con fines de anulación de duplicación y utilizar el procesamiento sincrónico. Si la llamada a Crear/Actualizar posibles clientes se realiza correctamente, el cuerpo de la respuesta contiene una matriz de &quot;resultados&quot; con el ID del posible cliente de Marketo y el estado de la operación Crear/actualizar. Según el parámetro &quot;action&quot; pasado en la solicitud, el estado puede ser &quot;updated&quot;, &quot;created&quot; o &quot;skipped&quot;. Continuando con nuestro ejemplo, supongamos que la primera pista no existió (Henry Adams) y la segunda pista sí existió (Suzie Smith). Una respuesta similar a la siguiente indicaría que se creó el primer posible cliente y se actualizó el segundo.

```json
{
    "success":true,
    "requestId":"118f8#14f1a0b82fc",
    "result":[
        {
            "id":318798,
            "status":"created"
        },
        {
            "id":318797,
            "status":"updated"
        }
    ]
}
```

Eso es todo por ahora. ¡Feliz codificación! **Código de programa**

```java
package com.marketo;

// minimal-json library (<https://github.com/ralfstx/minimal-json>)
import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;

import javax.net.ssl.HttpsURLConnection;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStreamWriter;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.\*;

public class CreateUpdateLeads {
    //
    // Define Marketo REST API access credentials: Account Id, Client Id, Client Secret.  For example:
    //    public static String CUSTOM_SERVICE_DATA[] =
    //      {"111-AAA-222", "2f4a4435-f6fa-4bd9-3248-098754982345", "asdf6IVE9h4Jjcl59cOMAKFSk78ut12W"};
    //
    private static final String CUSTOM_SERVICE_DATA[] =
            { INSERT YOUR CUSTOM SERVICE DATA HERE };

    //
    // Mock up data that could be read from an external data source.
    // An array of CSV strings the form:
    //   "firstName, lastName, email, title"
    private static String externalLeadData[] = {
        "Henry, Adams, henry@superstar.com, Director of Demand Generation",
        "Suzie, Smith, ssmith@gmail.com, VP Marketing"
    };

    public static void main(String[] args) {
        // Compose base URL
        String baseUrl = String.format("https://%s.mktorest.com",
                CUSTOM_SERVICE_DATA[0]);

        // Compose Identity URL
        String identityUrl = String.format("%s/identity/oauth/token?grant_type=%s&client_id=%s&client_secret=%s",
                baseUrl, "client_credentials", CUSTOM_SERVICE_DATA[1], CUSTOM_SERVICE_DATA[2]);

        // Call Identity API
        JsonObject identityObj = JsonObject.readFrom(httpRequest("GET", identityUrl, null));
        String accessToken = identityObj.get("access_token").asString();

        // Compose URL for Create/Update Leads
        String createUpdateLeadsUrl = String.format("%s/rest/v1/leads.json?access_token=%s", baseUrl, accessToken);

        // Convert String array into JsonArray to pass as part of request body
        JsonArray input = convertExternalLeadDataToJson(externalLeadData);

        // Build request body JSON
        JsonObject requestObj = new JsonObject();
        requestObj.add("input", input);

        // Call Create/Update Lead API
        JsonArray result = new JsonArray();
        JsonObject leadObj = JsonObject.readFrom(httpRequest("POST", createUpdateLeadsUrl, requestObj.toString()));
        if (leadObj.get("success").asBoolean()) {
            if (leadObj.get("result") != null) {
                result = leadObj.get("result").asArray();
            }
        }

        // Print out results object
        System.out.println(result);
        System.exit(0);
    }

    // Convert array of CSV formatted Strings into an array of JSON objects
    private static JsonArray convertExternalLeadDataToJson(String input[]) {
        JsonArray output = new JsonArray();

        // Loop through each CSV row in array
        for (int i = 0; i < input.length; i++) {
            // Split CSV row into separate fields
            List items = Arrays.asList(input[i].split(","));

            // Add fields to JSON object
            JsonObject lead = new JsonObject();
            lead.add("firstName", items.get(0));
            lead.add("lastName", items.get(1));
            lead.add("email", items.get(2));
            lead.add("title", items.get(3));
            output.add(lead);
        }
        return output;
    }

    // Perform HTTP request
    private static String httpRequest(String method, String endpoint, String body) {
        String data = "";
        try {
            URL url = new URL(endpoint);
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
            urlConn.setDoOutput(true);
            urlConn.setRequestMethod(method);
            switch (method) {
                case "GET":
                    break;
                case "POST":
                    urlConn.setRequestProperty("Content-type", "application/json");
                    urlConn.setRequestProperty("accept", "text/json");
                    OutputStreamWriter wr = new OutputStreamWriter(urlConn.getOutputStream());
                    wr.write(body);
                    wr.flush();
                    break;
                default:
                    System.out.println("Error: Invalid method.");
                    return data;
            }
            int responseCode = urlConn.getResponseCode();
            if (responseCode == 200) {
                InputStream inStream = urlConn.getInputStream();
                data = convertStreamToString(inStream);
            } else {
                System.out.println(responseCode);
                data = "Status:" + responseCode;
            }
        } catch (MalformedURLException e) {
            System.out.println("URL not valid.");
        } catch (IOException e) {
            System.out.println("IOException: " + e.getMessage());
            e.printStackTrace();
        }
        return data;
    }

    private static String convertStreamToString(InputStream inputStream) {
        try {
            return new Scanner(inputStream).useDelimiter("A").next();
        } catch (NoSuchElementException e) {
            return "";
        }
    }
}
```

Publicado el _2015-08-14_ por _David_

## Añadir datos del vendedor a Marketo

Con las nuevas API SalesPerson, puede asociar libremente posibles clientes de Marketo con registros SalesPerson en instancias sin una integración nativa de CRM. Esto permite el uso de {{lead.Lead Owner Email Address}} y campos y tokens relacionados dentro de Marketo.

### Creación de registros de SalesPerson

Para asociar posibles clientes con registros de SalesPerson, primero debemos introducir nuestros registros de SalesPerson en Marketo. Esta es una clase de ejemplo en PHP:

```php
<?php

class SalesPerson{
 private $auth;//auth object
 private $action;// string designating request action, createOnly, updateOnly, createOrUpdate
 private $dedupeBy;//dedupeFields or idField
 private $input;//array of salesperson objects for input

 //takes an Auth object as the first argument
 public function _construct($auth, $input){
  $this->auth = $auth;
  $this->input = $input;
 }

 //constructs the json request body
 private function bodyBuilder(){
  $body = new stdClass();
  $body->input = $this->input;
  if (isset($this->action)){
   $body->action = $this->action;
  }
  if (isset($this->dedupeBy)){
   $body->dedupeBy = $this->dedupeBy;
  }
  return json_encode($body);
 }
 //submits the request to Marketo and returns the response as a string
 public function postData(){
  $url = $this->auth->getHost() . "/rest/v1/salespersons.json?access_token=" . $this->auth->getToken();
  $ch = curl_init($url);
  $requestBody = $this->bodyBuilder();
  curl_setopt($ch,  CURLOPT_RETURNTRANSFER, 1);
  curl_setopt($ch, CURLOPT_HTTPHEADER, array('accept: application/json','Content-Type: application/json'));
  curl_setopt($ch, CURLOPT_POST, 1);
  curl_setopt($ch, CURLOPT_POSTFIELDS, $requestBody);
  curl_getinfo($ch);
  $response = curl_exec($ch);
  curl_close($ch);
  return $response;
 }
 //getters and setters
 public function setAction($action){
  $this->action = $action;
 }
 public function getAction($action){
  return $this->action;
 }
 public function setDedupeBy($dedupeBy){
  $this->dedupeBy = $dedupeBy;
 }
 public function getDedupeBy($dedupeBy){
  return $this->dedupeBy;
 }
 public function addSalesPersons($input){
  if($this->input != null){
   if (is_array($input)){
    foreach($input as $item){
     array_push($this->input, $item);
    }
   }else{
    array_push($this->input, $input);
   }
  }else{
   $this->input = $input;
  }
 }
 public function getInput(){
  return $this->input;
 }

}
```

En la clase anterior, la variable $input es una matriz de objetos stdClass con posibles miembros:

* externalSalesPersonId (solo válido al crear)
* id(solo como clave en modo updateOnly)
* correo electrónico
* fax
* firstName
* lastName
* mobilePhone
* teléfono
* title

Se puede obtener información más detallada sobre los tipos y las longitudes de los campos a través de la llamada Describir al vendedor.

### Sincronización de posibles clientes

Esta es una clase de ejemplo rápido para sincronizar los posibles clientes que necesitamos:

```php
<?php

class Leads{
 private $auth;//auth object
 private $action;// string designating request action, createOnly, updateOnly, createOrUpdate
 private $lookupField;//dedupeFields or idField
 private $input;//array of salesperson objects for input
 private $asyncProcessing;//specify whether input should be processed asynchronously
 private $partitionName;//partition name for update if requires

 //takes an Auth object as the first argument
 public function _construct($auth, $input){
  $this->auth = $auth;
  $this->input = $input;
 }

 //constructs the json request body
 private function bodyBuilder(){
  $body = new stdClass();
  $body->input = $this->input;
  if (isset($this->action)){
   $body->action = $this->action;
  }
  if (isset($this->lookupField)){
   $body->lookupField = $this->lookupField;
  }
  return json_encode($body);
 }
 //submits the request to Marketo and returns the response as a string
 public function postData(){
  $url = $this->auth->getHost() . "/rest/v1/leads.json?access_token=" . $this->auth->getToken();
  $ch = curl_init($url);
  $requestBody = $this->bodyBuilder();
  curl_setopt($ch,  CURLOPT_RETURNTRANSFER, 1);
  curl_setopt($ch, CURLOPT_HTTPHEADER, array('accept: application/json','Content-Type: application/json'));
  curl_setopt($ch, CURLOPT_POST, 1);
  curl_setopt($ch, CURLOPT_POSTFIELDS, $requestBody);
  curl_getinfo($ch);
  $response = curl_exec($ch);
  curl_close($ch);
  return $response;
 }
 //getters and setters
 public function setAction($action){
  $this->action = $action;
 }
 public function getAction(){
  return $this->action;
 }
 public function setDedupeBy($lookupField){
  $this->dedupeBy = $lookupField;
 }
 public function getDedupeBy(){
  return $this->dedupeBy;
 }
 public function addLeads($input){
  if($this->input != null){
   if (is_array($input)){
    foreach($input as $item){
     array_push($this->input, $item);
    }
   }else{
    array_push($this->input, $input);
   }
  }else if (is_array($input)){
   $this->input = $input;
  }else{
   $this->input = [$input];
  }
 }
 public function getInput(){
  return $this->input;
 }
 public function setAsyncProcessing($asyncProcessing){
  $this->asyncProcessing = $asyncProcessing;
 }
 public function getAsyncProcessing() {
  return $this->asyncProcessing;
 }
 public function setPartitionName($partitionName){
  $this->partitionName = $partitionName;
 }
 public function getPartitionName() {
  return $this->partitionName;
 }

}
```

### Procesar

Este es un ejemplo de creación de dos registros de vendedor y de asociación con dos registros de posibles clientes:

```php
<?php

require 'Auth.php';
require 'SalesPerson.php';
require 'Leads.php';

//Create an Auth object for authentication
$auth = new Auth("https://284-RPR-133.mktorest.com", "7f99bd49-0e0e-4715-a72a-0c9319f75552", "O5lZHhrQDfDKRhulY89IU42PfdhRSe6m");

//Compose new SalesPerson Records
$sales1 = new stdClass();
$sales1->externalSalesPersonId = "SalesPerson 1";
$sales1->email = "sales1@example.com";
$sales1->firstName = "Jane";
$sales1->lastName = "Doe";

$sales2 = new stdClass();
$sales2->externalSalesPersonId = "SalesPerson 2";
$sales2->email = "sales2@example.com";
$sales2->firstName = "John";
$sales2->lastName = "Doe";

//Compose lead records
$lead1 = new stdClass();
$lead1->email = "marketoDev@example.com";
//Add the external id of the desired salesperson
$lead1->externalSalesPersonId = $sales1->externalSalesPersonId;

$lead2 = new stdClass();
$lead2->email = "devBlog@example.com";
$lead2->externalSalesPersonId = $sales2->externalSalesPersonId;

//Create a new instance of SalesPerson to submit records
$salesPerson = new SalesPerson($auth, [$sales1, $sales2]);
$spResponse = $salesPerson->postData();
print_r($spResponse . "rn");
$json = json_decode($spResponse);

//Check for success on synching SalesPersons
if ($json->success){
 $leads = new Leads($auth, [$lead1, $lead2]);
 $leadsResponse = $leads->postData();
 print_r($leadsResponse . "rn");
}else{
 print_r("SalesPerson request failed with errors:rn");
 foreach ($json->errors as $error){
  print_r("code: " . $error->code . ", message: " . $error->message . "rn");
 }
}
```

Para nuestras clases de ejemplo, estamos creando objetos stdClass para representar nuestros registros SalesPerson y Lead que necesitan sincronizarse, con cada campo deseado añadido como miembro. Después de la ejecución de este código, los posibles clientes <marketoDev@example.com> y <devBlog@example.com> tendrán rellenados los campos Correo electrónico del propietario del posible cliente, Nombre y Apellidos del propietario del posible cliente, lo que les permitirá utilizar los tokens relevantes para esos campos y filtrarse mediante los filtros de lista inteligente relevantes.

### Clase de autenticación

```php
<?php

class Auth{
 private $host;//host of the target Marketo instance
 private $clientId;//client Id
 private $clientSecret;//client secret
 private $token;//access_token
 private $expiry;

 function _construct($host, $clientId, $clientSecret){
  $this->host = $host;
  $this->clientId = $clientId;
  $this->clientSecret = $clientSecret;
 }
 public function getToken(){
  if (!isset($this->token) || $this->expiry > time()){
   $data = $this->getData();
   $this->expiry = time() + $data->expires_in;
   $this->token = $data->access_token;
   return $this->token;
  }else{
   return $this->token;
  }
 }
 private function getData(){
  $url = $this->host . "/identity/oauth/token?grant_type=client_credentials&client_id=" . $this->clientId . "&client_secret="
     . $this->clientSecret;
  $ch = curl_init($url);
  curl_setopt($ch,  CURLOPT_RETURNTRANSFER, 1);
  curl_setopt($ch, CURLOPT_HTTPHEADER, array('accept: application/json','Content-Type: application/json'));
  $response = curl_exec($ch);
  $json = json_decode($response);
  curl_close($ch);
  return $json;
 }
 public function getHost(){
  return $this->host;
 }
}
```

Publicado el _2015-08-21_ por _Kenny_

## Prácticas recomendadas para usuarios de API y servicios personalizados

Las API de REST de Marketo utilizan servicios personalizados para la autenticación y cada uno de estos servicios es propiedad de un usuario de Marketo solo de API. Las capacidades de cada servicio personalizado están determinadas por los permisos de cada función asignada a ese usuario. La asignación de usuarios individuales y servicios personalizados a sus integraciones le ofrece varias ventajas:

* Puede ajustar los [permisos](/help/rest-api/custom-services.md) otorgados a cada servicio individual a través de la función asignada a su usuario.
* Puede deshabilitar servicios web individuales para que no realicen llamadas a su instancia eliminando el servicio personalizado correspondiente, sin deshabilitar otros.
* Los informes sobre el uso de llamadas de API se desglosarán por usuario, lo que le permite identificar un uso alto y anormal
* Es más fácil determinar a qué datos se está dando acceso a cada servicio web
* Las instancias habilitadas para Workspace pueden restringir el acceso a unidades de negocio específicas mediante la asignación de funciones únicamente a espacios de trabajo accesibles

### Uso de API

Cada uno de los usuarios de la API se recoge de forma individual en el informe de uso de la API, por lo que la división de los servicios web por usuario permite contabilizar fácilmente el uso de cada una de las integraciones. Si el número de llamadas de API a su instancia supera el límite y provoca que las llamadas posteriores fallen, el uso de esta práctica le permite contabilizar el volumen de cada uno de sus servicios y evaluar cómo resolver el problema. Para ver su uso, vaya a Administración -> Servicios web y haga clic en el número de llamadas en los últimos 7 días.

### Desactivación de un servicio

Si una integración tiene efectos no deseados, puede resultar tedioso y difícil de deshabilitar si no se ha asignado a cada una un servicio personalizado individual. Desglosarlos uno por uno facilita tanto como eliminar el servicio infractor en su Administrador -> Punto de inicio.

### Administración de Workspace

Para las suscripciones de Marketo Enterprise, es común que un servicio solo necesite acceso a un único espacio de trabajo, y esto se puede [aplicar mediante la asignación de funciones al usuario de API](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/administration/workspaces-and-person-partitions/allow-user-access-to-a-workspace). Cada función de usuario se puede asignar de forma global o por espacio de trabajo, por lo que el acceso se puede restringir en los espacios de trabajo siempre que sea adecuado, siempre que se proporcione el conjunto de permisos más mínimo posible.

Publicado el _2015-08-28_ por _Kenny_

## Cómo especificar particiones de posibles clientes mediante la API de REST

**Partición de posibles clientes** Las particiones de posibles clientes de Marketo son una forma cómoda de aislar los posibles clientes. Las particiones pueden permitir que diferentes grupos de marketing de su organización compartan una sola instancia de Marketo. Para obtener más información, vea [Comprender los espacios de trabajo y las particiones de posibles clientes](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/administration/workspaces-and-person-partitions/understanding-workspaces-and-person-partitions). Supongamos que está utilizando particiones de posibles clientes y creando posibles clientes mediante programación mediante la API de REST de Marketo. ¿Cómo se asegura de que los posibles clientes que crea terminen en la partición correcta? ¡Este post te muestra cómo hacerlo! Para este ejemplo, utilizaremos los espacios de trabajo y las particiones para aislar nuestros posibles clientes en función de la ubicación geográfica.

En primer lugar, definiremos un espacio de trabajo llamado &quot;País&quot;. A continuación, creamos dos particiones dentro de ese espacio de trabajo llamadas &quot;México&quot; y &quot;Canadá&quot;.  **Crear posible cliente en la partición** Supongamos que ahora queremos crear dos posibles clientes en la partición &quot;México&quot;. Para crear posibles clientes, se llama a. Para especificar la partición, debemos incluir el atributo &quot;partitionName&quot; en el cuerpo de la solicitud. ¿Cómo sabemos qué utilizar para el valor partitionName? Podemos recuperar una lista de valores de nombre de partición válidos para nuestra instancia llamando a la API [Obtener particiones de posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeProgramMemberUsingGET) de la siguiente manera:

`GET /rest/v1/leads/partitions.json`

```json
{
    "requestId": "20ae#14f9a1a5a30",
    "result": [
        {
            "id": 1,
            "name": "Default",
            "description": "Initial system lead partition"
        },
        {
            "id": 2,
            "name": "Mexico",
            "description": "Leads that live in Mexico"
        },
        {
            "id": 3,
            "name": "Canada",
            "description": "Leads that live in Canada"
        }
    ],
    "success": true
}
```

En este caso, el valor correcto es &quot;México&quot;, por lo que se pasa a Crear/Actualizar posibles clientes de la siguiente manera:

`POST /rest/v1/leads.json`

```json
{
    "input": [
        {"email":"enrique.pena-nieto@gob.mx"},
        {"email":"angelica.rivera@gob.mx"}
    ],
    "action":"createOrUpdate",
    "partitionName":"Mexico"
}
```

Estos son los posibles clientes recién creados en Marketo.  **Actualizar posible cliente en la partición** Para actualizar los posibles clientes existentes en una partición, simplemente llamamos a Crear/actualizar posibles clientes y especificamos partitionName igual que antes. Para esta actualización, asignaremos nombre, apellido y título a los posibles clientes que hemos creado anteriormente.

`POST /rest/v1/leads.json`

```json
{
"input": [
        {"email":"enrique.pena-nieto@gob.mx", "firstName":"Enrique", "lastName":"Pena Neito", "title": "El Presidente"},
        {"email":"angelica.rivera@gob.mx", "firstName":"Angelica", "lastName": "Rivera", "title": "Primera Dama"}
    ],
    "action":"createOrUpdate",
    "partitionName":"Mexico"
}
```

Estos son nuestros posibles clientes recién actualizados en Marketo.
**Identificar partición para un posible cliente** ¿Cómo sabemos en qué partición se encuentra un posible cliente? Para esto usamos la API [Obtener posible cliente por id](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET) y especificamos &quot;leadPartitionId&quot; en el parámetro de consulta &quot;fields&quot;. En este caso, recuperaremos la información del ID de posible cliente 318816 que hemos creado anteriormente.

`GET /rest/v1/lead/318816.json?fields=leadPartitionId,email,firstName,lastName,title`

```json
{
    "requestId": "5c57#14f9a495b1f",
    "result": [
        {
            "id": 318816,
            "lastName": "Pena Neito",
            "title": "El Presidente",
            "email": "enrique.pena-nieto@gob.mx",
            "firstName": "Enrique",
            "leadPartitionId": 2
        }
    ],
    "success": true
}
```

Tenga en cuenta que recuperamos leadPartitionId en lugar de partitionName. Para obtener el partitionName, debemos volver a visitar la salida de la API Obtener particiones de posibles clientes anterior.

```json
{
    "id": 2,
    "name": "Mexico",
    "description": "Leads that live in Mexico"
},
```

En este caso, un valor leadPartitionId de 2 se asigna al partitionName de &quot;México&quot;. Eso es todo por ahora. ¡Feliz particionado!

Publicado el _04-09-2015_ por _David_

## Comparación de los campos de puntuación en los scripts de correo electrónico de Marketo

**Nota:** Esta es una publicación invitada de Cathal Moran. Cathal es consultor de soluciones, trabaja en la oficina EMEA de Marketo en Dublín, Irlanda.

### Comparación de campos de puntuación

Muchos clientes de Marketo, especialmente los que se centran en la venta cruzada, tienen varios campos de puntuación, que a menudo se utilizan para medir el interés de un posible cliente en un producto o área en particular. Imaginen que vendo manzanas y plátanos, si un plomo tiene una puntuación de 50 para las manzanas y 10 para los plátanos, entonces está claro dónde está la preferencia, ¿no sería bueno si mi contenido reflejara esta preferencia? Los scripts de correo electrónico se pueden utilizar para comparar puntuaciones y personalizar el contenido de un correo electrónico, según la puntuación más alta (o más baja) para ese posible cliente en particular que recibe el correo electrónico.

### Como quiero comparar 2 números, necesito convertir los valores de mis campos a enteros

```
#set ($score1 = $math.toInteger(${lead.Apple_Score}))
#set ($score2 = $math.toInteger(${lead.Banana_Score}))
##check if the lead score is greater than feature score
#if($score1 >= $score2)
                ##if Apple score is greater
                #set($Interest = "Special offer on Apples")
##check is the feature score is higher
#elseif($score2 >= $score1)
                ##if Feature score is greater
                #set($Interest = "Special offer on Bananas")
#else
                ##otherwise
                #set($Interest = "Special offer on Fruit")
#end
##display the Interest as content
${Interest}
```

En el ejemplo anterior, solo personalizo el texto, pero podría mostrar fácilmente una imagen diferente en función de la puntuación más alta.

Publicado el _2015-09-14_ por _Kenny_

## Realizar un envío de formulario Marketo en segundo plano

Cuando su organización tiene muchas plataformas diferentes para alojar contenido web y datos de clientes, es bastante común necesitar envíos paralelos de un formulario para que los datos resultantes se puedan recopilar en plataformas independientes. Existen varias estrategias para hacerlo, pero la mejor suele ser la más sencilla: Usar la API de Forms 2 para enviar un formulario Marketo oculto. Esto funcionará con cualquier nuevo formulario de Marketo, pero lo ideal es crear un formulario vacío para este, que no tenga campos de. Esto garantizará que el formulario no cargue más datos de los necesarios, ya que no necesitamos procesar nada. Ahora simplemente toma el [código incrustado](https://experienceleague.adobe.com/es/docs/marketo/using/home) de tu formulario y agrégalo al cuerpo de tu página deseada, haciendo una pequeña modificación. El código incrustado incluye un elemento de formulario como este:

`<form id="mktoForm_1068"></form>`

Desea agregar &#39;style=&quot;display:none&quot;&#39; al elemento para que no esté visible, de esta manera:

`<form id="mktoForm_1068" style="display:none"></form>`

Una vez incrustado y oculto el formulario, el código para enviarlo es realmente sencillo:

```javascript
var myForm = MktoForms2.allForms()[0];
myForm.addHiddenFields({
 //These are the values which will be submitted to Marketo
 "Email":"test@example.com",
 "FirstName":"John",
 "LastName":"Doe"
 });
myForm.submit();
```

Forms enviado de esta manera se comportará exactamente igual que si el posible cliente hubiera rellenado y enviado un formulario visible. La activación del envío variará entre implementaciones, ya que cada una tiene una acción diferente para solicitarla, pero puede hacer que se produzca básicamente en cualquier acción. Lo importante es configurar correctamente los campos y los valores. Asegúrese de usar el nombre de API de SOAP de sus campos, que puede encontrar con [Exportar nombres de campo](/help/rest-api/list-of-standard-fields.md) para garantizar el envío correcto de los valores.

Migración desde Munchkin Associate Lead

El envío del formulario de fondo es uno de los métodos de reemplazo recomendados para el cliente potencial asociado de Munchkin. La página de HTML de ejemplo siguiente muestra la migración en un nivel superior, reutilizando los mismos valores que se envían a Associate Lead.

```html
<html>
<head>
    <!--
      Munchkin Code
      Replace with your own instance code
    -->
    <script type="text/javascript">
        (function() {
          var didInit = false;
          function initMunchkin() {
            if(didInit === false) {
              didInit = true;
              Munchkin.init('CHANGE ME');
            }
          }
          var s = document.createElement('script');
          s.type = 'text/javascript';
          s.async = true;
          s.src = '//munchkin.marketo.net/munchkin-beta.js';
          s.onreadystatechange = function() {
            if (this.readyState == 'complete' || this.readyState == 'loaded') {
              initMunchkin();
            }
          };
          s.onload = initMunchkin;
          document.getElementsByTagName('head')[0].appendChild(s);
        })();
        </script>
</head>

<body>
  <!--
    Start Embed code.
    Pasted from Form Actions -> Embed Code except for addition of 'style="display:none"' to the form tag in order to hide it, and instance-specific codes redacted
    Replace with your own code for testing
  -->
  <script src="CHANGE ME"></script>
  <form id="CHANGE ME" style="display:none"></form>
  <script>MktoForms2.loadForm("CHANGE ME", "CHANGE ME", "CHANGE ME TO AN INTEGER ID");</script>
  <!--End Embed Code-->

  <!--Demo code-->
    <script>
        //The same map which is assembled for the Munchkin submission can be reused for the form submission
        let values = {
            "Email": "email@example.com",
            "FirstName": "Test",
            "LastName": "Record"
        }
        //whenReady lets us apply a callback to all mkto forms on the page
        //the callback function fires whenever a form is completely loaded
        //for most use cases this will just be used to capture a reference to the form for later usage
        //submission is done in line here for demonstration only
        MktoForms2.whenReady(function(form){

            //the addHiddenFields methods lets us add arbitrary fields to the form as well as their values
            form.addHiddenFields(values);

            //submit the form
            form.submit();


        })
    </script>
</body>
</html>
```

Publicado el _2015-09-25_ por _Kenny_

## Excepción y administración de errores de la API de REST de Marketo: Parte 1

La API de REST de Marketo puede devolver una excepción o un error que, por comodidad, solo llamaremos errores a partir de ahora. Los errores pueden producirse en tres niveles diferentes:

* A nivel HTTP, estos errores se indican mediante un código 4xx
* Nivel de respuesta, estos errores se incluyen en la matriz &quot;errors&quot; de la respuesta JSON
* En el nivel de registro, estos errores se incluyen en la matriz &quot;result&quot; de la respuesta JSON y se indican en un registro individual con el campo &quot;status&quot; y la matriz &quot;reason&quot;

### Errores de HTTP

En circunstancias normales de funcionamiento, Marketo solo debe devolver dos errores de estado HTTP, la entidad de solicitud 413 es demasiado grande y la URI de solicitud 414 es demasiado larga. Ambos se pueden recuperar detectando el error, modificando la solicitud y reintentando, pero con las prácticas de codificación inteligente, nunca debe encontrarlos en el comodín. Marketo devolverá 413 si la carga de la solicitud supera 1 MB o 10 MB en el caso de [Importar posible cliente](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads). En la mayoría de los casos, es poco probable que se alcancen estos límites, pero añadir una comprobación al tamaño de la solicitud y mover cualquier registro que haga que se supere el límite a una nueva solicitud debe evitar cualquier circunstancia que lleve a que este error sea devuelto por cualquier extremo. 414 se devolverá cuando el URI de una petición GET exceda de 8 KiB. Para evitarlo, compare la longitud de la cadena de consulta para ver si supera este límite. Si cambia la solicitud a un método POST, escriba la cadena de consulta como el cuerpo de la solicitud con el parámetro adicional &#39;_method=GET&#39;. Esto renuncia a la limitación de URI. Es raro alcanzar este límite en la mayoría de los casos, pero es algo común cuando se recuperan grandes lotes de registros con valores de filtro individuales largos, como un GUID.

### Errores de nivel de respuesta

Los errores de nivel de respuesta están presentes cuando el parámetro &quot;success&quot; de la respuesta se establece en false. Cada entrada en &quot;errors&quot; tiene dos miembros, &quot;code&quot; un número, ya sea 6xx o 7xx, y un &quot;message&quot; que da la razón de texto simple del error. Los códigos 6xx siempre indican que una solicitud ha fallado completamente y no se ha ejecutado. Un ejemplo de esto es un 601, &quot;Token de acceso no válido&quot;, que se puede recuperar volviendo a autenticar y pasando el nuevo token de acceso con la solicitud. Los errores 7xx indican que la solicitud falló, ya sea porque no se devolvieron datos o porque se parametrizó incorrectamente la solicitud, como incluir una fecha no válida o faltar un parámetro requerido.

### Errores de nivel de registro

Los errores de nivel de registro indican que no se pudo completar una operación para un registro individual, pero la solicitud en sí era válida. Estos se producen dentro de un registro individual en la matriz &quot;result&quot; de una respuesta. El campo &quot;estado&quot; de estos registros se &quot;omitirá&quot; y aparecerá una matriz &quot;motivos&quot;. Cada motivo contiene un miembro &quot;code&quot; y un miembro &quot;message&quot;. El código siempre será 1xxx y el mensaje indicará por qué se omitió el registro. Un ejemplo sería cuando una solicitud de Crear/Actualizar posibles clientes tiene &quot;action&quot; establecida en &quot;createOnly&quot; pero ya existe un posible cliente para una de las claves de los registros enviados. Este caso devuelve un código de 1005 y un mensaje de &quot;El posible cliente ya existe&quot;. En las siguientes publicaciones, analizaremos los errores recuperables y algunos ejemplos de cómo gestionarlos dentro de su código.

Publicado el _2015-10-09_ por _Kenny_

## Publicación de invitado: Direccione los conectores SQL a la base de datos de Marketo

**Esta es una publicación invitada de Summit Sarkar of Progress, Inc.**

**Conectores SQL a la base de datos de Marketo** Marketo tiene API bien documentadas para acceder a los datos. Sin embargo, a veces las organizaciones necesitan acceso directo a SQL. Estamos viendo las líneas borrosas en las tiendas de Marketo entre Marketing y TI, lo que está aumentando la demanda de conectividad SQL basada en estándares. Los conectores SQL directos a Marketo están disponibles a través de [Progress DataDirect Cloud](https://www.progress.com/connectors/cloud-data-integration). DataDirect Cloud es un servicio de conectividad de datos que expone los datos de Marketo a través de estándares abiertos del sector para el acceso SQL entre ODBC (&quot;Conectividad abierta de bases de datos&quot;) o JDBC (&quot;Conectividad de bases de datos Java&quot;) y REST con OData (&quot;Protocolo de datos abiertos&quot;). A continuación se muestran algunos casos de uso populares para conectar datos de Marketo de forma predeterminada mediante la nube de DataDirect:

* Detección y visualización de datos (Qlik, Tableau, SAP Lumira)
* Informes empresariales (objetos empresariales SAP, microestrategia, Cognos)
* Integración de datos (SQL Server Integration Services - SSIS, Oracle Data Integrator - ODI, Informatica)
* Federación de datos (SQL Server Linked Server, SAP Hana SDA, Oracle Database Gateway)
* Ad Hoc Query (Microsoft Office, DB Visualizer, Aqua Data Studio)
* Preparación de datos (Alteryx, Trifecta, Paxata)

**¿Cómo funciona el acceso de SQL a Marketo?**

* DataDirect Cloud crea un esquema lógico para los datos expuestos por las API de integración de Marketo.
* La nube de DataDirect procesa solicitudes SQL de clientes ODBC o JDBC ligeros mediante un motor de consultas en tiempo real elástico en los datos recuperados de las API de Marketo.
* La conectividad de DataDirect Cloud es directa y en tiempo real sin duplicación de datos.
* DataDirect Cloud Service abstrae la API de Marketo de modo que los usuarios finales no tienen que preocuparse por los cambios de versión de la API ni por utilizar SOAP en comparación con REST.

DataDirect ha estado creando este estilo de conectividad con fuentes de datos SaaS desde 2006, empezando con el primer controlador ODBC de Salesforce creado sobre sus API de servicio web. **Introducción a la conexión con Marketo:**

1. [Registrarse para iniciar sesión en la nube con DataDirect](https://pacific.progress.com/console/register?productName=d2c&ignoreCookie=true)
1. Haga clic en &quot;Fuentes de datos&quot; y, a continuación, en el botón &quot;+Nuevo Source de datos&quot;
1. Seleccione &quot;Marketo&quot; e introduzca la información de conexión. Puede consultar con su administrador de Marketo o iniciar sesión para encontrar [información de conexión para la integración de SOAP](/help/soap-api/soap-api.md).
1. Haga clic en el botón &quot;Probar conexión&quot;. Tenga en cuenta que hay una pestaña OData para producir OData de Marketo y analizaremos en una publicación de blog futura.
1. Haga clic en &quot;Pruebas SQL&quot; si desea inspeccionar el esquema de Marketo expuesto o emitir consultas SQL básicas desde la interfaz de usuario.
1. Haga clic en &quot;Descargas&quot; a la izquierda y seleccione el controlador JDBC o ODBC de DataDirect Cloud para su aplicación y plataforma que desea instalar.
1. Una vez instalado el controlador ODBC o JDBC de DataDirect Cloud, puede conectar cualquier aplicación basada en estándares a Marketo.

Este es un ejemplo en vídeo de [conexión mediante el cliente ODBC de nube DataDirect](https://www.youtube.com/watch?v=H6PHra56Iig). Estos son otros tutoriales de nube de DataDirect que se aplican a Marketo:

* [Análisis de datos SAP](http://scn.sap.com/community/lumira/blog/2015/08/05/connect-sap-lumira-to-eloqua-marketo-google-analytics)
* [Informes Empresariales De Microestrategia](https://community.microstrategy.com/t5/Tech-Corner/What-MSTR-developers-should-know-about-Cloud-Data-Sources/ba-p/253083)
* [Integración de datos de Oracle](https://www.ateam-oracle.com/post/a-universal-cloud-applications-adapter-for-odi/)

**Retos de I+D al crear conectividad SQL en fuentes de nube como Marketo**

No todas las API de SaaS exponen un idioma de consulta estándar. En estos casos, el equipo de ingeniería examina cada objeto de forma individual. Cada objeto puede exponerse con una API diferente con reglas únicas para invocar, buscar, filtrar, etc. Se requirió un esfuerzo significativo para proporcionar una experiencia estándar en la consulta en todo el modelo de datos.

Gestión de las capacidades de unión completa. En los casos en los que las API de SaaS no admiten un idioma de consulta con capacidad JOIN, el equipo de ingeniería debe realizar esa operación. Esto requiere una traducción de SQL para llamar de forma eficaz a las API de Marketo y devolver la cantidad mínima de datos antes de realizar la unión. Al unir dos objetos muy grandes, la capa de acceso a datos puede consumir recursos considerables en el servidor de aplicaciones o en el escritorio. Por lo tanto, la implementación de la capa de acceso a datos en un servicio en la nube elástico como DataDirect Cloud tiene mucho sentido por dos razones:

1. Rendimiento más rápido y uso de menos recursos de memoria/CPU en el servidor de aplicaciones o el equipo de sobremesa del cliente
1. Aproveche el ancho de banda superior entre DataDirect Cloud y Marketo, donde se intercambian conjuntos de datos unidos previamente.

¿Cómo se gestionan los modelos de datos? ¿Es estático o dinámico? ¿Cómo se detectan y comunican los cambios al cliente? Cada fuente de datos SaaS es diferente y, en el caso de Marketo, es mejor consultar ciertos objetos a través de vistas y otros a través de tablas. El manejo de esta matriz de modelos de datos y objetos en todas las fuentes de SaaS fue sin duda un desafío.

**Referencia de nube de Marketo y DataDirect para desarrolladores:**

* Propiedades de conexión de Marketo ([vínculo a documentos](https://www.progress.com/output/DataDirect/DataDirectCloud/index.html))
* SQL y extensiones compatibles con Marketo ([vínculo a documentos](https://www.progress.com/output/DataDirect/DataDirectCloud/index.html,-hubspot,-and-marketo.html))
* Tablas y vistas de Marketo expuestas ([vínculo a documentos](https://www.progress.com/output/DataDirect/DataDirectCloud/index.html))
* Mensajes de error comunes devueltos por Marketo ([vínculo a documentos](https://www.progress.com/output/DataDirect/DataDirectCloud/index.html))

**Biografía:** Sumit Sarkar es un Jefe de Evangelización de Datos en Progreso, con más de 10 años de experiencia trabajando en el campo de la conectividad de datos. El consultor líder mundial en conectividad de estándares de datos abiertos con datos en la nube, los intereses de Summit incluyen la optimización del rendimiento de la capa de acceso a datos para la que ha desarrollado una tecnología pendiente de patente para su análisis; inteligencia empresarial y almacenamiento de datos para plataformas SaaS; y conectividad de datos para entornos PaaS, con un enfoque en estándares como ODBC, JDBC, ADO.NET y ODATA. Es consultor certificado de IBM para IBM Cognos Business Intelligence y miembro de TDWI. Ha presentado sesiones sobre conectividad de datos en varias conferencias, incluidas Dreamforce, Oracle OpenWorld, Strata Hadoop, MongoDB World y SAP Analytics and Business Objects Conference, entre muchas otras. Twitter: @SAsInSumit LinkedIn: [www.linkedin.com/in/meetsumit](http://www.linkedin.com/in/meetsumit)

Publicado el _2015-12-07_ por _Kenny_

## Creación de un tablero para el uso de la API y los recuentos de errores

Como consumidor de API de Marketo, esta información es útil y debe vigilarse. ¿Qué sucede si se pueden obtener datos de uso históricos para detectar las tendencias a lo largo del tiempo? ¿Qué sucede si puede obtener un resumen de los códigos de error de API para ayudar a medir el estado de la integración? Como socio tecnológico de Marketo, ¿qué sucede si puede obtener datos de uso y error en todas las cuentas de cliente en un solo panel? Este post le ofrecerá un enfoque para responder a las preguntas anteriores. ¡Abróchense el cinturón, allá vamos!
**Trabajo programado para la recuperación de estadísticas** Vamos a crear una aplicación que recupere datos de uso y error usando los extremos [Obtener uso diario](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLast7DaysErrorsUsingGET) y [Obtener errores diarios](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getDailyErrorsUsingGET). La aplicación está diseñada para que se programe su ejecución una vez al día. Cada vez que se ejecuta la aplicación, anexa datos de uso correspondientes a un día a un archivo y datos de error correspondientes a un día a otro archivo. Al principio de cada mes, se crea un nuevo par de archivos. Estos archivos servirán como un registro histórico al que podemos acceder en cualquier momento. Esta es la lógica de la aplicación...

* Lea la información de la cuenta de Marketo (ID de Munchkin y credenciales del cliente) desde un origen externo. Nota: Esta fuente debe ser segura para evitar que otros usuarios accedan a los datos de la cuenta.
* Itere por cada cuenta y...
   * Llame a Obtener uso diario para recuperar los datos de uso de un día
   * Anexe datos de uso diario a un archivo mensual de &quot;uso&quot;
   * Llame a Get Daily Errors para recuperar los datos de errores de un día
   * Anexe datos de errores diarios a un archivo mensual de &quot;errores&quot;

Formato del archivo de salida El formato de los archivos de salida es JSON, que coincide con la matriz de &quot;resultado&quot; devuelta desde las respectivas llamadas de API (Uso y Error). Cada elemento de la matriz &quot;result&quot; es un objeto JSON que contiene datos de un día. Nombres de los archivos de salida Los archivos de salida reciben los siguientes nombres:

`<type>_<yyyy>_<mm>_<account>.json`

Donde,

`<type>` - El tipo de datos (&quot;uso&quot; o &quot;errores&quot;) `<yyyy>` - El año (4 dígitos) `<mm>` - El mes (2 dígitos) `<account>` - El ID de cuenta (ID de Munchkin)

Ejemplos de archivos de salida usage_2015_10_111-AAA-222.json

```json
[
    { "date": "2015-10-15", "total": 0, "users" : [] },
    { "date": "2015-10-16", "total": 9, "users": [ { "userId": "some.body@yahoo.com", "count": 9 } ] },
    { "date": "2015-10-17", "total": 1120, "users": [ { "userId": "some.body@yahoo.com", "count": 200 }, { "userId": "some.body@marketo.com", "count": 200 }, { "userId": "some.body@gmail.com", "count": 720 } ] },
]
```

`errors_2015_10_111-AAA-222.json:`

```json
[
    { "date": "2015-10-15", "total":80, "errors": [ { "errorCode":"1003", "count":80 } ] },
    { "date": "2015-10-16", "total":148, "errors": [ { "errorCode":"612", "count":40 }, { "errorCode":"609", "count":70 }, { "errorCode":"1008", "count":38 } ] },
    { "date": "2015-10-17", "total":73, "errors": [ { "errorCode":"604", "count":1 }, { "errorCode":"609", "count":56 }, { "errorCode":"610", "count":16 } ] },
]
```

El código de esta aplicación se presenta al final de esta publicación (Stats.java). **Servicio web de estadísticas**. Ahora necesitamos una forma de introducir estos datos en nuestro explorador. La propuesta es crear un servicio web para entregar los datos. El servicio web consumirá los archivos de salida de la aplicación y, a continuación, devolverá datos al explorador en un formulario que se pueda presentar fácilmente. Para simplificar y para evitar las restricciones de políticas del mismo origen, aprovecharemos el patrón [JSONP](https://en.wikipedia.org/wiki/JSONP). Esta es la especificación de extremo REST propuesta para el servicio web externo: **URI**: /stats **Método**: GET

**Parámetro**

**Descripción**

**Ejemplo**

mes

Recupere los datos de este mes. Lista de meses separados por comas que se van a incluir (representación de 2 dígitos). De forma predeterminada, todos los meses.

10,11

año

Recupere los datos de este año. Lista de años separados por comas que se van a incluir (representación de 4 dígitos). Predeterminado para todos los años.

2015

account

Recupere datos de esta cuenta (Munchkin id).

111-AAA-222

callback

Nombre de la función con la que ajustar el contenido JSON.

processStats

Solicitud de ejemplo

`GET //localhost:8080/stats?month=10&year=2015&account=111-AAA-222&callback=processStats`

El servicio web lee archivos de &quot;uso&quot; y &quot;error&quot; y los combina y los devuelve en este formato:

```html
**<Name of Callback here>**

<Contents of Usage file here>,

<Contents of Error file here>
```

Esta es una llamada de retorno JSONP con 2 argumentos. El primer argumento es la matriz de &quot;resultados&quot; de uso. El segundo argumento es la matriz &quot;result&quot; de errores. Respuesta de ejemplo

```json
processStats(
    [
        { "date": "2015-10-15", "total": 0, "users" : [] },
        { "date": "2015-10-16", "total": 9, "users": [ { "userId": "some.body@yahoo.com", "count": 9 } ] },
        { "date": "2015-10-17", "total": 1120, "users": [ { "userId": "some.body@yahoo.com", "count": 200 }, { "userId": "some.body@marketo.com", "count": 200 }, { "userId": "some.body@gmail.com", "count": 720 } ] }
    ],
    [
        { "date": "2015-10-15", "total":80, "errors": [ { "errorCode":"1003", "count":80 } ] },
        { "date": "2015-10-16", "total":148, "errors": [ { "errorCode":"612", "count":40 }, { "errorCode":"609", "count":70 }, { "errorCode":"1008", "count":38 } ] },
        { "date": "2015-10-17", "total":73, "errors": [ { "errorCode":"604", "count":1 }, { "errorCode":"609", "count":56 }, { "errorCode":"610", "count":16 } ] },
    ]
);
```

Como puede ver, el servicio web simplemente ha ajustado el contenido de los dos archivos de salida de nuestra aplicación. Hemos creado una respuesta de servicio web ficticio con [Mocky](https://designer.mocky.io). Un ejemplo del servicio web para el simulador es [aquí.](http://www.mocky.io/v2/5627b2f9270000f2226eec63?month=10&year=2015&account=111-AAA-222&callback=processStats) La creación de este servicio web se deja como un ejercicio para el lector : **Página web del panel**. Así que ahora todo lo que necesitamos es una página web que llame a nuestro servicio web y dé formato a los datos. Para utilizar el patrón JSONP, solo necesitamos agregar una etiqueta `<script>` que invoque el servicio web:

`<script src="http: //<hostname>/stats?month=10&year=2015&account=284-RPR-133&callback=processStats"></script>`

Esto insertará el cuerpo de respuesta del servicio web directamente en la página de HTML. A continuación, agregamos la función de llamada de retorno JSONP :

```javascript
function processStats(usage, errors) {
    var cfg = { maxDepth: 5};
    document.write("<h2>Usage</h2>");
    document.body.appendChild(prettyPrint(usage, cfg));
    document.write("<h2>Errors</h2>");
    document.body.appendChild(prettyPrint(errors, cfg));
;
```

Se llama automáticamente a esta función después de la llamada al servicio web. En este ejemplo, llamamos a un &quot;volcado de variables&quot; simple de JavaScript llamado [prettyPrint.js](https://github.com/padolsey-archive/prettyprint.js/tree/master) en cada matriz. La función prettyPrint simplemente produce una tabla HTML utilizando el contenido de la matriz. Esta es una captura de pantalla de las tablas de HTML. ¡Voilà, ese es nuestro tablero! Por supuesto que esto no es muy elegante, pero debería darte una idea de lo que es posible. No hay nada que le impida transformar los datos de la manera que desee para crear sus propias visualizaciones llamativas. A continuación, se muestra la página de HTML (Index.html). Puede ver las tablas anteriores en directo en su explorador, para hacerlo, siga los siguientes pasos:

1. Guardar una copia local de Index.html
1. Guardar una copia local de prettyPrint.js
1. Abra Index.html en el explorador

Así que eso es todo. Esperamos que esta publicación le haya dado algunas ideas sobre cómo monitorizar las estadísticas de la API de Marketo. ¡Feliz codificación! **Stats.java**

```java
package com.marketo;

// minimal-json library (https://github.com/ralfstx/minimal-json)
import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;

import java.io.\*;
import java.lang.reflect.Array;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.\*;

import java.nio.file.Files;
import java.nio.file.Paths;

import javax.net.ssl.HttpsURLConnection;

public class Stats {
    //
    // Define Marketo instance meta data here. Each row contains three elements: Account Id, Client Id, Client Secret.
    //
    // Note: that this information would typically be stored read from an external source (i.e. database)
    //
    // For example:
    //
    // private static String final CUSTOM_SERVICE_DATA[][] = {
    // {"111-AAA-222", "2f4a4435-f6fa-4bd9-3248-098754982345", "asdf6IVE9h4Jjcl59cOMAKFSk78ut12W"},
    // {"222-BBB-333", "5f4a6657-f6fa-4cd9-4356-123083238821", "gfjgfIVE9h4Jjcl59cOMAKFSk78ut12W"},
    // {"444-CCC-444", "9f4a4678-f6fa-4dd9-7735-908713247721", "xzcxvbVE9h4Jjcl59cOMAKFSk78ut12W"}
    // };
    //
    private static final String CUSTOM_SERVICE_DATA[][] = {
    };

    // Output directory for stats files
    private static final String OUTPUT_DIR = "C:stats";

public static void main(String[] args) {

    // Loop through each Marketo instance
    for (int i = 0; i < CUSTOM_SERVICE_DATA.length; i++) {

        // Compose base URL
        String baseUrl = String.format("https://%s.mktorest.com",
            CUSTOM_SERVICE_DATA[i][0]);

        // Compose Identity URL
        String identityUrl = String.format("%s/identity/oauth/token?grant_type=%s&client_id=%s&client_secret=%s",
            baseUrl, "client_credentials", CUSTOM_SERVICE_DATA[i][1], CUSTOM_SERVICE_DATA[i][2]);

        // Call Identity API
        JsonObject identityObj = JsonObject.readFrom(httpRequest("GET", identityUrl, null));
        String accessToken = identityObj.get("access_token").asString();

        // Compose Get Last 7 Days Usage URL
        String usageUrl = String.format("%s/rest/v1/stats/usage/last7days.json?access_token=%s",
            baseUrl, accessToken);

        // Compose Get Last 7 Days Errors URL
        String errorsUrl = String.format("%s/rest/v1/stats/errors/last7days.json?access_token=%s",
            baseUrl, accessToken);

        // Process usage data
        JsonObject usageObj = JsonObject.readFrom(httpRequest("GET", usageUrl, null));
        if (usageObj.get("success").asBoolean()) {
            if (usageObj.get("result") != null) {
                updateFile(usageObj, "usage", CUSTOM_SERVICE_DATA[i][0]);
            }
        }

        // Process errors data
        JsonObject errorsObj = JsonObject.readFrom(httpRequest("GET", errorsUrl, null));
        if (usageObj.get("success").asBoolean()) {
            if (errorsObj.get("result") != null) {
                updateFile(errorsObj, "errors", CUSTOM_SERVICE_DATA[i][0]);
            }
        }
    }
    System.exit(0);
}

// Write yesterday's data to output file
private static void updateFile(JsonObject usageObj, String statsType, String account){

    JsonArray usageResultAry = usageObj.get("result").asArray();
    JsonObject yesterdayUsageResultObj = usageResultAry.get(1).asObject();

    String yesterdayDate = yesterdayUsageResultObj.get("date").asString();
    String[] yesterdayDateAry = yesterdayDate.split("[-]+");

    // "C:statsstats_yyyy_mm_account.json"
    String statsFile = String.format("%s%s_%s_%s_%s.json",
        OUTPUT_DIR, statsType, yesterdayDateAry[0], yesterdayDateAry[1], account);

    // Create file
    File file = new File(statsFile);
    try {
        if (file.createNewFile()) {
            // created new file, seed with empty array
            FileWriter fw = new FileWriter(file.getAbsoluteFile());
            BufferedWriter bw = new BufferedWriter(fw);
            bw.write("[n]");
            bw.close();
        }
    } catch (IOException e) {
        e.printStackTrace();
    }

    // Read file
    String content = null;
    try {
        content = new String(Files.readAllBytes(Paths.get(statsFile)));
    } catch (IOException e) {
        e.printStackTrace();
    }

    // Remove trailing "]", append new record, append trailing "]"
    content = content.substring(0, content.length() - 1);
    content += yesterdayUsageResultObj.toString();
    content += "n]";

    // Write file
    FileWriter fw = null;
    try {
        fw = new FileWriter(file.getAbsoluteFile());
        BufferedWriter bw = new BufferedWriter(fw);
        bw.write(content);
        bw.close();
    } catch (IOException e) {
        e.printStackTrace();
    }
}

// Perform HTTP request
private static String httpRequest(String method, String endpoint, String body) {
    String data = "";
    try {
        URL url = new URL(endpoint);
        HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
        urlConn.setDoOutput(true);
        urlConn.setRequestMethod(method);
        switch (method) {
            case "GET":
                break;
            case "POST":
                urlConn.setRequestProperty("Content-type", "application/json");
                urlConn.setRequestProperty("accept", "text/json");
                OutputStreamWriter wr = new OutputStreamWriter(urlConn.getOutputStream());
                wr.write(body);
                wr.flush();
                break;
            default:
                System.out.println("Error: Invalid method.");
                return data;
        }
        int responseCode = urlConn.getResponseCode();
        if (responseCode == 200) {
            InputStream inStream = urlConn.getInputStream();
            data = convertStreamToString(inStream);
        } else {
            System.out.println(responseCode);
            data = "Status:" + responseCode;
        }
    } catch (MalformedURLException e) {
        System.out.println("URL not valid.");
    } catch (IOException e) {
        System.out.println("IOException: " + e.getMessage());
        e.printStackTrace();
    }
    return data;
}

private static String convertStreamToString(InputStream inputStream) {
    try {
        return new Scanner(inputStream).useDelimiter("A").next();
    } catch (NoSuchElementException e) {
        return "";
    }
}
}
```

**Índice.html**

```html
<html>
<head>
<title>Marketo API Stats</title>
<!-- Browser JavaScript variable dumper                  -->
<!-- https://github.com/padolsey-archive/prettyprint.js  -->
<script src="prettyPrint.js"></script>
</head>
<body>

<h1>Marketo API Stats</h1>

<script>
// JSONP callback that uses prettyPrint to format API stats
function processStats(usage, errors) {
    var cfg = { maxDepth: 5};
    document.write("<h2>Usage</h2>");
    document.body.appendChild(prettyPrint(usage, cfg));
    document.write("<h2>Errors</h2>");
    document.body.appendChild(prettyPrint(errors, cfg));
};
</script>

<!-- Web service for you to implement as an exercise                                                        -->
<!-- <script src="http://localhost:8080/stats?month=10&account=111-AAA-222&callback=processStats"></script> -->

<!-- Mock web service that returns sample payload -->
<!-- http://www.mocky.io/                         -->
<script src="http://www.mocky.io/v2/5627b2f9270000f2226eec63?month=10&year=2015&account=111-AAA-222&callback=processStats"></script>"
</body>
</html>
```

Publicado el _2015-10-22_ por _David_

## Excepción y administración de errores de la API de REST de Marketo: Parte 3

En su mayor parte, los errores recibidos de nuevo desde la API de REST de Marketo no se recuperarán automáticamente. Sin embargo, hay un puñado de casos en los que puede recuperarse automáticamente o en los que puede asegurarse de que nunca vea un determinado tipo de error.

### Errores de tamaño de solicitud

Como vimos en el último post de esta serie, Marketo emitirá el código de estado HTTP 414 si su URI excede los 8KiB de longitud, o 413 si su cuerpo de solicitud excede 1MB, o 10MB para importar posible cliente. Aunque los 414 serán poco frecuentes, puede verlos si utiliza el tipo Obtener posibles clientes por filtro para solicitar registros basados en 300 GUID independientes o criterios similares. Supongamos que tiene la siguiente solicitud: `<https://AAA-BBB-CCC.mktorest.com/rest/v1/leads.json?filterType=customGUID&fields=email`, empresa...firstName, lastName> Cuando envía la solicitud, Marketo devuelve el estado 414 porque el URI supera los 8 KiB.

Para manejarlo, necesitamos alterar el patrón de esta solicitud, y enviar un POST en lugar de un GET, anexar &#39;_method=GET&#39; al URI, y pasar la cadena de consulta en el cuerpo de la solicitud como una solicitud x-www-form-urlencoded en su lugar: URI: `<https://AAA-BBB-CCC.mktorest.com/rest/v1/leads.json?_method=GET>` Cuerpo de la solicitud: filterType=customGUID&amp;fields=email,company...firstName, lastName En lugar de capturar esta excepción de la respuesta HTTP, sin embargo, podemos simplemente comprobar la longitud total de la solicitud en tiempo de ejecución e implementar este patrón alternativo si el URI supera los 8 k. Como alternativa, puede utilizar el método POST en todos los casos para las recuperaciones por lotes de registros. Para 413s, podemos seguir un patrón similar, comprobando la longitud del cuerpo de la solicitud al añadir registros durante el paso de serialización y dividiendo la solicitud en varias partes si se supera este límite.

### Errores de autenticación

Nuestra siguiente clase de errores recuperables está relacionada con la autenticación. Cuando se utiliza un token de acceso anteriormente válido después de que haya transcurrido su periodo expires_in, el primer uso devolverá un código de error de 602, &quot;Token de acceso caducado&quot;. Después de esto, el uso del mismo token devolverá un 601, &quot;Token de acceso no válido&quot;. Cualquier otro uso de una cadena que no sea un token de acceso válido para la suscripción de destino resultará en un 601. En ambos casos, este error se puede recuperar de volviendo a autenticarse y pasando el nuevo token de acceso con un reintento de la solicitud fallida.

### Tiempos de espera

En circunstancias muy excepcionales, una llamada de puede devolver un error 604, &quot;Se ha agotado el tiempo de espera de la solicitud&quot;, después de que haya transcurrido el período de tiempo de espera de 30 segundos. En el caso de las solicitudes por lotes, como Crear/Actualizar posibles clientes, la solicitud se puede dividir en lotes más pequeños y volver a intentarlo hasta que se devuelva el resultado (si el lote se divide en menos de 100 registros y se sigue agotando el tiempo de espera de la solicitud, probablemente debería presentar un caso de asistencia técnica). El otro caso más común es el de las llamadas de aprobación de recursos, en el que otro usuario o servicio puede retener un bloqueo en el registro aprobado actual, como en el caso de un [correo electrónico](https://developer.adobe.com/marketo-apis/api/asset/approve-email-by-id/) o [plantilla de correo electrónico](https://developer.adobe.com/marketo-apis/api/asset/approve-email-template-by-id/). En estos casos, se debe usar [retroceso exponencial](https://en.wikipedia.org/wiki/Exponential_backoff) en los reintentos para permitir que se resuelva cualquier bloqueo existente. Vuelva a consultar en las próximas semanas la parte final de la serie, donde analizaremos más de cerca algunos errores específicos no recuperables.

Publicado el _2015-10-30_ por _Kenny_

## Mejoras de seguridad de Marketo

En Marketo nos tomamos muy en serio la seguridad. Como parte de una **[iniciativa en todo el sector](https://security.googleblog.com/2014/09/gradually-sunsetting-sha-1.html)**, Marketo está actualizando la autenticación y el cifrado web para mejorar nuestras protecciones de seguridad. El despliegue está programado para el 12 de diciembre de 2011. **¿Quién se verá afectado?** Solo se verá afectado un pequeño número de usuarios, solamente aquellos que tengan una integración a Marketo desde un sistema que tenga más de diez años y no se haya actualizado recientemente. Consulte **[esta lista](https://www.digicert.com/faq/sha2/sha-2-compatibility)** para obtener más información sobre los sistemas y las versiones compatibles. **Los siguientes usuarios no se verán afectados:**

* Usuarios finales que acceden a Marketo.com a través de exploradores modernos ([consulte la lista](https://www.digicert.com/faq/sha2/sha-2-compatibility))
* Clientes que utilizan socios de integración como Informatica, Dell Boomi y Scribe.
* Clientes que utilizan socios de LaunchPoint.

Todos los demás dominios de Marketo ya utilizan certificados SHA2.

Publicado el _18-11-2015_ por _Kenny_

## Sondeo de actividades mediante la API de REST

Las actividades son un objeto principal de la plataforma de Marketo. Las actividades son los datos de comportamiento almacenados sobre cada visita a la página web, cada apertura de correo electrónico, la asistencia a un seminario web o la asistencia a una feria comercial. Un caso de uso común es la combinación de datos de actividad con datos de otras partes de una organización. Este programa de ejemplo contiene 6 pasos:

1. Llame a [Obtener actividades de posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadActivitiesUsingGET) para generar una lista de todos los registros de actividad creados en una fecha u hora determinadas. Utilizamos un filtro para limitar el tipo de registros de actividad que se devuelven.
1. Extraiga los campos de interés de cada registro de actividad.
1. Llame a [Obtener varios posibles clientes por tipo de filtro](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) para generar una lista de registros de posibles clientes que se correspondan con las actividades del paso 1. Utilizamos el campo leadId extraído del registro de actividad en el paso 2 como filtro para especificar qué posibles clientes se devuelven.
1. Extraer campos de interés de cada registro de posibles clientes.
1. Una los datos de actividad del paso 2 con los datos de posible cliente del paso 4.
1. Transforme los datos del paso 5 en un formato que pueda utilizar un sistema externo.

Como se muestra en el diagrama siguiente, para este ejemplo hemos elegido capturar las actividades relacionadas con el correo electrónico.

**Entrada de programa** De manera predeterminada, el programa retrocede en el tiempo un día desde la fecha actual para buscar cambios. Por lo tanto, podría ejecutar este programa a la misma hora cada día, por ejemplo. Para retroceder más en el tiempo, puede especificar el número de días como argumento de la línea de comandos, lo que aumenta de manera efectiva la ventana de tiempo. El programa contiene varias variables que puede modificar: CUSTOM_SERVICE_DATA: Contiene los datos del servicio personalizado de Marketo (ID de cuenta, ID de cliente y Secreto de cliente). READ_BATCH_SIZE: es el número de registros que se recuperarán a la vez. Utilice esto para ajustar la respuesta al tamaño del cuerpo. LEAD_FIELDS: contiene una lista de los campos de posibles clientes que queremos recopilar. ACTIVITY_TYPES: contiene una lista de tipos de actividades que queremos recopilar.

**Lógica de programa** Primero establecemos nuestra ventana de tiempo, componemos nuestras URL de punto final REST y obtenemos nuestro token de acceso de autenticación. A continuación, activamos un bucle Obtener token de paginación/Obtener actividades de posible cliente que se ejecuta hasta que agotamos el suministro de actividades. El propósito de este bucle es recuperar registros de actividad y extraer campos de interés de esos registros. A las actividades de obtención de posibles clientes solo se les indica que busquen los siguientes tipos de actividades:

* Email entregado
* Cancelar suscripción a los correos electrónicos
* Abrir correo electrónico
* Haga clic en Correo electrónico.

Extraemos los siguientes campos de interés de cada registro de actividad:

* leadId
* activityType
* activityDate
* primaryAttributeValue

Puede seleccionar cualquier combinación de tipos de actividades y campos de actividades para adaptarlos a sus necesidades. A continuación activamos un bucle Get Multiple Leads by Filter Type que funciona hasta que agotamos el suministro de cables. Tenga en cuenta que utilizamos el parámetro &quot;filterType=id&quot; en combinación con una serie de parámetros &quot;filterValues&quot; para limitar los registros de posibles clientes recuperados únicamente a los posibles clientes asociados con actividades que recuperamos anteriormente. Extraemos los siguientes campos de interés de cada registro de posibles clientes:

* firstName
* lastName
* correo electrónico

De nuevo, tendrá la sensación de seleccionar cualquier campo de posible cliente que desee. A continuación, unimos los campos de posible cliente con los campos de actividad mediante el uso del ID de posible cliente para vincularlos juntos. Finalmente, recorremos todos los datos, los transformamos en formato JSON e imprimimos en la consola. **Salida de programa** Este es un ejemplo de la salida del programa de ejemplo. Esto muestra los campos de actividad y los campos de posible cliente combinados como objetos de &quot;resultado&quot; JSON. La idea aquí es que pueda pasar este JSON como una carga útil de solicitud a un servicio web externo.

```json
{
    "result": [
        {
            "leadId": 318581,
            "activityType": "Email Delivered",
            "activityDate": "2015-03-17T20:00:06Z",
            "primaryAttributeValue": "My Email Program",
            "firstName": "David",
            "lastName": "Everly",
            "email": "everlyd@marketo.com"
        },
        {
            "leadId":318581,
            "activityType":"Open Email",
            "activityDate":"2015-03-17T23:23:12Z",
            "primaryAttributeValue":"My Email Program - Auto Response",
            "firstName":"David",
            "lastName":"Everly",
            "email":"everlyd@marketo.com"
       },
        ... more result objects here...
    ]
}
```

**Código de programa**

```java
package com.marketo;

// minimal-json library (https://github.com/ralfstx/minimal-json)
import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;
import com.eclipsesource.json.JsonValue;

import java.io.\*;
import java.net.MalformedURLException;
import java.net.URL;
import java.text.DateFormat;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.\*;

import javax.net.ssl.HttpsURLConnection;

public class LeadActivities {
//
// Define Marketo REST API access credentials: Account Id, Client Id, Client Secret.  For example:
    private static Map<String, String> CUSTOM_SERVICE_DATA;
    static {
        CUSTOM_SERVICE_DATA = new HashMap<String, String>();
//        CUSTOM_SERVICE_DATA.put("accountId", "111-AAA-222");
//        CUSTOM_SERVICE_DATA.put("clientId", "2f4a4435-f6fa-4bd9-3248-098754982345");
//        CUSTOM_SERVICE_DATA.put("clientSecret", "asdf6IVE9h4Jjcl59cOMAKFSk78ut12W");
    }

    // Number of lead records to read at a time
    private static final String READ_BATCH_SIZE = "200";

    // Lookup lead records using lead id as primary key
    private static final String LEAD_FILTER_TYPE = "id";

    // Lead record lookup returns these fields
    private static final String LEAD_FIELDS = "firstName,lastName,email";

    // Lookup activity records for these activity types
    private static Map<Integer, String> ACTIVITY_TYPES;
    static {
        ACTIVITY_TYPES = new HashMap<Integer, String>();
        ACTIVITY_TYPES.put(7, "Email Delivered");
        ACTIVITY_TYPES.put(9, "Unsubscribe Email");
        ACTIVITY_TYPES.put(10, "Open Email");
        ACTIVITY_TYPES.put(11, "Click Email");
    }

    public static void main(String[] args) {
        // Command line argument to set how far back to look for lead changes (number of days)
        int lookBackNumDays = 1;
        if (args.length == 1) {
            lookBackNumDays = Integer.parseInt(args[0]);
        }

        // Establish "since date" using current timestamp minus some number of days (default is 1 day)
        Calendar cal = Calendar.getInstance();
        cal.add(Calendar.DAY_OF_MONTH, -lookBackNumDays);
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
        String sinceDateTime = sdf.format(cal.getTime());

        // Compose base URL
        String baseUrl = String.format("https://%s.mktorest.com",
            CUSTOM_SERVICE_DATA.get("accountId"));

        // Compose Identity URL
        String identityUrl = String.format("%s/identity/oauth/token?grant_type=%s&client_id=%s&client_secret=%s",
                baseUrl, "client_credentials", CUSTOM_SERVICE_DATA.get("clientId"), CUSTOM_SERVICE_DATA.get("clientSecret"));

        // Call Identity API
        JsonObject identityObj = JsonObject.readFrom(getData(identityUrl));
        String accessToken = identityObj.get("access_token").asString();

        // Compose URLs for Get Lead Changes, and Get Paging Token
        String activityTypesUrl = String.format("%s/rest/v1/activities/types.json?access_token=%s",
                baseUrl, accessToken);
        String pagingTokenUrl = String.format("%s/rest/v1/activities/pagingtoken.json?access_token=%s&sinceDatetime=%s",
                baseUrl, accessToken, sinceDateTime);

        // Use activity ids to create filter parameter
        String activityTypeIds = "";
        for (Integer id : ACTIVITY_TYPES.keySet()) {
            activityTypeIds += "&activityTypeIds=" + id.toString();
        }

        // Compose URL for Get Lead Activities
        // Only retrieve activities that match our defined activity types
        String leadActivitiesUrl = String.format("%s/rest/v1/activities.json?access_token=%s%s&batchSize=%s",
                baseUrl, accessToken, activityTypeIds, READ_BATCH_SIZE);

        Map<Integer, List> activitiesMap = new HashMap<Integer, List>();
        Set leadsSet = new HashSet();

        // Call Get Paging Token API
        JsonObject pagingTokenObj = JsonObject.readFrom(getData(pagingTokenUrl));
        if (pagingTokenObj.get("success").asBoolean()) {
            String nextPageToken = pagingTokenObj.get("nextPageToken").asString();
            boolean moreResult;
            do {
                moreResult = false;

                // Call Get Lead Activities API to retrieve activity data
                JsonObject leadActivitiesObj = JsonObject.readFrom(getData(String.format("%s&nextPageToken=%s",
                        leadActivitiesUrl, nextPageToken)));
                if (leadActivitiesObj.get("success").asBoolean()) {
                    moreResult = leadActivitiesObj.get("moreResult").asBoolean();
                    nextPageToken = leadActivitiesObj.get("nextPageToken").asString();

                    if (leadActivitiesObj.get("result") != null) {
                        JsonArray activitiesResultAry = leadActivitiesObj.get("result").asArray();
                        for (JsonValue activitiesResultObj : activitiesResultAry) {

                            // Extract activity fields of interest (leadID, activityType, activityDate, primaryAttributeValue)
                            JsonObject leadObj = new JsonObject();
                            int leadId = activitiesResultObj.asObject().get("leadId").asInt();
                            leadObj.add("activityType", ACTIVITY_TYPES.get(activitiesResultObj.asObject().get("activityTypeId").asInt()));
                            leadObj.add("activityDate", activitiesResultObj.asObject().get("activityDate").asString());
                            leadObj.add("primaryAttributeValue", activitiesResultObj.asObject().get("primaryAttributeValue").asString());

                            // Store JSON containing activity fields in a map using lead id as key
                            List leadLst = activitiesMap.get(leadId);
                            if (leadLst == null) {
                                activitiesMap.put(leadId, new ArrayList());
                                leadLst = activitiesMap.get(leadId);
                            }
                            leadLst.add(leadObj);

                            // Store unique lead ids for use as lead filter value below
                            leadsSet.add(leadId);
                        }
                    }
                }
            } while (moreResult);
        }

        // Use unique lead id values to create filter parameter
        String filterValues = "";
        for (Object object : leadsSet) {
            if (filterValues.length() > 0) {
                filterValues += ",";
            }
            filterValues += String.format("%s", object);
        }

        // Compose Get Multiple Leads by Filter Type URL
        // Only retrieve leads that match the list of lead ids that was accumulated during activity query
        String getMultipleLeadsUrl = String.format("%s/rest/v1/leads.json?access_token=%s&filterType=%s&fields=%s&filterValues=%s&batchSize=%s",
                baseUrl, accessToken, LEAD_FILTER_TYPE, LEAD_FIELDS, filterValues, READ_BATCH_SIZE);
        String nextPageToken = "";

        do {
            String gmlUrl = getMultipleLeadsUrl;

            // Append paging token to Get Multiple Leads by Filter Type URL
            if (nextPageToken.length() > 0) {
                gmlUrl = String.format("%s&nextPageToken=%s", getMultipleLeadsUrl, nextPageToken);
            }

            // Call Get Multiple Leads by Filter Type API to retrieve lead data
            JsonObject multipleLeadsObj = JsonObject.readFrom(getData(gmlUrl));
            if (multipleLeadsObj.get("success").asBoolean()) {
                if (multipleLeadsObj.get("result") != null) {
                    JsonArray multipleLeadsResultAry = multipleLeadsObj.get("result").asArray();

                    // Iterate through lead data
                    for (JsonValue leadResultObj : multipleLeadsResultAry) {
                        int leadId = leadResultObj.asObject().get("id").asInt();

                        // Join activity data with lead fields of interest (firstName, lastName, email)
                        List leadLst = activitiesMap.get(leadId);
                        for (JsonObject leadObj : leadLst) {
                            leadObj.add("firstName", leadResultObj.asObject().get("firstName").asString());
                            leadObj.add("lastName", leadResultObj.asObject().get("lastName").asString());
                            leadObj.add("email", leadResultObj.asObject().get("email").asString());
                        }
                    }
                }
            }

            nextPageToken = "";
            if (multipleLeadsObj.asObject().get("nextPageToken") != null) {
                nextPageToken = multipleLeadsObj.get("nextPageToken").asString();
            }
        } while (nextPageToken.length() > 0);

        // Now place activity data into an array of JSON objects
        JsonArray activitiesAry = new JsonArray();
        for (Map.Entry<Integer, List> activity : activitiesMap.entrySet()) {
            int leadId = activity.getKey();
            for (JsonObject leadObj : activity.getValue()) {
                // do something with leadId and each leadObj
                leadObj.add("leadId", leadId);
                activitiesAry.add(leadObj);
            }
        }

        // Print out result objects
        JsonObject result = new JsonObject();
        result.add("result", activitiesAry);
        System.out.println(result);

        System.exit(0);
    }

    // Perform HTTP GET request
    private static String getData(String endpoint) {
        String data = "";
        try {
            URL url = new URL(endpoint);
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
            urlConn.setRequestMethod("GET");
            urlConn.setAllowUserInteraction(false);
            urlConn.setDoOutput(true);
            int responseCode = urlConn.getResponseCode();
            if (responseCode == 200) {
                InputStream inStream = urlConn.getInputStream();
                data = convertStreamToString(inStream);
            } else {
                System.out.println(responseCode);
                data = "Status:" + responseCode;
            }
        } catch (MalformedURLException e) {
            System.out.println("URL not valid.");
        } catch (IOException e) {
            System.out.println("IOException: " + e.getMessage());
            e.printStackTrace();
        }
        return data;
    }

    private static String convertStreamToString(InputStream inputStream) {
        try {
            return new Scanner(inputStream).useDelimiter("A").next();
        } catch (NoSuchElementException e) {
            return "";
        }
    }
}
```

Eso es todo. ¡Feliz codificación!

Publicado el _2015-11-20_ por _David_

## Integración del reproductor SoundCloud con la API de Munchkin

SoundCloud proporciona una increíble plataforma de alojamiento de audio, con análisis y funcionalidad enriquecidos para todo, desde aspirantes a actos de rock independiente, a artistas de EDM en la cima de la industria musical, hasta podcasts de narración. Junto con la increíble funcionalidad nativa de la plataforma, viene un programa de API de primera clase para mover sus datos y rastrear el comportamiento de escucha. Esto es especialmente útil para los podcasters y puede permitirle correlacionar acciones de escucha específicas, como reproducciones, pausas y difusiones, con contenido específico del script y del audio. Hoy echaremos un vistazo a cómo aprovechar la API de widget [SoundCloud](https://developers.soundcloud.com/docs/api/html5-widget) para enviar y rastrear estas actividades en Marketo. En primer lugar, veamos la generación de una actividad de Munchkin que se registrará en la actividad de un posible cliente para iniciar sesión en Marketo. En su más básico, hacemos una llamada a Munchkin.munchkinFunction y pasamos &quot;visitWebPage&quot; como el primer argumento. Esto registrará una actividad de la página web de visitas con Marketo y cualquier dato arbitrario de URL y de la cadena de consulta que pasemos al método. El segundo argumento acepta un objeto JavaScript con nuestros datos, que tienen dos miembros, &quot;url&quot; y &quot;params&quot;, ambas cadenas. El miembro de la URL corresponde a la página web de la actividad en Marketo, mientras que los parámetros corresponden a la cadena de consulta. Para nuestros fines, utilizaremos la dirección URL como identificador para las acciones relacionadas con SoundCloud, &quot;soundCloudInteraction&quot;, mientras que los parámetros contendrán datos adicionales sobre la actividad en particular. Esta es la función que utilizamos para rastrear cada acción:

```javascript
var trackActivity = function(action){

    //set action param to be the string passed to the function
    var qs = "action=" + action;

    //use getCurrentSound callback to get the name of the current track
    soundCloudMunchkin.widget.getCurrentSound(function(currentSound){

        //add it to our querystring
        qs = qs + "&sound=" + currentSound.title;

        //use the getPosition callback to get the position of the track in ms
        soundCloudMunchkin.widget.getPosition(function(position){

            //add it to the querystring
            qs = qs + "&position=" + position;

            //assemble our data object for the munchkin activity
            var dataObject = {
                "url": "soundCloudInteraction",
                "params": qs
            }

            //call the munchkinFunction to submit the activity
            Munchkin.munchkinFunction("visitWebPage", dataObject);
        });
    });
}
```

Dado que el widget estándar de SoundCloud está incrustado en un iframe, el widget utiliza mensajes posteriores para comunicarse y las llamadas de retorno deben utilizarse para obtener datos, como puede ver con los métodos currentSound y getPosition. La API del widget de SoundCloud proporciona un conjunto de llamadas de retorno de JavaScript que podemos utilizar para responder a eventos individuales en el reproductor y enviarlos a Marketo. Los eventos que nos interesan son lo que el usuario escucha, cuánto tiempo escucha e interacciones que realiza con el reproductor, por lo que estamos viendo los siguientes eventos:

* REPRODUCIR
* PAUSA
* FINALIZAR
* BUSCAR
* CLICK_DOWNLOAD
* CLICK_BUY
* OPEN_SHARE_PANEL

También tendremos que utilizar el método bind() del widget para añadir llamadas de retorno a cada uno de estos eventos. Veamos un ejemplo...

```javascript
widget.bind(SC.Widget.Events.PLAY, function(){
    soundCloudMunchkin.trackPlay();
});
```

De este modo, siempre que se reproduzca una pista, se activará el método trackPlay para enviar un evento a Marketo con datos sobre la pista actual. Puede encontrar el script completo [aquí](https://gist.github.com/kelkingtron/6750bb07c1397d93d9c7#file-soundcloudmunchkin-js). El objeto soundCloudMunchkin tiene un método init, que acepta un objeto widget de SoundCloud como su único argumento, que enlaza los métodos de seguimiento a las llamadas de retorno relevantes, y configurará el widget para rastrear la actividad hasta Marketo. Tu página necesitará tener cargado tu [código Munchkin](/help/javascript-api/lead-tracking.md), así como la [biblioteca de API de SoundCloud](https://w.soundcloud.com/player/api.js). También tendrá que inicializar todo, además de incrustar el widget de SoundCloud real:

```javascript
window.onload=function(){
var iframe = document.getElementById(iframeId);
    if(iframe) {
        widget = SC.Widget(iframe);
        soundCloudMunchkin.init(widget);
    };
};
```

Publicado el _2015-12-21_ por _Kenny_

## RTP y la Directiva sobre privacidad en línea de la UE

Este artículo explica cómo puede usar RTP para notificar a los visitantes de un sitio web que están siendo rastreados o deshabilitar automáticamente el seguimiento para los visitantes europeos. Desde 2012, cualquier sitio web disponible para los visitantes europeos debe cumplir con la Directiva de privacidad electrónica de la UE. En 2011 entraron en vigor nuevas leyes que impiden que la información de identificación se almacene en el ordenador de un usuario sin su conocimiento y consentimiento. Si utiliza cookies o cualquier otra tecnología para el seguimiento no esencial, debe:

1. Indique a los usuarios que se utilizan tecnologías de seguimiento.
1. Explique las razones para utilizar esas tecnologías.
1. Obtener el consentimiento del usuario antes de utilizar esa tecnología y permitirle retirar el permiso en cualquier momento.

Publicado el _1970-01-01_ por _Yanir_

## Actualización de la información de clientes y clientes potenciales en Marketo mediante la API

Hay casos en los que se utilizan sistemas propietarios para actualizar la información de clientes y clientes potenciales. El equipo de marketing desea que esas actualizaciones se reflejen de nuevo en Marketo para que tengan el sistema de registro más preciso que utilizar en sus campañas de marketing. Con el siguiente método, puede configurar cargas periódicas a Marketo para mantener su información de contacto de Marketo actualizada con los datos modificados en ese sistema propietario. El diagrama siguiente muestra las llamadas de API que se realizan en un temporizador fijo periódico. A medida que se activa el temporizador periódico, la lógica del cliente recuperará primero los contactos actualizados del sistema propietario. La forma en que se realiza esto será diferente de un sistema a otro, ya sea mediante API o exportaciones de datos del sistema propietario. Detallaremos las API de Marketo que se ejecutan una vez que se recupera la información de contacto actualizada. Solicitud de SOAP para syncMultipleLeads:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ns2:paramsSyncMultipleLeads xmlns:ns2="<http://www.marketo.com/mktows/">
  <leadRecordList>
    <leadRecord>
      <Email>henry@superstar.com</Email>
      <ns2:leadAttributeList>
        <attribute>
          <attrName>FirstName</attrName>
          <attrValue>Henry</attrValue>
        </attribute>
        <attribute>
          <attrName>LastName</attrName>
          <attrValue>Adams</attrValue>
        </attribute>
        <attribute>
          <attrName>Title</attrName>
          <attrValue>Director of Demand Generation</attrValue>
        </attribute>
      </ns2:leadAttributeList>
    </leadRecord>
    <leadRecord>
      <Email>ssmith@gmail.com</Email>
      <ns2:leadAttributeList>
        <attribute>
          <attrName>FirstName</attrName>
          <attrValue>Suzie</attrValue>
        </attribute>
        <attribute>
          <attrName>LastName</attrName>
          <attrValue>Smith</attrValue>
        </attribute>
        <attribute>
          <attrName>Title</attrName>
          <attrValue>VP Marketing</attrValue>
        </attribute>
      </ns2:leadAttributeList>
    </leadRecord>
  </leadRecordList>
  <dedupEnabled>true</dedupEnabled>
</ns2:paramsSyncMultipleLeads>
```

Respuesta de SOAP de syncMultiLeads:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ns2:successSyncMultipleLeads xmlns:ns2="<http://www.marketo.com/mktows/">
  <result>
    <syncStatusList>
      <syncStatus>
        <leadId>1094593</leadId>
        <status>UPDATED</status>
        <error xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
      </syncStatus>
      <syncStatus>
        <leadId>1094594</leadId>
        <status>UPDATED</status>
        <error xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
      </syncStatus>
    </syncStatusList>
  </result>
</ns2:successSyncMultipleLeads>
```

`syncMultipleLeads` realiza una operación UPSERT. Si ya existe un contacto en Marketo en función de la dirección de correo electrónico enviada, los atributos se actualizarán. Si un contacto no existe, se crea. La respuesta de `syncMultipleLeads` devuelve el estado de cada uno de los contactos enviados. Los valores de `<attrName/>` dentro de `<leadAttributeList/>` deben coincidir con el nombre de API de SOAP definido para esa suscripción de Marketo. Puede descubrir los nombres de las API de SOAP en la sección de administración de campos del panel de administración de Marketo exportando los nombres de los campos.

Consulte el siguiente ejemplo de programa Java que ejecuta el escenario descrito anteriormente:

```java
 import com.marketo.mktows.\*;
import java.net.URL;
import javax.xml.namespace.QName;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import org.apache.commons.codec.binary.Hex;
import javax.xml.bind.JAXBContext;
import javax.xml.bind.JAXBElement;
import javax.xml.bind.Marshaller;
import java.util.\*;

public class SyncMultipleLeadsExample {

  public static void main(String[] args) {

    try {
      URL marketoSoapEndPoint = new URL("CHANGE ME" + "?WSDL");
      String marketoUserId = "CHANGE ME";
      String marketoSecretKey = "CHANGE ME";

      QName serviceName = new QName("http://www.marketo.com/mktows/", "MktMktowsApiService");
      MktMktowsApiService service = new MktMktowsApiService(marketoSoapEndPoint, serviceName);
      MktowsPort port = service.getMktowsApiSoapPort();

      // Create Signature
      DateFormat df = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
      String text = df.format(new Date());
      String requestTimestamp = text.substring(0, 22) + ":" + text.substring(22);
      String encryptString = requestTimestamp + marketoUserId ;

      SecretKeySpec secretKey = new SecretKeySpec(marketoSecretKey.getBytes(), "HmacSHA1");
      Mac mac = Mac.getInstance("HmacSHA1");
      mac.init(secretKey);
      byte[] rawHmac = mac.doFinal(encryptString.getBytes());
      char[] hexChars = Hex.encodeHex(rawHmac);
      String signature = new String(hexChars);

      // Set Authentication Header
      AuthenticationHeader header = new AuthenticationHeader();
      header.setMktowsUserId(marketoUserId);
      header.setRequestTimestamp(requestTimestamp);
      header.setRequestSignature(signature);

      // Create Request
      ParamsSyncMultipleLeads request = new ParamsSyncMultipleLeads();

      ObjectFactory objectFactory = new ObjectFactory();

      JAXBElement dedup = objectFactory.createParamsSyncMultipleLeadsDedupEnabled(true);
      request.setDedupEnabled(dedup);

      // The list of contacts defined here would be retrieved from the proprietary system
      Contact contact = new Contact("Henry","Adams","henry@superstar.com", "Director of Demand Generation");
      Contact contact2 = new Contact("Suzie","Smith","ssmith@gmail.com", "VP Marketing");

      ArrayList updatedContacts = new ArrayList();
      updatedContacts.add(contact);
      updatedContacts.add(contact2);

      ArrayOfLeadRecord arrayOfLeadRecords = new ArrayOfLeadRecord();

      Iterator it = updatedContacts.iterator();
      while(it.hasNext())
      {
        Contact c = it.next();

        LeadRecord leadRec = new LeadRecord();

        JAXBElement email = objectFactory.createLeadRecordEmail(c.email);
        leadRec.setEmail(email);

        Attribute attr1 = new Attribute();
        attr1.setAttrName("FirstName");
        attr1.setAttrValue(c.fname);

        Attribute attr2 = new Attribute();
        attr2.setAttrName("LastName");
        attr2.setAttrValue(c.lname);

        Attribute attr3 = new Attribute();
        attr3.setAttrName("Title");
        attr3.setAttrValue(c.title);

        ArrayOfAttribute aoa = new ArrayOfAttribute();
        aoa.getAttributes().add(attr1);
        aoa.getAttributes().add(attr2);
        aoa.getAttributes().add(attr3);

        QName qname = new QName("http://www.marketo.com/mktows/", "leadAttributeList");
        JAXBElement attrList = new JAXBElement(qname, ArrayOfAttribute.class, aoa);

        leadRec.setLeadAttributeList(attrList);
        arrayOfLeadRecords.getLeadRecords().add(leadRec);

      }

      request.setLeadRecordList(arrayOfLeadRecords);

      JAXBContext context = JAXBContext.newInstance(SuccessSyncMultipleLeads.class);
      Marshaller m = context.createMarshaller();
      m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
      m.marshal(request, System.out);

      SuccessSyncMultipleLeads result = port.syncMultipleLeads(request, header);

      m.marshal(result, System.out);
    }

    catch(Exception e) {
      e.printStackTrace();
    }
  }

  public static class Contact {
    public String fname, lname, email, title;

      public Contact(String fname, String lname, String email, String title) {
          this.fname = fname;
          this.lname = lname;
          this.email = email;
          this.title = title;
      }
  }
}
```

Este artículo contiene el código utilizado para implementar integraciones personalizadas. Debido a su naturaleza personalizada, el equipo de soporte técnico de Marketo no puede solucionar problemas del trabajo personalizado. No intente implementar el siguiente código de ejemplo sin tener la experiencia técnica adecuada o sin tener acceso a un desarrollador experimentado.

Publicado el _2014-03-24_ por _Travis Kaufman_

## Envío de un correo electrónico transaccional desde Marketo mediante la API

Requiere que se cree una campaña inteligente existente mediante la interfaz de usuario de Marketo. También requiere que el destinatario del correo electrónico exista en Marketo. Por lo tanto, antes de llamar a la API requestCampaign, utilice [getLead API]&#x200B;(/help/soap-api/getlead.md) para comprobar si el correo electrónico existe en Marketo. Después de realizar una llamada a través de la API requestCampaign, puede confirmarla comprobando si la campaña inteligente se ha ejecutado en Marketo. En primer lugar, se muestra cómo crear una campaña inteligente, en segundo, cómo configurar un déclencheur para enviar una campaña a través de la API, en tercer lugar, cómo definir un correo electrónico como parte de una acción de flujo y, en cuarto lugar, un ejemplo de código que se utilizaría para ejecutar esta campaña.
**Cómo crear una nueva campaña inteligente en Marketo** Las campañas inteligentes en Marketo ejecutan todas sus actividades de marketing. Puede configurar una serie de acciones automatizadas para realizar en una lista inteligente de contactos. En el caso del envío de correos electrónicos transaccionales, se configura un déclencheur en la campaña, como se muestra a continuación, para enviar correos electrónicos mediante la API. Primero vamos a configurar la campaña inteligente. 1. En Actividades de marketing, elija un Programa y, a continuación, en la lista desplegable Nuevo, haga clic en Nuevo recurso local.

1. Haga clic en Campaña inteligente
1. Introduzca un nombre de campaña inteligente y haga clic en Crear

**Agregar Déclencheur a una campaña inteligente** Agregar Déclencheur a una campaña inteligente le permite hacer que una campaña inteligente se ejecute de una persona a la vez en función de un evento en vivo, que en este caso es una solicitud a través de la [API requestCampaign](https://developer.adobe.com/marketo-apis/api/mapi/#operation/triggerCampaignUsingPOST). 1. Busque el déclencheur &quot;Campaña solicitada&quot; y arrástrelo y suéltelo en el lienzo.

1. En el déclencheur, seleccione &quot;es&quot; y &quot;API de servicio web&quot;.

**Cómo crear una acción de flujo de correo electrónico en una campaña** La asociación de un correo electrónico con una campaña inteligente permite a los especialistas en marketing administrar el aspecto que desean que tenga un correo electrónico, así como a la aplicación de terceros determinar quién lo recibe y cuándo lo hace. Después de crear un correo electrónico como un nuevo recurso local, puede establecerlo como una acción de flujo en una campaña.  Busque y seleccione el correo electrónico que desea enviar.

**Ejemplo de código para llamar a la API requestCampaign** Después de configurar la campaña y las déclencheur en la interfaz de Marketo, le mostramos cómo usar la API para enviar un correo electrónico. El primer ejemplo es una solicitud XML, el segundo es una respuesta XML y el último es un ejemplo de código Java que se puede utilizar para generar la solicitud XML. También le mostramos cómo encontrar el ID de campaña que se usa al realizar una llamada a la API `requestCampaign`.
La llamada de API también requiere que conozca previamente el ID de la campaña de Marketo. Puede determinar el ID de campaña mediante cualquiera de los siguientes métodos: 1. Use la API 1 de [getCampaignsForSource](/help/soap-api/getcampaignsforsource.md). Abra la campaña de Marketo en un explorador y observe la barra de direcciones URL. El ID de campaña (representado como un entero de 4 dígitos) se puede encontrar inmediatamente después de &quot;SC&quot;. Por ejemplo, `<https://app-stage.marketo.com/#SC**1025**A1>`. La parte en negrita es el ID de campaña: &quot;1025&quot;. Solicitud de SOAP para `requestCampaign`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809939944BFABAE58E5D27</mktowsUserId><requestSignature>48397ad47b71a1439f13a51eea3137df46441979</requestSignature><requestTimestamp>2013-08-01T12:31:14-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsRequestCampaign>
      <source>MKTOWS</source>
      <campaignId>4496</campaignId>
      <leadList>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>lead@company.com</keyValue>
        </leadKey>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>anotherlead@company.com</keyValue>
        </leadKey>
      </leadList>
    </ns1:paramsRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Respuesta de SOAP para requestCampaign

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successRequestCampaign>
      <result>
        <success>true</success>
      </result>
    </ns1:successRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Consulte a continuación un ejemplo de programa Java que ejecuta el escenario descrito anteriormente.

```java
import com.marketo.mktows.*;
import java.net.URL;
import javax.xml.namespace.QName;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import org.apache.commons.codec.binary.Hex;
import javax.xml.bind.JAXBContext;
import javax.xml.bind.JAXBElement;
import javax.xml.bind.Marshaller;


public class RequestCampaign {

    public static void main(String[] args) {
        System.out.println("Executing Request Campaign");
        try {
            URL marketoSoapEndPoint = new URL("CHANGE ME" + "?WSDL");
            String marketoUserId = "CHANGE ME";
            String marketoSecretKey = "CHANGE ME";

            QName serviceName = new QName("http://www.marketo.com/mktows/", "MktMktowsApiService");
            MktMktowsApiService service = new MktMktowsApiService(marketoSoapEndPoint, serviceName);
            MktowsPort port = service.getMktowsApiSoapPort();

            // Create Signature
            DateFormat df = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
            String text = df.format(new Date());
            String requestTimestamp = text.substring(0, 22) + ":" + text.substring(22);
            String encryptString = requestTimestamp + marketoUserId ;

            SecretKeySpec secretKey = new SecretKeySpec(marketoSecretKey.getBytes(), "HmacSHA1");
            Mac mac = Mac.getInstance("HmacSHA1");
            mac.init(secretKey);
            byte[] rawHmac = mac.doFinal(encryptString.getBytes());
            char[] hexChars = Hex.encodeHex(rawHmac);
            String signature = new String(hexChars);

            // Set Authentication Header
            AuthenticationHeader header = new AuthenticationHeader();
            header.setMktowsUserId(marketoUserId);
            header.setRequestTimestamp(requestTimestamp);
            header.setRequestSignature(signature);

            // Create Request
            ParamsRequestCampaign request = new ParamsRequestCampaign();

            request.setSource(ReqCampSourceType.MKTOWS);

            ObjectFactory objectFactory = new ObjectFactory();
            JAXBElement<Integer> campaignId = objectFactory.createParamsRequestCampaignCampaignId(4496);
            request.setCampaignId(campaignId);

            ArrayOfLeadKey leadKeyList = new ArrayOfLeadKey();
            LeadKey key = new LeadKey();
            key.setKeyType(LeadKeyRef.EMAIL);
            key.setKeyValue("lead@company.com");

            LeadKey key2 = new LeadKey();
            key2.setKeyType(LeadKeyRef.EMAIL);
            key2.setKeyValue("anotherlead@company.com");

            leadKeyList.getLeadKeies().add(key);
            leadKeyList.getLeadKeies().add(key2);

            JAXBElement<ArrayOfLeadKey> arrayOfLeadKey = objectFactory.createParamsRequestCampaignLeadList(leadKeyList);
            request.setLeadList(arrayOfLeadKey);

            SuccessRequestCampaign result = port.requestCampaign(request, header);

            JAXBContext context = JAXBContext.newInstance(SuccessRequestCampaign.class);
            Marshaller m = context.createMarshaller();
            m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
            m.marshal(result, System.out);

        }
        catch(Exception e) {
            e.printStackTrace();
        }
    }
}
```

Este artículo contiene el código utilizado para implementar integraciones personalizadas. Debido a su naturaleza personalizada, el equipo de soporte técnico de Marketo no puede solucionar problemas del trabajo personalizado. No intente implementar el siguiente código de ejemplo sin tener la experiencia técnica adecuada o sin tener acceso a un desarrollador experimentado.

Publicado el _2014-03-27_ por _Murta_

## Envío de un correo electrónico con contenido dinámico desde Marketo mediante el AP-

Imagine que desea automatizar los correos electrónicos de seguimiento del centro de llamadas. Después de que el representante de asistencia hable con un cliente, le gustaría enviar automáticamente un correo electrónico para agradecerle por ponerse en contacto con su compañía. Vamos a llevar esto un paso más allá y digamos que desea incluir el tema de conversación específico tratado con el cliente que rastrea en su CRM. Puede hacerlo desde Marketo utilizando la API requestCampaign SOAP para enviar un correo electrónico con contenido dinámico. La API requestCampaign le permite pasar un posible cliente o posibles clientes. También le permite pasar tokens de programa que se pueden utilizar con una campaña existente para enviar contenido dinámico. La API requestCampaign SOAP requiere que el destinatario del correo electrónico exista en Marketo. Por lo tanto, antes de llamar a la API requestCampaign, use la [API getLead](/help/soap-api/getlead.md) para comprobar si el correo electrónico existe en Marketo. Primero le mostramos cómo crear una campaña inteligente, segundo cómo configurar un déclencheur para enviar una campaña a través de la API, tercero cómo crear un correo electrónico que acepte contenido dinámico a través de tokens de programa, cuarto cómo definir un correo electrónico como parte de una acción de flujo y quinto un ejemplo de código que se utilizaría para ejecutar esta campaña. **Cómo crear una nueva campaña inteligente en Marketo** Las campañas inteligentes en Marketo ejecutan todas sus actividades de marketing. Puede configurar una serie de acciones automatizadas para realizar en una lista inteligente de contactos. En el caso del envío de correos electrónicos transaccionales, se configura un déclencheur en la campaña, como se muestra a continuación, para enviar correos electrónicos mediante la API. Primero vamos a configurar la campaña inteligente. 1. En Actividades de marketing, elija un Programa y, a continuación, en la lista desplegable Nuevo, haga clic en Nuevo recurso local

1. Haga clic en Campaña inteligente
1. Escriba el nombre de la campaña inteligente y haga clic en Crear **Agregar Déclencheur a una campaña inteligente**. Agregar Déclencheur a una campaña inteligente le permite ejecutar una campaña inteligente en una persona a la vez basándose en un evento activo, que en este caso es una solicitud a través de la [API requestCampaign](https://developer.adobe.com/marketo-apis/api/mapi/#operation/triggerCampaignUsingPOST).
1. Busque el déclencheur &quot;Campaña solicitada&quot; y arrástrelo y suéltelo en el lienzo.
1. En el déclencheur, seleccione &quot;es&quot; y &quot;API de servicio web&quot;.

**Cómo pasar contenido dinámico mediante la API** En Marketo, mis tokens son variables que puede usar en su programa. Mis tokens le permiten introducir información relativa a su programa en un lugar, reemplazar esa información por un valor que especifique y recuperar esta información en otras partes de la aplicación, como una plantilla de correo electrónico. Mediante la API de SOAP de Campaign, puede pasar una matriz de tokens de programa, que anularán los tokens existentes. Después de ejecutar la campaña, los tokens se descartan. Puede crear Mis tokens en el nivel de carpeta de Campaign o en el nivel de programa. Mis tokens en el nivel de carpeta de Campaign se heredarán en todos los programas contenidos en la carpeta de Campaign. Si crea Mis tokens en el nivel de carpeta de Campaign, puede sobrescribir el valor heredado en el nivel de Programa. Por ejemplo, si define tokens para la Fecha del programa y la Descripción del programa en el nivel de carpeta de campaña, puede sobrescribir estos valores en el nivel de programa individual.

Así es como se hace esto. 1. En el árbol Actividades de marketing, seleccione la carpeta Campaña o el Programa donde desee crear los tokens. En la barra de menús superior, seleccione Mis tokens. A continuación, se muestra el lienzo Mis tokens. Desde el árbol del lado derecho, arrastre un Tipo de token al lienzo, que en este caso es &quot;Texto&quot;. En el campo Nombre del token, resalte Mi token e introduzca un Nombre de token único, que en este caso es &quot;my.conversationtopic&quot;. En el campo Value, introduzca un Value relevante para el token, que en este caso es &quot;Gracias por llamarnos hoy&quot;. Tenga en cuenta que al utilizar la API anularemos el valor predeterminado de Mi token. Haga clic en &quot;Guardar&quot; para guardar el token personalizado.  1. Cree un nuevo correo electrónico haciendo clic en Nuevo. A continuación, haga clic en New Local Assets y seleccione Email. A continuación, rellene los campos relevantes para asignar un nombre al correo electrónico. Al redactar el borrador del correo electrónico, haga clic en el icono Token para incluir tokens en el correo electrónico. Ahora que ha creado su correo electrónico de plantilla con tokens, añadiremos el correo electrónico como una acción de flujo para la campaña en el paso siguiente. Por lo tanto, cuando llama a la campaña a través de la API, se envía el correo electrónico.
**Cómo crear una acción de flujo de correo electrónico en una campaña** La asociación de un correo electrónico con una campaña inteligente permite a los especialistas en marketing administrar el aspecto que desean que tenga un correo electrónico, así como a la aplicación de terceros determinar quién lo recibe y cuándo lo hace. Después de crear un correo electrónico como un nuevo recurso local, puede establecerlo como una acción de flujo en una campaña. Busque y seleccione el correo electrónico que desea enviar.
**Ejemplo de código para llamar a la API requestCampaign** Después de configurar la campaña y las déclencheur en la interfaz de Marketo, le mostramos cómo usar la API para enviar un correo electrónico. El primer ejemplo es una solicitud XML, el segundo es una respuesta XML y el último es un ejemplo de código Java que se puede utilizar para generar la solicitud XML. También le mostramos cómo encontrar el ID de campaña que se utiliza al realizar una llamada a la API requestCampaign. La llamada de API también requiere que conozca previamente el ID de la campaña de Marketo. Puede determinar el ID de campaña mediante cualquiera de los siguientes métodos: 1. Use la API 1 de [getCampaignsForSource](/help/soap-api/getcampaignsforsource.md). Abra la campaña de Marketo en un explorador y observe la barra de direcciones URL. El ID de campaña (representado como un entero de 4 dígitos) se puede encontrar inmediatamente después de &quot;SC&quot;. Por ejemplo, `<https://app-stage.marketo.com/#SC&#x200B;**1025**&#x200B;A1>`. La parte en negrita es el ID de campaña: &quot;1025&quot;. Solicitud de SOAP para requestCampaign

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809939944BFABAE58E5D27</mktowsUserId><requestSignature>48397ad47b71a1439f13a51eea3137df46441979</requestSignature><requestTimestamp>2013-08-01T12:31:14-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsRequestCampaign>
      <source>MKTOWS</source>
      <campaignId>4496</campaignId>
      <leadList>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>lead@company.com</keyValue>
        </leadKey>
      </leadList>
      <programTokenList>
        <attrib>
          <name>{{my.conversationtopic}}</name>
          <value>Thank you for calling about adding a line of service to your current plan.</value>
        </attrib>
      </programTokenList>
    </ns1:paramsRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Respuesta de SOAP para requestCampaign

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successRequestCampaign>
      <result>
        <success>true</success>
      </result>
    </ns1:successRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Consulte a continuación un ejemplo de programa Java que ejecuta el escenario descrito anteriormente.

```java
import com.marketo.mktows.\*;
import java.net.URL;
import javax.xml.namespace.QName;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import org.apache.commons.codec.binary.Hex;
import javax.xml.bind.JAXBContext;
import javax.xml.bind.JAXBElement;
import javax.xml.bind.Marshaller;


public class RequestCampaign {

    public static void main(String[] args) {
        System.out.println("Executing Request Campaign");
        try {
            URL marketoSoapEndPoint = new URL("CHANGE ME" + "?WSDL");
            String marketoUserId = "CHANGE ME";
            String marketoSecretKey = "CHANGE ME";

            QName serviceName = new QName("http://www.marketo.com/mktows/", "MktMktowsApiService");
            MktMktowsApiService service = new MktMktowsApiService(marketoSoapEndPoint, serviceName);
            MktowsPort port = service.getMktowsApiSoapPort();

            // Create Signature
            DateFormat df = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
            String text = df.format(new Date());
            String requestTimestamp = text.substring(0, 22) + ":" + text.substring(22);
            String encryptString = requestTimestamp + marketoUserId ;

            SecretKeySpec secretKey = new SecretKeySpec(marketoSecretKey.getBytes(), "HmacSHA1");
            Mac mac = Mac.getInstance("HmacSHA1");
            mac.init(secretKey);
            byte[] rawHmac = mac.doFinal(encryptString.getBytes());
            char[] hexChars = Hex.encodeHex(rawHmac);
            String signature = new String(hexChars);

            // Set Authentication Header
            AuthenticationHeader header = new AuthenticationHeader();
            header.setMktowsUserId(marketoUserId);
            header.setRequestTimestamp(requestTimestamp);
            header.setRequestSignature(signature);

            // Create Request
            ParamsRequestCampaign request = new ParamsRequestCampaign();

            request.setSource(ReqCampSourceType.MKTOWS);

            ObjectFactory objectFactory = new ObjectFactory();
            JAXBElement<Integer> campaignId = objectFactory.createParamsRequestCampaignCampaignId(4496);
            request.setCampaignId(campaignId);

            ArrayOfLeadKey leadKeyList = new ArrayOfLeadKey();
            LeadKey key = new LeadKey();
            key.setKeyType(LeadKeyRef.EMAIL);
            key.setKeyValue("lead@company.com");

            leadKeyList.getLeadKeies().add(key);

            JAXBElement<ArrayOfLeadKey> arrayOfLeadKey = objectFactory.createParamsRequestCampaignLeadList(leadKeyList);
            request.setLeadList(arrayOfLeadKey);

            ArrayOfAttrib aoa = new ArrayOfAttrib();

            Attrib attrib = new Attrib();
            attrib.setName("{{my.conversationtopic}}");
            attrib.setValue("Thank you for calling about adding a line of service to your current plan.");

            aoa.getAttribs().add(attrib);

            JAXBElement<ArrayOfAttrib> arrayOfAttrib = objectFactory.createParamsRequestCampaignProgramTokenList(aoa);
            request.setProgramTokenList(arrayOfAttrib);

            SuccessRequestCampaign result = port.requestCampaign(request, header);

            JAXBContext context = JAXBContext.newInstance(SuccessRequestCampaign.class);
            Marshaller m = context.createMarshaller();
            m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
            m.marshal(result, System.out);

        }
        catch(Exception e) {
            e.printStackTrace();
        }
    }
}
```

Después de realizar una llamada a través de la API requestCampaign, puede confirmarla comprobando si la campaña inteligente se ha ejecutado en Marketo.

Este artículo contiene el código utilizado para implementar integraciones personalizadas. Debido a su naturaleza personalizada, el equipo de soporte técnico de Marketo no puede solucionar problemas del trabajo personalizado. No intente implementar el siguiente código de ejemplo sin tener la experiencia técnica adecuada o sin tener acceso a un desarrollador experimentado.

Publicado el _2014-04-03_ por _Murta_

## Capturar actividad de visitante anónima en función de la lógica empresarial

Imagine que desea rastrear a los usuarios que visitan una publicación específica en el blog de su empresa. Supongamos que, del número total de usuarios que visitan una publicación, solo le gustaría rastrear a los usuarios que señalan su interés pasando al menos 5 segundos y desplazándose hacia abajo por la página. Para los usuarios anónimos, le gustaría crear un nuevo posible cliente en Marketo con este evento y, para los usuarios conocidos, le gustaría actualizar su actividad de posible cliente con este evento. Puede hacerlo usando el [código de seguimiento de Munchkin](/help/javascript-api/lead-tracking.md) en su sitio web. Cuando un usuario sin cookies va a una página con el código de seguimiento de Munchkin, se crea una nueva cookie en el explorador del usuario y se crea un nuevo posible cliente anónimo en Marketo. Si el usuario ya tiene cookies y ya es un posible cliente de Marketo, la visita a la página se registrará en el registro de actividad del usuario en Marketo. En primer lugar, se muestra cómo generar el código de seguimiento de Munchkin en Marketo, en segundo, cómo modificar el código de muestra de Munchkin para que solo se almacene en déclencheur si se cumplen determinadas condiciones y, en tercer lugar, cómo comprobar que se ha registrado en Marketo una visita de página de un usuario anónimo.

**Cómo generar el código de seguimiento de Munchkin** El código de seguimiento de Munchkin le permite hacer un seguimiento de las visitas a su sitio web. A continuación se describen tres tipos de código Munchkin, pero en este ejemplo utilizamos el código de seguimiento Munchkin asincrónico. A) Simple: tiene las menos líneas de código, pero no optimiza el tiempo de carga de la página web. Este código carga la biblioteca jQuery cada vez que se carga una página web. B) Asincrónica: reduce el tiempo de carga de la página web. Este código comprueba si la biblioteca jQuery ya existe, la carga si falta y la utiliza para ejecutar el código de seguimiento una vez que se ha cargado el resto de la página web. C) Async jQuery: reduce el tiempo de carga de la página web y también mejora el rendimiento del sistema. Este código supone que ya tiene jQuery y no marca la casilla para cargarlo. 1. Haga clic en Administración en la parte superior derecha de la aplicación.  1. Haga clic en Munchkin en el árbol de la izquierda.  1. Seleccione Asincrónica para el tipo de código de seguimiento. 1. Haga clic en y copie el código de seguimiento de JavaScript que desea colocar en el sitio web.
**Ejemplo de código para rastrear eventos y usuarios de cookies** Coloque el código de seguimiento en sus páginas web justo antes de la etiqueta `</body>`. Las páginas de aterrizaje creadas en Marketo contienen automáticamente código de seguimiento, por lo que no es necesario colocarlo. Este ejemplo de código llamaría a la API de Munchkin después de cargar el script:

```javascript
<script type="text/javascript">
(function() {
  var didInit = false;
  function initMunchkin() {
    if(didInit === false) {
      didInit = true;
      Munchkin.init('XXX-XXX-XXX');
    }
  }
  var s = document.createElement('script');
  s.type = 'text/javascript';
  s.async = true;
  s.src = '//munchkin.marketo.net/munchkin.js';
  s.onreadystatechange = function() {
    if (this.readyState == 'complete' || this.readyState == 'loaded') {
      initMunchkin();
    }
  };
  s.onload = initMunchkin;
  document.getElementsByTagName('head')[0].appendChild(s);
})();
</script>
```

Este ejemplo de código llama a la API de Munchkin después de que el usuario haya estado en la página durante 5 segundos y también haya desplazado 500 píxeles hacia abajo en la página:

```javascript
<script src="https://code.jquery.com/jquery-2.1.0.min.js"></script>
<script type="text/javascript">
$(function(){
 setTimeout(function(){
  $(window).scroll(function() {
      var y_scroll_position = window.pageYOffset;
      var scroll_position = 500; //Sets number of pixels user must scroll to be tracked

  if(y_scroll_position > scroll_position) {
  //Munchkin tracking code
   (function() {
     var didInit = false;
     function initMunchkin() {
      if(didInit === false) {
        didInit = true;
        Munchkin.init('XXX-XXX-XXX');
      }
     }

     var s = document.createElement('script');
     s.type = 'text/javascript';
     s.async = true;
    s.src = '//munchkin.marketo.net/munchkin.js';
     s.onreadystatechange = function() {
      if (this.readyState == 'complete' || this.readyState == 'loaded') {
          initMunchkin();
      }
     };
     s.onload = initMunchkin;
     document.getElementsByTagName('head')[0].appendChild(s);
   })();
   }
 },5000); //Sets time delay before tracking user
});
</script>
```

**Cómo verificar que la visita a la página de un usuario anónimo se haya registrado en Marketo**

1. Haga clic en Analytics en el menú superior y, a continuación, haga clic en Nuevo informe. Elija Actividad de página web como tipo de informe y asigne un nombre al informe.
1. Después de crear un informe, haga clic en Lista inteligente. A continuación, seleccione el filtro Página web visitada en el cuadro de la derecha. Introduzca la página web donde colocó el código de seguimiento de Munchkin.
1. Haga clic en Configuración. Seleccione Visitantes anónimos de los ISP y cambie la opción a Mostrado.
1. Haga clic en Informe. Ahora verá la actividad rastreada en la página web que seleccionó.
1. Haga doble clic en el registro de posibles clientes, que luego mostrará el registro de actividad donde puede ver la página específica que visitó el usuario anónimo.

Este artículo contiene el código utilizado para implementar integraciones personalizadas. Debido a su naturaleza personalizada, el equipo de soporte técnico de Marketo no puede solucionar problemas del trabajo personalizado. No intente implementar el siguiente código de ejemplo sin tener la experiencia técnica adecuada o sin tener acceso a un desarrollador experimentado.

Publicado el _2014-04-17_ por _Murta_

## Cambiar dinámicamente el número de teléfono local mediante RTP

Personalization lo es todo: lo descubrimos hace mucho tiempo. Dicho esto, todavía me sorprende que cada vez que necesito asistencia inmediata, sea tan difícil encontrar los números de teléfono locales relevantes en un sitio web. Menos mal que tenemos [Marketo Real-Time Personalization](https://business.adobe.com/es/products/marketo/content-personalization.html) (RTP) instalado en <https://business.adobe.com/es/products/marketo/adobe-marketo.html>. Podemos aprovechar la [API de visitante RTP](/help/javascript-api/web-personalization.md) para cambiar dinámicamente el número de teléfono que un visitante web ve en diferentes secciones del sitio web. ¡Guau! ¿Puedes creer esto? ¿Cómo funciona esta magia? Primero, debe tener RTP instalado en el sitio web como se describe [aquí](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript). A continuación, siga las instrucciones que se indican a continuación e implemente el código JavaScript en su sitio web:

1. Inserta tu número de teléfono internacional en la configuración de **defaultPhone**
1. Inserte los identificadores de elemento HTML en la configuración **divIds**
1. Si desea que el número de teléfono sea un vínculo de clic para llamar para navegadores móviles, establezca la configuración de **mobileLink** en **true**.
1. Mapa las diferentes ubicaciones con sus números de teléfono en las configuraciones de **cityPhone**, **statePhone** y **countryPhone**

Por ejemplo, estos son valores de ejemplo para los ajustes de configuración:

```json
  defaultPhone:"+1.503.608.4679", // Optional
  divIds:["phoneId1","phoneId2"],
  mobileLink: true,
  cityPhone: {
    "<a href="#">yanir</a>": ["San Mateo", "San Francisco"],
    "+353.1.242.3000": ["tel-aviv"]
  },
  statePhone: {
    "+1.650.376.2300": ["CA"],
    "+1.650.376.2302": ["OR"]
  },
  countryPhone: {
    "+1.650.376.2300": ["United States"],
    "+353.1.242.3000": ["Israel"]
  }
```

Finalmente, inserte una etiqueta de anclaje de HTML que contenga un id que coincida con uno de los id en **divIds** (del paso 2 anterior). Por ejemplo, si especificó &quot;phoneId1&quot; en **divIds**, la etiqueta de anclaje de HTML tendría este aspecto:

`<a href="tel:+1800229933" id="phoneId1">+1800229933</a>`

La secuencia de comandos comprueba si hay una coincidencia en este orden: cityPhone > statePhone > countryPhone > defaultPhone También puede reemplazar los números de teléfono con texto (Ejemplo: &quot;Únase a nuestro grupo de usuarios de San Francisco&quot;) o código HTML y cambie el contenido de forma dinámica en función de la ubicación geográfica. ¡Disfrútelo!

```html
<a href="tel:+1800229933" id="phoneId1">+1800229933</a>
<script>
(function(a){
    rtp('get','visitor',function(yc){
        var location = yc.results.location;
        var loop = true;
        var phoneChanged = false;
        console.log(yc.results);
        function checkObj(obj){
            return Object.getOwnPropertyNames(obj).length >0;
        }
        function changePhone(p){
            d=a.divIds;
            for(i=0;i<d.length;i++){
                if(document.getElementById(d[i]) !== null){
                    document.getElementById(d[i]).innerHTML = p;
                    if(a.mobileLink){
                        document.getElementById(d[i]).href= "tel:" + p;
                    }
                    console.log(p);
                }
            }
            loop = false;
            phoneChanged = true;
        }
        function matchLocation(loc,objc){
            for (var key in objc) {
                for(i=0;i<objc[key].length && loop;i++){
                    if (!loop) { return true;};
                    val = objc[key][i];
                    //console.log(loc + location[loc] + " ? " + val);
                    if(location[loc].toLowerCase() === val.toLowerCase()){
                        changePhone(key);
                    }
                }
            }
        }
        if(checkObj(a.cityPhone)){
            matchLocation("city",a.cityPhone);
        }else if(checkObj(a.statePhone)){
            matchLocation("state",a.statePhone);
        }else if(checkObj(a.countryPhone)){
            matchLocation("country", a.countryPhone);
        }else if(!phoneChanged && a.defaultPhone.length > 0 ){
            changePhone(a.divId,a.defaultPhone);
        }
    });
})({
    defaultPhone:"",  //  [Optional] the number to show if visitor does not match the mapping below
    divIds:["phoneId1","Floater"],  //the phone HTML element ID, can be <div>, <a>, <span>, <p> etc.
    mobileLink: true,  //if you use click to call link (with href="tel:") you can also change its number

    cityPhone: {
        "<a href='#'>yanir</a>": ["San Mateo", "San Francisco"],
        "+353.1.242.3000": ["tel-aviv"]
    },
    statePhone: {
        "+1.650.376.2300": ["CA"],
        "+1.650.376.2302": ["OR"]
    },
    countryPhone: {
        "+1.650.376.2300": ["United States"],
        "+353.1.242.3000": ["Israel"]
    }
});
</script>
```

Publicado el _2016-02-02_ por _Yanir_

## Actualizaciones de invierno de 2016

### Objetos personalizados

* [Ahora se admiten relaciones de objetos personalizados N:N](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/administration/marketo-custom-objects/create-marketo-custom-objects)
   * Los registros de cliente potencial o Cuenta ahora pueden tener relaciones &quot;varios a varios&quot; a través de objetos personalizados mediante la definición de objetos intermedios. Después de crear un tipo de objeto personalizado independiente, se puede crear un tipo de objeto intermedio con campos de vínculo al objeto independiente y a posibles clientes o cuentas.
   * No hay llamadas nuevas a la API para esta capacidad, pero las definiciones de objeto deben configurarse correctamente para aprovechar estas relaciones a través de la API.
* `getLeadActivities` y `getLeadChanges` ya no devolverán actividades de posibles clientes anónimos. Consulte las [Preguntas frecuentes sobre el seguimiento de Munchkin de próxima generación](https://experienceleague.adobe.com/es/docs/marketo/using/home) para obtener más información

Publicado el _2016-02-05_ por _Kenny_

## Recuperar actividades para un solo posible cliente mediante la API de REST

Esta es una pregunta que nuestra comunidad de desarrolladores nos hace repetidamente:

&quot;¿Cómo obtengo una lista de actividades pasadas para un posible cliente individual?&quot;

Hasta hace poco, no había una manera directa de lograr esto usando la API de REST. ¡Pero ahora lo hay! La versión de invierno de 2016 de nuestra API de REST contiene una pequeña mejora. [Obtener actividades de posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadActivitiesUsingGET) ahora acepta el parámetro **leadIds** que se puede usar para especificar un ID de posible cliente. Cuando se especifica el parámetro **leadIds**, solo se devolverán las actividades para ese ID de posible cliente. Esto se puede considerar como un filtro de ID de posible cliente. Tenga en cuenta que el parámetro **leadIds** puede tomar una lista de ID de posibles clientes separados por comas en caso de que desee filtrar los resultados en más de un posible cliente (hasta 30). Esto puede resultar útil, por ejemplo, al limitar las actividades a posibles clientes para una compañía en particular. **Ejemplo** a continuación se muestra una solicitud de ejemplo para [Obtener actividades de posible cliente](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadActivitiesUsingGET) que contiene el parámetro **leadIds**. He especificado el valor &quot;50&quot; para el parámetro **leadIds**, que corresponde a un posible cliente arbitrario en mi instancia de Marketo. He especificado el valor 129 para el parámetro activityTypeIds, que corresponde a la actividad Sesión de la aplicación móvil en mi instancia de Marketo.

`<https://123-abc-456.mktorest.com/rest/v1/activities.json?leadIds=50&activityTypeIds=129&nextPageToken=WQV2VQVPPCKHC6AQYVK7JDSA3J4SMAZRQO4RKIXCEMLFCM2APRSQ====>`

A continuación figura un extracto de la respuesta a esa solicitud. Como puede ver, solo contiene objetos de resultado con &quot;leadId&quot;: 50 y &quot;activityTypeId&quot;: 129.

```json
{
    "id": 846,
    "leadId": 50,
    "activityDate": "2015-04-06T21:58:59Z",
    "activityTypeId": 129,
    "primaryAttributeValueId": 13,
    "primaryAttributeValue": "Sample App",
    "attributes": [
        {
            "name": "Device Type",
            "value": "iPhone"
        },
        {
            "name": "Mobile App Session Length",
            "value": 7
        },
        {
            "name": "Platform",
            "value": "ios"
        }
    ]
}
{
    "id": 879,
    "leadId": 50,
    "activityDate": "2015-04-07T00:45:11Z",
    "activityTypeId": 129,
    "primaryAttributeValueId": 13,
    "primaryAttributeValue": "Sample App",
    "attributes": [
        {
            "name": "Device Type",
            "value": "iPhone"
        },
        {
            "name": "Mobile App Session Length",
            "value": 5
        },
        {
            "name": "Platform",
            "value": "ios"
        }
    ]
}
{
    "id": 1114,
    "leadId": 50,
    "activityDate": "2015-04-08T00:02:41Z",
    "activityTypeId": 129,
    "primaryAttributeValueId": 13,
    "primaryAttributeValue": "Sample App",
    "attributes": [
        {
            "name": "Device Type",
            "value": "iPhone"
        },
        {
            "name": "Mobile App Session Length",
            "value": 241
        },
        {
            "name": "Platform",
            "value": "ios"
        }
    ]
}
{
    "id": 1551,
    "leadId": 50,
    "activityDate": "2015-04-09T23:31:56Z",
    "activityTypeId": 129,
    "primaryAttributeValueId": 13,
    "primaryAttributeValue": "Sample App",
    "attributes": [
        {
            "name": "Device Type",
            "value": "iPhone"
        },
        {
            "name": "Mobile App Session Length",
            "value": 223
        },
        {
            "name": "Platform",
            "value": "ios"
        }
    ]
}
{
    "id": 1716,
    "leadId": 50,
    "activityDate": "2015-04-15T22:44:19Z",
    "activityTypeId": 129,
    "primaryAttributeValueId": 13,
    "primaryAttributeValue": "Sample App",
    "attributes": [
        {
            "name": "Device Type",
            "value": "iPhone"
        },
        {
            "name": "Mobile App Session Length",
            "value": 223
        },
        {
            "name": "Platform",
            "value": "ios"
        }
    ]
}
```

## Integración perfecta con Marketo y más de 500 aplicaciones con Zapier

Esta es una publicación de Philippe Delle Case, consultor de soluciones principales de Marketo.

### Objetivos

Este artículo explica en detalle cómo integrar Marketo con más de 500 aplicaciones en la nube, gracias a Zapier. Para ello, vamos a construir desde cero un conector Zapier para Marketo e implementar dos casos prácticos de integración: Caso de uso 1: Integración de posibles clientes unidireccional desde FullContact Card Reader a Marketo

* Escanee la tarjeta de presentación de cualquier contacto con la aplicación Reader de la tarjeta móvil FullContact y obtenga un posible cliente creado automáticamente en Marketo.
* Agregue un posible cliente existente a una lista estática en Marketo y busque el posible cliente agregado automáticamente a su hoja de Google.
* Modifique cualquier posible cliente de la hoja de Google y busque el cambio repetido en Marketo.

### Requisitos previos

**Regístrese para obtener una cuenta gratuita con Zapier** [Zapier](https://zapier.com/) es un servicio de automatización de aplicaciones web que le permite automatizar fácilmente tareas entre otras aplicaciones en línea sin necesidad de programadores ni recursos de TI. Se puede encontrar una breve introducción [aquí](https://zapier.com/api/v4/helpdocs/category/redirect/what-is-zapier). Hoy en día Zapier soporta más de 500 aplicaciones en muchos dominios diferentes como Marketing, CRM, CMS, Atención al cliente, Firma electrónica, Forms, etc. Una sola integración entre una aplicación y otra se denomina Zap. Consulta [zapbook](https://zapier.com/apps) de Zapier para ver una lista exhaustiva de las aplicaciones web compatibles.  Regístrese para obtener una cuenta gratuita [aquí](https://zapier.com/sign-up/). Puede acceder a hasta 100 tareas al mes, 5 zaps y zaps que se ejecutan cada 15 minutos. Por supuesto, puedes obtener mucho más suscribiéndote a los planes de pago de Zapier (básico, comercial, business plus, etc.)

**Acceso a una instancia de Marketo como administrador o con una cuenta de usuario de API proporcionada** Nuestro conector Zapier utilizará la API de REST de Marketo para insertar los datos de posibles clientes en Marketo. Para utilizar esta API, necesita un usuario de API y un servicio personalizado que pueda crear usted mismo si es el administrador de su instancia de Marketo. Si no es así, un administrador deberá proporcionárselos. También hay un webhook para crear, al que solo puede acceder un administrador de Marketo. Encontrará una explicación paso a paso de cómo crear el usuario de la API de Marketo y el servicio personalizado [aquí](http://rest-api/custom-services/). Una vez finalizado, debe tener las siguientes credenciales para invocar la API de REST de Marketo: ID de cliente, Secreto de cliente, ID de cuenta de Munchkin e ID de cuenta de Munchkin

Puede obtener el ID de cuenta de Munchkin desde las pantallas de administración de Munchkin o de servicios web. Su patrón es el siguiente: `000-XXX-000`.  No es necesario obtener un token de acceso, ya que solo sería válido durante una hora. El conector generará tokens automáticamente.
**Regístrate para obtener una cuenta gratuita con Google Docs, Sheets y Slides son aplicaciones de productividad que te permiten crear diferentes tipos de documentos en línea, trabajar en ellos en tiempo real con otras personas y almacenarlos en tu Google Drive en línea. Nuestro caso de uso necesita una hoja de Google. Se pueden encontrar diferentes características de Google Docs y la creación de una cuenta con Google [aquí](https://workspace.google.com/products/docs/).
**Regístrate para obtener una cuenta gratuita con FullContact** FullContact te mantiene conectado a las personas que más te importan, al extraer todos tus contactos y sincronizarlos continuamente con cambios en perfiles sociales, fotos, firmas de correo electrónico, información de la compañía y mucho más. Ofrecen un lector de tarjetas de visita móvil que puede escanear tarjetas en más de 250 aplicaciones web, incluyendo Zapier. Puede registrarse para obtener una cuenta gratuita [aquí](https://app.fullcontact.com/login). También puedes suscribirte a una cuenta de pago premium con más funciones y capacidad. La aplicación móvil se puede descargar desde [Apple AppStore](https://apps.apple.com/us/app/fullcontact-business-card/id576780807) o desde [Google Play](https://play.google.com/store/apps/details?id=com.fullcontact.cardreader). Los Zaps de FullContact están documentados [aquí](https://zapier.com/apps/contacts-plus/integrations).

### Implementación de Marketo Connector para Zapier

**Cree la aplicación Marketo** Desde la interfaz web de Zapier, vaya al Portal para desarrolladores. Haz clic en **Agregar nueva aplicación** y rellena como mínimo el Título (por ejemplo, &quot;Marketo&quot;) y la Descripción. El logotipo es opcional, pero es agradable tenerlo.\ **Autenticación** En esta sección declaramos los diferentes campos utilizados para la autenticación de la API de REST de Marketo y la configuración de autenticación. Cree primero los campos siguientes:

Edite la &quot;Configuración de autenticación&quot;:

* Tipo de autenticación: autenticación de sesión
* Asignación de autenticación:

  `{"access_token":"{{access_token}}"}`

* Ubicación del token de acceso&#x200B;**:** Token en Querystring

Una vez creado un servicio personalizado de Marketo, el ID y el secreto de cliente están disponibles. Utilizamos el ID de cliente y el secreto de cliente para generar un token de acceso a través del extremo de la API de REST [Authentication](/help/rest-api/authentication.md). Podemos usar este token de acceso para realizar solicitudes posteriores a la API de REST. El token caduca después de una hora y debe generarse de nuevo para continuar llamando a la API de REST. Elegimos Tipo de autenticación = &quot;Autenticación de sesión&quot;, ya que nos permite ejecutar un script de autenticación personalizado cada vez que caduca nuestro token de sesión. En la sección &quot;API de scripts&quot; veremos cómo implementar este mecanismo que solo puede funcionar con este tipo de autenticación.
**Déclencheur** Los Déclencheur Zapier están ahí para traer datos a Zapier. No necesitamos uno para nuestros casos de uso, ya que en su lugar aprovecharemos un webhook de Marketo. Sin embargo, todavía tenemos que escribir un Déclencheur ficticio como prueba obligatoria para nuestro conector Marketo. Vamos a crear un Déclencheur de prueba que llame al extremo [Obtener uso diario](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getDailyUsageUsingGET) de la API de REST de Marketo. Haga clic en **Agregar nuevo Déclencheur** para iniciar el asistente y rellenar los campos siguientes (los campos no mencionados pueden dejarse en blanco): Nombre y descripción

* Nombre: Déclencheur de prueba
* Clave: test_déclencheur
* Descripción: Déclencheur de prueba de la aplicación de Marketo
* ¿Importante? Sin marcar
* ¿Esconderse? Verificado

Campos de déclencheur

* Ninguna

Procedencia de los datos

* Data Source: Sondeo
* URL de sondeo: `https://{{munchkin_account_id}}.mktorest.com/rest/v1/stats/usage.json`

Resultado de muestra

* Dejar en blanco

Haga clic en **Administrar configuración de Déclencheur** y configure el Déclencheur de prueba para que sea el que usaremos para comprobar las credenciales de un usuario. **Acciones** Acciones Zapier están ahí para enviar datos desde Zapier. Vamos a implementar la acción de posible cliente Create_Update llamando a la API de REST de Marketo. Esta acción nos permite crear un nuevo posible cliente dentro de Marketo o, si el posible cliente ya existe, lo actualiza con los valores enviados. Utilizaremos el campo &quot;correo electrónico&quot; para la deduplicación. Haga clic en **Agregar nueva acción** para iniciar el asistente y rellenar los campos siguientes (los campos no mencionados pueden dejarse en blanco): Nombre y descripción

* Nombre: Create_Update_Lead
* Nombre: Lead
* Clave: create-update-lead
* Descripción: Cree un nuevo posible cliente dentro de Marketo o, si el posible cliente ya existe, actualícelo con los valores enviados
* ¿Importante? Verificado
* ¿Esconderse? Sin marcar

Campos de acción Los campos de acción son los campos a los que los usuarios asignan los datos. Elimínelos cuidadosamente según sus propias necesidades, ya que representarán todos los datos que puede actualizar en Marketo. Hay una opción en Zapier para ofrecer al usuario final todos los campos disponibles en Marketo, pero eso induciría más código y complejidad, no necesarios para un conector desechable. A modo de ejemplo, seleccionamos los campos siguientes.

El nombre de partición es obligatorio en nuestro caso, ya que nuestra instancia de Marketo tiene particiones de posibles clientes en servicio. De lo contrario, podría omitirse. Lo separamos del grupo &quot;entrada&quot; para que el usuario final entienda que no es un campo que sincronizar. El campo &quot;Notas&quot; proviene de una sincronización entre Marketo y Salesforce, no lo utilice si no lo tiene en la instancia de Marketo. El campo &quot;Llamado&quot; se ha creado en nuestra instancia de Marketo, no lo utilice si no lo tiene en su instancia de Marketo. Por supuesto, el objetivo es permitirle elegir los campos que necesita de Marketo. Se recomienda empezar con un tamaño pequeño y añadir los campos adicionales más adelante. Dónde enviar datos

* URL de extremo de acción: `https://{{munchkin_account_id}}.mktorest.com/rest/v1/leads.json`

Resultado de muestra

* Dejar en blanco

### API de scripts Zapier

La función de scripts de Zapier le permite manipular las solicitudes y respuestas que se intercambian entre la API de su aplicación y Zapier. Puede modificar las solicitudes HTTP justo antes de enviarlas y puede analizar las respuestas antes de que Zapier haga algo con ellas. Lo necesitamos para completar nuestra autenticación personalizada &quot;Autenticación de sesión&quot; para que funcione con Marketo. Más información [aquí](https://zapier.com/developer/documentation/scripting/). Copie el siguiente código y veremos algunas explicaciones más adelante:

```javascript
var Zap = {

    get_session_info: function(bundle) {

       console.log('Entering get_session_info method ...');

         var access_token,
            access_token_request_payload,
            access_token_response;


        // Assemble the meta data for our Access Token swap request
         console.log('building Request with client_id=' + bundle.auth_fields.client_id + ', and client_secret=' + bundle.auth_fields.client_secret);
        access_token_request_payload = {
            method: 'POST',
            url: 'https://' + bundle.auth_fields.munchkin_account_id + '.mktorest.com/identity/oauth/token',
            params: {
                'grant_type' : 'client_credentials',
                'client_id' : bundle.auth_fields.client_id,
                'client_secret' : bundle.auth_fields.client_secret
            },
            headers: {
                'Content-Type': 'application/json',  // Could be anything.
                Accept: 'application/json'
            }
        };

        // Fire off the Access Token request.
        access_token_response = z.request(access_token_request_payload);

        // Extract the Access Token from returned JSON.
        access_token = JSON.parse(access_token_response.content).access_token;
        console.log('New Access_Token=' + access_token);

        // This will be mixed into bundle.auth_fields in future calls.
        //bundle.auth_fields.access_token=access_token;
        return {'access_token': access_token};
    },


    test_trigger_pre_poll: function(bundle) {

         console.log('Entering test_trigger_pre_poll method ...');

         bundle.request.params = {
         'access_token':bundle.auth_fields.access_token
         };

         return bundle.request;

    },


    test_trigger_post_poll: function(bundle) {

        console.log('Entering test_trigger_post_poll method ...');

        var data = JSON.parse(bundle.response.content);
        if ((!data.success)&&((data.errors[0].code=="601")||(data.errors[0].code=="600"))){
            console.log('Access Token expired or invalid, requesting new one - data.success=' + data.success + ', data.errors[0].code=' + data.errors[0].code);

           throw new InvalidSessionException(); // Calling get_session_info() to regenerate Access Token
        }

        return JSON.parse(bundle.response.content);
    },

    create_update_lead_pre_write: function(bundle) {

       bundle.request.params = {'access_token':bundle.auth_fields.access_token};
       return bundle.request;
    },

    create_update_lead_post_write: function(bundle) {

         var data = JSON.parse(bundle.response.content);
         if ((!data.success)&&((data.errors[0].code=="601")||(data.errors[0].code=="600"))){
            console.log('Access Token expired or invalid, requesting new one - data.success=' + data.success + ', data.errors[0].code=' + data.errors[0].code);
            throw new InvalidSessionException(); // Calling get_session_info() to regenerate Access Token
        }
        return JSON.parse(bundle.response.content);
    }
};
```

**Métodos** **get_session_info**

* Este método es responsable de generar o regenerar un token de acceso que llame al extremo [Authentication](/help/rest-api/authentication.md) de la API de REST de Marketo.
* Se llama cada vez que un método &quot;post_poll&quot; encuentra un error &quot;Token de acceso caducado&quot;. Se ha programado un token de acceso para que caduque cada 1 hora, por lo que se espera.
* URL de extremo de acción: https://{{munchkin_account_id}}.mktorest.com/identity/oauth/token

**pre_poll, pre_write**

* Debemos crear un método &quot;pre-sondeo&quot; en cualquier Déclencheur que hayamos creado para modificar la solicitud HTTP justo antes de enviarla, de modo que podamos añadir el token de acceso de Marketo en sus parámetros.
* Debemos crear un método de &quot;preescritura&quot; en cualquier acción que hayamos creado, por la misma razón.

**post_poll, post_write**

* Debemos crear un método &#39;post-sondeo&#39; en cualquier Déclencheur que hayamos creado, para analizar las respuestas antes de que Zapier haga algo con ellas, y eventualmente interceptar el error &#39;Token de acceso caducado&#39;.
* Debemos crear un método de &#39;post-escritura&#39; en cualquier Acción que hayamos creado, por la misma razón.
* Si se ha producido un error de este tipo, se genera una excepción InvalidSessionException que indica a Zapier que reproduzca la autenticación y ejecute de nuevo el método get_session_info.

Tenga en cuenta que puede acceder a los registros del paquete desde la API de scripts desde el menú &quot;Vínculos rápidos&quot; en la esquina superior derecha de la pantalla. Esto resulta muy útil para depurar los scripts.

Caso de uso 1: Integración de Marketo con la tarjeta de contacto completa Reader

Para esta integración creamos un único Zap de FullContact a Marketo. Con este Zap, usted es capaz de escanear tarjetas de visita con la tarjeta móvil FullContact Reader y empujar los posibles clientes a Marketo.   **Zap FullContact -> Marketo** Desde el panel de Zapier haga clic en el botón &#39;Hacer un nuevo Zap&#39;.

**Déclencheur en Zapier**

* Elija el contacto completo de la aplicación
* Elija el Déclencheur FullContact &#39;Nueva tarjeta de presentación&#39;
* Conéctese a su cuenta de FullContact
* Probar la aplicación FullContact

**Acción en Zapier**

* Elija el Marketo de la aplicación que acabamos de crear; debería mostrarse en Beta
* Elija la acción de Marketo &quot;Create_Update_Lead&quot;
* Conéctese a su cuenta de Marketo rellenando los parámetros de autenticación (ID de cuenta de Munchkin, ID de cliente y Secreto de cliente)
* Asignar los campos de FullContact a Marketo
* Rellene finalmente un Nombre de partición donde deben ir los nuevos posibles clientes (solo si existen particiones en la instancia de Marketo)
* Prueba de la aplicación de Marketo
* Activa tu Zap

Nota: Asegúrese de descargar las tarjetas de visita Reader desde FullContact y activar la integración de Zapier directamente desde su dispositivo móvil.

Caso de uso 2: Integración de Marketo con hojas de Google-

Para esta integración creamos dos Zaps. Uno de Marketo a Hojas de cálculo de Google y otro de Hojas de cálculo de Google a Marketo. Con este Zap, usted es capaz de sincronizar algunos de sus clientes potenciales o contactos entre Marketo y una hoja de Google.   **Zap Marketo Webhook -> Hojas de cálculo de Google**
Para el primer Zap, no dependemos de un conector personalizado para Marketo, sino que aprovechamos los Webhooks de Marketo y el Déclencheur &quot;Webhooks by Zapier&quot;. En el panel de Zapier haga clic en el botón &#39;Hacer un nuevo Zap&#39;. **Déclencheur Parte 1 en Zapier**

* Elija la aplicación de Déclencheur &#39;Webhooks by Zapier&#39;
* Marque &quot;Catch Hook&quot; que permite esperar un POST o GET a una URL de Zapier
* No hay necesidad de elegir una clave secundaria
* Zapier generó una **URL de gancho web** personalizada para que enviaras solicitudes a y la copiaras en el portapapeles

**Webhook en Marketo (pasos que debe realizar un administrador)**

* Vaya a Administración -> Webhooks
* Cree un nuevo webhook llamado &#39;Insertar posible cliente a Zapier&#39; y edite el formulario de webhook.

En el campo de la plantilla, declare todos los campos del posible cliente que desee transferir a Zapier y aproveche los tokens de Marketo. Para nuestros casos de uso, utilizamos los mismos campos que definimos para el conector Zapier personalizado que inserta posibles clientes en Marketo:

```json
{
"firstName":"{{lead.First Name}}",
"lastName":"{{lead.Last Name}}",
"email":"{{lead.Email Address}}",
"phone":"{{lead.Phone Number}}",
"leadOwner":"{{lead.Lead Owner First Name}} {{lead.Lead Owner Last Name}}",
"leadOwnerEmail":"{{lead.Lead Owner Email Address}}",
"leadNotes":"{{lead.Lead Notes:default=edit me}}",
"called":"{{lead.Called}}"
}
```

* Guarde el formulario
* No hay necesidad de una asignación de respuestas, así que ha terminado con el webhook

**Campaña de prueba en Marketo (los pasos debe realizarlos un experto en marketing o un administrador)**

* Desde las actividades de marketing, cree una nueva campaña inteligente

Con fines de prueba, vamos a crear una campaña que ponga en déclencheur nuestro webhook cada vez que un estado de posible cliente cambie a MQL. Por supuesto, puede utilizar el webhook para cualquier otro fin comercial.

* Editar la lista inteligente
* Llame al webhook en el flujo
* Programación de la campaña
* Asegúrese de que cada posible cliente pueda pasar por el flujo cada vez
* Activación de la campaña inteligente

**Déclencheur Parte 2 en Zapier**

* Para completar la aplicación de Déclencheur &quot;Webhooks by Zapier&quot;, necesitamos activar la Marketo Smart Campaign una vez y coger el webhook en Zapier
* En nuestro caso de prueba, solo tenemos que ir a la base de datos de posibles clientes de Marketo, abrir un posible cliente y cambiar su estado a &quot;MQL&quot;

**Crear la hoja de cálculo en Hojas de cálculo de Google**

* Crear nueva hoja de cálculo
* Crear una hoja de cálculo o usar la hoja predeterminada
* Agregue una columna para cada campo que desee sincronizar desde Marketo (los declarados en el webhook de Marketo)

  **Acción en Zapier**

* Elija las hojas de Google de la aplicación
* Marque la opción &quot;Crear fila de hoja de cálculo&quot;.
* Conectar con su cuenta de Google Sheets
* Seleccionar hoja de cálculo de hojas de Google
* Seleccione la hoja de cálculo
* Mapa todos los campos entre la aplicación de Déclencheur &quot;Webhooks by Zapier&quot; y las hojas de Google.

* Prueba de la aplicación de hojas de Google
* Activa tu Zap

**Zap Google Sheets -> Marketo**

En el panel de Zapier haga clic en el botón &#39;Hacer un nuevo Zap&#39;.

**Déclencheur en Zapier**

* Elija la aplicación de Déclencheur &quot;Hojas de cálculo de Google&quot;.
* Marque la opción &quot;Fila de hoja de cálculo actualizada&quot; que aparece en déclencheur cuando se agrega o modifica una nueva fila en una hoja de cálculo
* Conectar con su cuenta de Google
* Seleccione la hoja de cálculo desde la que desea realizar el déclencheur (debe ser la misma que se utilizó en el Zap anterior) y la hoja de cálculo
* Establecer columna de Déclencheur en &#39;any_column&#39;
* Prueba de la aplicación de hojas de Google

**Acción en Zapier**

* Elija el Marketo de la aplicación que acabamos de crear; debería mostrarse en Beta
* Elija la acción de Marketo &quot;Create_Update_Lead&quot;
* Conéctese a su cuenta de Marketo rellenando los parámetros de autenticación (ID de cuenta de Munchkin, ID de cliente y Secreto de cliente)
* Asignar los campos de las hojas de Google a Marketo
* Rellene finalmente un Nombre de partición donde deben ir los nuevos posibles clientes (solo si existen particiones en la instancia de Marketo)
* Prueba de la aplicación de Marketo
* Activa tu Zap

### Conclusión

Estas son algunas ideas para mejorar nuestro conector Marketo para Zapier:

* Adición de otros Déclencheur y acciones relacionados con diversos objetos de Marketo (listas, objetos personalizados, etc.)
* En lugar de codificar los campos desde Marketo, es posible extraer dinámicamente los campos de Marketo, pero eso requeriría algún trabajo de traducción técnica entre Marketo y Zapier.
* Compartir el conector con el equipo de desarrollo y, finalmente, ponerlo a disposición del público.

Es posible que Zapier implemente un adaptador Marketo Premium, lo que facilitaría la implementación de nuestros casos de uso. En cualquier caso, este artículo siempre se podría aprovechar para integrar Marketo con Zapier con un plan Zapier gratuito, y también para construir casos de uso personalizados que podrían no ser compatibles con un adaptador premium. Esperamos que haya disfrutado de este artículo y que le ayudará a tener aún más éxito con Marketo y Zapier. ¡Gracias!

Publicado el _17-04-2016_ por _David_

## Actualizaciones de primavera de 2016

**API DE REST**

* API de recursos: páginas web
   * **Las páginas de aterrizaje** ahora se exponen a través de quince nuevos puntos de conexión que permitirán la creación, actualización, eliminación, clonación y administración de borradores para páginas de aterrizaje. Las plantillas de página de aterrizaje ahora también tienen extremos de administración de borrador expuestos
      * Obtener páginas de aterrizaje
      * Obtener página de aterrizaje por ID
      * Obtener página de aterrizaje por nombre
      * Crear página de destino
      * Actualizar metadatos de página de aterrizaje
      * Obtener contenido de la página de aterrizaje
      * Sección Añadir contenido de la página de aterrizaje
      * Actualizar Sección De Contenido De Página De Aterrizaje
      * Eliminar sección de contenido de página de aterrizaje
      * Obtener sección de contenido dinámico
      * Actualizar sección de contenido dinámico
      * Descartar borrador de página de aterrizaje
      * Aprobar página de destino
      * Desaprobar borrador de página de aterrizaje
      * Eliminar página de aterrizaje
   * **Plantillas de página de aterrizaje**
      * Descartar borrador de plantilla de página de aterrizaje
      * Aprobar plantilla de página de destino
      * Desaprobar plantilla de página de aterrizaje
      * Eliminar plantilla de página de destino
   * **Forms** tiene 21 nuevos extremos publicados que proporcionan capacidades completas de creación, edición y administración a través de la API. Las API no admitirán cambios en los formularios Forms 1.0.
      * Obtener Forms
      * Obtener formulario por identificador
      * Obtener formulario por nombre
      * Obtener lista de campos de formulario
      * Actualizar lista de campos de formulario
      * Crear formulario
      * Obtener formulario de la página de agradecimiento
      * Actualizar página de agradecimiento del formulario
      * Actualizar formulario
      * Descartar borrador de formulario
      * Aprobar formulario
      * Desaprobar formulario
      * Clonar formulario
      * Eliminar formulario
      * Actualizar campo de formulario
      * Quitar campo de formulario
      * Actualizar reglas de visibilidad de campos de formulario
      * Agregar campo de formulario de texto enriquecido
      * Agregar conjunto de campos
      * Quitar campo del conjunto de campos
      * Obtener campos de formulario disponibles
      * Cambiar posiciones de campo de formulario
      * Botón Actualizar envío
   * Al usar **Obtener o Examinar programas**, se devolverá el identificador de SFDC Campaign para los programas vinculados a una campaña de SFDC

Los objetos personalizados **Custom Objects** ahora admitirán tipos de datos de área de texto, lo que permitirá almacenar campos de cadena de hasta 2000 caracteres en campos de objeto personalizados de este tipo. **Lista blanca de direcciones IP** Los usuarios administradores ahora podrán administrar una lista blanca de direcciones IP para evitar el acceso no autorizado a través de las API. [Puede leer más sobre esta característica aquí](https://experienceleague.adobe.com/es/docs/marketo/using/home). **IU de actividad personalizada** Los usuarios administradores ahora podrán definir tipos de actividades personalizadas en su menú de administración y agregar registros a los posibles clientes a través de la API [Agregar actividades personalizadas](https://developer.adobe.com/marketo-apis/api/mapi/#operation/addCustomActivityUsingPOST). [Aquí puede leer más sobre la definición de tipos de actividades personalizadas](https://experienceleague.adobe.com/es/docs/marketo/using/home).

Publicado el _2016-06-01_ por _Kenny_

## Actualizaciones del verano de 2016

Para la versión de verano de 2016 del 23 de septiembre, se lanzarán tres funciones orientadas al desarrollador.

### Compatibilidad con Email 2.0 en la API de REST

Todas las [API de recursos preexistentes](/help/rest-api/assets.md) que solo eran compatibles con los correos electrónicos y las plantillas de la versión 1.0 ahora están habilitadas para su uso con los recursos de correo electrónico de la versión 2.0.

### Insertar el cliente potencial en Marketo

[Push Lead](/help/rest-api/leads.md) es un método de sincronización de posibles clientes alternativo diseñado para facilitar la activación en campañas inteligentes. Puede crear un solo elemento de registro de actividad, asociar un posible cliente y actualizar el registro de este en una sola llamada. Esto funciona de manera similar a un solo formulario rellenado por un posible cliente, y se puede utilizar más fácilmente como método proxy para el envío de formularios en lugar de utilizar el método existente de sincronizar posibles clientes.

### Compresión HTTP

La API de REST ahora puede comprimir las respuestas utilizando el estándar definido por la especificación HTTP 1.1. Esto puede ayudar a reducir el tamaño de la respuesta, lo que aumentará la velocidad de transferencia y minimizará el uso del ancho de banda.  

Publicado el _2016-09-23_ por _Kenny_

## Uso de swagger-codegen con Marketo

[Swagger-codegen](https://github.com/swagger-api/swagger-codegen) es una potente biblioteca Java que puede generar tanto código auxiliar de servidor como clientes de API a partir de definiciones de Swagger. Esto puede aliviar drásticamente la dificultad y el coste de generar clientes para cualquier idioma específico. Para empezar y generar su primer cliente, primero querrá obtener una copia específica de una de las definiciones de Swagger de Marketo. Introduzca el Munchkin ID desde la instancia con la que desea realizar la prueba. Comience con la definición de identidad. Ahora que tiene una definición específica para su instancia, debe descargar e instalar swagger-codegen. El proceso es específico del sistema operativo y puede obtener las instrucciones [aquí](https://github.com/swagger-api/swagger-codegen#prerequisites). Con la configuración predeterminada, codegen generará un cliente que abarcará todos los extremos y modelos proporcionados. Normalmente se administran mediante una clase denominada DefaultApi, que contiene métodos para llamar a los extremos disponibles con ejemplos proporcionados en una carpeta &quot;docs&quot; (no todos los idiomas incluyen plantillas para esto de forma predeterminada). Ahora vamos a crear el primer cliente. Cree una carpeta en la que desee que el código resida y vaya allí en la sesión de terminal y utilice el comando generate para crear un cliente en el idioma que desee (supondremos que ha utilizado el método de instalación homebrew):

swagger-codegen generate -i $definitionLocation -l $yourLanguage -o $yourLocation

Esto mostrará el código de cliente en la ubicación deseada. Ahora veamos cómo se usa esto para llamar al extremo de identidad y obtener un token de acceso:

```java
 public static void main(String[] args){
  String client_id = args[0];
  String client_secret = args[1];
  ApiClient client = new ApiClient();
  DefaultApi id = new DefaultApi();
  try {
   String token = id.identityOauthTokenGet(client_id, client_secret, "client_credentials").getAccessToken();
   System.out.println(token);
  } catch (ApiException e) {
   e.printStackTrace();
  }
 }
```

```php
<?php
require_once(_DIR_ . '/vendor/autoload.php');

$api_instance = new Swagger\\Client\\Api\\DefaultApi();
$client_id = "client_id_example";
$client_secret = "client_secret_example";
$grant_type = "grant_type_example";

try {
    $result = $api_instance->identityOauthTokenGet($client_id, $client_secret, $grant_type);
    print_r($result->getAccessToken);
} catch (Exception $e) {
    echo 'Exception when calling DefaultApi->identityOauthTokenGet: ', $e->getMessage(), "\\n";
}
?>
```

```c#
  public static void Main(string[] args)
  {
      string clientId = "CHANGE ME";
      string clientSecret = "CHANGE ME";

      IdentityApi instance = new IdentityApi();
      ResponseOfIdentity response = instance.IdentityUsingGET(clientId, clientSecret, "client_credentials");

      string message = string.Format("Access Token: {0}, Expires In: {1}, Scope: {2}, Token Type: {3}",
          response.AccessToken, response.ExpiresIn, response.Scope, response.TokenType);
      Console.WriteLine(message);
  }
```

Ahora que sabemos cómo obtener autorización, analizaremos casos de uso más avanzados de clientes autogenerados en las próximas semanas.

Publicado el _2016-10-10_ por _Kenny_

## Integración de Excel, parte 1: Extraer y dar forma a datos de Marketo mediante Power Query

Este es el primero de una serie de artículos que explican cómo aprovechar la tecnología de Power BI integrada en Microsoft Excel para crear una verdadera experiencia de análisis empresarial de autoservicio con Marketo. Con los conceptos que se tratan en estos artículos, puede:

* Importación de datos de Marketo a Excel
* Importar y combinar datos de otras fuentes (aplicaciones SaaS, bases de datos, archivos planos, etc.)
* Datos de formas para fines de análisis y necesidades empresariales
* Actualizar los datos de Excel a petición
* Creación de columnas calculadas y medidas mediante fórmulas
* Crear relaciones entre datos heterogéneos
* Analizar datos y crear informes avanzados con tablas dinámicas y gráficos dinámicos
* Produzca impresionantes visualizaciones de datos

### Power Query para Excel

Este primer artículo trata sobre el proceso de importación y configuración de datos mediante la tecnología Power Query. Power Query es una tecnología de conexión de datos que le permite descubrir, conectar, combinar y refinar las fuentes de datos para satisfacer sus necesidades de análisis. Las funciones de Power Query están disponibles en Excel y Power BI Desktop. Power Query puede conectarse a muchas fuentes de datos, como bases de datos, Facebook, Salesforce, MS Dynamics CRM, etc. Marketo no es compatible de serie, pero afortunadamente podemos usar las API de REST de Marketo para la ejecución remota de muchas de las capacidades del sistema, y Power Query viene con un completo conjunto de fórmulas (informalmente conocidas como &quot;M&quot;) que le permite crear una secuencia de comandos de una fuente de datos personalizada.

### Conector personalizado

La ejecución de scripts en una sola llamada a la API de REST es trivial con Power Query, pero se vuelve más difícil gestionar los siguientes requisitos:

* Administración de tokens de acceso, incluido el mecanismo de autenticación y la actualización periódica de tokens
* Mecanismo de paginación para grandes conjuntos de datos
* Control de errores

Este artículo explica cómo crear un conector personalizado robusto que pueda consumir las API de REST de Marketo para extraer todo tipo de datos (posibles clientes, actividades, objetos personalizados, programas, etc.). La única restricción se reduce al límite diario de solicitudes de la API de Marketo. Los conceptos que se explican aquí se centran en Marketo, pero también podrían utilizarse para integrar otras soluciones de SaaS que proporcionan una API de REST.

### Requisitos previos

#### Power Query

Antes de la versión de Excel 2016, Microsoft Power Query para Excel funcionaba como un complemento de Excel que se descargaba e instalaba en Excel 2010 o Excel 2011. A partir de Excel 2016, esta tecnología es una función nativa integrada en la cinta &quot;Datos&quot; de la sección &quot;Obtener y transformar&quot;. Todos los scripts producidos para este artículo se han probado en Excel 2016 para Windows. Los conceptos deben ser los mismos para Excel 2013 o Excel 2010, pero podrían requerirse algunas adaptaciones.  Actualmente, Power Query solo está disponible en la versión de Excel para Microsoft Windows; lamentablemente, la versión de Mac no es compatible.

#### Marketo

Power Query utilizará las API de REST de Marketo para acceder a los datos de Marketo. Para utilizar estas API, necesita un usuario de API y un servicio personalizado que pueda crear usted mismo si es el administrador de su instancia de Marketo. Si no es así, un administrador deberá proporcionárselos. Encontrará una explicación paso a paso de cómo crear el usuario de la API de Marketo y el servicio personalizado [aquí](/help/rest-api/custom-services.md). Una vez finalizado, debe tener las siguientes credenciales para invocar las API de REST de Marketo: **ID de cliente** y **Secreto de cliente**. El **extremo de API REST** se encuentra en la sección de API REST del administrador de servicios web de Marketo y debe tener el siguiente patrón:

`<https://XXX-XXX-XXX.mktorest.com/rest>`

Marketo tiene un límite de solicitudes diarias para su API, que se puede encontrar en el Administrador de servicios web junto con un informe de consumo. **Asegúrese de no exceder nunca el límite diario al diseñar las consultas, ya que es posible que se pierdan algunos datos en los informes**.

### Creación de libro de Power Query

Empecemos con un nuevo libro de Excel. Creamos una hoja de cálculo de configuración específica para declarar todos los ajustes de la API de REST de Marketo. En esta hoja de cálculo, se crean tres tablas:

Tabla &#39;**REST_API_Authentication**&#39; con las columnas: **URL**: su punto final de la API REST de Marketo. **ID de cliente**: desde su credencial OAuth2.0 de la API de REST de Marketo. **Secreto de cliente**: de su credencial OAuth2.0 de la API de REST de Marketo.
Tabla &#39;**Ámbito**&#39; con las columnas: **Token de paginación SinceDatetime**: una fecha posterior a la notación de fecha estándar ISO 8601 (por ejemplo, &quot;2016-10-06T13:22:17-08:00&quot;, &quot;2016-10-06&quot; son fecha/hora válida) que se utiliza para recuperar actividades de Marketo desde un período determinado, gracias a un token de paginación inicial &#39;basado en fecha&#39;. Esta fecha se utiliza principalmente para limitar la cantidad de datos que se importarán en el libro. **ID de lista**: el ID de una lista estática en Marketo que hace referencia a todos los posibles clientes o contactos con los que estamos tratando. Esta lista estática se puede administrar libremente en Marketo (por ejemplo, una campaña inteligente puede alimentarla periódicamente o en tiempo real con posibles clientes y contactos).
Para obtener el ID de una lista estática, ábrala en Marketo y obtenga su ID numérico de la URL, por ejemplo `<https://myorg.marketo.com/#ST3517A1LA1>`, List ID=3511. **Páginas de registros máximos**: se usa para nuestros algoritmos seudorecursivos que iteran a través de los datos de salida de Marketo, usando tokens de paginación &quot;basados en posiciones&quot;, con una capacidad de 300 registros máximos por página. Dado que este es nuestro interés para obtener tantos registros por página como sea posible, nos atenemos a 300. Por lo tanto, normalmente, una página de registros máxima establecida en 33.333 significa una capacidad de 33.333 X 300 = 9.9999 millones de registros; pero también significa 33.333 K en el límite de solicitudes diarias de la API de Marketo. Los algoritmos se detendrán de todos modos en cuanto se obtengan todos los datos de las consultas, por lo que este parámetro es solo un límite de seguridad para un bucle.

Tabla `Leads` con la columna: **Campos de posibles clientes**: campos de posibles clientes separados por comas que se recopilarán de Marketo al consultar los posibles clientes y contactos. Declarar una tabla en Excel es sencillo. Introduzca dos filas en la hoja de cálculo con los nombres y valores de las columnas, resalte con el ratón el perímetro de la tabla y seleccione el icono Tabla en el menú &quot;Insertar&quot; y, a continuación, asígnele un nombre. Los nombres que se dan a las tablas y sus columnas son importantes, ya que nuestras secuencias de comandos las llaman directamente.

## Token de autenticación y acceso

### Acerca de la autenticación de API REST de Marketo

Las API de REST de Marketo están autenticadas con OAuth 2.0 de 2 patas. Los ID y los secretos de cliente los proporcionan servicios personalizados definidos por el usuario. Cada servicio personalizado es propiedad de un usuario solo de API que tiene un conjunto de funciones y permisos que autorizan al servicio a realizar acciones específicas. Un token de acceso está asociado a un único servicio personalizado.
El mecanismo de autenticación completo está documentado [aquí](/help/rest-api/authentication.md) en el sitio para desarrolladores de Marketo. Cuando se crea un token de acceso originalmente, su duración es de 3600 segundos o 1 hora. Cada llamada de autenticación consecutiva para el mismo servicio personalizado devuelve el token de acceso actual con la duración restante. Una vez caducado el token, la autenticación devuelve un token de acceso completamente nuevo. La administración de la caducidad del token de acceso es importante para garantizar que la integración funcione correctamente y evitar que se produzcan errores de autenticación inesperados durante el funcionamiento normal.

#### Crear consulta

Cree una nueva consulta haciendo clic en el icono &quot;Nueva consulta&quot; de la sección &quot;Get&amp;Transform&quot; del menú &quot;Data&quot;. Seleccione una consulta en blanco para empezar y asígnele un nombre como &quot;MktoAccessToken&quot;. Inicie el Editor avanzado desde el Editor de consultas para poder crear manualmente secuencias de comandos de algunas fórmulas de Power Query. Introduzca el siguiente código en el editor avanzado:

```
let
    // Get url and credentials from config worksheet - Table REST_API_Authentication
    mktoUrlStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[URL],
    clientIdStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[Client ID],
    clientSecretStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[Client Secret],

    // Calling Marketo API Get Access Token
    getAccessTokenUrl = mktoUrlStr & "/identity/oauth/token?grant_type=client_credentials&client_id=" & clientIdStr & "&client_secret=" & clientSecretStr,
    TokenJson = try Json.Document(Web.Contents(getAccessTokenUrl)) otherwise "Marketo REST API Authentication failed, please check your credentials",

    // Parsing access token
    accessTokenStr = TokenJson [access_token]

in
    accessTokenStr
```

Los comentarios incrustados en el código fuente, precedidos de &quot;//&quot;, hacen que el código se explique por sí mismo. Si necesita alguna referencia a una función, consulte los vínculos que aparecen en la sección Referencia de este artículo. Haga clic en el botón &quot;Listo&quot;. Compruebe que el token de acceso se muestre correctamente en la salida para el paso final aplicado &quot;accessTokenStr&quot;.  Un comentario rápido sobre la seguridad en Excel; en ocasiones se le puede pedir que habilite Conexiones de datos externas desde el aviso amarillo. Esto es necesario para que las consultas funcionen correctamente.

#### Convertir una consulta en una función

Vuelva al Editor avanzado y ajuste el código con la siguiente declaración de función:

```
let
    FnMktoGetAccessToken =()=>

        let
            // Get url and credentials from config worksheet - Table REST_API_Authentication
            mktoUrlStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[URL],
            clientIdStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[Client ID],
            clientSecretStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[Client Secret],

            // Calling Marketo API Get Access Token
           getAccessTokenUrl = mktoUrlStr & "/identity/oauth/token?grant_type=client_credentials&client_id=" & clientIdStr & "&client_secret=" & clientSecretStr,
           TokenJson = try Json.Document(Web.Contents(getAccessTokenUrl)) otherwise "Marketo REST API Authentication failed, please check your credentials",

            // Parsing access token from Json
           accessTokenStr = TokenJson [access_token]

        in
            accessTokenStr

in FnMktoGetAccessToken
```

La función no toma ningún parámetro en la entrada pero los obtiene de la hoja de cálculo de configuración. Produce el token de acceso como salida. Cambie el nombre de la consulta `FnMktoGetAccessToken` y guárdela. Tenga en cuenta que puede ver todas las consultas en cualquier momento en Excel haciendo clic en el botón &quot;Mostrar consultas&quot; en la sección &quot;Obtener y transformar&quot; del menú Datos. La función debe marcarse con el icono de función &quot;Fx&quot;.

### Cargar miembros de la lista estática

#### Obtener posibles clientes

La API de Marketo Lead proporciona operaciones sencillas de CRUD con registros de posibles clientes, la capacidad de modificar la pertenencia de un posible cliente a listas y programas estáticos e iniciar el procesamiento de campañas inteligentes para posibles clientes. Todas estas capacidades están documentadas [aquí](/help/rest-api/leads.md). Se puede recuperar un gran conjunto de registros de posibles clientes en función de la pertenencia a una lista estática o a un programa. Con el ID de una lista estática, puede recuperar todos los registros de posibles clientes que sean miembros de esa lista estática. El ID de la lista es un parámetro de ruta en la llamada. Consulte el capítulo &quot;Lista y pertenencia a programas&quot; en la documentación para desarrolladores de Marketo para obtener más información. El número máximo de registros de posibles clientes que podemos obtener por llamada de API es 300, por lo que necesitamos aprovechar los tokens de paginación para recopilar los registros por páginas de 300 registros. Obtenemos el token de paginación en la respuesta JSON después de la primera llamada y sabemos que hemos terminado cuando el token de paginación ya no está en la salida.

### Consulta básica

Empecemos con una consulta que funcione completamente y que apunte a descargar todos los posibles clientes de una lista estática. Cree una nueva consulta en blanco llamada &quot;MktoLeads&quot; e introduzca el siguiente código en el editor avanzado:

```
let
    // Get Url from config worksheet - Table REST_API_Authentication
    mktoUrlStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[URL],
    // Get the number of iterations (pages of 300 records) - Table Scoping
    iterationsNum = Excel.CurrentWorkbook(){[Name="Scoping"]}[Content]{0}[Max Records Pages],
    // Get the List id - Table Scoping
    listIdStr = Number.ToText(Excel.CurrentWorkbook(){[Name="Scoping"]}[Content]{0}[List ID], "D", ""),
    // Get the Lead fields to extract - Table Leads
    LeadFieldsStr = Excel.CurrentWorkbook(){[Name="Leads"]}[Content]{0}[Lead Fields],


    // Build Multiple Leads by List Id URL
    getMultipleLeadsByListIdUrl = mktoUrlStr & "/rest/v1/list/" & listIdStr & "/leads.json?fields=" & LeadFieldsStr,

    // Build Marketo Access Token URL parameter
    accessTokenParamStr = "&access_token=" & FnMktoGetAccessToken(),

    pagingTokenParamStr = "",

    // Function iterating though the pages
    FnProcessOnePage =
    (accessTokenParamStr, pagingTokenParamStr) as record =>
        let

            // Send REST API Request
            content = Web.Contents(getMultipleLeadsByListIdUrl & accessTokenParamStr & pagingTokenParamStr),

            // Recover Json output and watch if token is expired, in that case, regenerate access token
            newAccessTokenParamStr = if Json.Document(content)[success]=true then accessTokenParamStr else "?access_token=" & FnMktoGetAccessToken(),
            getMultipleLeadsByListIdJson = if Json.Document(content)[success]=true then Json.Document(content) else Json.Document(Web.Contents(getMultipleLeadsByListIdUrl & newAccessTokenParamStr & pagingTokenParamStr)),

            // Parse Json outputs: data and next page token
            data = try getMultipleLeadsByListIdJson[result] otherwise null,
            next  = try  "&nextPageToken=" & getMultipleLeadsByListIdJson[nextPageToken] otherwise null,
            res = [Data=data, Next=next, Access=newAccessTokenParamStr]
        in
            res,

    // Generates a list of values given four functions that generate the initial value initial, test against a condition, and if successful select the result and generate the next value next. An optional parameter, selector, may also be specified
    GeneratedList =
        List.Generate(
            ()=>[i=0, res = FnProcessOnePage(accessTokenParamStr, pagingTokenParamStr)],
            each [i]<iterationsNum and [res][Data]<>null,
            each [i=[i]+1, res = FnProcessOnePage([res][Access],[res][Next])],
            each [res][Data])
in
    GeneratedList
```

Power Query no ofrece funciones de bucle tradicionales (p. ej. For-loop, While-loop) y no admite la recursión. Una buena solución es implementar un bucle For mediante List.Generate. Esta función está documentada [aquí](http://msdn.microsoft.com/query-bi/m/list-generate). Con lista. Generar: es posible iterar en las páginas. En cada paso de la iteración extraemos una página de datos, conservando la dirección URL que incluye el token de paginación para la página siguiente, y almacenamos los resultados en el elemento siguiente de la lista generada. El blog de [Datachant](https://datachant.com/2016/06/27/cursor-based-pagination-power-query/) fue un excelente recurso para resolver esto. Nuestro parámetro &quot;Máximo de páginas de registros&quot; está aquí para limitar el número de páginas y restringirlo a un rango realista evitando un bucle infinito. Otro desafío es garantizar que el token de acceso nunca caduque. Rastrear la duración restante sería demasiado complejo con Power Query. Por lo tanto, se realiza una copia de seguridad de todas las llamadas a la API de REST con una comprobación de errores. Si se produce un error, suponemos que el token ha caducado y lo renovamos primero y reproducimos la llamada de nuevo. Si falla la segunda llamada, el segundo error se notificará a Excel (en el peor de los casos, no obtendrá datos como resultado). Inicie la consulta después de guardarla o haciendo clic en el botón Actualizar en cualquier momento.  En nuestro caso, se extrajeron 1364 registros de posibles clientes encajando en 5 páginas de datos dentro de 5 listas.

### Dar forma a los datos

Necesitamos dar forma a los datos para tener todos estos registros en una sola lista plana de registros. Hay dos formas de hacerlo:

* Uso de más código
* Aprovechamiento de la IU de Power Query

Haga clic con el botón derecho en la cuadrícula de salida y elija &quot;A la tabla&quot; en el menú contextual para convertirla en una tabla de listas.  En la ventana emergente &quot;A la tabla&quot;, deje los valores predeterminados en las dos listas de selección.  Ahora expanda la tabla de listas resultante. Ahora tenemos todos los registros en una sola lista. Los registros codificados en formato Json contienen los campos y sus valores asociados. Expandir de nuevo. Seleccione en la ventana emergente todos los campos que desee conservar y desmarque la casilla de verificación Usar el nombre de columna original como prefijo.  ¡Et voila! Todos los registros se muestran bien en nuestra tabla. Si volvemos a abrir el editor avanzado, podemos ver que se han agregado 3 líneas de código para dar forma a los datos:

```
# "Converted to Table" = Table.FromList(GeneratedList, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
# "Expanded Column1" = Table.ExpandListColumn(#"Converted to Table", "Column1"),
# "Expanded Column2" = Table.ExpandRecordColumn(#"Expanded Column1", "Column1", {"id", "updatedAt", "lastName", "email", "createdAt", "firstName"}, {"id", "updatedAt", "lastName", "email", "createdAt", "firstName"})
```

Puede hacer mucho más con Power Query, como crear columnas adicionales con valores calculados; veremos algunas posibilidades más adelante. Vamos a guardar y cerrar esta consulta. Ahora se puede actualizar manualmente en cualquier momento o automáticamente mediante actualizaciones en segundo plano.

### Redireccionamiento de los resultados

Ahora la pregunta es, ¿dónde enviar los datos de resultados? Pase el ratón sobre la consulta y seleccione el menú &quot;Cargar en...&quot; en el menú contextual. En la ventana emergente, puede seleccionar:

* &#39;Tabla&#39; si desea enviar todos los datos con forma a una hoja de cálculo (nueva o existente),
* &quot;Crear conexión únicamente&quot; si su objetivo es realizar un análisis más detallado en Power Pivot.

La casilla de verificación &quot;Agregar esto al modelo de datos&quot; permite aprovechar los datos de Power Pivot; esto es lo que queremos para la segunda parte de este artículo.

### Administración de paginación

Dado que el objetivo de nuestro proyecto es crear muchas más consultas, vamos a hacer algo de refactorización y extraer una función reutilizable que administre la paginación. Cree una nueva consulta en blanco llamada **FnMktoGetPagedData** e introduzca el código siguiente en el editor avanzado:

```
let
    FnMktoGetPagedData =(url, accessTokenParamStr, pagingTokenParamStr)=>

    let

        // Get the number of iterations (pages of 300 records) - Table Scoping
        iterationsNum = Excel.CurrentWorkbook(){[Name="Scoping"]}[Content]{0}[Max Records Pages],

        // Sub-function iterating though the REST API service result pages
        FnProcessOnePage =
        (accessTokenParamStr, pagingTokenParamStr) as record =>
            let

                // Send REST API Request
                content = Web.Contents(url& accessTokenParamStr & pagingTokenParamStr),

                // Recover Json output and watch if token is expired, in that case, regenerate access token
                newAccessTokenParamStr = if Json.Document(content)[success]=true then accessTokenParamStr else "?access_token=" & FnMktoGetAccessToken(),
                contentJson = if Json.Document(content)[success]=true then Json.Document(content) else Json.Document(Web.Contents(url & newAccessTokenParamStr & pagingTokenParamStr)),

                // Parse Json outputs: data and next page token
                data = try contentJson[result] otherwise null,
                next  = try  "&nextPageToken=" & contentJson[nextPageToken] otherwise null,
                res = [Data=data, Next=next, Access=newAccessTokenParamStr]
            in
                res,

        // Generates a list of values given four functions that generate the initial value initial, test against a condition, and if successful select the result and generate the next value next. An optional parameter, selector, may also be specified
        GeneratedList =
            List.Generate(
                ()=>[i=0, res = FnProcessOnePage(accessTokenParamStr, pagingTokenParamStr)],
                each [i]<iterationsNum and [res][Data]<>null,
                each [i=[i]+1, res = FnProcessOnePage([res][Access],[res][Next])],
                each [res][Data])
    in
        GeneratedList

in FnMktoGetPagedData
```

Guarde la consulta. Vamos a usarlo a continuación.

### Consulta simplificada

Vamos a volver a escribir nuestra consulta &quot;MktoLeads&quot;, que llamará a la función **FnMktoGetPagedData**.

```
let
    // Get Url from config worksheet - Table REST_API_Authentication
    mktoUrlStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[URL],
    // Get the List id - Table Scoping
    listIdStr = Number.ToText(Excel.CurrentWorkbook(){[Name="Scoping"]}[Content]{0}[List ID], "D", ""),
    // Get the Lead fields to extract - Table Leads
    LeadFieldsStr = Excel.CurrentWorkbook(){[Name="Leads"]}[Content]{0}[Lead Fields],


    // Build Multiple Leads by List Id URL
    getMultipleLeadsByListIdUrl = mktoUrlStr & "/rest/v1/list/" & listIdStr & "/leads.json?fields=" & LeadFieldsStr,

    // Build Marketo Access Token URL parameter
    accessTokenParamStr = "&access_token=" & FnMktoGetAccessToken(),

    // No initial paging token required for this call
    pagingTokenParamStr = "",

    // Invoke the multiple REST API calls through the FnMktoGetPagedData function
    result = FnMktoGetPagedData (getMultipleLeadsByListIdUrl , accessTokenParamStr, pagingTokenParamStr)

in
    result
```

Como puede ver, nuestra consulta ahora es muy fácil de leer y mantener. Vamos a aprovechar nuevamente la función **FnMktoGetPagedData** para las demás consultas.

### Cargar actividades específicas desde un período de tiempo definido

#### obtener actividades con paginación

Marketo permite una gran variedad de tipos de actividades relacionadas con los registros de posibles clientes. Casi todos los pasos de cambio, acción o flujo se registran en el registro de actividad de un posible cliente y se pueden recuperar mediante la API o aprovechar en los déclencheur y filtros de listas inteligentes y campañas inteligentes. Las actividades siempre se relacionan con el registro de posibles clientes a través de leadId, correspondiente al ID del registro, y también tienen un ID entero único propio. Encontrará la documentación completa de la API de REST [aquí](/help/rest-api/activities.md).

Existen un gran número de tipos de actividades potenciales, que pueden variar de una suscripción a otra y tienen definiciones únicas para cada uno. Aunque cada actividad tendrá su propio ID único, leadId y activityDate, primaryAttributeValueId y primaryAttributeValue variarán en su significado. Nos vamos a centrar en los momentos interesantes, un tipo de actividades rastreadas por Marketo con el ID 41. Los nuevos desafíos que vamos a resolver son:

* Necesitamos iniciar un token de paginación &quot;basado en la fecha&quot; para definir el período de tiempo en el que se produjeron las actividades,
* La forma de los datos es un poco más complicada, ya que, según los tipos de actividad, se proporciona una lista de atributos específicos de la actividad en JSON y deben analizarse y aplanarse para facilitar el análisis.

#### Token de paginación basado en fecha

Necesitamos generar primero esta función para generar el token de paginación inicial &quot;basado en la fecha&quot;, necesario para abarcar el período de tiempo para nuestras consultas de actividad. Encontrará la documentación sobre el token de paginación [aquí](/help/rest-api/paging-tokens.md). Cree una nueva consulta en blanco llamada **FnMktoGetPagingToken** e introduzca el siguiente código en el editor avanzado:

```
let
    FnMktoGetPagingToken =(accessTokenStr)=>

        let
            // Get url from config worksheet - Table REST_API_Authentication
            mktoUrlStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[URL],

            // Get Paging Token SinceDatetime from config worksheet - Table Scoping
            mktoPTSinceDatetimeStr = DateTime.ToText(Excel.CurrentWorkbook(){[Name="Scoping"]}[Content]{0}[Paging Token SinceDatetime], "yyyy-MM-ddThh:mm:ss"),

            // Building URL for API Call
            getPagingTokenUrl = mktoUrlStr & "/rest/v1/activities/pagingtoken.json?access_token=" & accessTokenStr & "&sinceDatetime=" & mktoPTSinceDatetimeStr,

            // Calling Marketo API Get Paging Token
            content = Web.Contents(getPagingTokenUrl),

            // Recover Json output and watch if access token is expired, in that case, regenerate it
            newAccessTokenStr = if Json.Document(content)[success]=true then accessTokenStr else "?access_token=" & FnMktoGetAccessToken(),
            pagingTokenJson = if Json.Document(content)[success]=true then Json.Document(content) else Json.Document(Web.Contents(mktoUrlStr & "/rest/v1/activities/pagingtoken.json?access_token=" & newAccessTokenStr & "&sinceDatetime=" & mktoPTSinceDatetimeStr)),

            // Parsing Paging Token
            pagingTokenStr = pagingTokenJson[nextPageToken]

        in
            pagingTokenStr

in FnMktoGetPagingToken
```

Guarde la función. Vamos a usarlo a continuación.

#### Actividades de momentos interesantes

Ahora vamos a escribir la consulta &#39;MktoInterestingMomentsActivities&#39; que llamará a las funciones **FnMktoGetPagedData** y **FnMktoGetPagingToken**.

```
let

    // Get Url from config worksheet - Table REST_API_Authentication
    mktoUrlStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[URL],
    // Get the List id - Table Scoping
    listIdStr = Number.ToText(Excel.CurrentWorkbook(){[Name="Scoping"]}[Content]{0}[List ID], "D", ""),

    // Build Get Activities URL
    getActivitiesUrl = mktoUrlStr & "/rest/v1/activities.json?ListId=" & listIdStr & "&activityTypeIds=46",

    // Build Marketo Access Token URL parameter
    accessTokenStr = FnMktoGetAccessToken(),
    accessTokenParamStr = "&access_token=" & accessTokenStr,

    // Obtain date-based paging token used to scope in time the activities
    pagingTokenParamStr = "&nextPageToken=" & FnMktoGetPagingToken(accessTokenStr),

    // Invoke the multiple REST API calls through the FnMktoGetPagedData function
    result = FnMktoGetPagedData (getActivitiesUrl , accessTokenParamStr, pagingTokenParamStr)

in
    result
```

El resultado de esta consulta es de nuevo una lista de listas, por lo que necesita algún procesamiento de datos adicional para poder utilizarse en el análisis.

### Dar forma a los datos

Hagamos las mismas operaciones de modelado que hicimos para los Leads:

* Haga clic con el botón derecho en la cuadrícula de salida y elija &quot;A la tabla&quot; en el menú contextual para convertirla en una tabla de listas,
* Expanda la tabla de listas resultante,
* Expanda una vez más, seleccionando en la ventana emergente todos los campos que desee conservar (desmarque la casilla de verificación &quot;Usar nombre de columna original como prefijo&quot;).

Puede ver las columnas con sus valores, excepto la columna &quot;atributos&quot; que aún contiene una lista de atributos específicos asociados a los momentos interesantes. Vamos a expandir estos atributos. Ahora la lista se ha expandido en registros, ampliamos nuevamente, seleccionando los campos que queremos (nombre y valor de cada atributo) y desmarcamos la casilla de verificación &quot;usar nombre de columna original como prefijo&quot;.  Como resultado, todos nuestros datos son visibles, incluidos los atributos, pero cada actividad de momento interesante se expande en 3 líneas. Esto será difícil de usar para nuestro análisis.  Lo ideal es que solo deseemos una línea por actividad, con todos los atributos mostrados como columnas adicionales. Podemos hacerlo fácilmente girando los 3 atributos de nuestra tabla. Seleccione las 2 columnas &quot;Nombre&quot; y &quot;Valor&quot; de los atributos de actividad y haga clic en &quot;Columna dinámica&quot; en el menú &quot;Transformar&quot;. Pida las opciones de anticipos en la ventana emergente y seleccione la función &quot;Columna de valores&quot; = valor y &quot;No acumular&quot; valor.  Haga clic en &quot;Aceptar&quot; y tendrá que generar una sola línea de datos por actividad.  Las siguientes líneas de código de &quot;forma de datos&quot; deberían haberse anexado automáticamente a la secuencia de comandos de la consulta:

```
  #"Converted to Table" = Table.FromList(result, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
    #"Expanded Column1" = Table.ExpandListColumn(#"Converted to Table", "Column1"),
    #"Expanded Column2" = Table.ExpandRecordColumn(#"Expanded Column1", "Column1", {"id", "leadId", "activityDate", "activityTypeId", "campaignId", "primaryAttributeValue", "attributes"}, {"id", "leadId", "activityDate", "activityTypeId", "campaignId", "primaryAttributeValue", "attributes"}),
    #"Expanded attributes" = Table.ExpandListColumn(#"Expanded Column2", "attributes"),
    #"Expanded attributes1" = Table.ExpandRecordColumn(#"Expanded attributes", "attributes", {"name", "value"}, {"name", "value"}),
    #"Pivoted Column" = Table.Pivot(#"Expanded attributes1", List.Distinct(#"Expanded attributes1"[name]), "name", "value")
in
    #"Pivoted Column"
```

### Pasos siguientes

Ahora debería poder diseñar todas las consultas que necesite para acceder a cualquier dato específico de Marketo disponible a través de sus API de REST. Esperamos que haya disfrutado de este artículo y que le haya ayudado a aprovechar las grandes ventajas de Excel y Marketo juntos. En el segundo artículo también se proporciona un libro de ejemplo con todas las consultas.

### Referencias

#### Power Query

* [Power Query: información general y aprendizaje](https://support.microsoft.com/en-us/article/Power-Query-Overview-and-Learning-ed614c81-4b00-4291-bd3a-55d80767f81d)
* [Referencia de fórmula de consulta avanzada](http://msdn.microsoft.com/query-bi/m/power-query-m-reference)
* [Matt Masson Blog](http://www.mattmasson.com/tag/power-query/) proporciona algunos buenos recursos sobre Power Query
* [El blog DataChant](https://datachant.com/2016/06/27/cursor-based-pagination-power-query/) es muy útil para implementar el mecanismo de paginación

Publicado el _18-10-2016_ por _Philippe_

## Actualizaciones de otoño de 2016

En la versión de otoño de 2016, se ha añadido la compatibilidad con CRUD para las variables y los módulos de Email v2 y la compatibilidad con CRUD para las cuentas con nombre. Ahora puede leer y editar localmente el contenido mediante las API de REST de Marketo, así como moverlo, reordenarlo y eliminarlo. Consulte la lista completa de actualizaciones a continuación:

### API de base de datos de clientes potenciales

* [**Cuentas con nombre**](/help/rest-api/named-accounts.md)
   * Nuevos extremos para leer, actualizar y eliminar cuentas con nombre
   * Problemas conocidos:
      * A partir de la versión de otoño de 2016, los posibles clientes no se pueden asociar a cuentas con nombre mediante la API

### Recurso API

* [**Correo electrónico**](https://developer.adobe.com/marketo-apis/api/asset/#operation/describeUsingGET_5)
   * Nuevos extremos para manipular las variables de Email v2
   * Nuevos extremos para manipular los módulos del correo electrónico v2
   * Problemas conocidos:
      * Las consultas y actualizaciones de secciones que contienen tokens predictivos devolverán un error
      * Es posible que los correos electrónicos con secciones de contenido que contienen tokens predictivos no se aprueben mediante la API

Publicado el _2016-12-07_ por _Kenny_

## Por qué nos retiramos @MarketoDev Twitter Handl3

Hemos decidido retirar el identificador de @MarketoDev en Twitter. La cuenta se desactivará el 9 de diciembre de 2011. ¿Por qué? **Retroceder hasta principios de 2014...** Acabábamos de lanzar el Sitio para desarrolladores de Marketo para ayudar a los desarrolladores a compilar en función de nuestras API. Queríamos aumentar el conocimiento de la plataforma de Marketo y reducir las barreras de entrada para los desarrolladores. Junto con ese esfuerzo, creamos @MarketoDev para interactuar con nuestros socios y clientes tecnológicos que se inclinaban por crear soluciones integradas con Marketo. Como cualquier nueva empresa valiente, no estábamos seguros de cómo se desarrollaría esto. Inicialmente, tuiteamos nuevas publicaciones de blog y nuevas versiones de API. Las cosas estaban bien; el tráfico al sitio de desarrolladores subió! También comenzamos a recibir una variedad de preguntas, las cuales respondimos diligentemente. **Avance rápido hasta finales de 2016...** A medida que el ecosistema de [LaunchPoint Partner](https://exchange.adobe.com/apps/browse/ec?product=MRKTO) crecía, la cantidad de actividad de tweet aumentaba. Responder al volumen de preguntas publicadas en @MarketoDev se había convertido en un gran trabajo para nuestro pequeño equipo. Recibimos una cantidad cada vez mayor de preguntas generales de productos y ventas, que redirigimos a @Marketo o @MarketoCares. También descubrimos que 140 caracteres simplemente no eran suficientes para preguntas relacionadas con el desarrollo. Responder a estas preguntas a menudo condujo a largas e involucradas conversaciones, un enfoque que no se escaló. Y finalmente, analizamos las fuentes de tráfico para los visitantes del sitio de desarrolladores y encontramos que la mayoría provienen de la búsqueda orgánica, y la funcionalidad &quot;Suscribirse ahora&quot; en nuestro blog. Por estas razones, decidimos tirar del enchufe en @MarketoDev. **A partir de ahora...** Si eres fan de Twitter (y no lo eres), ¡no temas! Nuestro manejo corporativo de Twitter @Marketo perdura; al igual que nuestro servicio de atención al cliente gestiona @MarketoCares.

Publicado el _2016-12-02_ por _David_

## Integración con Excel, parte 2: Creación de informes avanzados de Marketo y visualizaciones de datos mediante Power Pivot y Power View-

Este es el segundo de una serie de dos artículos que explican cómo aprovechar la tecnología de Power BI integrada en Microsoft Excel para crear una verdadera experiencia de análisis empresarial de autoservicio con Marketo. Con los conceptos que se tratan en estos artículos, puede:

* Importación de datos de Marketo a Excel
* Importar y combinar datos de otras fuentes (aplicaciones SaaS, bases de datos, archivos planos, etc.)
* Datos de formas para necesidades comerciales y fines de análisis
* Actualizar datos bajo demanda en Excel
* Creación de columnas calculadas y medidas mediante fórmulas
* Crear relaciones entre datos heterogéneos
* Analizar datos y crear informes avanzados con tablas dinámicas y gráficos dinámicos
* Produzca impresionantes visualizaciones de datos

### Power Pivot y Power View para Excel

En este artículo, proporcionamos ejemplos de cómo crear lo siguiente:

* Informes avanzados de Marketo que aprovechan las relaciones entre diferentes colecciones de datos de Marketo mediante Power Pivot
* Visualizaciones estáticas y animadas interesantes con Power View

**Power Pivot** es un complemento de Excel, ya incluido en Excel 2016, que puede usar para realizar análisis de datos eficaces y crear modelos de datos sofisticados. Con Power Pivot, puede combinar grandes volúmenes de datos de varias fuentes, realizar análisis de información rápidamente y compartir perspectivas fácilmente. Los datos extraídos de diferentes fuentes de datos con Power Query se pueden enviar al modelo de datos, a la hoja de cálculo de Excel o a ambos. En el primer artículo, importamos y aplicamos forma a los datos de Marketo y los enviamos al modelo de datos para realizar un análisis más sofisticado antes de que estuvieran disponibles en la hoja de cálculo. **Power View** es una alternativa a la capa de visualización de Excel. Se trata de una experiencia de exploración, visualización y presentación de datos interactiva que fomenta la creación de informes y el uso del tablero intuitivos y específicos.

Todos los pasos explicados en este artículo se han probado en Excel 2016 para Windows. Los conceptos deben ser los mismos para Excel 2013 o Excel 2010 (sin Power View), pero podrían requerirse algunas adaptaciones. Power Pivot y Power View solo están disponibles actualmente en la versión de Excel 2016 de Microsoft Windows, la versión de Excel 2016 de Office 365 no admite totalmente Power Pivot y Power View.

### El libro de Marketo Power

#### Descargar libro

En el primer artículo, tratamos el proceso de importación y configuración de datos mediante la tecnología Power Query. Hemos aprendido a implementar algunas consultas de energía avanzadas para extraer posibles clientes y actividades de Marketo. Porque algunos querrán ir directamente al punto en el que crean informes y visualizaciones, sin codificación. Este libro contiene todas las consultas detalladas en el primer artículo y algunas más. Hemos mejorado la gestión de errores y hemos añadido algunos parámetros adicionales en la hoja de cálculo de configuración. Si ha leído el primer artículo, le recomendamos que descargue el libro de Marketo Power y que compruebe lo que se ha agregado.

Descargo de responsabilidad: el libro de trabajo de Marketo Power no es un producto oficial de Marketo y, por lo tanto, no es compatible con Marketo. Siéntase libre de usar y ampliar para sus necesidades personales de negocios, pero hágalo bajo su propio riesgo.

#### Configurar libro

Rellene toda la información necesaria de la hoja de cálculo de configuración de Marketo:

* Se requiere **autenticación de API de REST de Marketo:**
* **Ámbito:** establezca el token de paginación SinceDatetime y el identificador de su lista estática de Marketo que contiene todos los posibles clientes que desea analizar
* **Posibles clientes:** para los informes futuros, debe especificar al menos los siguientes campos de Posible cliente: `id`, `firstName`, `lastName`, `email`, crear `edAt`, `updatedAt`, `title`, `company`, `industry`, `inferredCountry`, `inferredCity`
   * Si la información de la ciudad es más precisa en uno de los campos personalizados, puede utilizar su propio campo en su lugar
* **Actividades:** Los tipos de actividad que se van a obtener de la base de datos de Marketo se especifican aquí para cada conjunto de actividades, no es necesario cambiarlos ahora.
   * Tenga en cuenta que proporcionamos una consulta de utilidad en el libro que enumera, justo en el libro de Excel, todos los tipos de actividad existentes si desea ajustar esta información más adelante

Tenga en cuenta que puede ver algunas ventanas emergentes relacionadas con la seguridad. Confíe en las conexiones externas y configúrelas en &quot;Público&quot;. Si ve la ventana emergente a continuación, permanezca con el contenido de acceso web &quot;Anónimo&quot;. La autenticación en Marketo la administran directamente nuestras consultas personalizadas, por lo que no es necesario habilitar ningún otro tipo de acceso.

#### Descargar datos de Marketo

Asegúrese primero de que el parámetro definido en el área Ámbito de la hoja de cálculo de configuración de Marketo no resulte en la descarga de demasiados datos, excediendo el límite diario de solicitudes de la API de Marketo. Cuando esté listo, haga clic en el botón &quot;Actualizar todo&quot; del menú &quot;Datos&quot; y espere a que todos los datos se descarguen en el libro. Si se muestran mensajes de error de formato al descargar los datos, similar a &quot;column1 not found&quot;, significa que una o más consultas no consiguen obtener los datos, por lo que el formato también falla. Vuelva a intentarlo más tarde. Si el error persiste, compruebe la versión de Excel (no utilice Excel 2016 desde Office 365). También es importante respetar la latencia de la plataforma de Marketo. Si realiza cambios en una lista estática o en los datos de posibles clientes, es preferible esperar antes de iniciar las consultas de energía.

### Modelado de datos en Power Pivot

Abra Power Pivot haciendo clic en el botón &quot;Administrar&quot; del menú Power Pivot, disponible en la barra de menús superior (si no está disponible, compruebe su versión de Excel, Power Pivot se puede instalar como complemento en algunas versiones de Excel). Todos los datos descargados de Marketo y enviados al modelo de datos deben ser accesibles desde las diferentes pestañas en la parte inferior de la ventana de Power Pivot.

### Expresiones de análisis de datos (DAX)

Es necesario enriquecer o volver a dar formato a los datos de algunos informes. Usemos expresiones de análisis de datos de Power Pivot (DAX) para definir algunos cálculos personalizados como columnas calculadas y medidas (también conocidos como campos calculados). Consulte el vínculo &quot;DAX en Power Pivot&quot; en la sección Referencias para obtener más información sobre DAX. Asegúrese de que el área de cálculo aparece en la ventana de Power Pivot; si no es así, actívela en el menú Inicio de Power Pivot.  Seleccione la ficha **MktoLeads** y agregue la medida **Recuento de posibles clientes** a cualquier lugar del área de cálculo de posibles clientes: **Recuento de posibles clientes:=**&#x200B;**DISTINCTCOUNT**&#x200B;**([id])**. Esta medida cuenta los distintos posibles clientes disponibles en la lista en función de su ID. También tendría en cuenta los posibles filtros establecidos en el contexto de un informe. Esta medida no es realmente necesaria ya que los informes son capaces de resumir el número de posibles clientes, pero lo hicimos para tener un recuento de posibles clientes con un nombre más bonito que &quot;suma de MktoLeads&quot;. También es un ejemplo sencillo que le permite imaginar fácilmente algunas medidas más complejas realizando promedios, mínimo, máximo para un tipo específico de entrada de datos (por ejemplo, todos los posibles clientes con una puntuación superior a 50, puntuación media, etc.). ...).  Ahora vamos a seleccionar la ficha **MktoWebActivities** y crear tres columnas calculadas. Inserte las siguientes columnas calculadas desplazándose hacia la derecha de la tabla y haciendo clic en la columna &quot;Agregar columna&quot;. **Actividad:** Obtenga la etiqueta de actividad fácil de usar buscando el ID de actividad en la tabla MktoActivityTypes. **\=**&#x200B;**LOOKUPVALUE**&#x200B;**(MktoActivityTypes[name],MktoActivityTypes[id],[activityTypeId])** **Year-Month:** vuelva a dar formato a la fecha de la actividad con un patrón &quot;AAAAmm&quot; que sea más adecuado para algunos informes. **\=**&#x200B;**LEFT**&#x200B;**([activityDate],4)&amp;**&#x200B;**MID**&#x200B;**([activityDate],6,2)** **Date:** La fecha de actividad es solo una cadena de nuestra consulta original, transfórmela a una fecha apropiada. **\=**&#x200B;**DATE**&#x200B;**(**&#x200B;**LEFT**&#x200B;**([activityDate],4),**&#x200B;**MID**&#x200B;**([activityDate],6,2),**&#x200B;**MID**&#x200B;**([activityDate],9,2))** Ahora vamos a crear las tres mismas medidas para la ficha **MktoEmailActivities** y dos más: **Campaña:** Obtenga el nombre descriptivo de la campaña buscando el identificador de campaña en la tabla MktoCampaigns. **\=**&#x200B;**LOOKUPVALUE**&#x200B;**(MktoCampaigns[name],MktoCampaigns[id],[campaignId])** **Programa:** Obtenga el nombre descriptivo del programa buscando el ID de campaña en la tabla MktoCampaigns. La tabla MktoPrograms puede proporcionar más detalles sobre el programa, como la carpeta, el espacio de trabajo, etc. **\=**&#x200B;**LOOKUPVALUE**&#x200B;**(MktoCampaigns[programName],MktoCampaigns[id],[campaignId])**

### Relaciones de entidad

Anteriormente habíamos visto una forma de buscar información de otra tabla dentro del modelo para completar la información que faltaba. Power Pivot ofrece una opción más potente para definir las relaciones entre algunas tablas del modelo de datos, lo que nos permite aprovechar esas relaciones directamente desde los informes. Vamos a definir las relaciones clave para nuestros informes. Seleccione la vista Diagrama en la ventana de Power Pivot. Rastree las siguientes relaciones dentro del diagrama del modelo de datos:

* **MktoInterestingMomentActivities:leadId →** **MktoLeads:id**
* **MktoScoringActivities:leadId →** **MktoLeads:id**
* **MktoRevenueStageActivities:leadId →** **MktoLeads:id**
* **MktoWebActivities:leadId →** **MktoLeads:id**
* **MktoEmailActivities:leadId →** **MktoLeads: id**

No utilizaremos todas estas relaciones y objetos en nuestros informes, solo los posibles clientes, las actividades web y las actividades de correo electrónico. Ahora es el momento de crear algunos informes.

### Gráfico dinámico de rendimiento de correos electrónicos

Este primer informe muestra los KPI de rendimiento del correo electrónico basados en un gráfico dinámico estándar de Excel. Nos permite filtrar los datos por sector o por campaña. Puede crear un gráfico dinámico directamente desde el menú Power Pivot seleccionando &quot;Gráfico dinámico&quot; en el selector &quot;Tabla dinámica&quot;.  Una alternativa es crear un gráfico dinámico directamente desde la hoja de cálculo de Excel, marcando la opción &#39;Usar el modelo de datos de este libro&#39;.  Arrastre y suelte los campos de las tablas **MktoEmailActivities** y **MktoLeads**, como se muestra en la figura siguiente: **MktoEmailActivities.Activity →** **Legend** (se usa la columna calculada DAX que implementamos en **MktoEmailActivities** anteriormente) **MktoEmailActivities.Date →** **Axis** (se usa la columna calculada DAX que implementamos en **MktoEmailActivities** anteriores) **MktoEmailActivities.Id →** **∑ Valores** **MktoEmailActivities.Campaign →** **Filtro** **MktoLeads.industry →** **Filtro**

Puede crear un nombre personalizado seleccionando &quot;Configuración del campo de valor&quot; en cada campo colocado. En este caso, soltamos el campo ID de actividad de correo electrónico en la sección &quot;Valores de ∑&quot; y editamos su nombre personalizado como &quot;Número de actividades&quot;. Ahora vamos a configurar el gráfico dinámico. Haga clic con el botón derecho directamente en el gráfico y seleccione la opción Cambiar tipo de gráfico en el menú contextual. Y así es como seleccionamos los diferentes tipos de gráficos para todas las series de datos.

### Mapa de posibles clientes con vista Power

El segundo informe muestra los posibles clientes y contactos por ubicación geográfica en un mapa del mundo y por sector. Necesitamos Power View para este informe. Siga el vínculo de referencia siguiente para activar el menú en Excel. O simplemente puede escribir &quot;vista avanzada&quot; en el cuadro de búsqueda de Excel. Seleccione &quot;Insertar un informe de Power View&quot;.  En el informe de Power View en blanco, seleccione la tabla **MktoLeads** en el panel derecho y arrastre y suelte el campo de ubicación del posible cliente (por ejemplo, **inferredCity**). Ahora el menú &#39;Diseño&#39; aparece en el menú principal.

Cambie a la visualización de Mapa seleccionando &quot;Mapa&quot; en el menú &quot;Diseño&quot; de la vista de Power View. Arrastre y suelte los campos de la tabla **MktoLeads**, como en la figura siguiente: **MktoLeads.industry →** **Color** **MktoLeads.inferredCity →** **Ubicaciones** **MktoLeads.Leads Count →** **∑ Size** (con la medida DAX que implementamos en **MktoLeads** antes) y asigne sus posibles clientes está listo! Basta con ajustar el tamaño del mapa, personalizar el título y las leyendas. Power View le permite crear paneles avanzados con varios gráficos en una sola hoja de cálculo. Consulte el tutorial al que se hace referencia a continuación &#39;[Crear informes de Power View increíbles](https://support.microsoft.com/en-us/article/Tutorial-Create-Amazing-Power-View-Reports-Part-1-e2842c8f-585f-4a07-bcbd-5bf8ff2243a7)&#39; para ver cómo continuar con más componentes de panel con Power View.

### Actividades Web animadas en un mapa 3D

Este tercer informe muestra las actividades web de los posibles clientes, por sector, en un mapa del mundo en 3D. Necesitamos un mapa 3D para este informe. Simplemente escriba &quot;3D&quot; en el cuadro de búsqueda de Excel y seleccione &quot;Mapa 3D&quot;. Cree un nuevo recorrido desde la ventana emergente.  Seleccione el gráfico de burbujas en el panel derecho. Arrastre y suelte los campos de las tablas **MktoLeads** y **MktoWebActivities**, como se muestra en la figura siguiente: **MktoLeads.industry →** **Category** **MktoLeads.inferredCity →** **Location** **MktoWebActivities.Activity →** **Time** (este uso la columna calculada DAX implementada en **MktoWebActivities** antes. El campo de ID también se puede usar para contar actividades.) **MktoWebActivities.Date →** **Time** (este uso usa la columna calculada DAX que implementamos en **MktoWebActivities** anteriormente) **MktoWebActivities.Activity** también se puede usar como filtro para filtrar los diferentes tipos de actividades web.

Utilice el botón &quot;Temas&quot; para cambiar el esquema de colores de su mapa 3D. Abra &quot;Opciones de escena&quot; para personalizar las animaciones.
Y ya terminaron con el Mapa Mundial 3D, ahora pueden divertirse animando el globo y creando video a partir de él.

### Pasos siguientes

Acabamos de rascar la superficie de lo que es posible hacer con las herramientas de Excel Power BI. Le recomendamos que busque en la web otros buenos artículos y tutoriales para ampliar sus habilidades con Excel y diseñar los informes que necesite para lograr sus objetivos comerciales. Esperamos que haya disfrutado de estos artículos y que le hayan ayudado a aprovechar las grandes ventajas de Excel y Marketo juntos.

### Referencias

#### Power Pivot

* [Power Pivot: análisis y modelado de datos eficaces en Excel](https://support.microsoft.com/en-us/article/Power-Pivot-Powerful-data-analysis-and-data-modeling-in-Excel-d7b119ed-1b3b-4f23-b634-445ab141b59b)
* [Expresiones de análisis de datos (DAX) en Power Pivot](https://support.microsoft.com/en-us/article/Data-Analysis-Expressions-DAX-in-Power-Pivot-bab3fbe3-2385-485a-980b-5f64d3b0f730)

#### Power View

* [Activar Power View en Excel 2016](https://support.microsoft.com/en-us/article/Turn-on-Power-View-in-Excel-2016-for-Windows-f8fc21a6-08fc-407a-8a91-643fa848729a)
* [Tutorial: Crear informes de Power View increíbles](https://support.microsoft.com/en-us/article/Tutorial-Create-Amazing-Power-View-Reports-Part-1-e2842c8f-585f-4a07-bcbd-5bf8ff2243a7)

Publicado el _2017-02-02_ por _Philippe_

## Cambio importante en los registros de actividad en la API de Marketo

**Nota: Esta publicación se actualizará para reflejar los cambios realizados en los registros de actividad devueltos por la API debido a la migración a una nueva infraestructura.** **Última actualización: 13 de septiembre de 2018** Con el despliegue del servicio de actividades de próxima generación de Marketo a partir de septiembre de 2017, no podremos aplicar la exclusividad o presencia del campo &quot;id&quot; entero en las actividades, los cambios de valor de datos o los registros de eliminación de posibles clientes devueltos por las API de Marketo. Para evitar interrupciones en el servicio de las integraciones que recuperan registros de actividad, el campo de ID debe tratarse como opcional. La migración de este cambio empezará a afectar a las suscripciones y a la próxima versión. Este cambio afectará a los siguientes extremos: API de REST

Los tipos de SOAP afectados son `ActivityRecord` y `LeadChangeRecord`.

### Ejemplos

Los siguientes ejemplos muestran los tipos de registro que se verán afectados. En ambos ejemplos, el campo afectado se denomina &quot;id&quot;.

**Ejemplo de campo REST: id**

```json
  {
      "id" : 102988,
      "leadId" : 1,
      "activityDate" : "2015-01-16T23:32:19Z",
      "activityTypeId" : 1,
      "primaryAttributeValueId" : 71,
      "primaryAttributeValue" : "localhost/munchkintest2.html",
      "attributes" : [
          {
              "name" : "Client IP Address",
              "value" : "10.0.19.252"
          },
          {
              "name" : "Query Parameters",
              "value" : ""
          },
          {
              "name" : "Referrer URL",
              "value" : ""
          },
          {
              "name" : "User Agent",
              "value" : "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.95 Safari/537.36"
          },
          {
              "name" : "Webpage URL",
              "value" : "/munchkintest2.html"
          }
      ]
  }
```

**Ejemplo de campo de SOAP: id**

```xml
  <activityRecord>
    <id>1030680</id>
    <activityDateTime>2013-07-09T16:44:28-05:00</activityDateTime>
    <activityType>Visit Webpage</activityType>
    <mktgAssetName>ClickDemo</mktgAssetName>
    <activityAttributes>
      <attribute>
        <attrName>Webpage ID</attrName>
        <attrType xsi:nil="true" />
        <attrValue>2547</attrValue>
      </attribute>
      <attribute>
        <attrName>Webpage URL</attrName>
        <attrType xsi:nil="true" />
        <attrValue>/ClickDemo.html</attrValue>
      </attribute>
    </activityAttributes>
    <campaign />
    <personName xsi:nil="true" />
    <mktPersonId>1089965</mktPersonId>
    <foreignSysId xsi:nil="true" />
    <orgName xsi:nil="true" />
    <foreignSysOrgId xsi:nil="true" />
  </activityRecord>
```

Publicado el _2017-03-01_ por _Kenny_

## Actualizaciones de invierno de 2017

En la versión de invierno de 2017, se ha añadido la capacidad de importar de forma masiva datos de objetos personalizados de forma asíncrona y de manipular variables en páginas de aterrizaje guiadas. Consulte la lista completa de actualizaciones a continuación.

### API de base de datos de clientes potenciales

#### Importación masiva de objetos personalizados

Nuevos extremos para admitir la importación masiva de objetos personalizados. Encontrará detalles [aquí](/help/rest-api/custom-objects.md).

#### Aviso de los próximos cambios en las actividades de

Tenga en cuenta que se producirá un cambio significativo cuando Marketo publique su servicio de actividad de próxima generación. Este cambio afectará a los siguientes extremos:

SOAP

[getLeadActivity](/help/soap-api/getleadactivity.md), [getLeadChanges](/help/soap-api/getleadchanges.md)

El campo entero &quot;id&quot; contenido en los registros devueltos por estos extremos ya no se garantizará como único. Esto afectará la actividad, el cambio del valor de los datos y los tipos de registro de eliminación de posibles clientes. Para evitar interrupciones en el servicio de las integraciones que recuperan estos tipos de registros, el campo de ID debe tratarse como opcional.

#### Eliminación de token de inserción

Se ha añadido la capacidad de eliminar tokens push mediante la API de SDK. Encontrará detalles aquí: [iOS](/help/mobile/push-notifications.md), [Android](/help/mobile/push-notifications.md).

Publicado el _01-03-2017_ por _David_

## Actualizaciones de primavera de 2017

En la versión de primavera de 2017, se ha añadido la capacidad de extraer de forma masiva datos de clientes potenciales y objetos de actividad de forma asíncrona, así como de manipular listas de cuentas con nombre. Consulte la lista completa de actualizaciones a continuación.

### Extracción masiva de posibles clientes

Nuevos extremos para admitir la extracción de posibles clientes de forma masiva. Especifique los criterios de selección de registros mediante diversas opciones. Encontrará detalles [aquí](/help/rest-api/bulk-lead-extract.md).

### Extracción masiva de actividades

Nuevos extremos para admitir la extracción de actividades de forma masiva. Especifique los criterios de selección de registros mediante el intervalo de fechas y la lista de actividades. Encontrará detalles [aquí](/help/rest-api/bulk-extract.md).

### Listas de cuentas con nombre

Nuevos extremos para admitir operaciones de CRUD en Listas de cuentas con nombre. Encontrará detalles [aquí](/help/rest-api/named-account-lists.md).

### Otras mejoras

* El extremo [Obtener campañas](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getCampaignByIdUsingGET) ahora le permite filtrar campañas &quot;activables&quot;. Esto se logra pasando &quot;isTriggerable=true&quot; como parámetro de consulta.
* El extremo [Clone Program](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST) ahora admite programas que contienen todos los tipos de recursos excepto SMS Message.

### Iónico

Ahora puede usar MME de Marketo Mobile y con el marco de aplicación [Ionic](https://ionicframework.com/). Encontrará detalles [aquí](/help/mobile/ionic.md).

### Anular el registro del token de notificación push

Se ha agregado un nuevo método a SDK que le permite anular el registro del token de notificación push con Marketo. Esto resulta útil, por ejemplo, cuando un usuario cierra sesión en la aplicación. Para su uso, consulte `unregisterPushDeviceToken` (iOS) o `uninitializeMarketoPush` (Android) método [aquí](/help/mobile/push-notifications.md).

Publicado el _16-06-2017_ por _David_

## Internet de las cosas para los especialistas en marketing con IFTTT y Zapier

El Internet de las Cosas (IoT) es la interconexión de dispositivos, electrodomésticos, wearables, vehículos, etc. conectados. con electrónica integrada, software, sensores y conectividad de red que permiten que estos objetos recopilen e intercambien datos con sistemas de información en la nube. Estas tecnologías están creciendo y haciendo tendencia tan rápido que impactarán en cómo vivimos, cómo trabajamos y cómo hacemos negocios en poco tiempo. Marketo, la plataforma de participación en marketing líder, está preparada para el IoT, con sus capacidades de escalar e interactuar con cualquier forma de canal de comunicación. Marketo ya puede rastrear más de 70 tipos de actividades relacionadas con correos electrónicos, web, móvil, CRM, etc... y también admite [actividades personalizadas](https://experienceleague.adobe.com/docs/marketo/using/product-docs/administration/marketo-custom-activities/create-a-custom-activity.html?lang=es) que cualquier sistema de terceros puede suministrar. Los [objetos personalizados](https://experienceleague.adobe.com/docs/marketo/using/product-docs/administration/marketo-custom-objects/understanding-marketo-custom-objects.html?lang=es) de Marketo hacen posible el seguimiento de todo tipo de métricas de terceros relacionadas con su negocio y permiten a los especialistas en marketing aprovechar esas métricas directamente desde los déclencheur y filtros de campañas inteligentes de Marketo. La implementación de IoT para los consumidores requeriría un servidor centralizado para interactuar con los dispositivos de los consumidores y este servidor intercambiaría datos con la plataforma abierta de Marketo, con capacidades como API de REST, objetos personalizados, actividades personalizadas, etc. - documentó [aquí](http://eto.com/). No es fácil de demostrar a través de una publicación de blog. En lugar de eso, integraremos el servicio IFTTT con Marketo para implementar algunos casos de uso de IoT interesantes para los especialistas en marketing como:

* Animar a su equipo de marketing cada vez que se registra un posible cliente en un programa de viajes parpadeando una luz de color en la oficina
* Animar a su equipo de ventas cada vez que se gana un acuerdo activando automáticamente una campana conectada a un enchufe de alimentación conectado
* Publique automáticamente hitos de éxito de marketing en redes sociales como LinkedIn, Facebook, Slack, etc. ...
* Iniciar automáticamente una campaña de marketing basada en:
   * cuando se produce una alerta meteorológica (viento, temperatura, lluvia, etc.)
   * cuando un nuevo artículo es publicado por un periódico como el New York Times, con algunos criterios específicos
   * cuando el Senado o la Cámara de Representantes de Estados Unidos voten
   * cuando la Estación Espacial Internacional pasa sobre una determinada ubicación
   * etc. ...

Estos escenarios pueden resultar divertidos pero inútiles, pero están aquí para demostrar una nueva forma conceptual de hacer marketing no solo con las personas, sino también con las cosas en nuestro mundo conectado. Otro punto interesante cubierto en este artículo, es cómo aprovechar una plataforma de integración web abierta como Zapier como una &quot;escotilla de servicio&quot; entre un sistema de terceros y Marketo, para administrar la autenticación, por ejemplo.

### El servicio IFTTT

IFTTT es un acrónimo de &quot;IF This Then That&quot;. Se trata de un servicio gratuito basado en la web que la gente utiliza para crear cadenas de afirmaciones condicionales simples, llamadas applets. Un subprograma se activa por los cambios que se producen en algunos servicios web de socios y, como resultado, las acciones se envían a otros servicios web de socios. El IFTTT fue lanzado en 2011 por Linden Tibbets, Jesse Tane, Scott Tong y Alexander Tibbets en San Francisco. A primera vista, IFTTT se parece a un servicio como [Zapier](https://zapier.com/), por ejemplo, tiene un enfoque mucho más fuerte en los consumidores y dispositivos IoT (mandos a distancia, alarmas, luces, termostatos, automóviles, impresoras, teléfonos móviles y mucho más).  En primer lugar, debe crear una cuenta para IFTTT a partir del [sitio web de IFTTT](https://ifttt.com/explore). Siéntase libre de descubrir todos los applets geniales ya disponibles, ya que le dará algunas otras ideas de escenarios con seguridad!

### El canal Maker

Una aplicación web que no tiene un canal, es decir, una asociación con IFTTT, debe utilizar el canal Maker. Con el canal Maker, puedes crear Applets que funcionen con cualquier dispositivo o aplicación que pueda hacer o recibir una solicitud web. Ofrece las siguientes integraciones:

* Déclencheur entrantes para recibir una solicitud web de un sistema de terceros para almacenar en déclencheur una acción
* Acciones salientes para hacer que una solicitud web a un sistema de terceros sea accesible públicamente en Internet

Desde IFTTT, busque el servicio &quot;Maker&quot; y haga clic en él.  La primera vez, debe activarlo haciendo clic en el botón &quot;Conectar&quot;.  Ahora el canal Maker está activo. Puede obtener su clave secreta haciendo clic en el botón Maker Settings: copie y pegue la URL proporcionada en su explorador para obtener más detalles.

### Activación directa de una acción IFTTT desde el mercado

En primer lugar, nos centraremos en activar todo tipo de acciones de servicios web de terceros desde Marketo. Para ello, vamos a usar un [webhook de Marketo](https://experienceleague.adobe.com/docs/marketo/using/product-docs/administration/additional-integrations/create-a-webhook.html?lang=es). Empezaremos con un mensaje push en su teléfono móvil o tableta a través de la aplicación móvil IFTTT y luego implementaremos un escenario de IoT que parpadea una luz Philips Hue.

### Webhook de Marketo

Almacenar en déclencheur un evento de Marketo, que actúa como el &quot;if&quot; de IFTTT, es sencillo. Todo lo que debe hacer es enviar una petición web POST a IFTTT con un nombre de evento y su clave secreta, siguiendo este patrón de URL:

`<https://maker.ifttt.com/trigger/{event_name}/with/key/{secret_key}>`

Maker también permite comunicar hasta 3 parámetros a través de la solicitud web. Esto se puede hacer mediante parámetros de consulta,

`<https://maker.ifttt.com/trigger/{event_name}/with/key/{secret_key}?value1={value1}&value2={value2}&value3={value3}>`

o utilizando un cuerpo JSON que consta de hasta tres valores:

`{ "value1" : "{value1}", "value2" : "{value2}", "value3" : "{value3}" }`

En Marketo, cree un nuevo webhook desde la interfaz de administración.  Proporcione la siguiente información para su nuevo webhook:

**Nombre de webhook:** Éxito del programa IFTTT

**Descripción:** Déclencheur un evento en IFTTT desde una campaña inteligente para un programa de éxito

**URL:** `<https://maker.ifttt.com/trigger/{event_name}/with/key/{secret_key}?value1={{program.name}}&value2={{lead.Email> Address}}&value3={{lead.Full Name}}`

event_name, utilice MarketoProgramSuccess, por ejemplo

secret_key, utilice la clave secreta del servicio IFTTT Maker

Utilice texto estático o tokens de Marketo para los tres valores disponibles. Puede insertar más mensajes interactivos definiendo sus propios tokens en el nivel de programa y pasándolos a través de estos valores.

**Tipo de solicitud:** PUBLICACIÓN

**Plantilla:** Dejar en blanco

**Codificación De Token De Solicitud:** Formulario/Url

**Tipo de respuesta:** Ninguno

No es necesario definir una asignación de respuesta.

### applet IFTTT

En el portal web de IFTTT, seleccione &quot;Mis applets&quot; en el menú principal.  Haga clic en el botón &quot;Nuevo applet&quot; y luego en la sección **+this**.  Busque el servicio Maker.  Cree la Déclencheur que se activará cada vez que el servicio Maker reciba una solicitud web para notificarle un evento. Utilice el mismo Nombre del evento que el especificado en la URL del webhook de Marketo, por ejemplo &quot;MarketoProgramSuccess&quot; y haga clic en el botón &quot;Crear déclencheur&quot;.  Ahora es el momento de especificar el Servicio de acción haciendo clic en la sección **+that**.  Vamos a empezar con un servicio de acción simple que cualquiera podría probar sin tener que invertir en ningún dispositivo IoT, el Servicio de Notificaciones. Busque y seleccione el Servicio de notificaciones.
Elija la acción &quot;Enviar una notificación&quot; que envía una notificación a sus dispositivos.  Puede aprovechar los 3 valores que ha enviado desde Marketo añadiéndolos como ingredientes para enviar una notificación significativa al usuario, como en el ejemplo siguiente... Y luego haga clic en el botón &quot;Crear acción&quot;. Revise y finalice el applet IFTTT. Asegúrese de que esté habilitado.

### Prueba del applet IFTTT

Si desea recibir notificaciones en su dispositivo móvil, primero debe descargar la aplicación IFTTT para su dispositivo.  Puede almacenar en déclencheur un evento de éxito del programa de Marketo mediante el webhook en un flujo de campaña inteligente de Marketo. Recuerde que los webhooks de Marketo funcionan exclusivamente con campañas inteligentes activadas (por ejemplo, un déclencheur una vez que un contacto rellena el formulario, se añade a una lista, etc.).  Y aquí hay un ejemplo de una notificación de IFTTT en su teléfono móvil.

### Vamos a usar Creative con IFTTT

IFTTT ofrece Applet Actions con más de 300 socios, por lo que su cartera de aplicaciones y electrodomésticos junto con su imaginación son los límites... Veamos un ejemplo con las [luces Hue de Philips](https://www.philips-hue.com/en-us) que puede comprar en cualquier lugar en tiendas de electrónica o en línea. Las siguientes miniaplicaciones destellarían una de las luces con el color asignado actualmente cuando Marketo obtiene un déclencheur del éxito de un programa, lo que podría impulsar a su equipo de marketing en la oficina. Creamos un nuevo Applet, siguiendo los mismos pasos que antes, donde Marketo se activa con un webhook, pero esta vez elegimos la acción del servicio Philips Hue.

Vamos a seleccionar la acción &quot;Luces parpadeantes&quot;. La aplicación solicitará a Philips Hue todas las luces disponibles, para que puedas elegir la que parpadee. Primero debe configurar una cuenta con Philips Hue, el puente Hue y, por supuesto, al menos una bombilla, tira de luz, proyector o lámpara Hue.  Acabamos de agregar una nueva miniaplicación que parpadeará con una luz de color cada vez que se registre un posible cliente en una gira o seminario web. Su equipo de marketing se alegrará cada día con esa configuración en la oficina.

### Ejecución de una acción de Marketo desde IFTTT, a través de Zapie

Ahora, vamos a almacenar en déclencheur una Marketo Smart Campaign desde la plataforma IFTTT. Para ello, vamos a usar la [API de REST de Marketo](/help/rest-api/rest-api.md). Dado que esta API está protegida y requiere una autenticación OAuth2 antes de invocar cualquier cosa, necesitamos gestionar esa autenticación a través de otra plataforma como Zapier, ya que IFTTT no permite encadenar dos llamadas consecutivas en una API con el canal Maker. Elegimos el servicio de automatización de aplicaciones web [Zapier](https://zapier.com/) porque ya publicamos la presentación de Zapier y explicamos paso a paso cómo implementar un conector de Marketo personalizado para Zapier. Otras plataformas como [Workato](https://www.workato.com/integrations/marketo) también podrían ser una solución.

### Marketo Campaign

Cree su programa Marketo con una campaña inteligente programada. Con fines de prueba, podría crear la siguiente campaña inteligente como ejemplo: **Smart List** Use solo filtros, no déclencheur. Asegúrate de que al menos calificas. **Flujo** Enviarle un correo electrónico o cualquier otro tipo de notificación. **Programación** Asegúrese de que puede ejecutar el flujo cada vez que desee controlar varias pruebas. Puede obtener el ID de campaña inteligente de la dirección URL. Ejemplo: _https://{{marketo_url}}/#SC4289A1_ - el identificador de campaña inteligente sería 4289. Puede almacenar en déclencheur esta campaña a través de la API de REST de Marketo. Puede usar, por ejemplo, el complemento [Postman](https://www.postman.com/) para Chrome y enviar las dos llamadas HTTPS consecutivas siguientes: **Paso de autenticación:**

`https://{{Your Munchkin_Account_id}}.mktorest.com/identity/oauth/token?grant_type=client_credentials&client_id={{Your_Client_Id}}&client_secret={{Your_Client_Secret}}`

Recupere el token de acceso de la respuesta JSON. **Paso de inicio de la campaña:**

`https://{{Your_Munchkin_Account_id}}.mktorest.com/rest/v1/campaigns/{{Campaign_Id}}/schedule.json?access_token={{access_token}}`

### Conector personalizado Zapier intermedio para iniciar la campaña de Marketo

Necesitamos crear un conector Zapier personalizado que se autentique con la API de REST de Marketo y que ponga en marcha nuestra campaña inteligente.

* Requisitos previos
* Implementación de Marketo Connector para Zapier
* Utilice un título diferente, como &quot;Marketo Campaign&quot;
   * Realice el paso &quot;Autenticación&quot;
   * Realice el paso &quot;Déclencheur&quot; (requerido para fines de prueba de Zapier)
   * Realice el siguiente paso específico &quot;Acciones&quot;, responsable de iniciar una campaña de Marketo, como se explica a continuación:

URL de extremo de acción de envío de datos:

`https://{{munchkin_account_id}}.mktorest.com/rest/v1/campaigns/{{CampaignId}}/schedule.json`

Deje en blanco los demás campos opcionales.

#### API de scripts

La función de scripts de Zapier le permite manipular las solicitudes y respuestas que se intercambian entre la API de su aplicación y Zapier. Puede modificar las solicitudes HTTP justo antes de enviarlas y puede analizar las respuestas antes de que Zapier haga algo con ellas. Lo necesitamos para completar nuestra autenticación personalizada de &quot;Autenticación de sesión&quot;. Encontrará más información en el artículo original. Copie el siguiente código muy similar al original, solo hemos cambiado los métodos de acción:

```javascript
var Zap = {

 get_session_info: function(bundle) {

 console.log('Entering get_session_info method ...');

 var access_token,
 access_token_request_payload,
 access_token_response;


 // Assemble the meta data for our Access Token swap request
 console.log('building Request with client_id=' + bundle.auth_fields.client_id + ', and client_secret=' + bundle.auth_fields.client_secret);
 access_token_request_payload = {
 method: 'POST',
 url: 'https://' + bundle.auth_fields.munchkin_account_id + '.mktorest.com/identity/oauth/token',
 params: {
 'grant_type' : 'client_credentials',
 'client_id' : bundle.auth_fields.client_id,
 'client_secret' : bundle.auth_fields.client_secret
 },
 headers: {
 'Content-Type': 'application/json', // Could be anything.
 Accept: 'application/json'
 }
 };

 // Fire off the Access Token request.
 access_token_response = z.request(access_token_request_payload);

 // Extract the Access Token from returned JSON.
 access_token = JSON.parse(access_token_response.content).access_token;
 console.log('New Access_Token=' + access_token);

 // This will be mixed into bundle.auth_fields in future calls.
 //bundle.auth_fields.access_token=access_token;
 return {'access_token': access_token};
 },

 test_trigger_pre_poll: function(bundle) {

 console.log('Entering test_trigger_pre_poll method ...');

 bundle.request.params = {
 'access_token':bundle.auth_fields.access_token
 };

 return bundle.request;

 },

 test_trigger_post_poll: function(bundle) {

 console.log('Entering test_trigger_post_poll method ...');

 var data = JSON.parse(bundle.response.content);
 if ((!data.success)&&((data.errors[0].code=="601")||(data.errors[0].code=="600"))){
 console.log('Access Token expired or invalid, requesting new one - data.success=' + data.success + ', data.errors[0].code=' + data.errors[0].code);

 throw new InvalidSessionException(); // Calling get_session_info() to regenerate Access Token
 }

 return JSON.parse(bundle.response.content);
 },

 launch_campaign_pre_write: function(bundle) {

 bundle.request.params = {'access_token':bundle.auth_fields.access_token};
 return bundle.request;
 },

 launch_campaign_post_write: function(bundle) {

 var data = JSON.parse(bundle.response.content);
 if ((!data.success)&&((data.errors[0].code=="601")||(data.errors[0].code=="600"))){
 console.log('Access Token expired or invalid, requesting new one - data.success=' + data.success + ', data.errors[0].code=' + data.errors[0].code);
 throw new InvalidSessionException(); // Calling get_session_info() to regenerate Access Token
 }
 return JSON.parse(bundle.response.content);
 }

};
```

##### Nuevo Zap

En el panel de Zapier haga clic en el botón &quot;Hacer un nuevo Zap&quot;. **Activador**

* Elija la aplicación de Déclencheur &quot;Webhooks by Zapier&quot;
* Marque &quot;Catch Hook&quot;
* No hay necesidad de elegir una clave secundaria
* Zapier generó una URL de webhook personalizada para que usted envíe solicitudes a, manténgala segura en algún lugar
* Pruebe la URL del webhook iniciando el escenario &quot;Subprograma IFTTT que llama al webhook Zapier&quot; a continuación. Esto permite a Zapier obtener información sobre la carga útil del gancho web y le permite asignar el ID de campaña a la acción

**Acción**

* Seleccione el conector de Marketo Campaign creado anteriormente
* Elija la única acción disponible: **Iniciar campaña**
* Conéctese a su cuenta de Marketo rellenando los parámetros de autenticación (ID de cuenta de Munchkin, ID de cliente y Secreto de cliente)
* Edite la plantilla y asocie el ID de campaña del Déclencheur al parámetro de ID de campaña &quot;Launch Campaign&quot;
* Pruebe el paso y compruebe que se inicia Marketo Campaign

### Applet IFTTT que llama al webhook de Zapier

Empezamos con un escenario simple que es fácil de probar. Elegimos en IFTTT un déclencheur de fecha y hora que iniciará la campaña de Marketo cada hora. La acción es una publicación de solicitud web en la URL del webhook de Zapier y que pasa el ID de campaña inteligente. Asegúrese de que el Zapier Zap y el IFTTT Applet están activos y pruebe que todo funciona según lo esperado.

### Vamos a usar Creative con IFTTT

IFTTT ofrece Déclencheur de Applet con más de 300 socios, así que nuevamente su portafolio de aplicaciones y electrodomésticos junto con su imaginación son los límites... Veamos un ejemplo con el servicio [Weather Underground](https://www.wunderground.com/) que vamos a usar para lanzar nuestra campaña de Marketo sobre alertas meteorológicas. El siguiente déclencheur comenzaría cuando se anunciara una condición de Lluvia. Y luego asocia el Déclencheur con la acción Maker Webhook, y al igual que antes rellena los parámetros del webhook Zapier.  Et voila, solo necesitas ahora una buena lluvia para venir a comprobar que esto está funcionando.

Esperamos que se divierta mucho aplicando los conceptos proporcionados en este artículo. Pero lo más importante, creemos que ayudará a cualquiera que desee integrar Marketo con otros sistemas de terceros, gracias a los conceptos clave de este artículo:

* API DE REST DE MARKETO
* Webhooks de Marketo
* Cómo aprovechar una plataforma de integración web abierta como Zapier como &quot;trampilla de servicio&quot; entre un sistema de terceros y Marketo, para administrar la autenticación, por ejemplo

Publicado el _2017-06-20_ por _Philippe_

## Actualizaciones del verano de 2017

En la versión de verano de 2017, publicaremos algunas mejoras menores en las API de nuestro programa.

### Examinar programas

Estamos agregando la capacidad de obtener programas por intervalo de fechas a nuestro punto de conexión [Obtener programas](https://developer.adobe.com/marketo-apis/api/asset/#operation/browseProgramsUsingGET), mediante la adición de los parámetros opcionales soonUpdatedAt y latestUpdatedAt. Puede configurar uno o ambos parámetros con la fecha y hora que elija para devolver solo los programas que se hayan creado o actualizado entre las dos fechas y horas. Esto resulta útil para recuperar conjuntos nuevos y actualizados de material promocional de marketing, lo que es más importante en los casos de uso de traducción e inteligencia empresarial.

Publicado el _1970-01-01_ por _Kenny_

## Ampliar la lógica empresarial de Marketo con la función de nube de Google:

Este artículo propone una solución para ampliar Marketo con algunas funciones de lógica empresarial con Google Cloud Platform (GCP), basada en el siguiente ejemplo simple: 3 campos personalizados en el registro de Marketo Lead:

* **OnLinePreference**: una puntuación incremental que indica una aptitud de cliente potencial/cliente para las comunicaciones en línea.
* **OfflinePreference**: una puntuación incremental que indica una aptitud de cliente/cliente potencial para comunicaciones sin conexión.
* **Preferencia**: un campo calculado por GCP que muestra &quot;sin conexión&quot; si la puntuación sin conexión es mayor que la en línea y &quot;en línea&quot; al contrario

Esta tecnología abre el camino para una lógica empresarial más avanzada y, finalmente, para llamar a servicios web externos, transformando y consolidando los resultados en Marketo.  

### Acerca de Google Cloud Platform y sus funciones

[Google Cloud Platform](https://cloud.google.com/) (GCP) es un conjunto de servicios de computación en la nube que se ejecuta en la misma infraestructura que Google usa internamente para sus productos de usuario final, como Google Search y YouTube. Además de un conjunto de herramientas de gestión, ofrece una serie de servicios modulares en la nube que incluyen informática, almacenamiento de datos, análisis de datos, aprendizaje automático, big data y mucho más. Podríamos haber usado muchos servicios GCP diferentes para nuestras necesidades, como Compute Engine, App Engine o Kubernetes Engine, pero optamos por [Cloud Functions](https://cloud.google.com/functions) (aún en Beta) para las siguientes ventajas principales:

* La computación en la nube sin servidor es el lugar donde la lógica se puede aplicar bajo demanda en respuesta a eventos como llamadas HTTP.
* Alivia la mayor parte del dolor causado por el mantenimiento y las implementaciones del servidor.
* Rentable, ya que paga GCP solo por cada llamada de función y no por mantener un servidor en funcionamiento.
* Implementación sencilla y rápida porque solo se centra en la lógica de la aplicación.
* Escalado automático, listo para cargas de trabajo muy altas.

Consulte el [sitio web de GCP](https://cloud.google.com/) para obtener más información sobre esta tecnología y sus precios. Por lo general, este tutorial no debe inducir ningún coste importante y se ajustará perfectamente al crédito gratuito de una prueba de GCP.  

### Preparación del entorno de nube de Google

Necesita una cuenta de Google Cloud. Puedes probar GCP gratis con un crédito que sea más que suficiente para ejecutar este tutorial, solo haz clic en el botón &quot;Probar gratis&quot; en el [sitio web de GCP](https://cloud.google.com/). Siga todos los pasos de la sección &quot;Antes de comenzar&quot; en el [Tutorial HTTP](https://cloud.google.com/functions/docs/calling) de Google:

1. Creación de un proyecto de Cloud Platform: VAYA A LA PÁGINA ADMINISTRAR RECURSOS
1. Habilite la facturación para su proyecto: [HABILITAR FACTURACIÓN](https://cloud.google.com/billing/docs/how-to/modify-project?visit_id=638816637273392093-1926929734&rd=1)
1. Habilitar la API de funciones de nube: [HABILITAR LA API](https://accounts.google.com/InteractiveLogin?continue=https://console.cloud.google.com/flows/enableapi?apiid%3Dcloudfunctions%26redirect%3Dhttps://cloud.google.com/functions/docs/tutorials/http&followup=https://console.cloud.google.com/flows/enableapi?apiid%3Dcloudfunctions%26redirect%3Dhttps://cloud.google.com/functions/docs/tutorials/http&osid=1&passive=1209600&service=cloudconsole&ifkv=ASKV5Mh81NGNsqcJqhx7hst0KFnyA0MJ-2zay8ovyluBfpvDoM820nF9Wq_SKbC1m_sjQvvRNoKSuA)
1. [Instalar e inicializar Cloud SDK](https://cloud.google.com/sdk/docs/)
1. Actualización e instalación de componentes de Gcloud

   **actualización de componentes de nube y** **Los componentes de nube instalan la versión beta**

1. Prepare su entorno para el desarrollo de Node.js: [VAYA A LA GUÍA DE INSTALACIÓN](https://cloud.google.com/nodejs/docs/setup)

### Implementación de la función scoreCompare Cloud

1. Cree un contenedor de almacenamiento en la nube para almacenar en zona intermedia los archivos de funciones de la nube. Puede hacerlo con la línea de comandos:

   `gsutil mb gs://[YOUR_STAGING_BUCKET_NAME]`

   o desde la interfaz web de Google Cloud, seleccionando el proyecto y haciendo clic en el menú Almacenamiento:
   * Dé un nombre único a su contenedor de almacenamiento
   * Seleccionar la clase de almacenamiento predeterminada
   * Seleccione la ubicación regional más adecuada

1. Cree un directorio en el sistema local para el código de la aplicación.
1. Cree un archivo &quot;index.js&quot; en este directorio con el siguiente código JavaScript: el código es realmente fácil de entender. Analiza los dos parámetros de entrada desde el cuerpo de solicitud HTTP en JSON, realiza el procesamiento y codifica en JSON la respuesta HTTP.

```javascript
 /\*\*
     \* HTTP scoreCompare Cloud Function.
     \*
     \* @param {Object} req Cloud Function request context.
     \* @param {Object} res Cloud Function response context.
     \*/
    exports.scoreCompare = function scoreCompare (req, res) {
     var onlineScore=parseInt(req.body.onlineScore);
     var offlineScore=parseInt(req.body.offlineScore);
     console.log('/scoreCompare: got values onlineScore =' + onlineScore + ', offlineScore =' + offlineScore);
     var result;
     if (onlineScore>offlineScore) {result = 'online';} else {result = 'offline';}
     console.log('/scoreCompare: and result is ' + result);
     res.status(200).json({output: result}).end();
    };
```

Implementar la puntuación de funciónComparar con un déclencheur HTTP. Ejecute el siguiente comando desde el directorio:

**Las funciones beta de la nube implementan [FUNCTION] —stage-bucket [YOUR_STAGING_BUCKET_NAME] —déclencheur-http**

Donde [YOUR_STAGING_BUCKET_NAME] es el nombre del bloque de almacenamiento en la nube de ensayo. En nuestro ejemplo:

**funciones beta de nube implementan scoreCompare —stage-bucket mktostorage —déclencheur-http**

1. Tenga en cuenta la URL de función de nube (httpsTrigger URL) de la salida de la consola, que tiene este aspecto: `https://[YOUR_REGION]-[YOUR_PROJECT_ID].cloudfunctions.net/[FUNCTION]` donde

   * [YOUR_REGION] es la región donde se implementa la función. Esto es visible en el terminal cuando la función termina de implementarse.
   * [ID_DE_PROYECTO_DEL] es su ID de proyecto de nube. Esto es visible en el terminal cuando la función termina de implementarse.
   * [FUNCTION] es su nombre de función.

   En nuestro ejemplo: [**https://us-central1-marketo-cloud-logic.cloudfunctions.net/scoreCompare**](https://us-central1-marketo-cloud-logic.cloudfunctions.net/scoreCompare)
1. Pruebe su función con una herramienta como [Postman](https://www.postman.com/):
   * Verbo HTTP: POST
   * URL: [https://us-central1-marketo-cloud-logic.cloudfunctions.net/scoreCompare](https://us-central1-marketo-cloud-logic.cloudfunctions.net/scoreCompare)
   * Encabezados: content-type = application/json
   * Cuerpo: {&quot;onlineScore&quot;:110, &quot;offlineScore&quot;:200}La salida debe dar: {&quot;output&quot;: &quot;offline&quot;}.

### Llamar a la función de nube desde un webhook de Marketo

Los tres campos personalizados siguientes deben crearse en el registro de posible cliente de Marketo:

* **OnlinePreference**: entero
* **OfflinePreference**: entero
* **Preferencia**: Cadena

Cree el siguiente webhook desde la interfaz de administración de Marketo con la URL de la función de nube &quot;scoreCompare&quot; y los tokens del campo personalizado. Pruebe el webhook con una campaña inteligente activada por Marketo.

* **Los webhooks de Marketo solo se pueden invocar desde campañas inteligentes activadas, no desde campañas inteligentes por lotes.**
* **Si no usa su función de nube, elimínela o elimine todo el proyecto para evitar incurrir en cargos en su cuenta de Google Cloud Platform.**

Esperamos que este tutorial haya valido la pena y le haga pensar en escenarios más avanzados que impliquen un procesamiento complejo y servicios de terceros. Un buen ejemplo sería aprovechar la IA de la nube de Google, los servicios de aprendizaje automático de Google. Por ejemplo, puede analizar texto libre de un formulario de Marketo y solicitar a la API de lenguaje natural de Google que revele la estructura y el significado del texto y, a continuación, guardar este análisis en Marketo; solo tiene que abrir las puertas para obtener ideas.

Publicado el _2017-11-21_ por _Philippe_

## Actualizaciones de otoño de 2017

En la versión de otoño de 2017 de, publicamos varias mejoras en nuestras API de recursos. Consulte la lista completa de actualizaciones a continuación.

### Examinar programas por intervalo de fechas

Hemos agregado la capacidad de obtener programas por intervalo de fechas a nuestro extremo [Obtener programas](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneEmailUsingPOST). Esto se hace con los parámetros `earliestUpdatedAt` y `latestUpdatedAt`. Puede configurar uno o ambos parámetros con la fecha y hora que elija para devolver solo los programas que se hayan creado o actualizado entre las dos fechas y horas.

### Obtener vista previa del email

Ahora puede obtener una vista previa de un correo electrónico mediante el punto de conexión [Obtener contenido completo del correo electrónico](https://developer.adobe.com/marketo-apis/api/asset/#operation/getEmailFullContentUsingGET), que devuelve la versión serializada de HTML de un correo electrónico. Todos los tokens, fragmentos de código, contenido dinámico y componentes incrustados se representan completamente. Se puede pasar un parámetro **leadId** opcional para suplantar a un posible cliente determinado.

### Reemplazar HTML de correo electrónico 2.0

Hemos agregado el punto de conexión [Actualizar contenido completo del correo electrónico](https://developer.adobe.com/marketo-apis/api/asset/#operation/createEmailFullContentUsingPOST) para permitirle reemplazar bloques del contenido del correo electrónico de HTML. Si edita el código HTML de un correo electrónico de Marketo con el Editor de correo electrónico 2.0 de Marketo, la relación entre el correo electrónico y su plantilla se romperá; más información sobre ese(a) [aquí](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/edit-an-emails-html). Con este punto de conexión, puede actualizar mediante programación el contenido de HTML de un correo electrónico cuya relación se haya roto. Además, hemos modificado todos los demás extremos relacionados con el ciclo vital de los correos electrónicos para que sean compatibles con los correos electrónicos en los que se ha roto la relación:

* Aprobar borrador de correo electrónico
* Desaprobar correo electrónico
* Eliminar email
* Descartar borrador de correo electrónico
* Clonar Email
* Actualizar metadatos de correo electrónico

### Otras mejoras

* El intervalo de tiempo máximo para los filtros de intervalo de fechas se ha aumentado a 31 días. Esto pertenece a [Filtros de extracción masiva de posibles clientes](/help/rest-api/bulk-lead-extract.md) (createdAd o updatedAt) y [Filtro de extracción masiva de actividades](/help/rest-api/bulk-activity-extract.md) (createdAt).

Publicado el _15_-12-2017 por _David_

## TEST: vínculo de vídeo externo en la comunidad

[Vídeo del tutorial de Power Pack para la entrega de correo electrónico](https://nation.marketo.com:443/t5/product-space-archive-videos/email-deliverability-power-pack-tutorial-video/m-p/283550)  

Publicado el _1970-01-01_ por _David_

## Actualizaciones de invierno de 2018

En la versión de invierno de 2018 de, publicamos algunas mejoras en nuestras API. Consulte la lista completa de actualizaciones a continuación.

### Activar/Desactivar campañas de Déclencheur

Se ha agregado la capacidad de activar y desactivar campañas de déclencheur, lo que puede simplificar el proceso de automatización de las plantillas de programa. Esto se logra llamando a dos extremos recién agregados: [Activar campaña inteligente](https://developer.adobe.com/marketo-apis/api/asset/#operation/activateSmartCampaignUsingPOST), [Desactivar campaña inteligente](https://developer.adobe.com/marketo-apis/api/asset/#operation/deactivateSmartCampaignUsingPOST). Consulte la sección Déclencheur en la documentación de [Campañas](/help/rest-api/assets.md) para obtener más información.

### Obtener programa por nombre

Se agregaron dos parámetros al extremo [Obtener programa por nombre](https://developer.adobe.com/marketo-apis/api/asset/#operation/getProgramByNameUsingGET) para facilitar la recuperación de costos y etiquetas de programa. Consulte los parámetros **includeCosts** y **includeTags** en la documentación de [Programas](/help/rest-api/assets.md) para obtener más información.

### Otras mejoras

La API de extracción masiva ahora reconoce el espacio de trabajo. Cuando crea un usuario solo de API para un [servicio personalizado](/help/rest-api/custom-services.md), debe seleccionar un rol de usuario con acceso a API para uno o más espacios de trabajo. Anteriormente, el servicio personalizado tenía acceso a todos los espacios de trabajo. Ahora, el servicio personalizado solo tiene acceso a los espacios de trabajo seleccionados durante la creación de usuarios solo de API.

Publicado el _02-03-2018_ por _David_

## Actualizaciones de primavera de 2018

En la versión de primavera de 2018 lanzamos nuevas API de REST y mejoras en la privacidad de seguimiento web. Consulte la lista completa de actualizaciones a continuación.

API de REST

### Lista estática CRUD

Permite a los usuarios crear, leer, actualizar y eliminar registros de listas estáticas de forma remota. Permite la administración de todo el ciclo de vida de una lista estática mediante las API de REST, incluida la rellenación y el mantenimiento de la pertenencia. Encontrará detalles [aquí](/help/rest-api/static-lists.md).

### Metadatos de actividad personalizados

Permite a los usuarios crear de forma remota registros de actividad personalizados. Permite la administración de todo el ciclo de vida de las actividades personalizadas mediante las API de REST, incluida la manipulación de tipos y atributos de tipo. Encontrará detalles [aquí](/help/rest-api/activities.md).

### Metadatos de listas inteligentes

Permite a los usuarios leer, clonar y eliminar registros de listas inteligentes de forma remota. Habilita la administración de listas inteligentes mediante las API de REST. Encontrará detalles [aquí](/help/rest-api/smart-lists.md).

### Privacidad de seguimiento web

El código de seguimiento web de Munchkin JavaScript se ha mejorado para incluir las siguientes mejoras relacionadas con la privacidad:

* Exclusión: capacidad para permitir que los visitantes excluyan permanentemente el seguimiento web. Encontrará detalles [aquí](/help/javascript-api/lead-tracking.md).
* Anonimización de direcciones IP: cumpla con las regulaciones de privacidad locales e internacionales al proporcionar la capacidad de anonimizar las direcciones IP de los visitantes web. Consulte el parámetro de configuración **anonymizeIP** aquí[.](/help/javascript-api/configuration.md)

### Otras mejoras

* El extremo [Get Folders](https://developer.adobe.com/marketo-apis/api/asset/#operation/getFolderUsingGET) devuelve ahora todas las carpetas cuando se especifican un elemento principal no raíz y maxDepth=1.
* El punto de conexión [Obtener página de aterrizaje por identificador](https://developer.adobe.com/marketo-apis/api/asset/#operation/getLandingPageByIdUsingGET) ahora devuelve atributos de URL con el protocolo en todos los casos (http:// o https://).

Publicado el _2018-06-29_ por _David_

## Actualizaciones del verano de 2018

La versión de verano de 2018 es principalmente de mantenimiento y consta de mejoras menores y resoluciones de defectos. Consulte la lista completa de actualizaciones a continuación.

### API de REST

* Se ha agregado compatibilidad con los campos de disposición de correo electrónico que se omitieron originalmente de forma innecesaria. Estos campos ahora estarán disponibles para leer y escribir en REST, según corresponda.
   * En la lista de bloqueados
   * Marketing suspendido
   * Correo electrónico suspendido
   * Urgencia relativa
* El extremo [Obtener posibles clientes por tipo de filtro](https://developer.adobe.com/marketo-apis/api/lead-database-endpoint-reference/#/Leads/getLeadsByFilterUsingGET) ahora admite leadPartionId como filterType.

### Resoluciones de defectos

**Punto final REST**

**Descripción**

[Aprobar programa](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveProgramUsingPOST)

Si hubiera establecido Bloquear correos electrónicos no operativos en falso al crear el programa, una llamada a Aprobar programa se restablecería en verdadero.

[Extracción en lote](/help/rest-api/bulk-extract.md)

Algunos caracteres Unicode estaban dañados en el archivo de salida de extracción.

[Clonar programa](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST)

* Si clonó un programa de correo electrónico, la lógica del filtro SmartList se restableció a &quot;Todo&quot; en el programa resultante, independientemente de la configuración inicial.
* Si intentó clonar un programa que contenía una lista estática (eliminada), recibió el error 709, &quot;Los siguientes recursos no son compatibles:List&quot;.
* Si ha intentado clonar un programa en espacios de trabajo, ha recibido el error 611, &quot;No se puede clonar el programa&quot;.

[Obtener lista estática por identificador](https://developer.adobe.com/marketo-apis/api/asset/#operation/getStaticListByIdUsingGET)

Si el servicio personalizado tuviera permiso de función de recurso de solo lectura, recibiría el error 603, &quot;Acceso denegado&quot;.

[Insertar posible cliente en Marketo](https://developer.adobe.com/marketo-apis/api/mapi/#operation/pushToMarketoUsingPOST)

Si el atributo **cookies** se especificó en un objeto de posible cliente de matriz **input**, la actividad anónima anterior no se asoció con el posible cliente recién creado.

[Programar campaña](https://developer.adobe.com/marketo-apis/api/mapi/#operation/scheduleCampaignUsingPOST)

Si especificaste una fecha de **runAt** en un futuro lejano, entonces la campaña nunca se ejecutó y se devolvió **success=true**. Ahora, si la fecha **runAt** se acerca a más de 2 años en el futuro, se devuelve un error [1042](/help/rest-api/error-codes.md).

[Sincronizar posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncLeadUsingPOST)

Si especificó `action=createDuplicate` y un parámetro `externalCompanyId` (para asociar el nuevo posible cliente con una compañía existente), entonces el posible cliente se asoció con una compañía vacía (en lugar de con la compañía especificada).

#### Varios

* Se ha eliminado el siguiente valor de cambio de datos de todas las respuestas de extremos: mktoClientReqId. Esto fue solo para uso interno.
* Funcionalidad de búsqueda de errores añadida. Introduzca un código de error de API de REST en el cuadro de búsqueda y seleccione en la lista de autocompletar debajo para ir a la descripción del error.
* Se agregó la página [Referencia de extremo](/help/rest-api/endpoint-reference.md). Esta es una lista ordenable de todos los extremos de la API de REST en un solo lugar. También puede utilizar esta página para generar el conjunto mínimo de permisos que necesita la aplicación. Esto resulta útil al crear un servicio personalizado.

Publicado el _12-10-2018_ por _David_

## Actualizaciones de otoño de 2018

La versión de otoño de 2019 es principalmente de mantenimiento e incluye mejoras menores y resoluciones de defectos. Consulte la lista completa de actualizaciones a continuación.

### Mejoras

* Se ha agregado compatibilidad con [Campos CC de correo electrónico](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/email-marketing/general/email-cc) para [API de recursos](/help/rest-api/assets.md). La configuración del campo CC se propaga según lo esperado durante las operaciones de aprobación/clonación (aprobación de borrador de correo electrónico o plantilla de correo electrónico, correo electrónico o clonación de programa). Todos los extremos relacionados con el correo electrónico ahora devuelven valores de Campos CC en la propiedad **ccFields**. Desplácese hacia abajo en la respuesta para ver un ejemplo. Este cambio afecta a los siguientes extremos: [Obtener correo electrónico por identificador](https://developer.adobe.com/marketo-apis/api/asset/#operation/getEmailByIdUsingGET), [Obtener correo electrónico por nombre](https://developer.adobe.com/marketo-apis/api/asset/#operation/getEmailByNameUsingGET), [Obtener correos electrónicos](https://developer.adobe.com/marketo-apis/api/asset/#operation/getEmailUsingGET), [Aprobar borrador de correo electrónico](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveDraftUsingPOST), [Aprobar borrador de plantilla de correo electrónico](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveDraftUsingPOST_1), [Clonar correo electrónico](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneEmailUsingPOST), [Clonar programa.](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST)

```json
{
    "success": true,
    "errors": [],
    "requestId": "cc96#166e836348b",
    "warnings": [],
    "result": [
        {
            "id": 2061,
            "name": "Test Email",
            "description": "This is a test",
            "createdAt": "2018-11-06T05:27:10Z+0000",
            "updatedAt": "2018-11-06T08:33:16Z+0000",
            "url": "<https://app-sjqe.marketo.com/#EM2061A1LA1>",
            "subject": {
                "type": "Text",
                "value": "This is a test with CC Fields"
            },
            "fromName": {
                "type": "Text",
                "value": "Tommy Tester"
            },
            "fromEmail": {
                "type": "Text",
                "value": "<tommy.tester@marketo.com>"
            },
            "replyEmail": {
                "type": "Text",
                "value": "<tommy.tester@marketo.com>"
            },
            "folder": {
                "type": "Program",
                "value": 7494,
                "folderName": "Initiative"
            },
            "operational": false,
            "textOnly": false,
            "publishToMSI": false,
            "webView": false,
            "status": "approved",
            "template": 1218,
            "workspace": "Default",
            "version": 2,
            "autoCopyToText": true,
            "ccFields": [
                {
                    "attributeId": "855",
                    "objectName": "lead",
                    "displayName": "emailcc",
                    "apiName": "emailcc"
                },
                {
                    "attributeId": "857",
                    "objectName": "lead",
                    "displayName": "leadDetails",
                    "apiName": "leadDetails"
                },
                {
                    "attributeId": "859",
                    "objectName": "company",
                    "displayName": "headquarters",
                    "apiName": "headquarters"
                }
            ]
        }
    ]
}
```

### Resoluciones de defectos

* Se ha ajustado la compatibilidad con [Varios dominios de promoción de la marca](https://experienceleague.adobe.com/es/docs/marketo/using/home) para [Asset API](/help/rest-api/assets.md). Anteriormente, la configuración de varios dominios de promoción de la marca no se propagaba al aprobar un borrador de correo electrónico, al clonar un correo electrónico o al clonar un programa. Esto se ha corregido. Este cambio afecta a los siguientes extremos: [Aprobar borrador de correo electrónico](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveDraftUsingPOST), [Clonar correo electrónico](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneEmailUsingPOST), [Clonar programa.](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST)
* Se agregó el parámetro de configuración [apiOnly](/help/javascript-api/configuration.md). De forma predeterminada, las páginas web que contienen la etiqueta Munchkin activan el evento &quot;Visitas a página web&quot; cuando la página web se carga en el explorador. En algunos casos, esto no es deseable. Por ejemplo, aplicaciones web de una sola página que necesitan un control total de cuándo se activa este evento. Para admitir este caso de uso, hemos agregado un nuevo parámetro de configuración **apiOnly**. Cuando se establece en true, la etiqueta Munchkin no genera una actividad &quot;Visitas a página web&quot; durante la carga de la página.
* Se ha agregado la opción de configuración [domainSelectorV2](/help/javascript-api/configuration.md). De forma predeterminada, la etiqueta Munchkin no administra correctamente las páginas web alojadas en sitios con [dominios de nivel superior con código de país de dos letras](https://en.wikipedia.org/wiki/Country_code_top-level_domain) (ejemplos: .io, .co, .ly). Esto hace que el atributo de dominio de la cookie de Munchkin se establezca incorrectamente. Para lograr una mejor experiencia predeterminada, agregamos una nueva configuración de **domainSelectorV2**. Cuando se establece en true, se utiliza un algoritmo mejorado para establecer automáticamente el atributo de dominio de cookie de Munchkin.
* Se ha ajustado el dominio de cookie [Opt-Out](/help/javascript-api/lead-tracking.md). En algunos casos, el atributo de dominio de la cookie de exclusión de Munchkin (mkto_opt_out) se configuraba incorrectamente. La cookie de exclusión de Munchkin ahora usa la misma lógica que la cookie de Munchkin (_mkto_trk) para determinar el atributo de la cookie del dominio, incluido el cumplimiento de la configuración de **domainLevel**.
* Los desarrolladores de aplicaciones de Android ahora pueden usar directamente [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) (FCM) de Google con este SDK. Encontrará detalles [aquí](/help/mobile/installation.md).

Publicado el _2018-12-07_ por _David_

## Actualizaciones de primavera de 2019

La versión de primavera de 2019 es principalmente de mantenimiento e incluye mejoras menores y resoluciones de defectos. Consulte la lista completa de actualizaciones a continuación.

Publicado el _19-03-2019_ por _David_

## Actualizaciones de junio de 2019

La versión de junio de 2019 es principalmente de mantenimiento e incluye mejoras menores y resoluciones de defectos. Consulte la lista completa de actualizaciones a continuación.

### API de REST

1. Se ha agregado una suma de comprobación a los extremos de estado de exportación masiva. Puede comparar la suma de comprobación con un hash del archivo recuperado para comprobar la integridad del archivo recuperado. La suma de comprobación es un hash SHA-256 del archivo exportado y se almacena en el atributo fileCheckSum cuando se completa un trabajo de exportación.

Los siguientes extremos devuelven la suma de comprobación: [Obtener el estado del trabajo del posible cliente de exportación](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportLeadsStatusUsingGET), [Obtener los trabajos del posible cliente de exportación](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportLeadsFileUsingGET), [Obtener el estado del trabajo de actividad de exportación](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportLeadsStatusUsingGET), [Obtener los trabajos de actividad de exportación](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportActivitiesUsingGET)

#### Resoluciones de defectos

1. Se ha corregido un problema con [Importación masiva de objetos personalizados](/help/rest-api/bulk-custom-object-import.md) al importar números decimales en campos enteros. Antes de la corrección, el número decimal se convertía en un entero asignando la parte del número entero y descartando la parte fraccionaria (por ejemplo, 5,432 se convertía en 5). Ahora se genera el error &quot;Tipo de datos no válido en el campo Source ID&quot; para cada fila que contiene una discrepancia en los datos.
1. Se ha corregido un problema en el cual un programa de correo electrónico creado con el extremo [Clone Program](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST) no respetaba la configuración de límites de comunicación en algunos casos.
1. Se ha corregido un problema con el punto de conexión [Aprobar borrador de página de aterrizaje](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveLandingPageUsingPOST) por el que devolvía 611. &quot;Error del sistema&quot; cuando la página de aterrizaje contenía el formulario de cancelación de suscripción de correo electrónico.
1. Se ha corregido un problema con el punto de conexión [Aprobar borrador de página de aterrizaje](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveLandingPageUsingPOST) por el que devolvía 611. &quot;Error del sistema&quot; cuando la página de aterrizaje se clonó con el extremo [Clone Landing Page](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneLandingPageUsingPOST).

#### En desuso

1. Como parte de la obsolescencia de [Email Editor 1.0](https://nation.marketo.com:443/t5/knowledgebase/email-editor-1-0-is-being-deprecated-june-18th/ta-p/250666), la versión 1.0 de Email Assets será de solo lectura a finales de 2019. Todos los Assets de correo electrónico 1.0 deben convertirse a 2.0 como descriDescartar borrador de correo electrónico, insertar [aquí](https://nation.marketo.com:443/t5/knowledgebase/email-editor-1-0-is-being-deprecated-june-18th/ta-p/250666). Para ayudar a los desarrolladores a prepararse para ese evento, hemos agregado advertencias a todos los extremos relacionados con el correo electrónico que intentan modificar Assets de correo electrónico 1.0. Esta es una respuesta de ejemplo que contiene la advertencia:

`{` `"success": true,` `"errors": [],` `"requestId": "15c57#16b338d6e75",` `"warnings": [` `"This is a v1 email asset. API support for modifying v1 emails is being dropped, and this operation will not work on v1 emails in the future. To avoid service interruptions, upgrade this and related assets by editing them in the User Interface."` `],` `"result": [` `"{\"service\":\"sendTestEmail\",\"result\":true}"` `]` `}`

Los siguientes extremos relacionados con el correo electrónico devuelven la advertencia:

* Actualizar contenido completo del correo electrónico
* Actualizar contenido del correo electrónico
* Enviar muestra de email
* Desaprobar correo electrónico
* Clonar programa
* Clonar Email
* Aprobar borrador de correo electrónico
* Actualizar sección de contenido dinámico de correo electrónico
* Actualizar metadatos de correo electrónico
* Aprobar programa

Scripts de correo electrónico (Apache Velocity)

#### En desuso

1. Un subconjunto de la funcionalidad Secuencia de comandos de Velocity se deshabilitó por motivos de seguridad. Esto incluye los siguientes métodos y variables: getClass(), $class, $context, $text. Encontrará más información [aquí](https://nation.marketo.com:443/t5/knowledgebase/unsupported-velocity-tools-disabled-in-june-2019-release/ta-p/251177).

Publicado el _14-06-2019_ por _David_

## Actualizaciones de agosto de 2019

En agosto de 2019 lanzaremos nuevas API de REST, mejoraremos las API existentes y resolveremos defectos. Consulte la lista completa de actualizaciones a continuación.

### Mejoras

1. Funciones mejoradas del ciclo vital de Smart Campaign. Se han añadido nuevos puntos de conexión para permitirle realizar las siguientes operaciones en campañas inteligentes: obtener por nombre, crear, actualizar, clonar y eliminar. Encontrará información completa [aquí](/help/rest-api/smart-campaigns.md).
1. Puntos finales de listas inteligentes mejorados para mejorar las capacidades de consulta.
   1. El extremo [Get Smart List by Id](https://developer.adobe.com/marketo-apis/api/asset/#operation/getSmartListByIdUsingGET) ahora devuelve descripciones de reglas de listas inteligentes (déclencheur y filtros) cuando pasa el parámetro booleano **includeRules**.
   1. El punto de conexión [Obtener listas inteligentes](https://developer.adobe.com/marketo-apis/api/asset/#operation/getSmartListsUsingGET) ahora le permite filtrar los resultados por intervalo de fechas cuando pasa los parámetros de fecha y hora **soonUpdatedAt** y **latestUpdatedAt**. Además, este extremo ahora devuelve listas inteligentes que son miembros de campañas y programas de correo electrónico.
1. Se han agregado puntos finales para extraer definiciones de listas inteligentes.
   1. Obtener la lista inteligente [por el identificador de campaña inteligente](https://developer.adobe.com/marketo-apis/api/asset/#operation/getSmartListBySmartCampaignIdUsingGET) devuelve el registro de lista inteligente para un identificador de campaña inteligente determinado.
   1. El extremo Obtener [lista inteligente por id. de programa](https://developer.adobe.com/marketo-apis/api/asset/#operation/getSmartListByProgramIdUsingGET) devuelve el registro de lista inteligente para un id. de programa determinado.
1. Se ha mejorado el punto de conexión [Actualizar contenido de correo electrónico](https://developer.adobe.com/marketo-apis/api/asset/#operation/updateEmailContentUsingPOST) para permitir actualizaciones de los campos de encabezado de correo electrónico de los mensajes de correo electrónico que se han separado de su plantilla (asunto, nombre, correo electrónico, responder a). Los elementos rotos de la plantilla se describen [aquí](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/edit-an-emails-html).

### Resoluciones de defectos

1. Se corrigió un problema en el cual llamar a [Eliminar página de aterrizaje](https://developer.adobe.com/marketo-apis/api/asset/#operation/deleteLandingPageByIdUsingPOST) en una página de aterrizaje aprobada eliminaba la página de aterrizaje. Ahora devuelve correctamente un error &quot;709, La página de aterrizaje aprobada no se puede eliminar&quot;. [LM-127271]
1. Se ha corregido un problema con el extremo [Enviar correo electrónico de muestra](https://developer.adobe.com/marketo-apis/api/asset/#operation/sendSampleEmailUsingPOST) por el que devolvía 611. &quot;Error del sistema&quot; cuando el correo electrónico se ha separado de su plantilla. [LM-127288]
1. Se ha corregido un problema con el extremo [Delete Program](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST) en el cual, en algunos casos, se eliminaba un programa en uso en lugar de emitir un &quot;709, No se puede eliminar el programa. Los recursos están en uso en otra parte o error &quot;no eliminable&quot;. [LM-125431]

### En desuso

1. La compatibilidad con la API del Editor de correo electrónico 1.0 está programada para dejar de usarse en enero de 2020. Recuerde convertir sus recursos a la versión 2.0 antes de esa fecha. Los intentos de escribir o clonar recursos de Email 1.0 después de enero resultarán en errores en lugar de advertencias. Obtenga más información acerca de las API de correo electrónico [aquí](https://nation.marketo.com:443/t5/knowledgebase/email-2-0-and-email-api-faq-s/ta-p/251423).
1. Para alinearnos con el estándar de seguridad de primer nivel de Adobe, dejaremos de ofrecer soporte para Transport Layer Security (TLS) 1.0 y 1.1 a partir del 13 de diciembre de 2019. Los sistemas integrados con Marketo que no cumplan con el protocolo 1.2 podrían perder el acceso a los servicios de Marketo Engage. Para mantener su acceso a Marketo Engage, asegúrese de que todos los sistemas cliente sean compatibles con TLS 1.2 antes del 13 de diciembre de 2019. Encontrará información más detallada [aquí](https://nation.marketo.com:443/t5/knowledgebase/tls-1-0-1-1-deprecation-faq/ta-p/249085).

1. Todo el contenido relacionado con campañas inteligentes ahora reside en el elemento de menú [Campañas inteligentes](/help/rest-api/smart-campaigns.md) (debajo de API de REST > Assets).

Publicado el _16-08-2019_ por _David_

## Actualizaciones de enero de 2020

En enero de 2020 lanzaremos nuevas API de REST, mejoraremos las API existentes y resolveremos defectos. Consulte la lista completa de actualizaciones a continuación.

* Se ha añadido la capacidad de crear mediante programación definiciones de esquema de objeto personalizado. Esto le permite definir un objeto personalizado una vez y aprovisionarlo en tantas instancias como sea necesario. Esto le permite a los usuarios aprovechar de forma eficaz los modelos de zona protegida y centro de excelencia. También permite a los ISV simplificar el proceso de incorporación del cliente. Necesita un tipo de suscripción adecuado para acceder a la API de metadatos de objeto personalizado.
* Se ha agregado la capacidad de importar y exportar miembros del programa de forma masiva. Este nuevo conjunto de puntos de conexión sigue el patrón de API de REST de Marketo existente para crear trabajos de procesamiento masivo asincrónicos. Los registros de miembros del programa pueden contener campos personalizados de miembros del programa o campos de posibles clientes.
* Se ha agregado el punto de conexión Obtener formulario disponible para campos de miembros de programa para admitir el uso de campos personalizados de miembros de programa como campos de formulario. Esto devuelve una lista de todos los campos personalizados de miembros de programa que se pueden utilizar en un formulario Marketo Forms.
* Se agregó [Obtener plantilla de correo electrónico usada por un extremo &#x200B;](/help/rest-api/email-templates.md) que devuelve una lista de recursos de correo electrónico que dependen de una plantilla de correo electrónico determinada. Esto le permite comprender rápidamente el impacto de un posible cambio de plantilla de correo electrónico y abordar más fácilmente estas dependencias.

Publicado el _2020-01-17_ por _David_

## Cómo recuperar cada objeto personalizado

A menudo se nos pregunta cómo usar la API de Marketo para obtener una lista de todos los [objetos personalizados](https://experienceleague.adobe.com/es/docs/marketo/using/home) (CO). La consulta de CO requiere algo más que su nombre: también se requiere algún conocimiento _a priori_ sobre cada CO. Los métodos para obtener ese conocimiento pueden no ser obvios, ya que la API no proporciona ningún método para consultarlo directamente. Al igual que con muchos objetivos en Marketo Engage, las listas inteligentes proporcionan una respuesta para los CO vinculados a personas (posibles clientes). Las listas inteligentes funcionan de forma diferente con Compañías y usted terminará con una lista de todas las Personas cuyas Compañías están vinculadas al tipo de objeto para el filtro, por lo que puede que necesite deduplicar compañías según sus objetivos. Cada vez que se aprueba un nuevo objeto personalizado, se crea un filtro asociado. Se le asignará un nombre con el formato &quot;**Tiene CO NAME**&quot;. En el ejemplo siguiente, el nombre de objeto personalizado es &quot;**Suscripción a seguimiento de conferencia&quot;** y su filtro se denomina &quot;**Tiene suscripción a seguimiento de conferencia**&quot;. Una vez creada la Smart List, puede recuperar la información necesaria para consultar los CO asociados mediante el [extremo de objetos personalizados](/help/rest-api/custom-objects.md). Exporte la lista para asegurarse de que se incluye el campo vinculado (ID o dirección de correo electrónico). Puede exportar con el filtrado de la API [extracción masiva de posibles clientes](/help/rest-api/bulk-lead-extract.md) mediante el filtro **smartListName** o **smartListId**, o bien [exportar desde la interfaz de usuario](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/core-marketo-concepts/smart-lists-and-static-lists/managing-people-in-smart-lists/export-people-to-excel-from-a-list-or-smart-list). En el siguiente paso, utilizará cada valor de campo vinculado para consultar individualmente los objetos personalizados asociados. El nombre del objeto personalizado es **&quot;Suscripción a seguimiento de conferencia&quot;** en este ejemplo y su nombre de API es **conferenceTrackSubscription_c**. Encuentra el nombre de la API tanto en la interfaz de usuario como &quot;**Nombre de la API**&quot; y a través de la API como &quot;**nombre**&quot;.  Administrador | Objetos personalizados de Marketo[/caption] Y aquí hay un fragmento devuelto por el extremo de la API [List Custom Objects](https://developer.adobe.com/marketo-apis/api/mapi/#operation/listCustomObjectsUsingGET):

```json
{
    "name": "conferenceTrackSubscription_c",
    "displayName": "Conference Track Subscription",
    "description": "Track subscription for conference attendees",
    "createdAt": "2020-01-13T19:50:06Z",
    "updatedAt": "2020-01-13T19:50:06Z",
    "idField": "marketoGUID",
    "dedupeFields": [
        "subscriptionID"
    ],
    "searchableFields": [
        [
            "subscriptionID"
        ],
        [
            "marketoGUID"
        ],
        [
            "leadID"
        ]
    ],
    "relationships": [
        {
            "field": "leadID",
            "type": "child",
            "relatedTo": {
                "name": "Lead",
                "field": "Id"
            }
        }
    ]
}
```

Para recuperar los objetos personalizados asociados uno a uno (1:1) o uno a varios (1:N) con las personas de la lista inteligente, realice una solicitud como esta:

`GET /rest/v1/customobjects/conferenceTrackSubscription_c.json?filterType=leadID&filterValues=1000302,1000303,1000304,1000306,1000307`

En este ejemplo, este objeto personalizado está vinculado a Persons por el campo **leadID**, por lo que el tipo de filtro es &quot;**leadID**&quot;. El parámetro de valores de filtro es una lista separada por comas de los ID tomados de la exportación de listas inteligentes. La solicitud puede incluir tantos valores de filtro como se pueda incluir en un único URI de solicitud: hasta 800 caracteres. Las solicitudes que superan esta longitud devuelven un código de error de nivel HTTP 414. La respuesta se puede devolver en más de un fragmento. Si es así, **moreResult** será **true** y se incluirá **nextPageToken**. Luego necesitará [pasar página por](/help/rest-api/paging-tokens.md) los resultados hasta que **moreResult** sea **false**. Esta es parte del resultado de la solicitud de API anterior:

```json
"result": [
    {
        "seq": 0,
        "marketoGUID": "d6b3ed3d-4eb8-4066-a9cd-184c8d385cfe",
        "leadID": "1000302",
        "subscriptionID": "4ad59184-6bf1-4eeb-a583-d82aeee68210",
        "createdAt": "2020-01-13T21:24:04Z",
        "updatedAt": "2020-01-13T21:24:04Z"
    },
    {
        "seq": 1,
        "marketoGUID": "e5e0aba4-f27f-494d-93ed-9cb580989bf3",
        "leadID": "1000303",
        "subscriptionID": "fc5596d5-6fa2-4848-b4a2-89d96e245c59",
        "createdAt": "2020-01-13T21:24:04Z",
        "updatedAt": "2020-01-13T21:24:04Z"
    },
    {
        "seq": 2,
        "marketoGUID": "e65007cd-86b1-4c17-8d55-057c96e1788a",
        "leadID": "1000304",
        "subscriptionID": "7e54b8a0-2170-4d81-a809-4eac349508d0",
        "createdAt": "2020-01-13T21:24:04Z",
        "updatedAt": "2020-01-13T21:24:04Z"
    },
    {
        "seq": 3,
        "marketoGUID": "39d956b2-85e2-4c24-94e7-e9fa5a09d3d0",
        "leadID": "1000306",
        "subscriptionID": "644c8e5b-fc0c-4d4a-80f8-358a65ce0a68",
        "createdAt": "2020-01-13T21:24:04Z",
        "updatedAt": "2020-01-13T21:24:04Z"
    },
    {
        "seq": 4,
        "marketoGUID": "a2151350-50c8-437f-bc71-7a054bb601f0",
        "leadID": "1000307",
        "subscriptionID": "bf14218c-ae6a-42b3-a14e-f7182903cbcd",
        "createdAt": "2020-01-13T21:24:04Z",
        "updatedAt": "2020-01-13T21:24:04Z"
    }
```

Ahora tiene los valores de cada objeto personalizado directamente asociado con las personas de su lista inteligente y, más allá de recuperar los valores, puede usar **marketoGUID** para [actualizar](/help/rest-api/custom-objects.md) o [eliminar](/help/rest-api/custom-objects.md) estos objetos. Para los objetos personalizados asociados con Personas en una relación de varios a varios (N:N), la técnica anterior devuelve el objeto de primer nivel que es el objeto intermedio que conecta cada Persona con varios CO de segundo nivel.

Para recuperar los CO de segundo nivel, inicie un nuevo conjunto de consultas para el tipo de CO de segundo nivel filtrando en el campo de vínculo y los valores extraídos del objeto intermedio de primer nivel. Por ejemplo, el objeto &quot;**Suscripción a seguimiento de conferencia&quot;** anterior podría tener otro nivel de objetos que representen sesiones denominadas **&quot;Sesión&quot;** que probablemente estarían vinculadas por **subscriptionID**. La solicitud para recuperar sesiones vinculadas a las suscripciones de seguimiento de conferencia anteriores tendría este aspecto:

`GET /rest/v1/customobjects/session_c.json?filterType=subscriptionID&filterValues=4ad59184-6bf1-4eeb-a583-d82aeee68210,e5e0aba4-f27f-494d-93ed-9cb580989bf3,e65007cd-86b1-4c17-8d55-057c96e1788a,39d956b2-85e2-4c24-94e7-e9fa5a09d3d0,bf14218c-ae6a-42b3-a14e-f7182903cbcd`

Los tipos de filtro _Footnote_ _1)**smartListName**&#x200B;y **smartListId**&#x200B;no están disponibles para algunas suscripciones. Si no está disponible para su suscripción, recibirá un error al llamar al extremo del trabajo de creación de posibles clientes (**&quot;1035, tipo de filtro no admitido para la suscripción de destino&quot;**). Los clientes pueden ponerse en contacto con el Soporte técnico de Marketo para habilitar esta funcionalidad en su suscripción._

Publicado el _2020-01-14_ por _Tony_

## Cómo recuperar a cada persona (posible cliente)

Recibimos muchas consultas sobre los procesos necesarios para recuperar cada persona (posible cliente) de una instancia de Marketo Engage. Hemos proporcionado muchas respuestas útiles, pero ninguna ha sido tan completa como esta. He identificado algunos conceptos clave necesarios para extraer cada posible cliente mediante la API de extracción masiva de Marketo. Todos los demás detalles se pueden aprender del código de demostración que creé para ir con él. Después de leer esta publicación y explorar el código de demostración, tendrá toda la información que necesita saber para recuperar todos los posibles clientes de una instancia de Marketo Engage.

### Información general

La técnica principal usa la [API de extracción masiva de posibles clientes](/help/rest-api/bulk-lead-extract.md). Es probable que pueda crear un trabajo de exportación de posibles clientes en lotes sin ningún filtro, pero no puede: **la API requiere un filtro**. Los filtros disponibles son **createdAt**, **staticListName**, **staticListId,** **updatedAt**, **smartListName** y **smartListId**. El hecho de filtrar por una lista inteligente sin filtros también parece atractivo. Pruebe esto y verá que el sistema es lo suficientemente inteligente como para tratar una lista inteligente sin filtros de la misma manera: la API también requiere un filtro aquí. Como necesitamos un filtro, el filtro confiable y canónico que se debe usar es **createdAt**. Este tipo de filtro permite intervalos de fecha y hora de hasta 31 días, por lo que necesitamos ejecutar varios trabajos y combinar los resultados. Empezaremos por encontrar la fecha de creación más antigua posible para un posible cliente en la instancia de destino. A partir de la fecha más antigua posible, creamos trabajos que abarcan 31 días menos un segundo (más adelante). Después de crear cada trabajo, lo pondremos en cola y esperaremos a que se complete. Luego descargaremos el archivo resultante y comprobaremos su integridad usando una suma de comprobación. Y, por último, deduplique los posibles clientes por ID y, a continuación, escriba posibles clientes únicos en un archivo CSV de salida.

### Encontrar el posible cliente más antiguo

Estoy utilizando un pequeño &quot;truco&quot; para obtener la fecha más antigua posible para un posible cliente en la instancia de destino. No hay ningún punto final de API dedicado a esa tarea, así que necesitamos un poco de creatividad. Lo que hago es consultar todas las carpetas con un **maxDepth = 1** que nos dará una lista de todas las carpetas de nivel superior de la instancia. Luego recopilo las fechas **createdAt**, las analizo y busco la fecha más antigua. Este método funciona porque algunas carpetas predeterminadas de nivel superior se crean con la instancia y no se podía crear ningún posible cliente antes de entonces.

### Seleccionar campos obligatorios

Debe decidir qué campos debe extraer. Encuentre los campos disponibles para su instancia de destino usando el [punto final de Describir posible cliente 2](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeUsingGET_2). La respuesta a esa solicitud incluirá una lista denominada &quot;campos&quot;. Este es un extracto de una respuesta de ejemplo:

```json
  "fields": [
      {
          "name": "AccountSource",
          "displayName": "Account Source",
          "dataType": "string",
          "length": 40,
          "updateable": true,
          "crmManaged": false
      },
      {
          "name": "acquisitionProgramId",
          "displayName": "Acquisition Program",
          "dataType": "reference",
          "updateable": true,
          "crmManaged": false
      },
      {
          "name": "Active_c",
          "displayName": "Active",
          "dataType": "string",
          "length": 255,
          "updateable": true,
          "crmManaged": false
      },
      {
          "name": "address",
          "displayName": "Address",
          "dataType": "text",
          "updateable": true,
          "crmManaged": false
      },
      {
          "name": "Address_lead",
          "displayName": "Address (L)",
          "dataType": "string",
          "length": 255,
          "updateable": true,
          "crmManaged": false
      },
      {
          "name": "annualRevenue",
          "displayName": "Annual Revenue",
          "dataType": "currency",
          "updateable": true,
          "crmManaged": false
      },
      {
          "name": "anonymousIP",
          "displayName": "Anonymous IP",
          "dataType": "string",
          "length": 255,
          "updateable": true,
          "crmManaged": false
      }, ...
```

Este extremo devuelve una lista exhaustiva que incluye campos estándar y personalizados. Cuantos más campos solicite, más tardará en completarse el trabajo de exportación y mayor será el archivo resultante. Normalmente, debe elegir solo los campos que necesite. Nada impide solicitar todos los campos disponibles, por lo que estoy demostrando que es así. El identificador de campo necesario al crear un trabajo de exportación es el valor **name**. Extraeré los valores de nombre a una lista de todos los nombres de campo. A continuación, lo utilizaré para solicitar todos los campos disponibles al crear cada trabajo de exportación.

### Exportar Intervalos De Fechas De Trabajo: 31 Días Cada Uno

Cada trabajo de exportación puede abarcar hasta 31 días. La instancia de demostración que estoy utilizando se creó en agosto de 2016, por lo que necesito crear un poco más de 40 trabajos hoy. Es el número de días transcurridos desde la primera fecha de creación dividida por 31 redondeados hacia arriba. La API permite procesar dos trabajos de exportación a la vez para que pueda realizar la extracción con dos trabajos que se ejecuten en paralelo. Los trabajos de extracción masiva son un recurso compartido con todas las demás integraciones, así que voy a ser amable. Dejo el otro trabajo disponible para otras integraciones y demostraré la ejecución de trabajos individuales uno tras otro. Las fechas utilizadas para el filtro **createdAt** tienen el formato [especificación ISO 8601](https://www.w3.org/TR/NOTE-datetime). Siempre están en GMT (Z+0000), por lo que la zona horaria será simplemente &quot;Z&quot; o &quot;+00:00&quot;. El 1 de agosto de 2016 es **2016-08-01T00:00:00+00:00** y 31 días después es el 1 de septiembre de 2016, que es **2016-09-01T00:00:00+00:00.** Tanto la hora de inicio como la de finalización son inclusivas, así que voy a restar 1 segundo de la hora de finalización: **2016-09-01T00:00:00+00:00** se convierte en **2016-08-31T23:59:59+00:00**. Restar un segundo evita tiempos superpuestos. Como GMT es el valor predeterminado, también puede dejar la **Z** o la **+00:00** desactivada.

### Deduplicación

Aunque me he tomado la molestia de evitar tiempos superpuestos, también he implementado la deduplicación. Lo hice porque hay algunos casos extremos en los que los tiempos cambian ([Horario de verano](https://en.wikipedia.org/wiki/Daylight_saving_time)), lo que da como resultado valores ambiguos y, como resultado, la API de extracción masiva de Marketo puede devolver posibles clientes duplicados inesperados. Es raro que esto ocurra, pero debe tenerse en cuenta en cualquier integración que utilice intervalos de filtros de fecha y hora. He eliminado un segundo para dejar claro que los tiempos son inclusivos. No me gustaría que pienses que crear un trabajo con las **createdAt** y **endAt** veces de **2016-08-01T00:00:00Z** y **2016-09-01T00:00:00Z** respectivamente, no incluirá los posibles clientes creados el **2016-09-01T00:00:00Z**; lo hará.

### Crear un trabajo

El primer paso es crear un trabajo con [Crear extremo de trabajo de posible cliente de exportación](https://developer.adobe.com/marketo-apis/api/mapi/#operation/createExportLeadsUsingPOST). En esta demostración, la solicitud para crear su primer trabajo de exportación tiene este aspecto:

`POST /bulk/v1/leads/export/create.json`

```json
{ "filter": {"createdAt": {"startAt": "2016-08-01T00:00:00",
                           "endAt": "2016-09-01T00:00:00"}},
"fields":["AccountSource","acquisitionProgramId","Active_c","address","Address_lead","annualRevenue","anonymousIP","BillingAddress","billingCity","billingCountry","BillingGeocodeAccuracy","BillingLatitude","BillingLongitude","billingPostalCode","billingState","billingStreet","blackListed","blackListedCause","city","CleanStatus","CleanStatus_account","company","CompanyDunsNumber","contactCompany","cookies","country","createdAt","customfield","DandbCompanyId","DandbCompanyId_account","dateOfBirth","dddS","department","doNotCall","doNotCallReason","dS","dueDate","DunsNumber","email","EmailBouncedDate","EmailBouncedReason","emailInvalid","emailInvalidCause","emailSuspended","emailSuspendedAt","emailSuspendedCause","externalCompanyId","externalSalesPersonId","facebookDisplayName","facebookId","facebookPhotoURL","facebookProfileURL","facebookReach","facebookReferredEnrollments","facebookReferredVisits","fax","firstName","gender","GeocodeAccuracy","holll","id","industry","inferredCity","inferredCompany","inferredCountry","inferredMetropolitanArea","inferredPhoneAreaCode","inferredPostalCode","inferredStateRegion","interested","interestedIn","isAnonymous","IsEmailBounced","isLead","iSTRUE","Jigsaw","JigsawCompanyId_account","JigsawContactId_lead","Jigsaw_account","Languages_c","lastName","LastReferencedDate","LastReferencedDate_account","lastReferredEnrollment","lastReferredVisit","LastViewedDate","LastViewedDate_account","Latitude","leadPartitionId","leadPerson","leadRevenueCycleModelId","leadRevenueStageId","leadRole","leadScore","leadSource","leadStatus","linkedInDisplayName","linkedInId","linkedInPhotoURL","linkedInProfileURL","linkedInReach","linkedInReferredEnrollments","linkedInReferredVisits","links","Longitude","MailingAddress","MailingGeocodeAccuracy","MailingLatitude","MailingLongitude","mainPhone","marketingSuspended","marketingSuspendedCause","middleName","mktoAcquisitionDate","mktocomment1","mktocomments2","mktoCompanyNotes","mktoDoNotCallCause","mktoIsCustomer","mktoIsPartner","mktoName","mktoPersonNotes","mktosync","mktotest1","mobile","mobilePhone","NaicsCode","NaicsDesc","newcustom","numberOfEmployees","originalReferrer","originalSearchEngine","originalSearchPhrase","originalSourceInfo","originalSourceType","OtherAddress","OtherGeocodeAccuracy","OtherLatitude","OtherLongitude","personPrimaryLeadInterest","personTimeZone","personType","phone","PhotoUrl","PhotoUrl_account","postalCode","priority","ProductInterest_c","rating","referal","registrationSourceInfo","registrationSourceType","relativeScore","relativeUrgency","requiredNumberofCylinder","salutation","sfdcAccountId","sfdcContactId","sfdcId","sfdcLeadId","sfdcLeadOwnerId","sfdcType","ShippingAddress","ShippingGeocodeAccuracy","ShippingLatitude","ShippingLongitude","sicCode","SicDesc","site","state","surveyAnswers","syndicationId","testBooleanField","testscore","title","totalReferredEnrollments","totalReferredVisits","Tradestyle","twitterDisplayName","twitterId","twitterPhotoURL","twitterProfileURL","twitterReach","twitterReferredEnrollments","twitterReferredVisits","uNSUBSCIBE","unsubscribed","unsubscribedReason","updatedAt","urgency","url","website","YearStarted"]}
```

La respuesta tiene este aspecto:

```json
{
  "requestId": "6902#16fb52118bf",
  "result": [
      {
          "exportId": "4f2b9115-c3f2-4e40-a87c-bf803bbfed99",
          "format": "CSV",
          "status": "Created",
          "createdAt": "2020-01-17T20:10:43Z"
      }
  ],
  "success": true
}
```

### Poner el trabajo en cola

El trabajo ahora está creado, pero simplemente sentado allí sin hacer nada. Para ejecutar el trabajo, tenemos que llamar al [extremo en cola](https://developer.adobe.com/marketo-apis/api/mapi/#operation/enqueueExportLeadsUsingPOST) con el valor **exportId** para generar el URI de la solicitud. Esto tiene este aspecto:

`POST /bulk/v1/leads/export/4f2b9115-c3f2-4e40-a87c-bf803bbfed99/enqueue.json`

No hay cuerpo para este POST, simplemente estamos usando el verbo HTTP POST aquí. Esa solicitud generará una respuesta como esta:

```json
{
  "requestId": "1836a#16fb5238a48",
  "result": [
      {
          "exportId": "4f2b9115-c3f2-4e40-a87c-bf803bbfed99",
          "format": "CSV",
          "status": "Queued",
          "createdAt": "2020-01-17T20:10:43Z",
          "queuedAt": "2020-01-17T20:13:23Z"
      }
  ],
  "success": true
}
```

Como he mencionado anteriormente, el número de trabajos que se pueden ejecutar a la vez es limitado. También hay un límite en el número de trabajos en cola al mismo tiempo: 10. Necesitamos más de 40 para que ese límite nos impida crear todos los puestos de trabajo a la vez. Otras integraciones también pueden ejecutar trabajos, por lo que debemos tener en cuenta la posibilidad de que todas las ranuras estén llenas. Si intenta poner en cola un nuevo trabajo cuando ya hay 10 trabajos en cola, se generará un error de [1029](/help/rest-api/error-codes.md). Si recibes un **1029**, usa un retroceso exponencial hasta que el trabajo se pueda poner en cola. Espero 1 minuto y duplico ese valor cada vez que recibo un código de error de **1029** hasta 4 minutos entre solicitudes, pero nunca más que eso. Esta técnica se conoce como [Retroceso exponencial binario truncado](https://devopedia.org/binary-exponential-backoff) y es una práctica recomendada para errores recuperables y comprobaciones de estado.

### Esperar a que se complete el trabajo

Cada trabajo tarda un poco en ejecutarse, así que llamaremos al [extremo de estado](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportLeadsStatusUsingGET) para monitorizar su progreso. De nuevo, incluiremos **exportId** en el URI de solicitud de la siguiente manera:

`GET /bulk/v1/leads/export/4f2b9115-c3f2-4e40-a87c-bf803bbfed99/status.json`

Antes de que se complete el trabajo, obtiene una respuesta similar a esta:

```json
{
    "requestId": "153cb#16fb525435d",
    "result": [
        {
            "exportId": "4f2b9115-c3f2-4e40-a87c-bf803bbfed99",
            "format": "CSV",
            "status": "Processing",
            "createdAt": "2020-01-17T20:10:43Z",
            "queuedAt": "2020-01-17T20:13:23Z",
            "startedAt": "2020-01-17T20:13:49Z"
        }
    ],
    "success": true
}
```

Ejecuto el mismo retroceso exponencial (de 1 minuto a 4 minutos) mientras el estado del trabajo es &quot;En cola&quot;. El estado no es en tiempo real; se actualiza una vez por minuto y el sondeo es muy poco beneficioso más rápido. Restablezca el retroceso cuando el estado del trabajo cambie a Procesando. Estamos esperando un estado &quot;Completado&quot;, que tiene este aspecto:

```json
{
  "requestId": "10ad9#16fb5268f9b",
  "result": [
      {
          "exportId": "4f2b9115-c3f2-4e40-a87c-bf803bbfed99",
          "format": "CSV",
          "status": "Completed",
          "createdAt": "2020-01-17T20:10:43Z",
          "queuedAt": "2020-01-17T20:13:23Z",
          "startedAt": "2020-01-17T20:13:49Z",
          "finishedAt": "2020-01-17T20:15:28Z",
          "numberOfRecords": 59,
          "fileSize": 74436,
          "fileChecksum": "sha256:de553362e7ffad6556ae9ea749655c35010c7f0e944fc5a85782183130dca18d"
      }
  ],
  "success": true
}
```

El valor **numberOfRecords** es cero cuando la solicitud no devuelve posibles clientes. Comprobación de este valor y omito los pasos siguientes cuando tiene sentido. Cuando se devuelven posibles clientes, extraigo el valor **fileChecksum**. Lo utilizamos para comprobar la integridad del archivo cuando se descarga.

### Obtener sus posibles clientes

Si **numberOfRecords** es superior a cero, descargue el archivo exportado mediante [Obtener archivo de cliente potencial de exportación](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportLeadsFileUsingGET) con una solicitud como esta:

`GET /bulk/v1/leads/export/4f2b9115-c3f2-4e40-a87c-bf803bbfed99/file.json`

Compruebe que el archivo se transfirió sin errores: calcule la suma de comprobación del archivo y compárelo con la **suma de comprobación de archivo** que guardamos anteriormente. Calcule la suma de comprobación utilizando [SHA-2](https://en.wikipedia.org/wiki/SHA-2) y específicamente la función hash SHA256. Si las sumas de comprobación calculadas no coinciden, se ha producido un error en la transferencia de archivos y puede volver a intentar la transferencia o cancelar y recuperar manualmente.

### Agregar los datos

Después de recorrer cada rango de 31 días desde la primera ventaja hasta hoy, tendrás un set completo. También tendrá un archivo para cada rango. La forma más sencilla de generar un solo archivo agregado con cada posible cliente sería concatenar estos archivos después de eliminar la fila de encabezado para todos los archivos excepto el primero. Si lo hace, no olvide esperar y planificar la posible duplicación más adelante en su proceso de procesamiento de datos. En mi demostración, proceso los archivos a medida que los descargo. Antes de agregar cada fila de datos al archivo de salida, anulo la duplicación comprobando si ya se ha escrito el ID de posible cliente de la fila.

He desarrollado código de demostración alojado [aquí](https://github.com/Marketo/REST-Sample-Code/tree/master/python/LeadDatabase/Leads) que esperamos complete los detalles de este proceso y que pueda servir como plantilla para su propio desarrollo. El código de demostración está diseñado para ser una herramienta de aprendizaje, por lo que se requieren mejoras en la solidez para un sistema de producción. **El código se proporciona AS-IS** bajo la licencia de MIT, pero probablemente sea lo suficientemente bueno para un uso único con supervisión humana. ¡Nada te detiene ahora! Cuando siga este proceso, extraerá todos los posibles clientes mediante la API de extracción masiva de Marketo y, posiblemente, todos los campos de la instancia de Marketo Engage de destino. Para ampliar aún más los datos, obtenga las actividades de cada posible cliente mediante las técnicas.

Publicado el _2020-03-05_ por _Tony_

## Actualizaciones de febrero de 2020

En febrero de 2020 lanzaremos nuevas API de REST. Consulte la lista completa de actualizaciones a continuación.

### Anuncios

* A partir de septiembre de 2020, los puntos de conexión de [Asset API](/help/rest-api/assets.md) dejarán de aceptar el parámetro de consulta **_method**. Se utilizó para pasar parámetros de consulta en un cuerpo de POST para omitir las limitaciones de longitud de URI. Para dar cabida a las solicitudes que requerían este parámetro, el límite de URI para las API de recursos aumentará de 6 KiB a 65 KiB.
* Con respecto a nuestra posición en ITP, consulte esta publicación de la comunidad de Marketo: [Actualizaciones de cookies del explorador: Cómo se ve afectado Marketo/Munchkin](https://nation.marketo.com:443/t5/knowledgebase/browser-cookie-updates-how-marketo-munchkin-is-affected/ta-p/251524)
* Se ha realizado un cambio en la actividad &quot;Cambiar estado en progresión&quot;. Se ha agregado el atributo &quot;ID de miembro de programa&quot; a para ofrecer compatibilidad con la próxima función &quot;Campos personalizados de miembro de programa&quot;.

Publicado el _2020-02-26_ por _David_

## Obsolescencia del método de posible cliente asociado de Munchkin

Con la próxima versión del cliente JavaScript de Munchkin, versión 159, comenzaremos la desaprobación del método de cliente potencial asociado [de Munchkin](/help/javascript-api/api-reference.md). A partir de esta versión, cuando se invoque el método, se emitirá una advertencia en la consola del explorador indicando que el método se eliminará en una versión futura. Una vez eliminado el método, los intentos de utilizarlo resultarán en un error. Los clientes de Marketo que hayamos identificado como que han utilizado este método recientemente recibirán una notificación individual de su uso. Para obtener más información sobre la versión de Munchkin 159, [vea esta publicación de nación de marketing](https://nation.marketo.com:443/t5/product-documents/munchkin-javascript-version-159-amp-associate-lead-deprecation/ta-p/299687).

### Preguntas frecuentes

¿Cómo puedo saber si estoy afectado?

Adobe notificará a los clientes si hemos observado el uso de este método en su suscripción y lo hará varias veces durante el periodo de desuso.

¿Cuándo se eliminará el método?

Este método se eliminará en Munchkin JS v161, programado para su disponibilidad general junto con la versión de Marketo de octubre de 2021.

¿Por qué se elimina este método?

Desde la introducción de este método se han implementado y publicado formas más eficientes de cumplir los mismos casos de uso. Para mejorar el rendimiento y el estado de nuestros servicios, a veces es necesario eliminar las funciones que no cumplen con los estándares aceptables.

¿Qué debo usar en lugar de este método?

El posible cliente asociado de Munchkin tiene dos casos de uso principales, el envío de datos de persona y la asociación de una cookie de seguimiento web de explorador al registro de persona correspondiente en Marketo. Desde que se introdujo Munchkin Associate Lead, se han implementado formas más sólidas de realizar el envío de datos y la asociación de cookies.

#### Envío del lado del servidor

Si no necesita envío del lado del explorador, la API de REST ofrece muchos métodos para [envío de datos de persona](/help/rest-api/leads.md) y un método creado específicamente para asociar cookies con registros de persona[.](/help/rest-api/leads.md)

¿Cuándo se está desplegando la versión 159 de Munchkin?

La versión 159 ha estado en General Availability desde la versión de Marketo de octubre de 2020.

Publicado el _2020-05-06_ por _Kenny_

## Realizar un envío de formulario Marketo en segundo plano

Cuando su organización tiene muchas plataformas diferentes para alojar contenido web y datos de clientes, es bastante común necesitar envíos paralelos de un formulario para que los datos resultantes se puedan recopilar en plataformas independientes. Existen varias estrategias para hacerlo, pero la mejor suele ser la más sencilla: Usar la API de Forms 2 para enviar un formulario Marketo oculto. Esto funcionará con cualquier nuevo formulario de Marketo, pero lo ideal es crear un formulario vacío para este, que no tenga campos de. Esto garantizará que el formulario no cargue más datos de los necesarios, ya que no necesitamos procesar nada. Ahora simplemente toma el [código incrustado](https://experienceleague.adobe.com/es/docs/marketo/using/home) de tu formulario y agrégalo al cuerpo de tu página deseada, haciendo una pequeña modificación. El código incrustado incluye un elemento de formulario como este:

`<form id="mktoForm_1068"></form>`

Desea agregar &#39;style=&quot;display:none&quot;&#39; al elemento para que no esté visible, de esta manera:

`<form id="mktoForm_1068" style="display:none"></form>`

Una vez incrustado y oculto el formulario, el código para enviarlo es realmente sencillo:

```javascript
var myForm = MktoForms2.allForms()[0];
myForm.addHiddenFields({
 //These are the values which will be submitted to Marketo
 "Email":"<test@example.com>",
 "FirstName":"John",
 "LastName":"Doe"
 });
myForm.submit();
```

Forms enviado de esta manera se comportará exactamente igual que si el posible cliente hubiera rellenado y enviado un formulario visible. La activación del envío variará entre implementaciones, ya que cada una tiene una acción diferente para solicitarla, pero puede hacerlo ocurrir básicamente en cualquier acción. Lo importante es configurar correctamente los campos y los valores. Asegúrese de utilizar el nombre de la API de SOAP de los campos que puede encontrar con Exportar nombres de campo para garantizar el envío correcto de los valores.

### Migración desde Munchkin Associate Lead

El envío del formulario de fondo es uno de los métodos de reemplazo recomendados para el cliente potencial asociado de Munchkin. La página de HTML de ejemplo siguiente muestra la migración en un nivel superior, reutilizando los mismos valores que se envían a Associate Lead.

```html
<html>
<head>
    <!--
      Munchkin Code
      Replace with your own instance code
    -->
    <script type="text/javascript">
        (function() {
          var didInit = false;
          function initMunchkin() {
            if(didInit === false) {
              didInit = true;
              Munchkin.init('CHANGE ME');
            }
          }
          var s = document.createElement('script');
          s.type = 'text/javascript';
          s.async = true;
          s.src = '//munchkin.marketo.net/munchkin-beta.js';
          s.onreadystatechange = function() {
            if (this.readyState == 'complete' || this.readyState == 'loaded') {
              initMunchkin();
            }
          };
          s.onload = initMunchkin;
          document.getElementsByTagName['head'](0).appendChild(s);
        })();
        </script>
</head>

<body>
  <!--
    Start Embed code.
    Pasted from Form Actions -> Embed Code except for addition of 'style="display:none"' to the form tag in order to hide it, and instance-specific codes redacted
    Replace with your own code for testing
  -->
  <script src="CHANGE ME"></script>
  <form id="CHANGE ME" style="display:none"></form>
  <script>MktoForms2.loadForm("CHANGE ME", "CHANGE ME", "CHANGE ME TO AN INTEGER ID");</script>
  <!--End Embed Code-->

  <!--Demo code-->
    <script>
        //The same map which is assembled for the Munchkin submission can be reused for the form submission
        let values = {
            "Email": "email@example.com",
            "FirstName": "Test",
            "LastName": "Record"
        }
        //whenReady lets us apply a callback to all mkto forms on the page
        //the callback function fires whenever a form is completely loaded
        //for most use cases this will just be used to capture a reference to the form for later usage
        //submission is done in line here for demonstration only
        MktoForms2.whenReady(function(form){

            //the addHiddenFields methods lets us add arbitrary fields to the form as well as their values
            form.addHiddenFields(values);

            //pass the same set of values to associateLead
            //hashString: secret + email
            Munchkin.munchkinFunction('associateLead', values, "CHANGE ME");

            //submit the form
            form.submit();


        })
    </script>
</body>

</html>
```

Publicado el _2020-05-26_ por _Kenny_

## Actualizaciones de julio de 2020

En julio de 2020 lanzaremos nuevas API de REST, mejoraremos las API existentes y resolveremos defectos. Consulte la lista completa de actualizaciones a continuación.

* Se han agregado dos extremos que le permiten consultar y eliminar usuarios que aún no han aceptado su invitación (es decir, son usuarios &quot;pendientes&quot;). El punto de conexión [Obtener usuario invitado por el identificador](/help/rest-api/user-management.md) le permite consultar un usuario pendiente. El punto de conexión [Eliminar usuario invitado](/help/rest-api/user-management.md) le permite eliminar un usuario pendiente.
* El extremo [Invitar al usuario](/help/rest-api/user-management.md) se ha actualizado para aceptar cadenas de fecha y hora compatibles con ISO 8601 para el parámetro **expiresAt**.
* Los extremos de [Obtener usuario por identificador](/help/rest-api/user-management.md) y [Actualizar atributo de usuario](/help/rest-api/user-management.md) se han actualizado para devolver la última hora de inicio de sesión del usuario en el atributo **lastLoginAt**.
* Se ha corregido un problema en el cual el extremo [Crear lista estática](https://developer.adobe.com/marketo-apis/api/asset/#operation/createStaticListUsingPOST) devolvía el error &quot;611, error del sistema&quot; al intentar crear una lista estática que ya existía. Se ha cambiado para devolver el error &quot;709, ya existe una lista estática con el mismo nombre&quot;. [LM-135934]
* Se ha corregido un problema en el cual el extremo [Crear correo electrónico](https://developer.adobe.com/marketo-apis/api/asset/#operation/createEmailUsingPOST) devolvía el error &quot;611, Error del sistema&quot; al intentar crear un correo electrónico que ya existía. Se ha cambiado para devolver el error &quot;709, ya existe un correo electrónico con el mismo nombre&quot;. [LM-138648]
* Se ha corregido un problema por el cual los extremos de consulta de página de aterrizaje devolvían valores **createdAt** incorrectos. Los extremos devolvían la hora en que se aprobó por última vez una página de aterrizaje. Ahora regresan a la hora en que se creó una página de aterrizaje. [LM-138648]
* Se ha corregido un problema en el cual el extremo [Merge Leads](https://developer.adobe.com/marketo-apis/api/mapi/#operation/mergeLeadsUsingPOST) devolvía el error &quot;611, System error&quot; para una operación de combinación no válida. Se produjo cuando la combinación resultó en un posible cliente duplicado y **mergeCRM** se estableció en true. Se ha cambiado para que devuelva el error &quot;712, está creando un registro duplicado. Le recomendamos que utilice un registro existente en su lugar&quot;. [LM-137463]

Publicado el _2020-08-01_ por _David_

## Publicación de invitado: análisis profundo: objeto personalizado vs. actividades personalizadas vs. campo personalizado

**Esta es una publicación de invitado de Amit Jain, Campeón de Marketo 2020, Especialista en TI de MarTech** Como plataforma de automatización de marketing empresarial, Marketo administra la información de usuarios, clientes y clientes potenciales adquirida de varias fuentes y la mantiene en Marketo para mejorar la personalización, la segmentación y la creación de informes. Estas fuentes pueden variar desde los formularios del sitio web hasta la compilación de listas, los datos de CRM o los datos de comercio electrónico. Marketo ofrece objetos estándar, es decir, cliente potencial, empresa, oportunidad y actividad, etc. Estos objetos estándar a veces no son suficientes para satisfacer las necesidades de marketing emergentes de conjuntos de datos extendidos para que estén disponibles en Marketo.

Por ejemplo: artículos añadidos al carro de compras, estudiantes que solicitaron diferentes cursos, productos propiedad de una persona específica y consentimientos para diferentes suscripciones, etc. Esta información puede ser crítica para los especialistas en marketing a fin de mantener el interés del cliente o cliente potencial mediante la provisión de contenido más personalizado y una experiencia de usuario unificada en todas las plataformas. Para dar cabida a estas necesidades comerciales, Marketo proporciona formas personalizadas de almacenar este tipo de datos en campos personalizados, actividades personalizadas y objetos personalizados. He observado que la gente tiene problemas para entender cuándo usar cuál de las opciones. En esta publicación de blog, **entenderemos en detalle cuáles son estas tres cosas y cuándo usar una con algunos ejemplos.**

**Campos personalizados**

El objeto &quot;Posible cliente&quot; en Marketo es el objeto maestro y todo lo demás está conectado con este, directa o indirectamente. Marketo le permite crear campos personalizados en el objeto &quot;Posible cliente&quot;, objeto &quot;Compañía&quot; y recientemente anunciaron la compatibilidad de campos personalizados &quot;Miembro del programa&quot;. Estos campos personalizados satisfacen sus necesidades de almacenar cierto tipo de datos. Por ejemplo, es posible que necesite almacenar la función de trabajo o consentimientos diferentes del usuario. Existen dos tipos de campos personalizados en Marketo:

1. **Sincronizado desde campos de CRM:** Como parte de la sincronización automática de Marketo ↔︎ SFDC, Marketo sincroniza todos los campos personalizados visibles para el usuario de Marketo en SFDC en el objeto de posible cliente, contacto, cuenta y oportunidad.
1. **Campos solo de Marketo:** Puede crear directamente un campo en Marketo. Los datos de estos campos no se sincronizan con SFDC.

**Sugerencias rápidas**

* ¿Tiene un campo de solo Marketo y ahora desea sincronizar los datos en SFDC? Consulte [este blog](https://themarketingautomationblog.com/2019/12/06/field-management-merging-remapping-hiding-marketo-fields/) para saber cómo puede hacerlo.
* Obtenga más información acerca de los campos personalizados [aquí](https://themarketingautomationblog.com/2019/11/15/knowing-your-marketo-fields/).
* De momento, las API de Marketo no admiten la actualización/creación de campos personalizados.

**Objetos personalizados**

Aparte de los objetos estándar, Marketo le permite crear sus propios objetos personalizados. _Puede crear objetos personalizados y relacionarlos con los objetos Company, Lead u otro objeto personalizado._ Un objeto personalizado es un conjunto de registros personalizados que complementan los registros estándar de clientes potenciales y empresas. Los objetos personalizados le permiten almacenar datos adicionales de forma escalable y vincular esos datos a un registro de posible cliente o compañía. Puede generar un objeto personalizado con cualquier combinación de campos estándar (Campos de vínculo) y personalizados, rellenar esos campos para crear _registros_ de objeto personalizado y, a continuación, vincular esos registros a un registro de cliente potencial o de compañía. Los registros vinculados, potentes y flexibles, enriquecen los segmentos, las listas inteligentes y las campañas al permitirle crear esos recursos con información que no se encuentra en los campos de posibles clientes y de la compañía. Un objeto personalizado puede tener uno de los siguientes tipos de relaciones:

* **Relación uno a uno**: donde cada objeto personalizado tiene un único registro de objeto de cliente potencial/compañía.
* **Relación uno a varios**: cada objeto personalizado contiene varios registros de objetos personalizados relacionados con un posible cliente/compañía.
* **Relación varios a varios:** donde varios registros de objetos personalizados pueden vincularse con varios objetos de cliente potencial/compañía. Por ejemplo, hay varios alumnos inscritos en varios cursos de un catálogo de cursos.

**Sugerencias rápidas**

* Obtenga más información para configurar objetos personalizados [aquí](https://experienceleague.adobe.com/es/docs/marketo/using/home).
* Puede utilizar el objeto personalizado de Marketo como objeto intermedio, así como el objeto personalizado del objeto personalizado.

### ¿Por qué objetos personalizados?

Los objetos personalizados le permiten compilar y utilizar datos únicos que son relevantes para una empresa o posible cliente, pero no son necesariamente información estática sobre la empresa o el propio cliente. Aunque los campos de posible cliente están relacionados con la información de posible cliente de un individuo (_Dirección de correo electrónico_, _Código postal_, etc.), información empresarial (_Puesto_, _Sector_, etc.) o información dirigida por el sistema (_Marketo ID_, un SFDC ID o una Fecha de creación, etc.), los campos de objeto personalizados son totalmente personalizables. Por ejemplo, utilice objetos personalizados para almacenar información como la siguiente:

* Historial de compras de un usuario.
* Información del carro de compras.
* Si un cliente ha utilizado o no uno de sus códigos de descuento promocionales por tiempo limitado.
* Información complementaria obtenida de los resultados de la encuesta, entrevistas y asistencia al evento.
* Información de prueba de un usuario, incluida la URL de la instancia de prueba, la fecha de inicio, la fecha de finalización, el número de usuarios, etc.

### Limitaciones de objetos personalizados de Marketo

1. De forma predeterminada, Marketo permite crear 10 objetos personalizados. Puede aumentar el límite con una tarifa de suscripción adicional.
1. El número predeterminado de campos por objeto es 50, pero puede solicitar soporte de Marketo para campos adicionales si es necesario. Gracias [Michael Florin](https://www.linkedin.com/in/michaelflorin/) por recibir más información aquí.
1. Hay un límite en el número de registros que se pueden tener en todos los objetos personalizados. Depende de su suscripción a Marketo. Este límite se puede aumentar con una tarifa de suscripción adicional.
1. Si se creó un objeto personalizado con la API, Marketo no permite editar el esquema CO desde la interfaz de usuario de Marketo.

**Sugerencias rápidas**

* La API de Marketo admite la operación CRUD (crear, leer, actualizar y eliminar) en objetos personalizados.
* Puede utilizar estos datos de objeto personalizados para la personalización de correo electrónico mediante Secuencias de comandos de Velocity.
* Puede encontrar todos los extremos relacionados con objetos personalizados [aquí](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportProgramMembersStatusUsingGET).

### Terminología de objeto personalizado

**Objeto personalizado**: contenedor que contiene una agrupación de todos los registros de objetos personalizados. Conocido formalmente como conjunto de tarjetas de datos o tabla personalizada. **Registro de objeto personalizado**: Registro de datos que contiene información de campo adicional que puede estar vinculada a un posible cliente o a una compañía. Un registro puede estar compuesto por campos de cliente potencial o empresa estándar y campos de registro de objeto personalizados. Conocido formalmente como fila de tabla de datos o tarjeta de datos. **Campo de registro de objeto personalizado**: campos totalmente personalizables para recopilar información única o temporal. Estos campos se crean y alojan dentro del propio objeto personalizado. Formalmente se denomina campo de tarjeta de datos o campo/columna de tabla de base de datos. **Campo de vínculo**: tipo especial de campo de registro de objeto personalizado para definir la relación entre el registro de objeto personalizado y el registro de objeto de cliente potencial/compañía vinculado. Al crear objetos personalizados, debe proporcionar campos de vínculo para conectar el registro de objeto personalizado al registro principal correcto.

* Para una estructura personalizada de uno a varios o de uno a uno, utilice el campo de vínculo del objeto personalizado para conectarlo a una persona o compañía.
* Para una estructura de varios a varios, se utilizan dos campos de vínculo, conectados desde un objeto intermedio creado por separado (que también es un tipo de objeto personalizado). Un vínculo se conecta a personas o empresas de la base de datos y el otro al objeto personalizado. En este caso, el campo de vínculo no se encuentra en el propio objeto personalizado.

### Actividades personalizadas

**Hay varias formas en que alguien puede interactuar con nuestra organización. Pueden visitar el sitio web de su empresa, asistir a una de sus ferias comerciales o quizás hacer clic en un vínculo de un correo electrónico enviado por usted. Estas acciones son actividades** y, sea cual sea la acción que realicen, Marketo las capturará para que sus equipos de marketing y ventas puedan comprender mejor el comportamiento del usuario y lograr una participación personalizada y unificada. **_Actividades personalizadas_** _pueden ayudarle a realizar el seguimiento de una actividad que no esté relacionada con un formulario, un correo electrónico o una página de aterrizaje de Marketo_. Por ejemplo, si desea rastrear cuándo alguien vio un vídeo en un sitio web o realizó una encuesta, utilice una actividad personalizada. Las actividades personalizadas difieren de los objetos personalizados. Utilice objetos personalizados cuando el valor pueda cambiar (por ejemplo, el &quot;color del coche&quot; cambia de azul a rojo). Utilice actividades personalizadas cuando rastree momentos que se produjeron y sus detalles no puedan cambiar (por ejemplo, &quot;coche comprado&quot;). De forma predeterminada, el límite del número máximo de actividades personalizadas que se pueden definir es de 10. Esto se puede aumentar con una tarifa de suscripción adicional. Según la [política de retención de datos de Marketo](https://nation.marketo.com/t5/knowledgebase/tkb-p/support_solutions-documents), las actividades personalizadas se eliminarán automáticamente pasados 25 meses.

**Actividad personalizada:** eventos que no son de Marketo y que desearía rastrear dentro de Marketo. **ID de actividad personalizado:** Marketo asigna un ID numérico a la actividad personalizada que se puede usar al intentar insertar o extraer los datos de la actividad mediante la API de Marketo. **Campos de actividad personalizados:** Los metadatos de actividad se pueden almacenar en un campo de actividad. Por ejemplo, si está realizando un seguimiento de las vistas en vídeo, los campos podrían ser la URL de la página, el Título del vídeo, etc. **Campo principal de actividad personalizada:** Campos de actividad personalizados que se pueden usar como criterios de filtro de lista inteligente.

**Sugerencias rápidas**

* Los extremos de API para las actividades personalizadas están disponibles [aquí.](https://developer.adobe.com/marketo-apis/api/mapi/#operation/addCustomActivityUsingPOST)

## Objeto personalizado frente a actividad personalizada

**Objeto personalizado**

**Actividad personalizada**

1

Máximo de 10 objetos personalizados de forma predeterminada por instancia.

Máximo de 10 actividades personalizadas de forma predeterminada.

2

Máximo de 50 campos de objeto personalizado por objeto personalizado.

Cada tipo de actividad personalizada puede tener hasta 20 atributos secundarios.

3

Máximo de registros de objetos personalizados de 1 MM; puede variar según la suscripción.

Después de 25 meses, las actividades personalizadas se eliminarán según la política de retención de datos de Marketo.

4

Se puede utilizar como filtro y Déclencheur en listas inteligentes y campañas inteligentes.

Se puede utilizar como filtro y Déclencheur en listas inteligentes y campañas inteligentes.

5

Se puede utilizar para personalizar el contenido del correo electrónico.

No se puede usar para personalizar el contenido del correo electrónico.

6

Puede realizar la operación CRUD en un registro de objeto personalizado.

Solo se permite Crear y Leer en actividades personalizadas.

7

Utilice objetos personalizados cuando el valor pueda cambiar (por ejemplo, el &quot;color del coche&quot; cambia de azul a rojo).

Utilice actividades personalizadas cuando rastree momentos que se produjeron y sus detalles no puedan cambiar (por ejemplo, &quot;coche comprado&quot;).

8

Los objetos personalizados indican el hecho. es decir, valor actual.

Las actividades personalizadas indican los eventos que se han producido en el pasado.

Publicado el _2020-10-18_ por _Amit_

## Actualizaciones de enero de 2021

En enero de 2021 lanzamos nuevas API de REST y resolvemos varios defectos. Consulte la lista completa de actualizaciones a continuación.

* Se agregó el extremo [Submit Form](/help/rest-api/leads.md) que le permite realizar envíos de formularios mediante programación. Los formularios de terceros ahora se pueden integrar con los formularios de Marketo para aprovechar los flujos de trabajo de marketing existentes.
* Se ha agregado el punto de conexión [Obtener contenido completo de la página de aterrizaje](/help/rest-api/landing-pages.md) que devuelve la versión serializada de HTML de una página de aterrizaje. Permite procesar vistas previas totalmente personalizadas de páginas de aterrizaje sin tener que iniciar sesión en Marketo Engage. Esto puede ayudar a optimizar los flujos de trabajo de edición y traducción dentro de las aplicaciones integradas.
* Ahora puede configurar el número de objetos personalizados disponibles para acceder a través del script de Velocity. Las instrucciones de configuración se encuentran [aquí](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/administration/email-setup/change-custom-object-retrieval-limits-in-velocity-scripting).

### Resoluciones de defectos

* Se ha corregido un problema en el cual el punto de conexión [Eliminar usuario](/help/rest-api/user-management.md) permitía eliminar un usuario solo de API que estaba en uso por un servicio personalizado. Ahora devuelve el error &quot;611, No puede eliminar un usuario de API que se esté utilizando en el servicio de API&quot;. [LM-141893]
* Se ha corregido un problema en el cual el extremo [Obtener usuarios](/help/rest-api/user-management.md) devolvía usuarios eliminados en algunos casos. [LM-141542]
* Se corrigió un problema en el extremo [Clone Program](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST). Si especifica un nombre de programa que supere los 255 caracteres, devolverá el mensaje &quot;611, Unable to clone program error&quot;. Ahora devuelve &quot;701, el nombre no puede superar los 255 caracteres&quot;. [LM-143436]
* Se ha corregido un problema con el punto de conexión [Aprobar borrador de página de aterrizaje](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveLandingPageUsingPOST). Cuando aprobaba una página de aterrizaje con la versión móvil activada, en algunos casos veía contenido de la versión móvil en la versión de escritorio. [LM-146867]
* Se ha corregido un problema con el extremo [Desaprobar página de aterrizaje](https://developer.adobe.com/marketo-apis/api/asset/#operation/unapproveLandingPageByIdUsingPOST) que le permitía desaprobar una página de aterrizaje que uno o más formularios estaba utilizando como página de seguimiento. Ahora devuelve el error &quot;709, Unapprove landing page failed. Uno o más formularios están usando la página de aterrizaje como página de seguimiento con identificadores de formulario: [_formId1,formId2,..._]&quot;. [LM-143326]

Publicado el _2021-01-15_ por _David_

## API de Munchkin 160 Beta y Beacon

**27 de enero de 2021:** Algunos usuarios de Marketo que se verán afectados por la obsolescencia de Associate Lead recibieron una notificación por correo electrónico en la que se indica que tienen la configuración de Munchkin Beta habilitada en una o varias de sus instancias. Esta versión se mantendrá hasta que se pueda notificar a la audiencia correcta. A partir de la versión 160 de Munchkin JavaScript, la [API de señalización](https://developer.mozilla.org/en-US/docs/Web/API/Beacon_API) se convierte en la forma predeterminada en que Munchkin se comunica con el servidor de Marketo. Esto estuvo disponible como opción en verano de 2020 con el lanzamiento de la versión 159 a través del parámetro de configuración **useBeaconAPI**. La API de Beacon tiene varias ventajas con respecto al antiguo método XMLHttpRequest, pero la principal mejora es que es una API asincrónica sin bloqueo para la comunicación HTTP que está disponible para su uso en todos los exploradores de Internet modernos. Aunque la mayoría de los usuarios de Munchkin no notarán ningún cambio en el comportamiento del sitio web, esta actualización evitará que Munchkin bloquee la navegación mientras espera a enviar un evento de clic al back-end o, más simplemente, todo esto elimina la posibilidad de que Munchkin haga que un explorador se &quot;cuelgue&quot; después de hacer clic en un vínculo a una nueva página. Esto ha sido un suceso raro pero frustrante para algunos clientes de Marketo.

A partir del 27 de enero de 2021, el despliegue de esta versión está en espera a la espera de una nueva programación. Aunque no se prevén problemas relacionados con este cambio y no se ha identificado ninguno durante las pruebas, es imposible para Marketo probar todas las configuraciones de implementación posibles de Munchkin y es posible que desee probar estos cambios con antelación o renunciar a este cambio hasta que esté disponible de forma general esta versión. A continuación se presentan las instrucciones para varios escenarios.

### API de señalización de prueba

Si desea probar la API de señalización actualizada anticipándose a la próxima versión, puede hacerlo agregando el parámetro **useBeaconAPI** a la configuración de Munchkin en una página de prueba externa. Esta prueba funcionará con la versión beta o disponible de forma general de Munchkin. El parámetro de configuración se muestra a continuación en el segundo argumento de la invocación del método `Munchkin.init()` en la línea 7: `{ 'useBeaconAPI': true}`

```javascript
<script type="text/javascript">
(function() {
  var didInit = false;
  function initMunchkin() {
    if(didInit === false) {
      didInit = true;
      Munchkin.init('299-BYM-827', {"useBeaconAPI":true});
    }
  }
  var s = document.createElement('script');
  s.type = 'text/javascript';
  s.async = true;
  s.src = '//munchkin.marketo.net/munchkin.js';
  s.onreadystatechange = function() {
    if (this.readyState == 'complete' || this.readyState == 'loaded') {
      initMunchkin();
    }
  };
  s.onload = initMunchkin;
  document.getElementsByTagName['head'](0).appendChild(s);
})();
</script>
```

### Deshabilitar Munchkin Beta en las páginas de aterrizaje de Marketo

Para deshabilitar Munchkin Beta en las páginas de aterrizaje de Marketo, debes acceder al menú [Cofre del tesoro](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/administration/settings/enable-or-disable-treasure-chest-features) en la sección Admin de tu suscripción y cambiar la configuración de Munchkin Beta en las páginas de aterrizaje a deshabilitada.

### Deshabilitar Munchkin Beta en páginas externas

Si ha implementado la versión de Beta de Munchkin JavaScript en páginas web externas y desea renunciar a este cambio hasta que esté disponible de forma general, debe modificar el fragmento de JS de Munchkin para que se dirija a **munchkin.**&#x200B;**js** archivo en lugar del **munchkin-beta.**&#x200B;**js** archivo. En el ejemplo siguiente, este es el valor de la variable **s.src** de la línea 11. Es posible que el fragmento no se parezca mucho al ejemplo o que lo implemente un administrador de etiquetas en las páginas externas y que necesite ponerse en contacto con los recursos de TI o con quien administre sus sitios web con el seguimiento de Munchkin habilitado.

```javascript
<script type="text/javascript">
(function() {
  var didInit = false;
  function initMunchkin() {
    if(didInit === false) {
      didInit = true;
      Munchkin.init('299-BYM-827');
    }
  }
  var s = document.createElement('script');
  s.type = 'text/javascript';
  s.async = true;
  s.src = '//munchkin.marketo.net/munchkin.js';//This line should have the munchkin.js file, not munchkin-beta.js
  s.onreadystatechange = function() {
    if (this.readyState == 'complete' || this.readyState == 'loaded') {
      initMunchkin();
    }
  };
  s.onload = initMunchkin;
  document.getElementsByTagName['head'](0).appendChild(s);
})();
</script>
```

Publicado el _2021-01-08_ por _Kenny_

## Desaprobación final de API del correo electrónico V1

[La obsolescencia del correo electrónico V1 comenzó hace casi dos años](https://nation.marketo.com:443/t5/knowledgebase/email-editor-1-0-is-being-deprecated-june-18th/ta-p/250666) y a partir de la versión de mantenimiento de marzo de las suscripciones de Londres y Países Bajos el 17 de marzo de 2021 y de todas las demás suscripciones el 19 de marzo de 2021, finalizará toda la compatibilidad de la API con los correos electrónicos V1. Después de esta versión, cualquier intento de interactuar con correos electrónicos de la versión 1 a través de las API de recursos dará como resultado errores y no se realizarán acciones. Se ha notificado a todos los usuarios conocidos restantes desde el 24 de febrero de 2021, pero es posible que aún haya integraciones que puedan intentar interactuar con estos recursos. Los tipos más comunes de integraciones afectadas son servicios que ofrecen administración, traducción y localización de recursos digitales. Si observa errores de integración como resultado de este cambio, [aún podrá actualizar los recursos problemáticos editándolos y aprobándolos](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/email-marketing/general/email-editor-2/transitioning-to-email-editor-2-0). Una vez que un recurso de correo electrónico se haya actualizado a la versión 2, podrá volver a utilizarlo con los servicios integrados.

Publicado el _2021-03-17_ por _Kenny_

## Actualizaciones de mayo de 2021

En mayo de 2021 lanzaremos nuevas API de REST, mejoraremos las API de REST existentes y resolveremos varios defectos. Consulte la lista completa de actualizaciones a continuación.

* Se han agregado API de miembros de programa que permiten recuperar, actualizar y eliminar registros de pertenencia a programas. Para obtener más información, consulte [API de REST > Base de datos de posibles clientes > Miembros del programa](/help/rest-api/program-members.md).
* Se han agregado API de extracción de objetos personalizados en lote que le permiten exportar registros de objetos personalizados de Marketo de primer nivel asociados a posibles clientes en una relación &quot;uno a varios&quot;. Para obtener más información, consulte [API de REST > Extracción en lotes > Extracción en lotes de objetos personalizados](/help/rest-api/bulk-custom-object-extract.md).
* Hemos mejorado tanto la [API de posible cliente](/help/rest-api/leads.md) como la [API de extracción masiva de posibles clientes](/help/rest-api/bulk-lead-extract.md) para permitir que los usuarios recuperen el ID de Adobe Experience Cloud (ECID). Esto permite a los usuarios que [sincronizan audiencias de Adobe Experience Cloud](https://experienceleague.adobe.com/docs/marketo/using/product-docs/core-marketo-concepts/miscellaneous/set-up-adobe-experience-cloud-audience-sharing.html?lang=es) identificar posibles clientes que tengan ECID asociados. Esto abre [posibilidades de integración](https://adobeexchangeec.zendesk.com/hc/en-us/articles/360024277392-Adobe-Experience-Cloud-Using-the-ECID-for-integration) con otros productos de Adobe Experience Cloud.
* Hemos mejorado la [API de importación masiva de posibles clientes](/help/rest-api/bulk-lead-import.md) para admitir la adición de posibles clientes a los registros de la compañía durante el proceso de importación. Para ello, incluya el campo **externalCompanyId** en el archivo de importación.
* Hemos mejorado varios extremos del programa para proporcionar paridad con la funcionalidad que se encuentra en la interfaz de usuario de Marketo Engage. Hemos mejorado los extremos de [Crear programas](/help/rest-api/assets.md) y [Clonar programas](https://developer.adobe.com/marketo-apis/api/asset/) para permitir operaciones de creación, clonación o movimiento en programas de eventos. Para los usuarios que organizan programas de eventos &quot;anidándolos&quot; debajo de otros tipos de programas. También hemos mejorado el punto de conexión [Eliminar programa](https://developer.adobe.com/marketo-apis/api/asset/) para permitir la eliminación de programas que contengan los siguientes recursos: notificaciones push, mensajes en la aplicación, informes y páginas de aterrizaje con Assets social incrustado.
* Como administrador de Marketo, puede [marcar un campo específico como &quot;confidencial&quot;](https://experienceleague.adobe.com/es/docs/marketo/using/home) para que sus valores [nunca se prerrellenen en los formularios](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/demand-generation/forms/form-fields/disable-pre-fill-for-a-form-field), y así proteger los datos confidenciales de los usuarios. Hemos mejorado varios extremos de campo de formulario para proporcionar paridad con esta funcionalidad que se encuentra en la interfaz de usuario de Marketo Engage.

### Resoluciones de defectos

* Se ha corregido un problema con Eliminar punto final del programa. Si intentara eliminar un programa en una carpeta compartida, devolverá &quot;611, Error del sistema&quot;. Ahora devuelve &quot; &quot;El programa de Target está en una carpeta compartida y no se puede eliminar. Se debe dejar de compartir la carpeta antes de intentar eliminarla&quot;.
* Se ha corregido un problema con el punto final Clonar programa. Si intentara clonar un programa que contenía un DateTime en un paso de flujo, devolverá &quot;611, System Error&quot;. Ahora clona correctamente el programa.
* Se ha corregido un problema con el extremo de Crear programas que permitía crear de forma involuntaria un programa debajo de un programa de correo electrónico (lo que no está permitido).
* Se ha corregido un problema con el punto final Clonar programa. Si clonó un programa que contenía una página de aterrizaje, el nombre de la página de aterrizaje en el programa de destino no tenía un guion bajo entre el nombre del programa y el nombre de la página de aterrizaje. P. ej.: `http://<_pod_\>.marketo.com/lp/<_munchkin_\>/<_program name_\>**_**<_LP name_\>.html`

Publicado el _2021-05-07_ por _David_

## Oferta por tiempo limitado para unirse a Adobe Exchange

El soporte de la comunidad de socios de Marketo Engage es uno de los pilares del éxito de nuestros clientes. Queremos garantizar que el ecosistema de integración de Marketo Engage esté bien representado en Exchange Marketplace y que tenga una oferta especial para socios de LaunchPoint. Durante un tiempo muy limitado que no se ampliará, ofrecemos a nuestros socios de LaunchPoint una asociación de innovación gratuita en el programa Exchange hasta finales de 2022 (valor aproximado de 15 000 $). Hemos diseñado esta oferta para animar a los socios de LaunchPoint a crear sus listas de integración en el portal de socios de Exchange, que luego se podrá buscar públicamente en Adobe Exchange Marketplace. Para ver una lista completa de los beneficios de la asociación Innovate que recibe sin cargo hasta diciembre de 2022.

1. Vaya a [Centro de asistencia para socios de Adobe Exchange](https://adobeexchangeec.zendesk.com/hc/en-us?mkt_tok=NjA4LURIVi05MTUAAAF-P5lIeVWOuBmKMS_uE_NpgFKtC0ukt7z_ksnq_Sbzb6mzXUuXpqpqQeujtPdZ24WcjMdptygQSR9XrYt_Cw)
1. Haga clic en &quot;Enviar una solicitud&quot; en la esquina superior derecha
1. En **Elija su problema a continuación**, elija &quot;Soporte de Adobe Exchange&quot;
1. En **Tu dirección de correo electrónico** escribe tu dirección de correo electrónico
1. En el cuadro **Asunto**, escriba &quot;Oferta de LaunchPoint&quot;
1. En el cuadro **Descripción**, escriba &quot;Oferta de LaunchPoint&quot;
1. En el menú desplegable **Tipo de soporte**, seleccione &quot;Soporte del programa&quot;.
1. En el menú desplegable **Producto Adobe Exchange**, seleccione &quot;Programa Adobe Exchange&quot;
1. Envíe el formulario. Nuestro equipo estará en contacto con usted en breve.

Publicado el _2021-07-22_ por _David_

## Actualizaciones de agosto de 2021

En agosto de 2021, vamos a mejorar las API de REST existentes y a resolver varios defectos. Consulte la lista completa de actualizaciones a continuación.

* Hemos mejorado la API de extracción masiva de actividades para permitir a los usuarios filtrar usando atributos principales para 6 tipos diferentes de actividades. Para obtener más información, consulte [Extracto de actividades en lotes](/help/rest-api/bulk-activity-extract.md).
* Para que los usuarios de Marketo Sales Connect tengan más acceso a los datos de su actividad de ventas, hemos activado atributos de actividad de ventas adicionales. Hemos añadido los atributos siguientes para las actividades Enviar correo electrónico de ventas, Abrir correo electrónico de ventas y Hacer clic en Correo electrónico de ventas:

* ID de vendedor de Marketo - ID único de registro de persona en Sales Connect
* Nombre de campaña de ventas: nombre de la campaña de ventas, si el correo electrónico formaba parte de una campaña de ventas
* URL de campaña de ventas: URL de conexión de ventas para la campaña de ventas si el correo electrónico formaba parte de una campaña de ventas
* Nombre de plantilla de ventas: nombre de la plantilla de correo electrónico en Sales Connect, si se ha utilizado una plantilla
* URL de plantilla de ventas: URL de conexión de ventas para la plantilla de correo electrónico, si se ha utilizado una plantilla

### Correos electrónicos

* Hemos mejorado el extremo de obtención de correos electrónicos al agregar el filtro `earliestUpdatedAt`/`latestUpdatedAt`. Esto le permite utilizar el campo `updatedAt` para buscar únicamente un subconjunto de correos electrónicos, lo que permite una sincronización incremental.
* Hemos mejorado los puntos finales de Obtener correos electrónicos, Obtener correo electrónico por nombre y Obtener correo electrónico por ID para admitir la recuperación de los registros de correo electrónico de tipo [Champion y Challenger](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/email-tests-champion-challenger/add-an-email-champion-challenger).

### Resoluciones de defectos

* Se ha corregido un problema con el extremo Obtener usuarios. Los usuarios a los que se había emitido una licencia de [Calendario de mercadotecnia](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/core-marketo-concepts/marketing-calendar/understanding-the-calendar/issue-revoke-a-marketing-calendar-license) no se devolvían. Los usuarios del calendario de marketing ahora se devuelven correctamente.
* Se ha corregido un problema con el punto final Enviar formulario. En caso de registros de posibles clientes duplicados, el formulario de envío se utiliza para emitir un error &quot;1007, Multiple lead match lookup criteria&quot;. El formulario de envío ahora actualiza el registro actualizado más recientemente del mismo modo que lo hace la [API de Forms 2.0](/help/javascript-api/forms-api-reference.md).
* Se han mejorado varios mensajes de error engañosos devueltos por los extremos Actualizar campo de posible cliente y Crear campos de posible cliente. [LM-151890, LM-151888, LM-151889]
* Se ha corregido un problema con los extremos de Obtener campo de posible cliente por nombre y Obtener campos de posible cliente. Ambos extremos podrían devolver información ligeramente obsoleta. Ahora siempre devuelven información actual.
* Se ha corregido un problema con [API de extracción masiva](/help/rest-api/bulk-extract.md) al usar el encabezado HTTP de &quot;intervalo&quot; para la recuperación parcial donde no se devolvió el último byte del intervalo.
* Se ha corregido un problema con Actualizar extremo de metadatos de fragmento. Al actualizar el nombre o la descripción del fragmento, el estado del fragmento se cambió a &quot;aprobado con borrador&quot;. Ahora el estado recortado permanece sin cambios después de actualizar el nombre o la descripción del fragmento.
* Se ha corregido un problema con Añadir extremo del módulo de correo electrónico. Al agregar un módulo que contenía un fragmento, devolvía un &quot;611, Error del sistema&quot;. Ahora agrega correctamente el módulo al correo electrónico.
* Se ha corregido un problema con Eliminar punto final del programa. Al eliminar un programa que contenía un recurso local de mensaje en la aplicación, devolvía un &quot;611, error del sistema&quot;. Ahora elimina correctamente el programa.

Publicado el _2021-08-22_ por _David_

## Despliegue de Munchkin versión 161

El 7 de septiembre de 2021, la versión 161 de Munchkin empezará a implementarse en el 10 % de las suscripciones con Munchkin Beta habilitado, seguido del 50 % el 16 de septiembre y del 100 % el 30 de septiembre. Este cambio afectará a las páginas de aterrizaje de Marketo y a la versión del archivo munchkin-beta.js que se proporciona a páginas de aterrizaje externas que se cargan desde suscripciones a las que se ha implementado la nueva versión. Esta versión desaprueba por completo el método Associate Lead de Munchkin, que es una función que permitía el envío de datos de persona a una suscripción de Marketo y el historial de navegación web asociado con un registro de persona conocida. El posible cliente asociado se está eliminando en favor de alternativas más modernas y seguras, como la [API de Forms JS](/help/javascript-api/forms-api-reference.md), la API de envío de formularios y la [API de REST de posible cliente asociada](/help/rest-api/leads.md). Si usted o su organización utilizan este método, debe dejar de utilizarlo antes del 12 de octubre de 2021, cuando está programado que comience el despliegue de la versión de octubre. Si ya no desea adherirse a la versión beta de Munchkin, puede deshabilitar el uso en las páginas de aterrizaje de Marketo si cambia la función &quot;Munchkin Beta en las páginas de aterrizaje&quot; a `disabled` en el [menú del cofre del tesoro](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/administration/settings/enable-or-disable-treasure-chest-features). Si ha implementado Munchkin Beta JavaScript en páginas web externas y desea cambiar al canal de publicación predeterminado de Munchkin, debe actualizar el fragmento de código para cargar Munchkin JavaScript desde munchkin.js en lugar de munchkin-beta.js.

Publicado el _2021-08-24_ por _Kenny_

## Finalización del servicio de TLS 1.0 y TLS 1.1 para Munchkin

A partir del 21 de octubre de 2021, munchkin.marketo.net, que se usa para ofrecer Munchkin JavaScript a los visitantes, dejará de aceptar conexiones con [TLS 1.0 o TLS 1.1.](https://en.wikipedia.org/wiki/Transport_Layer_Security) Estos estándares de cifrado ya no se aceptan como parte de las prácticas recomendadas de seguridad web y ya no son compatibles con las versiones modernas de los exploradores. No se prevén efectos negativos como resultado de este cambio.

Publicado el _2021-10-04_ por _Kenny_

## Actualizaciones de octubre de 2021

En octubre de 2021, vamos a mejorar las API de REST existentes y a resolver varios defectos. Consulte la lista completa de actualizaciones a continuación.

* Hemos mejorado el extremo [Enviar formulario](https://developer.adobe.com/marketo-apis/api/mapi/#operation/SubmitFormUsingPOST) para admitir campos personalizados de miembros del programa como parte del envío del formulario. Opcionalmente, se puede especificar un programa como el programa al que agregar el formulario o el programa al que agregar campos personalizados de miembros del programa como se describe [aquí](/help/rest-api/leads.md).
Hemos mejorado el punto de conexión [Obtener miembros del programa](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getProgramMembersUsingGET) para admitir consultas basadas en intervalos de fechas según el atributo updatedAt. Esto se hace pasando los parámetros de fecha y hora de inicio y finalización como se describe [aquí](/help/rest-api/program-members.md).
* Hemos mejorado las API de [Campos de posibles clientes](/help/rest-api/leads.md) para admitir [Campos confidenciales](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/administration/field-management/mark-a-field-as-sensitive). Los extremos [Obtener campo de posible cliente por nombre](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadFieldByNameUsingGET), [Obtener campos de posible cliente](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadFieldsUsingGET), [Crear campos de posible cliente](https://developer.adobe.com/marketo-apis/api/mapi/#operation/createLeadFieldUsingPOST) y [Actualizar campo de posible cliente](https://developer.adobe.com/marketo-apis/api/mapi/#operation/updateLeadFieldUsingPOST) ahora admiten el atributo isSensitive.

### Resoluciones de defectos

* Se ha corregido un problema con la API [Administración de usuarios](/help/rest-api/user-management.md). Se refiere a los usuarios de Marketo configurados para usar con [Sales Insight](https://business.adobe.com/es/products/marketo/sales-insight.html). Estos usuarios ahora son devueltos por el extremo [Obtener usuarios](https://developer.adobe.com/marketo-apis/api/user/#operation/getUsersUsingGET), y estos usuarios ahora pueden ser eliminados usando el extremo [Eliminar usuario](https://developer.adobe.com/marketo-apis/api/user/#operation/deleteUserUsingPOST). [LM-155864]
* Se ha corregido un problema con el extremo Agregar [campo de texto enriquecido](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/addRichTextFieldUsingPOST). Al agregar un campo de texto enriquecido que supera los 65 000 caracteres en un correo electrónico, una página de aterrizaje, un fragmento de código o un formulario, se devuelve el error &quot;611, Error del sistema&quot;. Ahora devuelve el error &quot;701, la operación no se puede completar. &#39;contenido&#39; supera una longitud máxima de 65.535 bytes&quot;.

Publicado el _2021-10-25_ por _David_

## Actualizaciones de enero de 2022

En enero de 2022, vamos a mejorar las API de REST existentes y a resolver varios defectos. Consulte la lista completa de actualizaciones a continuación.

* Hemos mejorado la API [Extracción de objetos personalizados en lotes](/help/rest-api/bulk-custom-object-extract.md) para permitir que los usuarios filtren con un intervalo de fechas **updatedAt**.
* Se han agregado API de metadatos de campo de miembro de programa que permiten crear, actualizar y recuperar metadatos para campos de miembro de programa. Para obtener más información, vea [Miembros del programa > Campos](/help/rest-api/program-members.md).
* Se han añadido API de metadatos de campos de compañía que permiten recuperar metadatos de campos de compañía. Para obtener más información, vea [Compañías > Campos](/help/rest-api/companies.md).
* Se han agregado API de metadatos de campo de oportunidad que le permiten recuperar metadatos de campos de oportunidad. Para obtener más información, vea [Oportunidades > Campos](/help/rest-api/opportunities.md).
* Se han añadido API de metadatos de campos de cuentas con nombre que permiten recuperar metadatos de campos de cuentas con nombre. Para obtener más información, vea [Cuentas con nombre > Campos](/help/rest-api/named-accounts.md).
* Se han actualizado los extremos de los metadatos de campo para devolver una nueva propiedad booleana **isApiCreated** que indica si la API de REST creó o no un campo.

### Resoluciones de defectos

* Se ha corregido un problema de latencia entre el momento de la llamada al extremo [Crear campos de posible cliente](https://developer.adobe.com/marketo-apis/api/mapi/#operation/createLeadFieldUsingPOST) y el momento en el que el campo de posible cliente recién creado estaba disponible en la lista inteligente. [LM-152838]
* Se ha corregido un problema con el extremo [Crear campos de posible cliente](https://developer.adobe.com/marketo-apis/api/mapi/#operation/createLeadFieldUsingPOST) en el cual los campos creados no estaban disponibles en la lista desplegable de campos de formulario que se usa para [agregar campos al formulario](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/demand-generation/forms/creating-a-form/add-a-field-to-a-form) en la interfaz de usuario de Marketo Engage. [LM-158243]
* Se ha corregido un problema con el extremo [Get Campaigns](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getCampaignsUsingGET) en el cual no se devolvían campañas activables cuando se especificaba el parámetro isTriggerable=true. [LM-158283]
* Se ha corregido un problema en el cual el extremo [Obtener posibles clientes por id. de lista](https://developer.adobe.com/marketo-apis/api/mapi/#operation/deleteTokenByNameUsingPOST) devolvía un error &quot;611, Error del sistema&quot; en algunos casos. [LM-157214]
* Se han limpiado varios mensajes de error devueltos por el extremo [Actualizar campo de posible cliente](/help/rest-api/leads.md). [LM-151886, LM-151888, LM-151889]

Publicado el _2022-01-27_ por _David_

## Actualizaciones de marzo de 2022

En marzo de 2022, vamos a mejorar las API de REST existentes y a resolver varios defectos. Consulte la lista completa de actualizaciones a continuación.

* Hemos agregado el campo **actionResult** al archivo de exportación producido por la API de extracción masiva de actividades. Este campo se puede utilizar para distinguir entre actividades correctas, omitidas y fallidas.
* Hemos agregado el campo **isOpenTrackingDisabled** a las respuestas de la [API de correos electrónicos](/help/rest-api/emails.md). Este campo se puede usar para determinar si la característica [Deshabilitar seguimiento de aperturas](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/email-marketing/general/email-editor-2/email-editor-v2-0-overview) está habilitada.
* Hemos agregado dos extremos que le permiten administrar selectivamente las etiquetas de programa. El punto de conexión [Actualizar etiquetas de programa](/help/rest-api/programs.md) le permite actualizar selectivamente una etiqueta de programa. El punto de conexión [Eliminar etiquetas de programa](/help/rest-api/programs.md) le permite eliminar selectivamente una etiqueta de programa.
* Hemos agregado el parámetro **isExecutable** al extremo [Clone Smart Campaign](/help/rest-api/smart-campaigns.md). Este parámetro permite clonar un programa como un programa ejecutable.
* Hemos agregado el campo **headStart** a la [API de programas](/help/rest-api/programs.md). Esto le permite crear, actualizar y recuperar la configuración de [Head Start](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/email-marketing/email-programs/email-program-actions/head-start-for-email-programs) para los programas de correo electrónico.

### Resoluciones de defectos

* Se ha corregido un problema con el extremo [Obtener contenido dinámico de correo electrónico](https://developer.adobe.com/marketo-apis/api/asset/#operation/getEmailDynamicContentUsingGET). Al intentar recuperar líneas de asunto con contenido dinámico de correos electrónicos que tenían relaciones de plantilla rotas, se devolvió un error, 709, &quot;API solo permite operaciones en correos electrónicos con una plantilla&quot;. El punto final ahora devuelve el contenido dinámico. [LM-152331]
* Se ha corregido un problema con el extremo [Sync Leads](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncLeadUsingPOST). Cuando se utiliza externalSalesPersonId para asociar el vendedor con un posible cliente mediante externalSalesPersonId y se utiliza action = createDuplicate, la asociación del vendedor no se produciría. [LM-158990]

### Integración de IMS de Adobe

* Aquellos que han sido incorporados a [Adobe IMS](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview) no pueden usar todas las [API de administración de usuarios de Marketo](/help/rest-api/user-management.md). Los siguientes extremos devolverán un error en cuando se invoquen en instancias de Marketo que se hayan integrado con Adobe IMS: [Invitar usuario](https://developer.adobe.com/marketo-apis/api/user/#operation/inviteUserUsingPOST), [Obtener usuario invitado por el identificador](https://developer.adobe.com/marketo-apis/api/user/#operation/getInvitedUserUsingGET), [Actualizar atributos del usuario](https://developer.adobe.com/marketo-apis/api/user/#operation/updateUserAttributeUsingPOST), [Eliminar usuario](https://developer.adobe.com/marketo-apis/api/user/#operation/deleteUserUsingPOST) y [Eliminar usuario invitado](https://developer.adobe.com/marketo-apis/api/user/#operation/deleteInvitedUserUsingPOST). Como reemplazo, se deben usar las [API de administración de usuarios de Adobe](https://developer.adobe.com/umapi/).

Publicado el _2022-03-14_ por _David_

## Actualizaciones de mayo de 2022-

En mayo de 2022, vamos a mejorar las API de REST existentes y a resolver varios defectos. Consulte la lista completa de actualizaciones a continuación.

* Hemos agregado la capacidad de recuperar los registros de [Compañía](/help/rest-api/companies.md), [Oportunidad](/help/rest-api/opportunities.md) y [Vendedores](/help/rest-api/sales-persons.md) cuando [SFDC Sync](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync) o [Microsoft Dynamics Sync](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync) estén habilitados en su instancia de Marketo Engage.
* Hemos actualizado el punto de conexión [Obtener contenido dinámico para correo electrónico](https://developer.adobe.com/marketo-apis/api/asset/#operation/getEmailDynamicContentUsingGET) para permitirle recuperar [contenido dinámico](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/using-dynamic-content-in-an-email) de una línea de asunto de correo electrónico. Esto funciona independientemente de si el correo electrónico dado está vinculado a una plantilla de correo electrónico.

`POST /rest/asset/v1/form/{id}/field/State.json?values=[{"label":"Alaska"},{"value":"AK"},{"label":"West Virginia","value":"WV"},{"label":"Wyoming","value":"WY"}]`

* Hemos actualizado el extremo [Agregar reglas de visibilidad de campo de formulario](https://developer.adobe.com/marketo-apis/api/asset/#operation/getAllProgramMemberFieldsUsingGET) para permitirle agregar varios valores de comparación para **isNot**, tipo [Reglas de invisibilidad](/help/rest-api/forms.md). Vea el siguiente ejemplo:

`POST /rest/asset/v1/form/{id}/field/LastName/visibility.json?visibilityRule={"ruleType":"show","rules":[{"subjectField":"LastName","operator":"isNot","values":["A","B","C"]}`

### Resoluciones de defectos

* Se ha corregido un problema con el extremo [Submit Form](/help/rest-api/leads.md) que se produjo al pasar &quot;null&quot; para un atributo en el parámetro [leadFormFields](/help/rest-api/leads.md); se devolvió un error: &quot;611, System Error&quot;. Ahora devuelve correctamente el error &quot;1003, Form validation failed&quot;. [LM-162213]

Publicado el _2022-05-09_ por _David_

## Actualizaciones de agosto de 2022

En agosto de 2022, vamos a mejorar las API de REST existentes. Consulte la lista completa de actualizaciones a continuación.

Hemos añadido varios filtros nuevos que se pueden utilizar al llamar al punto final del trabajo Crear trabajo de miembro del programa de exportación. Tenga en cuenta que muchos de los filtros se pueden utilizar en combinación para restringir el conjunto de datos extraídos.

* El filtro **programIds** se puede usar para especificar hasta 10 identificadores de programa que pueden ayudar a mejorar el rendimiento.
* El filtro **isExhausted** se puede usar para filtrar registros para [personas que han agotado el contenido](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/email-marketing/drip-nurturing/using-engagement-programs/people-who-have-exhausted-content).
* El filtro **nurtureCadence** se puede usar para filtrar registros basados en [cadencia del programa de participación](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/program-flow-actions/change-engagement-program-cadence).
* El filtro **statusNames** se puede usar para filtrar registros para uno o más [estados de programa](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/core-marketo-concepts/programs/creating-programs/understanding-program-membership).
* El filtro **updatedAt** se puede usar para filtrar registros basados en un intervalo de fechas.

### Anuncios

* El comportamiento del extremo [Identity](https://developer.adobe.com/marketo-apis/api/identity/#operation/identityUsingGET) ha cambiado. Cuando llama al extremo y no incluye un parámetro **access_token**, se devuelve el error &quot;603, acceso denegado&quot;. Anteriormente, se devolvía el error &quot;600, Empty access token&quot;. Tenga en cuenta que el error &quot;600, Empty access token&quot; (600, token de acceso vacío) ha quedado obsoleto.

Publicado el _2022-09-03_ por _David_

## Actualizaciones de octubre de 2022

En octubre de 2022, vamos a mejorar las API de REST existentes. Consulte la lista completa de actualizaciones a continuación.

* Hemos mejorado la [API de importación masiva de posibles clientes](/help/rest-api/bulk-lead-import.md) para admitir la adición de posibles clientes a los registros de vendedores durante el proceso de importación. Para ello, incluya el campo **externalSalesPersonId** en el archivo de importación.
* Se ha corregido un problema con el extremo [Crear campos de posibles clientes](/help/rest-api/leads.md) que se producía al crear campos de tipo Puntuación. Estos campos no estaban disponibles para usarlos en la acción de flujo [Cambiar puntuación](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/change-score) en la interfaz de usuario de Marketo Engage. [LM-166815]

### Anuncios

* El atributo de pertenencia a programa `acquiredBy` se ha cambiado para que se pueda actualizar.

Publicado el _2022-10-18_ por _David_

## Próximos cambios en la API de REST de Marketo Forms

A partir de la versión 2022.R2, programada actualmente para la semana del 24 de marzo de 2023, las API de recursos de Adobe Marketo Engage Forms devolverán de forma consistente solo el nombre del formulario sin un nombre de programa prefijado, independientemente de si el formulario es hijo de un programa o no. Este cambio hará que los comportamientos de la API de Forms con respecto a los nombres de los recursos sean coherentes con el resto de las API de recursos de Adobe Marketo Engage. Para evitar interrupciones en el servicio, debe revisar las integraciones que utilizan las API de Forms de Marketo Engage y trabajar con los integradores para ver si es necesario realizar cambios para adaptarse a esto. Antes de este próximo cambio, los nombres devueltos por las API de Forms añadían de forma incoherente un nombre de programa para los formularios que son secundarios de los programas en las actividades de marketing. Por ejemplo, un formulario denominado &quot;Formulario 1&quot;, que era un elemento secundario del &quot;Programa 1&quot;, puede tener su nombre devuelto por la API como: Programa 1.Formulario 1 O Formulario 1 A partir de la versión 2022.R2, el nombre de un formulario siempre se devolverá sin un nombre de programa con prefijo. Con el mismo ejemplo, el nombre siempre sería: Formulario 1

Publicado el _2022-11-04_ por _Kenny_

## Actualizaciones de enero de 2023

En enero de 2023, realizamos una mejora relacionada con la API en la IU del administrador y anunciamos dos cosas. Consulte la lista completa de actualizaciones a continuación.

IU de administración

### Extracción de posibles clientes

* Hemos mejorado la IU de administración de Marketo Engage para permitirle ver la asignación de capacidad diaria de la API de extracción masiva para su suscripción. Además, puede ver el uso de capacidad por parte del usuario de la API durante los últimos 7 días. Encontrará más información [aquí](https://experienceleague.adobe.com/es/docs/marketo/using/product-docs/administration/settings/bulk-export-api-information).

### Resoluciones de defectos

* Se ha corregido un problema con el punto de conexión [Eliminar oportunidades](https://developer.adobe.com/marketo-apis/api/mapi/#operation/deleteOpportunitiesUsingPOST). En algunos casos, no se generaba la actividad &quot;Quitar de la oportunidad&quot;. [LM-172208]

### Anuncios

* Consulte [este artículo](https://nation.marketo.com/t5/product-documents/upcoming-change-to-marketo-rest-api/ta-p/331698) en la comunidad de Marketo con respecto a la API de REST y un cambio en el mensaje de respuesta HTTP [frase de motivo](https://www.rfc-editor.org/rfc/rfc7230#section-3.1.2).
* El atributo de pertenencia a programa **statusReason** se ha cambiado para que se pueda actualizar.

Publicado el _2023-01-21_ por _David_
