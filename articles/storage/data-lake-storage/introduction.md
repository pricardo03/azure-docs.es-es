---
title: Introducción a Azure Data Lake Storage Gen2 (versión preliminar)
description: Proporciona una introducción a Azure Data Lake Storage Gen2 (versión preliminar).
services: storage
author: jamesbak
ms.service: storage
ms.topic: article
ms.date: 06/27/2018
ms.author: jamesbak
ms.component: data-lake-storage-gen2
ms.openlocfilehash: 51f38cf7ade01b58ad5ce7925af5546d1a4f1a0c
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2018
ms.locfileid: "39525389"
---
# <a name="introduction-to-azure-data-lake-storage-gen2-preview"></a>Introducción a Azure Data Lake Storage Gen2 (versión preliminar)

Azure Data Lake Storage Gen2 (versión preliminar) es un conjunto de funcionalidades dedicadas al análisis de macrodatos, creadas a partir de [Azure Blob Storage](../blobs/storage-blobs-introduction.md). Permite establecer una conexión con los datos usando tanto el sistema de archivos como el almacenamiento de objetos. Esto convierte Azure Data Lake Storage Gen 2 en el único servicio de almacenamiento multimodal basado en la nube, lo que permite extraer el valor de análisis de todos los datos.

Data Lake Storage Gen2 incluye todas las cualidades necesarias para el ciclo de vida completo de los datos de análisis. Este es el resultado de la conversión de las funcionalidades de nuestros dos servicios de almacenamiento existentes. Las características de [Azure Data Lake Storage Gen1](../../data-lake-store/index.md), como la semántica del sistema de archivo, la escala y la seguridad a nivel de archivo, se combinan con funcionalidades de recuperación ante desastres o alta disponibilidad y de almacenamiento por niveles de bajo costo, así como con un ecosistema de herramientas o SDK de [Azure Blob Storage](../blobs/storage-blobs-introduction.md). En Data Lake Storage Gen2, permanecen todas las cualidades del almacenamiento de objetos y se agregan las ventajas de una interfaz de sistema de archivos optimizada para cargas de trabajo de análisis.

## <a name="designed-for-enterprise-big-data-analytics"></a>Se ha diseñado para el análisis de macrodatos empresariales.

Data Lake Storage Gen2 es el principal servicio de almacenamiento para crear lagos de datos empresariales (EDL) en Azure. Diseñado desde el principio para prestar servicios para varios petabytes de información y soportar cientos de gigabits, Data Lake Storage Gen2 le ofrece una forma fácil de administrar cantidades masivas de datos.

Una característica fundamental de Data Lake Storage Gen2 es la adición de un [espacio de nombres jerárquico](./namespace.md) al servicio de Blob Storage que organiza los objetos o archivos en una jerarquía de directorios para el acceso a datos de rendimiento. El espacio de nombres jerárquico también habilita la compatibilidad de Data Lake Storage Gen2 tanto con el sistema de archivos como con el almacenamiento de objetos al mismo tiempo. Por ejemplo, una convención de nomenclatura de almacenamiento de objetos común utiliza barras diagonales en el nombre para imitar una estructura jerárquica de carpetas. Esta estructura se hace realidad con Data Lake Storage Gen2. Operaciones como el cambio de nombre o la eliminación de un directorio se convierten en operaciones de metadatos atómicas únicas en el directorio, en lugar de enumerar y procesar todos los objetos que comparten el prefijo del nombre del directorio.

Anteriormente, los análisis basados en la nube tenían que llegar a un acuerdo en materia de rendimiento, administración y seguridad. Data Lake Storage Gen2 aborda cada uno de estos aspectos de las siguientes formas:

- El **rendimiento** se optimiza porque no es necesario copiar ni transformar datos como requisito previo para el análisis. El espacio de nombres jerárquico mejora considerablemente el rendimiento de las operaciones de administración de directorios y, consecuentemente, el rendimiento general del trabajo.

- La **administración** es más fácil, ya que puede organizar y manipular archivos a través de directorios y subdirectorios.

- La **rentabilidad** se hace posible gracias a que Data Lake Storage Gen2 se ha diseñado a partir de [Azure Blob Storage](../blobs/storage-blobs-introduction.md) de bajo costo. Las características adicionales reducen aún más el costo total de propiedad para la ejecución de análisis de macrodatos en Azure.

## <a name="key-features-of-data-lake-storage-gen2"></a>Características clave de Data Lake Storage Gen2

> [!NOTE]
> Durante la versión preliminar pública de Data Lake Storage Gen2, puede variar la disponibilidad de algunas de las características enumeradas a continuación. Como las regiones y características nuevas se lanzan durante el programa de versión preliminar, se comunicará esta información.
> [Regístrese](https://aka.ms/adlsgen2signup) en la versión preliminar pública de Data Lake Storage Gen2.  

- **Acceso a la compatibilidad con Hadoop**: Data Lake Storage Gen2 le permite administrar y obtener acceso a los datos igual que lo haría con un [sistema de archivos distribuido de Hadoop (HDFS)](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html). El nuevo [controlador ABFS](./abfs-driver.md) está disponible en todos los entornos de Apache Hadoop, incluidos [Azure HDInsight](../../hdinsight/index.yml) y [Azure Databricks](../../azure-databricks/index.yml), para obtener acceso a los datos almacenados en Data Lake Storage Gen2.

- **Acceso a datos multiprotocolo y multimodal**: Data Lake Storage Gen2 se considera un servicio de almacenamiento **multimodal**, ya que proporciona las interfaces de almacenamiento de objetos y del sistema para los mismos datos **a la vez**. Esto se logra al proporcionar varios puntos de conexión de protocolo que pueden obtener acceso a los mismos datos. 

    A diferencia de otras soluciones de análisis, no es necesario mover ni transformar los datos almacenados en Data Lake Storage Gen2 para poder ejecutar una variedad de herramientas de análisis. Puede tener acceso a datos a través de las [API de Blob Storage](../blobs/storage-blobs-introduction.md) tradicionales (por ejemplo: recopilar datos a través de [Event Hubs Capture](../../event-hubs/event-hubs-capture-enable-through-portal.md)) y procesar dichos datos con HDInsight o Azure Databricks al mismo tiempo. 

- **Rentabilidad**: Data Lake Storage Gen2 ofrece transacciones y capacidad de almacenamiento de bajo costo. Al igual que las transiciones de datos a lo largo de su ciclo de vida completo, las tasas de facturación cambian y mantienen los costos al mínimo a través de características integradas, como el [ciclo de vida de Azure Blob Storage](../common/storage-lifecycle-managment-concepts.md).

- **Funciona con herramientas, marcos y aplicaciones de Blob Storage**: Data Lake Storage Gen2 sigue funcionando con una amplia gama de herramientas, marcos y aplicaciones que existen actualmente para Blob Storage.

- **Controlador optimizado**: el controlador `abfs` está [optimizado específicamente](./abfs-driver.md) para el análisis de macrodatos. Las API REST correspondientes se exponen a través del punto de conexión `dfs`, `dfs.core.windows.net`.

## <a name="scalability"></a>Escalabilidad

Azure Storage es escalable de forma natural si obtiene acceso a través de las interfaces de Data Lake Storage Gen2 o Blob Storage. Es capaz de almacenar y atender *muchos exabytes de datos*. Esta cantidad de almacenamiento está disponible con rendimiento medido en gigabits por segundo (Gbps) en niveles altos de operaciones de entrada/salida por segundo (IOPS). Más allá de la persistencia, el procesamiento se ejecuta en las latencias por solicitud casi constantes que se miden en los niveles de servicio, cuenta y archivo.

## <a name="cost-effectiveness"></a>Rentabilidad

Una de las numerosas ventajas de la creación de Data Lake Storage Gen2 sobre Azure Blob Storage es el [bajo costo](https://azure.microsoft.com/pricing/details/storage) de la capacidad de almacenamiento y las transacciones. A diferencia de otros servicios de almacenamiento en nube, Data Lake Storage Gen2 disminuye los costos porque no es necesario mover ni transformar los datos antes de realizar el análisis.

Además, características como el [espacio de nombres jerárquico](./namespace.md) mejoran significativamente el rendimiento general de muchos trabajos de análisis. Esta mejora del rendimiento significa que se requerirá menos eficacia de proceso para procesar la misma cantidad de datos, lo que genera un menor costo total de propiedad (TCO) para el trabajo de análisis de un extremo a otro.

## <a name="next-steps"></a>Pasos siguientes

En los artículos siguientes se describen algunos de los principales conceptos de Data Lake Storage Gen2 y se detalla cómo almacenar y administrar los datos, además de cómo obtener información y acceso a ellos:

* [Espacio de nombres jerárquico](./namespace.md)
* [crear una cuenta de almacenamiento](./quickstart-create-account.md)
* [Creación de un clúster de HDInsight con Azure Data Lake Storage Gen2](./quickstart-create-connect-hdi-cluster.md)
* [Uso de una cuenta de Azure Data Lake Storage Gen2 en Azure Databricks](./quickstart-create-databricks-account.md) 