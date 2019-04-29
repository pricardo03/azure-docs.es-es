---
title: Uso de un Shell de Spark interactivo en Azure HDInsight
description: Un Shell de Spark interactivo proporciona un proceso de impresión de lectura-ejecución para ejecutar comandos de Spark puntuales y ver los resultados.
ms.service: hdinsight
author: maxluk
ms.author: maxluk
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/09/2018
ms.openlocfilehash: 9044ed3ad9cf9ffa2f54d130bb50b37df121b86f
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62116080"
---
# <a name="run-apache-spark-from-the-spark-shell"></a>Ejecución de Apache Spark desde el shell de Spark

Un shell interactivo de [Apache Spark](https://spark.apache.org/) proporciona un entorno REPL (bucle leer, ejecutar e imprimir) para ejecutar comandos Spark de uno en uno y ver los resultados. Este proceso es útil para tareas de desarrollo y depuración. Spark proporciona un shell para cada uno de los lenguajes que admite: Scala, Python y R.

## <a name="get-to-an-apache-spark-shell-with-ssh"></a>Obtención de un shell de Apache Spark con SSH

Acceda al shell de Apache Spark en HDInsight mediante la conexión del nodo principal del clúster con SSH:

     ssh <sshusername>@<clustername>-ssh.azurehdinsight.net

Puede obtener el comando SSH completo del clúster en Azure Portal:

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. Acceda al panel del clúster de HDInsight Spark.
3. Seleccione Secure Shell (SSH).

    ![Panel de HDInsight en Azure Portal](./media/apache-spark-shell/hdinsight-spark-blade.png)

4. Copie el comando SSH que se muestra y ejecútelo en el terminal.

    ![Panel SSH de HDInsight en Azure Portal](./media/apache-spark-shell/hdinsight-spark-ssh-blade.png)

Para obtener más información sobre cómo utilizar SSH con HDInsight, consulte [Uso de SSH con HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="run-an-apache-spark-shell"></a>Ejecución del shell de Apache Spark

Spark proporciona shells para Scala (spark-shell), Python (pyspark) y R (sparkR). En la sesión SSH, en el nodo principal del clúster de HDInsight, escriba uno de los siguientes comandos:

    ./bin/spark-shell
    ./bin/pyspark
    ./bin/sparkR

Ahora puede escribir comandos de Spark en el lenguaje adecuado.

## <a name="sparksession-and-sparkcontext-instances"></a>Instancias de objetos SparkSession y SparkContext

De forma predeterminada, cuando se ejecute el shell de Spark, se crea automáticamente las instancias de los objetos SparkSession y SparkContext.

Para acceder a la instancia de SparkSession, escriba `spark`. Para acceder a la instancia de SparkContext, escriba `sc`.

## <a name="important-shell-parameters"></a>Parámetros de shell importantes

El comando de shell de Spark (`spark-shell`, `pyspark`, o `sparkR`) admite muchos parámetros de línea de comandos. Para ver una lista completa de parámetros, inicie el shell de Spark con el modificador `--help`. Tenga en cuenta que algunos de estos parámetros solo se pueden aplicar a `spark-submit`, que es el que incluye el shell de Spark.

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
