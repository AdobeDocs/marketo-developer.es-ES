---
title: syncLead
feature: SOAP
description: Aprenda a utilizar Marketo SOAP syncLead para insertar o actualizar un solo posible cliente, gestionar identificadores y espacios de trabajo, con campos de solicitud, ejemplos XML y PHP.
exl-id: e6cda794-a9d4-4153-a5f3-52e97a506807
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '540'
ht-degree: 2%

---

# syncLead

Esta función inserta o actualiza un único registro de posibles clientes. Al actualizar un posible cliente existente, este se identifica con una de las siguientes claves:

- Identificación de Marketo
- ID de sistema externo (implementado como `foreignSysPersonId`)
- Cookie de Marketo (creada por un script JS de Munchkin)
- Correo electrónico

Si se encuentra una coincidencia existente, la llamada realiza una actualización. Si no es así, inserta y crea un posible cliente. Los posibles clientes anónimos se pueden actualizar con el ID de cookie de Marketo y se conocerán tras la actualización.

Excepto el correo electrónico, todos estos identificadores se tratan como claves únicas. El ID de Marketo tiene prioridad sobre el resto de claves. Si tanto `foreignSysPersonId` como el Marketo ID están presentes en el registro de posibles clientes, el Marketo ID tiene prioridad y `foreignSysPersonId` se actualizará para ese posible cliente. Si se proporciona el único `foreignSysPersonId`, se utiliza como identificador único. Si tanto `foreignSysPersonId` como el correo electrónico están presentes pero el Marketo ID no está presente, el `foreignSysPersonId` tiene prioridad y el correo electrónico se actualizará para ese posible cliente.

De forma opcional, se puede especificar una cabecera de contexto para asignar un nombre al espacio de trabajo de destino.

Cuando se habilitan espacios de trabajo de Marketo y se utiliza el encabezado, se aplican las siguientes reglas:

- Si se establecen reglas de asignación y un nuevo posible cliente cumple los requisitos de cualquiera de las reglas configuradas, se crean nuevos posibles clientes en la partición definida por la regla de asignación. De lo contrario, se crean nuevos posibles clientes en la partición principal del espacio de trabajo con nombre.
- Los posibles clientes coincidentes con el ID de posible cliente de Marketo, un ID de sistema externo o una cookie de Marketo deben existir en la partición principal del espacio de trabajo designado; de lo contrario, se devuelve un error
- Si un posible cliente existente coincide por correo electrónico, el espacio de trabajo con nombre se ignora y el posible cliente se actualiza en la partición actual del cliente

Cuando los espacios de trabajo de Marketo están activados y el encabezado NO se utiliza, se aplican las siguientes reglas:

- Si se establecen reglas de asignación y un nuevo posible cliente cumple los requisitos de cualquiera de las reglas configuradas, se crean nuevos posibles clientes en la partición definida por la regla de asignación. De lo contrario, se crean nuevos posibles clientes en la partición principal del espacio de trabajo &quot;Predeterminado&quot;.
- Los posibles clientes existentes se actualizan en su partición actual

Si los espacios de trabajo de Marketo NO están activados, el espacio de trabajo de destino DEBE ser el espacio de trabajo &quot;predeterminado&quot;. No es necesario pasar el encabezado.

## Solicitud

| Nombre del campo | Obligatorio/Opcional | Descripción |
| --- | --- | --- |
| leadRecord->Id | Obligatorio: solo cuando el correo electrónico o `foreignSysPersonId` no están presentes | El ID de Marketo del registro de posibles clientes |
| leadRecord->Correo electrónico | Obligatorio: solo cuando el identificador o `foreignSysPersonId` no están presentes | La dirección de correo electrónico asociada al registro de posibles clientes |
| leadRecord->`foreignSysPersonId` | Obligatorio: Solo cuando el ID o el correo electrónico no están presentes | Identificador del sistema externo asociado al registro de posibles clientes |
| leadRecord->ForeignSysType | Opcional: solo es necesario cuando `foreignSysPersonId` está presente | El tipo de sistema externo. Valores posibles: CUSTOM, SFDC, NETSUITE |
| leadRecord->leadAttributeList->attribute->attrName | Obligatorio | El nombre del atributo de posible cliente cuyo valor desea actualizar. |
| leadRecord->leadAttributeList->attribute->attrValue | Obligatorio | El valor que desea establecer en el atributo de posible cliente especificado en attrName. |
| returnLead | Obligatorio | Cuando es true, devuelve el registro de posibles clientes actualizado completo tras la actualización. |
| marketoCookie | Opcional | La cookie [Munchkin javascript](../javascript-api/lead-tracking.md) |

## Solicitar XML

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns1="http://www.marketo.com/mktows/">
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
        <Email>t@t.com</Email>
        <leadAttributeList>
          <attribute>
            <attrName>FirstName</attrName>
            <attrValue>George</attrValue>
          </attribute>
          <attribute>
            <attrName>LastName</attrName>
            <attrValue>of the Jungle</attrValue>
          </attribute>
        </leadAttributeList>
      </leadRecord>
      <returnLead>false</returnLead>
    </ns1:paramsSyncLead>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## XML de respuesta

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:ns1="http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successSyncLead>
      <result>
        <leadId>1089965</leadId>
        <syncStatus>
          <leadId>1089965</leadId>
          <status>UPDATED</status>
          <error xsi:nil="true" />
        </syncStatus>
        <leadRecord xsi:nil="true" />
      </result>
    </ns1:successSyncLead>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## Código de muestra: PHP

```php
 <?php

  $debug = true;

  $marketoSoapEndPoint     = "";  // CHANGE ME
  $marketoUserId           = "";  // CHANGE ME
  $marketoSecretKey        = "";  // CHANGE ME
  $marketoNameSpace        = "http://www.marketo.com/mktows/";

  // Create Signature
  $dtzObj = new DateTimeZone("America/Los_Angeles");
  $dtObj  = new DateTime('now', $dtzObj);
  $timeStamp = $dtObj->format(DATE_W3C);
  $encryptString = $timeStamp . $marketoUserId;
  $signature = hash_hmac('sha1', $encryptString, $marketoSecretKey);

  // Create SOAP Header
  $attrs = new stdClass();
  $attrs->mktowsUserId = $marketoUserId;
  $attrs->requestSignature = $signature;
  $attrs->requestTimestamp = $timeStamp;
  $authHdr = new SoapHeader($marketoNameSpace, 'AuthenticationHeader', $attrs);
  $options = array("connection_timeout" =20, "location" =$marketoSoapEndPoint);
  if ($debug) {
    $options["trace"] = true;
  }

  // Create Request
  $leadKey = new stdClass();
  $leadKey->Email = "george@jungle.com";

  // Lead attributes to update
  $attr1 = new stdClass();
  $attr1->attrName  = "FirstName";
  $attr1->attrValue = "George";

  $attr2= new stdClass();
  $attr2->attrName  = "LastName";
  $attr2->attrValue = "of the Jungle";

  $attrArray = array($attr1, $attr2);
  $attrList = new stdClass();
  $attrList->attribute = $attrArray;
  $leadKey->leadAttributeList = $attrList;

  $leadRecord = new stdClass();
  $leadRecord->leadRecord = $leadKey;
  $leadRecord->returnLead = false;
  $params = array("paramsSyncLead" =$leadRecord);

  $soapClient = new SoapClient($marketoSoapEndPoint ."?WSDL", $options);
  try {
    $result = $soapClient->__soapCall('syncLead', $params, $options, $authHdr);
  }
  catch(Exception $ex) {
    var_dump($ex);
  }

  if ($debug) {
    print "RAW request:\n" .$soapClient->__getLastRequest() ."\n";
    print "RAW response:\n" .$soapClient->__getLastResponse() ."\n";
  }
  print_r($result);

?>
```

## Código de muestra: Java

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


public class SyncLead {

    public static void main(String[] args) {
        System.out.println("Executing syncLead");
        try {
            URL marketoSoapEndPoint = new URL("https://100-AEK-913.mktoapi.com/soap/mktows/2_1" + "?WSDL");
            String marketoUserId = "demo17_1_809934544BFABAE58E5D27";
            String marketoSecretKey = "27272727aa";

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
            ParamsSyncLead request = new ParamsSyncLead();
            LeadRecord key = new LeadRecord();

            ObjectFactory objectFactory = new ObjectFactory();
            JAXBElement<Stringemail = objectFactory.createLeadRecordEmail("george@jungle.com");
            key.setEmail(email);
            request.setLeadRecord(key);

            Attribute attr1 = new Attribute();
            attr1.setAttrName("FirstName");
            attr1.setAttrValue("George2");

            Attribute attr2 = new Attribute();
            attr2.setAttrName("LastName");
            attr2.setAttrValue("of the Jungle");

            ArrayOfAttribute aoa = new ArrayOfAttribute();
            aoa.getAttributes().add(attr1);
            aoa.getAttributes().add(attr2);

            QName qname = new QName("http://www.marketo.com/mktows/", "leadAttributeList");
            JAXBElement<ArrayOfAttributeattrList = new JAXBElement(qname, ArrayOfAttribute.class, aoa);
            key.setLeadAttributeList(attrList);

            MktowsContextHeader headerContext = new MktowsContextHeader();
            headerContext.setTargetWorkspace("default");

            SuccessSyncLead result = port.syncLead(request, header, headerContext);

            JAXBContext context = JAXBContext.newInstance(SuccessSyncLead.class);
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

## Código de muestra: Ruby

```ruby
require 'savon' # Use version 2.0 Savon gem
require 'date'

mktowsUserId = "" # CHANGE ME
marketoSecretKey = "" # CHANGE ME
marketoSoapEndPoint = "" # CHANGE ME
marketoNameSpace = "http://www.marketo.com/mktows/"

#Create Signature
Timestamp = DateTime.now
requestTimestamp = Timestamp.to_s
encryptString = requestTimestamp + mktowsUserId
digest = OpenSSL::Digest.new('sha1')
hashedsignature = OpenSSL::HMAC.hexdigest(digest, marketoSecretKey, encryptString)
requestSignature = hashedsignature.to_s

#Create SOAP Header
headers = {
    'ns1:AuthenticationHeader' ={ "mktowsUserId" =mktowsUserId, "requestSignature" =requestSignature,
    "requestTimestamp"  =requestTimestamp
    }
}

client = Savon.client(wsdl: 'http://app.marketo.com/soap/mktows/2_3?WSDL', soap_header: headers, endpoint: marketoSoapEndPoint, open_timeout: 90, read_timeout: 90, namespace_identifier: :ns1, env_namespace: 'SOAP-ENV')

#Create Request
request = {
    :lead_record ={
        :Email ="t@t.com",
          :lead_attribute_list ={
              :attribute ={
                :attr_name ="FirstName",
                :attr_value ="George" },
              :attribute! ={
                :attr_name ="LastName",
                :attr_value ="of the Jungle" }
        }
    },
    :return_lead ="false"
}

response = client.call(:sync_lead, message: request)

puts response
```
