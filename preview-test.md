---
title: Prueba de previsualización de EXL
description: Ejemplos de sintaxis de Markdown de Adobe EXL para probar la previsualización de la extensión.
source-git-commit: 8f7ff2e1b6d0a4d8f63affb7bd1a2d0abbcc118c
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 8%

---


# Prueba de previsualización de EXL

## Bloques de alerta

>[!NOTE]
>
>Esta es una nota. Utilice notas para información adicional que el lector debe tener en cuenta.

>[!TIP]
>
>Esto es una propina. Utilice sugerencias para obtener información opcional pero útil.

>[!IMPORTANT]
>
>Esta es una alerta importante. Se utiliza para obtener información que el lector no debe pasar por alto.

>[!WARNING]
>
>Esta es una advertencia. Se utiliza para obtener información sobre posibles problemas.

>[!CAUTION]
>
>Esto es una advertencia. Se utiliza para obtener información sobre los riesgos potenciales.

>[!ADMIN]
>
>Esta es una alerta de administrador. Para contenido de solo administración.

>[!AVAILABILITY]
>
>Esta es una nota de disponibilidad. Para obtener detalles de disponibilidad de funcionalidades.

>[!PREREQUISITES]
>
>Este es un bloque de requisitos previos. Enumere lo que necesita el lector antes de empezar.

## Cuadros de sombra

>[!BEGINSHADEBOX &quot;Título opcional&quot;]

Este contenido aparece con un fondo gris. Utilice cuadros de sombreado para agrupar visualmente el contenido relacionado.

Puede incluir listas:

- Elemento uno
- Elemento dos
- Elemento tres

>[!ENDSHADEBOX]

>[!BEGINSHADEBOX]

Cuadro de sombreado sin título.

>[!ENDSHADEBOX]

## Secciones contraíbles

+++Haga clic para expandir: ejemplo básico

Este contenido se oculta hasta que el usuario selecciona el título.

Puede incluir cualquier contenido aquí, incluidos los bloques de código:

```javascript
const example = 'hello world';
console.log(example);
```

+++

+++Configuración avanzada

Utilice secciones contraíbles para contenido opcional o avanzado que, de lo contrario, saturarían el flujo principal.

| Configuración | Valor | Descripción |
| --- | --- | --- |
| timeout | 30 | Segundos antes de que se agote el tiempo de espera de solicitud |
| reintentos | 3 | Número de intentos de reintento |

{style="table-layout:auto"}

+++

## Vídeo incrustado

>[!VIDEO](https://video.tv.adobe.com/v/3427028/?quality=12&learn=on)

## Macros de localización

Use [!DNL Marketo] para ajustar los nombres de los productos de modo que no estén localizados.

Use **[!UICONTROL Admin]** > **[!UICONTROL LaunchPoint]** para las etiquetas de elementos de la interfaz de usuario.

Ejemplo combinado: en [!DNL Adobe Analytics], seleccione **[!UICONTROL Workspace]** > **[!UICONTROL Crear proyecto]**.

## Insignias

[!BADGE Beta]{type=Informative}

[!BADGE Disponible de forma general]{type=Positive}

[!BADGE Obsoleto]{type=Negative}

[!BADGE Experimental]{type=Caution}

## Pestañas

>[!BEGINTABS]

>[!TAB Requisitos]

>[!IMPORTANT]
>
>Debe tener permisos de administrador para completar esta tarea.

Campos obligatorios:

| Campo | Tipo | Requerido |
| --- | --- | --- |
| Nombre | Cadena | Sí |
| Correo electrónico | Cadena | Sí |
| Función | Enumeración | No |

>[!TAB Pasos]

1. Abra Admin Console.
1. Seleccione **[!UICONTROL Usuarios]** > **[!UICONTROL Agregar usuario]**.
1. Rellene los campos obligatorios.
1. Seleccione **[!UICONTROL Guardar]**.

>[!NOTE]
>
>Los cambios entrarán en vigor inmediatamente.

>[!TAB Resultado]

El nuevo usuario recibe un correo electrónico de bienvenida con un vínculo para establecer su contraseña.

- El vínculo caduca pasadas 24 horas.
- Los usuarios pueden solicitar un vínculo nuevo desde la página de inicio de sesión.

>[!ENDTABS]

## Bloques de código

```json
{
  "name": "example",
  "version": "1.0.0",
  "enabled": true
}
```

```javascript
function greet(name) {
  return `Hello, ${name}!`;
}
```

## Tablas

| Columna uno | Columna dos | Columna tres |
| --- | --- | --- |
| Fila 1, celda 1 | Fila 1, celda 2 | Fila 1, celda 3 |
| Fila 2, celda 1 | Fila 2, celda 2 | Fila 2, celda 3 |
