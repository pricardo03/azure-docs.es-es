---
title: 'Visualización de datos de Apache Hive con Power BI: Azure HDInsight'
description: Aprenda a usar Microsoft Power BI para visualizar datos de Hive procesados por Azure HDInsight.
keywords: hdinsight,hadoop,hive,consulta interactiva,hive interactivo,LLAP,odbc
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: hrasheed
ms.openlocfilehash: 69353968f6b38f0d16b68c58b9b00c3e6d45850b
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446871"
---
# <a name="visualize-apache-hive-data-with-microsoft-power-bi-using-odbc-in-azure-hdinsight"></a>Visualización de los datos de Apache Hive con Microsoft Power BI mediante ODBC en Azure HDInsight

Aprenda a conectar Microsoft Power BI Desktop a Azure HDInsight mediante ODBC y a visualizar los datos de Apache Hive.

>[!IMPORTANT]
> Puede aprovechar el controlador ODBC de Hive para importar a través del conector ODBC genérico en Power BI Desktop. Sin embargo, no se recomienda para las cargas de trabajo de BI dada la naturaleza no interactiva del motor de consulta de Hive. El [conector de HDInsight Interactive Query](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md) y el [conector de HDInsight Spark ](https://docs.microsoft.com/power-bi/spark-on-hdinsight-with-direct-connect) son mejores soluciones para su rendimiento.

En este artículo, cargaremos los datos de una tabla de Hive (`hivesampletable`) en Power BI. La tabla de Hive contiene algunos datos de uso de teléfonos móviles. Luego se trazan los datos de uso en un mapamundi:

![Informe de mapa de Power BI en HDInsight](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-visualization.png)

La información también se aplica al nuevo tipo de clúster de [consulta interactiva](../interactive-query/apache-interactive-query-get-started.md). Para saber cómo conectarse a HDInsight Interactive Query mediante DirectQuery, consulte [Visualización de datos de Interactive Query Hive con Power BI mediante DirectQuery en Azure HDInsight](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).

## <a name="prerequisites"></a>Requisitos previos

Antes de seguir los pasos de este artículo, debe disponer de los siguientes elementos:

* **Clúster de HDInsight**. El clúster puede ser un clúster de HDInsight con Hive o un clúster de consulta interactiva recién lanzado. Para crear clústeres, vea [Crear clúster](apache-hadoop-linux-tutorial-get-started.md#create-cluster).

* **[Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/)** . Puede descargar una copia desde el [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=45331).

## <a name="create-hive-odbc-data-source"></a>Creación de un origen de datos de Hive ODBC

Vea [Creación de un origen de datos de Hive ODBC](apache-hadoop-connect-excel-hive-odbc-driver.md#create-apache-hive-odbc-data-source).

## <a name="load-data-from-hdinsight"></a>Carga de datos desde HDInsight

La tabla de Hive hivesampletable incluye todos los clústeres de HDInsight.

1. Inicie Power BI Desktop.

2. En el menú superior, vaya a **Inicio** > **Obtener datos** > **Más...**

    ![Datos abiertos de Power BI en HDInsight](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-open-odbc.png)

3. En el cuadro de diálogo **Obtener datos**, seleccione **Otro** a la izquierda, **ODBC** a la derecha y, luego, **Conectar** en la parte inferior.

4. En el cuadro de diálogo **Desde ODBC**, seleccione en la lista desplegable el nombre del origen de datos que creó en la sección anterior y, luego, seleccione **Aceptar**.

5. En el cuadro de diálogo **Navegador**, expanda **ODBC > HIVE > Predeterminado**, seleccione **hivesampletable** y, después, seleccione **Cargar**.

6. En el cuadro de diálogo **Controlador de ODBC**, seleccione **Predeterminado o Personalizado** y, luego, seleccione **Conectar**.

## <a name="visualize-data"></a>Visualización de datos

Continúe a partir del último procedimiento.

1. En el panel Visualizaciones, seleccione **Mapa**.  Es un icono de globo terráqueo.

    ![HDInsight Power BI personaliza el informe](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-customize.png)
2. En el panel **Campos**, seleccione **country** y **devicemake**. Puede ver los datos trazados en el mapa.
3. Expanda el mapa.

## <a name="next-steps"></a>Pasos siguientes

En este artículo ha aprendido a visualizar datos de HDInsight mediante Power BI.  Para obtener más información, consulte los artículos siguientes:

* [Uso de Apache Zeppelin para ejecutar consultas de Apache Hive en Azure HDInsight](../interactive-query/hdinsight-connect-hive-zeppelin.md).
* [Conexión de Excel a Hadoop en Azure HDInsight con Microsoft Hive ODBC Driver](./apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Conexión de Excel a Apache Hadoop con Power Query](apache-hadoop-connect-excel-power-query.md).
* [Conectarse a Azure HDInsight y ejecutar consultas de Apache Hive con Herramientas de Data Lake para Visual Studio](apache-hadoop-visual-studio-tools-get-started.md).
* [Uso de Herramientas de Azure HDInsight para Visual Studio Code](../hdinsight-for-vscode.md).
* [Carga de datos en HDInsight](./../hdinsight-upload-data.md).
