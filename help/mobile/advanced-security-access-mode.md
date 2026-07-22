---
title: Modo de acceso de seguridad avanzado
feature: Mobile Marketing
description: Descubra el modo de acceso de seguridad avanzado para Marketo Mobile SDK, con generación de firmas HMAC, configuración de extremos de servidor, uso de ID de dispositivo y ejemplos de iOS y Android
exl-id: bd4730ff-708b-465e-b494-485a4dbf67ff
TQID: https://experienceleague.adobe.com/F6lH1aGbCakK-E6IU4wLwYw58BG2-CRE-Ras2bMHeO8
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 217
ht-degree: 1%

---

# Modo de acceso de seguridad avanzado

El modo de acceso de seguridad avanzado requiere que Marketo SDK recupere y establezca una firma de seguridad. SDK proporciona métodos para establecer y quitar la firma y un método de utilidad para recuperar el ID del dispositivo.

Al iniciar sesión, envíe el ID de dispositivo y la dirección de correo electrónico al servidor del cliente para calcular la firma de seguridad. A continuación, SDK llama al extremo del cliente para recuperar los campos necesarios para crear una instancia del objeto de firma. Si el modo de acceso de seguridad está habilitado en Marketo Mobile Admin, debe establecer esta firma en SDK.

## Configuración del modo de acceso seguro

Implemente esta configuración antes de activar el modo de acceso seguro en la página Administración de Marketo > Aplicaciones y dispositivos móviles.

El modo de acceso seguro requiere un algoritmo de firma del lado del servidor y un punto final de cliente. El extremo devuelve los siguientes valores:

- Clave de acceso
- Firma calculada
- Marca de tiempo de caducidad
- Dirección de correo electrónico

El algoritmo utiliza la clave de acceso de usuario, el secreto de acceso, la dirección de correo electrónico, la marca de tiempo y el ID de dispositivo. El cliente debe configurar el punto de conexión, implementar el cálculo de firma y mantener la marca de tiempo de caducidad actualizada.

```python
import argparse
import datetime
import hashlib
import hmac


ACCESS_KEY = 'Your Access Key'
ACCESS_SECRET = 'Your access secret'

# Key should not be unicode
def get_signing_key(timestamp):
    return 'MKTO' + ACCESS_SECRET + str(timestamp)

def get_string_to_sign(email, uuid):
    return email + uuid

def get_hmac(key, string_to_sign):
    return hmac.new(key, string_to_sign.encode('utf-8'), hashlib.sha256).hexdigest()

def get_epoch_plus_day():
    epoch = datetime.datetime.utcfromtimestamp(0)
    valid_until_dt = datetime.datetime.utcnow() + datetime.timedelta(days=1)
    return long((valid_until_dt - epoch).total_seconds())

if __name__ == '__main__':
    parser = argparse.ArgumentParser()
    parser.add_argument("-e", "--email", required=True, help="email address")
    parser.add_argument("-u", "--uuid", required=True, help="Device install id")
    parser.add_argument("-t", "--timestamp", type=int, help="Valid until timestamp")
    args = parser.parse_args()
    string_to_sign = get_string_to_sign(args.email, args.uuid)
    if not args.timestamp:
        valid_until = get_epoch_plus_day()
    else:
        valid_until = args.timestamp
    signing_key = get_signing_key(valid_until)
    hmac_string = get_hmac(signing_key, string_to_sign)
    print 'HMAC is ', hmac_string
```

Utilice los métodos específicos de la plataforma para establecer o quitar la firma de seguridad y recuperar el ID del dispositivo.

### iOS

```objectivec
Marketo * sharedInstance =[Marketo sharedInstance];

// set secure signature
MKTSecuritySignature *signature =
[[MKTSecuritySignature alloc] initWithAccessKey:<ACCESS_KEY> signature:<SIGNATURE_TOKEN> timestamp:<EXPIRY_TIMESTAMP> email:<EMAIL>];
[sharedInstance setSecureSignature:signature];

// remove signature
[sharedInstance removeSecureSignature];

// get device id
[sharedInstance getDeviceId];
```

```swift
let sharedInstance = Marketo.sharedInstance()

 // set secure signature
let signature = MKTSecuritySignature(accessKey: <ACCESS_KEY>, signature: <SIGNATURE_TOKEN> , timestamp: <EXPIRY_TIMESTAMP>, email: <EMAIL>)
sharedInstance.setSecureSignature(signature)

// remove signature
[sharedInstance removeSecureSignature];

// get device id
sharedInstance.getDeviceId()
```

### Android

```java
Marketo sdk = Marketo.getInstance(getApplicationContext());

// set signature
MarketoConfig.SecureMode secureMode = new MarketoConfig.SecureMode();
secureMode.setAccessKey(<ACCESS_KEY>);
secureMode.setEmail(<EMAIL_ADDRESS>);
secureMode.setSignature(<SIGNATURE_TOKEN>);
secureMode.setTimestamp(<EXPIRY_DATE>);
if (secureMode.isValid()) {
  sdk.setSecureSignature(secureMode);
}

// remove signature
sdk.removeSecureSignature();

// get device id
sdk.getDeviceId();
```
