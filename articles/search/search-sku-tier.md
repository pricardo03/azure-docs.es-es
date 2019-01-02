---
title: 'Selección de SKU o plan de tarifa del servicio Azure Search: Azure Search'
description: 'Azure Search puede aprovisionarse en estas SKU: Gratis, Básico o Estándar; este último está disponible en varias configuraciones de recursos y niveles de capacidad.'
services: search
author: HeidiSteen
manager: cgronlun
tags: azure-portal
ms.service: search
ms.topic: conceptual
ms.date: 09/25/2018
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 3c5e4d568e7118d50ce8779402526fca77ccdda7
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53315561"
---
# <a name="choose-a-pricing-tier-for-azure-search"></a>Selección de un plan de tarifa de Azure Search

En Azure Search, un [servicio se aprovisiona](search-create-service-portal.md) en el plan de tarifa o SKU que se fija para la duración del servicio. Los planes incluyen los niveles **Gratis**, **Básico** o **Estándar**; **este último** está disponible en varias configuraciones y capacidades. La mayoría de los clientes comienza con el nivel **Gratis** para la evaluación y, a continuación, ascienden al nivel **Estándar** para implementaciones de desarrollo y producción. Puede completar todos los inicios rápidos y tutoriales en el nivel **Gratis**, incluidos los de búsqueda cognitiva que consumen gran cantidad de recursos. 

Los niveles determinan la capacidad, no las características, y se diferencian por lo siguiente:

+ Número de índices que puede crear
+ Tamaño y velocidad de particiones (almacenamiento físico)

Aunque todos los niveles, incluido el nivel **Gratis**, generalmente ofrecen paridad de características, las cargas de trabajo más grandes pueden dictar requisitos para niveles más altos. Por ejemplo, la indexación de [Cognitive Search](cognitive-search-concept-intro.md) tiene funcionalidades de ejecución prolongada que agotan el tiempo de espera en un servicio gratis a menos que el conjunto de datos sea muy pequeño.

> [!NOTE] 
> La excepción a la paridad de características reside en los [indizadores](search-indexer-overview.md), que no están disponibles en S3HD.
>

Dentro de un nivel, también puede [ajustar los recursos de réplica y partición](search-capacity-planning.md) para optimizar el rendimiento. Aunque puede comenzar con dos o tres de cada uno, podría aumentar temporalmente su capacidad de proceso para una carga de trabajo con una elevada carga de trabajo de indexación. La posibilidad de ajustar los niveles de recursos dentro de un nivel agrega flexibilidad, pero también complica ligeramente su análisis. Es posible que tenga que experimentar para ver si un nivel inferior con mayores recursos/réplicas ofrece mejor valor y rendimiento que un nivel superior con menos recursos. Para obtener más información acerca de cuándo y por qué debería ajustar la capacidad, consulte [Consideraciones de rendimiento y optimización](search-performance-optimization.md).

<!---
The purpose of this article is to help you choose a tier. It supplements the [pricing page](https://azure.microsoft.com/pricing/details/search/) and [Service Limits](search-limits-quotas-capacity.md) page with a digest of billing concepts and consumption patterns associated with various tiers. It also recommends an iterative approach for understanding which tier best meets your needs. 
--->

## <a name="how-billing-works"></a>Cómo funciona la facturación

En Azure Search, el concepto de facturación más importante que se debe entender es la *unidad de búsqueda* (SU). Dado que Azure Search depende de las réplicas y las particiones para su funcionamiento, no tiene sentido facturar solo por una o la otra. En su lugar, la facturación se basa en una composición de ambas. 

Una SU es el producto de las *réplicas* y las *particiones* utilizadas por un servicio: **`(R X P = SU)`**

Cada servicio se inicia como mínimo con 1 SU (una réplica multiplicada por una partición). El máximo de cualquier servicio es de 36 SU, que se pueden lograr de varias maneras: 6 particiones por 6 réplicas, o 3 particiones por 12 réplicas, por nombrar algunas. 

Es habitual usar menos de la capacidad total. Por ejemplo, un servicio de 3 réplicas y 3 particiones, que se factura como 9 SU. 

La tarifa de facturación es **por hora y por SU**, y cada nivel aumenta progresivamente a una tarifa más elevada. Los niveles superiores incluyen particiones mayores y más rápidas, que contribuyen a un precio por hora total superior para ese nivel. Las tarifas de cada nivel pueden consultarse en [Buscar Precios](https://azure.microsoft.com/pricing/details/search/). 

La mayoría de los clientes solo ponen en línea una parte de la capacidad total, manteniendo el resto en la reserva. En términos de facturación, es el número de particiones y réplicas que tiene en línea, calculado mediante la fórmula de la SU, lo que determina lo que paga realmente por hora.

### <a name="tips-for-reducing-costs"></a>Sugerencias para reducir los costos

No se puede apagar el servicio para disminuir la factura. Los recursos operativos están operativos constantemente y se asignan para su uso exclusivo mientras el servicio esté vigente. La única manera de reducir una factura es reducir las réplicas y particiones a un nivel bajo que siga proporcionando un rendimiento aceptable y [cumpla con el Acuerdo de Nivel de Servicio](https://azure.microsoft.com/support/legal/sla/search/v1_0/). 

Un medio para reducir costos es elegir un nivel con una tarifa por hora más baja. Las tarifas por hora de S1 son inferiores a las tarifas de S2 o S3. Podría aprovisionar un servicio dirigido al nivel mínimo de sus proyecciones de carga. Si supera el servicio, cree un segundo servicio con un mayor nivel, recompile los índices en ese segundo servicio y luego elimine el primero. Si ha realizado la planificación de la capacidad de los servidores locales, ya sabe que es común "seguir comprando" para poder asumir el crecimiento proyectado. Pero con un servicio en la nube, puede ahorrar de manera más significativa porque no está sujeto a una compra específica. Siempre puede cambiar a un servicio de nivel superior si el actual no es suficiente.

### <a name="capacity-drill-down"></a>Profundización en la capacidad

En Azure Search, la capacidad se estructura como *réplicas* y *particiones*. 

+ Las réplicas son instancias del servicio de búsqueda, donde cada réplica hospeda una copia con equilibrio de carga de un índice. Por ejemplo, un servicio con 6 réplicas tiene 6 copias de todos los índices cargados en el servicio. 

+ Las particiones almacenan índices y dividen automáticamente los datos de búsqueda: dos particiones dividen el índice en la mitad, tres particiones en tres tercios y así sucesivamente. En términos de capacidad, el *tamaño de partición* es la principal característica diferenciadora entre niveles.

> [!NOTE]
> Todos los niveles **Estándar** admiten [combinaciones flexibles de réplicas y particiones](search-capacity-planning.md#chart) para que pueda [optimizar el sistema para velocidad o para almacenamiento](search-performance-optimization.md) cambiando el balance. El nivel **Básico** ofrece hasta tres réplicas para alta disponibilidad, pero tiene solo una partición. Los niveles **Gratis** no proporcionan recursos dedicados: los recursos de cálculo se comparten entre varios suscriptores.

### <a name="more-about-service-limits"></a>Más información acerca de los límites de servicio

Los servicios hospedan recursos, como los índices, los indexadores y otros. Cada nivel impone [límites de servicio](search-limits-quotas-capacity.md) en la cantidad de recursos que puede crear. Por lo tanto, un límite en el número de índices (y otros objetos) es la segunda característica diferenciadora entre niveles. Cuando revise cada opción en el portal, tenga en cuenta los límites en cuanto al número de índices. Otros recursos, como los indexadores, los orígenes de datos y los conjuntos de habilidades, están asociados a los límites de los índices.

## <a name="consumption-patterns"></a>Patrones de consumo

La mayoría de los clientes comienza con el servicio **Gratis**, que mantienen indefinidamente y, a continuación, seleccionan uno de los niveles **Estándar** para cargas de trabajo de desarrollo o de producción relevantes. 

![Niveles de Azure Search](./media/search-sku-tier/tiers.png "Planes de tarifa de Azure Search")

En cualquiera de los extremos, los niveles **Básico** y **S3 HD** existen para patrones de consumo importantes pero poco frecuentes. El nivel **Básico** es para pequeñas cargas de trabajo de producción: ofrece Acuerdo de Nivel de Servicio, recursos dedicados y alta disponibilidad, con un almacenamiento moderado, con un máximo de 2 GB. Este nivel se ha diseñado para clientes que están constantemente por debajo de la capacidad disponible. En el otro extremo, el nivel **S3 HD** es para cargas de trabajo típicas de ISV, asociados, [soluciones para varios inquilinos](search-modeling-multitenant-saas-applications.md) o cualquier configuración que precise de un gran número de índices pequeños. A menudo resulta evidente determinar si el nivel **Básico** o **S3 HD** es la opción adecuada, pero si desea confirmación también puede publicar mensajes en [StackOverflow](https://stackoverflow.com/questions/tagged/azure-search) o bien [póngase en contacto con el equipo de soporte técnico de Azure](https://azure.microsoft.com/support/options/) para más información.

Situando el foco en los niveles Estándar utilizados con mayor frecuencia, **S1-S3** son una progresión de niveles de capacidad en aumento, con puntos de inflexión en el tamaño de la partición y máximos en el número de índices, indexadores y recursos en consecuencia:

|  | S1 | S2 | S3 |  |  |  |  |
|--|----|----|----|--|--|--|--|
| tamaño de la partición|  25 GB | 100 GB | 250 GB |  |  |  |  |
| límites de índices e indexadores| 50 | 200 | 200 |  |  |  |  |

**S1** es una opción común cuando los recursos dedicados y varias particiones se convierten en una necesidad. Con particiones de 25 GB y hasta 12 particiones, el límite por servicio en el nivel **S1** es de 300 GB en total si maximiza las particiones con respecto a las réplicas (consulte [Asignación de particiones y réplicas](search-capacity-planning.md#chart) para composiciones más equilibradas).

Tanto el portal como las páginas de precios centran la atención en el tamaño de partición y el almacenamiento, pero para cada nivel, todas las funcionalidades de proceso (capacidad del disco, velocidad, CPU) crecen de modo lineal con el precio. Una réplica en **S2** es más rápida que la réplica en **S1** y en **S3** es más rápida que en **S2**. Los niveles **S3** rompen el patrón de proceso y precio habitualmente lineal con una E/S desproporcionadamente más rápida. Si prevé que la E/S será el cuello de botella, un nivel **S3** ofrece muchas más IOPS que los niveles inferiores.

**S3** y **S3 HD** están respaldados por la misma infraestructura de alta capacidad, pero cada uno de ellos alcanza su límite máximo de maneras diferentes. **S3** se dirige a un menor número de índices muy grandes. Como tal, su límite máximo es el límite de recursos (2,4 TB para cada servicio). **S3 HD** se dirige a un gran número de índices muy pequeños. A los 1000 índices, **S3 HD** alcanza sus límites en forma de restricciones de índice. Si es un cliente de **S3 HD** que requiere más de 1 000 índices, póngase en contacto con el soporte técnico de Microsoft para obtener información sobre cómo proceder.

> [!NOTE]
> Anteriormente, los límites de documentos se debían tener en cuenta, pero ya no son aplicables para la mayoría de los servicios nuevos. Para más información acerca de las condiciones para la que se aplican todavía límites de documentos, consulte [Límites del servicio: límites de documentos](search-limits-quotas-capacity.md#document-limits).
>

## <a name="evaluate-capacity"></a>Evaluación de la capacidad

La capacidad y los costes de ejecutar el servicio están relacionados. Los niveles imponen límites en dos ejes (almacenamiento y recursos), por lo que debe pensar en ambos, ya que aquel al que llegue primero es el límite real. 

Los requisitos empresariales exigen normalmente el número de índices que se necesita. Por ejemplo, un índice global de un repositorio de documentos grande o quizás varios índices en función de la región, aplicación o nicho de negocios.

Para determinar el tamaño de un índice, tendrá que [crear uno](search-create-index-portal.md). La estructura de datos de Azure Search es principalmente un [índice invertido](https://en.wikipedia.org/wiki/Inverted_index), que tiene características diferentes que los datos de origen. Para un índice invertido, el tamaño y la complejidad se determinan por el contenido, no necesariamente por la cantidad de datos que se incorporan. Un origen de datos de gran tamaño con gran redundancia podría resultar en un índice más pequeño que un conjunto de datos más pequeño que incluye contenido muy variable. Por lo tanto, es poco probable deducir el tamaño del índice en función del tamaño del conjunto de datos original.

> [!NOTE] 
> Aunque el cálculo de las necesidades futuras de índices y almacenamiento parte de suposiciones, vale la pena hacerlo. Si la capacidad de un nivel resulta demasiado baja, debe aprovisionar un nuevo servicio en el nivel superior y, después, [volver a cargar los índices](search-howto-reindex.md). No hay ninguna actualización en marcha del mismo servicio desde una SKU a otra.
>

### <a name="step-1-develop-rough-estimates-using-the-free-tier"></a>Paso 1: Desarrollo de estimaciones aproximadas con el nivel Gratis

Un enfoque para calcular la capacidad es comenzar con el nivel **Gratis**. Recuerde que el servicio **Gratis** ofrece hasta 3 índices, 50 MB de almacenamiento y 2 minutos de tiempo de indexación. Puede resultar complicado calcular un tamaño proyectado de índice con estas restricciones, pero en el ejemplo siguiente se muestra un enfoque:

+ [Cree un servicio gratis](search-create-service-portal.md)
+ Prepare un conjunto de datos pequeño y representativo (supongamos cinco mil documentos y tamaño de la muestra del diez por ciento)
+ [Genere un índice inicial](search-create-index-portal.md) y anote su tamaño en el portal (supongamos 30 MB)

Si la muestra es representativa y es el diez por ciento del origen de datos completo, un índice de 30 MB se convierte aproximadamente en uno de 300 MB si todos los documentos se indexan. Con este número preliminar, podría doblar esa cantidad para presupuestar dos índices (desarrollo y producción), con un total de 600 MB en los requisitos de almacenamiento. Esto se cumple fácilmente con el nivel **Básico**, por lo que debería empezar por ahí.

### <a name="step-2-develop-refined-estimates-using-a-billable-tier"></a>Paso 2: Desarrollo de estimaciones precisas mediante un nivel facturable

Algunos clientes prefieren comenzar con recursos dedicados que pueden adaptarse a mayores tiempos de muestreo y procesamiento y, a continuación, realizan una estimación realista de volúmenes de consultas y tamaño y cantidad de índices durante el desarrollo. Inicialmente, se aprovisiona un servicio basado en un cálculo basado en suposiciones y, a continuación, a medida que el proyecto evoluciona, los equipos suelen saben si el servicio está por encima o por debajo de la capacidad necesaria para las cargas de trabajo de producción previstas. 

1. [Revise los límites del servicio en cada nivel](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity#index-limits) para determinar si los niveles más bajos pueden admitir la cantidad de índices que necesita. En los niveles **Básico**-**S1**- **S2**, los límites de índices son 15-50-200, respectivamente.

1. [Cree un servicio en un nivel facturable](search-create-service-portal.md):

    + Comience bajo, en **Básico** o **S1** si está al principio de la curva de aprendizaje.
    + Comience alto, en **S2** o incluso **S3**, si las cargas de consulta e indexación a gran escala son evidentes.

1. [Genere un índice inicial](search-create-index-portal.md) para determinar cómo se traducen los datos de origen a un índice. Esta es la única manera de calcular el tamaño del índice.

1. [Supervise el almacenamiento, los límites del servicio, el volumen de consultas y la latencia](search-monitor-usage.md) en el portal. El portal muestra las consultas por segundo, las consultas limitadas y la latencia de búsqueda; esta información puede ayudarle a decidir si se encuentra en el nivel correcto. Además de las métricas del portal, puede configurar la supervisión detallada, como el análisis del porcentaje de clics, habilitando el [análisis del tráfico de búsqueda](search-traffic-analytics.md). 

El número de índices y el tamaño son igualmente importantes para el análisis porque los límites máximos se alcanzan mediante la utilización total del almacenamiento (particiones) o por los límites máximos en los recursos (índices, indexadores, etc.), lo que ocurra primero. El portal le ayuda a realizar un seguimiento de ambos, mostrando el uso actual y los límites máximos en paralelo en la página Información general.

> [!NOTE]
> Los requisitos de almacenamiento pueden ser demasiado excesivos si los documentos contienen datos extraños. Idealmente, los documentos contienen solo los datos que necesita para la experiencia de búsqueda. Los datos binarios no son utilizables en búsquedas y deben almacenarse por separado (por ejemplo, en un almacenamiento de tablas o blobs de Azure) con un campo en el índice para mantener una referencia de URL a los datos externos. El tamaño máximo de un documento individual es 16 MB (o menos si carga en masa varios documentos en una sola solicitud). Consulte [Límites del servicio en Azure Search](search-limits-quotas-capacity.md) para más información.
>

**Consideraciones del volumen de consultas**

Las consultas por segundo (QPS) son una medida que gana importancia durante la optimización del rendimiento, pero no suele ser una consideración de nivel a menos que espere un volumen muy elevado de consultas desde el principio.

Todos los niveles Estándar pueden entregar un balance entre réplicas y particiones, admitiendo una respuesta de consultas más rápida mediante réplicas adicionales para el equilibrio de carga y particiones adicionales para el procesamiento en paralelo. Puede ajustar el rendimiento después de aprovisionar el servicio.

Los clientes que esperan altos volúmenes de consultas sostenidos desde el principio deben considerar los niveles superiores, respaldados por un hardware más potente. A continuación, puede desconectar particiones y réplicas o incluso cambiar a un servicio de nivel inferior, si dichos volúmenes de consultas no se materializan. Para más información sobre cómo calcular el rendimiento de las consultas, consulte [Rendimiento y optimización de Azure Search](search-performance-optimization.md).


**Contratos de nivel de servicio**

El nivel **Gratis** y las características en versión preliminar no incluyen [contratos de nivel de servicio (SLA)](https://azure.microsoft.com/support/legal/sla/search/v1_0/). Para todos los niveles facturables, los SLA tomarán efecto cuando se aprovisione suficiente redundancia para el servicio. Son necesarias dos o más réplicas para el SLA de consulta (lectura). Son necesarias tres o más réplicas para el SLA de consulta e indexación (lectura y escritura). El número de particiones no es una consideración del SLA. 

## <a name="tips-for-tier-evaluation"></a>Sugerencias para la evaluación de nivel

+ Aprenda a crear índices eficaces y las metodologías de actualización con el menor impacto. Se recomienda el [análisis del tráfico de búsqueda](search-traffic-analytics.md) para los conocimientos adquiridos en la actividad de consulta.

+ Cree métricas sobre las consultas y recopile datos sobre los patrones de uso (consultas durante el horario laboral, indexación durante las horas sin picos) y utilice estos datos para informar en las futuras decisiones de aprovisionamiento del servicio. Aunque no resulta práctico en una base horaria o diaria, puede ajustar dinámicamente las particiones y los recursos para ajustarse a los cambios planeados en el volumen de consultas o cambios no planeados pero sostenidos si los niveles se mantienen para apoyar la toma de acciones.

+ Recuerde que la única desventaja de un aprovisionamiento inferior es que es posible que deba anular un servicio si los requisitos reales son mayores que los estimados. Para evitar la interrupción del servicio, se crea un nuevo servicio en la misma suscripción en un nivel superior y se ejecuta en paralelo hasta que todas las solicitudes y las aplicaciones se dirijan al nuevo punto de conexión.

## <a name="next-steps"></a>Pasos siguientes

Comience con un nivel **Gratis** y cree un índice inicial con un subconjunto de los datos para comprender sus características. La estructura de datos de Azure Search es un índice invertido, donde el tamaño y la complejidad de un índice invertido se determinan por el contenido. Recuerde que un contenido de gran redundancia tiende a producir un índice más pequeño que el contenido de alta irregularidad. Por lo tanto, son las características del contenido más que el tamaño del conjunto de datos lo que determina los requisitos de almacenamiento de índices.

Una vez que tenga una idea inicial del tamaño del índice, [aprovisione un servicio facturable](search-create-service-portal.md) en uno de los niveles descritos en este artículo, ya sea en el nivel **Básico** o **Estándar**. Rebaje las restricciones artificiales en los subconjuntos de datos y [vuelva a generar el índice](search-howto-reindex.md) para que incluya todos los datos sobre los que desea realizar búsquedas.

[Asigne particiones y réplicas](search-capacity-planning.md) según sea necesario para obtener el rendimiento y la escala que necesite.

Si la capacidad y rendimiento son válidos, ya ha terminado. En caso contrario, vuelva a crear un servicio de búsqueda en otro nivel que se adapte más estrechamente a sus necesidades.

> [!NOTE]
> Para más ayuda con sus preguntas, puede publicarlas en [StackOverflow](https://stackoverflow.com/questions/tagged/azure-search) o bien [póngase en contacto con el equipo de soporte técnico de Azure](https://azure.microsoft.com/support/options/).