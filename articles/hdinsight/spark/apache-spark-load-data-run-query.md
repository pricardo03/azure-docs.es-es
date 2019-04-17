---
title: 'Tutorial: Carga de datos y ejecución de consultas en un clúster de Apache Spark en Azure HDInsight '
description: Aprenda a cargar datos y a ejecutar consultas interactivas en clústeres de Spark en Azure HDInsight.
services: azure-hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,mvc
ms.topic: tutorial
ms.author: hrasheed
ms.date: 04/03/2019
ms.openlocfilehash: 18f5d34e50a4ed4ed82a3ceb4740d594ce4bd78d
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59274015"
---
# <a name="tutorial-load-data-and-run-queries-on-an-apache-spark-cluster-in-azure-hdinsight"></a>Tutorial: Carga de datos y ejecución de consultas en un clúster de Apache Spark en Azure HDInsight

En este tutorial, aprenderá a crear una trama de datos a partir de un archivo CSV y a ejecutar consultas SQL de Spark interactivas en un clúster de [Apache Spark](https://spark.apache.org/) en Azure HDInsight. En Spark, una trama de datos es una colección distribuida de datos que se organizan en columnas con nombre. Una trama de datos es conceptualmente equivalente a una tabla de una base de datos relacional o a una trama de datos de R/Python.
 
En este tutorial, aprenderá a:
> [!div class="checklist"]
> * Creación de una trama de datos a partir de un archivo csv
> * Ejecución de consultas en la trama de datos

## <a name="prerequisites"></a>Requisitos previos

* Complete [Creación de un clúster de Apache Spark en Azure HDInsight](apache-spark-jupyter-spark-sql.md).

## <a name="create-a-dataframe-from-a-csv-file"></a>Creación de una trama de datos a partir de un archivo csv

Las aplicaciones pueden crear tramas de datos directamente desde archivos o carpetas del almacenamiento remoto, como Azure Storage o Azure Data Lake Storage; desde una tabla de Hive; o desde otros orígenes de datos compatibles con Spark, como Cosmos DB, Azure SQL DB, DW, etc. La captura de pantalla siguiente muestra una instantánea del archivo HVAC.csv que se usa en este tutorial. El archivo csv incluye todos los clústeres de HDInsight Spark. Los datos capturan las variaciones de temperatura de varios edificios.
    
![Instantánea de los datos de consulta SQL interactiva de Spark](./media/apache-spark-load-data-run-query/hdinsight-spark-sample-data-interactive-spark-sql-query.png "Instantánea de los datos de consultas SQL interactivas de Spark")


1. Abra la instancia de Jupyter Notebook que creó en la sección de requisitos previos y cree un nuevo cuaderno con PySpark.

    > [!NOTE]  
    > Si se usa el kernel de PySpark para crear un cuaderno, la sesión `spark` se crea automáticamente al ejecutar la primera celda de código. No es necesario crear la sesión explícitamente.

2. Pegue el código siguiente en una celda vacía del cuaderno y presione **MAYÚS + ENTRAR** para ejecutarlo. El código importa los tipos necesarios para este escenario:

    ```python
    from pyspark.sql import *
    from pyspark.sql.types import *
    ```

    Al ejecutar una consulta interactiva en Jupyter, la ventana del explorador web o la leyenda de la pestaña muestran el estado **(Busy)** [(Ocupado)] junto con el título del cuaderno. También verá un círculo sólido junto al texto **PySpark** en la esquina superior derecha. Cuando finaliza el trabajo, cambia a un círculo vacío.

    ![Estado de consulta Spark SQL interactiva](./media/apache-spark-load-data-run-query/hdinsight-spark-interactive-spark-query-status.png "Estado de consulta Spark SQL interactiva")

3. Ejecute el código siguiente para crear una trama de datos y una tabla temporal (**hvac**). 

    ```python
    # Create a dataframe and table from sample data
    csvFile = spark.read.csv('/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv', header=True, inferSchema=True)
    csvFile.write.saveAsTable("hvac")
    ```

## <a name="run-queries-on-the-dataframe"></a>Ejecución de consultas en la trama de datos

Una vez creada la tabla, puede ejecutar una consulta interactiva en los datos.

1. Ejecute el siguiente código en una celda vacía del cuaderno:

    ```sql
    %%sql
    SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"
    ```

   Se muestra la siguiente salida tabular.

     ![Tabla de salida de resultados de consultas Spark interactivas](./media/apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result.png "Tabla de salida de resultados de consultas Spark interactivas")

2. También puede ver la salida en otras visualizaciones. Para ver un gráfico de áreas para la misma salida, seleccione **Área** y establezca otros valores tal como se muestra.

    ![Gráfico de área de resultados de consultas Spark interactivas](./media/apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result-area-chart.png "Gráfico de área de resultados de consultas Spark interactivas")

3. En la barra de menús de cuaderno, vaya a **Archivo** > **Save and Checkpoint** (Guardar y punto de control).

4. Si ahora va a empezar con el [siguiente tutorial](apache-spark-use-bi-tools.md), deje el bloc de notas abierto. Si no va a hacerlo, cierre el cuaderno para liberar los recursos de clúster: en la barra de menús del cuaderno, vaya a **Archivo** >  **Close and Halt** (Cerrar y detener).

## <a name="clean-up-resources"></a>Limpieza de recursos

Con HDInsight, los datos y los cuadernos de Jupyter se almacenan en Azure Storage o Azure Data Lake Storage, por lo que puede eliminar de forma segura un clúster cuando no esté en uso. También se le cobrará por un clúster de HDInsight aunque no se esté usando. Como en muchas ocasiones los cargos por el clúster son mucho más elevados que los cargos por el almacenamiento, desde el punto de vista económico tiene sentido eliminar clústeres cuando no se estén usando. Si tiene previsto pasar inmediatamente al siguiente tutorial, es aconsejable que no elimine el clúster.

Abra el clúster en Azure Portal y seleccione **Eliminar**.

![Eliminar clúster de HDInsight](./media/apache-spark-load-data-run-query/hdinsight-azure-portal-delete-cluster.png "Eliminar clúster de HDInsight")

También puede seleccionar el nombre del grupo de recursos para abrir la página del grupo de recursos y, después, hacer clic en **Eliminar grupo de recursos**. Al eliminar el grupo de recursos, se eliminan tanto el clúster de HDInsight Spark como la cuenta de almacenamiento predeterminada.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:
> [!div class="checklist"]
> * A crear una trama de datos de Apache Spark.
> * Ejecutar Spark SQL en la trama de datos.

En el siguiente artículo podrá ver cómo extraer en una herramienta de análisis de BI, como Power BI, los datos que registró en Apache Spark. 
> [!div class="nextstepaction"]
> [Análisis de datos mediante herramientas de inteligencia empresarial](apache-spark-use-bi-tools.md)
