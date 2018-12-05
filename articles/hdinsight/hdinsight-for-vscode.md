---
title: 'Herramientas de Azure HDInsight: uso de Visual Studio Code para Hive, LLAP o PySpark | Microsoft Docs'
description: Aprenda a usar Obtenga información sobre cómo usar Herramientas de Azure HDInsight para Visual Studio Code para crear y enviar consultas y scripts.
Keywords: VS Code,Azure HDInsight Tools,Hive,Python,PySpark,Spark,HDInsight,Hadoop,LLAP,Interactive Hive,Interactive Query
services: HDInsight
documentationcenter: ''
author: jejiang
ms.author: jejiang
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 10/27/2017
ms.openlocfilehash: 9603751db01eaffdf9fbe26164aed53017c5e23c
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "52499539"
---
# <a name="use-azure-hdinsight-tools-for-visual-studio-code"></a>Uso de las Herramientas de Azure HDInsight para Visual Studio Code

Obtenga información sobre cómo usar las [Herramientas de Azure HDInsight para Visual Studio Code](https://docs.microsoft.com/azure/hdinsight/hdinsight-for-vscode) (VS Code) para crear y enviar trabajos por lotes de [Apache Hive](https://hive.apache.org/), consultas interactivas de Apache Hive y scripts de PySpark. Las Azure HDInsight Tools se pueden instalar en las plataformas compatibles con VS Code. Entre ellas se incluyen Windows, Linux y macOS. Encontrará los requisitos previos de cada plataforma.


## <a name="prerequisites"></a>Requisitos previos

Para completar los pasos de este artículo, se requieren los elementos siguientes:

- Un clúster de HDInsight. Para crear un clúster, vea la [introducción a HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md).
- [Visual Studio Code](https://www.visualstudio.com/products/code-vs.aspx)
- [Mono](http://www.mono-project.com/docs/getting-started/install/). Mono solo es obligatorio para Linux y macOS.

## <a name="install-the-hdinsight-tools"></a>Instalar Herramientas de HDInsight
   
Después de instalar los requisitos previos, puede instalar las Herramientas de Azure HDInsight para VS Code. 

### <a name="to-install-azure-hdinsight-tools"></a>Para instalar Herramientas de Azure HDInsight

1. Abra Visual Studio Code.

2. En el panel izquierdo, seleccione **Extensiones**. En el cuadro de búsqueda, escriba **HDInsight**.

3. Junto a **Herramientas de Azure HDInsight** haga clic en **Instalar**. Después de unos segundos, el botón **Instalar** cambia a **Recargar**.

4. Haga clic en **Recargar** para activar la extensión **Herramientas de Azure HDInsight**.

5. Haga clic en **Recargar ventana** para confirmar. Verá **Herramientas de Azure HDInsight** en el panel **Extensiones**.

   ![Instalación de Python de HDInsight para Visual Studio Code](./media/hdinsight-for-vscode/install-hdInsight-plugin.png)

## <a name="open-hdinsight-workspace"></a>Abrir un área de trabajo de HDInsight

Cree un área de trabajo en VS Code para poder conectarse a Azure.

### <a name="to-open-a-workspace"></a>Para abrir un área de trabajo

1. En el menú **Archivo**, seleccione **Abrir carpeta**. A continuación, designe una carpeta existente como carpeta de trabajo o cree una carpeta nueva. La carpeta aparece en el panel izquierdo.

2. En el panel izquierdo, haga clic en el icono **Nuevo archivo** situado junto a la carpeta de trabajo.

   ![Nuevo archivo](./media/hdinsight-for-vscode/new-file.png)

3. Asigne al nuevo archivo el nombre de la extensión de archivo .hql (consultas de Hive) o .py (script de Spark). 

## <a name="connect-to-hdinsight-cluster"></a>Conexión al clúster de HDInsight

Antes de poder enviar scripts a clústeres de HDInsight desde VS Code, debe conectarse a su cuenta de Azure, o vincular un clúster (con el nombre de usuario y la contraseña de Ambari o una cuenta unida a un dominio).

### <a name="to-connect-to-azure"></a>Para conectarse a Azure

1. Si no tiene una carpeta de trabajo y un archivo de script, créelos.

2. Haga clic con el botón derecho en el editor de scripts y seleccione **HDInsight: Login** (HDInsight: inicio de sesión) en el menú contextual. También puede presionar **Ctrl+Mayús+P** y, después, especificar **HDInsight: Login** (HDInsight: inicio de sesión).

    ![Inicio de sesión de Herramientas de HDInsight para Visual Studio Code](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-login.png)

3. Para iniciar sesión, siga las instrucciones del panel **SALIDA**.
    + Para el entorno global, el inicio de sesión de HDInsight desencadenará el proceso de inicio sesión de Azure.

        ![Instrucciones de inicio de sesión para Azure](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-signin.png)

    + Para otros entornos, siga las instrucciones de inicio de sesión.

        ![Instrucciones de inicio de sesión para otro entorno](./media/hdinsight-for-vscode/hdi-azure-hdinsight-hdinsight-signin.png)

    Una vez establecida la conexión, el nombre de la cuenta de Azure se muestra en la barra de estado en la parte inferior izquierda de la ventana de VSCode. 

    > [!NOTE]
    > Debido a un problema conocido de autenticación de Azure es preciso que abra un explorador en modo privado o incógnito. Si la cuenta de Azure tiene dos factores habilitados, se recomienda usar la autenticación de teléfono en lugar de la del PIN.
  

4. Haga clic con el botón derecho en el editor de scripts para abrir el menú contextual. Desde el menú contextual, puede realizar las siguientes tareas:

    - Cerrar la sesión
    - Enumeración de clústeres
    - Establecer los clústeres predeterminados
    - Envío de consultas de Hive interactivas
    - Envío de scripts por lotes de Hive
    - Envíe consultas de PySpark interactivas
    - Enviar scripts por lotes de PySpark
    - Establecer la configuración

<h3 id="linkcluster">Para vincular un clúster</h3>

Puede vincular un clúster normal mediante un nombre de usuario administrado de [Apache Ambari](https://ambari.apache.org/) o vincular un clúster de Hadoop de seguridad de Enterprise Security Pack mediante un nombre de usuario de dominio (como user1@contoso.com).
1. Para abrir la paleta de comandos, presione **CTRL+MAYÚS+P** y, después, escriba **HDInsight: Link a Cluster** (HDInsight: vincular un clúster).

   ![comando de clúster de vinculación](./media/hdinsight-for-vscode/link-cluster-command.png)

2. Escriba la dirección URL del clúster de HDInsight, escriba el nombre de usuario y la contraseña y seleccione el tipo de clúster; si la comprobación se realiza correctamente, se muestra información de operación completada con éxito.
   
   ![cuadro de diálogo de vinculación de clúster](./media/hdinsight-for-vscode/link-cluster-process.png)

   > [!NOTE]
   > Si el clúster se registró en la suscripción de Azure y se vinculó, se usan el nombre de usuario y la contraseña vinculados. 
   
3. Puede ver un clúster vinculado mediante el comando **List Cluster**. Ahora puede enviar un script a este clúster vinculado.

   ![clúster vinculado](./media/hdinsight-for-vscode/linked-cluster.png)

4. También puede desvincular un clúster si escribe **HDInsight: Unlink a cluster** (HDInsight: desvincular un clúster) desde la paleta de comandos.


### <a name="to-link-a-generic-apache-livy-endpoint"></a>Para vincular un punto de conexión de Apache Livy genérico

1. Para abrir la paleta de comandos, presione **CTRL+MAYÚS+P** y, después, escriba **HDInsight: Link a Cluster** (HDInsight: vincular un clúster).
2. Seleccione **Generic Livy Endpoint** (Punto de conexión de Livy genérico).
3. Escriba el punto de conexión de Livy genérico, por ejemplo: http://10.172.41.42:18080.
4. Seleccione **Basic** (Básico) cuando necesite autorización para el punto de conexión de Livy genérico; en caso contrario, seleccione **None** (Ninguno).
5. Si ha seleccionado **Basic** en el paso 4, escriba el nombre de usuario.
6. Si ha seleccionado **Basic** en el paso 4, escriba la contraseña.
7. La vinculación del punto de conexión de Livy genérico se ha realizado correctamente.

   ![Clúster de Livy genérico vinculado](./media/hdinsight-for-vscode/link-cluster-process-generic-livy.png)

## <a name="list-hdinsight-clusters"></a>Enumerar los clústeres de HDInsight

Para probar la conexión, puede enumerar los clústeres de HDInsight:

### <a name="to-list-hdinsight-clusters-under-your-azure-subscription"></a>Para enumerar los clústeres de HDInsight en la suscripción de Azure
1. Abra un área de trabajo y conéctese a Azure. Para más información, consulte [Abrir un área de trabajo de HDInsight](#open-hdinsight-workspace) y [Conexión a Azure](#connect-to-hdinsight-cluster).

2. Haga clic con el botón derecho en el editor de scripts y, después, seleccione **HDInsight: List Cluster** (HDInsight: enumerar clústeres) en el menú contextual. 

3. Los clústeres de HDInsight aparecen en el panel **Salida**.

    ![Establecer la configuración de un clúster predeterminado](./media/hdinsight-for-vscode/list-cluster-result.png)

## <a name="set-a-default-cluster"></a>Establecimiento de un clúster predeterminado
1. Abra un área de trabajo y conéctese a Azure. Vea [Abrir un área de trabajo de HDInsight](#open-hdinsight-workspace) y [Conexión a Azure](#connect-to-hdinsight-cluster).

2. Haga clic con el botón derecho en el editor de scripts y, después, seleccione **HDInsight: Set Default Cluster** (HDInsight: establecer clúster predeterminado). 

3. Seleccione un clúster como clúster predeterminado del archivo de script actual. Las herramientas actualizan automáticamente el archivo de configuración **.VSCode\settings.json**. 

   ![Establecer la configuración de un clúster predeterminado](./media/hdinsight-for-vscode/set-default-cluster-configuration.png)

## <a name="set-the-azure-environment"></a>Establecimiento del entorno de Azure
1. Abra la paleta de comandos presionando **CTRL+MAYÚS+P**.

2. Escriba **HDInsight: establecer el entorno Azure**.

3. Seleccione un entorno, como "Azure" o "AzureChina", como la entrada de registro predeterminada.

4. Mientras tanto, la herramienta ya ha guardado la entrada de inicio de sesión predeterminada en **.VSCode\settings.json**. También puede actualizarlo directamente en este archivo de configuración. 

   ![Establecer la configuración de entrada de inicio de sesión predeterminada](./media/hdinsight-for-vscode/set-default-login-entry-configuration.png)

## <a name="submit-interactive-hive-queries-hive-batch-scripts"></a>Envío de consultas de Hive interactivas, script por lotes de Hive

Con Herramientas de HDInsight para VS Code, puede enviar consultas de Hive interactivas, scripts por lotes de Hive a clústeres de HDInsight.

1. Si no tiene una carpeta de trabajo y un archivo de script Hive, créelos.

2. Conéctese a la cuenta de Azure o a los clústeres de vínculo.

3. Copie y pegue el código siguiente en el archivo de Hive y guárdelo.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```
4. Haga clic con el botón derecho en el editor de scripts, seleccione **HDInsight: Hive Interactive** (HDInsight: Hive interactivo) para enviar la consulta, o bien use el método abreviado de teclado **Ctrl + Alt + I**. Seleccione **HDInsight: Hive Batch** (HDInsight: lote de Hive) para enviar el script, o bien use el método abreviado de teclado **Ctrl + Alt + H**. 

5. Seleccione el clúster si no ha especificado un clúster predeterminado. Las herramientas también permiten enviar un bloque de código, en lugar del archivo de script completo mediante el menú contextual. Transcurridos unos instantes, los resultados de la consulta aparecen en una pestaña nueva.

   ![Resultado de Hive interactivo](./media/hdinsight-for-vscode/interactive-hive-result.png)

    - Panel **RESULTADOS**: puede guardar todo el resultado en forma de archivo CSV, JSON o de Excel en una ruta de acceso local, o bien seleccionar únicamente varias líneas.

    - Panel **MENSAJES**: al seleccionar el número de **Línea**, salta la primera línea del script en ejecución.

## <a name="submit-interactive-pyspark-queries"></a>Envíe consultas de PySpark interactivas

### <a name="to-submit-interactive-pyspark-queries-to-hdinsight-spark-clusters"></a>Para enviar consultas de PySpark interactivas a clústeres de HDInsight Spark.

1. Cree una carpeta de trabajo y un archivo de script con la extensión .py si no los tiene aún.

2. Conéctese a su cuenta de Azure si no lo ha hecho aún.

3. Copie y pegue el código siguiente en el archivo de script:
   ```python
   from operator import add
   lines = spark.read.text("/HdiSamples/HdiSamples/FoodInspectionData/README").rdd.map(lambda r: r[0])
   counters = lines.flatMap(lambda x: x.split(' ')) \
                .map(lambda x: (x, 1)) \
                .reduceByKey(add)

   coll = counters.collect()
   sortedCollection = sorted(coll, key = lambda r: r[1], reverse = True)

   for i in range(0, 5):
        print(sortedCollection[i])
   ```
4. Resalte este script. Después, haga clic con el botón derecho en el editor de scripts y seleccione **HDInsight: PySpark Interactive** (HDInsight: PySpark interactivo), o bien use el método abreviado de teclado **Ctrl + Alt + I**.

5. Si aún no ha instalado la extensión **Python** en VS Code, haga clic en el botón **Instalar**, como se muestra en la siguiente ilustración:

    ![Instalación de Python de HDInsight para Visual Studio Code](./media/hdinsight-for-vscode/hdinsight-vscode-install-python.png)

6. Instale el entorno de Python en el sistema, en caso de que aún no lo haya hecho. 
   - Para Windows, descargue e instale [Python](https://www.python.org/downloads/). Luego, asegúrese de que `Python` y `pip` están en la RUTA DE ACCESO del sistema.

   - Para ver las instrucciones para macOS y Linux, consulte [Configuración del entorno interactivo de PySpark para Visual Studio Code](set-up-pyspark-interactive-environment.md).

7. Seleccione el clúster al que va a enviar la consulta de PySpark. Poco después, el resultado de la consulta se mostrará en la pestaña nueva de la derecha:

   ![Enviar el resultado del trabajo de Python](./media/hdinsight-for-vscode/pyspark-interactive-result.png) 
8. La herramienta también admite la consulta **Cláusula SQL**.

   ![Enviar resultado de trabajo de Python](./media/hdinsight-for-vscode/pyspark-ineteractive-select-result.png) El estado del envío aparece en la parte izquierda de la barra de estado inferior cuando se ejecutan consultas. No envíe otras consultas cuando el estado sea **PySpark Kernel (busy)** [Kernel de PySpark (ocupado)]. 

>[!NOTE]
>Los clústeres pueden mantener la información de la sesión. La variable definida, la función y los valores correspondientes se conservan en la sesión, con el fin de se pueda hacer referencia a ellos en varias llamadas de servicio para el mismo clúster. 

### <a name="to-disable-environment-check"></a>Para deshabilitar la comprobación del entorno

De forma predeterminada, las herramientas de HDInsight comprobarán del entorno e instalarán paquetes dependientes al enviar consultas de PySpark interactivas. Para deshabilitar la comprobación del entorno, establezca **hdinsight.disablePysparkEnvironmentValidation** en **yes** en **USER SETTINGS** (CONFIGURACIÓN DE USUARIO).

   ![Establecimiento de la comprobación del entorno desde configuración](./media/hdinsight-for-vscode/hdi-azure-hdinsight-environment-check.png)

Como alternativa, haga clic en el botón **Disable Validation** (Deshabilitar la validación) cuando aparezca el cuadro de diálogo.

   ![Establecimiento de la comprobación del entorno desde el cuadro de diálogo](./media/hdinsight-for-vscode/hdi-azure-hdinsight-environment-check-dialog.png)

### <a name="pyspark3-is-not-supported-with-spark2223"></a>PySpark3 no es compatible con Spark 2.2/2.3

PySpark3 ya no se admite con el clúster de Spark 2.2 y Spark 2.3, solo se admite "PySpark" para Python. Se sabe que con Python 3 se produce un error en los envíos a Spark 2.2/2.3.

   ![Error get de envío de Python 3](./media/hdinsight-for-vscode/hdi-azure-hdinsight-py3-error.png)

Siga los pasos para usar Python 2.x: 

1. Instale Python 2.7 en el equipo local y agréguelo a la ruta de acceso del sistema.

2. Reinicie VSCode.

3. Para cambiar a Python 2, haga clic en **Python XXX** en la barra de estado y, después, elija la versión de Python de destino.

   ![Selección de la versión de Python](./media/hdinsight-for-vscode/hdi-azure-hdinsight-select-python.png)

## <a name="submit-pyspark-batch-job"></a>Envío de un trabajo por lotes de PySpark

1. Cree una carpeta de trabajo y un archivo de script con la extensión .py si no los tiene aún.

2. Conéctese a la cuenta de Azure si todavía no lo ha hecho.

3. Copie y pegue el código siguiente en el archivo de script:

    ```python
    from __future__ import print_function
    import sys
    from operator import add
    from pyspark.sql import SparkSession
    if __name__ == "__main__":
        spark = SparkSession\
            .builder\
            .appName("PythonWordCount")\
            .getOrCreate()
    
        lines = spark.read.text('/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv').rdd.map(lambda r: r[0])
        counts = lines.flatMap(lambda x: x.split(' '))\
                    .map(lambda x: (x, 1))\
                    .reduceByKey(add)
        output = counts.collect()
        for (word, count) in output:
            print("%s: %i" % (word, count))
        spark.stop()
    ```
4. Haga clic con el botón derecho en el editor de scripts y seleccione **HDInsight: PySpark Batch** (HDInsight: lote de PySpark), o bien use el método abreviado de teclado **Ctrl + Alt + H**. 

5. Seleccione el clúster al que va a enviar el trabajo de PySpark. 

   ![Enviar el resultado del trabajo de Python](./media/hdinsight-for-vscode/submit-pythonjob-result.png) 

Después de enviar un trabajo de Python, los registros de envío aparecen en la ventana **SALIDA** de VS Code. También se muestran la **dirección URL de interfaz de usuario de Spark** y la **dirección URL de interfaz de usuario de Yarn**. Puede abrir la dirección URL en un explorador web para realizar el seguimiento de estado del trabajo.

## <a name="apache-livy-configuration"></a>Configuración de Apache Livy

Se admite la configuración de [Apache Livy](https://livy.incubator.apache.org/); se puede establecer en **.VSCode\settings.json** en la carpeta del área de trabajo. En la actualidad, la configuración de Livy solo admite el script de Python. Para obtener más información, consulte el archivo [léame de Livy](https://github.com/cloudera/livy/blob/master/README.rst ).

<a id="triggerlivyconf"></a>**Cómo activar la configuración de Livy**
   
Se puede encontrar en el menú **File** (Archivo); seleccione **Preferences** (Preferencias) y elija **Settings** (Configuración) en el menú contextual. Haga clic en la pestaña **WORKSPACE SETTINGS** (CONFIGURACIÓN DEL ÁREA DE TRABAJO) para empezar a establecer la configuración de Livy.

También puede enviar un archivo; observe que la carpeta .vscode se agrega automáticamente a la carpeta de trabajo. Puede encontrar la configuración de Livy si hace clic en **.vscode\settings.json**.

+ Configuración de proyecto:

    ![Configuración de Livy](./media/hdinsight-for-vscode/hdi-livyconfig.png)

>[!NOTE]
>Para las opciones **driverMomory** y **executorMomry**, establezca el valor con la unidad, por ejemplo, 1 g o 1024 m. 

+ Configuraciones de Livy admitidas:   

    **POST /batches**   
    Cuerpo de la solicitud

    | Nombre | description | Tipo | 
    | :- | :- | :- | 
    | file | Archivo que contiene la aplicación para ejecutar | ruta de acceso (obligatorio) | 
    | proxyUser | Usuario para suplantar cuando se ejecuta el trabajo | string | 
    | className | Clase principal de Spark o Java de la aplicación | string |
    | args | Argumentos de la línea de comandos para la aplicación | lista de cadenas | 
    | jars | Archivos JAR que se usarán en esta sesión | Lista de cadenas | 
    | pyFiles | Archivos Python que se usarán en esta sesión | Lista de cadenas |
    | files | Archivos que se usarán en esta sesión | Lista de cadenas |
    | driverMemory | Cantidad de memoria que se usará para el proceso de controlador | string |
    | driverCores | Número de núcleos que se usarán para el proceso de controlador | int |
    | executorMemory | Cantidad de memoria que se usará por proceso ejecutor | string |
    | executorCores | Número de núcleos que se usarán para cada ejecutor | int |
    | numExecutors | Número de ejecutores para iniciar en esta sesión | int |
    | archives | Archivos que se usarán en esta sesión | Lista de cadenas |
    | queue | El nombre de la cola YARN a la que se envió | string |
    | Nombre | Nombre de esta sesión | string |
    | conf | Propiedades de configuración de Spark. | Asignación de clave = val |

    Cuerpo de respuesta   
    El objeto Batch creado.

    | Nombre | description | Tipo | 
    | :- | :- | :- | 
    | id | Identificador de sesión | int | 
    | appId | Identificador de la aplicación de esta sesión |  string |
    | appInfo | Información detallada de la aplicación | Asignación de clave = val |
    | log | Líneas de registro | lista de cadenas |
    | state |   Estado de lote | string |

>[!NOTE]
>La configuración de Livy asignada se mostrará en el panel de salida al enviar el script.

## <a name="integrate-with-azure-hdinsight-from-explorer"></a>Integración con Azure HDInsight desde el explorador

Azure HDInsight se ha agregado al panel de la izquierda. Puede examinar y administrar el clúster directamente.

1. Expanda **AZURE HDINSIGHT**, si no ha iniciado sesión se mostrará el vínculo **Inicie sesión en Azure...**.

    ![Imagen del vínculo de inicio de sesión](./media/hdinsight-for-vscode/hid-azure-hdinsight-sign-in.png)

2. Haga clic en **Iniciar sesión en Azure**; aparecerá el vínculo de inicio de sesión y el código en la parte inferior derecha.

    ![Instrucciones de inicio de sesión para otro entorno](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-signin-code.png)

3. Al hacer clic en el botón **Copy & Open** (Copiar y abrir) se abrirá el explorador; pegue el código, haga clic en el botón **Continue** (Continuar) y verá la sugerencia sobre el inicio de sesión correcto.

4. Después de iniciar sesión, se enumerarán en **AZURE HDINSIGHT** las suscripciones y los clústeres disponibles (se admiten Spark, Hadoop y HBase). 

   ![Suscripción de Azure HDInsight](./media/hdinsight-for-vscode/hdi-azure-hdinsight-subscription.png)

5. Expanda el clúster para ver el esquema de base de datos y tablas de metadatos de Hive.

   ![Clúster de Azure HDInsight](./media/hdinsight-for-vscode/hdi-azure-hdinsight-cluster.png)

## <a name="additional-features"></a>Características adicionales

HDInsight para VS Code admite las siguientes características:

- **Autocompletar de IntelliSense**. Emergen sugerencias para la palabra clave, los métodos, las variables, etc. Los distintos iconos representan diferentes tipos de objetos.

    ![Tipos de objetos de IntelliSense de Herramientas de HDInsight para Visual Studio Code](./media/hdinsight-for-vscode/hdinsight-for-vscode-auto-complete-objects.png)
- **Marcador de error de IntelliSense**. El servicio de lenguaje subraya los errores de edición del script de Hive.     
- **Aspectos destacados de la sintaxis**. El servicio de lenguaje usa colores diferentes para diferenciar las variables, palabras clave, tipos de datos, funciones, etc. 

    ![Aspectos destacados de la sintaxis de Herramientas de HDInsight para Visual Studio Code](./media/hdinsight-for-vscode/hdinsight-for-vscode-syntax-highlights.png)

## <a name="next-steps"></a>Pasos siguientes

### <a name="demo"></a>Demostración
* HDInsight para VS Code: [vídeo](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>Herramientas y extensiones

* [Uso de Azure Toolkit for IntelliJ para depurar de forma remota aplicaciones de Apache Spark mediante VPN](spark/apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Uso de Azure Toolkit for IntelliJ para depurar de forma remota aplicaciones de Apache Spark mediante SSH](spark/apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Uso de las herramientas de HDInsight para IntelliJ con Hortonworks Sandbox](hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Uso de las herramientas de HDInsight de Azure Toolkit for Eclipse con el fin de crear aplicaciones Apache Spark](spark/apache-spark-eclipse-tool-plugin.md)
* [Uso de cuadernos de Apache Zeppelin con un clúster Apache Spark en HDInsight](spark/apache-spark-zeppelin-notebook.md)
* [Kernels disponible para Jupyter Notebook en clústeres Apache Spark para HDInsight](spark/apache-spark-jupyter-notebook-kernels.md)
* [Uso de paquetes externos con cuadernos de Jupyter Notebook](spark/apache-spark-jupyter-notebook-use-external-packages.md)
* [Instalación de un cuaderno de Jupyter Notebook en el equipo y conexión al clúster de Apache Spark en HDInsight de Azure](spark/apache-spark-jupyter-notebook-install-locally.md)
* [Visualización de datos de Apache Hive con Microsoft Power BI en Azure HDInsight](hadoop/apache-hadoop-connect-hive-power-bi.md)
* [Visualización de datos de Interactive Query Hive con Power BI en Azure HDInsight](./interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).
* [Configuración del entorno interactivo de PySpark para Visual Studio Code](set-up-pyspark-interactive-environment.md)
* [Uso de Apache Zeppelin para ejecutar consultas de Apache Hive en Azure HDInsight](./hdinsight-connect-hive-zeppelin.md)

### <a name="scenarios"></a>Escenarios
* [Apache Spark con BI: Realización de análisis de datos interactivos con Spark en HDInsight con las herramientas de BI](spark/apache-spark-use-bi-tools.md)
* [Apache Spark con Machine Learning: Uso de Spark en HDInsight para analizar la temperatura de un edificio mediante datos de HVAC](spark/apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark con Machine Learning: Uso de Spark en HDInsight para predecir los resultados de la inspección de alimentos](spark/apache-spark-machine-learning-mllib-ipython.md)
* [Análisis de registros de un sitio web mediante Apache Spark en HDInsight](spark/apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-running-applications"></a>Creación y ejecución de aplicaciones
* [Crear una aplicación independiente con Scala](spark/apache-spark-create-standalone-application.md)
* [Ejecución de trabajos de forma remota en un clúster de Apache Spark mediante Apache Livy](spark/apache-spark-livy-rest-interface.md)

### <a name="manage-resources"></a>Administración de recursos
* [Administración de recursos para el clúster Apache Spark en HDInsight de Azure](spark/apache-spark-resource-manager.md)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight (Seguimiento y depuración de trabajos que se ejecutan en un clúster de Apache Spark en HDInsight)](spark/apache-spark-job-debugging.md)



