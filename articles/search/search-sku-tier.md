---
title: 'Selección de SKU o plan de tarifa del servicio Azure Search: Azure Search'
description: 'Azure Search puede aprovisionarse en estas SKU: Gratis, Básico o Estándar; este último está disponible en varias configuraciones de recursos y niveles de capacidad.'
services: search
author: HeidiSteen
manager: cgronlun
tags: azure-portal
ms.service: search
ms.topic: conceptual
ms.date: 05/08/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 00422209302bbcc2139be4f6b490f0bb2816c051
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65539266"
---
# <a name="choose-a-pricing-tier-for-azure-search"></a>Selección de un plan de tarifa de Azure Search

Al crear un servicio Azure Search, [se crea un recurso](search-create-service-portal.md) en el plan de tarifa o SKU que se ha fijado para el período de vigencia del servicio. Los niveles incluyen Gratis, Básico, Estándar y Almacenamiento optimizado. Estándar y Almacenamiento optimizado están disponibles con varias configuraciones y capacidades.

La mayoría de los clientes empiezan con el nivel Gratis para poder evaluar el servicio. Después actualizan a uno de los niveles superiores para las implementaciones de desarrollo y producción. Si usa el nivel Gratis, puede completar todos los inicios rápidos y tutoriales, incluidos los de búsqueda cognitiva que consumen gran cantidad de recursos.

> [!NOTE]
> Actualmente, Microsoft proporciona el nivel Almacenamiento optimizado en una vista previa a un precio con descuento para pruebas y experimentación, con el fin de recabar comentarios de los usuarios. Los precios finales se anunciarán más adelante, cuando estos niveles estén disponibles con carácter general. Aconsejamos que no se usen estos niveles para aplicaciones de producción.

Los niveles reflejan las características del hardware que hospeda el servicio (en lugar de las características) y se diferencian por:

+ el número de índices que se pueden crear;
+ el tamaño y velocidad de particiones (almacenamiento físico).

Aunque todos los niveles, incluido el nivel Gratis, suelen ofrecer paridad de características, es posible que las cargas de trabajo más grandes necesiten niveles más altos. Por ejemplo, la [indexación de IA con Cognitive Services](cognitive-search-concept-intro.md) tiene funciones de ejecución prolongada que agotan el tiempo de espera en un servicio gratuito, a menos que el conjunto de datos sea pequeño.

> [!NOTE] 
> La excepción a la paridad de características reside en los [indizadores](search-indexer-overview.md), que no están disponibles en S3 HD.
>

Dentro de un nivel, también puede [ajustar los recursos de réplica y partición](search-capacity-planning.md) para aumentar o reducir la escala. Podría empezar con uno o dos de cada y, luego, aumentar temporalmente su capacidad de proceso para una elevada carga de trabajo de indexación. La posibilidad de ajustar los niveles de recursos dentro de un nivel agrega flexibilidad, pero también complica ligeramente su análisis. Es posible que tenga que hacer pruebas para ver si un nivel inferior con mayores recursos/réplicas ofrece mejor valor y rendimiento que un nivel superior con menos recursos. Para obtener más información acerca de cuándo y por qué debería ajustar la capacidad, consulte [Consideraciones de rendimiento y optimización](search-performance-optimization.md).

## <a name="tiers-for-azure-search"></a>Niveles para Azure Search

En esta tabla se muestran los niveles que hay disponibles. Encontrará más información sobre los distintos niveles en la [página de precios](https://azure.microsoft.com/pricing/details/search/) del artículo [Límites de servicio en Azure Search](search-limits-quotas-capacity.md) y en la página del portal cuando vaya a aprovisionar un servicio.

|Nivel | Capacity |
|-----|-------------|
|Gratuito | Compartido con otros suscriptores. No escalable. Limitado a 3 índices y 50 MB de almacenamiento. |
|Básica | Recursos de proceso dedicados para cargas de trabajo de producción a escala más pequeña. Una partición de 2 GB y hasta 3 réplicas. |
|Estándar 1 (S1) | Para S1 y niveles superiores, máquinas dedicadas con más almacenamiento y capacidad de procesamiento en cada nivel. Para S1, el tamaño de partición es 25 GB por partición (con un máximo de 300 GB por servicio). |
|Estándar 2 (S2) | Similar a S1, pero con las particiones de 100 GB (y un máximo de 1,2 TB por servicio). |
|Estándar 3 (S3) | Particiones de 200 GB (con un máximo de 2,4 TB por servicio). |
|Estándar 3 de alta densidad (S3 HD) | Alta densidad es un *modo de hospedaje* para S3. El hardware subyacente está optimizado para un gran número de índices más pequeños y está pensado para escenarios multiempresa. S3 HD tiene el mismo cargo por unidad que S3, pero el hardware está optimizado para lecturas de archivos rápidas en un gran número de índices más pequeños.|
|Almacenamiento optimizado 1 (L1) | Particiones de 1 TB (con un máximo de 12 TB por servicio). |
|Almacenamiento optimizado 2 (L2) | Particiones de 2 TB (con un máximo de 24 TB por servicio). |

> [!NOTE] 
> El nivel Almacenamiento optimizado ofrece mayor capacidad de almacenamiento a un precio menor por cada TB que el nivel Estándar. El principal inconveniente es una mayor latencia de consulta, lo cual debe validar para sus requisitos de aplicación específica.  Para saber más sobre las consideraciones de rendimiento de este nivel, vea [Estrategias de implementación y procedimientos recomendados para optimizar el rendimiento en Azure Search](search-performance-optimization.md).
>

## <a name="how-billing-works"></a>Cómo funciona la facturación

Hay tres maneras de incurrir en costos en Azure Search y existen componentes fijos y variables. En esta sección se describen los tres componentes de facturación: costos de servicio principal, cargos por salida de datos e indexación enriquecida con inteligencia artificial.

### <a name="core-service-costs-fixed-and-variable"></a>Costos de servicio principal (fijos y variables)

Por el servicio en sí, el cargo mínimo es la primera unidad de búsqueda (1 réplica x 1 partición). Este valor mínimo es fijo durante la vigencia del servicio porque el servicio no se puede ejecutar en ninguna otra configuración menor que esta.

Más allá del mínimo, puede agregar réplicas y particiones por separado. Por ejemplo, puede agregar solo réplicas o solo particiones. Los aumentos incrementales en capacidad a través de réplicas y particiones conforman el componente de costo variable.

La facturación se basa en una [fórmula (réplicas x particiones x tarifa)](#search-units). La tarifa que se aplica depende del plan de tarifa que seleccione.

En la captura de pantalla de abajo, se indican los precios por unidad para Gratis, Básico y S1 (no se muestran S2, S3, L1 y L2). Si crea un servicio Básico, el promedio del costo mensual será el valor que aparece para *price-1* (precio-1). Para un servicio Estándar, el promedio del costo mensual será el valor que aparece para *price-2* (precio-2). Los costos de la unidad aumentan para cada nivel porque la capacidad de almacenamiento y la potencia computacional es mayor en cada nivel consecutivo. Las tarifas de Azure Search están disponibles en la [página de precios de Azure Search](https://azure.microsoft.com/pricing/details/search/).

![Precio por unidad](./media/search-sku-tier/per-unit-pricing.png "Per-unit pricing")

Cuando esté calculando el costo de una solución de búsqueda, tenga en cuenta que los precios y la capacidad no son lineales (cuanto más se duplica la capacidad, más se duplica el costo). Para ver un ejemplo de cómo funciona la fórmula, eche un vistazo a [Cómo asignar particiones y réplicas](search-capacity-planning.md#how-to-allocate-replicas-and-partitions).

#### <a name="billing-based-on-search-units"></a>Facturación según las unidades de búsqueda

El concepto de facturación más importante que se debe entender para las operaciones de Azure Search es la *unidad de búsqueda* (SU). Dado que Azure Search depende de las réplicas y las particiones para la indexación y las consultas, no tiene sentido facturar solo por una o la otra. En su lugar, la facturación se basa en una composición de ambas.

Una SU es el producto de las *réplicas* y las *particiones* usadas por un servicio: **(R x P = SU)** .

Cada servicio se inicia como mínimo con una SU (una réplica multiplicada por una partición). El máximo para cualquier servicio es 36 unidades de búsqueda. Este valor máximo se puede conseguir de varias maneras: por ejemplo, 6 particiones por 6 réplicas, o 3 particiones por 12 réplicas. Es habitual usar menos capacidad total (por ejemplo, un servicio de 3 réplicas y 3 particiones facturado como 9 SU). Vea el gráfico [Combinaciones de particiones y réplicas](search-capacity-planning.md#chart) para ver las combinaciones válidas.

La tarifa de facturación es de una hora por unidad. Cada nivel tiene una tarifa mayor de forma progresiva. Los niveles superiores incluyen particiones mayores y más rápidas, y esto contribuye a un total superior para ese nivel. Puede ver las tarifas de cada nivel en la página [detalles de precios](https://azure.microsoft.com/pricing/details/search/).

La mayoría de los clientes solo ponen en línea una parte de la capacidad total, manteniendo el resto en la reserva. Para la facturación, el número de particiones y réplicas que tiene en línea, calculado mediante la fórmula de SU, determina lo que se paga por hora.

### <a name="data-egress-charges-during-indexing"></a>Cargos de salida de datos durante la indexación

El uso de [indexadores de Azure Search](search-indexer-overview.md) podría afectar a la facturación, según la ubicación de los servicios. Puede eliminar por completo los cargos de salida de datos si crea el servicio Azure Search en la misma región que los datos. Para saber más, eche un vistazo a la [página de precios de ancho de banda](https://azure.microsoft.com/pricing/details/bandwidth/):

+ Microsoft no cobra por los datos de entrada a ningún servicio de Azure, ni por los datos de salida de Azure Search.

+ En soluciones de varios servicios, no hay ningún cargo por los datos que atraviesan la conexión cuando todos los servicios están en la misma región.

Si hay servicios en regiones diferentes, se aplican cargos por los datos de salida. Estos cargos realmente no forman parte de la factura de Azure Search. Se mencionan aquí porque si usa datos o indexadores enriquecidos con inteligencia artificial para extraer datos de distintas regiones, verá los costos reflejados en su factura general.

### <a name="ai-enriched-indexing-with-cognitive-services"></a>Indexación enriquecida con inteligencia artificial con Cognitive Services

Para la [indexación de inteligencia artificial con Cognitive Services](cognitive-search-concept-intro.md), debe planear asociar un recurso de Azure Cognitive Services facturable, en la misma región que Azure Search, en el plan de tarifa S0 para el procesamiento de pago por uso. No hay ningún costo fijo por asociar Cognitive Services. Solo se paga por el procesamiento que necesita.

La extracción de imágenes durante el descifrado de documentos es un cargo de Azure Search. Se factura según el número de imágenes que se extraen de los documentos. La extracción de texto es actualmente gratuita.

Los demás enriquecimientos, como el procesamiento de lenguaje natural, están basados en [habilidades cognitivas predefinidas](cognitive-search-predefined-skills.md) y se factura con un recurso de Cognitive Services. Se facturan con la misma tarifa que si hubiera realizado la tarea usando Cognitive Services directamente. Para más información, vea [Asociación de un recurso de Cognitive Services con un conjunto de aptitudes](cognitive-search-attach-cognitive-services.md).

<a name="search-units"></a>

#### <a name="billing-for-image-extraction-in-cognitive-search"></a>Facturación por la extracción de imágenes en Cognitive Search

Si extrae imágenes de archivos de una canalización de indexación de Cognitive Search, dicha operación se le cobrará en la factura de Azure Search. En una [configuración de indexador](https://docs.microsoft.com/rest/api/searchservice/create-indexer#indexer-parameters), **imageAction** es el parámetro que desencadena la extracción de imágenes. Si **imageAction** está establecido en "none" (valor predeterminado), no se le cobrará por la extracción de imágenes.

Los precios están sujetos a cambios. Está documentado en la página de [detalles de precios](https://azure.microsoft.com/pricing/details/search/) de Azure Search.

#### <a name="billing-for-built-in-skills-in-cognitive-search"></a>Facturación de las habilidades integradas en Cognitive Search

Al configurar una canalización de enriquecimiento, cualquier [habilidad integrada](cognitive-search-predefined-skills.md) que se use en ella se basa en modelos de aprendizaje automático. Estos modelos los proporciona Cognitive Services. Si usa estos modelos durante la indexación, se le facturará con la misma tarifa que si hubiera pedido el recurso directamente.

Por ejemplo, supongamos que tienen una canalización con reconocimiento óptico de caracteres (OCR) en archivos JPEG escaneados y el texto resultante se inserta en un índice de Azure Search para las consultas de búsqueda de forma libre. La canalización de indexación incluiría un indexador con la [habilidad OCR](cognitive-search-skill-ocr.md), que se [asociaría a un recurso de Cognitive Services](cognitive-search-attach-cognitive-services.md). Al ejecutar el indexador, aparecen cargos por la ejecución de OCR en la factura de Cognitive Services.

## <a name="tips-for-reducing-costs"></a>Sugerencias para reducir los costos

No se puede cerrar el servicio para reducir la factura. Los recursos dedicados siempre están operativos y se asignan para su uso exclusivo mientras el servicio esté vigente. La única manera de reducir el importe de una factura es reducir las réplicas y las particiones a un nivel que siga proporcionando un rendimiento aceptable y [cumpla con el contrato de nivel de servicio](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

Una forma de reducir los costos es elegir un nivel con una tarifa por hora menor. Las tarifas por hora de S1 son inferiores a las tarifas de S2 o S3. Supongamos que aprovisiona su servicio con la menor de las previsiones de carga; si se supera el servicio, puede crear un segundo servicio con un nivel mayor, regenerar los índices en el segundo servicio y eliminar el primero.

Si ha hecho el planeamiento de capacidad para los servidores locales, ya sabe que es común "comprar servicios ampliados" para poder asumir el crecimiento previsto. Con un servicio en la nube, puede ahorrar de manera más significativa porque no está sujeto a una compra específica. Siempre puede cambiar a un servicio de nivel superior si el actual no es suficiente.

### <a name="capacity"></a>Capacity

En Azure Search, la capacidad se estructura como *réplicas* y *particiones*.

+ Las réplicas son instancias del servicio de búsqueda. Cada réplica hospeda una copia de carga equilibrada de un índice. Por ejemplo, un servicio con seis réplicas tiene seis copias de todos los índices cargados en el servicio.

+ Las particiones almacenan índices y dividen automáticamente los datos utilizables en búsquedas. Dos particiones dividen el índice por la mitad; tres particiones lo dividen en tercios y así sucesivamente. En términos de capacidad, el *tamaño de partición* es la principal característica diferenciadora entre niveles.

> [!NOTE]
> Todos los niveles Estándar y Almacenamiento optimizado admiten [combinaciones flexibles de réplicas y particiones](search-capacity-planning.md#chart) para que pueda [optimizar el sistema para velocidad o para almacenamiento](search-performance-optimization.md) con solo cambiar el equilibrio. El nivel Básico ofrece hasta tres réplicas para alta disponibilidad, pero solo tiene una partición. El nivel Gratis no proporciona recursos dedicados: los recursos de cálculo se comparten entre varios suscriptores.

### <a name="more-about-service-limits"></a>Más información acerca de los límites de servicio

Los servicios hospedan recursos, como índices e indexadores. Cada nivel impone [límites de servicio](search-limits-quotas-capacity.md) en la cantidad de recursos que puede crear. Por eso, el número máximo de índices (y otros objetos) es la segunda característica diferenciadora entre niveles. Cuando revise cada opción en el portal, tenga en cuenta los límites en cuanto al número de índices. Otros recursos, como los indexadores, los orígenes de datos y los conjuntos de aptitudes, se adjuntan a los límites de los índices.

## <a name="consumption-patterns"></a>Patrones de consumo

La mayoría de los clientes empieza con el servicio Gratis, lo mantienen indefinidamente y luego eligen el nivel Estándar o Almacenamiento optimizado para cargas de trabajo de desarrollo o de producción relevantes.

![Planes de tarifa de Azure Search](./media/search-sku-tier/tiers.png "Azure Search pricing tiers")

En todas las gamas, los niveles Básico y S3 HD son para patrones de consumo importantes pero poco habituales. El nivel Básico es para pequeñas cargas de trabajo de producción. Ofrece contratos de nivel de servicio, recursos dedicados y alta disponibilidad, pero proporciona un almacenamiento modesto con un máximo de 2 GB. Este nivel se ha diseñado para clientes cuyo uso está constantemente por debajo de la capacidad disponible. En la gama alta, el nivel S3 HD es para cargas de trabajo típicas de ISV, asociados, [soluciones multiinquilino](search-modeling-multitenant-saas-applications.md) o cualquier configuración que necesite un gran número de índices pequeños. Suele ser evidente cuándo es más apropiado usar el nivel Básico y cuándo usar el nivel S3 HD. Si le gustaría pedir confirmación, puede publicar en [StackOverflow](https://stackoverflow.com/questions/tagged/azure-search) o [ponerse en contacto con el Soporte técnico de Azure](https://azure.microsoft.com/support/options/) para que le aconsejen.

Los niveles Estándar que se usan con más frecuencia, del S1 al S3, forman una progresión de niveles de capacidad que van en aumento. Hay puntos de inflexión en el tamaño de la partición y en los límites del número de índices, indizadores y recursos corolarios:

|  | S1 | S2 | S3 |  |  |  |  |
|--|----|----|----|--|--|--|--|
| Tamaño de la partición|  25 GB | 100 GB | 200 GB |  |  |  |  |
| Límites de índices e indexadores| 50 | 200 | 200 |  |  |  |  |

S1 es una opción común para clientes que necesitan recursos dedicados y varias particiones. S1 ofrece particiones de 25 GB y hasta 12 particiones, lo que proporciona un límite por servicio de 300 GB si maximiza las particiones a través de réplicas. (Vea [Asignar particiones y réplicas](search-capacity-planning.md#chart) para lograr unas asignaciones más equilibradas).

Tanto el portal como las páginas de precios centran la atención en el tamaño de partición y el almacenamiento, pero para cada nivel, todas las funciones de proceso (capacidad del disco, velocidad, CPU) suelen crecer de modo lineal con el precio. Una réplica en S2 es más rápida que la réplica en S1, y una en S3 es más rápida que en S2. Los niveles S3 rompen el patrón de proceso y precio lineal con una E/S desproporcionadamente más rápida. Si cree que la E/S será el cuello de botella, tenga en cuenta que puede obtener mucho más IOPS con S3 que con los niveles inferiores.

S3 y S3 HD están respaldados por la misma infraestructura de alta capacidad, pero cada uno de ellos alcanza su límite máximo de maneras diferentes. S3 tiene como destino un menor número de índices muy grandes, por lo que su límite máximo es el límite de recursos (2,4 TB para cada servicio). S3 HD se dirige a un gran número de índices muy pequeños. A los 1000 índices, S3 HD alcanza sus límites en forma de restricciones de índice. Si es un cliente de S3 HD que necesita más de 1000 índices, póngase en contacto con el Soporte técnico de Microsoft para saber lo que debe hacer.

> [!NOTE]
> Antes, había que tener en cuenta los límites de documentos, pero esta consideración ya no se aplica para la mayoría de los servicios nuevos. Para más información sobre las condiciones para la que se siguen aplicando límites de documentos, vea [Límites de documentos](search-limits-quotas-capacity.md#document-limits).
>

Los niveles Almacenamiento optimizado, L1 y L2, son ideales para aplicaciones con requisitos de datos muy exigentes pero con una cantidad relativamente baja de usuarios finales, donde minimizar la latencia de consulta no es la principal prioridad.  

|  | L1 | L2 |  |  |  |  |  |
|--|----|----|--|--|--|--|--|
| Tamaño de la partición|  1 TB | 2 TB |  |  |  |  |  |
| Límites de índices e indexadores| 10 | 10 |  |  |  |  |  |

L2 ofrece el doble de capacidad de almacenamiento general de L1.  Elija el nivel según la cantidad máxima de datos que cree que necesita el índice. Las particiones de nivel L1 escalan verticalmente en incrementos de 1 TB hasta un máximo de 12 TB. Las particiones de nivel L2 aumentan 2 TB por partición hasta llegar a un máximo de 24 TB.

## <a name="evaluating-capacity"></a>Evaluar la capacidad

La capacidad y los costos de ejecutar el servicio están directamente relacionados. Los niveles imponen límites tanto en el almacenamiento como en los recursos. Debe tener en cuenta estos dos aspectos, porque el límite real será el que se alcance primero.

Los requisitos empresariales suelen imponer el número de índices que se necesitará. Por ejemplo, podría necesitar un índice global para un repositorio de documentos de gran tamaño. O podría necesitar varios índices en función de la región, la aplicación o el nicho de negocio.

Para determinar el tamaño de un índice, tendrá que [crear uno](search-create-index-portal.md). La estructura de datos de Azure Search es principalmente una estructura de [índice invertido](https://en.wikipedia.org/wiki/Inverted_index), que tiene características diferentes que los datos de origen. Para un índice invertido, el tamaño y la complejidad vienen determinados por el contenido, y no necesariamente por la cantidad de datos que se incorporan. Un origen de datos de gran tamaño con mucha redundancia podría dar lugar a un índice más pequeño que un conjunto de datos más pequeño que incluya contenido muy variable. Así que es poco probable deducir el tamaño del índice en función del tamaño del conjunto de datos original.

> [!NOTE] 
> Aunque el cálculo de las necesidades futuras de índices y almacenamiento se hace a partir de suposiciones, vale la pena hacerlo. Si la capacidad de un nivel resulta demasiado baja, tendrá que aprovisionar un nuevo servicio en un nivel superior y luego [volver a cargar los índices](search-howto-reindex.md). No existe ninguna actualización local de un servicio de una SKU a otra.
>

### <a name="step-1-develop-rough-estimates-by-using-the-free-tier"></a>Paso 1: Desarrollar estimaciones aproximadas con el nivel Gratis

Un enfoque para calcular la capacidad es empezar con el nivel Gratis. Recuerde que el servicio Gratis ofrece hasta 3 índices, 50 MB de almacenamiento y 2 minutos de tiempo de indexación. Puede resultar complicado calcular un tamaño de índice proyectado con estas restricciones. Esta sería una forma de hacerlo:

+ [Cree un servicio gratuito](search-create-service-portal.md).
+ Preparar un conjunto de datos pequeño y representativo (por ejemplo, 5000 documentos y un tamaño de muestra del 10 %).
+ [Genere un índice inicial](search-create-index-portal.md) y anote su tamaño en el portal (por ejemplo, 30 MB).

Si la muestra es representativa y es el 10 % del origen de datos completo, un índice de 30 MB se convierte aproximadamente en uno de 300 MB si se indexan todos los documentos. Use esta cantidad preliminar como base para doblar esa cantidad para poder incluir dos índices (un para desarrollo y otro para producción). Esto le dará un total de 600 MB en requisitos de almacenamiento. Este requisito se cumple fácilmente con el nivel Básico, así que ahí es por donde debe empezar.

### <a name="step-2-develop-refined-estimates-by-using-a-billable-tier"></a>Paso 2: Desarrollar estimaciones precisas mediante un nivel facturable

Algunos clientes prefieren empezar con recursos dedicados que pueden adaptarse a mayores tiempos de muestreo y procesamiento y, después, realizar una estimación realista de volúmenes de consultas, tamaño y cantidad de índices durante el desarrollo. En un principio, un servicio se aprovisiona según un cálculo aproximado. Después, a medida que el proyecto evoluciona, los equipos suelen saber si el servicio actual está por encima o por debajo de la capacidad necesaria para las cargas de trabajo de producción previstas.

1. [Revise los límites del servicio en cada nivel](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity#index-limits) para determinar si los niveles más bajos pueden admitir la cantidad de índices que necesita. En los niveles Básico, S1 y S2, los límites de índices son 15, 50 y 200 respectivamente. El nivel Almacenamiento optimizado tiene un límite de 10 índices porque se ha diseñado para admitir un número reducido de índices muy grandes.

1. [Cree un servicio en un nivel facturable](search-create-service-portal.md):

    + Empiece por abajo, con un nivel Básico o S1, si está al principio de la curva de aprendizaje.
    + Empiece por arriba, con un nivel S2 o incluso S3, si sabe que va a tener cargas de consulta e indexación a gran escala.
    + Empiece con Almacenamiento optimizado, en L1 o L2, si va a indexar una gran cantidad de datos y la carga de consultas es relativamente baja, como con una aplicación empresarial interna.

1. [Genere un índice inicial](search-create-index-portal.md) para determinar cómo se traducen los datos de origen a un índice. Esta es la única manera de calcular el tamaño del índice.

1. [Supervise el almacenamiento, los límites del servicio, el volumen de consultas y la latencia](search-monitor-usage.md) en el portal. El portal muestra las consultas por segundo, las consultas limitadas y la latencia de búsqueda. Todos estos valores pueden ayudarle a decidir si ha seleccionado el nivel correcto. También puede habilitar el [análisis del tráfico de búsqueda](search-traffic-analytics.md) para configurar la supervisión detallada de valores como el análisis del porcentaje de clics.

El tamaño y la cantidad de índices son igualmente importantes para el análisis. Esto es así porque se alcanzan los límites máximos al usar por completo el almacenamiento (particiones) o al agotar todos los recursos (índices, indexadores, etc.), lo que ocurra primero. El portal le ayuda a realizar un seguimiento de ambos, mostrando el uso actual y los límites máximos en paralelo en la página Información general.

> [!NOTE]
> Los requisitos de almacenamiento pueden parecer excesivos si los documentos contienen datos extraños. Lo ideal es que los documentos contengan solo los datos que necesita para la experiencia de búsqueda. No es posible realizar búsquedas en datos binarios, así que deben almacenarse por separado (quizás en Azure Blob Storage o en una tabla de Azure). Después hay que agregar un campo en el índice para mantener una referencia de URL a los datos externos. El tamaño máximo de un documento individual es 16 MB (o menos si va a cargar de forma masiva varios documentos en una sola solicitud). Para más información, consulte [Límites de servicio en Azure Search](search-limits-quotas-capacity.md).
>

**Consideraciones del volumen de consultas**

Las consultas por segundo (QPS) son una medida importante durante la optimización del rendimiento, pero solo suele ser una consideración de nivel si espera un volumen elevado de consultas desde el principio.

El nivel Estándar puede proporcionar un equilibrio entre réplicas y particiones. Puede aumentar el tiempo de respuesta de las consultas mediante la adición de réplicas para el equilibrio de carga o agregar particiones para el procesamiento paralelo. Luego puede ajustar el rendimiento después de aprovisionar el servicio.

Si prevé grandes volúmenes de consultas sostenidos desde el principio, considere la opción de los niveles Estándar superiores, respaldados por un hardware más potente. Luego puede desconectar particiones y réplicas, o incluso cambiar a un servicio de nivel inferior, si no llega a tener los volúmenes de consultas previstos. Para más información sobre cómo calcular el rendimiento de las consultas, consulte [Rendimiento y optimización de Azure Search](search-performance-optimization.md).

El nivel Almacenamiento optimizado es útil para cargas de trabajo de datos de gran tamaño, ya que admite más almacenamiento de índice general disponible para cuando los requisitos de latencia de consulta sean menos importantes. Tendrá que seguir usando réplicas adicionales para el equilibrio de carga y particiones adicionales para el procesamiento paralelo. Luego puede ajustar el rendimiento después de aprovisionar el servicio.

**Contratos de nivel de servicio**

El nivel Gratis y las características en vista previa no ofrecen [contratos de nivel de servicio (SLA)](https://azure.microsoft.com/support/legal/sla/search/v1_0/). Para todos los niveles facturables, los SLA tomarán efecto cuando se aprovisione suficiente redundancia para el servicio. Necesitará dos o más réplicas para los SLA de consulta (lectura). Necesitará tres o más réplicas para los SLA de consulta e indización (lectura-escritura). El número de particiones no afecta a los SLA.

## <a name="tips-for-tier-evaluation"></a>Sugerencias para la evaluación de nivel

+ Aprenda a crear índices eficientes y conozca qué métodos de actualización tendrán un impacto menor. Use el [análisis del tráfico de búsqueda](search-traffic-analytics.md) para obtener información sobre la actividad de consultas.

+ Cree métricas sobre las consultas y recopile datos sobre los patrones de uso (consultas durante el horario laboral, indexación durante las horas de menor uso). Use estos datos para respaldar las decisiones de aprovisionamiento del servicio. Aunque no resulta práctico en una cadencia horaria o diaria, puede ajustar dinámicamente las particiones y los recursos para adaptar los cambios planeados en el volumen de consultas. También puede adaptar los cambios no planeados pero sostenidos si los niveles se mantienen lo suficiente para garantizar que se puedan realizar acciones.

+ Recuerde que la única desventaja de un aprovisionamiento inferior es que es posible que deba anular un servicio si los requisitos reales son mayores que los que había previsto. Para evitar la interrupción del servicio, se crea un nuevo servicio en la misma suscripción en un nivel superior y se ejecuta en paralelo hasta que todas las solicitudes y las aplicaciones se dirijan al nuevo punto de conexión.

## <a name="next-steps"></a>Pasos siguientes

Comience con un nivel Gratis y cree un índice inicial usando un subconjunto de los datos para comprender sus características. La estructura de datos de Azure Search es una estructura de índice invertido. El tamaño y la complejidad de un índice invertido vienen determinados por el contenido. Recuerde que un contenido de gran redundancia tiende a producir un índice más pequeño que el contenido de alta irregularidad. Por tanto, son las características del contenido más que el tamaño del conjunto de datos lo que determina los requisitos de almacenamiento de índices.

Una vez que tenga un cálculo inicial del tamaño del índice, [aprovisione un servicio facturable](search-create-service-portal.md) en uno de los niveles que hemos descrito en este artículo: Básico, Estándar o Almacenamiento optimizado. Rebaje cualquier restricción artificial sobre el tamaño de los datos y [recompile el índice](search-howto-reindex.md) para que incluya todos los datos sobre los que quiera realizar búsquedas.

[Asigne particiones y réplicas](search-capacity-planning.md) según sea necesario para obtener el rendimiento y la escala que necesite.

Si la capacidad y el rendimiento son válidos, ya estará listo. En caso contrario, vuelva a crear un servicio de búsqueda en otro nivel que se adapte más estrechamente a sus necesidades.

> [!NOTE]
> Si tiene alguna duda, puede publicarla en [StackOverflow](https://stackoverflow.com/questions/tagged/azure-search) o [ponerse en contacto con el Soporte técnico de Azure](https://azure.microsoft.com/support/options/).
