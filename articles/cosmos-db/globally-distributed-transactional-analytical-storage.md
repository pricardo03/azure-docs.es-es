---
title: Almacenamiento transaccional y analítico distribuido globalmente para contenedores de Azure Cosmos
description: Obtenga información sobre el almacenamiento transaccional y analítico y sus opciones de configuración para contenedores de Azure Cosmos.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/30/2019
ms.reviewer: sngun
ms.openlocfilehash: abf222b7a6d6e8fd053fa83c066d2b7850f575ab
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2019
ms.locfileid: "72756901"
---
# <a name="globally-distributed-transactional-and-analytical-storage-for-azure-cosmos-containers"></a>Almacenamiento transaccional y analítico distribuido globalmente para contenedores de Azure Cosmos

Los contenedores de Azure Cosmos están respaldados internamente por dos motores de almacenamiento: el motor de almacenamiento transaccional y un motor de almacenamiento analítico actualizable. Ambos motores de almacenamiento están estructurados en registros y optimizados para la escritura, lo que permite actualizaciones más rápidas. Aunque cada uno de ellos se codifica de forma diferente:

* **Motor de almacenamiento transaccional**: se codifica en formato orientado a filas a fin de realizar consultas y lecturas transaccionales rápidas.

* **Motor de almacenamiento analítico**: se codifica en formato de columnas a fin de realizar exámenes y consultas analíticas rápidas.

![Motores de almacenamiento y asignación de API de Azure Cosmos DB](./media/globally-distributed-transactional-analytical-storage/storage-engines-api-mapping.png)

El motor de almacenamiento transaccional está respaldado por discos SSD locales, mientras que el almacenamiento analítico se lleva a cabo en un almacenamiento SSD fuera del clúster económico. En la tabla siguiente se recogen las diferencias más importantes entre el almacenamiento transaccional y el almacenamiento analítico.


|Característica  |Almacenamiento transaccional  |Almacenamiento analítico |
|---------|---------|---------|
|Almacenamiento máximo por contenedor de Azure Cosmos |   Ilimitado      |    Ilimitado     |
|Almacenamiento máximo por clave de partición lógica   |   10 GB      |   Ilimitado      |
|Codificación de almacenamiento  |   Orientado a filas con formato interno.   |   Orientado a columnas con formato Parquet de Apache. |
|Localidad de almacenamiento |   Almacenamiento replicado respaldado por discos SSD locales o dentro del clúster. |  Almacenamiento replicado respaldado por discos SSD de clúster remotos o fuera del clúster.       |
|Durabilidad.  |    99,99999 (7-9 s)     |  99,99999 (7-9 s)       |
|API que accede a los datos  |   SQL, MongoDB, Cassandra, Gremlin, Tables y Etcd.       | Spark de Apache         |
|Retención (período de vida o TTL)   |  Controlado por directivas, configurado en el contenedor de Azure Cosmos mediante la propiedad `DefaultTimeToLive`.       |   Controlado por directivas, configurado en el contenedor de Azure Cosmos mediante la propiedad `ColumnStoreTimeToLive`.      |
|Precio por GB    |   0,25 USD/GB      |  0,02 USD/GB       |
|Precio de las transacciones de almacenamiento    | El rendimiento aprovisionado se cobra a 0,008 USD por cada 100 RU/s con facturación por hora.        |  El rendimiento basado en el consumo se cobra a 0,05 por 10 000 transacciones de escritura y 0,004 por 10 000 transacciones de lectura.       |

## <a name="benefits-of-transactional-and-analytical-storage"></a>Ventajas del almacenamiento transaccional y analítico

### <a name="no-etl-operations"></a>Sin operaciones de ETL

Las canalizaciones analíticas tradicionales son complejas con varias fases, cada una de las cuales requiere operaciones de extracción, transformación y carga de datos (ETL) hacia y desde las capas de proceso y almacenamiento. Esto se traduce en arquitecturas complejas de procesamiento de datos. Lo que implica costos elevados para varias fases de almacenamiento y proceso, y una latencia elevada debida a grandes volúmenes de datos transferidos entre varias fases de almacenamiento y proceso.  

No se generan gastos generales al realizar operaciones de ETL con Azure Cosmos DB. Cada contenedor de Azure Cosmos está respaldado por motores de almacenamiento analítico y transaccional, y la transferencia de datos entre el motor de almacenamiento analítico y transaccional se realiza dentro del motor de base de datos y sin saltos de red. La latencia y el costo resultantes son significativamente inferiores en comparación con las soluciones analíticas tradicionales. Además, obtendrá un solo sistema de almacenamiento distribuido globalmente para cargas de trabajo analíticas y transaccionales.  

### <a name="store-multiple-versions-update-and-query-the-analytical-storage"></a>Almacenamiento de varias versiones, actualización y consulta del almacenamiento analítico

El almacenamiento analítico es totalmente actualizable y contiene el historial de versiones completo de todas las actualizaciones transaccionales que se produjeron en el contenedor de Azure Cosmos.

Se garantiza que las actualizaciones realizadas en el almacenamiento transaccional serán visibles para el almacenamiento analítico en un plazo de 30 segundos. 

### <a name="globally-distributed-multi-master-analytical-storage"></a>Almacenamiento analítico de arquitectura multimaestro distribuido globalmente

Si su cuenta de Azure Cosmos tiene el ámbito de una sola región, los datos almacenados (en almacenamiento transaccional y analítico) en los contenedores también se limitan a una sola región. Por otro lado, si la cuenta de Azure Cosmos está distribuida globalmente, los datos almacenados en los contenedores también se distribuyen globalmente.

En el caso de las cuentas de Azure Cosmos configuradas con varias regiones de escritura, las escrituras en el contenedor (tanto en el almacenamiento transaccional como en el analítico) siempre se realizan en la región local y, por tanto, son rápidas.

Tanto en las cuentas de Azure Cosmos de una región como de varias, independientemente de si se trata de una región de escritura única (maestro único) o de varias regiones de escritura (también conocidas como de arquitectura multimaestro), las consultas y lecturas transaccionales y analíticas se realizan localmente en la región especificada.

### <a name="performance-isolation-between-transactional-and-analytical-workloads"></a>Aislamiento del rendimiento entre cargas de trabajo transaccionales y analíticas

En una región determinada, las cargas de trabajo transaccionales se ejecutan en el almacenamiento transaccional o de filas del contenedor. Por otro lado, las cargas de trabajo analíticas se ejecutan en el almacenamiento analítico o de columnas del contenedor. Los dos motores de almacenamiento se ejecutan independientemente y brindan un estricto aislamiento de rendimiento entre las cargas de trabajo.

Las cargas de trabajo transaccionales consumen el rendimiento aprovisionado (RU). A diferencia de las cargas de trabajo transaccionales, el rendimiento de las cargas de trabajo analíticas se basa en el consumo real. Las cargas analíticas consumen recursos a petición.

### <a name="on-demand-snapshots-and-time-travel-analytics"></a>Análisis de viaje en el tiempo e instantáneas a petición

Puede tomar instantáneas de los datos almacenados en el almacenamiento analítico de sus contenedores de Azure Cosmos, en cualquier momento, llamando al comando `CreateSnapshot (name, timestamp)` en el contenedor. Las instantáneas se denominan "marcadores" en el historial de las actualizaciones que se realizaron en el contenedor.

![Análisis de viaje en el tiempo e instantáneas a petición](./media/globally-distributed-transactional-analytical-storage/ondemand-analytical-data-snapshots.png)

En el momento de crear la instantánea, además del nombre, puede especificar la marca de tiempo que define el estado del contenedor en el historial de actualizaciones. Después, puede cargar los datos de la instantánea en Spark y realizar las consultas.

Actualmente, solo puede tomar instantáneas a petición en cualquier momento en el contenedor, ya que la capacidad de tomar instantáneas automáticamente basadas en una programación o en una directiva personalizada todavía no se admite.

### <a name="configure-and-tier-data-between-transactional-and-analytical-storage-independently"></a>Configuración y segmentación de datos entre los almacenamientos transaccional y analítico de forma independiente

En función del escenario, puede habilitar o deshabilitar de forma independiente cada uno de los dos motores de almacenamiento. A continuación se muestran las configuraciones de cada escenario:

|Escenario |Valor de almacenamiento transaccional  |Valor de almacenamiento analítico |
|---------|---------|---------|
|Ejecución exclusiva de cargas de trabajo analíticas (con retención infinita) |  DefaultTimeToLive = 0       |  ColumnStoreTimeToLive = -1       |
|Ejecución exclusiva de cargas de trabajo transaccionales (con retención infinita)  |   DefaultTimeToLive = -1      |  ColumnStoreTimeToLive = 0       |
|Ejecución de cargas de trabajo transaccionales y analíticas (con retención infinita)   |   DefaultTimeToLive = -1      | ColumnStoreTimeToLive = -1        |
|Ejecución de cargas de trabajo transaccionales y analíticas (con diferentes intervalos de retención, también conocidos como niveles de almacenamiento)  |  DefaultTimeToLive = <Value1>       |     ColumnStoreTimeToLive = <Value2>    |

1. **Configuración del contenedor exclusivamente para cargas de trabajo analíticas (con retención infinita)**

   Puede configurar el contenedor de Azure Cosmos en exclusiva para cargas de trabajo analíticas. Esta configuración tiene la ventaja de que no es necesario pagar por el almacenamiento transaccional. Si el objetivo es usar el contenedor solo para cargas de trabajo analíticas, puede deshabilitar el almacenamiento transaccional estableciendo `DefaultTimeToLive` en 0 en el contenedor de Cosmos y habilitar el almacenamiento analítico con retención infinita estableciendo `ColumnStoreTimeToLive` en -1.

   ![Cargas de trabajo analíticas con retención infinita](./media/globally-distributed-transactional-analytical-storage/analytical-workload-configuration.png)

1. **Configuración del contenedor exclusivamente para cargas de trabajo transaccionales (con retención infinita)**

   Puede configurar el contenedor de Azure Cosmos en exclusiva para cargas de trabajo transaccionales. Puede deshabilitar el almacenamiento analítico estableciendo `ColumnStoreTimeToLive` en 0 en el contenedor y habilitar el almacenamiento analítico con retención infinita estableciendo `DefaultTimeToLive` en -1.

   ![Cargas de trabajo transaccionales con retención infinita](./media/globally-distributed-transactional-analytical-storage/transactional-workload-configuration.png)

1. **Configuración del contenedor para cargas de trabajo transaccionales y analíticas (con retención infinita)**

   Puede configurar el contenedor de Azure Cosmos para cargas de trabajo transaccionales y analíticas con aislamiento de rendimiento completo entre ellas. Puede habilitar el almacenamiento analítico estableciendo `ColumnStoreTimeToLive` en -1 y habilitar el almacenamiento transaccional con retención infinita estableciendo `DefaultTimeToLive ` en -1.

   ![Cargas de trabajo transaccionales y analíticas con retención infinita](./media/globally-distributed-transactional-analytical-storage/analytical-transactional-configuration-infinite-retention.png)

1. **Configuración del contenedor para cargas de trabajo transaccionales y analíticas con niveles de almacenamiento**

   Puede configurar el contenedor de Azure Cosmos para cargas de trabajo transaccionales y analíticas con aislamiento de rendimiento completo entre ellas y distintos intervalos de retención. Azure Cosmos DB exigirá que el almacenamiento analítico se conserve siempre durante más tiempo que el almacenamiento transaccional.

   Puede habilitar el almacenamiento transaccional con retención infinita estableciendo `DefaultTimeToLive` en <valor 1> y habilitar el almacenamiento analítico con retención infinita estableciendo `ColumnStoreTimeToLive` en <valor 2>. Azure Cosmos DB exigirá que <valor 2> sea siempre mayor que <valor 1>.

   ![Cargas de trabajo transaccionales y analíticas con niveles de almacenamiento](./media/globally-distributed-transactional-analytical-storage/analytical-transactional-configuration-specified-retention.png)

## <a name="next-steps"></a>Pasos siguientes

* [Período de vida en Azure Cosmos DB](time-to-live.md)
