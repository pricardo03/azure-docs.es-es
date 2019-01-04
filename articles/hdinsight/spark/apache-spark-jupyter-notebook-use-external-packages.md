---
title: Uso de paquetes personalizados de Maven con Jupyter en Spark en Azure HDInsight
description: Instrucciones detalladas sobre cómo configurar cuadernos de Jupyter Notebook disponibles con clústeres de HDInsight Spark para usar paquetes personalizados de Maven.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/09/2018
ms.author: hrasheed
ms.openlocfilehash: c0394025fef15b987f0777d157c465e83d860601
ms.sourcegitcommit: e68df5b9c04b11c8f24d616f4e687fe4e773253c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/20/2018
ms.locfileid: "53652139"
---
# <a name="use-external-packages-with-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight"></a>Uso de paquetes externos con cuadernos de Jupyter en clústeres de Apache Spark en HDInsight
> [!div class="op_single_selector"]
> * [Uso de magic cell](apache-spark-jupyter-notebook-use-external-packages.md)
> * [Uso de acciones de script](apache-spark-python-package-installation.md)

Aprenda a configurar una instancia de [Jupyter Notebook](https://jupyter.org/) en clústeres de Apache Spark en HDInsight para usar paquetes **maven** externos aportados por la comunidad que no se incluyan listos para utilizarse en el clúster. 

Puede buscar el [repositorio de Maven](https://search.maven.org/) para obtener una lista completa de los paquetes que están disponibles. También puede obtener una lista de paquetes disponibles de otras fuentes. Por ejemplo, dispone de la lista completa de los paquetes externos aportados por la comunidad en [Spark Packages](https://spark-packages.org/)(Paquetes Spark).

En este artículo, aprenderá a utilizar el paquete [spark csv](https://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) con el cuaderno de Jupyter Notebook.

## <a name="prerequisites"></a>Requisitos previos
Debe tener lo siguiente:

* Un clúster de Apache Spark en HDInsight. Para obtener instrucciones, vea [Creación de clústeres Apache Spark en HDInsight de Azure](apache-spark-jupyter-spark-sql.md).

## <a name="use-external-packages-with-jupyter-notebooks"></a>Uso de paquetes externos con cuadernos de Jupyter Notebook
1. Desde [Azure Portal](https://portal.azure.com/), en el panel de inicio, haga clic en el icono del clúster Spark (si lo ancló al panel de inicio). También puede navegar hasta el clúster en **Examinar todo** > **Clústeres de HDInsight**.   

1. En la hoja del clúster Spark, haga clic en **Vínculos rápidos** y, luego, en la hoja **Panel de clúster**, haga clic en **Jupyter Notebook**. Cuando se le pida, escriba las credenciales del clúster.

    > [!NOTE]  
    > También puede comunicarse con el equipo Jupyter Notebook en el clúster si abre la siguiente dirección URL en el explorador. Reemplace **CLUSTERNAME** por el nombre del clúster:
    > 
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

1. Cree un nuevo notebook. Haga clic en **Nuevo** y luego en **Spark**.
   
    ![Crear un nuevo cuaderno de Jupyter](./media/apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-create-notebook.png "Crear un nuevo cuaderno de Jupyter")

1. Se crea y se abre un nuevo cuaderno con el nombre Untitled.pynb. Haga clic en el nombre del cuaderno en la parte superior y escriba un nombre descriptivo.
   
    ![Proporcionar un nombre para el cuaderno](./media/apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-name-notebook.png "Proporcionar un nombre para el cuaderno")

1. Utilizará la instrucción mágica `%%configure` para configurar el cuaderno para usar un paquete externo. En los cuadernos que utilizan paquetes externos, asegúrese de invocar la instrucción mágica `%%configure` en la primera celda de código. Esto garantiza que el kernel se configure para utilizar el paquete antes de iniciar la sesión.

    >[!IMPORTANT]  
    >Si se olvida de configurar el kernel en la primera celda, puede utilizar el parámetro `%%configure` con el parámetro `-f`, pero ello reiniciará la sesión y se perderá todo el trabajo.

    | Versión de HDInsight | Get-Help |
    |-------------------|---------|
    |Para HDInsight 3.3 y HDInsight 3.4 | `%%configure` <br>`{ "packages":["com.databricks:spark-csv_2.10:1.4.0"] }`|
    | Para HDInsight 3.5 y HDInsight 3.6 | `%%configure`<br>`{ "conf": {"spark.jars.packages": "com.databricks:spark-csv_2.10:1.4.0" }}`|

1. En el fragmento de código anterior, se esperan las coordenadas de Maven correspondientes al paquete externo de Maven Central Repository. En este fragmento de código, `com.databricks:spark-csv_2.10:1.4.0` es la coordenada de Maven para el paquete **spark-csv** . Le mostramos cómo crear las coordenadas de un paquete.
   
     a. Busque el paquete en el repositorio de Maven. En este tutorial, utilizamos [spark-csv](https://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).
   
    b. En el repositorio, recopile los valores de **GroupId**, **ArtifactId** y **Version**. Asegúrese de que los valores recopilados coincidan con el clúster. En este caso, estamos usando un paquete de Scala 2.10 y Spark 1.4.0, pero quizás tenga que seleccionar versiones diferentes para la versión de Scala o Spark concreta del clúster. Puede averiguar la versión de Scala del clúster mediante la ejecución de `scala.util.Properties.versionString` en el kernel de Spark Jupyter o en el envío de Spark. Puede averiguar la versión de Spark del clúster mediante la ejecución de `sc.version` en Jupyter Notebooks.
   
    ![Uso de paquetes externos con cuadernos de Jupyter](./media/apache-spark-jupyter-notebook-use-external-packages/use-external-packages-with-jupyter.png "Uso de paquetes externos con cuadernos de Jupyter")
   
    c. Concatene los tres valores separados por dos puntos (**:**).
   
        com.databricks:spark-csv_2.10:1.4.0

1. Ejecute la celda de código con la instrucción mágica `%%configure` . De esta forma, se configurará la sesión de Livy subyacente para utilizar el paquete que facilitó. En las siguientes celdas del cuaderno, ya podrá usar el paquete como se muestra a continuación.
   
        val df = sqlContext.read.format("com.databricks.spark.csv").
        option("header", "true").
        option("inferSchema", "true").
        load("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

    Para HDInsight 3.6, debe usar este fragmento de código.

        val df = spark.read.format("com.databricks.spark.csv").
        option("header", "true").
        option("inferSchema", "true").
        load("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

1. A continuación, podrá ejecutar los fragmentos de código como se muestra seguidamente para ver los datos de la trama de datos que creó en el paso anterior.
   
        df.show()
   
        df.select("Time").count()

## <a name="seealso"></a>Consulte también
* [Información general: Apache Spark en Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Escenarios
* [Apache Spark con BI: Análisis de datos interactivos con Spark en HDInsight con las herramientas de BI](apache-spark-use-bi-tools.md)
* [Apache Spark con Machine Learning: uso de Apache Spark en HDInsight para analizar la temperatura de edificios con los datos del sistema de acondicionamiento de aire](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark con Machine Learning: uso de Spark en HDInsight para predecir los resultados de la inspección de alimentos](apache-spark-machine-learning-mllib-ipython.md)
* [Análisis de registros de un sitio web mediante Apache Spark en HDInsight](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Creación y ejecución de aplicaciones
* [Crear una aplicación independiente con Scala](apache-spark-create-standalone-application.md)
* [Ejecución de trabajos de forma remota en un clúster de Apache Spark mediante Apache Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Herramientas y extensiones

* [Uso de paquetes externos de Python con cuadernos de Jupyter Notebook en clústeres de Apache Spark en HDInsight Linux](apache-spark-python-package-installation.md)
* [Uso del complemento de herramientas de HDInsight para IntelliJ IDEA para crear y enviar aplicaciones de Spark Scala](apache-spark-intellij-tool-plugin.md)
* [Use HDInsight Tools Plugin for IntelliJ IDEA to debug Apache Spark applications remotely](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md) (Uso del complemento de herramientas de HDInsight para IntelliJ IDEA para depurar aplicaciones de Apache Spark de forma remota)
* [Uso de cuadernos de Apache Zeppelin con un clúster de Apache Spark en HDInsight](apache-spark-zeppelin-notebook.md)
* [Kernels disponible para Jupyter Notebook en clústeres Apache Spark para HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Instalación de un cuaderno de Jupyter Notebook en el equipo y conexión al clúster de Apache Spark en HDInsight de Azure](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Administración de recursos
* [Administración de recursos para el clúster Apache Spark en HDInsight de Azure](apache-spark-resource-manager.md)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight (Seguimiento y depuración de trabajos que se ejecutan en un clúster de Apache Spark en HDInsight)](apache-spark-job-debugging.md)
