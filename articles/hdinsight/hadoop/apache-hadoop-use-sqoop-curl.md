---
title: Uso de Curl para exportar datos con Apache Sqoop en Azure HDInsight
description: Sepa cómo enviar remotamente trabajos de Apache Sqoop a Azure HDInsight mediante Curl.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/15/2019
ms.openlocfilehash: f70c0a0b68e24e3d61a6c0cef238d1f60911e271
ms.sourcegitcommit: fa4852cca8644b14ce935674861363613cf4bfdf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/09/2019
ms.locfileid: "70810732"
---
# <a name="run-apache-sqoop-jobs-in-hdinsight-with-curl"></a>Ejecución de trabajos de Apache Sqoop en HDInsight con Curl
[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

En este documento aprenderá a utilizar Curl para ejecutar trabajos de Apache Sqoop en un clúster de Apache Hadoop en HDInsight. En este artículo se muestra cómo exportar datos desde Azure Storage y cómo importarlos en una base de datos de SQL Server mediante Curl. Este artículo es una continuación de [Uso de Apache Sqoop con Hadoop en HDInsight](./hdinsight-use-sqoop.md).

Curl sirve para demostrar cómo se puede interactuar con HDInsight usando solicitudes HTTP sin procesar para ejecutar, supervisar y recuperar los resultados de los trabajos de Sqoop. Esto funciona mediante la API de REST de WebHCat (conocida anteriormente como Templeton) proporcionada por el clúster de HDInsight.

## <a name="prerequisites"></a>Requisitos previos

* Finalización de la [configuración de un entorno de prueba](./hdinsight-use-sqoop.md#create-cluster-and-sql-database) de [Uso de Apache Sqoop con Hadoop en HDInsight](./hdinsight-use-sqoop.md).

* Un cliente para consultar la base de datos de Azure SQL. Considere el uso de [SQL Server Management Studio](../../sql-database/sql-database-connect-query-ssms.md) o [Visual Studio Code](../../sql-database/sql-database-connect-query-vscode.md).

* [Curl](https://curl.haxx.se/). Curl es una herramienta que se usa para transferir datos desde o hacia un clúster de HDInsight.

* [jq](https://stedolan.github.io/jq/). La utilidad jq se usa para procesar los datos JSON que devuelven las solicitudes de REST.

## <a name="submit-apache-sqoop-jobs-by-using-curl"></a>Envío de trabajos de Apache Sqoop mediante Curl

Use Curl para exportar datos mediante trabajos de Apache Sqoop desde Azure Storage a SQL Server.

> [!NOTE]  
> Al usar Curl o cualquier otra comunicación REST con WebHCat, debe proporcionar el nombre de usuario y la contraseña del administrador del clúster de HDInsight para autenticar las solicitudes. También debe usar el nombre del clúster como parte del identificador uniforme de recursos (URI) que se usa para enviar las solicitudes al servidor.

Cuando se usen los comandos de esta sección, reemplace `USERNAME` por el usuario para autenticación en el clúster y `PASSWORD` por la contraseña de la cuenta de usuario. Reemplace `CLUSTERNAME` por el nombre del clúster.
 
La API de REST se protege con la [autenticación básica](https://en.wikipedia.org/wiki/Basic_access_authentication). Siempre debe crear solicitudes usando HTTP segura (HTTPS) para así garantizar que las credenciales se envían de manera segura al servidor.

1. Desde una línea de comandos, utilice el siguiente comando para comprobar que puede conectarse al clúster de HDInsight.

    ```cmd
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    Debería recibir una respuesta similar a la siguiente:

    ```json
    {"status":"ok","version":"v1"}
    ```

2. Reemplace `SQLDATABASESERVERNAME`, `USERNAME@SQLDATABASESERVERNAME`, `PASSWORD`, `SQLDATABASENAME` por los valores adecuados de los requisitos previos. Para enviar el trabajo de sqoop, use lo siguiente:

    ```cmd
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d command="export --connect jdbc:sqlserver://SQLDATABASESERVERNAME.database.windows.net;user=USERNAME@SQLDATABASESERVERNAME;password=PASSWORD;database=SQLDATABASENAME --table log4jlogs --export-dir /example/data/sample.log --input-fields-terminated-by \0x20 -m 1" -d statusdir="wasb:///example/data/sqoop/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/sqoop
    ```

    Los parámetros que se utilizan en este comando son los siguientes:

   * **-d**: como `-G` no se usa, la solicitud establece como valor predeterminado el método POST. `-d` especifica los valores de datos que se envían con la solicitud.

       * **user.name** : el usuario que ejecuta el comando.

       * **command** : el comando de Sqoop para ejecutar.

       * **statusdir** : el directorio donde se escribirá el estado de este trabajo.

     Este comando debe devolver un id. de trabajo que se pueda usar para comprobar el estado del trabajo.

       ```json
       {"id":"job_1415651640909_0026"}
       ```

3. Para revisar el estado del trabajo, use el siguiente comando. Reemplace `JOBID` por el valor devuelto en el paso anterior. Por ejemplo, si el valor devuelto fue `{"id":"job_1415651640909_0026"}`, entonces `JOBID` debería ser `job_1415651640909_0026`.

    ```cmd
    curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

    Si se completó el trabajo, el estado será **SUCCEEDED**.
   
   > [!NOTE]  
   > Esta solicitud de Curl devuelve un documento de notación de objetos JavaScript (JSON) con información acerca del trabajo; jq se usa para recuperar solo el valor de estado.

4. Una vez que el estado del trabajo cambió a **SUCCEEDED**, puede recuperar los resultados del trabajo desde Azure Blob Storage. El parámetro `statusdir` pasado con la consulta contiene la ubicación del archivo de salida; en este caso, `wasb:///example/data/sqoop/curl`. Esta dirección almacena la salida del trabajo en el directorio `example/data/sqoop/curl` en el contenedor de almacenamiento predeterminado que su clúster de HDInsight usa.

    Puede usar Azure Portal para tener acceso a los blobs "stderr" y "stdout".

5. Para comprobar que los datos se han exportado, use las siguientes consultas en el cliente SQL para ver los datos exportados:

    ```sql
    SELECT COUNT(*) FROM [dbo].[log4jlogs] WITH (NOLOCK);
    SELECT TOP(25) * FROM [dbo].[log4jlogs] WITH (NOLOCK);
    ```

## <a name="limitations"></a>Limitaciones
* Exportación masiva: con HDInsight basado en Linux, el conector Sqoop que se utiliza para exportar datos a Microsoft SQL Server o Azure SQL Database no es compatible actualmente con las inserciones masivas.
* Procesamiento por lotes: con HDInsight basado en Linux, cuando se usa `-batch` al realizar inserciones, Sqoop realizará varias inserciones en lugar de procesar por lotes las operaciones de inserción.

## <a name="summary"></a>Resumen
Tal como se ha mostrado en este documento, puede usar una solicitud HTTP sin procesar para ejecutar, supervisar y ver los resultados de los trabajos de Sqoop en su clúster de HDInsight.

Para más información sobre la interfaz REST utilizada en este artículo, consulte la <a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">guía de Apache Sqoop REST API</a>.

## <a name="next-steps"></a>Pasos siguientes
[Uso de Apache Sqoop con Apache Hadoop en HDInsight](hdinsight-use-sqoop.md)

Si quiere leer más artículos de HDInsight en los que se incluya Curl, consulte:
 
* [Creación de clústeres de Apache Hadoop mediante la API REST de Azure](../hdinsight-hadoop-create-linux-clusters-curl-rest.md)
* [Ejecución de consultas de Apache Hive con Apache Hadoop en HDInsight mediante REST](apache-hadoop-use-hive-curl.md)
* [Ejecución de trabajos de MapReduce con Apache Hadoop en HDInsight con REST](apache-hadoop-use-mapreduce-curl.md)

