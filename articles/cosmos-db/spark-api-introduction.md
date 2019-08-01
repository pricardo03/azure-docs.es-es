---
title: Introducción al análisis operativo en tiempo real integrado en Azure Cosmos DB con Apache Spark
description: Aprenda a usar la compatibilidad integrada con Apache Spark en Azure Cosmos DB para ejecutar análisis operativos en tiempo real e IA
ms.service: cosmos-db
ms.topic: overview
ms.date: 05/23/2019
author: rimman
ms.author: rimman
ms.openlocfilehash: 245df0632765c4000bdf5da3e428187d2b068866
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2019
ms.locfileid: "68402103"
---
# <a name="built-in-operational-analytics-in-azure-cosmos-db-with-apache-spark-preview"></a>Análisis operativo en tiempo real integrado en Azure Cosmos DB con Apache Spark (versión preliminar) 

La compatibilidad integrada con Apache Spark en Azure Cosmos DB permite ejecutar análisis de Apache Spark con los datos almacenados en una cuenta de Azure Cosmos. Proporciona la compatibilidad nativa para ejecutar directamente los trabajos de Apache Spark en las bases de datos distribuidas globalmente de Cosmos. Con estas funcionalidades, los desarrolladores, ingenieros de datos y científicos de datos pueden usar Azure Cosmos DB como una plataforma de datos flexible, escalable y de alto rendimiento para ejecutar las cargas de trabajo de **OLTP y OLAP/HTAP**. 

El proceso de Spark está disponible automáticamente en todas las regiones de Azure asociadas con su cuenta de Azure Cosmos. Los trabajos de Spark emplean la funcionalidad de arquitectura multimaestro de Azure Cosmos DB y se pueden escribir o consultar en las réplicas locales de cada región. 

> [!NOTE]
> La compatibilidad integrada con Apache Spark en Azure Cosmos DB está actualmente en versión preliminar limitada. Para registrarse para la versión preliminar, vaya a la [página de registro para la versión preliminar](https://portal.azure.com/?feature.customportal=false#create/Microsoft.DocumentDB). 

La compatibilidad con Apache Spark en Azure Cosmos DB ofrece las siguientes ventajas:

* Puede obtener información para datos y usuarios geográficamente distribuidos en el menor tiempo posible.

* Puede simplificar la arquitectura de la solución y reducir el [costo total de propiedad](total-cost-ownership.md) (TCO). El sistema tiene el mínimo número de componentes de procesamiento de datos, y evita los movimientos de datos innecesarios entre ellos.

* Crea límites de [seguridad](secure-access-to-data.md), [cumplimiento](compliance.md)y auditoría que abarcan todos los datos bajo la administración.

* Proporciona análisis de usuario final "siempre disponibles" o de [alta disponibilidad](high-availability.md) que están respaldados por rigurosos Acuerdos de Nivel de Servicio.

![Visualización de la compatibilidad con Apache Spark en Azure Cosmos DB](./media/spark-api-introduction/spark-api-visualization.png)
 
Mediante la compatibilidad con Apache Spark en Azure Cosmos DB puede compilar e implementar soluciones como la inteligencia artificial y modelos aprendizaje profundo, el análisis predictivo, recomendaciones, IoT, Customer 360, detección de fraudes, opiniones de texto y análisis clickstream. Dichas soluciones trabajan directamente con los datos de Azure Cosmos DB.

Puede configurar por lotes y transmitir en secuencias un trabajo de extracción, transformación y carga de datos (ETL) en Azure Cosmos DB, sin tener que salir del servicio de base de datos o agregar más servicios de proceso. Puede escalar de forma elástica el entorno de proceso cuando tenga que realizar trabajos ETL y volver a reducir la escala una vez que se haya realizado el trabajo.

La compatibilidad con Apache Spark en Azure Cosmos DB ofrece compatibilidad integrada con Machine Learning en los runtimes de Apache Spark. Los tiempos de ejecución incluyen Spark MLLib, Microsoft Machine Learning para Spark, Azure Machine Learning y Cognitive Services. Con estas características, los científicos de datos, ingenieros de datos y analistas de datos pueden generar y poner en marcha modelos de Machine Learning directamente dentro de Azure Cosmos DB, en una fracción del tiempo y con un bajo costo.


## <a name="key-benefits"></a>Ventajas principales

### <a name="globally-distributed-low-latency-operational-analytics-and-ai"></a>IA y análisis operativos de baja latencia distribuidos globalmente

Con Apache Spark en la base de datos distribuida globalmente de Azure Cosmos, ahora puede obtener un rápido acceso a los datos en todo el mundo. Azure Cosmos DB habilita **análisis operativos de baja latencia distribuidos globalmente** a escala elástica con tres técnicas clave:

* Puesto que la base de datos de Azure Cosmos está distribuida globalmente, la ingesta de todos los datos se produce localmente en donde se encuentren los productores de los datos (por ejemplo, los usuarios). Las consultas se sirven con las réplicas locales más cercanas tanto a los productores como a los consumidores de datos, independientemente de dónde se encuentren en el mundo. 

* Todas las consultas analíticas se ejecutan directamente en los datos almacenados indexados dentro de las particiones de datos sin que se realice ningún movimiento innecesario de datos. 

* Dado que Spark comparte ubicación con Azure Cosmos DB, hay menos traslaciones intermedias y movimientos de datos, lo que resulta en un mejor rendimiento y escalabilidad.

### <a name="unified-serverless-experience-for-apache-spark"></a>Experiencia sin servidor unificada para Apache Spark

Como base de datos multimodelo, Azure Cosmos DB ahora amplía su compatibilidad para las OSS API proporcionando una **experiencia sin servidor unificada para Apache Spark** con modelos de datos de familia de columnas de pares clave-valor, documento, grafos. Hay compatibilidad con diferentes modelos de datos mediante el uso de MongoDB, Cassandra, Gremlin, Etcd, y las SQL API, todos funcionando con los mismos datos subyacentes. 

Con la compatibilidad con Apache Spark en Azure Cosmos DB, puede admitir de forma nativa aplicaciones escritas en Scala, Python, Java y usar varias bibliotecas estrechamente integradas para SQL. Estas bibliotecas incluyen ([Spark SQL](https://spark.apache.org/sql/)), aprendizaje automático (Spark [MLlib](https://spark.apache.org/mllib/)), procesamiento de flujos ([Spark Structured Streaming](https://spark.apache.org/streaming/)) y procesamiento de grafos (Spark [GraphFrames]( https://docs.databricks.com/spark/latest/graph-analysis/graphframes/user-guide-python.html)). Estas herramientas facilitan la utilización de Apache Spark para varios casos de uso. No tiene que ocuparse de administrar Spark o los clústeres de Spark. Puede usar las Apache Spark API que le son familiares y las instancias de **Jupyter Notebook** para el análisis, y SQL API o cualquiera de las OSS NoSQL API como Cassandra para el procesamiento de transacciones en los mismos datos subyacentes al mismo tiempo.

### <a name="no-schema-or-index-management"></a>Sin administración de índices o esquemas

A diferencia de las bases de datos analíticas tradicionales, con Azure Cosmos DB, los ingenieros y científicos de datos ya no tienen que ocuparse del engorro que supone la administración de esquemas e índices. El motor de base de datos de Azure Cosmos DB no requiere ninguna administración de esquemas o índices explícita, y es capaz de indexar automáticamente todos los datos que ingiere para atender rápidamente las consultas de Apache Spark. 

### <a name="consistency-choices"></a>Opciones de coherencia

Teniendo en cuenta que los trabajos de Apache Spark se ejecutan en las particiones de datos de la base de datos de Azure Cosmos, las consultas tendrán las [cinco opciones de coherencia bien definidas](consistency-levels.md). Estos modelos de coherencia proporcionan la flexibilidad de elegir una coherencia estricta para proporcionar los resultados más precisos para los algoritmos de aprendizaje automático, sin poner en peligro la latencia y alta disponibilidad. 

### <a name="comprehensive-slas"></a>Contratos de nivel de servicio completos

Los trabajos de Apache Spark tendrán las ventajas de Azure Cosmos DB como los [Acuerdos de Nivel de Servicio](https://azure.microsoft.com/support/legal/sla/documentdb/v1_1/) más completos del sector (99,999) sin ninguna sobrecarga de administración de clústeres de Apache Spark independientes. Estos SLA incluyen: rendimiento, baja latencia en el percentil 99, coherencia y alta disponibilidad. 

### <a name="mixed-workloads"></a>Cargas de trabajo mixtas

La integración de Apache Spark en Azure Cosmos DB salva las diferencias en la separación analítica y transaccional, que ha sido uno de los principales puntos problemáticos para los clientes a la hora de compilar aplicaciones nativas de nube a escala global. 

## <a name="scenarios-for-azure-cosmos-db-spark-support"></a>Escenarios de compatibilidad con Spark en Azure Cosmos DB

### <a name="retail-and-consumer-goods"></a>Venta al por menor y bienes de consumo

Puede usar la compatibilidad con Spark en Azure Cosmos DB para ofrecer recomendaciones y ofertas en tiempo real. Puede ayudar a los clientes a descubrir los artículos que necesitan con la personalización en tiempo real y las recomendaciones de productos.

* Puede usar la compatibilidad integrada de Machine Learning proporcionada por el motor en tiempo de ejecución de Apache Spark para generar recomendaciones en tiempo real en los catálogos de productos.

* Puede extraer con un clic datos de streaming, datos de compra y datos de cliente para proporcionar recomendaciones específicas que promuevan valor a largo plazo.

* Con la característica de distribución global de Azure Cosmos DB, los grandes volúmenes de datos de productos que se distribuyen entre regiones se pueden analizar en milisegundos.

* Puede obtener rápidamente conclusiones para datos y usuarios geográficamente distribuidos. Puede mejorar la tasa de conversión de promoción ofreciendo el anuncio adecuado al usuario adecuado en el momento adecuado.

* Puede aprovechar la funcionalidad de streaming integrada de Spark para enriquecer datos en directo combinándolos con datos de clientes estáticos. De este modo, podrá ofrecer anuncios más personalizados y dirigidos en tiempo real y en contexto con lo que hacen los clientes.

La siguiente imagen muestra cómo se usa la compatibilidad de Spark con Azure Cosmos DB para optimizar los precios y las promociones:

![Compatibilidad de Spark con Azure Cosmos DB para optimizar los precios y las promociones](./media/spark-api-introduction/optimize-pricing-and-promotions.png)


La siguiente imagen muestra cómo se usa la compatibilidad de Spark con Azure Cosmos DB en el motor de recomendaciones en tiempo real:

![Compatibilidad de Spark con Azure Cosmos DB en el motor de recomendaciones en tiempo real](./media/spark-api-introduction/real-time-recommendation-engine.png)

### <a name="manufacturing-and-iot"></a>Fabricación y IoT

La plataforma de análisis incorporada de Azure Cosmos DB permite habilitar el análisis en tiempo real de los datos de IoT desde millones de dispositivos a escala global. Puede realizar innovaciones modernas como predecir patrones meteorológicos, análisis predictivos y optimizaciones de energía.

* Mediante el uso de Azure Cosmos DB, puede extraer datos, como métricas de recursos en tiempo real y factores meteorológicos, y luego aplicar el análisis de red de distribución inteligente de electricidad para optimizar el rendimiento de los dispositivos conectados en el campo. El análisis de red de distribución inteligente de electricidad es la clave para controlar los costos de funcionamiento, mejorar la confiabilidad de la red y ofrecer servicios energéticos personalizados a los consumidores.

La siguiente imagen muestra cómo se usa la compatibilidad de Spark con Azure Cosmos DB para leer métricas de dispositivos IoT y aplicar el análisis de red de distribución inteligente de electricidad:

![Compatibilidad de Spark con Azure Cosmos DB para leer métricas de dispositivos IoT](./media/spark-api-introduction/read-metrics-from-iot-devices.png)


### <a name="predictive-maintenance"></a>Mantenimiento predictivo

* El mantenimiento de recursos como los compresores que se usan en equipos de perforación pequeños a plataformas de aguas profunda, es un esfuerzo complejo. Estos recursos se encuentran por todo el mundo y generan petabytes de datos. Mediante el uso de Azure Cosmos DB, puede crear una canalización de datos predictivos de un extremo a otro, que use streaming de Spark para procesar grandes cantidades de telemetría de sensor, piezas de recursos de almacén y datos de asignaciones de sensor.

* Puede crear e implementar modelos de aprendizaje automático para predecir errores en los recursos antes de que se produzcan, y emitir pedidos de trabajo de mantenimiento antes de que se produzca el error.

La siguiente imagen muestra cómo se usa la compatibilidad de Spark con Azure Cosmos DB para crear un sistema de mantenimiento predictivo:

![Compatibilidad de Spark con Azure Cosmos DB para crear un sistema de mantenimiento predictivo](./media/spark-api-introduction/predictive-maintenance-system.png)

La siguiente imagen muestra cómo se usa la compatibilidad de Spark con Azure Cosmos DB para crear un sistema de diagnóstico de vehículos en tiempo real:

![Compatibilidad de Spark con Azure Cosmos DB para crear un sistema de diagnóstico de vehículos en tiempo real](./media/spark-api-introduction/real-time-vehicle-diagnostic-system.png)

### <a name="gaming"></a>Juegos

* Gracias a la compatibilidad integrada con Spark, Azure Cosmos DB le permite compilar, escalar e implementar fácilmente modelos de aprendizaje automático y análisis avanzados en unos minutos para crear la mejor experiencia de juego posible.

* Puede analizar el reproductor, la compra y los datos de comportamiento para crear ofertas personalizadas apropiadas con el fin de lograr altos índices de conversión.

* Con el aprendizaje automático de Spark, puede analizar y obtener conclusiones sobre los datos de telemetría de juegos. Puede diagnosticar y evitar tiempos de carga lentos y problemas en el juego.

La siguiente imagen muestra cómo se usa la compatibilidad de Spark con Azure Cosmos DB en los análisis de juego:

![Compatibilidad de Spark de Azure Cosmos DB en el análisis de juegos](./media/spark-api-introduction/gaming-analytics.png)

## <a name="built-in-jupyter-notebooks-support"></a>Compatibilidad integrada con cuadernos de Jupyter

Azure Cosmos DB admite cuadernos de Jupyter Notebook integrados para todas las API, como Cassandra, MongoDB, SQL, Gremlin y Table. Los cuadernos de Jupyter Notebook se ejecutan en las cuentas de Azure Cosmos y mejoran la experiencia del desarrollador. La compatibilidad integrada de los cuadernos con todas las API y modelos de datos de Azure Cosmos DB permite ejecutar consultas de forma interactiva. También puede ejecutar modelos de aprendizaje automático y analizar los datos almacenados en las bases de datos de Azure Cosmos. Gracias a la experiencia de los cuadernos de Jupyter, puede analizar los datos almacenados, crear y entrenar modelos de aprendizaje automático y realizar inferencias sobre los datos en Azure Portal, como se muestra en la siguiente imagen:

![Compatibilidad con cuadernos de Jupyter en Azure Cosmos DB](./media/spark-api-introduction/jupyter-notebooks-portal.png)

## <a name="next-steps"></a>Pasos siguientes

* Para obtener información acerca de las ventajas de Azure Cosmos DB, consulte el artículo de [introducción](introduction.md).
* [Introducción a la API de Azure Cosmos DB para MongoDB](mongodb-introduction.md)
* [Introducción a Cassandra API de Azure Cosmos DB](cassandra-introduction.md)
* [Introducción a Gremlin API de Azure Cosmos DB](graph-introduction.md)
* [Introducción a Table API de Azure Cosmos DB](table-introduction.md)




