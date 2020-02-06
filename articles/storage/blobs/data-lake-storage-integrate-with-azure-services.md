---
title: Integración de Azure Data Lake Storage con los servicios de Azure | Microsoft Docs
description: Obtenga información sobre qué servicios de Azure se integran con Azure Data Lake Storage Gen2.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 11/01/2019
ms.author: normesta
ms.openlocfilehash: 7f43f69ebdac05b8f99739ea6b51453671b9a70a
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024558"
---
# <a name="integrate-azure-data-lake-storage-with-azure-services"></a>Integración de Azure Data Lake Storage con los servicios de Azure

Puede usar los servicios de Azure para ingerir datos, realizar análisis y crear representaciones visuales. En este artículo se proporciona una lista de los servicios de Azure admitidos y se proporcionan vínculos a artículos que le ayudarán a usar estos servicios con Azure Data Lake Storage Gen2.

## <a name="azure-services-that-support-azure-data-lake-storage-gen2"></a>Servicios de Azure que admiten Azure Data Lake Storage Gen2

En la tabla siguiente se enumeran los servicios de Azure que admiten Azure Data Lake Storage Gen2.

| Servicio de Azure |  Artículos relacionados |
|---------------|-------------------|
|Azure Data Factory | [Carga de datos en Azure Data Lake Storage Gen2 con Azure Data Factory](https://docs.microsoft.com/azure/data-factory/load-azure-data-lake-storage-gen2?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Azure Databricks | [Uso con Azure Databricks](https://docs.azuredatabricks.net/data/data-sources/azure/azure-datalake-gen2.html) <br> [Inicio rápido: Análisis de datos en Azure Data Lake Storage Gen2 con Azure Databricks](data-lake-storage-quickstart-create-databricks-account.md) <br>[Tutorial: Extracción, transformación y carga de datos mediante Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/databricks-extract-load-sql-data-warehouse) <br>[Tutorial: Acceso a los datos de Azure Data Lake Storage Gen2 con Azure Databricks mediante Spark](data-lake-storage-use-databricks-spark.md) |
|Azure Event Hubs Capture| [Captura de eventos a través de Azure Event Hubs en Azure Blob Storage o Azure Data Lake Storage](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview)|
|Azure Logic Apps | [Introducción: ¿Qué es Azure Logic Apps?](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview)|
|Azure Machine Learning|[Acceso a los datos en los servicios de almacenamiento de Azure](https://docs.microsoft.com/azure/machine-learning/how-to-access-data)|
|Azure Cognitive Search | [Indexación y búsqueda de documentos de Azure Data Lake Storage Gen2 (versión preliminar)](https://docs.microsoft.com/azure/search/search-howto-index-azure-data-lake-storage)|
|Azure Stream Analytics| [Inicio rápido: Creación de un trabajo de Stream Analytics mediante Azure Portal](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-quick-create-portal) <br> [Salida a Azure Data Lake Gen2](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-define-outputs#blob-storage-and-azure-data-lake-gen2) |
|Data Box|  [Uso de Azure Data Box para migrar datos de un almacén HDFS local a Azure Storage](data-lake-storage-migrate-on-premises-hdfs-cluster.md)|
|HDInsight | [Uso de Data Lake Storage Gen2 con clústeres de Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)<br>[Uso de la CLI de HDFS con Data Lake Storage Gen2](data-lake-storage-use-hdfs-data-lake-storage.md) <br>[Tutorial: Extracción, transformación y carga de datos mediante Apache Hive en Azure HDInsight](data-lake-storage-tutorial-extract-transform-load-hive.md) |
|IoT Hub | [Uso del enrutamiento de mensajes de IoT Hub para enviar mensajes del dispositivo a la nube a distintos puntos de conexión](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c)|
|Power BI|  [Análisis de datos en Data Lake Storage Gen2 mediante Power BI](https://docs.microsoft.com/power-query/connectors/datalakestorage) |
|SQL Data Warehouse | [Uso con Azure SQL Data Warehouse](https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#azure-sql-data-warehouse-polybase)|
|SQL Server Integration Services (SSIS) | [Administrador de conexiones de Azure Storage](https://docs.microsoft.com/sql/integration-services/connection-manager/azure-storage-connection-manager?view=sql-server-2017)|

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre las herramientas que puede usar para procesar los datos del lago de datos. Consulte [Uso de Azure Data Lake Storage Gen2 para requisitos de macrodatos](data-lake-storage-data-scenarios.md).

- Obtenga más información sobre Data Lake Storage Gen2 y cómo empezar a trabajar con él. Consulte [Introducción a Azure Data Lake Storage Gen2](data-lake-storage-introduction.md)
