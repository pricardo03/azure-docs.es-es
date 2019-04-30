---
title: Instalar y usar Giraph en HDInsight de Azure
description: Obtenga información sobre cómo instalar Giraph en clústeres de HDInsight mediante acciones de script. Puede usar Giraph para realizar el procesamiento en Apache Hadoop en la nube de Azure de gráficos.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/22/2019
ms.openlocfilehash: aa13d8dfc65f020f3f27183423913933cd0b9404
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61395390"
---
# <a name="install-apache-giraph-on-hdinsight-hadoop-clusters-and-use-giraph-to-process-large-scale-graphs"></a>Instalar Apache Giraph en clústeres de Hadoop de HDInsight y usar Giraph para procesar gráficos a gran escala

Aprenda a instalar Apache Giraph en un clúster de HDInsight. La característica de acción de script de HDInsight le permite personalizar el clúster mediante la ejecución de un script de Bash. Se pueden usar scripts para personalizar los clústeres durante y después de la creación del clúster.

## <a name="whatis"></a>¿Qué es Giraph?

[Apache Giraph](https://giraph.apache.org/) permite realizar un procesamiento gráfico mediante Hadoop, y se puede usar con HDInsight de Azure. Los gráficos modelan las relaciones entre los objetos. Por ejemplo, las conexiones entre los enrutadores de una red de gran tamaño, como Internet, o las relaciones entre las personas en las redes sociales. El procesamiento gráfico le permite razonar sobre las relaciones entre los objetos de un gráfico. En este sentido, le ayuda por ejemplo a:

* Identificar los posibles amigos según las relaciones actuales.

* Identificar la ruta más corta entre dos equipos de una red.

* Calcular la posición de las páginas web.

> [!WARNING]  
> Los componentes proporcionados con el clúster de HDInsight son totalmente compatibles. Además, el soporte técnico de Microsoft le ayuda a aislar y resolver problemas relacionados con estos componentes.
>
> Los componentes personalizados, como Giraph, reciben soporte técnico comercialmente razonable para ayudarlo a solucionar el problema. El soporte técnico de Microsoft podría solucionar este problema. Si no es así, debe consultar las comunidades de código abierto donde grandes expertos en esa tecnología. Por ejemplo, hay diversos sitios de la comunidad que se pueden usar, como el [foro de MSDN para HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [https://stackoverflow.com](https://stackoverflow.com). Los proyectos de Apache también tienen sitios de proyecto en [https://apache.org](https://apache.org), por ejemplo: [Hadoop](https://hadoop.apache.org/).


## <a name="what-the-script-does"></a>Funcionamiento del script

Este script realiza las acciones siguientes:

* Instala Giraph en `/usr/hdp/current/giraph`

* Copia el archivo `giraph-examples.jar` en el almacenamiento predeterminado (WASB) del clúster: `/example/jars/giraph-examples.jar`

## <a name="install"></a>Instalación de Giraph mediante acciones de script

En la siguiente ubicación se encuentra disponible un script de ejemplo para instalar Giraph en un clúster de HDInsight:

    https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh

Esta sección proporciona instrucciones sobre cómo usar el script de ejemplo al crear el clúster mediante el Portal de Azure.

> [!NOTE]  
> Se puede aplicar una acción de script mediante cualquiera de los siguientes métodos:
> * Azure PowerShell
> * La CLI de Azure
> * El SDK de .NET para HDInsight
> * Plantillas del Administrador de recursos de Azure
> 
> También puede aplicar acciones de script a clústeres que ya se estén ejecutando. Para obtener más información, consulte [Personalización de clústeres de HDInsight mediante la acción de scripts](hdinsight-hadoop-customize-cluster-linux.md).

1. Comience a crear un clúster siguiendo los pasos que se describen en [Creación de clústeres de HDInsight basados en Linux](hdinsight-hadoop-create-linux-clusters-portal.md), pero no complete la operación.

2. En la sección **Configuración opcional**, seleccione **Acciones de script** y proporcione la información siguiente:

   * **NOMBRE**: escriba un nombre descriptivo para la acción de script.

   * **URI DE SCRIPT**: https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh

   * **PRINCIPAL**: active esta entrada.

   * **TRABAJO**: deje esta entrada desactivada.

   * **ZOOKEEPER**: deje esta entrada desactivada.

   * **PARÁMETROS**: deje este campo en blanco.

3. En la parte inferior de **Acciones de scripts**, use el botón **Seleccionar** para guardar la configuración. Por último, use el botón **Seleccionar** situado en la parte inferior de la sección **Configuración opcional** para guardar la información de configuración opcional.

4. Continúe creando el clúster, tal como se describe en [Creación de clústeres de HDInsight basados en Linux](hdinsight-hadoop-create-linux-clusters-portal.md).

## <a name="usegiraph"></a>¿Cómo uso Giraph en HDInsight?

Después de que se ha creado el clúster, use estos pasos para ejecutar el ejemplo SimpleShortestPathsComputation incluido con Giraph. En este ejemplo se usa la implementación de [Pregel](https://people.apache.org/~edwardyoon/documents/pregel.pdf) básica para buscar la ruta más corta entre los objetos de un gráfico.

1. Conéctese al clúster de HDInsight con SSH:

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Para más información, consulte [Uso de SSH con HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Use el comando siguiente para crear un archivo denominado **tiny_graph.txt**:

    ```bash
    nano tiny_graph.txt
    ```

    Use el texto siguiente como contenido de este archivo:

    ```text
    [0,0,[[1,1],[3,3]]]
    [1,0,[[0,1],[2,2],[3,1]]]
    [2,0,[[1,2],[4,4]]]
    [3,0,[[0,3],[1,1],[4,4]]]
    [4,0,[[3,4],[2,4]]]
    ```

    Estos datos describen una relación entre los objetos de un gráfico dirigido, mediante el formato `[source_id, source_value,[[dest_id], [edge_value],...]]`. Cada línea representa una relación entre un objeto `source_id` y uno varios objetos `dest_id`. El valor `edge_value` se puede considerar como la fuerza o la distancia de la conexión entre `source_id` y `dest\_id`.

    Extrapolados, y usando el valor (o peso) como la distancia entre los objetos, los datos podrían parecerse al siguiente diagrama:

    ![tiny_graph.txt drawn as circles with lines of varying distance between](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph.png)

3. Para guardar el archivo, presione **Ctrl+X**, a continuación, **Y**; finalmente, **Entrar** para aceptar el nombre de archivo.

4. Use la siguiente instrucción para almacenar los datos en el almacenamiento principal del clúster de HDInsight:

    ```bash
    hdfs dfs -put tiny_graph.txt /example/data/tiny_graph.txt
    ```

5. Ejecute el ejemplo SimpleShortestPathsComputation con el siguiente comando:

    ```bash
    yarn jar /usr/hdp/current/giraph/giraph-examples.jar org.apache.giraph.GiraphRunner org.apache.giraph.examples.SimpleShortestPathsComputation -ca mapred.job.tracker=headnodehost:9010 -vif org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat -vip /example/data/tiny_graph.txt -vof org.apache.giraph.io.formats.IdWithValueTextOutputFormat -op /example/output/shortestpaths -w 2
    ```

    Los parámetros que se usan con este comando se describen en la tabla siguiente:

   | Parámetro | Qué hace |
   | --- | --- |
   | `jar` |El archivo jar que contiene los ejemplos. |
   | `org.apache.giraph.GiraphRunner` |La clase que se usa para iniciar los ejemplos. |
   | `org.apache.giraph.examples.SimpleShortestPathsCoputation` |El ejemplo que se usa. En este caso, calcula la ruta más corta entre el identificador 1 y los restantes identificadores del gráfico. |
   | `-ca mapred.job.tracker` |El nodo principal del clúster. |
   | `-vif` |El formato de entrada que se va a usar para los datos de entrada. |
   | `-vip` |El archivo de datos de entrada. |
   | `-vof` |El formato de salida. En este caso, el identificador y el valor como texto sin formato. |
   | `-op` |La ubicación de salida. |
   | `-w 2` |El número de trabajados que se usan. En este ejemplo, 2. |

    Para obtener más información sobre estos y otros parámetros que se usan en los ejemplos de Giraph, consulte la [guía de inicio rápido de Giraph](https://giraph.apache.org/quick_start.html).

6. Una vez finalizado el trabajo, los resultados se almacenan en el **/example/out/shortestpaths** directory. Los nombres de los archivos de salida empezarán por **part-m-** y terminarán en un número que indica el primer, segundo, etc., archivo. Para ver la salida, use el comando siguiente:

    ```bash
    hdfs dfs -text /example/output/shortestpaths/*
    ```

    La salida es similar al siguiente texto:

        0    1.0
        4    5.0
        2    2.0
        1    0.0
        3    1.0

    El ejemplo SimpleShortestPathComputation se ha codificado de forma rígida para comenzar por el identificador de objeto 1 y encontrar la ruta más corta a los demás objetos. La salida tiene el formato de `destination_id` y `distance`. `distance` es el valor (o peso) de los bordes recorridos entre el identificador de objeto 1 y el identificador de destino.

    Visualizando estos datos, puede verificar los resultados recorriendo las rutas más cortas entre el identificador 1 y todos los demás objetos. La ruta más corta entre el identificador 1 y el identificador 4 es 5. Este valor es la distancia total entre el <span style="color:orange">identificador 1 y 3</span>, y, a continuación, el <span style="color:red">identificador 3 y 4</span>.

    ![Drawing of objects as circles with shortest paths drawn between](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph-out.png)

## <a name="next-steps"></a>Pasos siguientes

* [Instalación y uso de Hue en clústeres de HDInsight](hdinsight-hadoop-hue-linux.md).
