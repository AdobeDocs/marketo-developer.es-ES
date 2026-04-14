---
title: Servidor MCP
description: Aprenda a conectar un asistente de IA a Marketo mediante el servidor MCP. Configure Claude Desktop, Cursor, Claude Code o VS Code con sus credenciales de Marketo.
hidefromtoc: true
source-git-commit: a9946d79bfc4cabd27fe33d95f25ee99d777fb1b
workflow-type: tm+mt
source-wordcount: '1324'
ht-degree: 1%

---


# Servidor MCP [!DNL Marketo]

El Protocolo de contexto de modelo (MCP) es un estándar abierto que permite a las herramientas de IA comunicarse con servicios externos. El servidor MCP [!DNL Marketo] actúa como un puente entre su asistente de IA y [!DNL Marketo]. Expone más de 100 operaciones en formularios, programas, campañas inteligentes, posibles clientes, correos electrónicos, fragmentos de código, listas y carpetas.

Cuando la herramienta de IA llama al servidor MCP, el servidor ejecuta la llamada de API de REST correspondiente en su nombre, utilizando las credenciales proporcionadas en cada solicitud. No es necesario instalar, implementar ni ejecutar ningún software del lado del servidor.

## Requisitos previos

- Una instancia de [!DNL Marketo] con acceso a la API de REST habilitado
- Acceso de administrador para crear credenciales de API en [!DNL Marketo] LaunchPoint
- Una de las siguientes herramientas de IA: Claude Desktop, Cursor, Claude Code (CLI) o VS Code con GitHub Copilot
- Acceso de red a la URL del servidor MCP: `https://marketo-mcp.adobe.io/mcp`

## Obtener credenciales de Marketo

Necesita los siguientes valores de su instancia de [!DNL Marketo]:

- **ID de cliente**
- **Secreto de cliente**
- **ID de cuenta de Munchkin**
- **Punto final de API de REST**

Si ya los tiene, vaya a [Configurar la herramienta de IA](#configure-your-ai-tool).

### ID de cliente y secreto de cliente

1. Vaya a **[!UICONTROL Administración]** > **[!UICONTROL LaunchPoint]**.
1. Haga clic en su servicio API. Si no tiene uno, seleccione **[!UICONTROL Nuevo]** > **[!UICONTROL Nuevo servicio]**, elija **[!UICONTROL Personalizado]** como tipo de servicio y asigne un usuario de API dedicado.
1. Haga clic en **[!UICONTROL Ver detalles]** y copie los valores de **[!UICONTROL ID de cliente]** y **[!UICONTROL Secreto de cliente]**.

### ID de cuenta de Munchkin

1. Vaya a **[!UICONTROL Administración]** > **[!UICONTROL Munchkin]**.
1. Copie la **[!UICONTROL cuenta de Munchkin]**. El formato es `XXX-XXX-XXX` y coincide con el prefijo de la dirección URL de la instancia.

### Punto final de API REST

1. Vaya a **[!UICONTROL Administración]** > **[!UICONTROL Servicios web]**.
1. En la **[!UICONTROL API de REST]**, copie la dirección URL del **[!UICONTROL extremo]**. El formato es `https://XXX-XXX-XXX.mktorest.com`.

## Configuración de la herramienta IA

Cada herramienta de IA lee la configuración del servidor MCP desde una ubicación diferente. Busque la herramienta a continuación y siga los pasos para agregar el servidor MCP [!DNL Marketo].

### Claude Desktop

El archivo de configuración es `claude_desktop_config.json`. Ábralo desde una de estas ubicaciones:

- **macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Windows**: `%APPDATA%\Claude\claude_desktop_config.json`
- **Linux**: `~/.config/Claude/claude_desktop_config.json`

Si el archivo ya contiene otros servidores MCP, agregue la entrada `marketo` en `mcpServers`. El siguiente ejemplo muestra el bloque `mcpServers` completo:

```json
{
  "mcpServers": {
    "marketo": {
      "type": "http",
      "url": "https://marketo-mcp.adobe.io/mcp",
      "headers": {
        "X-Marketo-Client-Id": "YOUR-CLIENT-ID",
        "X-Marketo-Client-Secret": "YOUR-CLIENT-SECRET",
        "X-Marketo-Munchkin-Id": "YOUR-MUNCHKIN-ID",
        "X-Marketo-Endpoint": "YOUR-REST-API-ENDPOINT"
      }
    }
  }
}
```

Guarde el archivo, salga de Claude Desktop y vuelva a abrirlo.

### Cursor

Si la configuración de MCP de cursor ya contiene otros servidores, agregue la entrada `marketo` en `mcpServers`. El siguiente ejemplo muestra el bloque `mcpServers` completo en **[!UICONTROL Configuración]** > **[!UICONTROL MCP]** o `.cursor/mcp.json` en el directorio del proyecto:

```json
{
  "mcpServers": {
    "marketo": {
      "type": "http",
      "url": "https://marketo-mcp.adobe.io/mcp",
      "headers": {
        "X-Marketo-Client-Id": "YOUR-CLIENT-ID",
        "X-Marketo-Client-Secret": "YOUR-CLIENT-SECRET",
        "X-Marketo-Munchkin-Id": "YOUR-MUNCHKIN-ID",
        "X-Marketo-Endpoint": "YOUR-REST-API-ENDPOINT"
      }
    }
  }
}
```

Reiniciar cursor.

### Código Claude (CLI)

Ejecute el siguiente comando en el terminal, sustituyendo las credenciales:

```bash
claude mcp add --transport http marketo \
  https://marketo-mcp.adobe.io/mcp \
  --header "X-Marketo-Client-Id: YOUR-CLIENT-ID" \
  --header "X-Marketo-Client-Secret: YOUR-CLIENT-SECRET" \
  --header "X-Marketo-Munchkin-Id: YOUR-MUNCHKIN-ID" \
  --header "X-Marketo-Endpoint: YOUR-REST-API-ENDPOINT"
```

### Código VS con el copiloto de GitHub

Abra el código VS `settings.json` presionando **[!UICONTROL Ctrl+Mayús+P]** o **[!UICONTROL Cmd+Mayús+P]** en macOS y, a continuación, seleccionando **[!UICONTROL Preferencias: Abrir configuración de usuario (JSON)]**. Añada el siguiente ejemplo:

```json
{
  "mcp": {
    "servers": {
      "marketo": {
        "type": "http",
        "url": "https://marketo-mcp.adobe.io/mcp",
        "headers": {
          "X-Marketo-Client-Id": "YOUR-CLIENT-ID",
          "X-Marketo-Client-Secret": "YOUR-CLIENT-SECRET",
          "X-Marketo-Munchkin-Id": "YOUR-MUNCHKIN-ID",
          "X-Marketo-Endpoint": "YOUR-REST-API-ENDPOINT"
        }
      }
    }
  }
}
```

Presione **[!UICONTROL Ctrl+Mayús+P]** (o **[!UICONTROL Cmd+Mayús+P]** en macOS), escriba **[!UICONTROL Volver a cargar la ventana]** y presione Entrar.

>[!NOTE]
>
>Por motivos de seguridad, utilice la interpolación de variables de entorno en archivos de configuración en lugar de pegar las credenciales directamente. Puede hacer referencia a variables mediante sintaxis como `${MARKETO_CLIENT_SECRET}` y establecerlas en su entorno. Esto evita que las credenciales se almacenen en texto sin formato en archivos que puedan enviarse al control de versiones.

## Operaciones disponibles

Una vez conectado, puede pedir a su asistente de IA que realice operaciones en las siguientes categorías.

### Formularios

Examinar, crear, clonar y aprobar formularios. Agregue o quite campos, configure reglas de visibilidad de campos e identifique dónde están incrustados los formularios.

Ejemplos de peticiones de datos:

- &quot;Mostrar todos los formularios aprobados&quot;
- &quot;Clone el formulario Contact Us en la carpeta Q2 Campaign&quot;
- &quot;Agregar un campo Compañía al formulario de solicitud de demostración&quot;

### Campañas inteligentes

Cree campañas inteligentes, configure filtros de listas inteligentes, agregue pasos de flujo y active o desactive campañas.

Ejemplos de peticiones de datos:

- &quot;¿Qué campañas inteligentes están activas ahora?&quot;
- &quot;Cree una nueva campaña inteligente llamada Actualización de puntuación de posibles clientes en la carpeta Operaciones&quot;
- &quot;Mostrarme los pasos de flujo en la campaña de correo electrónico de bienvenida&quot;

### Posibles clientes y listas

Buscar posibles clientes por dirección de correo electrónico, crear o actualizar registros de posibles clientes y administrar la pertenencia a listas estáticas.

Ejemplos de peticiones de datos:

- &quot;Encontrar el posible cliente con el correo electrónico jane@example.com&quot;
- &quot;Agregar ID de posible cliente 12345 a la lista de MQL del segundo trimestre&quot;
- &quot;Crear una nueva lista estática llamada Asistentes al evento de verano&quot;

### Programas

Crear, clonar y etiquetar programas. Examinar programas por tipo, canal o intervalo de fechas.

Ejemplos de peticiones de datos:

- &quot;Clonar el programa de seminario web del cuarto trimestre en la carpeta Eventos de 2026&quot;
- &quot;Cree un nuevo programa de correo electrónico llamado Rebajas de verano en la carpeta Campañas&quot;
- &quot;Mostrar todos los programas etiquetados como seminario web&quot;

### Correos electrónicos y fragmentos

Examine correos electrónicos, cree correos electrónicos a partir de plantillas, actualice secciones de contenido y administre fragmentos reutilizables.

Ejemplos de peticiones de datos:

- &quot;Mostrar todos los borradores de correos electrónicos&quot;
- &quot;Actualizar la sección de encabezado del correo electrónico de bienvenida&quot;
- &quot;¿Qué recursos utilizan el fragmento de promoción de vacaciones?&quot;

### Estructura de instancia

Examine carpetas, canales, tipos de etiquetas y tipos de actividades para comprender la configuración de [!DNL Marketo].

Ejemplos de peticiones de datos:

- &quot;Enumerar todas las carpetas en Marketo&quot;
- &quot;Mostrar todos los canales disponibles&quot;
- &quot;¿Qué tipos de etiquetas están configurados?&quot;

### Operaciones masivas

Exportar datos de posibles clientes de forma masiva y comprobar el estado del trabajo de importación o exportación.

Ejemplos de peticiones de datos:

- &quot;Crear una exportación masiva de posibles clientes creada en los últimos 30 días&quot;
- &quot;Comprobar el estado del trabajo de exportación xx&quot;

## Resolución de problemas

| Error | Causa | Corregir |
| ------- | ------- | ----- |
| &quot;Extremo de Marketo no proporcionado&quot; | Falta el encabezado `X-Marketo-Endpoint` en la configuración. | Vuelva a comprobar la configuración de MCP y confirme que los cuatro encabezados están presentes. |
| &quot;No se han proporcionado las credenciales de Marketo&quot; | Falta uno o más de `X-Marketo-Client-Id`, `X-Marketo-Client-Secret` o `X-Marketo-Munchkin-Id`. | Compruebe que los cuatro encabezados estén presentes en la configuración. |
| &quot;Error de autenticación&quot; | Las credenciales no son válidas o han caducado. | Vuelva a comprobar el ID de cliente y el secreto de cliente en **[!UICONTROL Admin]** > **[!UICONTROL LaunchPoint]**. |
| &quot;403 Prohibido&quot; | Su Munchkin ID no está en la lista de permitidos del servidor. | Póngase en contacto con su administrador de MCP [!DNL Marketo] para agregar su Munchkin ID. |
| Tiempo de espera de conexión o rechazado | No se puede acceder al servidor MCP desde la red. | Confirme que puede acceder a la URL del servidor desde su entorno. Compruebe los requisitos de VPN si corresponde. |
| Las llamadas a herramientas devuelven resultados vacíos | El usuario de la API carece de permisos para el tipo de recurso solicitado. | Pida al administrador de [!DNL Marketo] que revise la función de usuario y los permisos de la API. |

## Preguntas frecuentes

### ¿Mis datos son seguros?

Las credenciales se transmiten en encabezados HTTP con cada solicitud individual. El servidor no almacena ni almacena en caché las credenciales entre sesiones y cada solicitud está completamente aislada.

### ¿Pueden utilizarlo varias personas al mismo tiempo?

Sí. El servidor es de varios usuarios. Cada usuario se conecta con sus propias credenciales y las solicitudes están aisladas entre sí.

### ¿Qué sucede si caduca mi token de acceso?

Cuando se autentica con ID de cliente y Secreto de cliente, el servidor gestiona la actualización de tokens automáticamente. No es necesario que realice ninguna acción.

### ¿Necesito instalar o ejecutar algo?

No. El servidor MCP se aloja en Adobe. Solo es necesario configurar la herramienta de IA para conectarse a ella.

### ¿Qué permisos de [!DNL Marketo] necesita mi usuario de API?

El usuario de API necesita acceder a los tipos de recursos que desea administrar. Como mínimo, asigne una función de solo lectura a las operaciones de exploración y una función de lectura y escritura a la creación o modificación de recursos. Trabaje con el administrador de [!DNL Marketo] para asignar los permisos adecuados.

### ¿Cuáles son los límites de tasa?

El servidor MCP hereda los límites de tasa de API de la instancia de Marketo. Utilice un usuario de API dedicado para rastrear y administrar el consumo de cuotas.

### ¿Qué herramientas de IA son compatibles?

Claude Desktop, Cursor, Claude Code (CLI) y VS Code con GitHub Copilot. Cualquier herramienta de IA que admita el protocolo de contexto de modelo a través de HTTP debe funcionar.

### ¿Puedo conectarme a varias instancias de [!DNL Marketo]?

Sí. Añada varias entradas en la configuración de MCP de la herramienta AI, cada una con un nombre único y las credenciales de la instancia correspondiente. Por ejemplo, puede configurar `marketo-prod` y `marketo-staging` como servidores independientes.

## Consideraciones de seguridad

>[!IMPORTANT]
>
>Use un usuario de API dedicado en [!DNL Marketo] con solo los permisos necesarios para su trabajo. No reutilice las credenciales de administrador para el acceso a la API.

- **Credenciales por solicitud.** El ID del cliente, el Secreto del cliente, el ID de Munchkin y el extremo de la API de REST se transmiten en encabezados HTTP con cada solicitud. El servidor no los almacena en caché.
- **Aislamiento de varios inquilinos.** Cada solicitud utiliza su propio conjunto de credenciales. Los datos no se cruzan con la sesión de ningún otro usuario.
- **lista de permitidos de Munchkin ID.** El servidor solo acepta solicitudes para instancias de [!DNL Marketo] aprobadas. Las solicitudes que utilizan un ID de Munchkin no autorizado se rechazan con un error 403.
- **Mantener credenciales fuera del control de versiones.** Utilice la interpolación de variables de entorno (`${MARKETO_CLIENT_SECRET}`) si la herramienta de IA la admite, de modo que las credenciales no se almacenen en texto sin formato en los archivos enviados a un repositorio.
