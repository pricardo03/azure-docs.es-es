---
title: 'Selección de SKU o plan de tarifa del servicio Azure Search: Azure Search'
description: 'Azure Search puede aprovisionarse en estas SKU: Gratis, Básico o Estándar; este último está disponible en varias configuraciones de recursos y niveles de capacidad.'
services: search
author: HeidiSteen
manager: cgronlun
tags: azure-portal
ms.service: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 3e813b8a1709675d0d870d64df83428ab82e25b3
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65024294"
---
# <a name="choose-a-pricing-tier-for-azure-search"></a>Selección de un plan de tarifa de Azure Search

En Azure Search, un [recurso se crea](search-create-service-portal.md) en el plan de tarifa o SKU que se fija para el periodo de vigencia del servicio. Los niveles incluyen **gratis**, **básica**, **estándar**, o **optimizadas para almacenamiento**.  **Estándar** y **optimizadas para almacenamiento** están disponibles en varias configuraciones y capacidades. 

La mayoría de clientes comienzan con la **gratis** nivel para su evaluación y, a continuación, se graduará en uno de los niveles más altos de pago para las implementaciones de desarrollo y producción. Puede completar todos los inicios rápidos y tutoriales en el nivel **Gratis**, incluidos los de búsqueda cognitiva que consumen gran cantidad de recursos.

> [!NOTE]
> Los niveles de servicio de almacenamiento optimizado disponibles actualmente como versión preliminar a un precio con descuento para fines de pruebas y experimentación con el fin de recabar comentarios. El precio final se anunciará más adelante cuando estos niveles están disponibles con carácter general. Aconsejamos que no se usen estos niveles para las aplicaciones de producción.

Los niveles reflejan las características del hardware que hospeda el servicio (en lugar de las características) y se diferencian por:

+ Número de índices que puede crear
+ Tamaño y velocidad de particiones (almacenamiento físico)

Aunque todos los niveles, incluido el nivel **Gratis**, generalmente ofrecen paridad de características, las cargas de trabajo más grandes pueden dictar requisitos para niveles más altos. Por ejemplo, [AI indexación con Cognitive Services](cognitive-search-concept-intro.md) tiene habilidades de larga ejecución ese tiempo de espera en un servicio gratuito, a menos que el conjunto de datos resulta ser pequeño.

> [!NOTE] 
> La excepción a la paridad de características reside en los [indizadores](search-indexer-overview.md), que no están disponibles en S3HD.
>

Dentro de un nivel, también puede [ajustar los recursos de réplica y partición](search-capacity-planning.md) para aumentar o disminuir la escala. Puede empezar con uno o dos de cada uno y, a continuación, elevar temporalmente su eficacia sin igual para una gran carga de trabajo de indexación. La posibilidad de ajustar los niveles de recursos dentro de un nivel agrega flexibilidad, pero también complica ligeramente su análisis. Es posible que tenga que experimentar para ver si un nivel inferior con mayores recursos/réplicas ofrece mejor valor y rendimiento que un nivel superior con menos recursos. Para obtener más información acerca de cuándo y por qué debería ajustar la capacidad, consulte [Consideraciones de rendimiento y optimización](search-performance-optimization.md).

## <a name="tiers-for-azure-search"></a>Niveles para Azure Search

En la tabla siguiente se enumera los niveles disponibles. Incluyen otras fuentes de información de nivel el [página de precios](https://azure.microsoft.com/pricing/details/search/), [los límites de servicio y los datos](search-limits-quotas-capacity.md)y la página del portal cuando un servicio de aprovisionamiento.

|Nivel | Capacity |
|-----|-------------|
|Gratuito | Se comparte con otros suscriptores. No escalable, limitada a 3 índices y 50 MB de almacenamiento. |
|Básica | Recursos informáticos dedicados para cargas de trabajo de producción en una escala menor. Una partición de 2 GB y hasta tres réplicas. |
|1 estándar (S1) | Desde S1 en seguridad de máquinas dedicadas con más capacidad de almacenamiento y procesamiento en cada nivel. Tamaño de la partición es 25 GB por partición (máximo de 300 GB por servicio) para S1. |
|Estándar 2 (S2) | Similar a S1, pero con 100 GB/particiones (máximo 1,2 TB por servicio) |
|Estándar 3 (S3) | 200 GB por partición (máximo 2,4 TB por servicio) |
|Estándar 3 alta densidad (S3-HD) | Alta densidad es una *modo del alojamiento* para S3. El hardware subyacente está optimizado para un gran número de índices más pequeños, destinado a escenarios multiinquilino. S3 HD tiene el mismo cargo por unidad como S3, pero el hardware está optimizado para las lecturas de archivos rápidas en un gran número de índices más pequeños.|
|Almacenamiento optimizado 1 (L1) | 1 TB/partición (máximo 12 TB por servicio) |
|Almacenamiento optimizado 2 (L2) | 2 TB o partición (máximo 24 TB por servicio) |

> [!NOTE] 
> Los niveles de almacenamiento optimizado para ofrecen mayor capacidad de almacenamiento a un precio menor por cada TB que los niveles estándar. El principal inconveniente es mayor latencia de consulta, lo que debe validar para sus requisitos de aplicación específica.  Para obtener más información acerca de consideraciones de rendimiento de este nivel, vea [consideraciones de rendimiento y optimización](search-performance-optimization.md).
>

## <a name="how-billing-works"></a>Cómo funciona la facturación

En Azure Search, hay tres maneras de incurrir en costes en Azure Search y existen componentes fijos y variables. Esta sección se examinan a su vez en cada componente de facturación.

### <a name="1-core-service-costs-fixed-and-variable"></a>1. Costos de servicio de núcleo (fijos y variables)

Para el servicio, el cargo mínimo es la primera unidad de búsqueda (partición 1 réplica 1), y esta cantidad es fijo durante la vigencia del servicio porque el servicio no se puede ejecutar en cualquier cosa menor que esta configuración. 

Más allá de la mínima, puede agregar réplicas y particiones por separado. Por ejemplo, puede agregar sólo las réplicas o solo las particiones. Aumento incremental en capacidad a través de las réplicas y particiones constituye el componente de coste de la variable. 

La facturación se basa en un [fórmula (réplicas x particiones x velocidad)](#search-units). El plan de tarifa que seleccione depende de la tarifa que se aplica.

En la siguiente captura de pantalla, por unidad de precio se indica para gratis, básico y S1 (no se muestran S2, S3, L1 y L2). Si ha creado un **básica**, **estándar**, o **optimizadas para almacenamiento** servicio, el costo mensual sería el valor promedio que aparece para *precio-1*y *precio 2* respectivamente. Los costos de unidad ir para cada nivel porque la capacidad de almacenamiento y potencia computacional es mayor en cada nivel consecutivo. Las tarifas de Azure Search se publican en el [página de precios de Azure Search](https://azure.microsoft.com/pricing/details/search/).

![Por el precio por unidades](./media/search-sku-tier/per-unit-pricing.png "por unidad de precio")

Cuando el costo de una solución de búsqueda, tenga en cuenta que los precios y la capacidad no son lineales (lo que duplica capacidad más que duplica el costo). Para obtener un ejemplo de cómo de la fórmula funciona, consulte ["Cómo asignar particiones y réplicas"](search-capacity-planning.md#how-to-allocate-replicas-and-partitions).


### <a name="2-data-egress-charges-during-indexing"></a>2. Cargos de salida de datos durante la indización.

El uso de [indexadores de Azure Search](search-indexer-overview.md) puede dar lugar a facturación impacto según donde se encuentran los servicios. Puede eliminar los cargos de salida de datos completamente si crea el servicio Azure Search en la misma región que los datos. Los puntos siguientes son desde el [página de precios de ancho de banda](https://azure.microsoft.com/pricing/details/bandwidth/).

+ Microsoft no cobra para los datos entrantes a cualquier servicio de Azure, o para los datos de salida de Azure Search.

+ En soluciones de varios servicios, no hay ningún cargo por datos que atraviesan la conexión cuando todos los servicios están en la misma región.

Si hay servicios en regiones diferentes se aplican cargos por los datos salientes. Estos cargos no forman parte de la factura de Azure Search Sí, pero se mencionan aquí porque si usa datos o indexadores enriquecida de inteligencia artificial para extraer datos desde distintas regiones, podrá ver esos costos reflejados en su factura general. 

### <a name="3-ai-enriched-indexing-using-cognitive-services"></a>3. IA-indexación enriquecida con Cognitive Services

Para [AI indexación con Cognitive Services](cognitive-search-concept-intro.md), debe planear sobre cómo conectar un recurso de Cognitive Services facturable en el S0 plan de tarifa para el procesamiento de pago por uso. No hay ningún "costo fijo" asociado asociar Cognitive Services. Solo paga por los procesos que necesite.

Extracción de la imagen durante la averiguación de documentos es un cargo de Azure Search, que se facturan en función del número de imágenes que se extraen de los documentos. La extracción de texto es actualmente gratuita. 

Enriquecimientos de otros, como el procesamiento de lenguaje natural, se basan en [integrados conocimientos cognitivos](cognitive-search-predefined-skills.md) se facturan con un recurso de Cognitive Services, con la misma velocidad como si hubiera realiza la tarea mediante Cognitive Services directamente. Para obtener más información, consulte [adjuntar un recurso de Cognitive Services con un conjunto de habilidades](cognitive-search-attach-cognitive-services.md).

<a name="search-units"></a>

### <a name="billing-based-on-search-units"></a>Facturación según las unidades de búsqueda

Para las operaciones de Azure Search, el concepto de facturación más importante que se debe entender es la *unidad de búsqueda* (SU). Dado que Azure Search depende de las réplicas y las particiones para la indexación y las consultas, no tiene sentido facturar solo por una o la otra. En su lugar, la facturación se basa en una composición de ambas. 

Una SU es el producto de las *réplicas* y las *particiones* utilizadas por un servicio: **`(R X P = SU)`**

Cada servicio se inicia como mínimo con una SU (una réplica multiplicada por una partición). El máximo de cualquier servicio es de 36 SU, que se pueden lograr de varias maneras: 6 particiones por 6 réplicas, o 3 particiones por 12 réplicas, por nombrar algunas. Es habitual usar menos de la capacidad total. Por ejemplo, un servicio de 3 réplicas y 3 particiones, que se factura como 9 SU. Puede revisar [este gráfico](search-capacity-planning.md#chart) para ver las combinaciones válidas de un vistazo.

La tarifa de facturación es **por hora y por SU**, y cada nivel aumenta progresivamente a una tarifa más elevada. Los niveles superiores incluyen particiones mayores y más rápidas, que contribuyen a un precio por hora total superior para ese nivel. Las tarifas de cada nivel pueden consultarse en [Buscar Precios](https://azure.microsoft.com/pricing/details/search/). 

La mayoría de los clientes solo ponen en línea una parte de la capacidad total, manteniendo el resto en la reserva. En términos de facturación, es el número de particiones y réplicas que tiene en línea, calculado mediante la fórmula de la SU, lo que determina lo que paga realmente por hora.

### <a name="billing-for-image-extraction-in-cognitive-search"></a>Facturación por la extracción de imágenes en Cognitive Search

Si va a extraer imágenes de archivos de una canalización de indexación de Cognitive Search, se le cobrará esa operación en la factura de Azure Search. El parámetro que desencadena la extracción de imágenes es **imageAction** en una [configuración del indexador](https://docs.microsoft.com/rest/api/searchservice/create-indexer#indexer-parameters). Si **imageAction** está establecida en none (valor predeterminado), no hay ningún cargo por extracción de imágenes.

Los precios están sujetos a cambios, pero siempre se documentan en la página de los [detalles de los precios](https://azure.microsoft.com/pricing/details/search/) de Azure Search. 

### <a name="billing-for-built-in-skills-in-cognitive-search"></a>Facturación de las habilidades integradas en Cognitive Search

Al configurar una canalización de enriquecimiento, cualquier [habilidad integrada](cognitive-search-predefined-skills.md) que se use en ella se basa en modelos de aprendizaje automático. Estos modelos los proporciona Cognitive Services. El uso de los modelos durante la indexación se factura a la misma tarifa que si hubiera solicitado el recurso directamente.

Por ejemplo, supongamos una canalización con reconocimiento óptico de caracteres (OCR) en archivos JPEG de imagen escaneada, donde el texto resultante se inserta en un índice de Azure Search para las consultas de búsqueda de forma libre. La canalización de indexación incluiría un indexador con la [habilidad OCR](cognitive-search-skill-ocr.md), que se [asociaría a un recurso de Cognitive Services](cognitive-search-attach-cognitive-services.md). Al ejecutar el indexador aparecen cargos en la factura de recursos de Cognitive por la ejecución de OCR.

## <a name="tips-for-reducing-costs"></a>Sugerencias para reducir los costos

No se puede apagar el servicio para disminuir la factura. Los recursos operativos están operativos constantemente y se asignan para su uso exclusivo mientras el servicio esté vigente. La única manera de reducir una factura es reducir las réplicas y particiones a un nivel bajo que siga proporcionando un rendimiento aceptable y [cumpla con el Acuerdo de Nivel de Servicio](https://azure.microsoft.com/support/legal/sla/search/v1_0/). 

Un medio para reducir costos es elegir un nivel con una tarifa por hora más baja. Las tarifas por hora de S1 son inferiores a las tarifas de S2 o S3. Supongamos que aprovisiona un servicio cuyo objetivo es el extremo inferior de las proyecciones de la carga; si se superara el servicio, podría crear un segundo con capas mayores, volver a generar los índices en ese segundo servicio y eliminar el primero. 

Si ha realizado la planificación de la capacidad de los servidores locales, ya sabe que es común "seguir comprando" para poder asumir el crecimiento proyectado. Pero con un servicio en la nube, puede ahorrar de manera más significativa porque no está sujeto a una compra específica. Siempre puede cambiar a un servicio de nivel superior si el actual no es suficiente.

### <a name="capacity-drill-down"></a>Profundización en la capacidad

En Azure Search, la capacidad se estructura como *réplicas* y *particiones*. 

+ Las réplicas son instancias del servicio de búsqueda, donde cada réplica hospeda una copia con equilibrio de carga de un índice. Por ejemplo, un servicio con 6 réplicas tiene 6 copias de todos los índices cargados en el servicio. 

+ Las particiones almacenan índices y dividen automáticamente los datos de búsqueda: dos particiones dividen el índice en la mitad, tres particiones en tres tercios y así sucesivamente. En términos de capacidad, el *tamaño de partición* es la principal característica diferenciadora entre niveles.

> [!NOTE]
> Todos los **estándar** y **optimizadas para almacenamiento** planes de soporte técnico [particiones y réplicas de combinaciones flexibles](search-capacity-planning.md#chart) para que pueda [el sistema para acelerar el proceso de peso o almacenamiento](search-performance-optimization.md) cambiando el saldo. El nivel **Básico** ofrece hasta tres réplicas para alta disponibilidad, pero tiene solo una partición. Los niveles **Gratis** no proporcionan recursos dedicados: los recursos de cálculo se comparten entre varios suscriptores.

### <a name="more-about-service-limits"></a>Más información acerca de los límites de servicio

Los servicios hospedan recursos, como los índices, los indexadores y otros. Cada nivel impone [límites de servicio](search-limits-quotas-capacity.md) en la cantidad de recursos que puede crear. Por lo tanto, un límite en el número de índices (y otros objetos) es la segunda característica diferenciadora entre niveles. Cuando revise cada opción en el portal, tenga en cuenta los límites en cuanto al número de índices. Otros recursos, como los indexadores, los orígenes de datos y los conjuntos de habilidades, están asociados a los límites de los índices.

## <a name="consumption-patterns"></a>Patrones de consumo

La mayoría de clientes comienzan con la **gratis** de servicio, que mantienen indefinidamente y, a continuación, elija uno de la **estándar** o **almacenamiento optimizado** niveles para el desarrollo grave o cargas de trabajo de producción. 

![Niveles de Azure Search](./media/search-sku-tier/tiers.png "Planes de tarifa de Azure Search")

En cualquiera de los extremos, los niveles **Básico** y **S3 HD** existen para patrones de consumo importantes pero poco frecuentes. El nivel **Básico** es para pequeñas cargas de trabajo de producción: ofrece Acuerdo de Nivel de Servicio, recursos dedicados y alta disponibilidad, con un almacenamiento moderado, con un máximo de 2 GB. Este nivel se ha diseñado para clientes que están constantemente por debajo de la capacidad disponible. En el otro extremo, el nivel **S3 HD** es para cargas de trabajo típicas de ISV, asociados, [soluciones para varios inquilinos](search-modeling-multitenant-saas-applications.md) o cualquier configuración que precise de un gran número de índices pequeños. A menudo resulta evidente determinar si el nivel **Básico** o **S3 HD** es la opción adecuada, pero si desea confirmación también puede publicar mensajes en [StackOverflow](https://stackoverflow.com/questions/tagged/azure-search) o bien [póngase en contacto con el equipo de soporte técnico de Azure](https://azure.microsoft.com/support/options/) para más información.

Situando el foco en los niveles Estándar utilizados con mayor frecuencia, **S1-S3** son una progresión de niveles de capacidad en aumento, con puntos de inflexión en el tamaño de la partición y máximos en el número de índices, indexadores y recursos en consecuencia:

|  | S1 | S2 | S3 |  |  |  |  |
|--|----|----|----|--|--|--|--|
| tamaño de la partición|  25 GB | 100 GB | 200 GB |  |  |  |  |
| límites de índices e indexadores| 50 | 200 | 200 |  |  |  |  |

**S1** es una opción común cuando los recursos dedicados y varias particiones se convierten en una necesidad. Con particiones de 25 GB y hasta 12 particiones, el límite por servicio en el nivel **S1** es de 300 GB en total si maximiza las particiones con respecto a las réplicas (consulte [Asignación de particiones y réplicas](search-capacity-planning.md#chart) para composiciones más equilibradas).

Tanto el portal como las páginas de precios centran la atención en el tamaño de partición y el almacenamiento, pero para cada nivel, todas las funcionalidades de proceso (capacidad del disco, velocidad, CPU) crecen de modo lineal con el precio. Una réplica en **S2** es más rápida que la réplica en **S1** y en **S3** es más rápida que en **S2**. Los niveles **S3** rompen el patrón de proceso y precio habitualmente lineal con una E/S desproporcionadamente más rápida. Si prevé que la E/S será el cuello de botella, un nivel **S3** ofrece muchas más IOPS que los niveles inferiores.

**S3** y **S3 HD** están respaldados por la misma infraestructura de alta capacidad, pero cada uno de ellos alcanza su límite máximo de maneras diferentes. **S3** se dirige a un menor número de índices muy grandes. Como tal, su límite máximo es el límite de recursos (2,4 TB para cada servicio). **S3 HD** se dirige a un gran número de índices muy pequeños. A los 1000 índices, **S3 HD** alcanza sus límites en forma de restricciones de índice. Si es un cliente de **S3 HD** que requiere más de 1 000 índices, póngase en contacto con el soporte técnico de Microsoft para obtener información sobre cómo proceder.

> [!NOTE]
> Anteriormente, los límites de documentos se debían tener en cuenta, pero ya no son aplicables para la mayoría de los servicios nuevos. Para más información acerca de las condiciones para la que se aplican todavía límites de documentos, consulte [Límites del servicio: límites de documentos](search-limits-quotas-capacity.md#document-limits).
>

Capas de almacenamiento optimizado **L1 y L2**, son ideales para aplicaciones con requisitos de datos de gran tamaño, pero con un número relativamente bajo de los usuarios finales donde minimizar la latencia de consulta no es la principal prioridad.  

|  | L1 | L2 |  |  |  |  |  |
|--|----|----|--|--|--|--|--|
| tamaño de la partición|  1 TB | 2 TB |  |  |  |  |  |
| límites de índices e indexadores| 10 | 10 |  |  |  |  |  |

*L2* ofrece dos veces la capacidad de almacenamiento total para una *L1*.  Elija su plan según la cantidad máxima de datos que piensa que necesita el índice.  El *L1* capa de escalado crea particiones de en incrementos de 1 TB para un máximo de 12 TB, mientras que el *L2* aumentando de 2 TB por partición hasta un máximo de 24 TB.

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

1. [Revise los límites del servicio en cada nivel](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity#index-limits) para determinar si los niveles más bajos pueden admitir la cantidad de índices que necesita. En los niveles **Básico**-**S1**-**S2**, los límites de índices son 15-50-200, respectivamente.  El **optimizadas para almacenamiento** nivel tiene un límite de 10 índices, ya que es el diseñador para admitir un número reducido de índices muy grandes.

1. [Cree un servicio en un nivel facturable](search-create-service-portal.md):

    + Comience bajo, en **Básico** o **S1** si está al principio de la curva de aprendizaje.
    + Comience alto, en **S2** o incluso **S3**, si las cargas de consulta e indexación a gran escala son evidentes.
    + Almacenamiento optimizado, en **L1** o **L2**si indiza una gran cantidad de datos y carga de consultas es relativamente bajo, por ejemplo, una aplicación empresarial interna.

1. [Genere un índice inicial](search-create-index-portal.md) para determinar cómo se traducen los datos de origen a un índice. Esta es la única manera de calcular el tamaño del índice.

1. [Supervise el almacenamiento, los límites del servicio, el volumen de consultas y la latencia](search-monitor-usage.md) en el portal. El portal muestra las consultas por las consultas en segundo lugar, limitadas y latencia de búsqueda; todos los cuales pueden ayudarle a decidir si ha seleccionado el nivel correcto. Además de las métricas del portal, puede configurar la supervisión detallada, como el análisis del porcentaje de clics, habilitando el [análisis del tráfico de búsqueda](search-traffic-analytics.md). 

El número de índices y el tamaño son igualmente importantes para el análisis porque los límites máximos se alcanzan mediante la utilización total del almacenamiento (particiones) o por los límites máximos en los recursos (índices, indexadores, etc.), lo que ocurra primero. El portal le ayuda a realizar un seguimiento de ambos, mostrando el uso actual y los límites máximos en paralelo en la página Información general.

> [!NOTE]
> Los requisitos de almacenamiento pueden ser demasiado excesivos si los documentos contienen datos extraños. Idealmente, los documentos contienen solo los datos que necesita para la experiencia de búsqueda. Los datos binarios no son utilizables en búsquedas y deben almacenarse por separado (por ejemplo, en un almacenamiento de tablas o blobs de Azure) con un campo en el índice para mantener una referencia de URL a los datos externos. El tamaño máximo de un documento individual es 16 MB (o menos si carga en masa varios documentos en una sola solicitud). Consulte [Límites del servicio en Azure Search](search-limits-quotas-capacity.md) para más información.
>

**Consideraciones del volumen de consultas**

Las consultas por segundo (QPS) son una medida que gana importancia durante la optimización del rendimiento, pero no suele ser una consideración de nivel a menos que espere un volumen elevado de consultas desde el principio.

Los niveles Estándar pueden entregar un balance entre réplicas y particiones, al admitir una respuesta de consultas más rápida mediante réplicas adicionales para el equilibrio de carga y particiones adicionales para el procesamiento en paralelo. Puede ajustar el rendimiento después de aprovisionar el servicio.

Los clientes que esperan los volúmenes de consulta sostenida seguro desde el principio deben considerar mayor **estándar** niveles, respaldados por un hardware más eficaz. A continuación, puede desconectar particiones y réplicas o incluso cambiar a un servicio de nivel inferior, si dichos volúmenes de consultas no se materializan. Para más información sobre cómo calcular el rendimiento de las consultas, consulte [Rendimiento y optimización de Azure Search](search-performance-optimization.md).

El almacenamiento optimizado para niveles lean hacia las cargas de trabajo de datos de gran tamaño, que admiten más general almacenamiento de índices disponible, donde los requisitos de latencia de consulta son relativamente relajados.  Todavía se deben aprovechar réplicas adicionales para la carga de las particiones de equilibrio y adicionales para el procesamiento paralelo. Puede ajustar el rendimiento después de aprovisionar el servicio.

**Contratos de nivel de servicio**

El nivel **Gratis** y las características en versión preliminar no incluyen [contratos de nivel de servicio (SLA)](https://azure.microsoft.com/support/legal/sla/search/v1_0/). Para todos los niveles facturables, los SLA tomarán efecto cuando se aprovisione suficiente redundancia para el servicio. Son necesarias dos o más réplicas para el SLA de consulta (lectura). Son necesarias tres o más réplicas para el SLA de consulta e indexación (lectura y escritura). El número de particiones no es una consideración del SLA. 

## <a name="tips-for-tier-evaluation"></a>Sugerencias para la evaluación de nivel

+ Aprenda a crear índices eficaces y las metodologías de actualización con el menor impacto. Se recomienda el [análisis del tráfico de búsqueda](search-traffic-analytics.md) para los conocimientos adquiridos en la actividad de consulta.

+ Cree métricas sobre las consultas y recopile datos sobre los patrones de uso (consultas durante el horario laboral, indexación durante las horas sin picos) y utilice estos datos para informar en las futuras decisiones de aprovisionamiento del servicio. Aunque no resulta práctico en una cadencia horaria o diaria, puede ajustar dinámicamente las particiones y los recursos para ajustarse a los cambios planeados en el volumen de consultas o cambios no planeados pero sostenidos si los niveles se mantienen para justificar la toma de medidas.

+ Recuerde que la única desventaja de un aprovisionamiento inferior es que es posible que deba anular un servicio si los requisitos reales son mayores que los estimados. Para evitar la interrupción del servicio, se crea un nuevo servicio en la misma suscripción en un nivel superior y se ejecuta en paralelo hasta que todas las solicitudes y las aplicaciones se dirijan al nuevo punto de conexión.

## <a name="next-steps"></a>Pasos siguientes

Comience con un nivel **Gratis** y cree un índice inicial con un subconjunto de los datos para comprender sus características. La estructura de datos de Azure Search es un índice invertido, donde el tamaño y la complejidad de un índice invertido se determinan por el contenido. Recuerde que un contenido de gran redundancia tiende a producir un índice más pequeño que el contenido de alta irregularidad. Por lo tanto, son las características del contenido más que el tamaño del conjunto de datos lo que determina los requisitos de almacenamiento de índices.

Una vez que tenga una idea inicial del tamaño del índice, [aprovisionar un servicio facturable](search-create-service-portal.md) en uno de los niveles descritos en este artículo, ya sea **básica**, **estándar**, o **Optimizadas para almacenamiento** nivel. Relajar las restricciones sobre el tamaño de datos artificiales y [a generar el índice](search-howto-reindex.md) para incluir todos los datos que desea poder buscar.

[Asigne particiones y réplicas](search-capacity-planning.md) según sea necesario para obtener el rendimiento y la escala que necesite.

Si la capacidad y rendimiento son válidos, ya ha terminado. En caso contrario, vuelva a crear un servicio de búsqueda en otro nivel que se adapte más estrechamente a sus necesidades.

> [!NOTE]
> Para más ayuda con sus preguntas, puede publicarlas en [StackOverflow](https://stackoverflow.com/questions/tagged/azure-search) o bien [póngase en contacto con el equipo de soporte técnico de Azure](https://azure.microsoft.com/support/options/).
