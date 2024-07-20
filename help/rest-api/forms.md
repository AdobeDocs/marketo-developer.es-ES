---
title: Formularios
feature: REST API, Forms
description: Cree y administre formularios a través de la API.
exl-id: 2e5dfa70-3163-4ab4-b269-3112417714c3
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '1598'
ht-degree: 2%

---

# Formularios

[Referencia de extremo de Forms](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms)

[Referencia de extremo de campos de formulario](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields)

Los formularios Marketo tienen un conjunto complejo de extremos que permiten un control total de la administración de formularios desde sistemas remotos. La estructura de los formularios puede ser compleja, ya que hay muchos tipos diferentes de objetos que deben administrarse como parte de un formulario: Forms, campos, conjuntos de campos, reglas de visibilidad y reglas de página de seguimiento.

## Consulta

Forms admite los métodos estándar de recuperación de recursos [por id.](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByIdUsingGET), [por nombre](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET) y [por exploración](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/browseForms2UsingGET). Cada respuesta de formulario contiene todas sus propiedades excepto su lista de campos.

### Por identificador

[Obtener formulario por id.](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByIdUsingGET) toma un formulario `id` como parámetro de ruta de acceso y devuelve un registro de formulario.

```
GET /rest/asset/v1/form/{id}.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "948f#154e3bad8e3",
    "result": [
        {
            "id": 736,
            "name": "newForm",
            "description": "test",
            "createdAt": "2016-05-24T17:05:54Z+0000",
            "updatedAt": "2016-05-24T17:05:54Z+0000",
            "url": "https://app-devlocal1.marketo.com/#FO736B2",
            "status": "draft",
            "theme": "simple",
            "language": "French",
            "locale": "fr_FR",
            "progressiveProfiling": false,
            "labelPosition": "left",
            "fontFamily": "Helvetica",
            "fontSize": "13px",
            "folder": {
                "type": "Folder",
                "value": 293,
                "folderName": "yyLNLHzgOM"
            },
            "knownVisitor": {
                "type": "form",
                "template": null
            },
            "thankYouList": [
                {
                    "followupType": "none",
                    "followupValue": null,
                    "default": true
                }
            ],
            "buttonLocation": 120,
            "buttonLabel": "Envoyer",
            "waitingLabel": "Veuillez patienter"
        }
    ]
}
```

### Por nombre

[Obtener formulario por nombre](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET) toma un formulario `name` como parámetro de ruta de acceso y devuelve un registro de formulario.

```
GET /rest/asset/v1/form/byName.json?name=newForm
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "948f#154e3bad8e3",
    "result": [
        {
            "id": 736,
            "name": "newForm",
            "description": "test",
            "createdAt": "2016-05-24T17:05:54Z+0000",
            "updatedAt": "2016-05-24T17:05:54Z+0000",
            "url": "https://app-devlocal1.marketo.com/#FO736B2",
            "status": "draft",
            "theme": "simple",
            "language": "French",
            "locale": "fr_FR",
            "progressiveProfiling": false,
            "labelPosition": "left",
            "fontFamily": "Helvetica",
            "fontSize": "13px",
            "folder": {
                "type": "Folder",
                "value": 293,
                "folderName": "yyLNLHzgOM"
            },
            "knownVisitor": {
                "type": "form",
                "template": null
            },
            "thankYouList": [
                {
                    "followupType": "none",
                    "followupValue": null,
                    "default": true
                }
            ],
            "buttonLocation": 120,
            "buttonLabel": "Envoyer",
            "waitingLabel": "Veuillez patienter"
        }
    ]
}
```

### Examinar

[Obtener formularios de Forms](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/browseForms2UsingGET) funciona como otros extremos de exploración de Asset API y permite el filtrado opcional en `status`, `maxReturn` y `offset`. El estado puede ser: aprobado, aprobado con borrador o borrador.

```
GET /rest/asset/v1/forms.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "645d#154e3d499ac",
    "result": [
        {
            "id": 227,
            "name": "aKAUVDfbsX",
            "description": "",
            "createdAt": "2016-05-18T20:36:20Z+0000",
            "updatedAt": "2016-05-18T20:36:20Z+0000",
            "url": "https://app-devlocal1.marketo.com/#FO227B2",
            "status": "draft",
            "theme": "simple",
            "language": "English",
            "locale": "en_US",
            "progressiveProfiling": false,
            "labelPosition": "left",
            "fontFamily": "Helvetica",
            "fontSize": "13px",
            "folder": {
                "type": "Folder",
                "value": 293,
                "folderName": "yyLNLHzgOM"
            },
            "knownVisitor": {
                "type": "form",
                "template": null
            },
            "thankYouList": [
                {
                    "followupType": "none",
                    "followupValue": null,
                    "default": true
                }
            ],
            "buttonLocation": 120,
            "buttonLabel": "Submit",
            "waitingLabel": "Please Wait"
        },
        {
            "id": 695,
            "name": "AoMXgfFbma",
            "description": "",
            "createdAt": "2016-05-19T18:50:40Z+0000",
            "updatedAt": "2016-05-19T18:50:40Z+0000",
            "url": "https://app-devlocal1.marketo.com/#FO695B2",
            "status": "draft",
            "theme": "simple",
            "language": "English",
            "locale": "en_US",
            "progressiveProfiling": true,
            "labelPosition": "left",
            "fontFamily": "Helvetica",
            "fontSize": "13px",
            "folder": {
                "type": "Folder",
                "value": 565,
                "folderName": "WfUvYmlcyT"
            },
            "knownVisitor": {
                "type": "form",
                "template": null
            },
            "thankYouList": [
                {
                    "followupType": "none",
                    "followupValue": null,
                    "default": true
                }
            ],
            "buttonLocation": 120,
            "buttonLabel": "Submit",
            "waitingLabel": "Please Wait"
        }
    ]
}
```

### Lista de campos

La recuperación de la lista de campos de un formulario se realiza por formulario.

```
GET /rest/asset/v1/form/{id}/fields.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "2165#154eee00d01",
    "result": [
        {
            "id": "FirstName",
            "label": "First Name:",
            "dataType": "text",
            "validationMessage": "This field is required.",
            "rowNumber": 0,
            "columnNumber": 0,
            "maxLength": 255,
            "required": false,
            "formPrefill": true,
            "visibilityRules": {
                "ruleType": "alwaysShow"
            }
        },
        {
            "id": "LastName",
            "label": "Last Name:",
            "dataType": "text",
            "validationMessage": "This field is required.",
            "rowNumber": 1,
            "columnNumber": 0,
            "maxLength": 255,
            "required": false,
            "formPrefill": true,
            "visibilityRules": {
                "ruleType": "alwaysShow"
            }
        },
        {
            "id": "Email",
            "label": "Email Address:",
            "dataType": "email",
            "validationMessage": "Must be valid email. <span class='mktoErrorDetail'>example@yourdomain.com</span>",
            "rowNumber": 2,
            "columnNumber": 0,
            "required": false,
            "formPrefill": true,
            "visibilityRules": {
                "ruleType": "alwaysShow"
            }
        },
        {
            "id": "Profiling",
            "dataType": "profiling",
            "rowNumber": 3,
            "columnNumber": 0
        }
    ]
}
```

Al editar campos, o su comportamiento dentro de un formulario, la lista de campos siempre debe recuperarse antes de intentar realizar ediciones. Esto garantiza que se proporcione el ID de campo adecuado al actualizar o eliminar.

### Tipos de campo

| Tipo de IU | Nombre de API |
|--------------|-----------------|
| Casillas de verificación | casilla de verificación |
| Botón de opción | radio |
| Área de texto | área de texto |
| Lista de selección | lista desplegable |
| Cadena | cadena |
| Correo electrónico | correo electrónico |
| Fecha | fecha |
| Número | número |
| Doble | doble |
| Teléfono | teléfono |
| URL | url |
| Moneda | currency |
| Casilla de verificación | single_checkbox |
| Control deslizante | intervalo |


### Dependencias

El extremo [Get Form utilizado por](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getFormUsedByUsingGET) toma un formulario `id` como parámetro de ruta y devuelve la lista de recursos que dependen del formulario. Los siguientes tipos de recursos pueden utilizar Forms: Páginas de destino, Listas inteligentes, Campañas inteligentes, Informes, Programas de correo electrónico.

```
GET /rest/asset/v1/form/{id}/usedBy.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "fdf4#17285b25038",
    "warnings": [],
    "result": [
        {
            "id": 1038,
            "name": "LP Redirect Rules Program.LP Test 01",
            "type": "Landing Page",
            "status": "approved",
            "updatedAt": "2020-02-23T01:31:21Z+0000"
        }
    ]
}
```

## Crear y actualizar

Al [crear un formulario](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/createLpFormsUsingPOST), solo hay dos campos obligatorios: la carpeta principal del formulario, el nombre del formulario. Todos los demás parámetros son opcionales con el valor predeterminado. Cuando se crea el formulario, incluye tres campos predeterminados: Nombre, Apellidos, Correo electrónico.

```
POST /rest/asset/v1/forms.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=newForm&description=test&folder={"type": "Folder","id": 293}&language=French
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "948f#154e3bad8e3",
    "result": [
        {
            "id": 736,
            "name": "newForm",
            "description": "test",
            "createdAt": "2016-05-24T17:05:54Z+0000",
            "updatedAt": "2016-05-24T17:05:54Z+0000",
            "url": "https://app-devlocal1.marketo.com/#FO736B2",
            "status": "draft",
            "theme": "simple",
            "language": "French",
            "locale": "fr_FR",
            "progressiveProfiling": false,
            "labelPosition": "left",
            "fontFamily": "Helvetica",
            "fontSize": "13px",
            "folder": {
                "type": "Folder",
                "value": 293,
                "folderName": "yyLNLHzgOM"
            },
            "knownVisitor": {
                "type": "form",
                "template": null
            },
            "thankYouList": [
                {
                    "followupType": "none",
                    "followupValue": null,
                    "default": true
                }
            ],
            "buttonLocation": 120,
            "buttonLabel": "Envoyer",
            "waitingLabel": "Veuillez patienter"
        }
    ]
}
```

Los Forms están [actualizados](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/updateFormsUsingPOST) con una llamada similar a través de su id. Durante la creación o actualización, cualquiera de los parámetros de estilo base es accesible y editable, lo que permite modificar cómo se muestra el formulario al usuario final.

```
POST /rest/asset/v1/form/736.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=updated name&description=This is a test for updateapi&language=English&progressiveProfiling=true&locale=en_US
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "6307#154e3cf6efe",
    "result": [
        {
            "id": 736,
            "name": "updated name",
            "description": "This is a test for update api",
            "createdAt": "2016-05-24T17:05:54Z+0000",
            "updatedAt": "2016-05-24T17:28:23Z+0000",
            "status": "draft",
            "theme": "simple",
            "language": "English",
            "locale": "en_US",
            "progressiveProfiling": true,
            "labelPosition": "left",
            "fontFamily": "Helvetica",
            "fontSize": "13px",
            "folder": {
                "type": "Folder",
                "value": 293,
                "folderName": "yyLNLHzgOM"
            },
            "knownVisitor": {
                "type": "form",
                "template": null
            },
            "thankYouList": [
                {
                    "followupType": "none",
                    "followupValue": null,
                    "default": true
                }
            ],
            "buttonLocation": 120,
            "buttonLabel": "Submit",
            "waitingLabel": "Please Wait"
        }
    ]
}
```

Los comportamientos conocidos de visitante y página de agradecimiento no se pueden modificar mediante las llamadas de creación o actualización de formulario, y se debe acceder a ellos a través de sus respectivos extremos.

## Metadatos de campo

Para añadir o editar correctamente campos pertenecientes a un formulario, debe recuperar la lista de campos válidos para la instancia de destino. Las interacciones de campo siempre se realizan en función de la propiedad id del campo, que se muestra para cada elemento en el resultado.

Para los campos de posible cliente, esto se realiza mediante el extremo [Obtener campos de formulario disponibles](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/getAllFieldsUsingGET) e incluye el tipo de datos y los metadatos predeterminados para el campo cuando se agrega a un formulario.

```
GET /rest/asset/v1/form/fields.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "176ca#167a9808f4c",
    "warnings": [],
    "result": [
        {
            "id": "AnnualRevenue",
            "isRequired": false,
            "dataType": "currency"
        },
        {
            "id": "City",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "Company",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "Country",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "Description",
            "isRequired": false,
            "dataType": "textarea",
            "maxLength": 32000,
            "visibleRows": 2
        },
        {
            "id": "Email",
            "isRequired": false,
            "dataType": "email"
        },
        {
            "id": "Fax",
            "isRequired": false,
            "dataType": "phone"
        },
        {
            "id": "FirstName",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "Industry",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "LastName",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "LeadSource",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "MobilePhone",
            "isRequired": false,
            "dataType": "phone"
        },
        {
            "id": "NumberOfEmployees",
            "isRequired": false,
            "dataType": "int"
        },
        {
            "id": "Phone",
            "isRequired": false,
            "dataType": "phone"
        },
        {
            "id": "PostalCode",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "Rating",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "Salutation",
            "isRequired": false,
            "dataType": "picklist",
            "picklistValues": "Mr.,Ms.,Mrs.,Dr.,Prof."
        },
        {
            "id": "State",
            "isRequired": false,
            "dataType": "picklist",
            "picklistValues": "AK::AK,AL::AL,AR::AR,AZ::AZ,CA::CA,CO::CO,CT::CT,DE::DE,FL::FL,GA::GA,HI::HI,IA::IA,ID::ID,IL::IL,IN::IN,KS::KS,KY::KY,LA::LA,MA::MA,MD::MD,ME::ME,MI::MI,MN::MN,MO::MO,MS::MS,MT::MT,NC::NC,ND::ND,NE::NE,NH::NH,NJ::NJ,NM::NM,NV::NV,NY::NY,OH::OH,OK::OK,OR::OR,PA::PA,RI::RI,SC::SC,SD::SD,TN::TN,TX::TX,UT::UT,VA::VA,VT::VT,WA::WA,WI::WI,WV::WV,WY::WY"
        },
        {
            "id": "Street",
            "isRequired": false,
            "dataType": "textarea",
            "maxLength": 2000,
            "visibleRows": 2
        },
        {
            "id": "Title",
            "isRequired": false,
            "dataType": "picklist"
        }
    ]
}
```

Para los campos personalizados de miembro de programa, llame a [Obtener formulario disponible Campos de miembro de programa](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/getAllProgramMemberFieldsUsingGET)  punto final para recuperar los tipos de datos de campo personalizado de miembro de programa y los metadatos predeterminados. Para utilizar estos campos en un formulario, el formulario debe residir debajo de un programa (no en Design Studio). Las páginas de destino que contienen formularios que utilizan estos campos también deben residir debajo de un programa (no pueden residir en Design Studio o clonarse en Design Studio).

```
GET /rest/asset/v1/form/programMemberFields.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "109c6#16fa0b9c51a",
    "warnings": [],
    "result": [
        {
            "id": "pMCFCustomField01",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "pMCFCustomField02",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "myPMCF",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        }
    ]
}
```

### Editar campo

Cada formulario contiene una lista editable de campos, que se muestra al usuario final cuando se carga. Cada campo se agrega, actualiza o elimina de la lista de campos de uno en uno a través de sus respectivos extremos.

[Para agregar un campo](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/addFieldToAFormUsingPOST) solo se necesita el id del formulario principal y el fieldId del campo. El resto de los campos estarán vacíos o tendrán valores predeterminados según su tipo de datos y los metadatos de campo. Los datos se pasan como POST x-www-form-urlencoded, no como JSON.

```
POST /rest/asset/v1/form/{id}/fields.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
fieldId=NumberOfEmployees&maxLength=125&defaultValue=this is default&required=true&fieldWidth=100&validationMessage=hey, you there?&label=employee count&hintText=Hint me&minValue=10
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "1826e#154f41b214c",
    "result": [
        {
            "id": "NumberOfEmployees",
            "label": "employee count",
            "fieldWidth": 100,
            "dataType": "number",
            "defaultValue": "this is default",
            "validationMessage": "hey, you there?",
            "rowNumber": 5,
            "columnNumber": 0,
            "required": true,
            "formPrefill": true,
            "fieldMetaData": {
                "minValue": 10,
                "maxValue": null
            },
            "visibilityRules": {
                "ruleType": "alwaysShow"
            },
            "hintText": "Hint me"
        }
    ]
}
```

Las actualizaciones pueden editar todos los campos del mismo modo que agregar un campo y, del mismo modo, requieren el ID de formulario y el fieldId, excepto que fieldId es un parámetro de ruta de acceso y no un parámetro de consulta al realizar actualizaciones.

```
POST /rest/asset/v1/form/{id}/field/LastName.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
label=enter the last name here
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "5634#15508303abb",
    "result": [
        {
            "id": "LastName",
            "label": "enter the last name here",
            "dataType": "text",
            "validationMessage": "This field is required.",
            "rowNumber": 0,
            "columnNumber": 0,
            "maxLength": 255,
            "required": false,
            "formPrefill": true,
            "visibilityRules": {
                "ruleType": "alwaysShow"
            }
        }
    ]
}
```

En el ejemplo anterior, se actualiza el campo LastName, que es una cadena simple. Algunos campos de formulario son más complejos. Por ejemplo, el campo Salutation es un tipo de campo de &quot;selección&quot; que contiene una lista de elementos y un valor predeterminado. Si agrega o actualiza un campo de tipo de selección, a menos que establezca una de las opciones para que tenga un valor `isDefault` de true, la primera opción no tendrá ningún valor y estará etiquetada como &quot;Seleccionar...&quot;

![Saludo](assets/form-field-salutation.png)

Para actualizar los elementos de la lista, el formato del parámetro &quot;values&quot; es el siguiente:

```
POST /rest/asset/v1/form/{id}/field/Salutation.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
values=[{"label":"Select...","value":"","isDefault":true,"selected":true}, {"label":"MR","value":"MR"}, {"label":"MS","value":"MS"}, {"label":"MRS","value":"MRS"}, {"label":"DR","value":"DR"}, {"label":"PROF","value":"PROF"}]
```

```json
{
  "success": true,
  "warnings": [ ],
  "errors": [ ],
  "requestId": "71fd#1588d9d1b0c",
  "result": [
    {
      "id": "Salutation",
      "label": "Salutation:",
      "dataType": "select",
      "validationMessage": "This field is required.",
      "rowNumber": 3,
      "columnNumber": 0,
      "required": false,
      "formPrefill": true,
      "fieldMetaData": {
        "multiSelect": false,
        "values": [
          {
            "label": "Select...",
            "value": "",
            "isDefault": true,
            "selected": true
          },
          {
            "label": "MR",
            "value": "MR"
          },
          {
            "label": "MS",
            "value": "MS"
          },
          {
            "label": "MRS",
            "value": "MRS"
          },
          {
            "label": "DR",
            "value": "DR"
          },
          {
            "label": "PROF",
            "value": "PROF"
          }
        ],
        "visibleLines": 1
      },
      "visibilityRules": {
        "ruleType": "alwaysShow"
      }
    }
  ]
}
```

 

Para determinar cómo dar formato a un campo de formulario complejo, consulte la respuesta de Agregar campo a formulario.

### Campo de reorganización

Los campos de un formulario se deben reorganizar todos como una sola unidad a través del punto de conexión [Cambiar posiciones de los campos de formulario](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/updateFieldPositionsUsingPOST). El extremo requiere un parámetro denominado `positions`, que es una matriz JSON de objetos con tres miembros:

- columnNumber
- rowNumber
- fieldName (hace referencia al ID del campo)

Los campos de un formulario se organizan en una interfaz similar a una tabla, con hasta tres columnas y hasta diez filas. Tanto la fila como la columna están indizadas desde 0, por lo que la primera fila y la primera columna se indican pasando un 0. Todos los campos deben ocupar una posición única

Si el campo de destino también es un conjunto de campos, su registro dentro de la matriz de posiciones también debe contener un parámetro denominado fieldList, una matriz de objetos que contiene los mismos miembros columnNumber, rowNumber y fieldName. El conjunto de campos en sí se trata como un único campo para su posición en la lista principal, mientras que sus subcampos se colocan según las posiciones dadas en el parámetro fieldList.

```
POST /rest/asset/v1/form/{id}/reArrange.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
positions=[{"columnNumber":0,"rowNumber":0,"fieldName":"FirstName"},{"columnNumber":0,"rowNumber":1,"fieldName":"LastName"}, {"columnNumber":0,"rowNumber":2, "fieldName":"Email"}]
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "bb18#15508ef9c04",
    "result": [
        {
            "id": 764
        }
    ]
}
```

### Texto enriquecido

Los campos de texto enriquecido se agregan a través de un [extremo independiente](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/addRichTextFieldUsingPOST) de los campos de posibles clientes. El contenido del campo se pasa como multipart/form-data. Debe estructurarse como contenido HTML que no contenga secuencias de comandos, metaetiquetas o etiquetas de vínculo.

```
POST /rest/asset/v1/form/{id}/richText.json
```

```
Content-Type: multipart/form-data; boundary=---------------------------9051914041544843365972754266
-----------------------------9051914041544843365972754266
Content-Disposition: form-data; name="text"
Content-Type: text/html
<div>Fancy Rich Text Component</div>
-----------------------------9051914041544843365972754266--
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "82c8#154f423bf5c",
    "result": [
        {
            "id": "SHRtbFRleHRfMjAxNi0wNS0yN1QxNDozNDoyNC4xMTVa",
            "labelWidth": 260,
            "dataType": "htmltext",
            "rowNumber": 8,
            "columnNumber": 0,
            "visibilityRules": {
                "ruleType": "alwaysShow"
            },
            "text": "<div>Fancy Rich Text Component</div>"
        }
    ]
}
```

### Conjunto de campos

Los formularios Marketo Forms presentan un componente opcional denominado conjuntos de campos. Los conjuntos de campos son grupos de campos que se tratan como un único campo dentro de la lista de campos de nivel superior con fines de movimiento y tratamiento mediante reglas de visibilidad. Por ejemplo, si hay un campo para Requisitos de cumplimiento y un cliente selecciona sí, puede mostrar un conjunto de campos que contenga campos para Requisitos de cumplimiento de HIPAA y PCI.

Los campos de los conjuntos de campos son únicos para el formulario en su conjunto, por lo que es posible que los campos duplicados no estén tanto en la lista de campos principales del formulario como en un conjunto de campos secundarios. Los conjuntos de campos se agregan a través del extremo [Add Fieldset to Form](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/addFieldSetUsingPOST) y aparecerán en el resultado de [Obtener campos para el formulario](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/getFormFieldByFormVidUsingGET). Los campos se agregan a un conjunto de campos moviéndolos a fieldList del conjunto de campos mediante [Actualizar posiciones de campo](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/updateFieldPositionsUsingPOST). Para estos extremos, los datos se pasan como POST x-www-form-urlencoded, no como JSON.

## Regla de visibilidad

Cada campo puede tener un conjunto de reglas de visibilidad que determinan si un visitante puede ver el campo según los valores que haya introducido en el formulario. Las reglas realizan una comparación entre el valor de un subjectField presente en el formulario y una lista de valores determinados en la regla. Cada campo puede tener un tipo de regla de visibilidad, mostrar, ocultar o mostrar siempre y, a continuación, una lista de reglas para evaluar. Las reglas se evalúan de arriba a abajo, y la primera regla que se evalúa como verdadera es la que se aplicará.

Cambiar las reglas de visibilidad es una actualización destructiva.

```
POST /rest/asset/v1/form/{id}/field/Email/visibility.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
visibilityRule={"ruleType":"show", "rules":[{"subjectField": "LastName", "operator": "isNotEmpty", "values": [], "altLabel": "Email:"}]}
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "ab4a#15509030601",
    "result": [
        {
            "formFieldId": "Email",
            "ruleType": "show",
            "rules": [
                {
                    "subjectField": "LastName",
                    "operator": "isNotEmpty",
                    "values": [],
                    "altLabel": "Email:"
                }
            ]
        }
    ]
}
```

Para obtener la lista completa de operadores disponibles, consulte la página de referencia de extremo para [Agregar reglas de visibilidad de campo de formulario](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/addFormFieldVisibilityRuleUsingPOST).

## Seguimiento

Los formularios Marketo pueden tener un comportamiento de página de seguimiento dinámico, donde las reglas para redireccionar a una página determinada o permanecer en la página actual pueden aplicarse en función del contenido de los campos designados al enviarlos. Las reglas pueden llamarse reglas de la página de agradecimiento o reglas de la página de seguimiento indistintamente. Estas reglas se representan como una matriz JSON con los miembros `followupType`, `followupValue`, `operator`, `subjectField`, `values` y `default`. `default` es un valor booleano para el cual solamente un registro de la matriz puede ser verdadero. Cuando un visitante no cumple los requisitos para otras reglas, se utiliza la regla designada como predeterminada. `followupType` puede ser lp o url, donde lp indica un identificador de página de aterrizaje de Marketo para `followupValue` y url indicará una dirección URL a otra página. El operador se utiliza para comparar el valor del campo de asunto con la lista de valores proporcionados.

## Botón enviar

El estilo del botón de envío del formulario se administra con el extremo [Actualizar botón de envío](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/updateFormSubmitButtonUsingPOST). ButtonPosition, buttonStyle, label y waitLabel (la etiqueta que se muestra mientras el envío está pendiente) se puede modificar.

Esta es una actualización destructiva.

## Aprobación

Al igual que la mayoría de los demás recursos, los formularios siguen un modelo aprobado por borrador, en el que puede haber una versión en borrador o una versión aprobada. Siempre que se aplican actualizaciones a un formulario, siempre se aplican primero a la versión de borrador y solo se verán activas cuando se haya aprobado el formulario. La aprobación de un formulario toma la versión en borrador actual y reemplaza la versión aprobada, si la hay, por el borrador. Si el formulario debe eliminarse de la versión activa, primero debe desaprobarse, lo que elimina los borradores actuales y devolver la versión aprobada a un estado de solo borrador. Forms siempre debe desaprobarse antes de intentar la eliminación.

## Generación progresiva de perfiles

Cuando se habilita la creación de perfiles progresiva en un formulario, se incluye un conjunto de campos denominado &quot;Creación de perfiles&quot; en su lista de campos. Para agregar o quitar campos de la lista de creación de perfiles progresiva, debe utilizar el punto final Actualizar posiciones de campo. Este extremo realiza actualizaciones destructivas, por lo que todos los campos del formulario deben incluirse en cada solicitud. El siguiente ejemplo añade el campo &quot;Phone&quot; a la lista de creación de perfiles progresiva.

```
POST /rest/asset/v1/form/{id}/reArrange.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
positions=[{"columnNumber":0,"rowNumber":0,"fieldName":"Email"},{"columnNumber":0,"rowNumber":1,"fieldName":"LastName"},{"columnNumber":0,"rowNumber":2,"fieldName":"Company"},{"columnNumber":0,"rowNumber":3,"fieldName":"Website"},{"columnNumber":0,"rowNumber":4,"fieldName":"Profiling","fieldList":[{"columnNumber":0,"rowNumber":0,"fieldName":"Phone"}]}]
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "3d6a#164190dbdf2",
    "result": [
        {
            "id": 1031
        }
    ]
}
```
