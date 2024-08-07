---
title: Descargar definiciones de Swagger
feature: REST API, Programs
description: Descargue los archivos de definición de Swagger para el consumo local.
source-git-commit: 85062243d57a3fc6d15251163e926495858edf2a
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 2%

---

# Descargar definiciones de Swagger

Las definiciones de [Swagger](https://swagger.io/) o [OpenAPI](https://www.openapis.org/) son archivos de datos que describen todos los métodos y parámetros de una API REST. Puede descargar y consumir estos archivos de datos localmente como referencia de API personal.

Para utilizar las definiciones de Marketo Engage Swagger/OpenAPI, el campo `host` de cada documento debe actualizarse para que coincida con el nombre de host de la API de REST de su instancia de Marketo Engage.

En primer lugar, descargue la definición de OpenAPI que desee utilizar:

* [ recurso](assets/swagger-asset.json)
* [Posible cliente](assets/swagger-mapi.json)
* [Identidad      ](assets/swagger-identity.json)
* [Gestión de usuarios](assets/swagger-user.json)

A continuación, obtenga su nombre de host de la API de REST del administrador de Marketo. Vaya al menú _Administración_-> _Servicios web_ en el Marketo Engage y copie el nombre de host del campo Punto de conexión. `hostname` es la cadena entre el protocolo `https://` y `/rest`, que tiene el aspecto `AAA-999-AAA.mktorest.com`

Abra el archivo OpenAPI en un editor de texto, busque el campo `host` en el JSON y cambie su valor a su nombre de host de API REST: `"host":"localhost:8080"` a `"host":"AAA-999-AAA.mktorest.com"`.

Una vez guardado, puede ejecutar el archivo de definición en su instancia de interfaz de usuario de Swagger o en cualquier otro software de OpenAPI.
