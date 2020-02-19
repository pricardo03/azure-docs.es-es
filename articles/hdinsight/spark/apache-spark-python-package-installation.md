---
title: Acción de script para paquetes de Python con Jupyter en Azure HDInsight
description: Instrucciones detalladas sobre cómo usar la acción de script para configurar cuadernos de Jupyter disponibles con clústeres de HDInsight Spark para usar paquetes externos de Python.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 11/19/2019
ms.openlocfilehash: 6342e6a75c8397712e028874b4d727bf3d6f5ff4
ms.sourcegitcommit: cfbea479cc065c6343e10c8b5f09424e9809092e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2020
ms.locfileid: "77087110"
---
# <a name="safely-manage-python-environment-on-azure-hdinsight-using-script-action"></a>Administración segura del entorno de Python en Azure HDInsight mediante la acción de scripts

> [!div class="op_single_selector"]
> * [Uso de magic cell](apache-spark-jupyter-notebook-use-external-packages.md)
> * [Uso de acciones de script](apache-spark-python-package-installation.md)

HDInsight tiene dos instalaciones de Python integradas en el clúster de Spark: Anaconda Python 2.7 y Python 3.5. En algunos casos, los clientes tienen que personalizar el entorno de Python, como la instalación de paquetes de Python externos u otra versión de Python. En este artículo, se muestra el procedimiento recomendado de administración segura de entornos de Python para un clúster de [Apache Spark](https://spark.apache.org/) en HDInsight.

## <a name="prerequisites"></a>Prerrequisitos

* Suscripción a Azure. Consulte [Obtención de una versión de evaluación gratuita](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* Un clúster de Apache Spark en HDInsight. Para obtener instrucciones, vea [Creación de clústeres Apache Spark en HDInsight de Azure](apache-spark-jupyter-spark-sql.md).

   > [!NOTE]  
   > Si aún no tiene un clúster de Spark en HDInsight Linux, puede ejecutar acciones de script durante la creación del clúster. Consulte la documentación sobre [cómo usar acciones de script personalizadas](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).

## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>Soporte técnico para el software de código abierto utilizado en clústeres de HDInsight

El servicio Microsoft Azure HDInsight usa un ecosistema de tecnologías de código abierto formado en torno a Apache Hadoop. Microsoft Azure proporciona un nivel general de soporte técnico para las tecnologías de código abierto. Para obtener más información, consulte el [sitio web de las preguntas más frecuentes de soporte técnico de Azure](https://azure.microsoft.com/support/faq/). Además, el servicio HDInsight ofrece un nivel adicional de soporte técnico para los componentes incorporados.

Hay dos tipos de componentes de código abierto que están disponibles en el servicio de HDInsight:

* **Componentes integrados** : estos componentes están instalados previamente en clústeres de HDInsight y proporcionan la funcionalidad básica del clúster. Por ejemplo, el administrador de recursos de Apache Hadoop YARN, el lenguaje de consulta de Apache Hive (HiveQL) y la biblioteca Mahout pertenecen a esta categoría. Hay una lista completa de componentes del clúster disponible en [Novedades en las versiones de clústeres de Apache Hadoop proporcionadas por HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning).
* **Componentes personalizados** : el usuario del clúster puede instalar o usar en la carga de trabajo cualquier componente que esté disponible en la comunidad o que haya creado personalmente.

> [!IMPORTANT]
> Los componentes proporcionados con el clúster de HDInsight son totalmente compatibles. Soporte técnico de Microsoft ayuda a aislar y a solucionar problemas relacionados con estos componentes.
>
> Los componentes personalizados reciben soporte técnico comercialmente razonable para ayudarle a solucionar el problema. El soporte técnico de Microsoft podría resolver el problema O pedirle que aborde los canales disponibles para las tecnologías de código abierto donde se encuentra la más amplia experiencia para esa tecnología. Por ejemplo, hay diversos sitios de la comunidad que se pueden usar, como el [foro de MSDN para HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [https://stackoverflow.com](https://stackoverflow.com). Los proyectos de Apache también tienen sitios de proyecto en [https://apache.org](https://apache.org), como por ejemplo: [Hadoop](https://hadoop.apache.org/).

## <a name="understand-default-python-installation"></a>Descripción de la instalación predeterminada de Python

El clúster de HDInsight Spark se crea con la instalación de Anaconda. Hay dos instalaciones de Python en el clúster: Anaconda Python 2.7 y Python 3.5 En la tabla siguiente se muestra la configuración predeterminada de Python para Spark, Livy y Jupyter.

| |Python 2.7|Python 3.5|
|----|----|----|
|Path|/usr/bin/anaconda/bin|/usr/bin/anaconda/envs/py35/bin|
|Spark|Valor predeterminado establecido en 2.7|N/D|
|Livy|Valor predeterminado establecido en 2.7|N/D|
|Jupyter|Kernel de PySpark|Kernel de PySpark3|

## <a name="safely-install-external-python-packages"></a>Instalación segura de paquetes externos de Python

El clúster de HDInsight depende del entorno integrado de Python, tanto Python 2.7 como Python 3.5. La instalación directa de paquetes personalizados en esos entornos integrados predeterminados puede producir cambios inesperados en la versión de la biblioteca e interrumpir el clúster. Para instalar de forma segura paquetes externos personalizados de Python para las aplicaciones de Spark, siga los pasos que se indican a continuación.

1. Cree un entorno virtual de Python mediante Conda. Un entorno virtual proporciona un espacio aislado para sus proyectos sin interrumpir otros. Al crear el entorno virtual de Python, puede especificar la versión de Python que quiere usar. Tenga en cuenta que aún tiene que crear el entorno virtual aunque quiera usar Python 2.7 y 3.5. Esto se hace para asegurarse de que no se interrumpa el entorno predeterminado del clúster. Ejecute las acciones de script en el clúster para todos los nodos con el siguiente script para crear un entorno virtual de Python. 

    -   `--prefix` especifica una ruta de acceso donde reside un entorno virtual de Conda. Hay varias configuraciones que deben seguirse cambiando en función de la ruta de acceso especificada aquí. En este ejemplo, usamos py35new, ya que el clúster tiene un entorno virtual existente denominado py35.
    -   `python=` especifica la versión de Python para el entorno virtual. En este ejemplo, usamos la versión 3.5, la misma versión que el clúster creado anteriormente. También puede usar otras versiones de Python para crear el entorno virtual.
    -   `anaconda` especifica el package_spec como anaconda para instalar paquetes de Anaconda en el entorno virtual.
    
    ```bash
    sudo /usr/bin/anaconda/bin/conda create --prefix /usr/bin/anaconda/envs/py35new python=3.5 anaconda --yes 
    ```

2. Instale los paquetes externos de Python en el entorno virtual creado si es necesario. Ejecute las acciones de script en el clúster para todos los nodos con el siguiente script para instalar los paquetes externos de Python. Debe tener el privilegio sudo aquí para poder escribir archivos en la carpeta del entorno virtual.

    Puede buscar en el [índice de paquetes](https://pypi.python.org/pypi) la lista completa de paquetes que están disponibles. También puede obtener una lista de paquetes disponibles de otras fuentes. Por ejemplo, puede instalar paquetes que están disponibles a través de [conda-forge](https://conda-forge.org/feedstocks/).

    -   `seaborn` es el nombre del paquete que desea instalar.
    -   `-n py35new` especifica el nombre del entorno virtual que acaba de crearse. Asegúrese de cambiar el nombre correspondiente en función de la creación del entorno virtual.

    ```bash
    sudo /usr/bin/anaconda/bin/conda install seaborn -n py35new --yes
    ```

    Si no conoce el nombre del entorno virtual, puede utilizar SSH en el nodo de encabezado del clúster y ejecutar `/usr/bin/anaconda/bin/conda info -e` para mostrar todos los entornos virtuales.

3. Cambie las configuraciones de Spark y Livy, y apunte al entorno virtual creado.

    1. Abra la UI de Ambari, vaya a la página Spark2, pestaña Configs.
    
        ![Cambiar la configuración de Spark y Livy a través de Ambari](./media/apache-spark-python-package-installation/ambari-spark-and-livy-config.png)
 
    2. Expanda Advanced livy2-env y agregue las siguientes instrucciones al final. Si instaló el entorno virtual con otro prefijo, cambie la ruta de acceso correspondiente.

        ```
        export PYSPARK_PYTHON=/usr/bin/anaconda/envs/py35new/bin/python
        export PYSPARK_DRIVER_PYTHON=/usr/bin/anaconda/envs/py35new/bin/python
        ```

        ![Cambiar la configuración de Livy a través de Ambari](./media/apache-spark-python-package-installation/ambari-livy-config.png)

    3. Expanda Advanced spark2-env y reemplace la instrucción de exportación PYSPARK_PYTHON existente al final. Si instaló el entorno virtual con otro prefijo, cambie la ruta de acceso correspondiente.

        ```
        export PYSPARK_PYTHON=${PYSPARK_PYTHON:-/usr/bin/anaconda/envs/py35new/bin/python}
        ```

        ![Cambiar la configuración de Spark a través de Ambari](./media/apache-spark-python-package-installation/ambari-spark-config.png)

    4. Guarde los cambios y reinicie los servicios afectados. Estos cambios necesitan el reinicio del servicio Spark2. La UI de Ambari le mostrará un aviso de reinicio obligatorio, haga clic en Restart para reiniciar todos los servicios afectados.

        ![Cambiar la configuración de Spark a través de Ambari](./media/apache-spark-python-package-installation/ambari-restart-services.png)
 
4.  Si desea usar el nuevo entorno virtual creado en Jupyter, debe cambiar las configuraciones de Jupyter y reiniciar Jupyter. Ejecute las acciones de script en todos los nodos de encabezado con la siguiente instrucción para apuntar Jupyter al nuevo entorno virtual creado. Asegúrese de modificar la ruta de acceso al prefijo que especificó para el entorno virtual. Después de ejecutar esta acción de script, reinicie el servicio de Jupyter a través de la UI de Ambari para que este cambio esté disponible.

    ```
    sudo sed -i '/python3_executable_path/c\ \"python3_executable_path\" : \"/usr/bin/anaconda/envs/py35new/bin/python3\"' /home/spark/.sparkmagic/config.json
    ```

    Puede volver a confirmar el entorno de Python en Jupyter Notebook ejecutando el código siguiente:

    ![Comprobar la versión de Python en Jupyter Notebook](./media/apache-spark-python-package-installation/check-python-version-in-jupyter.png)

## <a name="known-issue"></a>Problema conocido

Hay un error conocido para Anaconda versión 4.7.11, 4.7.12 y 4.8.0. Si ve que las acciones de script se bloquean en `"Collecting package metadata (repodata.json): ...working..."` y muestran el error `"Python script has been killed due to timeout after waiting 3600 secs"`. Puede descargar [este script](https://gregorysfixes.blob.core.windows.net/public/fix-conda.sh) y ejecutarlo como acciones de script en todos los nodos para corregir el problema.

Para comprobar la versión de Anaconda, puede aplicar SSH en el nodo de encabezado del clúster y ejecutar `/usr/bin/anaconda/bin/conda --v`.

## <a name="seealso"></a>Otras referencias

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

### <a name="manage-resources"></a>Administrar recursos

* [Administración de recursos para el clúster Apache Spark en HDInsight de Azure](apache-spark-resource-manager.md)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight (Seguimiento y depuración de trabajos que se ejecutan en un clúster de Apache Spark en HDInsight)](apache-spark-job-debugging.md)
