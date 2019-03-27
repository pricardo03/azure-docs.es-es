---
title: 'Conexión a Apache Hadoop mediante Herramientas de Data Lake para Visual Studio: Azure HDInsight'
description: Aprenda a instalar y utilizar Herramientas de Data Lake para Visual Studio para conectarse a clústeres de Apache Hadoop en Azure HDInsight y luego ejecutar consultas de Hive.
keywords: herramientas hadoop, consulta hive, visual studio, visual studio hadoop
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017,seodec18
ms.topic: conceptual
ms.date: 02/21/2019
ms.openlocfilehash: cf392bb254e38b2e07a92e87927b12e144b26f16
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "58447472"
---
# <a name="use-data-lake-tools-for-visual-studio-to-connect-to-azure-hdinsight-and-run-apache-hive-queries"></a>Conectarse a Azure HDInsight y ejecutar consultas de Apache Hive con Herramientas de Data Lake para Visual Studio

Aprenda a usar [Microsoft Azure Data Lake y herramientas de Stream Analytics para Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504) clústeres (también denominada herramientas de Data Lake) para conectarse a Apache Hadoop en [HDInsight de Azure](../hdinsight-hadoop-introduction.md) y enviar consultas de Hive.  

Para más información sobre el uso de HDInsight, consulte [Introducción a HDInsight](../hdinsight-hadoop-introduction.md) y [Primeros pasos con HDInsight](apache-hadoop-linux-tutorial-get-started.md).  

Para más información acerca de cómo conectarse a un clúster de Apache Storm, consulte [Desarrollo de topologías de C# para Apache Storm en HDInsight mediante Visual Studio](../storm/apache-storm-develop-csharp-visual-studio-topology.md).

Data Lake Tools para Visual Studio se puede utilizar para acceder a Azure Data Lake Analytics y a HDInsight. Para más información acerca de Data Lake Tools, consulte [Desarrollo de scripts U-SQL mediante Data Lake Tools para Visual Studio](../../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md).

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial y usar Data Lake Tools para Visual Studio, se necesitan los siguientes elementos:

* Un clúster de HDInsight de Azure. Para crear un clúster de HDInsight, consulte [Introducción a Apache Hadoop en Azure HDInsight](apache-hadoop-linux-tutorial-get-started.md). Para ejecutar consultas interactivas de Apache Hive, necesita un clúster de [HDInsight Interactive Query](../interactive-query/apache-interactive-query-get-started.md).  

* [Visual Studio](https://visualstudio.microsoft.com/downloads/) (2013 o versiones posteriores).  El [edición Visual Studio Community](https://visualstudio.microsoft.com/vs/community/) es gratuito.  Consulte también, [instalar Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio).

  > [!IMPORTANT]  
  > Ya no se admite herramientas de Data Lake para Visual Studio 2013. 

## <a name="install-data-lake-tools-for-visual-studio"></a>Instalación de Data Lake Tools para Visual Studio  
<a name="install-or-update-data-lake-tools-for-visual-studio"></a>

* Visual Studio 2017  
  Durante la instalación, asegúrese de incluir al menos las cargas de trabajo **desarrollo de Azure** o **procesamiento y almacenamiento de datos**.  

  Para las instalaciones existentes, en la barra de menús, vaya a **herramientas** > **obtener herramientas y características...**  para abrir el instalador de Visual Studio.  A continuación, seleccione al menos las cargas de trabajo **desarrollo de Azure** o **procesamiento y almacenamiento de datos**.

  ![Captura de pantalla del instalador de Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/vs2017_installation.png)

* Visual Studio 2013 y 2015  
  [Descargar Data Lake Tools](https://www.microsoft.com/download/details.aspx?id=49504). Elija la versión de Data Lake Tools que coincida con su versión de Visual Studio.  

> [!NOTE]  
> Actualmente, solo está disponible la versión en inglés de Data Lake Tools para Visual Studio.

## <a name="update-data-lake-tools-for-visual-studio"></a>Actualización de Data Lake Tools para Visual Studio  

1. Abra Visual Studio.

2. En la barra de menús, vaya a **herramientas** > **extensiones y actualizaciones...** .

3. Desde el **extensiones y actualizaciones** ventana, expanda **actualizaciones** a la izquierda.

4. Si hay alguna actualización disponible, **Azure Data Lake y herramientas de análisis de Stream** aparecerá en la ventana principal.  Seleccione **Actualizar**.

> [!NOTE]  
> Puede utilizar solo Data Lake Tools, versión 2.3.0.0 o posterior, para conectarse a clústeres de Interactive Query y ejecutar consultas de Hive interactivas.

## <a name="connect-to-azure-subscriptions"></a>Conexión a suscripciones de Azure
Data Lake Tools para Visual Studio se puede utilizar para conectarse a clústeres de HDInsight, realizar algunas operaciones básicas de administración y ejecutar consultas de Hive.

> [!NOTE]  
> Para más información acerca de cómo conectarse a un clúster de Hadoop genérico, consulte [How to write and submit Hive queries using Visual Studio](https://blogs.msdn.com/b/xiaoyong/archive/2015/05/04/how-to-write-and-submit-hive-queries-using-visual-studio.aspx) (Escritura y envío de consultas de Hive mediante Visual Studio).

Para conectarse a su suscripción de Azure:

1. Abra Visual Studio.

2. En la barra de menús, vaya a **vista** > **Explorador de servidores**.

3. Desde el Explorador de servidores, haga clic en **Azure**, seleccione **conectar a la suscripción de Microsoft Azure...** y complete el proceso de inicio de sesión.

4. Desde el Explorador de servidores, aparece una lista de los clústeres de HDInsight existentes. Si no tiene ningún clúster, puede crear uno mediante Azure Portal, Azure PowerShell o el SDK de HDInsight. Para más información, consulte [Creación de clústeres de Hadoop en HDInsight](../hdinsight-hadoop-provision-linux-clusters.md).

   ![Captura de pantalla de la lista de clústeres de Data Lake Tools para Visual Studio en el Explorador de servidores](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.server.explorer.png "Lista de clústeres de Data Lake Tools para Visual Studio en el Explorador de servidores")

5. Expanda un clúster de HDInsight. **Bases de datos de Hive**, aparecerán una cuenta de almacenamiento predeterminada, cuentas de almacenamiento vinculadas y un **registro del servicio Hadoop**. Puede expandir las entidades.

Una vez conectado a su suscripción de Azure, puede realizar las siguientes tareas.

Para conectarse a Azure Portal desde Visual Studio:

1. En el Explorador de servidores, vaya a **Azure** > **HDInsight** y seleccione el clúster.

2. Haga clic en un clúster de HDInsight y seleccione **administrar el clúster en Azure portal**.

Para formular preguntas o proporcionar comentarios desde Visual Studio:

1. En el Explorador de servidores, vaya a **Azure** > **HDInsight**.

2. Haga clic en **HDInsight** y seleccione **foro de MSDN** para formular preguntas, o **enviar comentarios** proporcionar comentarios.

## <a name="explore-linked-resources"></a>Exploración de recursos vinculados
Desde el Explorador de servidores, podrá ver la cuenta de almacenamiento predeterminada y las cuentas de almacenamiento vinculadas. Expanda la cuenta de almacenamiento predeterminada para ver los contenedores en la cuenta de almacenamiento. Se marcarán la cuenta de almacenamiento predeterminada y el contenedor predeterminado. Haga clic con el botón derecho en cualquiera de los contenedores para ver su contenido.

![Captura de pantalla de recursos vinculados de lista de Data Lake Tools para Visual Studio en el Explorador de servidores](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.linked.resources.png "Recursos vinculados de lista")

Después de abrir un contenedor, puede usar los siguientes botones para cargar, eliminar y descargar blobs:

![Captura de pantalla de operaciones de blob de Data Lake Tools para Visual Studio en el Explorador de servidores](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.blob.operations.png "Carga, eliminación y descarga de blobs en el Explorador de servidores")

## <a name="run-interactive-apache-hive-queries"></a>Ejecución de consultas interactivas de Apache Hive
[Apache Hive](https://hive.apache.org) es una infraestructura de almacenamiento de datos que se basa en Hadoop. Hive se utiliza para el análisis, las consultas y resumen de los datos. Data Lake Tools para Visual Studio se puede usar para ejecutar consultas de Hive desde Visual Studio. Para más información acerca de Hive, consulte [Uso de Apache Hive con HDInsight](hdinsight-use-hive.md).

[Interactive Query](../interactive-query/apache-interactive-query-get-started.md) usa [Hive en LLAP](https://cwiki.apache.org/confluence/display/Hive/LLAP) en Apache Hive 2.1. Interactive Query aporta la interactividad a las complejas consultas de estilo de almacenamiento de datos en grandes conjuntos de datos almacenados. La ejecución de consultas de Hive en Interactive Query es mucho más rápida en comparación con los trabajos por lotes tradicionales de Hive. Para obtener más información, vea ejecutar Apache Hive trabajos por lotes.

> [!NOTE]  
> Solo se pueden ejecutar consultas de Hive interactivas al conectarse a un clúster de [HDInsight Interactive Query](../interactive-query/apache-interactive-query-get-started.md).

También se puede utilizar Data Lake Tools para Visual Studio para ver el contenido de un trabajo de Hive. Data Lake Tools para Visual Studio recopila y muestra los registros Yarn de determinados trabajos de Hive.

En el Explorador de servidores, vaya a **Azure** > **HDInsight** y seleccione el clúster.  Este será el punto inicial en el Explorador de servidores para las secciones que siguen.

### <a name="view-hivesampletable"></a>Vista hivesampletable
Todos los clústeres de HDInsight tienen una tabla de Hive de ejemplo predeterminada denominada `hivesampletable`.  

En el clúster, vaya a **bases de datos de Hive** > **predeterminada** > **hivesampletable**.

* Para ver `hivesampletable` esquema:  
Expanda **hivesampletable**.

* Para ver `hivesampletable` datos:  
Haga clic en **hivesampletable**y seleccione **ver las 100 primeras filas**.  Esto equivale a ejecutar la siguiente consulta de Hive mediante el uso del controlador ODBC de Hive:

   `SELECT * FROM hivesampletable LIMIT 100`

  Puede personalizar el recuento de filas.

  ![Captura de pantalla de una consulta de Hive de HDInsight para Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.hive.schema.png "Resultados de la consulta de Hive")

### <a name="create-hive-tables"></a>Crear tablas de Hive
Para crear una tabla de Hive, puede usar la GUI o las consultas de Hive. Para obtener información acerca del uso de consultas de Hive, consulte [Ejecución de consultas de Apache Hive](#run.queries).

1. En el clúster, vaya a **bases de datos de Hive** > **predeterminada**.

2. Haga clic en **predeterminada**y seleccione **Create Table**.

3. Configurar la tabla según sea necesario.  

4. Seleccione **Crear tabla** para enviar el trabajo para crear la nueva tabla de Hive.

    ![Captura de la ventana Crear tabla de Visual Studio Tools en HDInsight](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.create.hive.table.png "Creación de tablas de Hive")

### <a name="run.queries"></a>Crear y ejecutar consultas de Hive
Para crear y ejecutar consultas de Hive, tiene dos opciones:

* Creación de consultas ad hoc
* Crear una aplicación de Hive

Para crear y ejecutar consultas ad hoc:

1. Haga clic en el clúster donde desea ejecutar la consulta y seleccione **escribir una consulta de Hive**.  

2. Escriba las consultas de Hive.  

    El editor de Hive es compatible con IntelliSense. Data Lake Tools para Visual Studio es compatible con la carga de metadatos remotos cuando se edita un script de Hive. Por ejemplo, si escribe `SELECT * FROM`, IntelliSense muestra todos los nombres de tabla sugeridos. Cuando se especifica un nombre de tabla, IntelliSense enumera los nombres de columna. Las herramientas admiten casi todas las instrucciones DML de Hive, subconsultas y UDF integradas.

    ![Captura de pantalla del ejemplo 1 de Visual Studio Tools en HDInsight: IntelliSense](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.intellisense.table.names.png "IntelliSense en U-SQL")

    ![Captura de pantalla del ejemplo 2 de Visual Studio Tools en HDInsight: IntelliSense](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.intellisense.column.names.png "IntelliSense en U-SQL")

   > [!NOTE]  
   > IntelliSense solo sugiere los metadatos del clúster que se seleccionan en la barra de herramientas de HDInsight.

3. Elija el modo de ejecución:

    * **Interactivo**  

      Asegúrese de **interactivo** está seleccionada y, a continuación, seleccione **Execute**.

      ![Captura de pantalla de consulta y ejecutar](./media/apache-hadoop-visual-studio-tools-get-started/execute.png)  

    * **Batch**  

      Asegúrese de **Batch** está seleccionada y, a continuación, seleccione **enviar**.  Si selecciona la opción de envío avanzado, configure **nombre del trabajo**, **argumentos**, **configuraciones adicionales**, y **estado de directorio**para la secuencia de comandos.

      ![Captura de pantalla de consulta y lote](./media/apache-hadoop-visual-studio-tools-get-started/batch.png)  

      ![Captura de pantalla de consulta de Hive para Hadoop en HDInsight](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.submit.jobs.advanced.png "Envío de consultas")

      > [!NOTE]  
      > No se puede enviar lotes para clústeres de Interactive Query.  Debe usar el modo interactivo.

Para crear y ejecutar una solución de Hive:

1. En la barra de menús, vaya a **archivo** > **New** > **proyecto...** .

2. En el panel izquierdo, vaya a **instalado** > **Azure Data Lake** > **HIVE (HDInsight)**.  

3. En el panel del medio, seleccione **Aplicación Hive**. Escriba las propiedades y seleccione **Aceptar**.

    ![Captura de un proyecto de Hive nuevo de Visual Studio Tools en HDInsight](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.new.hive.project.png "Creación de aplicaciones de Hive desde Visual Studio")

4. En el **Explorador de soluciones**, haga doble clic en **Script.hql** para abrir el script.

### <a name="view-job-summary-and-output"></a>Ver el resumen de trabajo y de salida

El resumen del trabajo varía ligeramente entre **Batch** y **interactivo** modo.

![Resumen del trabajo](./media/apache-hadoop-visual-studio-tools-get-started/jobSummary.png "resumen del trabajo de Hive")

Use la **actualizar** botón para actualizar el estado hasta que el estado del trabajo cambie a **finalizado**.  

* Para obtener los detalles del trabajo de **Batch** modo, seleccione los vínculos en la parte inferior para ver **trabajo consulta**, **salida del trabajo**, **registro de trabajo**, o **Registro yarn**.

* Para obtener los detalles del trabajo de **interactivo** modo, vea fichas **salida** y **salida de HiveServer2**.

  ![Detalles del trabajo](./media/apache-hadoop-visual-studio-tools-get-started/tabs.png "detalles del trabajo de Hive")

### <a name="view-job-graph"></a>Gráfico del trabajo de vista

Actualmente, solo se muestran gráficos de trabajo para trabajos de Hive que usan Tez como motor de ejecución.  Para obtener información acerca de la habilitación de Tez, consulte [¿Qué son Apache Hive y HiveQL en Azure HDInsight?](hdinsight-use-hive.md).  Consulte también, [Use Apache Tez en lugar de MapReduce](../hdinsight-hadoop-optimize-hive-query.md#use-apache-tez-instead-of-map-reduce).  

Para ver todos los operadores que hay en el vértice, haga doble clic en los vértices del gráfico del trabajo. También puede señalar a un operador concreto para ver más detalles de él.

El gráfico del trabajo no puede aparecer incluso si se especifica Tez como motor de ejecución si no se inicia ninguna aplicación de Tez.  Esto puede ocurrir porque el trabajo no contiene las instrucciones DML, o las instrucciones DML pueden ser devueltos sin iniciar una aplicación de Tez. Por ejemplo, `SELECT * FROM table1` no iniciará la aplicación de Tez.

![Gráfico del trabajo](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.fast.path.hive.execution.png "resumen del trabajo de Hive")


### <a name="task-execution-detail"></a>Detalles de ejecución de la tarea

En el gráfico del trabajo, puede seleccionar **detalles de ejecución de tarea** para obtener información de trabajos de Hive estructurada y visualizada. También puede obtener más información del trabajo. Si se produce algún problema de rendimiento, puede usar la vista para obtener más información sobre el mismo. Por ejemplo, puede obtener información acerca de cómo funciona cada tarea e información detallada acerca de cada tarea (lectura y escritura de datos, hora de programación/inicio/fin, etc.). Esta información se puede usar para ajustar las configuraciones de trabajo o la arquitectura del sistema en función de la información visualizada.

![Captura de pantalla de la ventana Vista de ejecución de tareas de Data Lake Tools para Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.task.execution.view.png "Vista de ejecución de tareas")


### <a name="view-hive-jobs"></a>Ver trabajos de Hive
Puede ver consultas de trabajo, salidas de trabajo, registros de trabajo y registros Yarn para trabajos de Hive.

En la versión más reciente de la herramienta puede ver el contenido de los trabajos de Hive al recopilar y exponer los registros Yarn. Un registro Yarn puede ayudarle a investigar problemas de rendimiento. Para más información acerca de la forma en que HDInsight recopila registros Yarn, consulte [Acceso a registros de aplicación de YARN en HDInsight basado en Windows](../hdinsight-hadoop-access-yarn-app-logs-linux.md).

Para ver trabajos de Hive:

1. Haga clic en un clúster de HDInsight y seleccione **ver trabajos**. Aparece una lista de los trabajos de Hive que se ejecutaron en el clúster.  

2. Seleccione un trabajo. En la ventana **Resumen de trabajos de Hive**, seleccione una de las siguientes opciones:
   - **Consulta de trabajo**
   - **Salida de trabajo**
   - **Registro de trabajo**  
   - **Registro Yarn**

     ![Captura de pantalla de la ventana Ver trabajos de Hive de Visual Studio Tools en HDInsight](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.view.hive.jobs.png "Ver trabajos de Hive")


## <a name="run-apache-pig-scripts"></a>Ejecución de scripts de Apache Pig

1. En la barra de menús, vaya a **archivo** > **New** > **proyecto...** .

2. En el panel izquierdo, vaya a **instalado** > **Azure Data Lake** > **Pig (HDInsight)**.  

3. En el panel central, seleccione **Pig Application**. Escriba las propiedades y seleccione **Aceptar**.

4. En **el Explorador de soluciones**, haga doble clic en **Script.pig** para abrir la secuencia de comandos.

## <a name="feedback-and-known-issues"></a>Comentarios y problemas conocidos
* Se ha corregido un problema en el que no se muestran los resultados que comienzan por valores null. Si está bloqueado debido a este problema, póngase en contacto con el equipo de soporte técnico.
* La codificación del script HQL que crea Visual Studio depende de la configuración regional del usuario. El script no se ejecuta correctamente si se carga en un clúster como un archivo binario.

## <a name="next-steps"></a>Pasos siguientes
En este artículo, ha aprendido a usar el paquete Data Lake Tools para Visual Studio para establecer conexión a los clústeres de HDInsight desde Visual Studio. También ha aprendido a ejecutar una consulta de Hive. Para obtener más información, consulte estos artículos:

* [Ejecutar consultas de Apache Hive mediante las herramientas de Data Lake para Visual Studio](apache-hadoop-use-hive-visual-studio.md)
* [Uso de Hive para Hadoop en HDInsight](hdinsight-use-hive.md)
* [Introducción al uso de Apache Hadoop en HDInsight](apache-hadoop-linux-tutorial-get-started.md)
* [Envío de trabajos de Apache Hadoop en HDInsight](submit-apache-hadoop-jobs-programmatically.md)
* [Análisis de datos de Twitter con Apache Hadoop en HDInsight](../hdinsight-analyze-twitter-data-linux.md)

