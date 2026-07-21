---
title: Importación masiva
feature: REST API
description: Importación masiva de Marketo para cargar posibles clientes, objetos personalizados y miembros de programas a través de cargas de varias partes, la creación de trabajos asincrónicos, el estado de sondeo y la administración de errores.
exl-id: f7922fd2-8408-4d04-8955-0f8f58914d24
TQID: https://experienceleague.adobe.com/lr9dyX-fY-oJ2LM5P0zE1m24HtFYKQYYbxMkVe--PkE
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 538
ht-degree: 3%

---

# Importación masiva

La importación masiva proporciona interfaces para insertar grandes conjuntos de datos relacionados con personas y personas. Se pueden importar tres tipos de objetos:

- Posibles clientes (personas)
- Objetos personalizados
- Miembros del programa

Para realizar una importación masiva, cree un trabajo que lea un archivo cargado. El trabajo se ejecuta de forma asíncrona, por lo que sondee para recuperar el estado de importación.

Cargue archivos mediante HTTP `multipart/form-data` según RFC 2399.

A diferencia de otros extremos, los extremos de API en lote no tienen el prefijo `/rest`.

## Autenticación

Las API de importación masiva utilizan el mismo método de autenticación OAuth 2.0 que otras API de REST de Marketo. Envíe un token de acceso válido en el encabezado HTTP `Authorization: Bearer {_AccessToken_}`.

>[!IMPORTANT]
>
>El 30 de junio de 2025 se eliminará la compatibilidad con la autenticación mediante el parámetro de consulta **access_token**. Si el proyecto usa un parámetro de consulta para pasar el token de acceso, debe actualizarse para usar el encabezado **Autorización** lo antes posible. El nuevo desarrollo debe usar el encabezado **Authorization** exclusivamente.

## Límites

- Máximo de trabajos de importación simultáneos: 2
- Máximo de trabajos de importación en cola, incluidos los trabajos que se están importando actualmente: 10
- Tamaño máximo del archivo de importación: 10 MB

## Permisos

La importación masiva utiliza el mismo modelo de permisos que la API de REST de Marketo. No requiere permisos adicionales, pero cada conjunto de extremos requiere permisos específicos.

## Operaciones de registro

La importación masiva es una operación de registro de &quot;inserción o actualización&quot;. Si la base de datos contiene un registro coincidente, la operación lo actualiza. De lo contrario, la operación crea un registro.

La respuesta de importación masiva no indica si se actualizó o insertó un registro individual.

## Creación de un trabajo

Cree un trabajo de importación de posibles clientes llamando al extremo [Importar posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Leads/operation/importLeadUsingPOST). Este extremo usa [multipart/form-data como tipo de contenido](https://www.w3.org/Protocols/rfc1341/7_2_Multipart.html).

Utilice una biblioteca de soporte HTTP para el idioma preferido para construir la solicitud de varias partes. También puedes usar [curl](https://curl.se/) para comenzar.

```http
POST /bulk/v1/leads.json?format=csv
```

```text
Content-Type: multipart/form-data; boundary=--------------------------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Length: 311
Host: <munchkinId>.mktorest.com
```

```text
------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Disposition: form-data; name="file"; filename="leads.csv"
Content-Type: text/csv

firstName,lastName,email
Able,Baker,ablebaker@marketo.com
Charlie,Dog,charliedog@marketo.com
Easy,Fox,easyfox@marketo.com
------WebKitFormBoundaryBQACkJZyaiIAXogC--
```

Esta solicitud crea un trabajo que importa valores del archivo CSV denominado `leads.csv`.

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

La respuesta devuelve un `batchId`. Utilice este valor para comprobar el estado del trabajo.

### Parámetros comunes

Cada extremo de creación de trabajo comparte parámetros para configurar el archivo de importación. Un subtipo de importación también puede admitir parámetros adicionales.

| Parámetro | Tipo de datos | Notas |
| --- | --- | --- |
| formato | Cadena | Determina el formato de archivo de los datos importados con opciones para valores separados por comas, valores separados por tabulaciones y valores separados por punto y coma. Acepta uno de: CSV, SSV, TSV. El formato predeterminado es CSV. |
| archivo | Cadena | Los datos se especifican mediante datos de formulario de varias partes en el archivo. |

## Estado del trabajo de sondeo

Pase `batchId` al extremo [Obtener estado de cliente potencial de importación](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Leads/operation/getImportLeadStatusUsingGET) para recuperar el estado del trabajo.

```http
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

El miembro `status` indica el progreso del trabajo. Su valor puede ser `Queued`, `Importing`, `Complete` o `Failed`.

En este ejemplo, el trabajo se ha completado, por lo que el sondeo puede detenerse.

## Errores de

El atributo `numOfRowsFailed` de la respuesta Obtener estado del posible cliente de importación indica el número de filas con errores. Un valor mayor que cero significa que se produjeron errores.

Para recuperar los registros con errores y sus causas, use el extremo [Obtener errores de importación de posibles clientes](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Leads/operation/getImportLeadFailuresUsingGET).

```http
GET /bulk/v1/leads/batch/{batchId}/failures.json
```

El archivo de error identifica cada fila fallida y explica por qué falló el registro.
