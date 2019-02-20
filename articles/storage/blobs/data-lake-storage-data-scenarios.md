---
title: Escenarios de datos relacionados con Azure Data Lake Storage Gen2 | Microsoft Docs
description: Descripción de los diferentes escenarios y herramientas mediante los cuales los datos se pueden ingerir, procesar, descargar y visualizar en Data Lake Storage Gen2 (anteriormente conocido como Azure Data Lake Store).
services: storage
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: normesta
ms.openlocfilehash: 1c50a6e14955b2c31222ff1317aa99ad28866ec8
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55864747"
---
# <a name="using-azure-data-lake-storage-gen2-for-big-data-requirements"></a>Uso de Azure Data Lake Storage Gen2 para requisitos de macrodatos

Hay cuatro fases principales en el procesamiento de macrodatos:

* Introducción de grandes cantidades de datos en un almacén de datos, en tiempo real o por lotes
* Procesamiento de los datos
* Descarga de los datos
* Visualización de los datos

En este artículo nos centramos en estas fases relativas a Azure Data Lake Storage Gen2 para comprender las opciones y herramientas disponibles para satisfacer sus necesidades de macrodatos.

## <a name="ingest-data-into-data-lake-storage-gen2"></a>Ingesta de datos en Data Lake Storage Gen2
En esta sección se resaltan los distintos orígenes de datos y las distintas formas en que esos datos se pueden ingerir en una cuenta de Data Lake Storage Gen2.

![Ingesta de datos en Data Lake Storage Gen2](./media/data-lake-storage-data-scenarios/ingest-data.png "Ingesta de datos en Data Lake Storage Gen2")

### <a name="ad-hoc-data"></a>Datos ad-hoc
Representan conjuntos de datos más pequeños que se utilizan para la creación de un prototipo de una aplicación de macrodatos. Hay diferentes maneras de introducir datos ad hoc, según el origen de los datos.

| Origen de datos | Introducir mediante |
| --- | --- |
| Equipo local |<ul> <li>[Explorador de Storage](https://azure.microsoft.com/features/storage-explorer/)</ul> |
| Blob de Azure Storage |<ul> <li>[Azure Data Factory](../../data-factory/connector-azure-data-lake-store.md)</li> <li>[Herramienta AzCopy](../common/storage-use-azcopy-v10.md)</li><li>[DistCp ejecutándose en un clúster de HDInsight](data-lake-storage-use-distcp.md)</li> </ul> |

### <a name="streamed-data"></a>Datos de streaming
Representa los datos que se pueden generar por diversos orígenes, como aplicaciones, dispositivos o sensores, entre otros. Estos datos se pueden ingerir en Data Lake Storage Gen2 mediante distintas herramientas. Estas herramientas normalmente capturan y procesan los datos en eventos individuales y en tiempo real y, después, escriben los eventos en lotes en Data Lake Storage Gen2 para que puedan procesarse posteriormente.

A continuación, se muestran las herramientas que se pueden usar:

* [Azure HDInsight Storm](../../hdinsight/storm/apache-storm-write-data-lake-store.md): se pueden escribir datos directamente en Data Lake Storage Gen2 desde el clúster de Storm.

### <a name="relational-data"></a>Datos relacionales
También se pueden originar datos desde bases de datos relacionales. Durante un tiempo, las bases de datos relacionales recopilan grandes cantidades de datos que pueden proporcionar información clave si se procesan a través de una canalización de macrodatos. Puede usar las herramientas siguientes para mover estos datos a Data Lake Storage Gen2.

* [Azure Data Factory](../../data-factory/copy-activity-overview.md)

### <a name="web-server-log-data-upload-using-custom-applications"></a>Datos de registro de servidor web (carga mediante aplicaciones personalizadas)
Este tipo de conjunto de datos es específicamente necesario porque el análisis de los datos de registro del servidor web es un caso de uso común para aplicaciones de macrodatos y requiere que se carguen grandes volúmenes de archivos de registro en Data Lake Storage Gen2. Puede utilizar cualquiera de las herramientas siguientes para escribir sus propios scripts o aplicaciones para cargar dichos datos.

* [Azure Data Factory](../../data-factory/copy-activity-overview.md)

Para cargar datos de registro del servidor web y también para cargar otros tipos de datos (por ejemplo, datos de opiniones sociales), un buen enfoque es escribir sus propios aplicaciones o scripts personalizados, porque le ofrece la flexibilidad de incluir el componente que carga los datos como parte de su aplicación de macrodatos mayor. En algunos casos, este código puede adoptar la forma de un script o de una utilidad de línea de comandos simple. En otros casos, el código puede utilizarse para integrar el procesamiento de macrodatos en una aplicación o solución de negocio.

### <a name="data-associated-with-azure-hdinsight-clusters"></a>Datos asociados con los clústeres de HDInsight de Azure
La mayoría de los tipos de clúster de HDInsight (Hadoop, HBase, Storm) admiten Data Lake Storage Gen2 como repositorio de almacenamiento de datos. Los clústeres de HDInsight acceden a los datos de Azure Storage Blob (WASB). Para mejorar el rendimiento, puede copiar los datos de WASB en una cuenta de Data Lake Storage Gen2 asociada con el clúster. Puede usar las herramientas siguientes para copiar los datos.

* [Apache DistCp](data-lake-storage-use-distcp.md)
* [Herramienta AzCopy](../common/storage-use-azcopy-v10.md)
* [Azure Data Factory](../../data-factory/connector-azure-data-lake-store.md)

### <a name="data-stored-in-on-premises-or-iaas-hadoop-clusters"></a>Datos almacenados en clústeres locales o de IaaS de Hadoop
Es posible que haya grandes cantidades de datos almacenados en clústeres de Hadoop existentes, de forma local en los equipos mediante HDFS. Los clústeres de Hadoop pueden encontrarse en una implementación local o dentro de un clúster de IaaS en Azure. Podrían existir requisitos para copiar estos datos en Azure Data Lake Storage Gen2 de forma puntual o periódica. Existen diversas opciones que puede usar para lograr esto. A continuación se muestra una lista de alternativas, además de las ventajas y desventajas asociadas.

| Enfoque | Detalles | Ventajas | Consideraciones |
| --- | --- | --- | --- |
| Usar Azure Data Factory (ADF) para copiar datos directamente desde los clústeres de Hadoop hasta Azure Data Lake Storage Gen2 |[ADF admite HDFS como origen de datos](../../data-factory/connector-hdfs.md) |ADF proporciona compatibilidad inmediata con HDFS, así como una supervisión y administración integrales y de primera clase. |Requiere que se implemente Data Management Gateway localmente o en el clúster de IaaS. |
| Usar Distcp para copiar datos de Hadoop en Azure Storage. Después, copiar los datos de Azure Storage en Data Lake Store Gen2 con el mecanismo adecuado. |Puede copiar datos de Azure Storage a Data Lake Store Gen2 por medio de: <ul><li>[Azure Data Factory](../../data-factory/copy-activity-overview.md)</li><li>[Herramienta AzCopy](../common/storage-use-azcopy-v10.md)</li><li>[Apache DistCp ejecutándose en clústeres de HDInsight](data-lake-storage-use-distcp.md)</li></ul> |Puede usar herramientas de código abierto. |Proceso de varios pasos en el que participan varias tecnologías. |

### <a name="really-large-datasets"></a>Conjuntos de datos realmente grandes
Para cargar conjuntos de datos cuyo tamaño oscila en varios terabytes, el uso de los métodos descritos anteriormente puede a veces resultar lento y costoso. En tales casos, puede utilizar las opciones siguientes.

* **Uso de Azure ExpressRoute**. Azure ExpressRoute permite crear conexiones privadas entre los centros de datos de Azure y la infraestructura de un entorno local. Esto ofrece una opción confiable para transferir grandes cantidades de datos. Para obtener más información, consulte la [documentación de Azure ExpressRoute](../../expressroute/expressroute-introduction.md).

## <a name="process-data-stored-in-data-lake-storage-gen2"></a>Procesamiento de los datos almacenados en Data Lake Storage Gen2
Cuando los datos están disponibles en Data Lake Storage Gen2, puede ejecutar un análisis en esos datos mediante las aplicaciones de macrodatos admitidas. Actualmente, se puede usar Azure HDInsight y Azure Databricks para ejecutar trabajos de análisis de datos en los datos almacenados en Data Lake Storage Gen2.

![Análisis de datos en Data Lake Storage Gen2](./media/data-lake-storage-data-scenarios/analyze-data.png "Análisis de datos en Data Lake Storage Gen2")

Para ver un ejemplo, consulte [Uso de Data Lake Storage Gen2 con clústeres de Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2).


## <a name="download-data-from-data-lake-storage-gen2"></a>Descarga de datos de Data Lake Storage Gen2
También puede descargar o mover datos de Azure Data Lake Storage Gen2 en escenarios como los siguientes:

* Mover datos a otros repositorios para interactuar con las canalizaciones de procesamiento de datos existentes. Por ejemplo, puede mover los datos de Data Lake Storage Gen2 a Azure SQL Database o a SQL Server local.
* Descargar datos en el equipo local para procesarlos en entornos IDE durante la creación de prototipos de aplicaciones.

![Salida de datos de Data Lake Storage Gen2](./media/data-lake-storage-data-scenarios/egress-data.png "Salida de datos de Data Lake Storage Gen2")

En tales casos, puede utilizar cualquiera de las opciones siguientes.

* [Azure Data Factory](../../data-factory/copy-activity-overview.md)
* [Apache DistCp](data-lake-storage-use-distcp.md)

## <a name="visualize-data-in-data-lake-storage-gen2"></a>Visualización de datos en Data Lake Storage Gen2
Puede usar una combinación de servicios para crear representaciones visuales de los datos almacenados en Data Lake Storage Gen2.

![Visualización de datos en Data Lake Storage Gen2](./media/data-lake-storage-data-scenarios/visualize-data.png "Visualización de datos en Data Lake Storage Gen2")

* Puede comenzar con [Azure Data Factory para mover datos desde Data Lake Storage Gen2 hasta Azure SQL Data Warehouse](../../data-factory/copy-activity-overview.md).
* Después, puede [integrar Power BI con Azure SQL Data Warehouse](../../sql-data-warehouse/sql-data-warehouse-get-started-visualize-with-power-bi.md) para crear una representación visual de los datos.
