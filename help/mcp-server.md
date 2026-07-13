---
title: Servidor MCP de Marketo Engage
description: Aprenda a conectar un asistente de IA a Marketo mediante el servidor MCP de Marketo Engage. Configure Claude Desktop, Cursor, Claude Code o VS Code con sus credenciales de Marketo.
badgeBeta: label="Disponibilidad limitada" type="informative" tooltip="Actualmente, esta función está en versión beta limitada"
exl-id: ab446e56-6250-4af5-b03e-162991d09a5c
autotag-review: '2026-06-02T13:31:15.329Z'
TQID: 'https://experienceleague.adobe.com/PJJm7yv8HmbwMB2fsnfDCXs8zprDJK5Q5z2uiiCJRZI'
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: a7170d27-32ab-462b-a333-269abc654483id: b0bb9048-d951-48d8-8232-45cf248a7e27id: b13bd2ad-8e65-49e5-9691-2a0d31067b35id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45id: c2dbad80-0f5c-4d96-a798-2a65f93b8721id: dca84292-69e9-4116-a575-667d31fa060did: e2290edd-b061-4880-9d79-dee306cf5aa9id: e64968b2-4ee5-47f9-8cae-0588f184b9ebid: f82558ea-6af5-44eb-a424-5b3389abb0a3
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: bbbea26f-9621-49eb-9ab8-e06fb3bbce8c
source-git-commit: b28708e92f44082eb247d9053d6ebf2306739b38
workflow-type: tm+mt
source-wordcount: 2199
ht-degree: 1%

---

# Servidor MCP [!DNL Marketo Engage]

>[!AVAILABILITY]
>
> Esta función tiene disponibilidad limitada. Para solicitar acceso, rellene [este formulario](https://forms.cloud.microsoft/Pages/ResponsePage.aspx?id=Wht7-jR7h0OUrtLBeN7O4Y-uSf63sAxCmWyqMJg8eMFUMVZSVExSNDA3T0I4SEcwRDFSVTBGWU01Uy4u&origin=QRCode){target="_blank"}. Asegúrese de tener preparado el Munchkin ID de su suscripción.

El Protocolo de contexto de modelo (MCP) es un estándar abierto que permite a las herramientas de IA comunicarse con servicios externos. El servidor MCP [!DNL Marketo] actúa como un puente entre su asistente de IA y [!DNL Marketo]. Expone más de 100 operaciones en formularios, programas, campañas inteligentes, posibles clientes, correos electrónicos, fragmentos de código, listas y carpetas.

Cuando la herramienta de IA llama al servidor MCP, el servidor ejecuta la llamada de API de REST correspondiente en su nombre, utilizando las credenciales proporcionadas en cada solicitud. No es necesario instalar, implementar ni ejecutar ningún software del lado del servidor.

Para obtener más información sobre cómo se gestionan los datos con la IA de Marketo y el servidor MCP de Marketo Engage, consulte la página [Información de datos](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/marketo-ai/data-information).

>[!IMPORTANT]
>
>El Modelo de Protocolo de Contexto (MCP) es un estándar de código abierto emergente y puede presentar riesgos de seguridad o fiabilidad. Las integraciones del servidor de Adobe MCP y la documentación relacionada se proporcionan &quot;tal cual&quot;, sin garantías de ningún tipo.La conexión de clientes o servidores MCP a los productos de Adobe es una configuración elegida por el cliente, y los clientes son responsables de evaluar la seguridad y la idoneidad de cualquier integración MCP. Adobe no se responsabiliza de los problemas que se deriven de una configuración incorrecta, un uso incorrecto del MCP, vulnerabilidades en implementaciones de terceros o acciones no deseadas realizadas a través de flujos de trabajo habilitados para MCP.Para reducir el riesgo, Adobe recomienda probar las integraciones en un entorno de zona protegida antes de usarlas de forma productiva y revisar y validar cuidadosamente todas las acciones y respuestas iniciadas por MCP antes de confirmarlas o depender de ellas.

## Conceptos básicos de MCP

>Piense en MCP como un puerto USB-C para aplicaciones de IA. USB-C proporciona una forma estandarizada de conectar sus dispositivos a varios periféricos y accesorios, y MCP proporciona una forma estandarizada de conectar modelos de IA a fuentes de datos y herramientas. — [Protocolo de contexto de modelo](https://modelcontextprotocol.io/docs/getting-started/intro){target="_blank"}

MCP permite que una herramienta de IA se conecte a varios servicios externos al mismo tiempo. Por ejemplo, un asistente de IA podría:

* Conectarse a un procesador de textos para generar documentos con asistencia de IA
* Conéctese a herramientas de animación, como Blender, para crear visualizaciones
* Conectar con Adobe After Effects para la edición de vídeo

MCP es un protocolo de comunicación: un estándar abierto que cualquier aplicación puede implementar para exponer sus datos y acciones a las herramientas de IA.

## Qué hace y no hace [!DNL Marketo Engage] MCP

Comprender el ámbito de MCP ayuda a establecer expectativas antes de conectar la herramienta de IA.

**MCP no:**

* Proporcionar acceso a los datos y las capacidades de [!DNL Marketo] mediante las API de REST estándar
* Ejecutar llamadas de API en su nombre mediante las credenciales que proporciona con cada solicitud
* Admitir varios usuarios simultáneos, cada uno conectado con sus propias credenciales
* Controlar la actualización del token de OAuth automáticamente. No es necesario administrar la caducidad del token
* Opere dentro de entornos aislados del inquilino para que los datos nunca se intersecten con la sesión de otro usuario

**MCP no:**

* Utilice, aloje o ejecute cualquier modelo de IA o aprendizaje automático. Todo el procesamiento de IA se realiza en la herramienta IA, no en el MCP
* Capacite o aprenda de cualquier dato, incluidos los datos de sus clientes
* Generar predicciones, recomendaciones o decisiones. La toma de decisiones es responsabilidad de la herramienta de IA descendente o del usuario
* Almacenar o conservar credenciales, datos de solicitud o estados de sesión entre solicitudes
* Requerir que instale, implemente o administre cualquier software del lado del servidor

MCP puede transmitir datos, incluidos campos potencialmente confidenciales, según el uso de API, pero los datos B2B implican datos empresariales del cliente y no implican datos PII.

## Requisitos previos

* Una instancia de [!DNL Marketo] con acceso a la API de REST habilitado
* Acceso de administrador para crear credenciales de API en [!DNL Marketo] LaunchPoint
* Una de las siguientes herramientas de IA: Claude Desktop, Cursor, Codex, Claude Code (CLI) o VS Code con GitHub Copilot
* Acceso de red a la URL del servidor MCP: `https://marketo-mcp.adobe.io/mcp`

## Obtener credenciales de Marketo

Necesita los siguientes valores de su instancia de [!DNL Marketo]:

* **ID de cliente**
* **Secreto de cliente**
* **ID de cuenta de Munchkin**

Si ya los tiene, vaya a [Configurar la herramienta de IA](#configure-your-ai-tool).

### ID de cliente y secreto de cliente

1. Vaya a **[!UICONTROL Administración]** > **[!UICONTROL LaunchPoint]**.
1. Seleccione su servicio API. Si no tiene uno, seleccione **[!UICONTROL Nuevo]** > **[!UICONTROL Nuevo servicio]**, elija **[!UICONTROL Personalizado]** como tipo de servicio y asigne un usuario de API dedicado.
1. Seleccione **[!UICONTROL Ver detalles]** y copie los valores **[!UICONTROL ID de cliente]** y **[!UICONTROL Secreto de cliente]**.

### ID de cuenta de Munchkin

1. Vaya a **[!UICONTROL Administración]** > **[!UICONTROL Munchkin]**.
1. Copie la **[!UICONTROL cuenta de Munchkin]**. El formato es `XXX-XXX-XXX` y coincide con el prefijo de la dirección URL de la instancia.

## Configuración de la herramienta IA

Cada herramienta de IA tiene una configuración ligeramente diferente. Se proporcionan ejemplos de conexión para herramientas comunes.

* [Claude Desktop](#claude-desktop)
* [Cursor](#cursor)
* [CLI de código Claude](#claude-code)
* [Códice OpenAI](#codex)
* [VSCode con el copiloto de GitHub](#vscode)
* [Espigar](#glean)
* [Otras herramientas](#other-tools)

>[!TIP]
>
>Para conectarse a varias instancias de [!DNL Marketo], agregue entradas independientes en la configuración de MCP con nombres únicos: `marketo-prod` y `marketo-staging`, cada uno con las credenciales correspondientes.

### Claude Desktop {#claude-desktop}

Para conectarte a Claude Desktop, descarga [marketo-mcp-bridge.zip](assets/marketo-mcp-bridge.zip) y desempaquete el paquete. Coloque `marketo-mcp-bridge.mjs` en una ubicación conocida para que pueda hacer referencia al siguiente paso.

También necesitará esto:

* Node.js v18+
* npm

1. Abrir Claude Desktop
1. Vaya a **Configuración > Desarrollador > Editar configuración**
1. Agregar lo siguiente a `claude_desktop_config.json`:

```json
{
  "preferences": {
    ...
  },
  "mcpServers": {
    "marketo-mcp": {
      "command": "node",
      "args": ["/path/to/marketo-bridge/bridge.mjs"],
      "env": {
        "MARKETO_MCP_PROD_CLIENT_ID": "<your-client-id>",
        "MARKETO_MCP_PROD_CLIENT_SECRET": "<your-client-secret>",
        "MARKETO_MCP_PROD_MUNCHKIN_ID": "<your-munchkin-id>"
      }
    }
  }
}
```

1. Reiniciar Claude Desktop

### Cursor {#cursor}

Si la configuración de MCP de cursor ya contiene otros servidores, agregue la entrada `marketo` en `mcpServers`.El siguiente ejemplo muestra el bloque `mcpServers` completo en **[!UICONTROL Configuración]** > **[!UICONTROL MCP]** o `.cursor/mcp.json` en el directorio del proyecto:

>[!BEGINTABS]

>[!TAB Token de IMS]

```json
{
  "mcpServers": {
    "marketo": {
      "type": "http",
      "url": "https://marketo-mcp.adobe.io/mcp",
      "headers": {
        "Authorization": "Bearer YOUR-IMS-TOKEN",
        "x-gw-ims-org-id": "YOUR-IMS-ORG-ID"
      }
    }
  }
}
```

>[!TAB Credenciales de cliente de Marketo]

```json
{
  "mcpServers": {
    "marketo": {
      "type": "http",
      "url": "https://marketo-mcp.adobe.io/mcp",
      "headers": {
        "X-Marketo-Client-Id": "YOUR-CLIENT-ID",
        "X-Marketo-Client-Secret": "YOUR-CLIENT-SECRET",
        "X-Marketo-Munchkin-Id": "YOUR-MUNCHKIN-ID"
      }
    }
  }
}
```

>[!ENDTABS]

Reiniciar cursor.

### Código Claude (CLI) {#claude-code}

Ejecute el siguiente comando en el terminal, sustituyendo las credenciales:

>[!BEGINTABS]

>[!TAB Token de IMS]

```bash
claude mcp add --transport http marketo \
  https://marketo-mcp.adobe.io/mcp \
  --header "Authorization: Bearer YOUR-IMS-TOKEN" \
  --header "x-gw-ims-org-id: YOUR-IMS-ORG-ID"
```

>[!TAB Credenciales de cliente de Marketo]

```bash
claude mcp add --transport http marketo \
  https://marketo-mcp.adobe.io/mcp \
  --header "X-Marketo-Client-Id: YOUR-CLIENT-ID" \
  --header "X-Marketo-Client-Secret: YOUR-CLIENT-SECRET" \
  --header "X-Marketo-Munchkin-Id: YOUR-MUNCHKIN-ID"
```

>[!ENDTABS]

### Códice OpenAI {#codex}

1. Vaya a Configuración > Servidores MCP > Agregar servidor
1. Agregar la dirección URL del servidor: `https://marketo-mcp.adobe.io/mcp`
1. Añada los encabezados del método de autenticación:

>[!BEGINTABS]

>[!TAB Token de IMS]

* Autorización: &quot;Portador YOUR-IMS-TOKEN&quot;
* x-gw-ims-org-id: &quot;YOUR-IMS-ORG-ID&quot;

>[!TAB Credenciales de cliente de Marketo]

* X-Marketo-Client-Id: &quot;YOUR-CLIENT-ID&quot;
* X-Marketo-Client-Secret: &quot;YOUR-CLIENT-SECRET&quot;
* X-Marketo-Munchkin-Id: &quot;YOUR-MUNCHKIN-ID&quot;

>[!ENDTABS]

1. Haga clic en Guardar para completar el proceso.


### Código VS con el copiloto de GitHub {#vscode}

Presione **[!UICONTROL Ctrl+Mayús+P]** (o **[!UICONTROL Cmd+Mayús+P]** en macOS), escriba **[!UICONTROL MCP: Abrir configuración de usuario]** y presione Entrar. Se abre `mcp.json`. Agregar la entrada `marketo` dentro del objeto `servers`:

>[!BEGINTABS]

>[!TAB Token de IMS]

```json
{
  "servers": {
    "marketo": {
      "type": "http",
      "url": "https://marketo-mcp.adobe.io/mcp",
      "headers": {
        "Authorization": "Bearer YOUR-IMS-TOKEN",
        "x-gw-ims-org-id": "YOUR-IMS-ORG-ID"
      }
    }
  }
}
```

>[!TAB Credenciales de cliente de Marketo]

```json
{
  "servers": {
    "marketo": {
      "type": "http",
      "url": "https://marketo-mcp.adobe.io/mcp",
      "headers": {
        "X-Marketo-Client-Id": "YOUR-CLIENT-ID",
        "X-Marketo-Client-Secret": "YOUR-CLIENT-SECRET",
        "X-Marketo-Munchkin-Id": "YOUR-MUNCHKIN-ID"
      }
    }
  }
}
```

>[!ENDTABS]

>[!NOTE]
>
>Por motivos de seguridad, utilice la interpolación de variables de entorno en archivos de configuración en lugar de pegar las credenciales directamente. Puede hacer referencia a variables mediante sintaxis como `${MARKETO_CLIENT_SECRET}` y establecerlas en su entorno. Esto evita almacenar las credenciales en texto sin formato en archivos con control de versiones.

### Espigar {#glean}

Para conectar Glean al servidor MCP de Marketo Engage, el [equipo de soporte técnico de Glean](https://docs.glean.com/release-notes/releases/2026-04-22-april-release#admin-features) debe configurar los siguientes encabezados personalizados.

| Encabezado | Valor |
| ------ | ----- |
| `X-Marketo-Client-Id` | Su ID de cliente |
| `X-Marketo-Client-Secret` | Secreto de cliente |
| `X-Marketo-Munchkin-Id` | Su ID de cuenta de Munchkin |

### Otras herramientas {#other-tools}

Adobe aloja el servidor MCP [!DNL Marketo] y lo expone en una dirección URL pública. Cualquier cliente MCP que admita servidores remotos a través de transporte HTTP transmisible puede conectarse a él.No necesita un puente específico de la herramienta ni ningún software instalado localmente. Si la herramienta no aparece en la lista anterior, utilice los detalles de conexión siguientes para configurarla manualmente.

**Detalles de conexión:**

| Configuración | Valor |
| ------- | ----- |
| Transporte | HTTP (HTTP transmisible) |
| URL del servidor | `https://marketo-mcp.adobe.io/mcp` |

**Encabezados de autenticación:**

Envíe los encabezados de uno de los siguientes métodos de autenticación con cada solicitud. El lugar donde se introduce la URL del servidor y los encabezados dependen de la herramienta, por lo que consulte su documentación de MCP.

>[!BEGINTABS]

>[!TAB Token de IMS]

| Encabezado | Valor |
| ------ | ----- |
| `Authorization` | `Bearer YOUR-IMS-TOKEN` |
| `x-gw-ims-org-id` | Su ID de organización de IMS |

>[!TAB Credenciales de cliente de Marketo]

| Encabezado | Valor |
| ------ | ----- |
| `X-Marketo-Client-Id` | Su ID de cliente |
| `X-Marketo-Client-Secret` | Secreto de cliente |
| `X-Marketo-Munchkin-Id` | Su ID de cuenta de Munchkin |

>[!ENDTABS]

Si la herramienta acepta una configuración JSON, comience con los ejemplos de [Cursor](#cursor) o [Código VS](#vscode), y ajuste las claves (`mcpServers`, `servers`) para que coincidan con el esquema de la herramienta.

## Operaciones disponibles

Una vez conectado, puede pedir a su asistente de IA que realice operaciones en las siguientes categorías. Para obtener la lista completa de operaciones compatibles con referencias de API, consulte [Operaciones de MCP compatibles](mcp-server-operations.md).

### Formularios

Examinar, crear, clonar y aprobar formularios. Agregue o quite campos, configure reglas de visibilidad de campos e identifique dónde están incrustados los formularios.

Ejemplos de peticiones de datos:

* &quot;Mostrar todos los formularios aprobados&quot;
* &quot;Clone el formulario Contact Us en la carpeta Q2 Campaign&quot;
* &quot;Agregar un campo Compañía al formulario de solicitud de demostración&quot;

### Campañas inteligentes

Cree campañas inteligentes, configure filtros de listas inteligentes, agregue pasos de flujo y active o desactive campañas.

Ejemplos de peticiones de datos:

* &quot;¿Qué campañas inteligentes están activas ahora?&quot;
* &quot;Cree una nueva campaña inteligente llamada Actualización de puntuación de posibles clientes en la carpeta Operaciones&quot;
* &quot;Mostrarme los pasos de flujo en la campaña de correo electrónico de bienvenida&quot;

### Posibles clientes y listas

Buscar posibles clientes por dirección de correo electrónico, crear o actualizar registros de posibles clientes y administrar la pertenencia a listas estáticas.

Ejemplos de peticiones de datos:

* &quot;Encontrar el posible cliente con el correo electrónico jane@example.com&quot;
* &quot;Agregar ID de posible cliente 12345 a la lista de MQL del segundo trimestre&quot;
* &quot;Crear una nueva lista estática llamada Asistentes al evento de verano&quot;

### Programas

Crear, clonar y etiquetar programas. Examinar programas por tipo, canal o intervalo de fechas.

Ejemplos de peticiones de datos:

* &quot;Clonar el programa de seminario web del cuarto trimestre en la carpeta Eventos de 2026&quot;
* &quot;Cree un nuevo programa de correo electrónico llamado Rebajas de verano en la carpeta Campañas&quot;
* &quot;Mostrar todos los programas etiquetados como seminario web&quot;

### Correos electrónicos y fragmentos

Examine correos electrónicos, cree correos electrónicos a partir de plantillas, actualice secciones de contenido y administre fragmentos reutilizables.

Ejemplos de peticiones de datos:

* &quot;Mostrar todos los borradores de correos electrónicos&quot;
* &quot;Actualizar la sección de encabezado del correo electrónico de bienvenida&quot;
* &quot;¿Qué recursos utilizan el fragmento de promoción de vacaciones?&quot;

### Estructura de instancia

Para comprender la configuración de [!DNL Marketo], examine carpetas, canales, tipos de etiquetas y tipos de actividades.

Ejemplos de peticiones de datos:

* &quot;Enumerar todas las carpetas en Marketo&quot;
* &quot;Mostrar todos los canales disponibles&quot;
* &quot;¿Qué tipos de etiquetas están configurados?&quot;

### Operaciones masivas

Exportar datos de posibles clientes de forma masiva y comprobar el estado del trabajo de importación o exportación.

Ejemplos de peticiones de datos:

* &quot;Crear una exportación masiva de posibles clientes creada en los últimos 30 días&quot;
* &quot;Comprobar el estado del trabajo de exportación xx&quot;

## Resolución de problemas

| Error | Causa | Corregir |
| ------- | ------- | ----- |
| &quot;No se han proporcionado las credenciales de Marketo&quot; | Falta uno o más de `X-Marketo-Client-Id`, `X-Marketo-Client-Secret` o `X-Marketo-Munchkin-Id`. | Compruebe que todos los encabezados de credenciales del cliente de Marketo estén presentes en la configuración. |
| &quot;401 No autorizado&quot; | Faltan las credenciales, no son válidas o han caducado. Con las credenciales del cliente de Marketo, el ID o el secreto del cliente son incorrectos. Con un token de IMS, el token no es válido o ha caducado. | Compruebe las credenciales del método de autenticación. Para obtener las credenciales de cliente, vuelva a comprobar **[!UICONTROL el ID de cliente]** y **[!UICONTROL el secreto de cliente]** en **[!UICONTROL Admin]** > **[!UICONTROL LaunchPoint]**. Para un token de IMS, genere un nuevo token y actualice el encabezado `Authorization`. |
| &quot;403 Prohibido&quot; | Sus credenciales son válidas, pero la instancia de [!DNL Marketo] no está habilitada para el acceso a MCP. | Póngase en contacto con su administrador de MCP [!DNL Marketo] para habilitar el acceso de MCP a su ID de cuenta de Munchkin. |
| &quot;Demasiadas solicitudes&quot; (límite de tasa) | Ha enviado demasiadas solicitudes en un breve período o demasiadas solicitudes al mismo tiempo y ha alcanzado los límites de API de su instancia de [!DNL Marketo]. | Reduzca la frecuencia y la cantidad de solicitudes que envía a la vez y espere un poco antes de volver a intentarlo. Use un usuario de API dedicado para rastrear y administrar su cuota. |
| Tiempo de espera de conexión o rechazado | No se puede acceder al servidor MCP desde la red. | Confirme que puede acceder a la URL del servidor desde su entorno. Compruebe los requisitos de VPN si corresponde. |
| Las llamadas a herramientas devuelven resultados vacíos | El usuario de la API carece de permisos para el tipo de recurso solicitado. | Pida al administrador de [!DNL Marketo] que revise la función de usuario y los permisos de la API. |

## Consideraciones de seguridad

>[!IMPORTANT]
>
>Use un usuario de API dedicado en [!DNL Marketo] con solo los permisos necesarios para su trabajo. No reutilice las credenciales de administrador para el acceso a la API.

* **Credenciales por solicitud.** El ID del cliente, el Secreto del cliente, el ID de Munchkin y el extremo de la API de REST se transmiten en encabezados HTTP con cada solicitud. El servidor no los almacena en caché.
* **Aislamiento de varios inquilinos.** Cada solicitud utiliza su propio conjunto de credenciales. Los datos no se cruzan con la sesión de ningún otro usuario.
* **lista de permitidos de Munchkin ID.** El servidor solo acepta solicitudes para instancias de [!DNL Marketo] aprobadas. Las solicitudes que utilizan un ID de Munchkin no autorizado se rechazan con un error 403.
* **límites de tasa de API.** El servidor MCP hereda los límites de tasa de API de su instancia [!DNL Marketo]. Utilice un usuario de API dedicado para rastrear y administrar el consumo de cuotas.
* **Mantener credenciales fuera del control de versiones.** Utilice la interpolación de variables de entorno (`${MARKETO_CLIENT_SECRET}`) si la herramienta de IA la admite, de modo que las credenciales no se almacenen en texto sin formato en los archivos del repositorio.

## Gobernanza y retención de datos

### Administración de credenciales

* Las credenciales del cliente no se mantienen en el lado del servidor y las proporciona el cliente por solicitud, lo que ayuda a limitar la exposición de las credenciales dentro del servicio.

### Modelo de interacción de API

* Uso del agente: los agentes pueden utilizar el servidor MCP para invocar las API de Marketo admitidas.
* Alineación del modelo de autenticación: El servicio utiliza el mismo modelo de autenticación de API externa documentado para las API de Marketo.

### Autenticación y autorización

* Privilegio Least: los permisos efectivos se heredan del usuario solo de la API de Marketo asignado al servicio LaunchPoint del cliente, lo que permite una administración de menos privilegios dentro de la configuración de Marketo del cliente.
* Sin persistencia de tokens del lado del servidor: El servicio sigue evitando el almacenamiento del lado del servidor de credenciales de cliente o tokens.  

### Registro y monitorización

* Registro de seguridad: los registros JSON estructurados se enrutan a través de Bit fluido a Splunk, con enmascaramiento de datos confidenciales y filtrado adicional para admitir requisitos de cumplimiento.
* Soporte de auditoría: estos controles admiten la monitorización continua de la disponibilidad del servicio, los eventos relevantes para la seguridad y la calidad operativa.
* Sin almacenamiento secreto del lado del servidor: la implementación de MCP no almacena las credenciales del cliente, que deben proporcionarlas los clientes por cada solicitud.
* Administración de tokens: los tokens de acceso son de corta duración, las respuestas de tokens se marcan como sin almacenamiento y los tokens se aceptan a través de mecanismos de autorización estándar en lugar de transmisión de cadena de consulta.
* Acceso operativo basado en funciones: el acceso de implementación administrativa se rige por las funciones de infraestructura de Adobe y los controles basados en grupos, mientras que los permisos del plano de datos se heredan de la configuración de usuario de la API de Marketo del cliente.
* Auditoría y observabilidad: el registro, el enmascaramiento, la monitorización y las alertas de seguridad están habilitados para admitir la investigación, el seguimiento del estado del servicio y la supervisión operativa.
