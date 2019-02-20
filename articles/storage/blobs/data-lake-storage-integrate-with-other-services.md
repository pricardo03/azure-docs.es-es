---
title: Integración de Azure Data Lake Storage Gen2 con otros servicios de Azure | Microsoft Docs
description: Descubra cómo Azure Data Lake Storage Gen2 se integra con otros servicios de Azure
documentationcenter: ''
services: storage
author: normesta
ms.component: data-lake-storage-gen2
ms.service: storage
ms.devlang: na
ms.topic: conceptual
ms.date: 01/04/2019
ms.author: nitinme
ms.openlocfilehash: 7bc4889403e1d52cfa3b18792a554f0faf605132
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55885546"
---
# <a name="integrate-azure-data-lake-storage-gen2-with-other-azure-services"></a>Integración de Azure Data Lake Storage Gen2 con otros servicios de Azure

Puede utilizar los servicios de Azure para ingerir, analizar y visualizar datos en una cuenta de almacenamiento de Azure Data Lake Storage Gen2. Elija los servicios que mejor se adapten a las tareas que está intentando realizar.

## <a name="ingest-data-into-your-data-lake"></a>Ingesta de datos en Data Lake

Estos servicios le ayudan a incorporar datos en su instancia de Data Lake.

### <a name="azure-data-factory"></a>Azure Data Factory

Puede usar [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) para ingerir datos desde estos orígenes:

* Tablas de Azure
* Bases de datos SQL de Azure
* Azure SQL DataWarehouse
* Azure Storage Blob
* Bases de datos locales

Consulte [Move data to and from Data Lake Storage Gen2 using Data Factory](../../data-factory/connector-azure-data-lake-store.md) (Movimiento de datos a Lake Storage Gen2, y desde él, mediante Data Factory).

## <a name="analyze-and-visualize-data-in-your-data-lake"></a>Análisis y visualización de los datos de Data Lake

Estos servicios pueden usar Data Lake Storage Gen2 como punto de conexión de almacenamiento.

### <a name="azure-hdinsight"></a>HDInsight de Azure

Puede aprovisionar un clúster de [Azure HDInsight](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/) que use Data Lake Storage Gen2 como almacenamiento compatible con HDFS. En esta versión, con los clústeres Hadoop y Storm de Windows y Linux, solamente puede usar Data Lake Storage Gen2 como almacenamiento adicional. Dichos clústeres todavía usan Azure Storage (WASB) como almacenamiento predeterminado. Sin embargo, con los clústeres HBase de Windows y Linux, puede usar Data Lake Storage Gen2 como almacenamiento predeterminado y como almacenamiento adicional.

Consulte [Use Azure Data Lake Storage Gen2 with Azure HDInsight clusters](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2) (Uso de Azure Data Lake Storage Gen2 con clústeres de Azure HDInsight)

### <a name="azure-data-lake-analytics"></a>Análisis con Azure Data Lake

Pude utilizar [Azure Data Lake Analytics](../../data-lake-analytics/data-lake-analytics-overview.md) para trabajar con macrodatos en la nube. Esta herramienta aprovisiona los recursos de forma dinámica y permite analizar exabytes de datos. Data Lake Analytics está optimizado para usar Data Lake Storage Gen2 como origen de datos. 

Consulte [Get Started with Data Lake Analytics using Data Lake Storage Gen2](../../data-lake-analytics/data-lake-analytics-get-started-portal.md) (Introducción a Data Lake Analytics con Data Lake Storage Gen2).

## <a name="copy-data-to-other-repositories"></a>Copia de datos en otros repositorios

Utilice estos servicios para copiar datos de la instancia de Data Lake y pegarlos en otros repositorios.

### <a name="sql-data-warehouse"></a>SQL Data Warehouse

Puede usar PolyBase para cargar datos en SQL Data Warehouse desde Data Lake Storage Gen2. Para más información, consulte [Use Data Lake Storage Gen2 with SQL Data Warehouse](../../sql-data-warehouse/sql-data-warehouse-load-from-azure-data-lake-store.md) (Uso de Data Lake Storage Gen2 con SQL Data Warehouse).

## <a name="see-also"></a>Otras referencias

* [Introducción a Azure Data Lake Storage Gen2](data-lake-storage-introduction.md)
* [Using Azure Data Lake Storage Gen2 for big data requirements](data-lake-storage-data-scenarios.md) (Uso de Azure Data Lake Storage Gen2 para satisfacer los requisitos de los macrodatos)
