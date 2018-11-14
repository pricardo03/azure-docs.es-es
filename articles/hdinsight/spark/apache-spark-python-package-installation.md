---
title: 'Acción de script: instalación de paquetes de Python con Jupyter en Azure HDInsight'
description: Instrucciones detalladas sobre cómo usar la acción de script para configurar cuadernos de Jupyter disponibles con clústeres de HDInsight Spark para usar paquetes externos de Python.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: hrasheed
ms.openlocfilehash: af25dcff2302827f2291d50972f09b8b5fda6cd3
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2018
ms.locfileid: "51255445"
---
# <a name="use-script-action-to-install-external-python-packages-for-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight"></a>Uso de acción de script para instalar paquetes externos de Python para cuadernos de Jupyter en clústeres de Apache Spark en HDInsight
> [!div class="op_single_selector"]
> * [Uso de magic cell](apache-spark-jupyter-notebook-use-external-packages.md)
> * [Uso de acciones de script](apache-spark-python-package-installation.md)
>
>

Aprenda a usar acciones de script para configurar un clúster de Apache Spark en HDInsight (Linux) para el uso de paquetes de **Python** externos aportados por la comunidad que no están incluidos de serie en el clúster.

> [!NOTE]
> También puede configurar un cuaderno de Jupyter mediante `%%configure` para usar paquetes externos. Para obtener instrucciones, consulte [Uso de paquetes externos con cuadernos de Jupyter en clústeres de Apache Spark en HDInsight Linux](apache-spark-jupyter-notebook-use-external-packages.md).
> 
> 

Puede buscar en el [índice de paquetes](https://pypi.python.org/pypi) la lista completa de paquetes que están disponibles. También puede obtener una lista de paquetes disponibles de otras fuentes. Por ejemplo, puede instalar paquetes que están disponibles a través de [Anaconda](https://docs.continuum.io/anaconda/pkg-docs) o [conda-forge](https://conda-forge.org/feedstocks/).

En este artículo, aprenderá a instalar el paquete [TensorFlow](https://www.tensorflow.org/) mediante una acción de script en su clúster y a usarlo mediante Jupyter Notebook como un ejemplo.

## <a name="prerequisites"></a>Requisitos previos
Debe tener lo siguiente:

* Una suscripción de Azure. Consulte [Obtención de una versión de evaluación gratuita](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Un clúster de Apache Spark en HDInsight. Para obtener instrucciones, vea [Creación de clústeres Apache Spark en HDInsight de Azure](apache-spark-jupyter-spark-sql.md).

   > [!NOTE]
   > Si aún no tiene un clúster de Spark en HDInsight Linux, puede ejecutar acciones de script durante la creación del clúster. Consulte la documentación sobre [cómo usar acciones de script personalizadas](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).
   > 
   > 
   
   ## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>Soporte técnico para el software de código abierto utilizado en clústeres de HDInsight

El servicio Microsoft Azure HDInsight usa un ecosistema de tecnologías de código abierto formado en torno a Hadoop. Microsoft Azure proporciona un nivel general de soporte técnico para las tecnologías de código abierto. Para más información, vea la sección de **ámbito de soporte técnico** del [sitio web Preguntas más frecuentes de soporte técnico de Azure](https://azure.microsoft.com/support/faq/). Además, el servicio HDInsight ofrece un nivel adicional de soporte técnico para los componentes incorporados.

Hay dos tipos de componentes de código abierto que están disponibles en el servicio de HDInsight:

* **Componentes integrados** : estos componentes están instalados previamente en clústeres de HDInsight y proporcionan la funcionalidad básica del clúster. Por ejemplo, el administrador de recursos de YARN, el lenguaje de consulta Hive (HiveQL) y la biblioteca Mahout pertenecen a esta categoría. Hay una lista completa de componentes del clúster disponible en [Novedades en las versiones de clústeres de Hadoop proporcionadas por HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-component-versioning).
* **Componentes personalizados** : el usuario del clúster puede instalar o usar en la carga de trabajo cualquier componente que esté disponible en la comunidad o que haya creado personalmente.

> [!WARNING]
> Los componentes proporcionados con el clúster de HDInsight son totalmente compatibles. Soporte técnico de Microsoft ayuda a aislar y a solucionar problemas relacionados con estos componentes.
>
> Los componentes personalizados reciben soporte técnico comercialmente razonable para ayudarle a solucionar el problema. El soporte técnico de Microsoft podría resolver el problema O pedirle que aborde los canales disponibles para las tecnologías de código abierto donde se encuentra la más amplia experiencia para esa tecnología. Por ejemplo, hay diversos sitios de la comunidad que se pueden usar, como el [foro de MSDN para HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Los proyectos de Apache también tienen sitios de proyecto en [http://apache.org](http://apache.org), por ejemplo: [Hadoop](http://hadoop.apache.org/).


## <a name="use-external-packages-with-jupyter-notebooks"></a>Uso de paquetes externos con cuadernos de Jupyter Notebook

1. Desde [Azure Portal](https://portal.azure.com/), en el panel de inicio, haga clic en el icono del clúster Spark (si lo ancló al panel de inicio). También puede navegar hasta el clúster en **Examinar todo** > **Clústeres de HDInsight**.   

2. En la hoja del clúster de Spark, haga clic en **Acciones de script** en el panel izquierdo. Use el tipo de script "Personalizado" y escriba un nombre descriptivo para la acción de script. Ejecute el script en los **nodos principal y de trabajo** y deje en blanco el campo de parámetros. Al script de Bash se puede hacer referencia desde: https://hdiconfigactions.blob.core.windows.net/linuxtensorflow/tensorflowinstall.sh. Consulte la documentación acerca de [cómo usar acciones de script personalizadas](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).

   > [!NOTE]
   > Hay dos instalaciones de Python en el clúster. Spark usará la instalación de Python de Anaconda ubicada en `/usr/bin/anaconda/bin`. Haga referencia a esa instalación en sus acciones personalizadas mediante `/usr/bin/anaconda/bin/pip` y `/usr/bin/anaconda/bin/conda`.
   > 
   > 

3. Abrir un cuaderno de Jupyter de PySpark

    ![Crear un nuevo cuaderno de Jupyter](./media/apache-spark-python-package-installation/hdinsight-spark-create-notebook.png "Crear un nuevo cuaderno de Jupyter")

4. Se crea y se abre un nuevo cuaderno con el nombre Untitled.pynb. Haga clic en el nombre del cuaderno en la parte superior y escriba un nombre descriptivo.

    ![Proporcionar un nombre para el cuaderno](./media/apache-spark-python-package-installation/hdinsight-spark-name-notebook.png "Proporcionar un nombre para el cuaderno")

5. Ahora realizará la acción `import tensorflow` y ejecutará un ejemplo de saludo (Hola mundo). 

    Código para copiar:

        import tensorflow as tf
        hello = tf.constant('Hello, TensorFlow!')
        sess = tf.Session()
        print(sess.run(hello))

    El resultado se parecerá al siguiente:
    
    ![Ejecución de código de TensorFlow](./media/apache-spark-python-package-installation/execution.png "Ejecución de código de TensorFlow")

## <a name="seealso"></a>Consulte también
* [Introducción a Apache Spark en HDInsight de Azure](apache-spark-overview.md)

### <a name="scenarios"></a>Escenarios
* [Spark with BI: Realizar el análisis de datos interactivos con Spark en HDInsight con las herramientas de BI](apache-spark-use-bi-tools.md)
* [Creación de aplicaciones de Machine Learning con Apache Spark en HDInsight de Azure](apache-spark-ipython-notebook-machine-learning.md)
* [Spark con Machine Learning: uso de Spark en HDInsight para predecir los resultados de la inspección de alimentos](apache-spark-machine-learning-mllib-ipython.md)
* [Website log analysis using Spark in HDInsight](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Creación y ejecución de aplicaciones
* [Crear una aplicación independiente con Scala](apache-spark-create-standalone-application.md)
* [Ejecutar trabajos de forma remota en un clúster de Spark mediante Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Herramientas y extensiones
* [Uso de paquetes externos con cuadernos de Jupyter en clústeres de Apache Spark en HDInsight](apache-spark-jupyter-notebook-use-external-packages.md)
* [Uso del complemento de herramientas de HDInsight para IntelliJ IDEA para crear y enviar aplicaciones de Spark Scala](apache-spark-intellij-tool-plugin.md)
* [Use HDInsight Tools Plugin for IntelliJ IDEA to debug Spark applications remotely (Uso del complemento de herramientas de HDInsight para IntelliJ IDEA para depurar aplicaciones de Spark de forma remota)](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Uso de cuadernos de Zeppelin con un clúster Spark en HDInsight](apache-spark-zeppelin-notebook.md)
* [Kernels disponibles para el cuaderno de Jupyter en el clúster Spark para HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Instalación de un cuaderno de Jupyter Notebook en el equipo y conexión al clúster de Apache Spark en HDInsight de Azure](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Administración de recursos
* [Administración de recursos para el clúster Apache Spark en HDInsight de Azure](apache-spark-resource-manager.md)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight (Seguimiento y depuración de trabajos que se ejecutan en un clúster de Apache Spark en HDInsight)](apache-spark-job-debugging.md)
