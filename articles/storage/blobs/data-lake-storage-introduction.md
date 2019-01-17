---
title: Introducción a Azure Data Lake Storage Gen2 (versión preliminar)
description: Proporciona una introducción a Azure Data Lake Storage Gen2 (versión preliminar).
services: storage
author: jamesbak
ms.service: storage
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: jamesbak
ms.component: data-lake-storage-gen2
ms.openlocfilehash: 898080f445c54c93543a481bdd0487c70e772160
ms.sourcegitcommit: e7312c5653693041f3cbfda5d784f034a7a1a8f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2019
ms.locfileid: "54214408"
---
# <a name="introduction-to-azure-data-lake-storage-gen2-preview"></a>Introducción a Azure Data Lake Storage Gen2 (versión preliminar)

‎Azure Data Lake Storage Gen2 (versión preliminar) es un conjunto de funcionalidades dedicadas al análisis de macrodatos creado en [Azure Blob Storage](storage-blobs-introduction.md). Data Lake Storage Gen2 es el resultado de la convergencia de las funcionalidades de nuestros dos servicios de almacenamiento existentes: Azure Blob Storage y Azure Data Lake Storage Gen1. Las características de [Azure Data Lake Storage Gen1](https://docs.microsoft.com/azure/data-lake-store/index), como la semántica del sistema de archivos, la seguridad de nivel de directorio y archivo y la escala se combinan con las funcionalidades de recuperación ante desastres o alta disponibilidad, y de almacenamiento por niveles de bajo costo de [Azure Blob Storage](storage-blobs-introduction.md).

## <a name="designed-for-enterprise-big-data-analytics"></a>Se ha diseñado para el análisis de macrodatos empresariales.

Data Lake Storage Gen2 convierte a Azure Storage en los cimientos para crear lagos de datos empresariales en Azure. Diseñado desde el principio para servir varios petabytes de información y mantener cientos de gigabits de rendimiento, Data Lake Storage Gen2 le ofrece una forma fácil de administrar cantidades masivas de datos.

Parte fundamental de Data Lake Storage Gen2 es la incorporación de un [espacio de nombres jerárquico](data-lake-storage-namespace.md) en Blob Storage. El espacio de nombres jerárquico organiza los objetos o archivos en una jerarquía de directorios para un acceso eficaz a los datos. Una convención de nomenclatura de almacenamiento de objetos común usa barras diagonales en el nombre para imitar una estructura jerárquica de directorios. Esta estructura se hace realidad con Data Lake Storage Gen2. Operaciones como el cambio de nombre o la eliminación de un directorio se convierten en operaciones de metadatos atómicas únicas en el directorio, en lugar de enumerar y procesar todos los objetos que comparten el prefijo del nombre del directorio.

Anteriormente, los análisis basados en la nube tenían que llegar a un acuerdo en materia de rendimiento, administración y seguridad. Data Lake Storage Gen2 aborda cada uno de estos aspectos de las siguientes formas:

-   El **rendimiento** se optimiza porque no es necesario copiar ni transformar datos como requisito previo para el análisis. El espacio de nombres jerárquico mejora considerablemente el rendimiento de las operaciones de administración de directorios y, consecuentemente, el rendimiento general del trabajo.

-   La **administración** es más fácil, ya que puede organizar y manipular archivos a través de directorios y subdirectorios.

-   La **seguridad** se aplicable porque se pueden definir permisos POSIX en los directorios o archivos individuales.

-   La **rentabilidad** se hace posible gracias a que Data Lake Storage Gen2 se ha diseñado a partir de [Azure Blob Storage](storage-blobs-introduction.md) de bajo costo. Las características adicionales reducen aún más el costo total de propiedad para la ejecución de análisis de macrodatos en Azure.

## <a name="key-features-of-data-lake-storage-gen2"></a>Características clave de Data Lake Storage Gen2

-   **Acceso compatible con Hadoop**: Data Lake Storage Gen2 le permite administrar y acceder a los datos igual que lo haría con un [sistema de archivos distribuido de Hadoop (HDFS)](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html). El nuevo [controlador ABFS](data-lake-storage-abfs-driver.md) está disponible en todos los entornos de Apache Hadoop, incluidos [Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/index)*,* [Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/index) y [SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/) para acceder a los datos almacenados en Data Lake Storage Gen2.

-   **Superconjunto de permisos POSIX**: el modelo de seguridad de Data Lake Gen2 es compatible con los permisos de ACL y POSIX junto con granularidad adicional específica de Data Lake Storage Gen2. La configuración se puede realizar mediante el Explorador de Storage o a través de marcos como Hive y Spark.

-   **Rentabilidad**: Data Lake Storage Gen2 ofrece transacciones y capacidad de almacenamiento de bajo costo. Al igual que las transiciones de datos a lo largo de su ciclo de vida completo, las tasas de facturación cambian y mantienen los costos al mínimo a través de características integradas, como el [ciclo de vida de Azure Blob Storage](storage-lifecycle-management-concepts.md).

-   **Controlador optimizado**: el controlador abfs está [optimizado específicamente](data-lake-storage-abfs-driver.md) para el análisis de macrodatos. Las API REST correspondientes se exponen a través del punto de conexión dfs, dfs.core.windows.net.

### <a name="scalability"></a>Escalabilidad

Azure Storage es escalable de forma natural si obtiene acceso a través de las interfaces de Data Lake Storage Gen2 o Blob Storage. Es capaz de almacenar y atender *muchos exabytes de datos*. Esta cantidad de almacenamiento está disponible con rendimiento medido en gigabits por segundo (Gbps) en niveles altos de operaciones de entrada/salida por segundo (IOPS). Más allá de la persistencia, el procesamiento se ejecuta en las latencias por solicitud casi constantes que se miden en los niveles de servicio, cuenta y archivo.

### <a name="cost-effectiveness"></a>Rentabilidad

Una de las numerosas ventajas de la creación de Data Lake Storage Gen2 sobre Azure Blob Storage es el bajo costo de la capacidad de almacenamiento y las transacciones. A diferencia de otros servicios de almacenamiento en nube, no es necesario mover ni transformar los datos almacenados en Data Lake Storage Gen2 antes de realizar el análisis. Para más información sobre los precios, consulte [Precios de Azure Storage](https://azure.microsoft.com/pricing/details/storage).

Además, características como el [espacio de nombres jerárquico](data-lake-storage-namespace.md) mejoran significativamente el rendimiento general de muchos trabajos de análisis. Esta mejora del rendimiento significa que se requerirá menos eficacia de proceso para procesar la misma cantidad de datos, lo que genera un menor costo total de propiedad (TCO) para el trabajo de análisis de un extremo a otro.

### <a name="one-service-multiple-concepts"></a>Un servicio, varios conceptos

Data Lake Storage Gen2 es una funcionalidad adicional para el análisis de macrodatos basada en Azure Blob Storage. Aunque hay muchas ventajas en usar componentes de plataformas existentes de blobs para crear y usar lagos de datos para realizar el análisis, sí aparecen varios conceptos para describir los mismos aspectos compartidos.

A continuación, se muestran las entidades equivalentes, tal como se describen en distintos conceptos. A menos que se especifique lo contrario, estas entidades son sinónimos directos:

| Concepto                                | Organización de nivel superior | Organización de nivel inferior                                            | Contenedor de datos |
|----------------------------------------|------------------------|---------------------------------------------------------------------|----------------|
| Blobs: almacenamiento de objetos de uso general | Contenedor              | Directorio virtual (solo SDK, no proporciona manipulación atómica) | Blob           |
| ADLS Gen2: almacenamiento de análisis          | Filesystem             | Directorio                                                           | Archivo           |

## <a name="supported-open-source-platforms"></a>Plataformas de código abierto compatibles

Varias plataformas de código abierto son compatibles con Data Lake Storage Gen2. Esas plataformas aparecen en la tabla siguiente.

> [!NOTE]
> Solo se admiten las versiones que aparecen en esta tabla.

| Plataforma |  Versiones compatibles | Más información |
| --- | --- | --- |
| [HDInsight](https://azure.microsoft.com/services/hdinsight/) | 3.6+ | [¿Cuáles son los componentes y versiones de Apache Hadoop disponibles con HDInsight?](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning?toc=%2Fen-us%2Fazure%2Fhdinsight%2Fstorm%2FTOC.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json)
| [Hadoop](https://hadoop.apache.org/) | 2.7+ | [Archivo de las versiones de Apache Hadoop](https://hadoop.apache.org/release.html) |
| [Cloudera](https://www.cloudera.com/) | 6.1+ | [Notas de la versión 6.x de Cloudera Enterprise](https://www.cloudera.com/documentation/enterprise/6/release-notes/topics/rg_cdh_6_release_notes.html) |
| [Azure Databricks](https://azure.microsoft.com/services/databricks/) | 4.2+ | [Versiones de Databricks Runtime](https://docs.databricks.com/release-notes/runtime/databricks-runtime-ver.html) |
|[Hortonworks](https://hortonworks.com/)| 2.6+ | [Documentación de Hortonworks](https://docs.hortonworks.com/) |

## <a name="next-steps"></a>Pasos siguientes

En los artículos siguientes se describen algunos de los principales conceptos de Data Lake Storage Gen2 y se detalla cómo almacenar y administrar los datos, además de cómo obtener información y acceso a ellos:

-   [Espacio de nombres jerárquico](data-lake-storage-namespace.md)
-   [Cree una cuenta de almacenamiento](data-lake-storage-quickstart-create-account.md)
-   [Create an HDInsight cluster with Data Lake Storage Gen2](data-lake-storage-quickstart-create-connect-hdi-cluster.md) (Creación de un clúster de HDInsight con Data Lake Storage Gen2)
-   [Uso de una cuenta de Data Lake Storage Gen2 en Azure Databricks](data-lake-storage-quickstart-create-databricks-account.md)
