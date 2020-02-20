---
title: ¿Qué es Interactive Query en Azure HDInsight?
description: Introducción a Interactive Query, también denominado Apache Hive LLAP, en Azure HDInsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: overview
ms.date: 09/17/2019
ms.openlocfilehash: f03797a8c7df1609a32f934bc090c7adc899aa9a
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2020
ms.locfileid: "77198929"
---
# <a name="what-is-interactive-query-in-azure-hdinsight"></a>¿Qué es Interactive Query en Azure HDInsight?

Interactive Query, también llamado Apache Hive LLAP o [Low Latency Analytical Processing](https://cwiki.apache.org/confluence/display/Hive/LLAP) (Procesamiento analítico de baja latencia), es un nuevo [tipo de clúster](../hdinsight-hadoop-provision-linux-clusters.md#cluster-type) de Azure HDInsight. Interactive Query admite el almacenamiento en el caché de la memoria, lo que hace que las consultas de Apache Hive sean más rápidas y mucho más interactivas. Los clientes usan Interactive Query para consultar muy rápidamente los datos almacenados en Azure Storage y Azure Data Lake Storage. Interactive Query facilita que los desarrolladores y los científicos de datos trabajen con los macrodatos mediante las herramientas de BI que prefieran. Interactive Query de HDInsight admite varias herramientas para acceder a los macrodatos con facilidad.

[!INCLUDE [hdinsight-price-change](../../../includes/hdinsight-enhancements.md)]

Un clúster de Interactive Query no es igual que un clúster de Apache Hadoop. Solo contiene el servicio de Hive.

Solo se puede obtener acceso al servicio Hive en el clúster de Interactive Query mediante la vista Apache Ambari Hive, Beeline y el controlador de conectividad abierta de bases de datos de Microsoft Hive (Hive ODBC). No se puede acceder a él mediante la consola de Hive, Templeton, la CLI de Azure clásica ni Azure PowerShell.

## <a name="create-an-interactive-query-cluster"></a>Creación de un clúster de Interactive Query

Para obtener más información sobre cómo crear un clúster de HDInsight, consulte [Create Apache Hadoop clusters in HDInsight](../hdinsight-hadoop-provision-linux-clusters.md) (Creación de clústeres de Apache Hadoop en HDInsight). Elija el tipo de clúster de Interactive Query.

> [!IMPORTANT]
> El tamaño mínimo del nodo principal de los clústeres de Interactive Query es Standard_D13_v2. Para más información, consulte [Gráfico de tamaños de máquinas virtuales de Azure](../../cloud-services/cloud-services-sizes-specs.md#dv2-series).

## <a name="execute-apache-hive-queries-from-interactive-query"></a>Ejecutar consultas de Apache Hive desde Interactive Query

Para ejecutar consultas de Hive, tiene las siguientes opciones:

* Uso de Microsoft Power BI

    Consulte [Visualize Interactive Query Apache Hive data with Power BI in Azure HDInsight](./apache-hadoop-connect-hive-power-bi-directquery.md) (Visualización de datos de Interactive Query Apache Hive con Power BI en Azure HDInsight) y [Visualize big data with Power BI in Azure HDInsight](../hadoop/apache-hadoop-connect-hive-power-bi.md) (Visualizar macrodatos con Power BI en Azure HDInsight).

* Usar Visual Studio

    Consulte [Conectarse a Azure HDInsight y ejecutar consultas de Apache Hive con Herramientas de Data Lake para Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md#run-interactive-apache-hive-queries).

* Usar Visual Studio Code

    Consulte el artículo de [Uso de Visual Studio Code para Apache Hive, LLAP o pySpark](../hdinsight-for-vscode.md).
* Ejecute Apache Hive con la vista Apache Ambari Hive.
  
    Consulte [Use Apache Hive View with Apache Hadoop in Azure HDInsight](../hadoop/apache-hadoop-use-hive-ambari-view.md) (Uso de la vista de Apache Hive con Apache Hadoop en Azure HDInsight).

* Ejecute Apache Hive mediante Beeline.
  
    Consulte [Use Apache Hive with Apache Hadoop in HDInsight with Beeline](../hadoop/apache-hadoop-use-hive-beeline.md) (Uso de Apache Hive con Apache Hadoop en HDInsight con Beeline).
  
    Puede usar Beeline desde el nodo principal o desde un nodo perimetral vacío. Se recomienda usar Beeline en un nodo perimetral vacío. Para más información sobre la creación de un clúster de HDInsight con un nodo perimetral vacío, consulte [Uso de nodos perimetrales vacíos en HDInsight](../hdinsight-apps-use-edge-node.md).
* Ejecute Apache Hive mediante la conectividad abierta de bases de datos de Hive.
  
    Consulte [Connect Excel to Apache Hadoop with the Microsoft Hive ODBC driver](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md) (Conexión de Excel a Apache Hadoop con Microsoft Hive ODBC Driver).

Para buscar la cadena de conexión de Java Database Connectivity (JDBC):

1. Inicie sesión en Apache Ambari mediante la siguiente dirección URL: `https://<cluster name>.AzureHDInsight.net`.
2. En el menú de la izquierda, seleccione **Hive**.
3. Para copiar la dirección URL, seleccione el icono del Portapapeles:

   ![HDInsight Hadoop Interactive Query LLAP JDBC](./media/apache-interactive-query-get-started/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="next-steps"></a>Pasos siguientes

* Aprenda a [crear clústeres de Interactive Query en HDInsight](../hdinsight-hadoop-provision-linux-clusters.md).
* Aprenda a [visualizar datos de Hive con Microsoft Power BI en Azure HDInsight](../hadoop/apache-hadoop-connect-hive-power-bi.md).
* Aprenda a [usar Apache Zeppelin para ejecutar consultas de Apache Hive en Azure HDInsight](../interactive-query/hdinsight-connect-hive-zeppelin.md).
