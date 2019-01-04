---
title: Uso de Apache Hive y Escritorio remoto en HDInsight (Azure)
description: Aprenda a conectarse a un clúster de Hadoop en HDInsight mediante Escritorio remoto y, a continuación, ejecute consultas de Hive usando la interfaz de línea de comandos (CLI) de Hive.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/12/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: 6e0641f2d9427133f951ef63720b4efdac4defe5
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2018
ms.locfileid: "53409061"
---
# <a name="use-apache-hive-with-apache-hadoop-on-hdinsight-with-remote-desktop"></a>Uso de Apache Hive con Apache Hadoop en HDInsight con Escritorio remoto
[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

En este artículo, aprenderá a conectarse a un clúster de HDInsight a través del Escritorio remoto y a ejecutar después consultas de Apache Hive desde la interfaz de línea de comandos (CLI) de Hive.

> [!IMPORTANT]  
> Remote Desktop solo está disponible para clústeres de HDInsight que usan Windows como sistema operativo. Linux es el único sistema operativo que se usa en la versión 3.4 de HDInsight, o en las superiores. Consulte la información sobre la [retirada de HDInsight en Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement).
>
> En el caso de HDInsight 3.4 o superior, consulte [Uso de Apache Hive con HDInsight y Beeline](apache-hadoop-use-hive-beeline.md) para obtener información acerca de cómo ejecutar consultas de Hive directamente en el clúster desde una línea de comandos.

## <a id="prereq"></a>Requisitos previos
Para completar los pasos de este artículo, necesitará lo siguiente:

* Un clúster de HDInsight basado en Windows (Hadoop en HDInsight)
* Un equipo cliente con Windows 10, Window 8 o Windows 7

## <a id="connect"></a>Conexión con el Escritorio remoto
Habilite el Escritorio remoto para el clúster de HDInsight y conéctese a él siguiendo las instrucciones dadas en [Conexión a los clústeres de HDInsight con RDP](../hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

## <a id="hive"></a>Uso del comando de Hive
Cuando se haya conectado al escritorio para el clúster de HDInsight, utilice los pasos siguientes para trabajar con Hive.

1. Desde el escritorio de HDInsight, inicie la **línea de comandos de Hadoop**.
2. Introduzca el siguiente comando para iniciar la CLI de Hive:

        %hive_home%\bin\hive

    Una vez iniciada la CLI, verá el símbolo del sistema de la CLI de Hive: `hive>`.
3. Con la CLI, introduzca las siguientes instrucciones para crear una nueva tabla denominada **log4jLogs** con los datos de ejemplo:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Estas instrucciones realizan las acciones siguientes:

   * **DROP TABLE**: elimina la tabla y el archivo de datos si la tabla ya existe.
   * **CREATE EXTERNAL TABLE**: crea una nueva tabla "externa" en Hive. Las tablas externas solo almacenan la definición de tabla en Hive; los datos quedan en la ubicación original.

     > [!NOTE]  
     > Las tablas externas se deben usar cuando espera que un origen externo, como por ejemplo un proceso de carga de datos automático, u otra operación MapReduce, actualice los datos subyacentes, pero siempre desea que las consultas de Hive usen los datos más recientes.
     >
     > La eliminación de una tabla externa **no** elimina los datos, solamente la definición de tabla.
     >
     >
   * **ROW FORMAT**: indica a Hive cómo se da formato a los datos. En este caso, los campos de cada registro se separan mediante un espacio.
   * **STORED AS TEXTFILE LOCATION**: indica a Hive dónde se almacenan los datos (el directorio example/data) y que se almacenan como texto.
   * **SELECT**: Selecciona un recuento de todas las filas donde la columna **t4** contiene el valor **[ERROR]**. Esto debe devolver un valor de **3** porque hay tres filas que contienen este valor.
   * **INPUT__FILE__NAME LIKE '%.log'**: indica a Hive que solo deberíamos devolver datos de archivos que terminan en .log. Esto restringe la búsqueda al archivo sample.log que contiene los datos y le impide que devuelva datos de otros archivos de datos de ejemplo que no coinciden con el esquema que hemos definido.
4. Use las siguientes instrucciones para crear una nueva tabla "interna" llamada **errorLogs**.

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Estas instrucciones realizan las acciones siguientes:

   * **CREATE TABLE IF NOT EXISTS**: Crea una tabla, en caso de que no exista ya. Dado que la palabra clave **EXTERNAL** no se usa, se trata de una tabla interna, que se almacena en el almacenamiento de datos de Hive y es administrada completamente por Hive.

     > [!NOTE]  
     > A diferencia de las tablas **EXTERNAL** , la eliminación de una tabla interna también eliminará los datos subyacentes.
     >
     >
   * **STORED AS ORC**: almacena los datos en formato de columnas de filas optimizadas (ORC). Se trata de un formato altamente optimizado y eficiente para almacenar datos de Hive.
   * **INSERT OVERWRITE ... SELECT**: selecciona en la tabla **log4jLogs** las filas que contienen **[ERROR]** y, después, inserta los datos en la tabla **errorLogs**.

     Para comprobar que solamente las filas que contienen **[ERROR]** en la columna t4 se almacenaron en la tabla **errorLogs**, use la siguiente instrucción para devolver todas las filas de **errorLogs**:

       SELECT * de errorLogs;

     Se deben devolver tres filas de datos y todas ellas deben contener **[ERROR]** en la columna t4.

## <a id="summary"></a>Resumen
Como puede ver, el comando Hive proporciona una manera fácil de ejecutar consultas de Hive de manera interactiva en un clúster de HDInsight, supervisar el estado del trabajo y recuperar el resultado.

## <a id="nextsteps"></a>Pasos siguientes
Para obtener información general acerca de Hive en HDInsight:

* [Uso de Apache Hive con Apache Hadoop en HDInsight](hdinsight-use-hive.md)

Para obtener información sobre otras maneras de trabajar con Hadoop en HDInsight:

* [Uso de Apache Pig con Apache Hadoop en HDInsight](hdinsight-use-pig.md)
* [Uso de MapReduce con Apache Hadoop en HDInsight](hdinsight-use-mapreduce.md)

Si usa Tez con Hive, consulte los siguientes documentos para la información de depuración:

* [Uso de la interfaz de usuario de Apache Tez en HDInsight basado en Windows](../hdinsight-debug-tez-ui.md)
* [Uso de la vista Tez de Apache Ambari en HDInsight basado en Linux](../hdinsight-debug-ambari-tez-view.md)

[1]:apache-hadoop-visual-studio-tools-get-started.md

[azure-purchase-options]: https://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: https://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: https://azure.microsoft.com/pricing/free-trial/

[apache-tez]: https://tez.apache.org
[apache-hive]: https://hive.apache.org/
[apache-log4j]: https://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: https://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md





[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[Powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-here-strings]: https://technet.microsoft.com/library/ee692792.aspx
