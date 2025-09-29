---
title: syncMObjects
feature: SOAP
description: Marketo SOAP syncMObjects para insertar, actualizar o actualizar hasta 100 programas, oportunidades y roles de persona de oportunidad, que devuelven estados e ID de Marketo.
exl-id: 68bb69ce-aa8c-40b7-8938-247f4fe97b5d
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 7%

---

# syncMObjects

Acepta una matriz de [MObjects](marketo-objects.md) para crear o actualizar, hasta un máximo de 100 por llamada, y devuelve el resultado (estado) de la operación (CREATED, UPDATED, FAILED, UNCHANGED, SKIPPED) y los Marketo ID de los MObject(s). La API se puede llamar en uno de los tres modos de funcionamiento:

1. INSERT: insertar solo objetos nuevos, omitir objetos existentes
1. UPDATE: actualizar solo los objetos existentes, omitir los nuevos.
   - Para el objeto del programa, solo se puede llamar a la API en el modo ACTUALIZAR para agregar información nueva del coste del periodo o agregar/actualizar etiquetas/canal para programas existentes. (las etiquetas/canales ya deben estar definidos: no se pueden crear nuevas etiquetas/canales a través de la API)
1. UPSERT - Insertar objetos nuevos y actualizar objetos existentes

Las operaciones UPDATE y UPSERT utilizan el ID como clave. En una sola llamada de API, algunas actualizaciones pueden realizarse correctamente y otras pueden fallar. Se devuelve un mensaje de error por cada error

## Solicitud

| Nombre del campo | Obligatorio/Opcional | Descripción |
| --- | --- | --- |
| mObjectList->mObject->type | Obligatorio | Puede ser uno de:`Program`, `Opportunity`, `OpportunityPersonRole` |
| mObjectList->mObject->id | Obligatorio | ID del objeto MObject. Puede especificar hasta 100 MObjects por llamada. |
| mObjectList->mObject->typeAttribList->typeAttrib->attrType | Obligatorio | El costo (que solo se usa al actualizar el objeto de programa) puede ser uno de: `Cost`, `Tag` |
| mObjectList->mObject->typeAttribList->typeAttrib->attrList->attrib->name | Obligatorio | Para el objeto de programa, los siguientes atributos se pueden pasar como pares de nombre-valor. Costo: `Month (Required)`, `Amount (Required)`, `Id (Cost Id - Optional)`, `Note (Optional)`. Para Etiqueta/Canal: `Type (Required)`, `Value (Required)`. Para el objeto MObject de oportunidad, todos los campos de la salida del objeto [descriptionMObject](describemobject.md) se pueden pasar como pares nombre-valor. A continuación, se muestran todos los campos opcionales y el conjunto estándar de atributos. Es posible que tenga campos adicionales en el objeto MObject de oportunidad creados mediante una solicitud de asistencia. |

1. Monto
1. CloseDate
1. Descripción
1. ExpectedRevenue
1. ExternalCreatedDate
1. Fiscal
1. FiscalQuarter
1. FiscalYear
1. ForecastCategoryName
1. IsClosed
1. IsWon
1. LastActivityDate
1. LeadSource
1. Nombre
1. NextStep
1. Probabilidad
1. Cantidad
1. Fase
1. Tipo

Para el objeto MObject OpportunityPersonRole, puede proporcionar todos los campos de la salida de [describeMObject](./describemobject.md) como pares nombre-valor. El conjunto estándar de atributos en el objeto MObject OpportunityPersonRole se enumera aquí:

1. OpportunityId (obligatorio)
1. PersonId (obligatorio)
   1. Corresponde al ID de persona/contacto en Marketo.
1. Rol (opcional)
1. ExternalCreatedDate (opcional)
1. IsPrimary (Opcional)
1. Rol (opcional)

| Nombre del campo | Obligatorio/Opcional | Descripción |
| --- | --- | --- |
| mObjAssociationList->mObjAssociation->mObjType | Opcional | Se utiliza para actualizar los MObjects de Opportunity/OpportunityPersonRole mediante el identificador o la clave externa de un objeto asociado. Los objetos asociados pueden ser uno de: Compañía (para actualizar el objeto MObject de la oportunidad), Posible cliente (para actualizar el objeto MObject de la persona de la oportunidad), Oportunidad (para actualizar el objeto MObject de la persona de la oportunidad) |
| mObjAssociationList->mObjAssociation->id | Opcional | El ID del objeto asociado (cliente potencial/compañía/oportunidad) |
| mObjAssociationList->mObjAssociation->externalKey | Opcional | Un atributo personalizado del objeto asociado |

## Solicitar XML

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns1="http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809934544BFABAE58E5D27</mktowsUserId>
      <requestSignature>40e6d89bd2f7f0daaddaf150ce7a7ca49788ffe7</requestSignature>
      <requestTimestamp>2013-08-05T14:22:45-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsSyncMObjects>
      <mObjectList>
        <mObject>
          <type>Program</type>
          <id>1970</id>
          <typeAttribList>
            <typeAttrib>
              <attrType>Cost</attrType>
              <attrList>
                <attrib>
                  <name>Month</name>
                  <value>2013-06</value>
                </attrib>
                <attrib>
                  <name>Amount</name>
                  <value>2000</value>
                </attrib>
                <attrib>
                  <name>Id</name>
                  <value>153</value>
                </attrib>
              </attrList>
            </typeAttrib>
          </typeAttribList>
        </mObject>
      </mObjectList>
      <operation>UPDATE</operation>
    </ns1:paramsSyncMObjects>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## XML de respuesta

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns1="http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successSyncMObjects>
      <result>
        <mObjStatusList>
          <mObjStatus>
            <id>1970</id>
            <status>UPDATED</status>
          </mObjStatus>
        </mObjStatusList>
      </result>
    </ns1:successSyncMObjects>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## Código de muestra: PHP

```php
<?php
$debug = true;
$marketoSoapEndPoint    = "";  // CHANGE ME
$marketoUserId      = ""; // CHANGE ME
$marketoSecretKey   = ""; // CHANGE ME
$marketoNameSpace   = "http://www.marketo.com/mktows/";

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
$options = array("connection_timeout" => 15, "location" => $marketoSoapEndPoint);
if ($debug) {
  $options["trace"] = 1;
}

// Create Request
////////////////////////////////
$params = prepareUpdateProgramRequest();
// -or-
//$params = prepareCreateOpptyRequest();
// -or-
//$params= prepareCreateOpptyPersonRoleRequest();
////////////////////////////////

$soapClient = new SoapClient($marketoSoapEndPoint ."?WSDL", $options);
try {
  $leads = $soapClient->__soapCall('syncMObjects', array($params), $options, $authHdr);
}
catch(Exception $ex) {
  var_dump($ex);
}

if ($debug) {
  print "RAW request:\n" .$soapClient->__getLastRequest() ."\n";
  print "RAW response:\n" .$soapClient->__getLastResponse() ."\n";
}

function prepareUpdateProgramRequest() {
  $params = new stdClass();

  $mObj = new stdClass();
  $mObj->type = 'Program';
  $mObj->id="1970";

  $attrib1 = new stdClass();
  $attrib1->name="Month";
  $attrib1->value="2013-06";

  $attrib2 = new stdClass();
  $attrib2->name="Amount";
  $attrib2->value="2000";

  $attrib3 = new stdClass();
  $attrib3->name="Id";
  $attrib3->value="153";
  $attribList = array ($attrib1, $attrib2, $attrib3);

  $costAttrib = new stdClass();
  $costAttrib->attrType="Cost";
  $costAttrib->attrList = $attribList;

  $mObj->typeAttribList= array($costAttrib);
  $params->mObjectList = array($mObj);

  $params->operation="UPDATE";

  return $params;
}

function prepareCreateOpptyRequest() {
  $params = new stdClass();

  $mObj = new stdClass();
  $mObj->type = 'Opportunity';

  $attrib1 = new stdClass();
  $attrib1->name="Name";
  $attrib1->value="Q1 2014";

  $attrib2 = new stdClass();
  $attrib2->name="Amount";
  $attrib2->value="2000";

  $attrib3 = new stdClass();
  $attrib3->name="Probability";
  $attrib3->value="80%";
  $attribs = array ($attrib1, $attrib2, $attrib3);

  $attribList = new stdClass();
  $attribList->attrib = $attribs;

  $mObj->attribList = $attribList;
  $params->mObjectList = array($mObj);

  $params->operation="INSERT";

  return $params;
}

function prepareCreateOpptyPersonRoleRequest() {
  $params = new stdClass();

  $mObj = new stdClass();
  $mObj->type = 'OpportunityPersonRole';

  $attrib1 = new stdClass();
  $attrib1->name="OpportunityId";
  $attrib1->value="64";

  $attrib2 = new stdClass();
  $attrib2->name="PersonId";
  $attrib2->value="19";

  $attrib3 = new stdClass();
  $attrib3->name="Role";
  $attrib3->value="Influencer/Champion";
  $attribs = array ($attrib1, $attrib2, $attrib3);

  $attribList = new stdClass();
  $attribList->attrib = $attribs;

  $mObj->attribList = $attribList;
  $params->mObjectList = array($mObj);

  $params->operation="INSERT";
  return $params;
}
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
import javax.xml.bind.Marshaller;


public class SyncMObjects {
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
            ////////////////////////////////
            ParamsSyncMObjects request = prepareUpdateProgramRequest();
            // -or-
            //ParamsSyncMObjects request = prepareCreateOpptyRequest();
            // -or-
            //ParamsSyncMObjects request = prepareCreateOpptyPersonRoleRequest();
            ////////////////////////////////

            SuccessSyncMObjects result = port.syncMObjects(request, header);

            JAXBContext context = JAXBContext.newInstance(SuccessSyncMObjects.class);
            Marshaller m = context.createMarshaller();
            m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
            m.marshal(result, System.out);

        }
        catch (Exception e) {
            e.printStackTrace();
        }
    }

    private static ParamsSyncMObjects prepareUpdateProgramRequest() {
        ParamsSyncMObjects request = new ParamsSyncMObjects();
        request.setOperation(SyncOperationEnum.UPDATE);

        MObject mobj = new MObject();
        mobj.setType("Program");
        mobj.setId(1970);

        TypeAttrib typeAttrib = new TypeAttrib();
        typeAttrib.setAttrType("Cost");

        Attrib attrib = new Attrib();
        attrib.setName("Month");
        attrib.setValue("2013-06");

        Attrib attrib2 = new Attrib();
        attrib1.setName("Amount");
        attrib1.setValue("2000");

        Attrib attrib3 = new Attrib();
        attrib1.setName("Id");
        attrib1.setValue("153");

        ArrayOfAttrib attribList = new ArrayOfAttrib();
        attribList.getAttribs().add(attrib);
        attribList.getAttribs().add(attrib2);
        attribList.getAttribs().add(attrib3);
        typeAttrib.setAttrList(attribList);

        ArrayOfTypeAttrib typeAttribList = new ArrayOfTypeAttrib();
        typeAttribList.getTypeAttribs().add(typeAttrib);
        mobj.setTypeAttribList(typeAttribList);

        ArrayOfMObject objList = new ArrayOfMObject();
        objList.getMObjects().add(mobj);
        request.setMObjectList(objList);

        return request;
    }

    private static ParamsSyncMObjects prepareCreateOpptyRequest() {
        ParamsSyncMObjects request = new ParamsSyncMObjects();
        request.setOperation(SyncOperationEnum.INSERT);

        MObject mobj = new MObject();
        mobj.setType("Opportunity");

        Attrib attrib = new Attrib();
        attrib.setName("Name");
        attrib.setValue("Q1 2014");

        Attrib attrib2 = new Attrib();
        attrib1.setName("Amount");
        attrib1.setValue("2000");

        Attrib attrib3 = new Attrib();
        attrib1.setName("Probability");
        attrib1.setValue("80%");

        ArrayOfAttrib attribList = new ArrayOfAttrib();
        attribList.getAttribs().add(attrib);
        attribList.getAttribs().add(attrib2);
        attribList.getAttribs().add(attrib3);
        mobj.setAttribList(attribList);

        ArrayOfMObject objList = new ArrayOfMObject();
        objList.getMObjects().add(mobj);
        request.setMObjectList(objList);

        return request;
    }
    private static ParamsSyncMObjects prepareCreateOpptyPersonRoleRequest() {
        ParamsSyncMObjects request = new ParamsSyncMObjects();
        request.setOperation(SyncOperationEnum.INSERT);

        MObject mobj = new MObject();
        mobj.setType("OpportunityPersonRole");

        Attrib attrib = new Attrib();
        attrib.setName("OpportunityId");
        attrib.setValue("64"); // Id of the opportunity created earlier

        Attrib attrib2 = new Attrib();
        attrib1.setName("PersonId");
        attrib1.setValue("19");

        Attrib attrib3 = new Attrib();
        attrib1.setName("Role");
        attrib1.setValue("Influencer/Champion");

        ArrayOfAttrib attribList = new ArrayOfAttrib();
        attribList.getAttribs().add(attrib);
        attribList.getAttribs().add(attrib2);
        attribList.getAttribs().add(attrib3);
        mobj.setAttribList(attribList);

        ArrayOfMObject objList = new ArrayOfMObject();
        objList.getMObjects().add(mobj);
        request.setMObjectList(objList);

        return request;
    }

}
```

## Código de muestra: Ruby

```ruby
require 'savon' # Use version 1.0 Savon gem
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
    :m_object_list => {
          :m_object => {
              :type => "Program",
              :id => "1970",
              :type_attrib_list => {
                  :type_attrib => {
                      :attr_type => "Cost",
                      :attr_list => {
                          :attrib => {
                              :name => "Month",
                              :value => "2013-06" },
                        :attrib! => {
                              :name => "Amount",
                              :value =>  "2000" },
                        :attrib! => {
                              :name => "Id",
                              :value => "153" }
                    }
                }
            }
        }
    },
      :operation => "UPDATE"
}

response = client.call(:sync_m_objects, message: request)

puts response
```
