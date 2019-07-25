---
title: Apache Hive con herramientas Data Lake (Apache Hadoop) para Visual Studio en Azure HDInsight
description: Aprenda a usar las herramientas de Data Lake para Visual Studio a fin de ejecutar consultas de Apache Hive con Apache Hadoop en Azure HDInsight.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/14/2019
ms.author: hrasheed
ms.openlocfilehash: 7480dafe435e555bfba81ebd9242bb5724c0bf3f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65861592"
---
# <a name="run-apache-hive-queries-using-the-data-lake-tools-for-visual-studio"></a>Ejecución de las consultas de Apache Hive mediante las herramientas de Data Lake para Visual Studio

Aprenda a usar las herramientas de Data Lake para Visual Studio a fin de realizar consultas en Apache Hive. Las herramientas de Data Lake permiten crear, enviar y supervisar fácilmente consultas de Hive en Apache Hadoop en Azure HDInsight.

## <a id="prereq"></a>Requisitos previos

* Un clúster de Apache Hadoop en HDInsight. Consulte [Introducción a HDInsight en Linux](./apache-hadoop-linux-tutorial-get-started.md).

* Visual Studio (una de las siguientes versiones):

    * Visual Studio 2015, 2017 (cualquier edición)
    * Visual Studio 2013 Community/Professional/Premium/Ultimate con la actualización 4

* Herramientas de HDInsight para Visual Studio o Azure Data Lake Tools for Visual Studio. Consulte [Introducción al uso de herramientas de Hadoop en Visual Studio para HDInsight](apache-hadoop-visual-studio-tools-get-started.md) para ejecutar una consulta de Hive para conocer los pasos que le permitirán instalar y configurar las herramientas.

## <a id="run"></a> Ejecución de las consultas de Apache Hive usando Visual Studio

Para crear y ejecutar consultas de Hive, tiene dos opciones:

* Creación de consultas ad hoc
* Crear una aplicación de Hive

### <a name="ad-hoc"></a>Ad hoc

Las consultas ad hoc se pueden ejecutar en el modo **Lotes** o **Interactivo**.

1. Abra **Visual Studio**.

2. En el **Explorador de servidores**, vaya a **Azure** > **HDInsight**.

3. Expanda **HDInsight**, haga clic con el botón derecho en el clúster donde quiere ejecutar la consulta y luego seleccione **Escribir una consulta de Hive**.

4. Especifique esta consulta de Hive:

    ```hql
    SELECT * FROM hivesampletable;
    ```

5. Seleccione **Execute**(Ejecutar). Tenga en cuenta que el modo de ejecución está predeterminado en **Interactivo**.

    ![Captura de pantalla de ejecución de consultas interactivas de Hive](./media/apache-hadoop-use-hive-visual-studio/vs-execute-hive-query.png)

6. Para ejecutar la misma consulta en el modo **Lotes**, cambie la lista desplegable de **Interactivo** a **Lotes**. Tenga en cuenta que el botón de ejecución cambia de **Ejecutar** a **Enviar**.

    ![Captura de pantalla del envío de una consulta de Hive](./media/apache-hadoop-use-hive-visual-studio/vs-batch-query.png)

    El editor de Hive es compatible con IntelliSense. Data Lake Tools para Visual Studio es compatible con la carga de metadatos remotos cuando se edita un script de Hive. Por ejemplo, si escribe `SELECT * FROM`, IntelliSense enumera todos los nombres de tabla sugeridos. Cuando se especifica un nombre de tabla, IntelliSense enumera los nombres de columna. Las herramientas admiten casi todas las instrucciones DML de Hive, subconsultas y UDF integradas. IntelliSense solo sugiere los metadatos del clúster que se seleccionan en la barra de herramientas de HDInsight.

    ![Captura de pantalla del ejemplo 1 de Visual Studio Tools en HDInsight: IntelliSense](./media/apache-hadoop-use-hive-visual-studio/vs-intellisense-table-name.png "IntelliSense en U-SQL")
   
    ![Captura de pantalla del ejemplo 2 de Visual Studio Tools en HDInsight: IntelliSense](./media/apache-hadoop-use-hive-visual-studio/vs-intellisense-column-name.png "IntelliSense en U-SQL")

7. Seleccione **Enviar** o **Enviar (avanzado)** .

   Si selecciona la opción de envío avanzado, configure las opciones **Nombre del trabajo**, **argumentos**, **Configuraciones adicionales** y **Estado de directorio** del script:

    ![Captura de pantalla de consulta de Hive para Hadoop en HDInsight](./media/apache-hadoop-use-hive-visual-studio/hdinsight.visual.studio.tools.submit.jobs.advanced.png "Envío de consultas")

### <a name="hive-application"></a>Aplicación Hive

1. Abra **Visual Studio**.

2. En la barra de menús, vaya a **Archivo** > **Nuevo** > **Proyecto**.

3. En la ventana **Proyecto nuevo**, vaya a **Plantillas** > **Azure Data Lake** > **HIVE (HDInsight)**  > **Aplicación Hive**. 

4. Escriba un Nombre para el proyecto y, a continuación, seleccione **Aceptar**.

5. Abra el archivo **Script.hql** creado con este proyecto y péguelo en las siguientes instrucciones de HiveQL:

    ```hiveql
    set hive.execution.engine=tez;
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND  INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;
    ```

    Estas instrucciones realizan las acciones siguientes:

   * `DROP TABLE`: si la tabla existe, esta instrucción la elimina.

   * `CREATE EXTERNAL TABLE`: crea una nueva tabla "externa" en Hive. Las tablas externas solo almacenan la definición de tabla en Hive; los datos quedan en la ubicación original.

     > [!NOTE]  
     > Las tablas externas se deben utilizar cuando se espera que un origen externo actualice los datos subyacentes. Por ejemplo, un trabajo de MapReduce o un servicio de Azure.
     >
     > La eliminación de una tabla externa **no** elimina los datos, solamente la definición de tabla.

   * `ROW FORMAT`: indica cómo se da formato a los datos de Hive. En este caso, los campos de cada registro se separan mediante un espacio.

   * `STORED AS TEXTFILE LOCATION`: indica a Hive que los datos se almacenan como texto en el directorio example/data.

   * `SELECT`: seleccione un número de todas las filas donde la columna `t4` contiene el valor `[ERROR]`. Esta instrucción devuelve un valor de `3` porque hay tres filas que contienen este valor.

   * `INPUT__FILE__NAME LIKE '%.log'`: Indica a Hive que solo deberíamos devolver datos de archivos que terminan en .log. Esta cláusula limita la búsqueda al archivo sample.log que contiene los datos.

6. En la barra de herramientas, seleccione el **clúster de HDInsight** que desea usar para esta consulta. Seleccione **Enviar** para ejecutar las instrucciones como un trabajo de Hive.

   ![Barra Enviar](./media/apache-hadoop-use-hive-visual-studio/toolbar.png)

7. El **resumen del trabajo de Hive** aparecerá y mostrará información sobre el trabajo en ejecución. Use el vínculo **Actualizar** para actualizar la información del trabajo, hasta que el **estado del trabajo** cambie a **Completado**.

   ![resumen de trabajos que muestra un trabajo completado](./media/apache-hadoop-use-hive-visual-studio/jobsummary.png)

8. Utilice el vínculo **Salida de trabajo** para ver el resultado de este trabajo. Muestra `[ERROR] 3`, que es el valor que devuelve esta consulta.

### <a name="additional-example"></a>Ejemplo adicional

Este ejemplo se basa en la tabla `log4jLogs` creada en el paso anterior.

1. En el **Explorador de servidores**, haga clic con el botón derecho en el clúster y seleccione **Escribir una consulta de Hive**.

2. Especifique esta consulta de Hive:

    ```hql
    set hive.execution.engine=tez;
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
    ```

    Estas instrucciones realizan las acciones siguientes:

    * `CREATE TABLE IF NOT EXISTS`: crea una tabla, si todavía no existe. Dado que no se utiliza la palabra clave `EXTERNAL`, esta instrucción crea una tabla interna. Las tablas internas se guardan en el almacenamiento de datos de Hive y Hive las administra.
    
    > [!NOTE]  
    > A diferencia de las tablas `EXTERNAL`, la eliminación de una tabla interna también eliminará los datos subyacentes.

    * `STORED AS ORC`: almacena los datos en el formato de columnas de filas optimizadas (ORC, Optimized Row Columnar). ORC es un formato altamente optimizado y eficiente para almacenar datos de Hive.
    
    * `INSERT OVERWRITE ... SELECT`: selecciona filas de la tabla `log4jLogs` que contienen `[ERROR]` y luego inserta los datos en la tabla `errorLogs`.

3. Ejecute la consulta en modo **Lotes**.

4. Para comprobar que el trabajo ha creado la tabla, utilice el **Explorador de servidores** y expanda **Azure** > **HDInsight** > el clúster de HDInsight > **Bases de datos de Hive** > **predeterminado**. La tabla **errorLogs** y la tabla **log4jLogs** aparecen en la lista.

## <a id="nextsteps"></a>Pasos siguientes

Como puede ver, las herramientas de HDInsight para Visual Studio proporcionan una manera sencilla de trabajar con consultas de Hive en HDInsight.

Para obtener información general acerca de Hive en HDInsight:

* [Uso de Apache Hive con Apache Hadoop en HDInsight](hdinsight-use-hive.md)

Para obtener información sobre otras maneras de trabajar con Hadoop en HDInsight:

* [Uso de Apache Pig con Apache Hadoop en HDInsight](hdinsight-use-pig.md)

* [Uso de MapReduce con Apache Hadoop en HDInsight](hdinsight-use-mapreduce.md)

Para obtener más información acerca de las herramientas de HDInsight para Visual Studio:

* [Introducción a las herramientas de HDInsight para Visual Studio](apache-hadoop-visual-studio-tools-get-started.md)