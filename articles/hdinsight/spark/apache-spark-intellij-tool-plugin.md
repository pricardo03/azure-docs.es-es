---
title: 'Azure Toolkit for IntelliJ: Creación de aplicaciones Spark para un clúster de HDInsight '
description: Uso del kit de herramientas de Azure para IntelliJ con el fin de desarrollar aplicaciones Spark escritas en Scala y enviarlas a un clúster de HDInsight Spark.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/15/2019
ms.author: maxluk
ms.openlocfilehash: 682b2266442f3aba6696385862844e1b4b93aa3d
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62124261"
---
# <a name="use-azure-toolkit-for-intellij-to-create-apache-spark-applications-for-an-hdinsight-cluster"></a>Uso de Azure Toolkit for IntelliJ con el fin de crear aplicaciones Apache Spark para un clúster de HDInsight

Use el complemento de Azure Toolkit for IntelliJ con el fin de desarrollar aplicaciones [Apache Spark](https://spark.apache.org/) escritas en [Scala](https://www.scala-lang.org/) y enviarlas a continuación a un clúster de HDInsight Spark directamente desde el entorno de desarrollo integrado (IDE) de IntelliJ. Puede usar el complemento de varias maneras:

* Desarrollar y enviar una aplicación Spark en Scala en un clúster de Spark en HDInsight.
* Tener acceso a los recursos del clúster de Azure HDInsight Spark.
* Desarrollar y ejecutar localmente una aplicación Spark en Scala.

## <a name="prerequisites"></a>Requisitos previos

* Un clúster de Apache Spark en HDInsight. Para obtener instrucciones, vea [Creación de clústeres Apache Spark en HDInsight de Azure](apache-spark-jupyter-spark-sql.md).
* [Kit de desarrollo de Oracle Java](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).  En este tutorial se usa la versión 8.0.202 de Java.
* IntelliJ IDEA. En este artículo se usa [IntelliJ IDEA Community  2018.3.4](https://www.jetbrains.com/idea/download/).
* Azure Toolkit for IntelliJ.  Consulte [Instalación de Azure Toolkit for IntelliJ](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij-installation?view=azure-java-stable).
* WINUTILS.EXE.  Consulte [Problems running Hadoop on Windows](https://wiki.apache.org/hadoop/WindowsProblems) (Problemas al ejecutar Hadoop en Windows).

## <a name="install-scala-plugin-for-intellij-idea"></a>Instale el complemento de Scala para IntelliJ IDEA.
Para instalar el complemento Scala, siga estos pasos:

1. Abra IntelliJ IDEA.

2. En la pantalla de bienvenida, vaya a **Configure** > **Plugins** (Configurar > Complementos) para abrir la ventana **Plugins** (Complementos).
   
    ![Habilitar complemento de Scala](./media/apache-spark-intellij-tool-plugin/enable-scala-plugin.png)

3. Seleccione **Install** (Instalar) para el complemento de Scala que se presenta en la nueva ventana.  

    ![Instalar complemento de Scala](./media/apache-spark-intellij-tool-plugin/install-scala-plugin.png)

4. Después de que el complemento se instale correctamente, debe reiniciar el IDE.


## <a name="create-a-spark-scala-application-for-an-hdinsight-spark-cluster"></a>Creación de una aplicación Spark en Scala para un clúster de HDInsight Spark

1. Inicie IntelliJ IDEA y seleccione **Create New Project** (Crear proyecto) para abrir la ventana **New Project** (Nuevo proyecto).

2. Seleccione **Azure Spark/HDInsight** en el panel izquierdo.

3. Seleccione **Spark Project (Scala)** (Proyecto de Spark [Scala]) en la ventana principal.

4. En la lista desplegable **Build tool** (Herramienta de compilación), seleccione una de las siguientes:
   * **Maven**: para agregar compatibilidad con el asistente para la creación de proyectos de Scala.
   * **SBT** para administrar las dependencias y compilar el proyecto de Scala.

     ![Cuadro de diálogo Nuevo proyecto](./media/apache-spark-intellij-tool-plugin/create-hdi-scala-app.png)

5. Seleccione **Next** (Siguiente).

6. En la ventana **New Project** (Nuevo proyecto), proporcione la siguiente información:  

    |  Propiedad   | DESCRIPCIÓN   |  
    | ----- | ----- |  
    |Nombre de proyecto| Escriba un nombre.  En este tutorial se usa `myApp`.|  
    |Project&nbsp;location (Ubicación del proyecto)| Escriba la ubicación deseada para guardar el proyecto.|
    |Project SDK (SDK del proyecto)| Podría quedarse en blanco la primera vez que se usa IDEA.  Seleccione **New...** (Nuevo...) y vaya a su JDK.|
    |Versión de Spark|El asistente de creación integra la versión adecuada de los SDK de Spark y Scala. Si la versión del clúster de Spark es inferior a la 2.0, seleccione **Spark 1.x**. De lo contrario, seleccione **Spark 2.x**. En este ejemplo se usa **Spark 2.3.0 (Scala 2.11.8)**.|

    ![Selección del SDK de Spark](./media/apache-spark-intellij-tool-plugin/hdi-new-project.png)

7. Seleccione **Finalizar**.  El proyecto puede tardar unos minutos en estar disponible.

8. El proyecto Spark creará automáticamente un artefacto. Para ver el artefacto, haga lo siguiente:

    a. En la barra de menús, vaya a **Archivo** > **Estructura del proyecto...**.

   b. En la ventana **Estructura del proyecto**, seleccione **Artefactos**.  

   c. Seleccione **Cancelar** después de ver el artefacto.

      ![Información de artefacto en el cuadro de diálogo](./media/apache-spark-intellij-tool-plugin/default-artifact.png)

9. Agregue el código fuente de aplicación de la siguiente forma:

     a. En Proyecto, vaya a **myApp** > **src** > **main** > **scala**.  

    b. Haga clic con el botón derecho en **scala**, y, a continuación, vaya a **Nuevo** > **Clase Scala**.

   ![Comandos para crear una clase Scala desde Proyecto](./media/apache-spark-intellij-tool-plugin/hdi-spark-scala-code.png)

   c. En el cuadro de diálogo **Crear nueva clase Scala**, proporcione un nombre, seleccione **Objeto** en la lista desplegable **Variante** y seleccione **Aceptar**.

     ![Creación de un cuadro de diálogo de nueva clase de Scala](./media/apache-spark-intellij-tool-plugin/hdi-spark-scala-code-object.png)

   d. A continuación, el archivo **myApp.scala** se abre en la vista principal. Reemplace el código predeterminado por el código encontrado a continuación:  

        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
    
        object myApp{
            def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("myApp")
            val sc = new SparkContext(conf)
    
            val rdd = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
    
            //find the rows that have only one digit in the seventh column in the CSV file
            val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)
    
            rdd1.saveAsTextFile("wasbs:///HVACOut")
            }
    
        }

    El código lee los datos de HVAC.csv (disponible en todos los clústeres de HDInsight Spark), recupera las filas que solo tienen un dígito en la séptima columna del archivo CSV y escribe la salida en `/HVACOut` en el contenedor de almacenamiento predeterminado para el clúster.

## <a name="connect-to-your-hdinsight-cluster"></a>Conéctese a su clúster de HDInsight
El usuario puede [iniciar sesión en la suscripción a Azure](#sign-in-to-your-azure-subscription) o [vincular un clúster de HDInsight](#link-a-cluster) con el nombre de usuario y la contraseña de Ambari o credenciales unidas a un dominio para conectarse a su clúster de HDInsight.

### <a name="sign-in-to-your-azure-subscription"></a>Inicie sesión en la suscripción de Azure

1. En la barra de menús, vaya a **Ver** > **Ventanas de herramientas** > **Azure Explorer**.
       
   ![Vínculo de Azure Explorer](./media/apache-spark-intellij-tool-plugin/show-azure-explorer.png)

2. En Azure Explorer, haga clic con el botón derecho en el nodo **Azure** y después seleccione **Iniciar sesión**.

3. En el cuadro de diálogo **Inicio de sesión en Microsoft Azure**, seleccione **Iniciar sesión** y, a continuación, escriba las credenciales de Azure.

    ![Cuadro de diálogo de inicio de sesión en Azure](./media/apache-spark-intellij-tool-plugin/view-explorer-2.png)

4. Cuando haya iniciado sesión, en el cuadro de diálogo **Select Subscriptions** (Seleccionar suscripciones) se enumeran todas las suscripciones de Azure asociadas a las credenciales. Seleccione su suscripción y luego seleccione el botón **Seleccionar**.

    ![Cuadro de diálogo Seleccionar suscripciones](./media/apache-spark-intellij-tool-plugin/Select-Subscriptions.png)

5. En **Azure Explorer**, expanda **HDInsight** para ver los clústeres de HDInsight Spark de su suscripción.

    ![Clústeres de HDInsight Spark en Azure Explorer](./media/apache-spark-intellij-tool-plugin/view-explorer-3.png)

6. Para ver los recursos (por ejemplo, las cuentas de almacenamiento) asociados al clúster, puede expandir un nodo de nombre de clúster.

    ![Nodo de nombre de clúster expandido](./media/apache-spark-intellij-tool-plugin/view-explorer-4.png)

### <a name="link-a-cluster"></a>Vinculación de un clúster
Puede vincular un clúster de HDInsight mediante el nombre de usuario administrado de Apache Ambari. De forma similar, para un clúster de HDInsight unido a un dominio, puede vincular con el dominio y el nombre de usuario, como user1@contoso.com. También puede vincular el clúster del servicio de Livy.

1. En la barra de menús, vaya a **Ver** > **Ventanas de herramientas** > **Azure Explorer**.

2. En Azure Explorer, haga clic con el botón derecho en el nodo **HDInsight** y, a continuación, seleccione **Vincular un clúster**.

   ![menú contextual de vinculación de un clúster](./media/apache-spark-intellij-tool-plugin/link-a-cluster-context-menu.png)

3. Las opciones disponibles en la ventana **Vincular un clúster** variarán dependiendo del valor que seleccione en la lista desplegable **Tipo de recurso de vínculo**.  Escriba estos valores y, a continuación, seleccione **Aceptar**.

    * **Clúster de HDInsight**  
  
        |Propiedad |Valor |
        |----|----|
        |Tipo de recurso de vínculo|Seleccione **Clúster de HDInsight** en la lista desplegable.|
        |Nombre o dirección URL del clúster| Escriba el nombre del clúster.|
        |Tipo de autenticación| Dejar como **Autenticación básica**|
        |User Name| Escriba el nombre de usuario del clúster, el valor predeterminado es admin.|
        |Contraseña| Escriba la contraseña del nombre de usuario.|
    
        ![cuadro de diálogo vincular clúster de hdinsight](./media/apache-spark-intellij-tool-plugin/link-hdinsight-cluster-dialog.png)

    * **Livy Service**  
  
        |Propiedad |Valor |
        |----|----|
        |Tipo de recurso de vínculo|Seleccione **Livy Service** en la lista desplegable.|
        |Punto de conexión de Livy| Escribir punto de conexión de Livy|
        |Cluster Name| Escriba el nombre del clúster.|
        |Punto de conexión de Yarn|Opcional.|
        |Tipo de autenticación| Dejar como **Autenticación básica**|
        |User Name| Escriba el nombre de usuario del clúster, el valor predeterminado es admin.|
        |Contraseña| Escriba la contraseña del nombre de usuario.|

        ![cuadro de diálogo de vinculación de clúster de livy](./media/apache-spark-intellij-tool-plugin/link-livy-cluster-dialog.png)

4. Puede ver su clúster vinculado en el nodo **HDInsight**.

   ![clúster vinculado](./media/apache-spark-intellij-tool-plugin/linked-cluster.png)

5. También puede desvincular un clúster de **Azure Explorer**.

   ![clúster no vinculado](./media/apache-spark-intellij-tool-plugin/unlink.png)

## <a name="run-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a>Ejecución de una aplicación Spark en Scala en un clúster de HDInsight Spark
Después de crear una aplicación de Scala, puede enviarla al clúster.

1. En Proyecto, vaya a **myApp** > **src** > **main** > **scala** > **myApp**.  Haga clic con el botón derecho en **myApp** y seleccione **Enviar aplicación Spark** (probablemente estará en la parte inferior de la lista).
    
      ![Envío de aplicación Spark a comando HDInsight](./media/apache-spark-intellij-tool-plugin/hdi-submit-spark-app-1.png)

2. En el cuadro de diálogo **Enviar aplicación Spark**, seleccione **1. Spark en HDInsight**.

3. En la ventana **Editar configuración**, proporcione los valores siguientes y, a continuación, seleccione **Aceptar**:

    |Propiedad |Valor |
    |----|----|
    |Clústeres de Spark (solo en Linux)|Seleccione el clúster de HDInsight Spark en el que quiere ejecutar la aplicación.|
    |Seleccione un artefacto para enviarlo|Deje la configuración predeterminada.|
    |Nombre de la clase principal|El valor predeterminado es la clase principal del archivo seleccionado. Para cambiar la clase, puede seleccionar el botón de puntos suspensivos (**...** ) y elegir otra clase.|
    |Configuraciones del trabajo|Puede cambiar las claves o valores predeterminados. Para más información, consulte [API de REST de Apache Livy](https://livy.incubator.apache.org./docs/latest/rest-api.html).|
    |Argumentos de la línea de comandos|Puede especificar los argumentos divididos por un espacio para la clase principal, si es necesario.|
    |Archivos jar a los que se hace referencia y archivos a los que se hace referencia|puede especificar las rutas de acceso para los archivos jar y los archivos a los que se hace referencia, si existen. Para obtener más información: [Apache Spark Configuration](https://spark.apache.org/docs/latest/configuration.html#runtime-environment) (Configuración de Apache Spark).  Consulte también [cómo cargar recursos en un clúster](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-storage-explorer).|
    |Almacenamiento de carga del trabajo|Expanda para mostrar opciones adicionales.|
    |Tipo de almacenamiento|Seleccione la opción para **usar Azure Blob para cargar** en la lista desplegable.|
    |Cuenta de almacenamiento|Escriba su cuenta de Storage.|
    |Clave de almacenamiento|Escriba su clave de almacenamiento.|
    |Contenedor de almacenamiento|Seleccione su contenedor de almacenamiento en la lista desplegable una vez que se hayan escrito **Cuenta de Storage** y **Clave de almacenamiento**.|

    ![Cuadro de diálogo de envío de Spark](./media/apache-spark-intellij-tool-plugin/hdi-submit-spark-app-2.png)

4. Seleccione **SparkJobRun** para enviar el proyecto para el clúster seleccionado. En la pestaña **Remote Spark Job in Cluster** (Trabajo remoto de Spark en el clúster) se muestra el progreso de ejecución del trabajo en la parte inferior. Puede detener la aplicación haciendo clic en el botón rojo. Para obtener información sobre cómo tener acceso a la salida de trabajo, consulte la sección "Acceso y administración de clústeres de HDInsight Spark mediante el uso del kit de herramientas de Azure para IntelliJ" que encontrará más adelante en este artículo.  
      
    ![Ventana de envío de Spark](./media/apache-spark-intellij-tool-plugin/hdi-spark-app-result.png)

## <a name="debug-apache-spark-applications-locally-or-remotely-on-an-hdinsight-cluster"></a>Depuración de aplicaciones Apache Spark de forma local o remota en un clúster de HDInsight 
También se recomienda otra manera de enviar la aplicación Spark al clúster. Puede hacerlo estableciendo los parámetros del IDE de **configuraciones de ejecución o depuración**. Para más información, consulte [Depuración de aplicaciones Apache Spark en un clúster de HDInsight con Azure Toolkit for IntelliJ mediante SSH](apache-spark-intellij-tool-debug-remotely-through-ssh.md).

## <a name="access-and-manage-hdinsight-spark-clusters-by-using-azure-toolkit-for-intellij"></a>Acceso y administración de clústeres de HDInsight Spark mediante el uso del kit de herramientas de Azure para IntelliJ
Puede realizar varias operaciones mediante el Kit de herramientas de Azure para IntelliJ.  La mayoría de las operaciones se inician desde **Azure Explorer**.  En la barra de menús, vaya a **Ver** > **Ventanas de herramientas** > **Azure Explorer**.

### <a name="access-the-job-view"></a>Acceso a la vista de trabajo

1. En Azure Explorer, vaya a **HDInsight** > \<Su clúster> > **Trabajos**.

    ![Nodo Vista de trabajos](./media/apache-spark-intellij-tool-plugin/job-view-node.png)

2. En el panel derecho, la pestaña **Spark Job View** (Vista de trabajos de Spark) muestra todas las aplicaciones que se ejecutaron en el clúster. Seleccione el nombre de la aplicación para la que desea ver más detalles.

    ![Detalles de aplicación](./media/apache-spark-intellij-tool-plugin/view-job-logs.png)

3. Para mostrar información básica del trabajo de ejecución, mantenga el mouse sobre el gráfico del trabajo. Para ver el gráfico de fases y la información que todo trabajo genera, seleccione un nodo del gráfico del trabajo.

    ![Detalles de fase de trabajo](./media/apache-spark-intellij-tool-plugin/Job-graph-stage-info.png)

4. Para ver registros usados con frecuencia, como *Driver Stderr*, *Driver Stdout* y *Directory Info*, seleccione la pestaña **Registro**.

    ![Detalles del registro](./media/apache-spark-intellij-tool-plugin/Job-log-info.png)

5. También puede ver Spark History UI (IU de historial de Spark) y YARN UI (IU de YARN), en el nivel de aplicación, al seleccionar los vínculos correspondientes de la parte superior de la ventana.

### <a name="access-the-spark-history-server"></a>Acceso al servidor de historial de Spark

1. En Azure Explorer, expanda **HDInsight**, haga clic con el botón derecho en el nombre del clúster de Spark y seleccione **Abrir IU de historial de Spark**.  
2. Cuando se le solicite, escriba las credenciales de administrador del clúster, que especificó al configurar el clúster.

3. En el panel del servidor de historial de Spark, puede usar el nombre de aplicación para buscar la aplicación que acaba de terminar de ejecutar. En el código anterior, se establece el nombre de la aplicación mediante `val conf = new SparkConf().setAppName("myApp")`. Por lo tanto, el nombre de aplicación Spark es **myApp**.

### <a name="start-the-ambari-portal"></a>Inicio del portal de Ambari

1. En Azure Explorer, expanda **HDInsight**, haga clic con el botón derecho en el nombre del clúster de Spark y seleccione **Abrir el portal de administración de clústeres (Ambari)**.  

2. Cuando se le pida, escriba las credenciales de administrador para el clúster. Estas son las credenciales que especificó durante el proceso de configuración de clúster.

### <a name="manage-azure-subscriptions"></a>Administración de suscripciones de Azure
De forma predeterminada, el kit de herramientas de Azure para IntelliJ enumera los clústeres de Spark de todas las suscripciones de Azure. Si es necesario, puede especificar las suscripciones a las que desea tener acceso.  

1. En Azure Explorer, haga clic con el botón derecho en el nodo raíz de **Azure** y seleccione **Seleccionar suscripciones**.  

2. En la ventana **Seleccionar suscripciones**, desactive las casillas situadas junto a las suscripciones a las que no desea tener acceso y seleccione **Cerrar**.

## <a name="spark-console"></a>Consola de Spark
Puede ejecutar Spark Local Console(Scala) o ejecutar Spark Livy Interactive Session Console(Scala).

### <a name="spark-local-consolescala"></a>Spark Local Console(Scala)
Asegúrese de haber cumplido el requisito previo WINUTILS.EXE.

1. En la barra de menús, vaya a **Ejecutar** > **Editar configuraciones...**.

2. En la ventana **Ejecutar/depurar configuraciones**, en el panel izquierdo, vaya a **Apache Spark en HDInsight** > **[Spark en HDInsight] myApp**.

3. En la ventana principal, seleccione la pestaña **Ejecución en modo local**.

4. Proporcione los valores siguientes y, a continuación, seleccione **Aceptar**:

    |Propiedad |Valor |
    |----|----|
    |Clase Main del trabajo|El valor predeterminado es la clase principal del archivo seleccionado. Para cambiar la clase, puede seleccionar el botón de puntos suspensivos (**...** ) y elegir otra clase.|
    |Variables de entorno|Asegúrese de que el valor de HADOOP_HOME es correcto.|
    |Ubicación de WINUTILS.exe|Asegúrese de que la ruta de acceso es correcta.|

    ![Establecer configuración en la consola local](./media/apache-spark-intellij-tool-plugin/console-set-configuration.png)

5. En Proyecto, vaya a **myApp** > **src** > **main** > **scala** > **myApp**.  

6. En la barra de menús, vaya a **Herramientas** > **Consola de Spark** > **Ejecutar Spark Local Console(Scala)**.

7. A continuación, es posible que se muestren dos cuadros de diálogo para preguntar si desea corregir automáticamente las dependencias. Si es así, seleccione **Corregir automáticamente**.

    ![Corrección automática 1 de Spark](./media/apache-spark-intellij-tool-plugin/console-auto-fix1.png)

    ![Corrección automática 2 de Spark](./media/apache-spark-intellij-tool-plugin/console-auto-fix2.png)

8. La consola debe tener una apariencia similar a la de la siguiente imagen. En la ventana de consola, escriba `sc.appName` y, a continuación, presione ctrl+Intro.  Se mostrará el resultado. Puede terminar la consola local; para ello, haga clic en el botón rojo.

    ![Resultado de la consola local](./media/apache-spark-intellij-tool-plugin/local-console-result.png)


### <a name="spark-livy-interactive-session-consolescala"></a>Spark Livy Interactive Session Console(Scala)
Solo se admite en IntelliJ 2018.2 y 2018.3.

1. En la barra de menús, vaya a **Ejecutar** > **Editar configuraciones...**.

2. En la ventana **Ejecutar/depurar configuraciones**, en el panel izquierdo, vaya a **Apache Spark en HDInsight** > **[Spark en HDInsight] myApp**.

3. En la ventana principal, seleccione la pestaña **Ejecutar de forma remota en clúster**.

4. Proporcione los valores siguientes y, a continuación, seleccione **Aceptar**:

    |Propiedad |Valor |
    |----|----|
    |Clústeres de Spark (solo en Linux)|Seleccione el clúster de HDInsight Spark en el que quiere ejecutar la aplicación.|
    |Nombre de la clase principal|El valor predeterminado es la clase principal del archivo seleccionado. Para cambiar la clase, puede seleccionar el botón de puntos suspensivos (**...** ) y elegir otra clase.|

    ![Establecer configuración en la consola interactiva](./media/apache-spark-intellij-tool-plugin/interactive-console-configuration.png)

5. En Proyecto, vaya a **myApp** > **src** > **main** > **scala** > **myApp**.  

6. En la barra de menús, vaya a **Herramientas** > **Consola de Spark** > **Ejecutar Spark Livy Interactive Session Console(Scala)**.

7. La consola debe tener una apariencia similar a la de la siguiente imagen. En la ventana de consola, escriba `sc.appName` y, a continuación, presione ctrl+Intro.  Se mostrará el resultado. Puede terminar la consola local; para ello, haga clic en el botón rojo.

    ![Resultado de la consola interactiva](./media/apache-spark-intellij-tool-plugin/interactive-console-result.png)

### <a name="send-selection-to-spark-console"></a>Envío de la selección a la consola de Spark

Resulta cómodo predecir el resultado del script mediante el envío de código a la consola local o a Livy Interactive Session Console(Scala). Puede resaltar código en el archivo de Scala y luego hacer clic con el botón derecho en **Enviar selección a la consola de Spark**. El código seleccionado se enviará a la consola y se realizará. El resultado se mostrará después del código en la consola. La consola comprobará los errores, si los hay.  

   ![Envío de la selección a la consola de Spark](./media/apache-spark-intellij-tool-plugin/send-selection-to-console.png)

## <a name="convert-existing-intellij-idea-applications-to-use-azure-toolkit-for-intellij"></a>Conversión de las aplicaciones IntelliJ IDEA existentes para usar el kit de herramientas de Azure para IntelliJ

Puede convertir las aplicaciones Spark en Scala existentes creadas en IntelliJ IDEA para que sean compatibles con el kit de herramientas de Azure para IntelliJ. Esto le permitirá utilizar el complemento para enviar las aplicaciones a un clúster de HDInsight Spark.

1. Para una aplicación Spark en Scala existente creada con IntelliJ IDEA, abra el archivo .iml asociado.

1. En el nivel raíz, hay un elemento de **module** similar al siguiente:
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4">

   Edite el elemento para agregar `UniqueKey="HDInsightTool"` de forma que el elemento de **module** sea similar a lo siguiente:
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4" UniqueKey="HDInsightTool">

1. Guarde los cambios. La aplicación ahora debe ser compatible con el kit de herramientas de Azure para IntelliJ. Puede comprobarlo si hace clic con el botón derecho en el nombre de proyecto en Proyecto. El menú emergente ahora debería tener la opción **Submit Spark Application to HDInsight** (Enviar aplicación Spark a HDInsight).

## <a name="troubleshooting"></a>solución de problemas

### <a name="error-in-local-run-use-a-larger-heap-size"></a>Error en la ejecución local: *Utilice un tamaño de montón mayor*
En Spark 1.6, si utiliza un SDK de Java de 32 bits durante la ejecución local, puede encontrar los siguientes errores:

    Exception in thread "main" java.lang.IllegalArgumentException: System memory 259522560 must be at least 4.718592E8. Please use a larger heap size.
        at org.apache.spark.memory.UnifiedMemoryManager$.getMaxMemory(UnifiedMemoryManager.scala:193)
        at org.apache.spark.memory.UnifiedMemoryManager$.apply(UnifiedMemoryManager.scala:175)
        at org.apache.spark.SparkEnv$.create(SparkEnv.scala:354)
        at org.apache.spark.SparkEnv$.createDriverEnv(SparkEnv.scala:193)
        at org.apache.spark.SparkContext.createSparkEnv(SparkContext.scala:288)
        at org.apache.spark.SparkContext.<init>(SparkContext.scala:457)
        at LogQuery$.main(LogQuery.scala:53)
        at LogQuery.main(LogQuery.scala)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:606)
        at com.intellij.rt.execution.application.AppMain.main(AppMain.java:144)

Estos errores se producen porque el tamaño del montón no es lo suficientemente grande como para que se ejecute Spark. Spark requiere al menos 471 MB. (para obtener más información, consulte [SPARK-12081](https://issues.apache.org/jira/browse/SPARK-12081)). Una solución sencilla es utilizar un SDK de Java de 64 bits. También puede cambiar la configuración de JVM en IntelliJ agregando las siguientes opciones:

    -Xms128m -Xmx512m -XX:MaxPermSize=300m -ea

![Incorporación de opciones a la casilla de opciones de máquina virtual en IntelliJ](./media/apache-spark-intellij-tool-plugin/change-heap-size.png)

## <a name="faq"></a>Preguntas más frecuentes
Si el clúster está ocupado, puede aparecer el siguiente error.

![Intellij presenta errores si el clúster está ocupado](./media/apache-spark-intellij-tool-plugin/intellij-interactive-cluster-busy-upload.png)

![Intellij presenta errores si el clúster está ocupado](./media/apache-spark-intellij-tool-plugin/intellij-interactive-cluster-busy-submit.png)

## <a name="feedback-and-known-issues"></a>Comentarios y problemas conocidos
Actualmente, la visualización de salidas de Spark directamente no se admite.

Si tiene sugerencias o comentarios, o si se le presenta algún problema al usar este complemento, no dude en enviarnos un correo electrónico a la dirección hdivstool@microsoft.com.

## <a name="seealso"></a>Pasos siguientes
* [Información general: Apache Spark en Azure HDInsight](apache-spark-overview.md)

### <a name="demo"></a>Demostración
* Creación de proyecto de Scala (vídeo): [Crear aplicaciones Apache Spark en Scala](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)
* Depuración remota (vídeo): [Use Azure Toolkit for IntelliJ to debug Apache Spark applications remotely on HDInsight Cluster](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ) (Uso del kit de herramientas de Azure para IntelliJ para depurar de forma remota aplicaciones Apache Spark en clústeres de HDInsight)

### <a name="scenarios"></a>Escenarios
* [Apache Spark con BI: Análisis de datos interactivos con Spark en HDInsight con las herramientas de BI](apache-spark-use-bi-tools.md)
* [Apache Spark con Machine Learning: uso de Apache Spark en HDInsight para analizar la temperatura de edificios con los datos del sistema de acondicionamiento de aire](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark con Machine Learning: uso de Spark en HDInsight para predecir los resultados de la inspección de alimentos](apache-spark-machine-learning-mllib-ipython.md)
* [Análisis de registros de un sitio web mediante Apache Spark en HDInsight](apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a>Creación y ejecución de aplicaciones
* [Crear una aplicación independiente con Scala](apache-spark-create-standalone-application.md)
* [Ejecución de trabajos de forma remota en un clúster de Apache Spark mediante Apache Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Herramientas y extensiones
* [Uso de Azure Toolkit for IntelliJ para depurar de forma remota aplicaciones de Apache Spark mediante VPN](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Uso de Azure Toolkit for IntelliJ para depurar de forma remota aplicaciones de Apache Spark mediante SSH](apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Uso de las herramientas de HDInsight para IntelliJ con Hortonworks Sandbox](../hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Uso de las herramientas de HDInsight de Azure Toolkit for Eclipse con el fin de crear aplicaciones Apache Spark](apache-spark-eclipse-tool-plugin.md)
* [Uso de cuadernos de Apache Zeppelin con un clúster Apache Spark en HDInsight](apache-spark-zeppelin-notebook.md)
* [Kernels disponible para Jupyter Notebook en clústeres Apache Spark para HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Uso de paquetes externos con cuadernos de Jupyter Notebook](apache-spark-jupyter-notebook-use-external-packages.md)
* [Instalación de un cuaderno de Jupyter Notebook en el equipo y conexión al clúster de Apache Spark en HDInsight de Azure](apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a>Administración de recursos
* [Administración de recursos para el clúster Apache Spark en HDInsight de Azure](apache-spark-resource-manager.md)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight (Seguimiento y depuración de trabajos que se ejecutan en un clúster de Apache Spark en HDInsight)](apache-spark-job-debugging.md)
