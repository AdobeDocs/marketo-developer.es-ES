---
title: Prácticas recomendadas de integración de Marketo
feature: REST API
description: Prácticas recomendadas para integraciones de la API de Marketo que abarcan cuotas, límites de tasa y concurrencia, agrupamiento, importación y exportación masivas, almacenamiento en caché y planificación de latencia.
exl-id: 1e418008-a36b-4366-a044-dfa9fe4b5f82
TQID: https://experienceleague.adobe.com/Ld-rmFCwKSx-0W2-ceYICu0FQHK8BKAC1QgqtiOWDn4
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b13bd2ad-8e65-49e5-9691-2a0d31067b35
  - id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
  - id: f71e690b-4480-4b67-9ef5-88f42f9cdfdb
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: df401a2a-327d-468c-a5e4-b7b7ccd071a0
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 882
ht-degree: 0%

---

# Prácticas recomendadas de integración de Marketo

Diseñe integraciones en torno a los límites de API compartidas para su instancia de Marketo. Utilice lotes, almacenamiento en caché y tasas de solicitud conservadoras para mejorar el rendimiento y la fiabilidad.

## Límites de API

- **Cuota diaria:** A la mayoría de las suscripciones se les asignan 50 000 llamadas de API al día. La cuota se restablece diariamente a las 12:00 (CST). Póngase en contacto con su administrador de cuentas para aumentar la cuota diaria.
- **Límite de velocidad:** Cada instancia está limitada a 100 llamadas de API por 20 segundos.
- **Límite de concurrencia:** Cada instancia permite un máximo de diez llamadas API simultáneas.
- **Tamaño del lote:** La base de datos de posibles clientes admite 300 registros; la consulta de recursos admite 200 registros.
- **Tamaño de carga útil de API de REST:** 1 MB.
- **Tamaño de archivo de importación masiva:** 10 MB.
- **Tamaño máximo de lote de SOAP:** 300 registros.
- **Trabajos de extracción masiva:** Dos en ejecución y diez en cola, ambos incluidos.

## Sugerencias rápidas

- Establezca límites de uso conservadores porque su aplicación comparte recursos de cuota, tasa y concurrencia con otras aplicaciones.
- Utilice los métodos por lotes y por lotes de Marketo cuando estén disponibles. Utilice llamadas de un solo registro o de un solo resultado solo cuando sea necesario.
- Use [retroceso exponencial](https://en.wikipedia.org/wiki/Exponential_backoff) para reintentar las llamadas a la API que fallan debido a los límites de velocidad o concurrencia.
- Evite las llamadas de API simultáneas a menos que beneficien a su caso de uso.

## Lotes

Para las inserciones y actualizaciones, agrupe los registros en el menor número posible de transacciones. Al recuperar registros de un almacén de datos, agréguelos antes del envío en lugar de enviar una solicitud para cada cambio.

## Latencia aceptable

Defina la latencia aceptable (el tiempo máximo antes de enviar una llamada de API) al diseñar una integración. Esta opción determina qué métodos y opciones de configuración de Marketo se ajustan al caso de uso.

Por ejemplo, una integración en tiempo real que notifica a un vendedor cuando un usuario inicia una prueba puede enviar lotes de uno cuando se requiere un seguimiento inmediato. La mayoría de los casos de uso toleran una mayor latencia y funcionan de forma más eficiente poniendo en cola y agrupando llamadas.

| Latencia aceptable | Métodos preferidos | Notas |
| --- | --- | --- |
| Baja (&lt;10 s) | API sincrónicas (por lotes o sin lotes) | Asegúrese de que su caso de uso lo requiera. El envío de llamadas inmediatas y sincrónicas para casos de uso de alto volumen puede consumir rápidamente una cuota de API diaria y superar potencialmente los límites de tasa y concurrencia. |
| Medium(10 - 60 m) | API sincrónicas (por lotes) | Para integraciones de datos entrantes con Marketo, se recomienda utilizar una cola con un límite de edad y de tamaño. Cuando se alcance cualquiera de los límites, vacíe la cola y envíe la solicitud de API con los registros acumulados. Se trata de un compromiso sólido entre velocidad y eficacia, que garantiza que las solicitudes se produzcan en la cadencia requerida y, al mismo tiempo, agrupa tantos registros como la edad de la cola permita. |
| Alto(>60 m) | Importación/Exportación masiva (si es compatible) | Para integraciones de datos entrantes, los registros deben colocarse en cola y enviarse mediante las API por lotes de Marketo siempre que estén disponibles. |

## Límites diarios

Cada instancia de Marketo habilitada para la API tiene una asignación diaria de al menos 10 000 llamadas a la API de REST, aunque son comunes 50 000 o más. Cada instancia también tiene 500 MB o más de capacidad de extracción masiva. Se puede adquirir capacidad diaria adicional como parte de una suscripción de Marketo, pero los diseños de aplicaciones deben tener en cuenta los límites comunes de suscripción.

Todos los servicios y usuarios de API comparten la capacidad en una instancia. Elimine llamadas redundantes y registros por lotes en el menor número posible de llamadas.

El método de importación más eficiente para las llamadas es la API de importación masiva de Marketo, disponible para [posibles clientes/personas](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Leads/operation/importLeadUsingPOST) y [objetos personalizados](https://developer.adobe.com/marketo-apis/api/mapi#tag/Snippets/operation/createSnippetUsingPOST). Marketo también proporciona extracción en lotes para [posibles clientes](bulk-lead-extract.md) y [actividades](bulk-activity-extract.md).

### Almacenamiento en caché

Los resultados de las siguientes operaciones se pueden almacenar en caché en el lado del cliente durante un día o más, ya que cambian con poca frecuencia:

- Resultados de las operaciones de descripción
- [Tipos de actividad](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getAllActivityTypesUsingGET)
- [Particiones](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/getLeadPartitionsUsingGET)

En casos de uso, como el enriquecimiento de datos de clientes potenciales o de actividad, también puede almacenar en caché tipos de recursos como programas, correos electrónicos y carpetas.

## Límite de velocidad

Cada instancia de Marketo tiene un límite de tasa compartida de 100 llamadas por 20 segundos en todos los servicios de API de terceros. Si las llamadas superan este límite, la API devuelve un código de error 606.

En general, limite cada integración de terceros a 50 llamadas por 20 segundos o menos para que varias integraciones de API y usuarios puedan compartir la capacidad disponible. Algunos casos de uso pueden necesitar el límite completo. Sin embargo, las aplicaciones que utilizan el agrupamiento y se dirigen a un menor rendimiento suelen ser más adaptables y coherentes, con un pequeño aumento de la latencia.

## Límite de concurrencia

Cada instancia de Marketo tiene un límite compartido de diez llamadas a la API de REST que se ejecutan simultáneamente. No dé por hecho que su aplicación es el único consumidor de este límite.

Marketo cuenta las llamadas que se están procesando y aún no han devuelto. Cuando se devuelve una llamada a, ya no se cuenta para el límite de concurrencia.

La mayoría de las integraciones no se benefician de las llamadas simultáneas. Si implementa la concurrencia, limite inicialmente la aplicación a cinco o menos solicitudes simultáneas. Aumente el límite solo después de determinar que la aplicación requiere más.

## Errores

Excepto en casos excepcionales, las solicitudes de API devuelven el código de estado HTTP 200. Los errores de lógica empresarial también devuelven 200, pero incluyen detalles en el cuerpo de respuesta. Consulte [Códigos de error](error-codes.md) para obtener más información.

No evalúe la frase de motivo HTTP porque es opcional y está sujeta a cambios.
