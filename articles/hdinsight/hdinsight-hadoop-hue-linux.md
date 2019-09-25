---
title: 'Hue con Hadoop en clústeres de HDInsight basados en Linux: Azure'
description: Aprenda a instalar Hue en clústeres de HDInsight Linux y a usar tunelización para enrutar las solicitudes a Hue. Use Hue para examinar el almacenamiento y ejecutar Pig o Hive.
keywords: matiz hadoop
author: hrasheed-msft
ms.date: 12/11/2017
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.openlocfilehash: 8795d28cb36bacf4f57dba2048e30219047a9ac5
ms.sourcegitcommit: cd70273f0845cd39b435bd5978ca0df4ac4d7b2c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71098630"
---
# <a name="install-and-use-hue-on-hdinsight-hadoop-clusters"></a>Instalación y uso de Hue en clústeres de Hadoop para HDInsight

Aprenda a instalar Hue en clústeres de HDInsight Linux y a usar tunelización para enrutar las solicitudes a Hue.

## <a name="what-is-hue"></a>¿Qué es Hue?

Hue es un conjunto de aplicaciones web que se usan para interactuar con un clúster de Apache Hadoop. Puede usar Hue para examinar el almacenamiento asociado a un clúster de Hadoop (WASB, en el caso de clústeres de HDInsight), ejecutar trabajos de Hive y scripts de Pig, etc. Los siguientes componentes son compatibles con la instalación de Hue en un clúster de Hadoop para HDInsight.

* Editor Beeswax de Hive
* Apache Pig
* Administrador de la tienda de metadatos
* Apache Oozie
* FileBrowser (que se comunica con el contenedor predeterminado WASB)
* Explorador web

> [!WARNING]  
> Los componentes ofrecidos con HDInsight son totalmente compatibles. Además, el soporte técnico de Microsoft le ayudará a aislar y resolver problemas relacionados con estos componentes.
>
> Los componentes personalizados reciben soporte técnico comercialmente razonable para ayudarle a solucionar el problema. Esto podría resolver el problema o pedirle que forme parte de los canales disponibles para las tecnologías de código abierto donde se encuentra la más amplia experiencia para esa tecnología. Por ejemplo, hay diversos sitios de la comunidad que se pueden usar, como el [foro de MSDN para HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [https://stackoverflow.com](https://stackoverflow.com). Los proyectos de Apache también tienen sitios de proyecto en [https://apache.org](https://apache.org), por ejemplo: [Hadoop](https://hadoop.apache.org/).

## <a name="install-hue-using-script-actions"></a>Instalación de Hue mediante acciones de script

El script para instalar Hue en un clúster de HDInsight basado en Linux está disponible en https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh. Puede usar este script para instalar Hue en clústeres con Azure Storage Blob (WASB) o Azure Data Lake Storage como almacenamiento predeterminado.

En esta sección se proporcionan instrucciones sobre cómo usar el script durante el aprovisionamiento del clúster mediante Azure Portal.

> [!NOTE]  
> También se puede utilizar Azure PowerShell, la CLI de Azure clásica, el SDK de HDInsight para .NET o las plantillas de Azure Resource Manager para aplicar las acciones de script. También puede aplicar acciones de script a clústeres que ya se estén ejecutando. Para obtener más información, consulte [Personalización de clústeres de HDInsight mediante la acción de scripts](hdinsight-hadoop-customize-cluster-linux.md).

1. Inicie el aprovisionamiento de un clúster siguiendo los pasos que se describen en [Aprovisionamiento de clústeres de HDInsight en Linux](hdinsight-hadoop-provision-linux-clusters.md), pero no complete la operación.

   > [!NOTE]  
   > Para instalar Hue en clústeres de HDInsight, el tamaño recomendado de nodo principal es como mínimo A4 (8 núcleos, 14 gigabytes de memoria).

1. En la hoja **Configuración opcional**, seleccione **Acciones de script** y proporcione la información tal y como se muestra a continuación:

    ![Proporcionar parámetros de acción de script para Hue](./media/hdinsight-hadoop-hue-linux/hdi-hue-script-action.png "Proporcionar parámetros de acción de script para Hue")

   * **NOMBRE**: escriba un nombre descriptivo para la acción de script.
   * **URI DE SCRIPT**: https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
   * **PRINCIPAL**: active esta opción.
   * **TRABAJO**: Déjelo en blanco.
   * **ZOOKEEPER**: Déjelo en blanco.
   * **PARÁMETROS**: Déjelo en blanco.

1. En la parte inferior de **Acciones de scripts**, use el botón **Seleccionar** para guardar la configuración. Por último, use el botón **Seleccionar** situado en la parte inferior de la hoja **Configuración opcional** para guardar la información de configuración opcional.

1. Continúe aprovisionando el clúster tal como se describe en [Aprovisionamiento de clústeres de HDInsight en Linux](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="use-hue-with-hdinsight-clusters"></a>Use Hue con clústeres de HDInsight

La tunelización de SSH es la única forma de obtener acceso a Hue en el clúster cuando se está ejecutando. La tunelización a través de SSH permite al tráfico ir directamente al nodo principal del clúster en el que se ejecuta Hue. Cuando termine de aprovisionar el clúster, siga estos pasos para usar Hue en un clúster de HDInsight Linux.

> [!NOTE]  
> Se recomienda utilizar el explorador web de Firefox para seguir las instrucciones siguientes.

1. Consulte la información de [Uso de la tunelización SSH para acceder a la interfaz de usuario web de Apache Ambari, ResourceManager, JobHistory, NameNode, Oozie y otras interfaces de usuario web](hdinsight-linux-ambari-ssh-tunnel.md) para crear un túnel SSH desde el sistema cliente al clúster de HDInsight y luego configurar el explorador web para usar el túnel SSH como proxy.

2. Después de crear un túnel SSH y configurar el explorador para redirigir el tráfico mediante proxy a través de él, debe encontrar el nombre de host del nodo principal primario. Para ello, puede conectarse al clúster mediante SSH en el puerto 22. Por ejemplo, `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`, donde **USERNAME** es el nombre de usuario SSH y **CLUSTERNAME** es el nombre del clúster.

    Para más información, consulte [Uso SSH con HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

3. Una vez conectado, use el comando siguiente para obtener el nombre de dominio completo del nodo principal primario:

        hostname -f

    Devolverá un nombre similar al siguiente:

        hn0-myhdi-nfebtpfdv1nubcidphpap2eq2b.ex.internal.cloudapp.net

    Este es el nombre de host del nodo principal primario donde se encuentra el sitio web Hue.

4. Use el explorador para abrir el portal de Hue en http:\//HOSTNAME:8888. Reemplace HOSTNAME por el nombre obtenido en el paso anterior.

   > [!NOTE]  
   > Al iniciar sesión por primera vez, se le pedirá que cree una cuenta para iniciar sesión en el portal de Hue. Las credenciales que especifique aquí se limitarán al portal y no están relacionadas con las credenciales de administrador o de usuario SSH que especificó al aprovisionar el clúster.

    ![Ventana de inicio de sesión del portal de Hue de HDInsight](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-login.png "Especificar las credenciales del portal de Hue")

### <a name="run-a-hive-query"></a>Ejecución de una consulta de Hive

1. En el portal de Hue, haga clic en **Query Editors** (Editores de consultas), y, a continuación, haga clic en **Hive** para abrir el editor de Hive.

    ![Usar editor de Hive del portal de Hue de HDInsight](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-use-hive.png "Usar Hive")
2. En la pestaña **Assist** (Asistencia), en **Database** (Base de datos), debería de ver **hivesampletable**. Se trata de una tabla de ejemplo que se incluye con todos los clústeres de Hadoop en HDInsight. Escriba una consulta de ejemplo en el panel derecho y vea el resultado en la pestaña **Results** (resultados) en el panel debajo, como se muestra en la captura de pantalla.

    ![Consulta de Hive del portal de Hue de HDInsight](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-hive-query.png "Ejecutar consulta de Hive")

    También puede usar la pestaña **Chart** para ver una representación visual del resultado.

### <a name="browse-the-cluster-storage"></a>Examinar el almacenamiento del clúster

1. En el portal de Hue, haga clic en **File Browser** (explorador de archivos) en la esquina superior derecha de la barra de menús.
2. De forma predeterminada se abre el explorador de archivos en el directorio **/user/myuser** . Haga clic en la barra oblicua que se encuentra antes del directorio del usuario en la ruta de acceso para ir a la raíz del contenedor de Almacenamiento de Azure asociado con el clúster.

    ![Explorador de archivos del portal de Hue de HDInsight](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-file-browser.png "Usar el explorador de archivos")

3. Haga clic con el botón derecho en un archivo o carpeta para ver las operaciones disponibles. Use el botón **Cargar** situado en la esquina derecha para cargar archivos en el directorio actual. Use el botón **Nuevo** para crear nuevos archivos o directorios.

> [!NOTE]  
> El explorador de archivos de Hue solo puede mostrar el contenido del contenedor predeterminado asociado con el clúster de HDInsight. Cualquier cuenta o contenedor de almacenamiento adicional asociados con el clúster no serán accesibles mediante el explorador de archivos. Sin embargo, los contenedores adicionales asociados con el clúster siempre serán accesibles para los trabajos de Hive. Por ejemplo, si escribe el comando `dfs -ls wasb://newcontainer@mystore.blob.core.windows.net` en el editor de Hive, también podrá ver el contenido de los contenedores adicionales. En este comando, **newcontainer** no es el contenedor predeterminado asociado a un clúster.

## <a name="important-considerations"></a>Consideraciones importantes

1. El script que se usó para instalar Hue lo instala solo en el nodo principal primario del clúster.

2. Durante la instalación, se reinician varios servicios de Hadoop (HDFS, YARN, MR2, Oozie) para actualizar la configuración. Cuando el script finaliza la instalación de Hue, puede tardar algún tiempo hasta que otros servicios de Hadoop se inicien. Esto podría afectar inicialmente al rendimiento de Hue. Una vez que todos los servicios se inician, la funcionalidad de Hue será total.
3. Hue no entiende los trabajos Apache Tez, que es el valor predeterminado actual de Hive. Si desea usar MapReduce como el motor de ejecución de Hive, actualice el script para usar el comando siguiente en el script:

        set hive.execution.engine=mr;

4. Con los clústeres de Linux, se puede dar el caso de que los servicios se ejecutan en el nodo principal primario mientras Resource Manager se ejecuta en el secundario. Este escenario podría producir errores (que se muestra a continuación) cuando se usa Hue para ver detalles de trabajos de ejecución en el clúster. De todas formas, puede ver los detalles del trabajo una vez que el trabajo se complete.

   ![Mensaje de error de ejemplo del portal de Hue](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-error.png "Error del portal de Hue")

   Este es un problema conocido. Como alternativa, modifique Ambari para que la instancia de Resource Manager que está activa también se ejecute en el nodo principal primario.
5. Hue entiende WebHDFS, mientras que los clústeres de HDInsight usan Azure Storage mediante `wasb://`. Por lo tanto, el script personalizado que se usa con la acción de script instala WebWasb, que es un servicio compatible con WebHDFS para hablar con WASB. Así que aunque el portal de Hue dice HDFS en lugares (como cuando se mueve el mouse sobre el **Explorador de archivos**), se debe interpretar como WASB.

## <a name="next-steps"></a>Pasos siguientes

* [Instalación de Apache Giraph en clústeres de HDInsight](hdinsight-hadoop-giraph-install-linux.md). Use la personalización del clúster para instalar Giraph en clústeres de Hadoop para HDInsight. Giraph permite realizar un procesamiento gráfico con Hadoop y se puede usar con HDInsight de Azure.
* [Instalación de R en clústeres de HDInsight](hdinsight-hadoop-r-scripts-linux.md). Use la personalización del clúster para instalar R en clústeres de Hadoop para HDInsight. R es un entorno y lenguaje de código abierto para computación estadística. Proporciona cientos de de funciones estadísticas integradas y su propio lenguaje de programación que combina aspectos de la programación funcional y orientada a objetos. También proporciona amplias capacidades gráficas.

[powershell-install-configure]: install-configure-powershell-linux.md
[hdinsight-provision]: hdinsight-provision-clusters-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
