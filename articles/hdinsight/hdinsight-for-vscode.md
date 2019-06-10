---
title: 'Herramientas de HDInsight de Azure: usar Visual Studio Code para Hive, LLAP o PySpark'
description: Aprenda a usar Obtenga información sobre cómo usar Herramientas de Azure HDInsight para Visual Studio Code para crear y enviar consultas y scripts.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/30/2019
ms.openlocfilehash: d114a1e62ae0d28e7d4a3ad453d5d7bd3e1d5b7a
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66427691"
---
# <a name="use-azure-hdinsight-tools-for-visual-studio-code"></a>Uso de las Herramientas de Azure HDInsight para Visual Studio Code

Aprenda a usar las Herramientas de Azure HDInsight para Visual Studio Code para crear y enviar trabajos por lotes de Apache Hive, consultas de Hive interactivas y scripts de PySpark para Apache Spark. En primer lugar, vamos a describir cómo instalar las herramientas de HDInsight en Visual Studio Code y luego veremos cómo enviar trabajos a Hive y Spark.  

Las Herramientas de Azure HDInsight se pueden instalar en las plataformas compatibles con Visual Studio Code, lo que incluye Windows, Linux y macOS. A continuación encontrará los requisitos previos de cada plataforma.


## <a name="prerequisites"></a>Requisitos previos

Para completar los pasos de este artículo, se requieren los elementos siguientes:

- Un clúster de HDInsight. Para crear un clúster, vea la [introducción a HDInsight](hadoop/apache-hadoop-linux-create-cluster-get-started-portal.md).
- [Visual Studio Code](https://code.visualstudio.com/)
- [Mono](https://www.mono-project.com/docs/getting-started/install/). Mono solo es obligatorio para Linux y macOS.
- La [extensión de la cuenta de Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) para Visual Studio Code.
- [Configuración del entorno interactivo de PySpark para Visual Studio Code](set-up-pyspark-interactive-environment.md).
- Un directorio local llamado **HDexample**.  En este artículo se usa **C:\HD\HDexample**.

## <a name="install-azure-hdinsight-tools"></a>Instalación de Herramientas de Azure HDInsight

Después de completar los requisitos previos, puede instalar las Herramientas de Azure HDInsight para Visual Studio Code.  Complete los pasos siguientes para instalar las Herramientas de Azure HDInsight:

1. Abra Visual Studio Code.

2. En la barra de menús, vaya a **Vista** > **Extensiones**.

3. En el cuadro de búsqueda, escriba **HDInsight**.

4. Seleccione **Herramientas de Azure HDInsight** en los resultados de la búsqueda y, a continuación, seleccione **Instalar**.  

   ![Instalación de Python de HDInsight para Visual Studio Code](./media/hdinsight-for-vscode/install-hdInsight-plugin.png)

5. Haga clic en **Recargar** para activar la extensión **Herramientas de Azure HDInsight** después de que se cargue.


## <a name="open-hdinsight-work-folder"></a>Apertura de la carpeta de trabajo de HDInsight

Complete los pasos siguientes para abrir una carpeta de trabajo y cree un archivo en Visual Studio Code:

1. En la barra de menús, vaya a **Archivo** > **Abrir carpeta...**  > **C:\HD\HDexample** y, a continuación, seleccione el botón **Seleccionar carpeta**. La carpeta aparece en la vista **Explorador** a la izquierda.

2. En la vista **Explorador**, seleccione la carpeta, **HDexample** y, a continuación, el icono **Nuevo archivo** situado junto a la carpeta de trabajo.

   ![Nuevo archivo](./media/hdinsight-for-vscode/new-file.png)

3. Asigne al nuevo archivo el nombre de la extensión de archivo .hql (consultas de Hive) o .py (script de Spark).  En este ejemplo se utiliza **HelloWorld.hql**.

## <a name="connect-to-hdinsight-cluster"></a>Conexión al clúster de HDInsight

Antes de poder enviar scripts a clústeres de HDInsight desde Visual Studio Code, debe conectarse a su cuenta de Azure, o vincular un clúster (con el nombre de usuario y la contraseña de Ambari o una cuenta unida a un dominio).  Siga estos pasos para conectarse a Azure:

1. En la barra de menús, vaya a **Vista** > **Paleta de comandos...** y escriba **HDInsight: Login** (HDInsight: inicio de sesión).

    ![Inicio de sesión de Herramientas de HDInsight para Visual Studio Code](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-login.png)

2. Siga las instrucciones que aparecen en el inicio de sesión el **salida** panel.
    + Para el entorno global de Azure, el comando **HDInsight: Login** desencadenará la acción **Iniciar sesión en Azure** en el explorador de HDInsight y viceversa.

        ![Instrucciones de inicio de sesión para Azure](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-signin.png)

    + Para otros entornos, siga la instrucciones de inicio de sesión.

        ![Instrucciones de inicio de sesión para otro entorno](./media/hdinsight-for-vscode/hdi-azure-hdinsight-hdinsight-signin.png)

   Una vez establecida la conexión, el nombre de la cuenta de Azure se muestra en la barra de estado en la parte inferior izquierda de la ventana de Visual Studio Code.  
  

<h2 id="linkcluster">Creación de vínculo: HDInsight de Azure</h2>

Puede vincular un clúster normal mediante un nombre de usuario administrado de [Apache Ambari](https://ambari.apache.org/) o vincular un clúster de Hadoop de seguridad de Enterprise Security Pack mediante un nombre de usuario de dominio (como user1@contoso.com).

1. En la barra de menús, vaya a **Vista** > **Paleta de comandos...** y escriba **HDInsight: Link a Cluster** (HDInsight: vincular un clúster).

   ![comando de clúster de vinculación](./media/hdinsight-for-vscode/link-cluster-command.png)

2. Seleccione el tipo de clúster vinculado **Azure HDInsight**.

3. Escriba la dirección URL del clúster de HDInsight.

4. Escriba el nombre de usuario de Ambari, el valor predeterminado es **admin**.

5. Escriba la contraseña de Ambari.

6. Seleccione el tipo de clúster.

7. Revise la vista **SALIDA** para comprobarlo todo.

   > [!NOTE]  
   > Si el clúster se registró en la suscripción de Azure y se vinculó, se usan el nombre de usuario y la contraseña vinculados.  


## <a name="create-link-generic-livy-endpoint"></a>Creación de vínculo: Generic Livy Endpoint (Punto de conexión de Livy genérico)

1. En la barra de menús, vaya a **Vista** > **Paleta de comandos...** y escriba **HDInsight: Link a Cluster** (HDInsight: vincular un clúster).

2. Seleccione el tipo de clúster vinculado **Generic Livy Endpoint** (Punto de conexión de Livy genérico).

3. Escriba el punto de conexión de Livy genérico, por ejemplo: http\://10.172.41.42:18080.

4. Seleccione el tipo de autorización **Básica** o **Ninguna**.  Si es **Básica**, haga lo siguiente:  
    &emsp;a. Escriba el nombre de usuario de Ambari, el valor predeterminado es **admin**.  
    &emsp;b. Escriba la contraseña de Ambari.

5. Revise la vista **SALIDA** para comprobarlo todo.

## <a name="list-hdinsight-clusters"></a>Enumerar los clústeres de HDInsight

1. En la barra de menús, vaya a **Vista** > **Paleta de comandos...** y escriba **HDInsight: List Cluster** (HDInsight: enumeración de clústeres).

2. Seleccione la suscripción deseada.

3. Revise la vista **SALIDA**.  La vista mostrará los clústeres vinculados y todos los clústeres en su suscripción de Azure.

    ![Establecer la configuración de un clúster predeterminado](./media/hdinsight-for-vscode/list-cluster-result.png)

## <a name="set-default-cluster"></a>Establecer el clúster predeterminado

1. Vuelva a abrir la carpeta **HDexample** creada [anteriormente](#open-hdinsight-work-folder) si se ha cerrado.  

2. Seleccione el archivo **HelloWorld.hql** creado [anteriormente](#open-hdinsight-work-folder) y se abrirá en el editor de scripts.

3. [Conéctese](#connect-to-hdinsight-cluster) a su cuenta de Azure si no lo ha hecho aún.

4. Haga clic con el botón derecho en el editor de scripts y seleccione **HDInsight: Set Default Cluster** (HDInsight: establecer el clúster predeterminado).  

5. Seleccione un clúster como clúster predeterminado del archivo de script actual. Las herramientas actualizan automáticamente el archivo de configuración **.VSCode\settings.json**. 

   ![Establecer la configuración de un clúster predeterminado](./media/hdinsight-for-vscode/set-default-cluster-configuration.png)

## <a name="set-the-azure-environment"></a>Establecimiento del entorno de Azure

1. [Conéctese](#connect-to-hdinsight-cluster) a su cuenta de Azure si no lo ha hecho aún.

2. En la barra de menús, vaya a **Vista** > **Paleta de comandos...** y escriba **HDInsight: Set Azure Environment** (HDInsight: establecer el entorno de Azure).

3. Seleccione un entorno como entrada de inicio de sesión predeterminada.

4. Mientras tanto, la herramienta ya ha guardado la entrada de inicio de sesión predeterminada en **.VSCode\settings.json**. También puede actualizarlo directamente en este archivo de configuración. 

   ![Establecer la configuración de entrada de inicio de sesión predeterminada](./media/hdinsight-for-vscode/set-default-login-entry-configuration.png)


## <a name="submit-interactive-hive-queries-hive-batch-scripts"></a>Envío de consultas de Hive interactivas, script por lotes de Hive

Con Herramientas de HDInsight para Visual Studio Code, puede enviar consultas de Hive interactivas y scripts por lotes de Hive a clústeres de HDInsight.

1. Vuelva a abrir la carpeta **HDexample** creada [anteriormente](#open-hdinsight-work-folder) si se ha cerrado.  

2. Seleccione el archivo **HelloWorld.hql** creado [anteriormente](#open-hdinsight-work-folder) y se abrirá en el editor de scripts.

3. [Conéctese](#connect-to-hdinsight-cluster) a su cuenta de Azure si no lo ha hecho aún.

4. Copie y pegue el código siguiente en el archivo de Hive y guárdelo.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```

5. Haga clic con el botón derecho en el editor de scripts y seleccione **HDInsight: Hive Interactive** (HDInsight: Hive interactivo) para enviar la consulta, o bien use el acceso directo **Ctrl + Alt + I**.  Seleccione **HDInsight: Hive Batch** (HDInsight: lote de Hive) para enviar el script, o bien use el acceso directo **Ctrl + Alt + H**.  

6. Seleccione el clúster si no ha especificado un clúster predeterminado. Las herramientas también permiten enviar un bloque de código, en lugar del archivo de script completo mediante el menú contextual. Transcurridos unos instantes, los resultados de la consulta aparecen en una pestaña nueva.

   ![Resultado de Hive interactivo](./media/hdinsight-for-vscode/interactive-hive-result.png)

    - Panel **RESULTADOS**: puede guardar todo el resultado en forma de archivo CSV, JSON o de Excel en una ruta de acceso local, o bien seleccionar únicamente varias líneas.

    - Panel **MENSAJES**: al seleccionar el número de **Línea**, se salta a la primera línea del script en ejecución.

## <a name="submit-interactive-pyspark-queries"></a>Envíe consultas de PySpark interactivas

1. Vuelva a abrir la carpeta **HDexample** creada [anteriormente](#open-hdinsight-work-folder) si se ha cerrado.  

2. Cree un nuevo archivo **HelloWorld.py** siguiendo los pasos [anteriores](#open-hdinsight-work-folder).

3. Si no instaló Python como requisito previo, recibirá un cuadro de diálogo de recomendación de la extensión de Python.  Instale y vuelva a cargar Visual Studio Code para completar la instalación.

    >![Instalación de Python de HDInsight para Visual Studio Code](./media/hdinsight-for-vscode/hdinsight-vscode-install-python.png)

4. [Conéctese](#connect-to-hdinsight-cluster) a su cuenta de Azure si no lo ha hecho aún.

5. Copie y pegue el código siguiente en el archivo de script:
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

6. Haga clic con el botón derecho en el editor de scripts y seleccione **HDInsight: PySpark Interactive** (HDInsight: PySpark interactivo) para enviar la consulta, o bien use el acceso directo **Ctrl + Alt + I**.  

7. Seleccione el clúster si no ha especificado un clúster predeterminado. Las herramientas también permiten enviar un bloque de código, en lugar del archivo de script completo mediante el menú contextual. Transcurridos unos instantes, los resultados de la consulta aparecen en una pestaña nueva.

   ![Enviar el resultado del trabajo de Python](./media/hdinsight-for-vscode/pyspark-interactive-result.png) 

8. La herramienta también admite la consulta **Cláusula SQL**.

   ![Enviar resultado de trabajo de Python](./media/hdinsight-for-vscode/pyspark-ineteractive-select-result.png) El estado del envío aparece en la parte izquierda de la barra de estado inferior cuando se ejecutan consultas. No envíe otras consultas cuando el estado sea **PySpark Kernel (busy)** [Kernel de PySpark (ocupado)].  

>[!NOTE]  
>Los clústeres pueden mantener la información de la sesión. La variable definida, la función y los valores correspondientes se conservan en la sesión, con el fin de se pueda hacer referencia a ellos en varias llamadas de servicio para el mismo clúster. 

### <a name="pyspark3-is-not-supported-with-spark2223"></a>PySpark3 no es compatible con Spark 2.2/2.3

PySpark3 ya no se admite con el clúster de Spark 2.2 y Spark 2.3, solo se admite "PySpark" para Python. Se sabe que con Python 3 se produce un error en los envíos a Spark 2.2/2.3.

   ![Error get de envío de Python 3](./media/hdinsight-for-vscode/hdi-azure-hdinsight-py3-error.png)

Siga los pasos para usar Python 2.x: 

1. Instale Python 2.7 en el equipo local y agréguelo a la ruta de acceso del sistema.

2. Reinicie Visual Studio Code.

3. Para cambiar a Python 2, haga clic en **Python XXX** en la barra de estado y, después, elija la versión de Python de destino.

   ![Selección de la versión de Python](./media/hdinsight-for-vscode/hdi-azure-hdinsight-select-python.png)


## <a name="submit-pyspark-batch-job"></a>Envío de un trabajo por lotes de PySpark

1. Vuelva a abrir la carpeta **HDexample** creada [anteriormente](#open-hdinsight-work-folder) si se ha cerrado.  

2. Cree un nuevo archivo **BatchFile.py** siguiendo los pasos [anteriores](#open-hdinsight-work-folder).

3. [Conéctese](#connect-to-hdinsight-cluster) a su cuenta de Azure si no lo ha hecho aún.

4. Copie y pegue el código siguiente en el archivo de script:

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

4. Haga clic con el botón derecho en el editor de scripts y, después, seleccione **HDInsight: PySpark Batch** (HDInsight: lote de PySpark), o bien use el acceso directo **Ctrl + Alt + H**. 

5. Seleccione el clúster al que va a enviar el trabajo de PySpark. 

   ![Enviar el resultado del trabajo de Python](./media/hdinsight-for-vscode/submit-pythonjob-result.png) 

Después de enviar un trabajo de Python, los registros de envío aparecen en la ventana **SALIDA** de Visual Studio Code. También se muestran la **dirección URL de interfaz de usuario de Spark** y la **dirección URL de interfaz de usuario de Yarn**. Puede abrir la dirección URL en un explorador web para realizar el seguimiento de estado del trabajo.

## <a name="apache-livy-configuration"></a>Configuración de Apache Livy

Se admite la configuración de [Apache Livy](https://livy.incubator.apache.org/); se puede establecer en **.VSCode\settings.json** en la carpeta del área de trabajo. En la actualidad, la configuración de Livy solo admite el script de Python. Para obtener más información, consulte el archivo [léame de Livy](https://github.com/cloudera/livy/blob/master/README.rst ).

<a id="triggerlivyconf"></a>**Cómo activar la configuración de Livy**

Método 1  
1. En la barra de menús, vaya a **Archivo** > **Preferencias** > **Configuración**.  
2. En el cuadro de texto **Configuración de la búsqueda** escriba **HDInsight Job Sumission: Livy Conf**. (Envío de trabajo de HDInsight: Livy Conf).  
3. Seleccione **Edit in settings.json** (Editar en settings.json) para el resultado de búsqueda pertinente.

Método 2   
Envíe un archivo; observe que la carpeta .vscode se agrega automáticamente a la carpeta de trabajo. Puede encontrar la configuración de Livy si hace clic en **.vscode\settings.json**.

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
    | name | Nombre de esta sesión | string |
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

**Azure HDInsight** se ha agregado a la vista Explorador. Puede examinar y administrar el clúster directamente mediante **Azure HDInsight**.

1. [Conéctese](#connect-to-hdinsight-cluster) a su cuenta de Azure si no lo ha hecho aún.

2. En la barra de menús, vaya a **Vista** > **Explorador**.

3. En el panel izquierdo, expanda **AZURE HDINSIGHT**.  Se enumerarán en las suscripciones y los clústeres disponibles (se admiten Spark, Hadoop y HBase). 

   ![Suscripción de Azure HDInsight](./media/hdinsight-for-vscode/hdi-azure-hdinsight-subscription.png)

4. Expanda el clúster para ver el esquema de base de datos y tablas de metadatos de Hive.

   ![Clúster de Azure HDInsight](./media/hdinsight-for-vscode/hdi-azure-hdinsight-cluster.png)


## <a name="additional-features"></a>Características adicionales

HDInsight para Visual Studio Code admite las siguientes características:

- **Autocompletar de IntelliSense**. Emergen sugerencias para la palabra clave, los métodos, las variables, etc. Los distintos iconos representan diferentes tipos de objetos.

    ![Tipos de objetos de IntelliSense de Herramientas de HDInsight para Visual Studio Code](./media/hdinsight-for-vscode/hdinsight-for-vscode-auto-complete-objects.png)
- **Marcador de error de IntelliSense**. El servicio de lenguaje subraya los errores de edición del script de Hive.     
- **Aspectos destacados de la sintaxis**. El servicio de lenguaje usa colores diferentes para diferenciar las variables, palabras clave, tipos de datos, funciones, etc. 

    ![Aspectos destacados de la sintaxis de Herramientas de HDInsight para Visual Studio Code](./media/hdinsight-for-vscode/hdinsight-for-vscode-syntax-highlights.png)


## <a name="unlink-cluster"></a>Desvinculación del clúster

1. En la barra de menús, vaya a **Vista** > **Paleta de comandos...** y escriba **HDInsight: Unlink a Cluster** (HDInsight: desvincular un clúster).  

2. Seleccione el clúster que desea desvincular.  

3. Revise la vista **SALIDA** para comprobarlo todo.  


## <a name="logout"></a>Logout  

En la barra de menús, vaya a **Vista** > **Paleta de comandos...** y escriba **HDInsight: Logout** (HDInsight: cierre de sesión).  Habrá un elemento emergente en la esquina inferior derecha que indica **Logout successfully!** (La sesión se cerró correctamente).


## <a name="next-steps"></a>Pasos siguientes
Para ver un vídeo de demostración del uso de HDInsight para Visual Studio Code, consulte [HDInsight for Visual Studio Code](https://go.microsoft.com/fwlink/?linkid=858706) (HDInsight para Visual Studio Code).
