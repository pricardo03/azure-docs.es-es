---
title: Uso de Apache Zeppelin para ejecutar consultas de Apache Hive en Azure HDInsight
description: Aprenda a usar Apache Zeppelin para ejecutar consultas de Apache Hive.
keywords: hdinsight,hadoop,hive,interactive query,LLAP
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,
ms.topic: conceptual
ms.date: 11/05/2018
ms.author: hrasheed
ms.openlocfilehash: 035e70eef88d5d5dae08c329017430db25c20464
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "52494821"
---
# <a name="use-apache-zeppelin-to-run-apache-hive-queries-in-azure-hdinsight"></a>Uso de Apache Zeppelin para ejecutar consultas de Apache Hive en Azure HDInsight 

Los clústeres de HDInsight Interactive Query incluyen cuadernos de [Apache Zeppelin](https://zeppelin.apache.org/) con los que puede ejecutar consultas de Hive interactivas. Con este artículo aprenderá a usar Apache Zeppelin para ejecutar consultas de [Apache Hive](https://hive.apache.org/) en Azure HDInsight. 

## <a name="prerequisites"></a>Requisitos previos
Antes de seguir los pasos de este artículo, debe disponer de los siguientes elementos:

* **Clúster de HDInsight Interactive Query**. Vea [Crear clúster](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster) para crear un clúster de HDInsight.  Asegúrese de elegir el tipo Interactive Query. 

## <a name="create-an-apache-zeppelin-note"></a>Creación de una nota de Apache Zeppelin

1. Vaya a esta dirección URL:

        https://CLUSTERNAME.azurehdinsight.net/zeppelin
    Reemplace **CLUSTERNAME** por el nombre del clúster.

2. Escriba el nombre de usuario y la contraseña de Hadoop. En la página de Zeppelin, puede crear una nota o abrir notas existentes. HiveSample contiene algunos ejemplos de consultas de Hive.  

    ![Zeppelin de HDInsight Interactive Query](./media/hdinsight-connect-hive-zeppelin/hdinsight-hive-zeppelin.png)
3. Haga clic en **Create new Note** (Crear nueva nota).
4. Escriba o seleccione los valores siguientes:

    - Nombre de la nota: escriba un nombre para la nota.
    - Default interpreter (Intérprete predeterminado): seleccione **JDBC**.

5. Haga clic en **Crear nota**.
6. Ejecute esta consulta de Hive:

        %jdbc(hive)
        show tables

    ![Zeppelin de HDInsight Interactive Query ejecuta una consulta](./media/hdinsight-connect-hive-zeppelin/hdinsight-hive-zeppelin-query.png)

    La instrucción **%jdbc(hive)** en la primera línea indica al bloc de notas que debe usar el intérprete JDBC de Hive.

    La consulta devolverá una tabla de Hive denominada *hivesampletable*.

    Aquí se muestran dos consultas más de Hive que se pueden ejecutar en hivesampletable. 

        %jdbc(hive)
        select * from hivesampletable limit 10

        %jdbc(hive)
        select ${group_name}, count(*) as total_count
        from hivesampletable
        group by ${group_name=market,market|deviceplatform|devicemake}
        limit ${total_count=10}

    Comparado con Hive tradicional, los resultados de la consulta se devuelven mucho más rápido.


## <a name="next-steps"></a>Pasos siguientes
En este artículo ha aprendido a visualizar datos de HDInsight mediante [Microsoft Power BI](https://powerbi.microsoft.com/).  Para obtener más información, consulte los artículos siguientes:

* [Visualización de datos de Apache Hive con Microsoft Power BI en Azure HDInsight](hadoop/apache-hadoop-connect-hive-power-bi.md).
* [Visualización de datos de Interactive Query Apache Hive con Power BI en Azure HDInsight](./interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).
* [Conexión de Excel a Hadoop en Azure HDInsight con Microsoft Hive ODBC Driver](hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Conexión de Excel a Apache Hadoop con Power Query](hadoop/apache-hadoop-connect-excel-power-query.md).
* [Conectarse a Azure HDInsight y ejecutar consultas de Apache Hive con Herramientas de Data Lake para Visual Studio](hadoop/apache-hadoop-visual-studio-tools-get-started.md).
* [Uso de Herramientas de Azure HDInsight para Visual Studio Code](hdinsight-for-vscode.md).
* [Carga de datos en HDInsight](./hdinsight-upload-data.md).
