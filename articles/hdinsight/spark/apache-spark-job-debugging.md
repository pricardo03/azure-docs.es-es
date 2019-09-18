---
title: Depuración de trabajos de Apache Spark que se ejecutan en Azure HDInsight
description: Usar la interfaz de usuario de YARN, la de Spark y el servidor de historial de Spark para realizar un seguimiento y depurar trabajos que se ejecutan en un clúster de Spark en HDInsight de Azure
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 12/05/2018
ms.author: hrasheed
ms.openlocfilehash: d6a8ac97aa3896eaf98651f5f1120fcc6bf25516
ms.sourcegitcommit: fa4852cca8644b14ce935674861363613cf4bfdf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/09/2019
ms.locfileid: "70814118"
---
# <a name="debug-apache-spark-jobs-running-on-azure-hdinsight"></a>Depuración de trabajos de Apache Spark que se ejecutan en Azure HDInsight

En este artículo se aprende a realizar un seguimiento y a depurar los trabajos de [Apache Spark](https://spark.apache.org/) que se ejecutan en clústeres de HDInsight mediante la interfaz de usuario de [Apache Hadoop YARN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html), la de Spark y el servidor de historial de Spark. Se inicia un trabajo de Spark mediante un cuaderno disponible en el clúster de Spark, **Machine Learning: Predictive analysis on food inspection data using MLLib** (Aprendizaje automático: análisis predictivo en datos de inspección de alimentos con MLLib). Puede utilizar los pasos siguientes para realizar el seguimiento de una aplicación que haya enviado mediante cualquier otro enfoque, por ejemplo, **spark-submit**.

## <a name="prerequisites"></a>Requisitos previos
Debe tener lo siguiente:

* Una suscripción de Azure. Consulte [Obtención de una versión de evaluación gratuita](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Un clúster de Apache Spark en HDInsight. Para obtener instrucciones, vea [Creación de clústeres Apache Spark en HDInsight de Azure](apache-spark-jupyter-spark-sql.md).
* Debería haber iniciado la ejecución del cuaderno, **[Machine learning: Predictive analysis on food inspection data using MLLib](apache-spark-machine-learning-mllib-ipython.md)** (Aprendizaje automático: análisis predictivo en datos de inspección de alimentos con MLLib). Para obtener instrucciones sobre cómo ejecutar este cuaderno, siga el vínculo.  

## <a name="track-an-application-in-the-yarn-ui"></a>Seguimiento de una aplicación en la interfaz de usuario de YARN
1. Inicie la interfaz de usuario de YARN. Haga clic en **Yarn** en **Paneles de clúster**.
   
    ![Iniciar interfaz de usuario de YARN](./media/apache-spark-job-debugging/launch-yarn-ui.png)
   
   > [!TIP]  
   > También puede iniciar la interfaz de usuario de YARN desde la de Ambari. Para iniciar la UI de Ambari, haga clic en **Inicio de Ambari** en **Paneles de clúster**. En la interfaz de usuario de Ambari, haga clic en **YARN**, en **Vínculos rápidos**, en la instancia activa de Resource Manager y luego en la **interfaz de usuario de Resource Manager**. 

2. Dado que se inició el trabajo de Spark mediante cuadernos de Jupyter Notebook, la aplicación se llama **remotesparkmagics** (este es el nombre de todas las aplicaciones que se inician desde los cuadernos). Haga clic en el identificador de la aplicación en lugar de en el nombre de la aplicación para más información sobre el trabajo. Esto inicia la vista de la aplicación.
   
    ![Buscar identificador de aplicación Spark](./media/apache-spark-job-debugging/find-application-id.png)
   
    Para estas aplicaciones que se inician desde los cuadernos de Jupyter Notebook, el estado es siempre **RUNNING** (EN EJECUCIÓN) hasta que sale del cuaderno.
3. En la vista de la aplicación, puede explorar más en profundidad para averiguar los contenedores asociados con la aplicación y los registros (stdout y stderr). También puede iniciar la interfaz de usuario de Spark haciendo clic en el vínculo correspondiente a la **Dirección URL de seguimiento**, tal como se muestra a continuación. 
   
    ![Descargar registros de contenedor](./media/apache-spark-job-debugging/download-container-logs.png)

## <a name="track-an-application-in-the-spark-ui"></a>Seguimiento de una aplicación en la interfaz de usuario de Spark
En la interfaz de usuario de Spark, puede explorar en profundidad los trabajos de Spark generados por la aplicación que ha iniciado anteriormente.

1. Para iniciar la interfaz de usuario de Spark, desde la vista de la aplicación, haga clic en el vínculo en la **Dirección URL de seguimiento**, como se muestra en la captura de pantalla anterior. Puede ver todos los trabajos de Spark iniciados por la aplicación que se ejecuta en el cuaderno de Jupyter Notebook.
   
    ![Ver trabajos de Spark](./media/apache-spark-job-debugging/view-spark-jobs.png)
2. Haga clic en la pestaña **Executors** (Ejecutores) para ver la información de procesamiento y almacenamiento de cada elemento de ejecución. También puede recuperar la pila de llamadas haciendo clic en el vínculo **Thread Dump** (Volcado de subprocesos).
   
    ![Ver ejecutores de Spark](./media/apache-spark-job-debugging/view-spark-executors.png)
3. Haga clic en la pestaña **Stages** (Fases) para ver las fases asociadas a la aplicación.
   
    ![Visualización de las fases de Spark](./media/apache-spark-job-debugging/view-spark-stages.png "Visualización de las fases de Spark")
   
    Cada fase puede tener varias tareas de las que puede ver las estadísticas de ejecución, como se muestra a continuación.
   
    ![Visualización de los detalles de las fases de Spark](./media/apache-spark-job-debugging/view-spark-stages-details.png "Visualización de los detalles de las fases de Spark") 
4. En la página de detalles de la fase, puede iniciar la visualización de DAG. Expanda el vínculo **DAG Visualization** (Visualización de DAG) en la parte superior de la página, como se muestra a continuación.
   
    ![Ver visualización DAG de las fases de Spark](./media/apache-spark-job-debugging/view-spark-stages-dag-visualization.png)
   
    El DAG o grafo acíclico dirigido representa las distintas fases de la aplicación. Cada cuadro azul en el grafo representa una operación de Spark que se invoca desde la aplicación.
5. En la página de detalles de la fase, también puede iniciar la vista de escala de tiempo de la aplicación. Expanda el vínculo **Event Timeline** (Escala de tiempo del evento) en la parte superior de la página, como se muestra a continuación.
   
    ![Ver escala de tiempo de eventos de fases de Spark](./media/apache-spark-job-debugging/view-spark-stages-event-timeline.png)
   
    Esto permite mostrar los eventos de Spark en forma de una escala de tiempo. La vista de la escala de tiempo está disponible en tres niveles: entre trabajos, dentro de un trabajo y dentro de una fase. La imagen anterior captura la vista de la escala de tiempo para una fase determinada.
   
   > [!TIP]  
   > Si selecciona la casilla **Enable zooming** (Habilitar zoom), puede desplazarse a izquierda y derecha en la vista de escala de tiempo.

6. Otras pestañas de la interfaz de usuario de Spark proporcionan también información útil acerca de la instancia de Spark.
   
   * Pestaña Storage (Almacenamiento): si la aplicación crea varios RDD, podrá encontrar información sobre estos en esta pestaña.
   * Pestaña Environment (Entorno): esta pestaña proporciona mucha información útil sobre la instancia de Spark como, por ejemplo: 
     * La versión de la escala
     * El directorio de registro de eventos asociado con el clúster
     * El número de núcleos del ejecutor de la aplicación
     * Etc.

## <a name="find-information-about-completed-jobs-using-the-spark-history-server"></a>Búsqueda de información acerca de los trabajos completados con el servidor de historial de Spark
Cuando se completa un trabajo, se conserva la información sobre este en el servidor de historial de Spark.

1. Para iniciar el servidor de historial de Spark, en la hoja de información general, haga clic en **Servidor de historial de Spark** en **Paneles de clúster**.
   
    ![Inicio del servidor de historial de Spark1](./media/apache-spark-job-debugging/launch-spark-history-server.png "Inicio del servidor de historial de Spark1")
   
   > [!TIP]  
   > También puede iniciar la interfaz de usuario del servidor de historial de Spark desde la de Ambari. Para iniciar la UI de Ambari, en la hoja de información general, haga clic en **Inicio de Ambari** en **Paneles de clúster**. En la interfaz de usuario de Ambari, haga clic en **Spark**, en **Vínculos rápidos** y en **Spark History Server UI** (IU del servidor de historial de Spark).

2. Aparecerá una lista de todas las aplicaciones completadas. Haga clic en un identificador de aplicación para explorar en profundidad una aplicación para obtener más información.
   
    ![Inicio del servidor de historial de Spark2](./media/apache-spark-job-debugging/view-completed-applications.png "Inicio del servidor de historial de Spark2")

## <a name="see-also"></a>Otras referencias
*  [Administración de recursos para el clúster Apache Spark en HDInsight de Azure](apache-spark-resource-manager.md)
*  [Depuración de trabajos de Apache Spark mediante el servidor de historial de Spark extendido](apache-azure-spark-history-server.md)

### <a name="for-data-analysts"></a>Para analistas de datos

* [Apache Spark con Machine Learning: uso de Apache Spark en HDInsight para analizar la temperatura de edificios con los datos del sistema de acondicionamiento de aire](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark con Machine Learning: uso de Spark en HDInsight para predecir los resultados de la inspección de alimentos](apache-spark-machine-learning-mllib-ipython.md)
* [Análisis de registros de un sitio web mediante Apache Spark en HDInsight](apache-spark-custom-library-website-log-analysis.md)
* [Análisis de datos de telemetría de Application Insights con Apache Spark en HDInsight](apache-spark-analyze-application-insight-logs.md)
* [Uso de Caffe en Azure HDInsight Spark para el aprendizaje profundo distribuido](apache-spark-deep-learning-caffe.md)

### <a name="for-spark-developers"></a>Para desarrolladores de Spark

* [Crear una aplicación independiente con Scala](apache-spark-create-standalone-application.md)
* [Ejecución de trabajos de forma remota en un clúster de Apache Spark mediante Apache Livy](apache-spark-livy-rest-interface.md)
* [Uso del complemento de herramientas de HDInsight para IntelliJ IDEA para crear y enviar aplicaciones de Spark Scala](apache-spark-intellij-tool-plugin.md)
* [Use HDInsight Tools Plugin for IntelliJ IDEA to debug Apache Spark applications remotely](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md) (Uso del complemento de herramientas de HDInsight para IntelliJ IDEA para depurar aplicaciones de Apache Spark de forma remota)
* [Uso de cuadernos de Apache Zeppelin con un clúster de Apache Spark en HDInsight](apache-spark-zeppelin-notebook.md)
* [Kernels disponible para Jupyter Notebook en clústeres Apache Spark para HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Uso de paquetes externos con cuadernos de Jupyter Notebook](apache-spark-jupyter-notebook-use-external-packages.md)
* [Instalación de un cuaderno de Jupyter Notebook en el equipo y conexión al clúster de Apache Spark en HDInsight de Azure](apache-spark-jupyter-notebook-install-locally.md)
