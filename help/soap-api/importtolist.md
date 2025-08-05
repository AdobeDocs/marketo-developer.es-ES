---
title: importToList
feature: SOAP
description: Llamadas SOAP importToList
exl-id: 7e4930a9-a78f-44a3-9e8c-eeca908080c8
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '387'
ht-degree: 4%

---

# importToList

Esta función permite importar una lista de posibles clientes en una lista estática existente en Marketo, de forma similar a la función Importar lista de la interfaz de usuario de Marketo.

**Formato de importación:** Estos valores son idénticos a la estructura de un archivo CSV utilizado en una importación de lista.

**Ejemplo:**

| Correo electrónico | Primer/a | Último/a |
| --- | --- | --- |
| <joe@company.com> | Joe | Smith |
| <mary@company.com> | Mary | Rodgers |
| <wanda@megacorp.com> | Wanda | Williams |

Los valores de `displayName` deben usarse en los valores de `importFileHeader` en lugar de en los valores de `name`.

**Contenido dinámico de correo electrónico:** De forma opcional, puede pasar valores por posible cliente que actúen como reemplazos de Mis tokens en un mensaje de correo electrónico.

| Correo electrónico | Primer/a | Último/a | {{my.specialToken}} | {{my.otherToken}} |
| --- | --- | --- | --- | --- |
| <joe@company.com> | Joe | Smith | Pescado | Azul |
| <mary@company.com> | Mary | Rodgers | Pollo | Marrón |
| <wanda@megacorp.com> | Wanda | Williams | Vegetal | Hazel |

**Importante:** Si agrega tokens para los posibles clientes, debe especificar la campaña inteligente que los utiliza. La próxima vez que se ejecute la campaña inteligente especificada, utilizará los valores de la lista, en lugar de los valores normales de Mi token. Después de ejecutar una sola campaña, los tokens se descartan.

`importToList` puede tardar un tiempo en completarse, especialmente en el caso de listas grandes. Si planea usar la lista recién importada en otras llamadas a la API, debe usar `importToListStatus` para comprobar que la operación se haya completado.

**Nota:** La importación de valores NULL para campos numéricos en un archivo CSV puede generar una actividad &quot;Cambiar valor de datos&quot; para esos campos, incluso si el campo ya está en blanco. Cualquier campaña inteligente que utilice un Déclencheur &quot;Valor de datos cambiado&quot; o &quot;Cambios en el valor de datos&quot; puede provocar que los posibles clientes califiquen para esas campañas aunque los datos no estén cambiando realmente. Utilice restricciones en estos filtros o déclencheur para asegurarse de que los posibles clientes no cumplen los requisitos para las campañas incorrectas al realizar importaciones.

## Solicitud

| Nombre del campo | Obligatorio/Opcional | Descripción |
| --- | --- | --- |
| programName | Obligatorio | Nombre del programa que contiene la lista estática |
| campaignName | Opcional | Si se anula el uso de Mi token, este es el nombre de la campaña en la que se utilizarán esos tokens. La campaña debe estar dentro del programa especificado. |
| listName | Obligatorio | Nombre de la lista estática de Marketo a la que se agregan los posibles clientes |
| importFileHeader | Obligatorio | Encabezados de columna para los posibles clientes que se van a importar, incluidos los nombres de atributos de posibles clientes y mis tokens. |
| importFileRows->stringItem | Obligatorio | Valores separados por comas, con una fila por posible cliente |
| importListMode | Obligatorio | Opciones válidas: `UPSERTLEADS` y `LISTONLY` |
| clearList | Opcional | Si es true, la lista estática se borra antes de la importación; si se anexan falsos posibles clientes. |

## Solicitar XML

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns1="http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809934544BFABAE58E5D27</mktowsUserId>
      <requestSignature>17bf0b9e412e58eec836dc557ca9433f666944b6</requestSignature>
      <requestTimestamp>2013-08-05T14:56:58-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsImportToList>
      <programName>Trav-Demo-Program</programName>
      <importFileHeader>Last Name,First Name,Job Title,Company Name,Email Address</importFileHeader>
      <importFileRows>
        <stringItem>Awesomesauce,Developer,Code Slinger,Marketo,dawesomesauce@marketo.com</stringItem>
        <stringItem>Doe,Jane,VP Marketing,Jane Consulting,jdoe@janeconsulting.com</stringItem>
      </importFileRows>
      <importListMode>UPSERTLEADS</importListMode>
      <listName>Trav-Test-List</listName>
      <clearList>false</clearList>
      <campaignName>Batch Campaign Example</campaignName>
    </ns1:paramsImportToList>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## XML de respuesta

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns1="http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successImportToList>
      <result>
        <!-- Possible Values: COMPLETE/PROCESSING/FAILED/CANCELED -->
        <importStatus>PROCESSING</importStatus>
      </result>
    </ns1:successImportToList>
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
  $options = array("connection_timeout" => 20, "location" => $marketoSoapEndPoint);
  if ($debug) {
    $options["trace"] = true;
  }

  // Create Request
  $request = new stdClass();
  $request->programName = "Trav-Demo-Program";
  $request->campaignName = "Batch Campaign Example";
  $request->importFileHeader = "Last Name,First Name,Job Title,Company Name,Email Address";


  $request->importFileRows = array("Awesomesauce,Developer,Code Slinger,Marketo,dawesomesauce@marketo.com","Doe,Jane,VP Marketing,Jane Consulting,jdoe@janeconsulting.com");
  $request->importListMode = "UPSERTLEADS"; // UPSERTLEADS or LISTONLY
  $request->listName = "Trav-Test-List";
  $request->clearList = false;
  $params = array("paramsImportToList" => $request);

  $soapClient = new SoapClient($marketoSoapEndPoint ."?WSDL", $options);
  try {
    $response = $soapClient->__soapCall('importToList', $params, $options, $authHdr);
  }
  catch(Exception $ex) {
    var_dump($ex);
  }
  if ($debug) {
    print "RAW request:\n" .$soapClient->__getLastRequest() ."\n";
    print "RAW response:\n" .$soapClient->__getLastResponse() ."\n";
  }

  print_r($response);
?>
```

## Código de muestra: Java

```java
import com.marketo.mktows.*;
import java.net.URL;
import javax.xml.namespace.QName;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.ArrayList;
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

public class ImportToList {

    public static void main(String[] args) {
        System.out.println("Executing Import To List");
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
            ParamsImportToList request = new ParamsImportToList();

            request.setProgramName("Trav-Demo-Program");
            request.setCampaignName("Batch Campaign Example");
            request.setImportFileHeader("Last Name,First Name,Job Title,Company Name,Email Address");

            ArrayOfString rows = new ArrayOfString();
            rows.getStringItems().add("Awesomesauce,Developer,Code Slinger,Marketo,dawesomesauce@marketo.com");
            rows.getStringItems().add("Doe,Jane,VP Marketing,Jane Consulting,jdoe@janeconsulting.com");
            request.setImportFileRows(rows);

            request.setImportListMode(ImportToListModeEnum.UPSERTLEADS);
            request.setListName("Trav-Test-List");
            request.setClearList(false);

            SuccessImportToList result = port.importToList(request, header);


            JAXBContext context = JAXBContext.newInstance(SuccessImportToList.class);
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
    'ns1:AuthenticationHeader' => { "mktowsUserId" => mktowsUserId, "requestSignature" => requestSignature,
    "requestTimestamp"  => requestTimestamp
    }
}

client = Savon.client(wsdl: 'http://app.marketo.com/soap/mktows/2_3?WSDL', soap_header: headers, endpoint: marketoSoapEndPoint, open_timeout: 90, read_timeout: 90, namespace_identifier: :ns1, env_namespace: 'SOAP-ENV')

#Create Request
request = {
    :program_name => "Trav-Demo-Program",
    :import_file_header => "Last Name,First Name,Job Title,Company Name,Email Address",
    :import_file_rows => {
        :string_item => ["Awesomesauce,Developer,Code Slinger,Marketo,dawesomesauce@marketo.com", "Doe,Jane,VP Marketing,Jane Consulting,jdoe@janeconsulting.com"] },
    :import_list_mode => "UPSERTLEADS",
    :list_name => "Trav-Test-List",
    :clear_list => "false",
    :campaign_name => "Batch Campaign Example"
}

response = client.call(:import_to_list, message: request)

puts response
```
