---
title: Modo de acceso de seguridad avanzado
feature: Mobile Marketing
description: Descubra el modo de acceso de seguridad avanzado para Marketo Mobile SDK, con generación de firmas HMAC, configuración de extremos de servidor, uso de ID de dispositivo y ejemplos de iOS y Android
exl-id: bd4730ff-708b-465e-b494-485a4dbf67ff
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '321'
ht-degree: 0%

---

# Modo de acceso de seguridad avanzado

Marketo SDK expone métodos para establecer y quitar la firma de seguridad. También existe un método de utilidad para recuperar el ID del dispositivo. El ID del dispositivo debe pasarse junto con el correo electrónico, al iniciar sesión, al servidor del cliente para su uso en el cálculo de la firma de seguridad. La SDK debe mostrar el nuevo extremo de visita, que señala al algoritmo mencionado anteriormente, para recuperar los campos necesarios para crear una instancia del objeto de firma. La configuración de esta firma en SDK es un paso necesario si el modo de acceso de seguridad se ha habilitado en Marketo Mobile Admin.

## Configuración del modo de acceso seguro

Esta configuración debe implementarse antes de habilitar el modo de acceso seguro en la página Administración de Marketo > Aplicaciones y dispositivos móviles. Los siguientes pasos describen el proceso necesario para completar el proceso de validación de seguridad:

El modo de acceso seguro requiere la implementación del algoritmo de firma en el lado del servidor del cliente que proporcionará un punto final para recuperar la clave de acceso, la firma calculada, la marca de tiempo de caducidad y el correo electrónico. Este algoritmo requiere la clave de acceso del usuario, el secreto de acceso, el correo electrónico, la marca de tiempo y el ID de dispositivo para realizar el cálculo. El cliente es responsable de configurar el punto de conexión, implementar el algoritmo para realizar cálculos de firma y mantener la marca de tiempo de caducidad fresca.

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

Marketo SDK expone nuevos métodos para establecer y quitar la firma de seguridad. También existe un método de utilidad para recuperar el ID del dispositivo. El ID del dispositivo debe pasarse junto con el correo electrónico, al iniciar sesión, al servidor del cliente para su uso en el cálculo de la firma de seguridad. La SDK debe mostrar el nuevo extremo de visita, que señala al algoritmo mencionado anteriormente, para recuperar los campos necesarios para crear una instancia del objeto de firma. La configuración de esta firma en SDK es un paso necesario si el modo de acceso de seguridad se ha habilitado en Marketo Mobile Admin.

### iOS

```
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

```
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

```
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
