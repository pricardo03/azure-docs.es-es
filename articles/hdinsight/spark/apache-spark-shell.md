---
title: Uso de un Shell de Spark interactivo en Azure HDInsight
description: Un Shell de Spark interactivo proporciona un proceso de impresión de lectura-ejecución para ejecutar comandos de Spark puntuales y ver los resultados.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/12/2019
ms.openlocfilehash: f088b8210b8170d22e84d131f0a72f5f8caa3b92
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75435221"
---
# <a name="run-apache-spark-from-the-spark-shell"></a>Ejecución de Apache Spark desde el shell de Spark

Un shell interactivo de [Apache Spark](https://spark.apache.org/) proporciona un entorno REPL (bucle leer, ejecutar e imprimir) para ejecutar comandos Spark de uno en uno y ver los resultados. Este proceso es útil para tareas de desarrollo y depuración. Spark proporciona un shell para cada uno de los lenguajes que admite: Scala, Python y R.

## <a name="run-an-apache-spark-shell"></a>Ejecución del shell de Apache Spark

1. Use el [comando SSH](../hdinsight-hadoop-linux-use-ssh-unix.md) para conectarse al clúster. Modifique el comando siguiente: reemplace CLUSTERNAME por el nombre del clúster y, luego, escriba el comando:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Spark proporciona shells para Scala (spark-shell) y Python (pyspark). En su sesión SSH, escriba uno de los siguientes comandos:

    ```bash
    spark-shell
    pyspark
    ```

    Ahora puede escribir comandos de Spark en el lenguaje adecuado.

1. Algunos comandos básicos de ejemplo:

    ```scala
    // Load data
    var data = spark.read.format("csv").option("header", "true").option("inferSchema", "true").load("/HdiSamples/HdiSamples/SensorSampleData/building/building.csv")

    // Show data
    data.show()

    // Select certain columns
    data.select($"BuildingID", $"Country").show(10)

    // exit shell
    :q
    ```

## <a name="sparksession-and-sparkcontext-instances"></a>Instancias de objetos SparkSession y SparkContext

De forma predeterminada, cuando se ejecute el shell de Spark, se crea automáticamente las instancias de los objetos SparkSession y SparkContext.

Para acceder a la instancia de SparkSession, escriba `spark`. Para acceder a la instancia de SparkContext, escriba `sc`.

## <a name="important-shell-parameters"></a>Parámetros de shell importantes

El comando de shell de Spark (`spark-shell` o `pyspark`) admite muchos parámetros de línea de comandos. Para ver una lista completa de parámetros, inicie el shell de Spark con el modificador `--help`. Algunos de estos parámetros solo se pueden aplicar a `spark-submit`, que es el que incluye el shell de Spark.

| Modificador | description | ejemplo |
| --- | --- | --- |
| --master MASTER_URL | Especifica la URL principal. En HDInsight, este valor es siempre `yarn`. | `--master yarn`|
| --jars JAR_LIST | Lista separada por comas de archivos JAR locales que se incluirán en las rutas de clase del controlador y el ejecutor. En HDInsight, esta lista se compone de las rutas de acceso al sistema de archivos predeterminado en Azure Storage o Data Lake Storage. | `--jars /path/to/examples.jar` |
| --packages MAVEN_COORDS | Lista separada por comas de coordenadas de Maven de archivos JAR locales que se incluirán en las rutas de clase del controlador y el ejecutor. Busca en el repositorio de Maven local, luego, en el central y, después, en cualquier repositorio remoto adicional especificado con `--repositories`. El formato de las coordenadas es *idGrupo*:*idArtefacto*:*versión*. | `--packages "com.microsoft.azure:azure-eventhubs:0.14.0"`|
| --py-files LIST | Solo para Python; lista separada por comas de los archivos .zip, .egg o .py que se colocarán en PYTHONPATH. | `--pyfiles "samples.py"` |

## <a name="next-steps"></a>Pasos siguientes

- Para información general, consulte [Introducción a Apache Spark en Azure HDInsight](apache-spark-overview.md).
- Si quiere trabajar con SparkSQL y los clústeres de Spark, lea [Creación de un clúster de Apache Spark en Azure HDInsight](apache-spark-jupyter-spark-sql.md).
- Para escribir aplicaciones que procesen datos de streaming con Spark, consulte el artículo que explica [qué es el streaming estructurado de Apache Spark](apache-spark-streaming-overview.md).
