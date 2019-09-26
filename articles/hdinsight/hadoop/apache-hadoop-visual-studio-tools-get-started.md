---
title: 'Apache Hadoop y Data Lake Tools para Visual Studio: Azure HDInsight'
description: Aprenda a instalar y utilizar Herramientas de Data Lake para Visual Studio para conectarse a clústeres de Apache Hadoop en Azure HDInsight y luego ejecutar consultas de Hive.
keywords: herramientas hadoop, consulta hive, visual studio, visual studio hadoop
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017,seodec18
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: 44a076ee6979e207ac3992f76d3b89cc188d53b8
ms.sourcegitcommit: 8ef0a2ddaece5e7b2ac678a73b605b2073b76e88
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2019
ms.locfileid: "71076289"
---
# <a name="use-data-lake-tools-for-visual-studio-to-connect-to-azure-hdinsight-and-run-apache-hive-queries"></a>Conectarse a Azure HDInsight y ejecutar consultas de Apache Hive con Herramientas de Data Lake para Visual Studio

Aprenda a usar [Herramientas de Microsoft Azure Data Lake y Stream Analytics para Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504) (también conocido como Herramientas de Data Lake) para conectarse a clústeres de Apache Hadoop en [Azure HDInsight](../hdinsight-hadoop-introduction.md) y enviar consultas de Hive.  

Para más información sobre el uso de HDInsight, consulte [Introducción a HDInsight](../hdinsight-hadoop-introduction.md) y [Primeros pasos con HDInsight](apache-hadoop-linux-tutorial-get-started.md).  

Para más información acerca de cómo conectarse a un clúster de Apache Storm, consulte [Desarrollo de topologías de C# para Apache Storm en HDInsight mediante Visual Studio](../storm/apache-storm-develop-csharp-visual-studio-topology.md).

Data Lake Tools para Visual Studio se puede utilizar para acceder a Azure Data Lake Analytics y a HDInsight. Para más información acerca de Data Lake Tools, consulte [Desarrollo de scripts U-SQL mediante Data Lake Tools para Visual Studio](../../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md).

## <a name="prerequisites"></a>Requisitos previos

Para completar este artículo y usar Herramientas de Data Lake para Visual Studio, se necesitan los siguientes elementos:

* Un clúster de HDInsight de Azure. Para crear un clúster de HDInsight, consulte [Introducción a Apache Hadoop en Azure HDInsight](apache-hadoop-linux-tutorial-get-started.md). Para ejecutar consultas interactivas de Apache Hive, necesita un clúster de [HDInsight Interactive Query](../interactive-query/apache-interactive-query-get-started.md).  

* [Visual Studio](https://visualstudio.microsoft.com/downloads/) (2013 o más reciente).  La edición [Visual Studio Community](https://visualstudio.microsoft.com/vs/community/) es gratuita.  Vea también [Instalar Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) y [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/). Existen pequeñas variaciones de interfaz con Visual Studio de 2019.

  > [!IMPORTANT]  
  > Visual Studio 2013 ya no admite Herramientas de Data Lake.

## <a name="install-data-lake-tools-for-visual-studio"></a>Instalación de Data Lake Tools para Visual Studio  
<a name="install-or-update-data-lake-tools-for-visual-studio"></a>

* Visual Studio 2017 o Visual Studio 2019  
  Durante la instalación, asegúrese de incluir al menos las cargas de trabajo **Desarrollo de Azure** o **Almacenamiento y procesamiento de datos**.  

  En las instalaciones existentes, en la barra de menús, vaya a **Herramientas** > **Obtener herramientas y características...** para abrir el Instalador de Visual Studio.  Luego, seleccione como mínimo las cargas de trabajo **Desarrollo de Azure** o **Almacenamiento y procesamiento de datos**.

  ![Captura de pantalla del Instalador de Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/vs-2017-installation.png)

* Visual Studio 2013 y 2015  
  [Descargue Herramientas de Data Lake](https://www.microsoft.com/download/details.aspx?id=49504). Elija la versión de Data Lake Tools que coincida con su versión de Visual Studio.  

> [!NOTE]  
> Actualmente, solo está disponible la versión en inglés de Data Lake Tools para Visual Studio.

## <a name="update-data-lake-tools-for-visual-studio"></a>Actualización de Herramientas de Data Lake para Visual Studio  

1. Abra Visual Studio.

2. En la barra de menús, vaya a **Herramientas** > **Extensiones y actualizaciones...**

3. En la ventana **Extensiones y actualizaciones**, expanda **Actualizaciones** a la izquierda.

4. Si hay alguna actualización disponible, se mostrará **Herramientas de Azure Data Lake y Stream Analytics** en la ventana principal.  Seleccione **Actualizar**.

> [!NOTE]  
> Puede utilizar solo Data Lake Tools, versión 2.3.0.0 o posterior, para conectarse a clústeres de Interactive Query y ejecutar consultas de Hive interactivas.

## <a name="connect-to-azure-subscriptions"></a>Conexión a suscripciones de Azure
Data Lake Tools para Visual Studio se puede utilizar para conectarse a clústeres de HDInsight, realizar algunas operaciones básicas de administración y ejecutar consultas de Hive.

> [!NOTE]  
> Para más información acerca de cómo conectarse a un clúster de Hadoop genérico, consulte [How to write and submit Hive queries using Visual Studio](https://blogs.msdn.com/b/xiaoyong/archive/2015/05/04/how-to-write-and-submit-hive-queries-using-visual-studio.aspx) (Escritura y envío de consultas de Hive mediante Visual Studio).

Para conectarse a su suscripción de Azure:

1. Abra Visual Studio.

2. En la barra de menús, vaya a **Vista** > **Explorador de servidores**.

3. En el Explorador de servidores, haga clic con el botón derecho en **Azure**, seleccione **Conectar a la suscripción de Microsoft Azure...** y complete el proceso de inicio de sesión.

4. En el Explorador de servidores, aparece una lista de los clústeres de HDInsight existentes. Si no tiene ningún clúster, puede crear uno mediante Azure Portal, Azure PowerShell o el SDK de HDInsight. Para más información, consulte [Creación de clústeres de Hadoop en HDInsight](../hdinsight-hadoop-provision-linux-clusters.md).

   ![Lista de clústeres de Data Lake Tools para Visual Studio en el Explorador de servidores](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-server-explorer.png "Lista de clústeres de Data Lake Tools para Visual Studio en el Explorador de servidores")

5. Expanda un clúster de HDInsight. Se mostrarán **Bases de datos de Hive**, una cuenta de almacenamiento predeterminada, cuentas de almacenamiento vinculadas y un **registro del servicio Hadoop**. Puede expandir las entidades.

Una vez conectado a su suscripción de Azure, puede realizar las siguientes tareas.

Para conectarse a Azure Portal desde Visual Studio:

1. En el Explorador de servidores, vaya a **Azure** > **HDInsight** y seleccione el clúster.

2. Haga clic con el botón derecho en un clúster de HDInsight y seleccione **Administrar clústeres en Azure Portal**.

Para hacer preguntas y realizar comentarios desde Visual Studio:

1. En el Explorador de servidores, vaya a **Azure** > **HDInsight**.

2. Haga clic con el botón derecho en **HDInsight** y seleccione **Foro de MSDN** para formular preguntas, o bien **Enviar comentarios** para aportar sus comentarios.

## <a name="link-a-cluster"></a>Vinculación de un clúster
Para vincular un clúster, haga clic con el botón derecho en **HDInsight** y seleccione **Vincular un clúster de HDInsight**. Indique la **dirección URL de conexión**, el **nombre de usuario** y la **contraseña**, haga clic en **Siguiente** y, luego, en **Finalizar**. El clúster debería aparecer bajo el nodo de HDInsight.

![Captura de pantalla del cuadro de diálogo de vinculación de clúster de Herramientas de Data Lake para Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-link-cluster-dialog.png)

Haga clic con el botón derecho en el clúster vinculado y seleccione **Editar**. El usuario puede actualizar la información del clúster. Actualmente, la adición de un clúster de HDInsight solo es compatible con Hive.

![Captura de pantalla de actualización de clúster de Herramientas de Data Lake para Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-link-cluster-update.png)

## <a name="explore-linked-resources"></a>Exploración de recursos vinculados
Desde el Explorador de servidores, podrá ver la cuenta de almacenamiento predeterminada y las cuentas de almacenamiento vinculadas. Expanda la cuenta de almacenamiento predeterminada para ver los contenedores en la cuenta de almacenamiento. Se marcarán la cuenta de almacenamiento predeterminada y el contenedor predeterminado. Haga clic con el botón derecho en cualquiera de los contenedores para ver su contenido.

![Recursos vinculados de Data Lake Tools para Visual Studio en el Explorador de servidores](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-linked-resources.png "Recursos vinculados de lista")

Después de abrir un contenedor, puede usar los siguientes botones para cargar, eliminar y descargar blobs:

![Operaciones de blob de Data Lake Tools para Visual Studio en el Explorador de servidores](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-blob-operations.png "Carga, eliminación y descarga de blobs en el Explorador de servidores")

## <a name="run-interactive-apache-hive-queries"></a>Ejecución de consultas interactivas de Apache Hive
[Apache Hive](https://hive.apache.org) es una infraestructura de almacenamiento de datos que se basa en Hadoop. Hive se utiliza para el análisis, las consultas y resumen de los datos. Data Lake Tools para Visual Studio se puede usar para ejecutar consultas de Hive desde Visual Studio. Para más información acerca de Hive, consulte [Uso de Apache Hive con HDInsight](hdinsight-use-hive.md).

[Interactive Query](../interactive-query/apache-interactive-query-get-started.md) usa [Hive en LLAP](https://cwiki.apache.org/confluence/display/Hive/LLAP) en Apache Hive 2.1. Interactive Query aporta la interactividad a las complejas consultas de estilo de almacenamiento de datos en grandes conjuntos de datos almacenados. La ejecución de consultas de Hive en Interactive Query es mucho más rápida en comparación con los trabajos por lotes tradicionales de Hive. 

> [!NOTE]  
> Solo se pueden ejecutar consultas de Hive interactivas al conectarse a un clúster de [HDInsight Interactive Query](../interactive-query/apache-interactive-query-get-started.md).

También se puede utilizar Data Lake Tools para Visual Studio para ver el contenido de un trabajo de Hive. Data Lake Tools para Visual Studio recopila y muestra los registros Yarn de determinados trabajos de Hive.

En el Explorador de servidores, vaya a **Azure** > **HDInsight** y seleccione el clúster.  Este será el punto de partida en el Explorador de servidores en las secciones que siguen.

### <a name="view-hivesampletable"></a>Ver hivesampletable
Todos los clústeres de HDInsight tienen una tabla de Hive de ejemplo denominada `hivesampletable`.  

En el clúster, vaya a **Bases de datos de Hive** > **predeterminado** > **hivesampletable**.

* Para ver el esquema de `hivesampletable`:  
Expanda **hivesampletable**.

* Para ver datos de `hivesampletable`:  
Haga clic con el botón derecho en **hivesampletable** y seleccione **Ver las 100 primeras filas**.  Esto equivale a ejecutar la siguiente consulta de Hive mediante el uso del controlador ODBC de Hive:

   `SELECT * FROM hivesampletable LIMIT 100`

  Puede personalizar el recuento de filas.

  ![Captura de pantalla de una consulta de Hive de HDInsight para Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-hive-schema.png "Resultados de la consulta de Hive")

### <a name="create-hive-tables"></a>Crear tablas de Hive
Para crear una tabla de Hive, puede usar la GUI o las consultas de Hive. Para obtener información acerca del uso de consultas de Hive, consulte [Ejecución de consultas de Apache Hive](#run.queries).

1. En el clúster, vaya a **Bases de datos de Hive** > **predeterminado**.

2. Haga clic con el botón derecho en **predeterminado** y seleccione **Crear tabla**.

3. Configure la tabla como quiera.  

4. Seleccione **Crear tabla** para enviar el trabajo para crear la nueva tabla de Hive.

    ![Captura de la ventana Crear tabla de Visual Studio Tools en HDInsight](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-create-hive-table.png "Creación de tablas de Hive")

### <a name="run.queries"></a>Creación y ejecución de consultas de Hive
Para crear y ejecutar consultas de Hive, tiene dos opciones:

* Crear consultas ad hoc
* Crear una aplicación de Hive

Para crear y ejecutar consultas ad hoc:

1. Haga clic con el botón derecho en el clúster en que desee ejecutar la consulta y seleccione **Escribir una consulta de Hive**.  

2. Especifique esta consulta de Hive:

    ```sql
    SELECT devicemodel, COUNT(devicemodel) AS deviceCount
    FROM hivesampletable
    GROUP BY devicemodel
    ORDER BY devicemodel
    ```

    El editor de Hive es compatible con IntelliSense. Data Lake Tools para Visual Studio es compatible con la carga de metadatos remotos cuando se edita un script de Hive. Por ejemplo, si escribe `SELECT * FROM`, IntelliSense enumera todos los nombres de tabla sugeridos. Cuando se especifica un nombre de tabla, IntelliSense enumera los nombres de columna. Las herramientas admiten casi todas las instrucciones DML de Hive, subconsultas y UDF integradas.

    ![Captura de pantalla del ejemplo 1 de Visual Studio Tools en HDInsight: IntelliSense](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-intellisense-table-names.png "IntelliSense en U-SQL")

    ![Captura de pantalla del ejemplo 2 de Visual Studio Tools en HDInsight: IntelliSense](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-intellisense-column-names.png "IntelliSense en U-SQL")

   > [!NOTE]  
   > IntelliSense solo sugiere los metadatos del clúster que se seleccionan en la barra de herramientas de HDInsight.

3. Elija el modo de ejecución:

    * **Interactivo**  

      Asegúrese de que **Interactivo** está seleccionado y, después, seleccione **Ejecutar**.

      ![Captura de pantalla de la consulta y la opción Ejecutar](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-query-execute.png)  

    * **Batch**  

      Asegúrese de que **Lote** está seleccionado y, después, seleccione **Enviar**.  Si selecciona la opción de envío avanzado, configure las opciones **Nombre del trabajo**, **Argumentos**, **Configuraciones adicionales** y **Estado de directorio** del script.

      ![Opciones de consulta y lote de Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-query-batch.png)  

      ![Captura de pantalla de consulta de Hive para Hadoop en HDInsight](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-submit-jobs-advanced.png "Envío de consultas")

      > [!NOTE]  
      > No se puede enviar lotes a clústeres de consulta interactiva.  Hay que usar el modo interactivo.

Para crear y ejecutar una solución de Hive:

1. En la barra de menús, vaya a **Archivo** > **Nuevo** > **Proyecto...**

2. En el panel izquierdo, vaya a **Instalado** > **Azure Data Lake** > **HIVE (HDInsight)** .  

3. En el panel del medio, seleccione **Aplicación Hive**. Escriba las propiedades y seleccione **Aceptar**.

    ![Captura de un proyecto de Hive nuevo de Visual Studio Tools en HDInsight](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-new-hive-project.png "Creación de aplicaciones de Hive desde Visual Studio")

4. En el **Explorador de soluciones**, haga doble clic en **Script.hql** para abrir el script.

### <a name="view-job-summary-and-output"></a>Visualización de salida y resumen del trabajo

El resumen del trabajo varía ligeramente entre los modos por **lotes** e **interactivo**.

![Pantalla Pestaña de resumen de trabajos de Apache Hive](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-job-summary.png "Resumen de trabajos de Hive")

Utilice el botón **Actualizar** para actualizar el estado hasta que el estado del trabajo cambie a **Finalizado**.  

* Para obtener detalles del trabajo del modo por **lotes**, seleccione los vínculos de la parte inferior para ver **Consulta de trabajo**, **Salida del trabajo**, **Registro de trabajo**, o **Registro Yarn**.

* Para obtener detalles del trabajo del modo **interactivo**, vea las pestañas **Salida** y **Salida de HiveServer2**.

  ![Detalles de trabajos de Apache Hive para Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-job-details.png "Detalles de trabajos de Hive")

### <a name="view-job-graph"></a>Visualización del gráfico del trabajo

Actualmente, solo se muestran gráficos de trabajo de los trabajos de Hive que usan Tez como motor de ejecución.  Para obtener información acerca de la habilitación de Tez, consulte [¿Qué son Apache Hive y HiveQL en Azure HDInsight?](hdinsight-use-hive.md).  Vea también [Uso de Apache Tez en lugar de MapReduce](../hdinsight-hadoop-optimize-hive-query.md#use-apache-tez-instead-of-map-reduce).  

Para ver todos los operadores que hay en el vértice, haga doble clic en los vértices del gráfico del trabajo. También puede señalar a un operador concreto para ver más detalles de él.

El gráfico del trabajo puede no mostrarse, aun habiendo especificado Tez como motor de ejecución, si no hay iniciada ninguna aplicación Tez.  Esto puede ocurrir porque el trabajo no contiene instrucciones DML, o las instrucciones DML pueden volver sin iniciar una aplicación Tez. Por ejemplo, `SELECT * FROM table1` no iniciará la aplicación Tez.

![Gráfico de trabajo de Apache Hive para Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-fast-path-hive-execution.png "Resumen de trabajos de Hive")

### <a name="task-execution-detail"></a>Detalles de ejecución de la tarea

En el gráfico del trabajo, se puede seleccionar **Detalles de ejecución de la tarea** para obtener información estructurada y visualizada de los trabajos de Hive. También puede obtener más información del trabajo. Si se produce algún problema de rendimiento, puede usar la vista para obtener más información sobre el mismo. Por ejemplo, puede obtener información acerca de cómo funciona cada tarea e información detallada acerca de cada tarea (lectura y escritura de datos, hora de programación/inicio/fin, etc.). Esta información se puede usar para ajustar las configuraciones de trabajo o la arquitectura del sistema en función de la información visualizada.

![Visual Studio Tools de Data Lake: ventana Vista de ejecución de tareas](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-task-execution-view.png "Vista de ejecución de tareas")


### <a name="view-hive-jobs"></a>Ver trabajos de Hive
Puede ver consultas de trabajo, salidas de trabajo, registros de trabajo y registros Yarn para trabajos de Hive.

En la versión más reciente de la herramienta puede ver el contenido de los trabajos de Hive al recopilar y exponer los registros Yarn. Un registro Yarn puede ayudarle a investigar problemas de rendimiento. Para más información acerca de la forma en que HDInsight recopila registros Yarn, consulte [Acceso a registros de aplicación de YARN en HDInsight basado en Windows](../hdinsight-hadoop-access-yarn-app-logs.md).

Para ver trabajos de Hive:

1. Haga clic con el botón derecho en un clúster de HDInsight y seleccione **Ver trabajos**. Aparece una lista de los trabajos de Hive que se ejecutaron en el clúster.  

2. Seleccione un trabajo. En la ventana **Resumen de trabajos de Hive**, seleccione una de las siguientes opciones:
    - **Consulta de trabajo**
    - **Salida de trabajo**
    - **Registro de trabajo**  
    - **Registro Yarn**

    ![Captura de pantalla de la ventana Ver trabajos de Hive de Visual Studio Tools en HDInsight](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight-visual-studio-tools-view-hive-jobs.png "Ver trabajos de Hive")


## <a name="run-apache-pig-scripts"></a>Ejecución de scripts de Apache Pig

1. En la barra de menús, vaya a **Archivo** > **Nuevo** > **Proyecto...**

2. En el panel izquierdo, vaya a **Instalado** > **Azure Data Lake** > **Pig (HDInsight)** .  

3. En el panel central, seleccione **Aplicación Pig**. Escriba las propiedades y seleccione **Aceptar**.

4. En el **Explorador de soluciones**, haga doble clic en **Script.pig** para abrir el script.

## <a name="feedback-and-known-issues"></a>Comentarios y problemas conocidos
* Se ha corregido un problema en el que no se muestran los resultados que comienzan por valores null. Si está bloqueado debido a este problema, póngase en contacto con el equipo de soporte técnico.
* La codificación del script HQL que crea Visual Studio depende de la configuración regional del usuario. El script no se ejecuta correctamente si se carga en un clúster como un archivo binario.

## <a name="next-steps"></a>Pasos siguientes
En este artículo, ha aprendido a usar el paquete Data Lake Tools para Visual Studio para establecer conexión a los clústeres de HDInsight desde Visual Studio. También ha aprendido a ejecutar una consulta de Hive. Para obtener más información, consulte estos artículos:

* [Ejecución de las consultas de Apache Hive mediante Herramientas de Data Lake para Visual Studio](apache-hadoop-use-hive-visual-studio.md)
* [Uso de Hive para Hadoop en HDInsight](hdinsight-use-hive.md)
* [Introducción al uso de Apache Hadoop en HDInsight](apache-hadoop-linux-tutorial-get-started.md)
* [Envío de trabajos de Apache Hadoop en HDInsight](submit-apache-hadoop-jobs-programmatically.md)
* [Análisis de datos de Twitter con Apache Hadoop en HDInsight](../hdinsight-analyze-twitter-data.md)

