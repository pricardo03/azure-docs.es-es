---
title: 'Conexión a diferentes orígenes de datos de Azure Databricks '
description: Aprenda a conectarse a Azure SQL Database, Azure Data Lake Store, Blob Storage, Cosmos DB, Event Hubs y Azure SQL Data Warehouse desde Azure Databricks.
services: azure-databricks
author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.workload: big-data
ms.topic: conceptual
ms.date: 03/21/2018
ms.author: mamccrea
ms.openlocfilehash: c77d1d1a66d3ee92f5ad3f2016d2160831fa3ad9
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2019
ms.locfileid: "72299314"
---
# <a name="connect-to-data-sources-from-azure-databricks"></a>Conectarse a orígenes de datos de Azure Databricks

En este artículo se proporcionan vínculos a todos los diferentes orígenes de datos de Azure que pueden conectarse a Azure Databricks. Siga los ejemplos que se muestran en estos vínculos para extraer datos de los orígenes de datos de Azure (por ejemplo, Azure Blob Storage, Azure Event Hubs, etc.) en un clúster de Azure Databricks, y ejecutar trabajos analíticos en ellos. 

## <a name="prerequisites"></a>Requisitos previos

* Debe disponer de un área de trabajo de Azure Databricks y de un clúster de Spark. Siga las instrucciones que se muestran en [Introducción a Azure Databricks](quickstart-create-databricks-workspace-portal.md).

## <a name="data-sources-for-azure-databricks"></a>Orígenes de datos de Azure Databricks

En la lista siguiente se proporcionan los orígenes de datos de Azure que puede usar con Azure Databricks. Para obtener una lista completa de los orígenes de datos que pueden usarse con Azure Databricks, consulte los [orígenes de datos de Azure Databricks](https://docs.azuredatabricks.net/spark/latest/data-sources/index.html).

- [Azure SQL Database](https://docs.azuredatabricks.net/spark/latest/data-sources/sql-databases.html)

    Este vínculo permite que la API de DataFrame se conecte a bases de datos SQL con JDBC e indica cómo controlar el paralelismo de lecturas a través de la interfaz JDBC. En este tema se proporcionan ejemplos detallados con la API de Scala, con ejemplos abreviados de Python y Spark al final.
- [Azure Data Lake Store](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake-gen2.html)

    En este vínculo se proporcionan ejemplos acerca de cómo utilizar la entidad de servicio de Azure Active Directory para autenticar con Data Lake Store. También proporciona instrucciones acerca de cómo obtener acceso a los datos de Data Lake Store desde Azure Databricks.

- [Azure Blob Storage](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-storage.html)

    En este vínculo se proporcionan ejemplos acerca de cómo obtener acceso directo a Azure Blob Storage desde Azure Databricks con la clave de acceso o la SAS de un determinado contenedor. El vínculo también proporciona información acerca de cómo obtener acceso a Azure Blob Storage desde Azure Databricks mediante la API de RDD.

- [Azure Cosmos DB](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/cosmosdb-connector.html)

    En este vínculo se proporcionan instrucciones acerca de cómo usar el [conector de Azure Cosmos DB Spark](https://github.com/Azure/azure-cosmosdb-spark) desde Azure Databricks para obtener acceso a los datos en de Azure Cosmos DB.

- [Azure Event Hubs](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/eventhubs-connector.html)

    En este vínculo se proporcionan instrucciones acerca de cómo usar el [conector de Azure Event Hubs Spark](https://github.com/Azure/azure-event-hubs-spark) desde Azure Databricks para obtener acceso a los datos de Azure Event Hubs.

- [Azure SQL Data Warehouse](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/sql-data-warehouse.html)

    En este vínculo se proporcionan instrucciones acerca de cómo usar el conector de Azure SQL Data Warehouse para conectarse desde Azure Databricks.
    

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de los orígenes desde donde se pueden importar datos a Azure Databricks, consulte los [orígenes de datos de Azure Databricks](https://docs.azuredatabricks.net/spark/latest/data-sources/index.html#).


