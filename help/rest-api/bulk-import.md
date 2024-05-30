---
title: "Importación masiva"
feature: REST API
description: '"Importación por lotes de datos de persona".'
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '554'
ht-degree: 2%

---


# Importación masiva

Marketo proporciona interfaces para la inserción de grandes conjuntos de datos relacionados con personas y personas, denominados Importación masiva. Actualmente, se ofrecen interfaces para tres tipos de objetos:

- Posibles clientes (personas)
- Objetos personalizados
- Miembros del programa

La importación masiva se realiza creando un trabajo y, a continuación, esperando a que el trabajo complete la lectura de un archivo. Estos trabajos se ejecutan de forma asíncrona y se pueden sondear para recuperar el estado de la importación. Los archivos se cargan mediante HTTP multipart/form-data según RFC 2399.

Los extremos de API en lote no tienen el prefijo &#39;/rest&#39; como otros extremos.

## Autenticación

Las API de importación masiva utilizan el mismo método de autenticación OAuth 2.0 que otras API de REST de Marketo.  Esto requiere que se incruste un token de acceso válido como parámetro de cadena de consulta `access_token={_AccessToken_}`, o como encabezado HTTP `Authorization: Bearer {_AccessToken_}`.

## Límites

- Máximo de trabajos de importación simultáneos: 2
- Máximo de trabajos de importación en cola (incluidos los trabajos de importación actuales): 10
- Tamaño máximo del archivo de importación: 10 MB

## Permisos

La importación masiva utiliza el mismo modelo de permisos que la API de REST de Marketo y no requiere ningún permiso especial adicional para utilizar, aunque se requieren permisos específicos para cada conjunto de extremos.

## Operaciones de registro

La importación masiva es una operación de registro de &quot;inserción o actualización&quot;. Si se encuentra un registro coincidente en la base de datos, se actualiza. De lo contrario, se crea un nuevo registro. La respuesta de importación masiva no indica si un registro determinado se actualizó o insertó.

## Creación de un trabajo

Las API de importación masiva de Marketo utilizan el concepto de trabajo para ejecutar la importación de datos. Veamos la creación de un trabajo de importación de posibles clientes simple con la variable [Importar posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/importLeadUsingPOST) punto final.  Tenga en cuenta que este extremo utiliza [multipart/form-data como content-type](https://www.w3.org/Protocols/rfc1341/7_2_Multipart.html). Esto puede resultar difícil de hacer correctamente, por lo que la práctica recomendada es utilizar una biblioteca de compatibilidad con HTTP en el idioma que elija.  Si solo se está mojando los pies, le sugerimos que utilice [rizar](https://curl.se/).

```
POST /bulk/v1/leads.json?format=csv
```

```
Content-Type: multipart/form-data; boundary=--------------------------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Length: 311
Host: <munchkinId>.mktorest.com
```

```
------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Disposition: form-data; name="file"; filename="leads.csv"
Content-Type: text/csv

firstName,lastName,email
Able,Baker,ablebaker@marketo.com
Charlie,Dog,charliedog@marketo.com
Easy,Fox,easyfox@marketo.com
------WebKitFormBoundaryBQACkJZyaiIAXogC--
```

Esta solicitud creará un trabajo que importará los valores contenidos en el archivo CSV denominado &quot;leads.csv&quot; con los encabezados de columna &quot;FirstName&quot;, &quot;LastName&quot;, &quot;Email&quot;, &quot;Company&quot;.

```json
{
    "requestId": "d01f#15d672f8560",
    "result": [
        {
            "batchId": 3404,
            "importId": "3404",
            "status": "Queued"
        }
    ],
    "success": true
}
```

Cuando enviamos el trabajo, se devuelve un batchId, que podemos utilizar para comprobar su estado.

### Parámetros comunes

Cada extremo de creación de trabajo comparte algunos parámetros comunes para configurar el formato de archivo, los nombres de campo y el filtro de un trabajo de extracción masiva.  Cada subtipo del trabajo de extracción puede tener parámetros adicionales:

| Parámetro | Tipo de datos | Notas |
|---|---|---|
| formato | Cadena | Determina el formato de archivo de los datos importados con opciones para valores separados por comas, valores separados por tabulaciones y valores separados por punto y coma. Acepta uno de: CSV, SSV, TSV. El formato predeterminado es CSV. |
| archivo | Cadena | Los datos se especifican mediante datos de formulario de varias partes en el archivo. |


## Estado del trabajo de sondeo

La determinación del estado del trabajo es sencilla mediante [Obtener estado de cliente potencial de importación](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/getImportLeadStatusUsingGET) punto final.

```
GET /bulk/v1/leads/batch/{batchId}.json
```

```json
{
    "requestId": "1f63#15d6738fd15",
    "result": [
        {
            "batchId": 3404,
            "importId": "3404",
            "status": "Complete",
            "numOfLeadsProcessed": 3,
            "numOfRowsFailed": 0,
            "numOfRowsWithWarning": 0,
            "message": "Import succeeded, 3 records imported (3 members)"
        }
    ],
    "success": true
}
```

El interior `status` indicará el progreso del trabajo y puede ser uno de los siguientes valores: En cola, Importando, Completado, Error. En este caso, nuestro trabajo ha finalizado, así que podemos dejar de votar.

## Errores de

Los errores se indican mediante la variable `numOfRowsFailed` en Obtener respuesta de estado de cliente potencial de importación. If `numOfRowsFailed` es mayor que cero, ese valor indica el número de errores que se produjeron.

Para recuperar los registros y las causas de las filas fallidas, debe recuperar el archivo de error mediante el [Obtener errores de importación de posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/getImportLeadFailuresUsingGET) punto final.

```
GET /bulk/v1/leads/batch/{batchId}/failures.json
```

El archivo indica qué filas fallaron, junto con un mensaje que indica por qué falló el registro.
