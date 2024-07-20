---
title: Prácticas recomendadas de integración de Marketo
feature: REST API
description: Prácticas recomendadas para usar las API de Marketo.
exl-id: 1e418008-a36b-4366-a044-dfa9fe4b5f82
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '952'
ht-degree: 0%

---

# Prácticas recomendadas de integración de Marketo

## Límites de API

- **Cuota diaria:** A la mayoría de las suscripciones se les asignan 50.000 llamadas API al día (que se restablecen diariamente a las 12:00 horas CST). Puede aumentar su cuota diaria a través de su administrador de cuentas.
- **Límite de velocidad:** Acceso a API por instancia limitado a 100 llamadas por 20 segundos.
- **Límite de simultaneidad:**  Máximo de diez llamadas API simultáneas.
- **Tamaño del lote:** Base de datos de posibles clientes - 300 registros; Consulta de recursos - 200 registros
- **Tamaño de carga útil de API de REST:** 1 MB
- **Tamaño de archivo de importación masiva:** 10 MB
- SOAP **Tamaño máximo del lote de datos:** 300 registros
- **Trabajos de extracción masiva:** 2 en ejecución; 10 en cola (inclusive)

## Sugerencias rápidas

- Supongamos que su aplicación va a competir por recursos de cuota, tasa y concurrencia con otras aplicaciones, y establezca límites de uso conservadores.
- Utilice los métodos por lotes y por lotes de Marketo cuando estén disponibles y sean adecuados. Utilice únicamente llamadas de un solo registro o de un solo resultado cuando sea necesario.
- Use [retroceso exponencial](https://en.wikipedia.org/wiki/Exponential_backoff) para reintentar las llamadas a la API que fallan debido a los límites de velocidad o concurrencia.
- Evite realizar llamadas de API simultáneas si su caso de uso no se beneficia de él.

## Lotes

Para garantizar el mejor rendimiento de las integraciones, al realizar inserciones o actualizaciones, los registros deben agruparse en el menor número posible de transacciones. Al recuperar registros de un almacén de datos para su envío, los registros siempre deben agregarse antes del envío, en lugar de enviar una solicitud para cada cambio individual.

## Latencia aceptable

La determinación de las tolerancias de latencia o de la cantidad máxima de tiempo que puede pasar antes de enviar una llamada de API informará a muchos, si no a la mayoría, de las decisiones que tome al diseñar su integración en Marketo. Marketo proporciona muchos métodos y opciones de configuración diferentes que son adecuados para diferentes casos de uso y diferentes clases de latencia. Por ejemplo, una integración en tiempo real para notificar a un vendedor que un usuario se ha registrado en una prueba solo puede enviar lotes de uno si se requiere un seguimiento inmediato. Sin embargo, la mayoría de los casos no lo requieren y pueden tolerar una latencia adicional, y se pueden administrar de forma más eficiente mediante la colocación en cola y el agrupamiento de llamadas.

| Latencia aceptable | Métodos preferidos | Notas |
|---|---|---|
| Baja (&lt;10 s) | API sincrónicas (por lotes o sin lotes) | Asegúrese de que su caso de uso lo requiera. El envío de llamadas inmediatas y sincrónicas para casos de uso de alto volumen puede consumir rápidamente una cuota de API diaria y superar potencialmente los límites de tasa y concurrencia. |
| Medium(10 - 60 m) | API sincrónicas (por lotes) | Para integraciones de datos entrantes con Marketo, se recomienda utilizar una cola con un límite de edad y de tamaño. Cuando se alcance cualquiera de los límites, vacíe la cola y envíe la solicitud de API con los registros acumulados. Se trata de un compromiso sólido entre velocidad y eficacia, que garantiza que las solicitudes se produzcan en la cadencia requerida y, al mismo tiempo, agrupa tantos registros como la edad de la cola permita. |
| Alto(>60 m) | Importación/Exportación masiva (si es compatible) | Para integraciones de datos entrantes, los registros deben colocarse en cola y enviarse mediante las API por lotes de Marketo siempre que estén disponibles. |

## Límites diarios

Cada instancia de Marketo habilitada para la API tiene una asignación diaria de al menos 10 000 llamadas de API REST al día, pero normalmente 50 000 o más, y 500 MB o más de capacidad de extracción masiva. Aunque se puede adquirir capacidad diaria adicional como parte de una suscripción a Marketo, el diseño de la aplicación debe tener en cuenta los límites comunes de las suscripciones a Marketo.

Como la capacidad se comparte entre todos los servicios de API y los usuarios de una instancia, la práctica recomendada es eliminar llamadas redundantes y agrupar registros en el menor número posible de llamadas. La manera más eficiente de importar registros es usando las API de importación masiva de Marketo, que están disponibles para [posibles clientes/personas](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/importLeadUsingPOST) y [objetos personalizados](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Snippets/operation/createSnippetUsingPOST). Marketo también proporciona extracción en lotes para [posibles clientes](bulk-lead-extract.md) y [actividades](bulk-activity-extract.md).

### Almacenamiento en caché

Los resultados de las siguientes operaciones se pueden almacenar en caché en el lado del cliente durante un día o más, ya que cambian con poca frecuencia:

- Resultados de las operaciones de descripción
- [Tipos de actividades](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getAllActivityTypesUsingGET)
- [Particiones](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadPartitionsUsingGET)

El almacenamiento en caché de ciertos tipos de recursos, como programas, correos electrónicos y carpetas, también es adecuado para determinados casos de uso, como el enriquecimiento de datos para registros de posibles clientes o de actividades.

## Límite de velocidad

Cada instancia de Marketo tiene un límite de tasa de 100 llamadas por 20 segundos, que se comparten entre todos los servicios de API de terceros. Si se supera este límite, la API responde con un código de error 606 que indica que se ha superado el límite de velocidad. En general, las integraciones de terceros deben limitar su utilización a 50 llamadas por 20 segundos o menos para permitir un uso justo de los límites de velocidad por parte de varias integraciones y usuarios de API. Aunque puede ser apropiado saturar este límite en ciertos casos, en general, las aplicaciones que utilizan lotes y dirigen su rendimiento a menos de este límite son más receptivas y coherentes en su funcionamiento, a un pequeño coste de mayor latencia.

## Límite de concurrencia

Cada instancia de Marketo tiene un límite compartido de diez llamadas a la API de REST que se ejecutan simultáneamente. Al igual que los límites diarios de cuota y tasa, se comparte, por lo que no debe suponer que su aplicación será el consumidor exclusivo de este límite. Marketo cuenta el número de llamadas simultáneas como aquellas que se están procesando y aún no han devuelto, por lo que cuando se devuelve una llamada, ya no se cuenta con el límite de llamadas simultáneas.

La mayoría de los casos de uso de integración no se benefician de realizar llamadas simultáneas, por lo que debe considerar si la aplicación se beneficia de antes de decidir enviar solicitudes simultáneas a Marketo. Si desea implementar la concurrencia, debe limitar el número de solicitudes simultáneas a cinco o menos en el diseño inicial y solo aumentarlo después de determinar que la aplicación requiere más.

## Errores

Excepto en algunos casos excepcionales, las solicitudes de API devuelven un código de estado HTTP de 200. Los errores de lógica empresarial también devuelven un 200, pero contienen información detallada en el cuerpo de la respuesta. Consulte [Códigos de error](error-codes.md) para obtener una explicación detallada. La frase de motivo HTTP no debe evaluarse porque es opcional y está sujeta a cambios.
