---
title: 'Apache Sqoop con Apache Hadoop: Azure HDInsight'
description: Aprenda a usar Apache Sqoop para realizar importaciones y exportaciones entre Apache Hadoop en HDInsight y Azure SQL Database.
keywords: hadoop sqoop,sqoop
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 04/15/2019
ms.openlocfilehash: 6dcb6853daf34fede590011d165c0ba9001cbac6
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62128981"
---
# <a name="use-apache-sqoop-to-import-and-export-data-between-apache-hadoop-on-hdinsight-and-sql-database"></a>Usar Apache Sqoop para importar y exportar datos entre Apache Hadoop en HDInsight y SQL Database

[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

Aprenda a usar Apache Sqoop para realizar importaciones y exportaciones entre un clúster de Apache Hadoop en Azure HDInsight y Azure SQL Database o la base de datos de Microsoft SQL Server. En los pasos descritos en este documento se usa el comando `sqoop` directamente desde el nodo principal del clúster de Hadoop. Usaremos SSH para conectarnos al nodo principal y ejecutar los comandos de este documento. En este artículo es una continuación de [Use Apache Sqoop con Hadoop en HDInsight](./hdinsight-use-sqoop.md).

## <a name="prerequisites"></a>Requisitos previos

* Finalización de [configurar el entorno de prueba](./hdinsight-use-sqoop.md#create-cluster-and-sql-database) desde [Use Apache Sqoop con Hadoop en HDInsight](./hdinsight-use-sqoop.md).

* Un cliente para consultar la base de datos SQL de Azure. Considere el uso de [SQL Server Management Studio](../../sql-database/sql-database-connect-query-ssms.md) o [Visual Studio Code](../../sql-database/sql-database-connect-query-vscode.md).

* Un cliente SSH. Para más información, consulte [Conexión a través de SSH con HDInsight (Apache Hadoop)](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="sqoop-export"></a>Exportación de Sqoop

Desde Hive a SQL Server.

1. Use SSH para conectarse al clúster de HDInsight. Reemplace `CLUSTERNAME` con el nombre del clúster, a continuación, escriba el comando:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

2. Reemplace `MYSQLSERVER` con el nombre de SQL Server. Para comprobar que Sqoop puede ver la base de datos de SQL, escriba el comando siguiente en la conexión SSH abierta. Escriba la contraseña para el inicio de sesión de SQL Server cuando se le solicite. Este comando devuelve una lista de bases de datos.

    ```bash
    sqoop list-databases --connect jdbc:sqlserver://MYSQLSERVER.database.windows.net:1433 --username sqluser -P
    ```

3. Reemplace `MYSQLSERVER` con el nombre de SQL Server, y `MYDATABASE` con el nombre de la base de datos SQL. Para exportar datos desde el subárbol `hivesampletable` la tabla a la `mobiledata` de tabla de base de datos de SQL, escriba el comando siguiente en la conexión SSH abierta. Escriba la contraseña para el inicio de sesión de SQL Server cuando se le pida

    ```bash
    sqoop export --connect 'jdbc:sqlserver://MYSQLSERVER.database.windows.net:1433;database=MYDATABASE' --username sqluser -P -table 'mobiledata' --hcatalog-table hivesampletable
    ```

4. Para comprobar que se exportan los datos, use las siguientes consultas en el cliente SQL para ver los datos exportados:

    ```sql
    SELECT COUNT(*) FROM [dbo].[mobiledata] WITH (NOLOCK);
    SELECT TOP(25) * FROM [dbo].[mobiledata] WITH (NOLOCK);
    ```

## <a name="sqoop-import"></a>Importación de Sqoop

Desde SQL Server en Azure storage.

1. Reemplace `MYSQLSERVER` con el nombre de SQL Server, y `MYDATABASE` con el nombre de la base de datos SQL. Escriba el comando siguiente en la conexión de SSH abierta al importar datos desde el `mobiledata` a la tabla de base de datos SQL, el `wasb:///tutorials/usesqoop/importeddata` directorio en HDInsight. Escriba la contraseña para el inicio de sesión de SQL Server cuando se le solicite. Los campos de los datos se separan mediante un carácter de tabulación y las líneas terminan con un carácter de nueva línea.

    ```bash
    sqoop import --connect 'jdbc:sqlserver://MYSQLSERVER.database.windows.net:1433;database=MYDATABASE' --username sqluser -P --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

2. Una vez completada la importación, escriba el siguiente comando en la conexión SSH abierta a la lista de los datos en el nuevo directorio:

    ```bash
    hdfs dfs -text /tutorials/usesqoop/importeddata/part-m-00000
    ```

## <a name="limitations"></a>Limitaciones

* Exportación masiva: con HDInsight basado en Linux, el conector Sqoop que se usa para exportar datos a Microsoft SQL Server o Azure SQL Database no admite inserciones masivas.

* Procesamiento por lotes: con HDInsight basado en Linux, cuando se usa `-batch` al realizar inserciones, Sqoop realiza varias inserciones en lugar de procesar por lotes las operaciones de inserción.

## <a name="important-considerations"></a>Consideraciones importantes

* Tanto HDInsight como SQL Server deben estar en la misma red Azure Virtual Network.

    Para obtener un ejemplo, vea el documento [Conexión de HDInsight a la red local](./../connect-on-premises-network.md).

    Para más información sobre cómo usar HDInsight con redes Azure Virtual Network, vea el documento [Extensión de las funcionalidades de HDInsight con Azure Virtual Network](../hdinsight-extend-hadoop-virtual-network.md). Para más información sobre Azure Virtual Network, vea el documento [Información general sobre Virtual Network](../../virtual-network/virtual-networks-overview.md).

* SQL Server estar configurado para permitir la autenticación SQL. Para más información, vea el documento [Choose an Authentication Mode (Elegir un modo de autenticación)](https://msdn.microsoft.com/ms144284.aspx).

* Es posible que tenga que configurar SQL Server para aceptar conexiones remotas. Para más información, vea el documento [How to troubleshoot connecting to the SQL Server database engine (Solución de problemas de conexión al motor de base de datos de SQL Server)](https://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx).

## <a name="next-steps"></a>Pasos siguientes

Ahora ya ha aprendido a usar Sqoop. Para obtener más información, consulte:

* [Uso de Apache Oozie con HDInsight](../hdinsight-use-oozie-linux-mac.md): use la acción Sqoop en un flujo de trabajo de Oozie.
* [Análisis de la información de retraso de vuelos con HDInsight](../hdinsight-analyze-flight-delay-data-linux.md): use Apache Hive para analizar la información de retraso de los vuelos y luego use Sqoop para exportar los datos a una base de datos SQL de Azure.
* [Carga de datos en HDInsight](../hdinsight-upload-data.md): busque otros métodos para cargar datos en HDInsight o Azure Blob Storage.
