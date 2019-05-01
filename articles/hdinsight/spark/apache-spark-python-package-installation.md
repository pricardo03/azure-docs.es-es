---
title: 'Acción de script: instalación de paquetes de Python con Jupyter en Azure HDInsight'
description: Instrucciones detalladas sobre cómo usar la acción de script para configurar cuadernos de Jupyter disponibles con clústeres de HDInsight Spark para usar paquetes externos de Python.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/22/2019
ms.openlocfilehash: c07326cc3a4334f1873eef2dc23da05156a93577
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64574648"
---
# <a name="use-script-action-to-install-external-python-packages-for-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight"></a>Uso de acción de script para instalar paquetes externos de Python para cuadernos de Jupyter en clústeres de Apache Spark en HDInsight
> [!div class="op_single_selector"]
> * [Uso de magic cell](apache-spark-jupyter-notebook-use-external-packages.md)
> * [Uso de acciones de script](apache-spark-python-package-installation.md)

Obtenga información sobre cómo usar acciones de Script para configurar un [Apache Spark](https://spark.apache.org/) clúster en HDInsight para usar externo, proporcionado por la Comunidad **python** paquetes que no están incluyen de fábrica en el clúster.

> [!NOTE]  
> También puede configurar un cuaderno de Jupyter mediante `%%configure` para usar paquetes externos. Para obtener instrucciones, consulte [Uso de paquetes externos con cuadernos de Jupyter en clústeres de Apache Spark en HDInsight Linux](apache-spark-jupyter-notebook-use-external-packages.md).

Puede buscar en el [índice de paquetes](https://pypi.python.org/pypi) la lista completa de paquetes que están disponibles. También puede obtener una lista de paquetes disponibles de otras fuentes. Por ejemplo, puede instalar paquetes que están disponibles a través de [conda-forge](https://conda-forge.org/feedstocks/).

En este artículo, aprenderá a instalar el paquete [TensorFlow](https://www.tensorflow.org/) mediante una acción de script en su clúster y a usarlo mediante Jupyter Notebook como un ejemplo.

## <a name="prerequisites"></a>Requisitos previos
Debe tener lo siguiente:

* Una suscripción de Azure. Consulte [Obtención de una versión de evaluación gratuita](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Un clúster de Apache Spark en HDInsight. Para obtener instrucciones, vea [Creación de clústeres Apache Spark en HDInsight de Azure](apache-spark-jupyter-spark-sql.md).

   > [!NOTE]  
   > Si aún no tiene un clúster de Spark en HDInsight Linux, puede ejecutar acciones de script durante la creación del clúster. Consulte la documentación sobre [cómo usar acciones de script personalizadas](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).
   
## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>Soporte técnico para el software de código abierto utilizado en clústeres de HDInsight

El servicio Microsoft Azure HDInsight usa un ecosistema de tecnologías de código abierto formado en torno a Apache Hadoop. Microsoft Azure proporciona un nivel general de soporte técnico para las tecnologías de código abierto. Para más información, vea la sección de **ámbito de soporte técnico** del [sitio web Preguntas más frecuentes de soporte técnico de Azure](https://azure.microsoft.com/support/faq/). Además, el servicio HDInsight ofrece un nivel adicional de soporte técnico para los componentes incorporados.

Hay dos tipos de componentes de código abierto que están disponibles en el servicio de HDInsight:

* **Componentes integrados** : estos componentes están instalados previamente en clústeres de HDInsight y proporcionan la funcionalidad básica del clúster. Por ejemplo, el administrador de recursos de Apache Hadoop YARN, el lenguaje de consulta de Apache Hive (HiveQL) y la biblioteca Mahout pertenecen a esta categoría. Hay una lista completa de componentes del clúster disponible en [Novedades en las versiones de clústeres de Apache Hadoop proporcionadas por HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning).
* **Componentes personalizados** : el usuario del clúster puede instalar o usar en la carga de trabajo cualquier componente que esté disponible en la comunidad o que haya creado personalmente.

> [!IMPORTANT]   
> Los componentes proporcionados con el clúster de HDInsight son totalmente compatibles. Soporte técnico de Microsoft ayuda a aislar y a solucionar problemas relacionados con estos componentes.
>
> Los componentes personalizados reciben soporte técnico comercialmente razonable para ayudarle a solucionar el problema. El soporte técnico de Microsoft podría resolver el problema O pedirle que aborde los canales disponibles para las tecnologías de código abierto donde se encuentra la más amplia experiencia para esa tecnología. Por ejemplo, hay diversos sitios de la comunidad que se pueden usar, como el [foro de MSDN para HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [https://stackoverflow.com](https://stackoverflow.com). Los proyectos de Apache también tienen sitios de proyecto en [https://apache.org](https://apache.org), por ejemplo: [Hadoop](https://hadoop.apache.org/).


## <a name="use-external-packages-with-jupyter-notebooks"></a>Uso de paquetes externos con cuadernos de Jupyter Notebook

1. Desde el [portal Azure](https://portal.azure.com/), vaya a su clúster.  

2. Con el clúster seleccionado, en el panel izquierdo, en **configuración**, seleccione **acciones de Script**.

3. Seleccione **+ Enviar nuevo**.

4. Escriba los siguientes valores para el **enviar acción de script** ventana:  


    |Parámetro | Valor |
    |---|---|
    |Tipo de script | Seleccione **- Personalizado** en la lista desplegable.|
    |NOMBRE |Escriba `tensorflow` en el cuadro de texto.|
    |URI de script de Bash |Escriba `https://hdiconfigactions.blob.core.windows.net/linuxtensorflow/tensorflowinstall.sh` en el cuadro de texto. |
    |Tipos de nodos | Seleccione el **Head**, y **trabajo** casillas de verificación. |

    `tensorflowinstall.sh` contiene los siguientes comandos:

    ```bash
    #!/usr/bin/env bash
    /usr/bin/anaconda/bin/conda install --yes tensorflow
    ```

5. Seleccione **Crear**.  Consulte la documentación sobre [cómo usar acciones de script personalizadas](../hdinsight-hadoop-customize-cluster-linux.md).

6. Espere a que la secuencia de comandos completar.  El **acciones de Script** panel indicará **nuevas acciones de script pueden enviarse una vez finalizada la operación de clúster** mientras se está ejecutando la secuencia de comandos.  Una barra de progreso puede verse en la UI de Ambari **operaciones en segundo plano** ventana.

7. Abrir un cuaderno de Jupyter de PySpark.  Consulte [crear un cuaderno de Jupyter notebook en Spark HDInsight](./apache-spark-jupyter-notebook-kernels.md#create-a-jupyter-notebook-on-spark-hdinsight) para conocer los pasos.

    ![Crear un nuevo cuaderno de Jupyter](./media/apache-spark-python-package-installation/hdinsight-spark-create-notebook.png "Crear un nuevo cuaderno de Jupyter")

8. Ahora realizará la acción `import tensorflow` y ejecutará un ejemplo de saludo (Hola mundo). Escriba el siguiente código:

    ```
    import tensorflow as tf
    hello = tf.constant('Hello, TensorFlow!')
    sess = tf.Session()
    print(sess.run(hello))
    ```

    El resultado se parecerá al siguiente:
    
    ![Ejecución de código de TensorFlow](./media/apache-spark-python-package-installation/execution.png "Ejecución de código de TensorFlow")

> [!NOTE]  
> Hay dos instalaciones de Python en el clúster. Spark usará la instalación de Python de Anaconda ubicada en `/usr/bin/anaconda/bin` y usará como predeterminado el entorno de Python 2.7. Para usar Python 3.x e instalar paquetes en el kernel de PySpark3, use la ruta de acceso al ejecutable `conda` para ese entorno y use el parámetro `-n` para especificar el entorno. Por ejemplo, el comando `/usr/bin/anaconda/envs/py35/bin/conda install -c conda-forge ggplot -n py35`, instala el paquete `ggplot` en el entorno de Python 3.5 mediante el canal `conda-forge`.

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
* [Uso de paquetes externos con cuadernos de Jupyter en clústeres de Apache Spark en HDInsight](apache-spark-jupyter-notebook-use-external-packages.md)
* [Uso del complemento de herramientas de HDInsight para IntelliJ IDEA para crear y enviar aplicaciones de Spark Scala](apache-spark-intellij-tool-plugin.md)
* [Use HDInsight Tools Plugin for IntelliJ IDEA to debug Apache Spark applications remotely](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md) (Uso del complemento de herramientas de HDInsight para IntelliJ IDEA para depurar aplicaciones de Apache Spark de forma remota)
* [Uso de cuadernos de Apache Zeppelin con un clúster de Apache Spark en HDInsight](apache-spark-zeppelin-notebook.md)
* [Kernels disponible para Jupyter Notebook en clústeres Apache Spark para HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Instalación de un cuaderno de Jupyter Notebook en el equipo y conexión al clúster de Apache Spark en HDInsight de Azure](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Administración de recursos
* [Administración de recursos para el clúster Apache Spark en HDInsight de Azure](apache-spark-resource-manager.md)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight (Seguimiento y depuración de trabajos que se ejecutan en un clúster de Apache Spark en HDInsight)](apache-spark-job-debugging.md)
