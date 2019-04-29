---
title: 'Uso de Spark para leer y escribir datos de HBase: Azure HDInsight'
description: Use el conector Spark HBase para leer y escribir datos de un clúster Spark en un clúster HBase.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/12/2019
ms.openlocfilehash: e3f5cb726dddbdbfbd1b1f48c800ac681e7a174c
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/24/2019
ms.locfileid: "63765694"
---
# <a name="use-apache-spark-to-read-and-write-apache-hbase-data"></a>Uso de Apache Spark para leer y escribir datos de Apache HBase

Apache HBase se consulta, normalmente, con su API de bajo nivel (instrucciones scan, get y put) o con una sintaxis SQL que utiliza Apache Phoenix. Apache también proporciona el conector HBase de Apache Spark, que es una alternativa práctica y eficaz para consultar y modificar los datos almacenados por HBase.

## <a name="prerequisites"></a>Requisitos previos

* Dos diferentes al menos otro Spark con, uno HBase y clústeres de HDInsight Spark 2.1 (HDInsight 3.6) instalado.
* El clúster Spark debe comunicarse directamente con el clúster HBase con una latencia mínima, por lo que la configuración recomendada es implementar ambos clústeres en la misma red virtual. Para obtener más información, consulte [Crear clústeres basados en Linux en HDInsight con Azure Portal](hdinsight-hadoop-create-linux-clusters-portal.md).
* Un cliente SSH. Para más información, consulte [Conexión a través de SSH con HDInsight (Apache Hadoop)](hdinsight-hadoop-linux-use-ssh-unix.md).
* El [esquema URI](hdinsight-hadoop-linux-information.md#URI-and-scheme) para el almacenamiento principal de clústeres. Esto sería wasb: / / para Azure Blob Storage, abfs: / / para Azure Data Lake Storage Gen2 o adl: / / para Azure Data Lake Storage Gen1. Si la transferencia segura está habilitada para Blob Storage o Data Lake Storage Gen2, el identificador URI sería wasbs: / / o abfss: / /, respectivamente, consulte [transferencia segura](../storage/common/storage-require-secure-transfer.md).


## <a name="overall-process"></a>Proceso general

El proceso de alto nivel para habilitar el clúster Spark para consultar el clúster HDInsight es el siguiente:

1. Prepare algunos datos de ejemplo en HBase.
2. Recupere el archivo hbase-site.xml de la carpeta de configuración del clúster HBase (/etc/hbase/conf).
3. Coloque una copia de hbase-site.xml en la carpeta de configuración de Spark 2 (/etc/spark2/conf).
4. Ejecute `spark-shell` haciendo referencia al conector Spark HBase por sus coordenadas Maven en la opción `packages`.
5. Defina un catálogo que asigne el esquema de Spark a HBase.
6. Interactúe con los datos de HBase mediante las API de DataFrame o RDD.

## <a name="prepare-sample-data-in-apache-hbase"></a>Preparación de datos de ejemplo en Apache HBase

En este paso, se crea y rellena una tabla sencilla en Apache HBase que, después, se podrá consultar mediante Spark.

1. Conéctese al nodo principal del clúster HBase mediante SSH. Para más información, vea [Conexión a través de SSH con HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).  Modifique el comando siguiente reemplazando `HBASECLUSTER` con el nombre del clúster de HBase, `sshuser` con el ssh usuario el nombre de cuenta y, a continuación, escriba el comando.

    ```
    ssh sshuser@HBASECLUSTER-ssh.azurehdinsight.net
    ```

2. Escriba el siguiente comando para iniciar el shell de HBase:

        hbase shell

3. Escriba el comando siguiente para crear un `Contacts` tabla con las familias de columna `Personal` y `Office`:

        create 'Contacts', 'Personal', 'Office'

4. Escriba los siguientes comandos para cargar algunas filas de ejemplo de datos:

        put 'Contacts', '1000', 'Personal:Name', 'John Dole'
        put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
        put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
        put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
        put 'Contacts', '8396', 'Personal:Name', 'Calvin Raji'
        put 'Contacts', '8396', 'Personal:Phone', '230-555-0191'
        put 'Contacts', '8396', 'Office:Phone', '230-555-0191'
        put 'Contacts', '8396', 'Office:Address', '5415 San Gabriel Dr.'

5. Escriba el siguiente comando para salir del shell de HBase:

        exit 

## <a name="copy-hbase-sitexml-to-spark-cluster"></a>Copie hbase-site.xml para el clúster de Spark
Copie hbase-site.xml del almacenamiento local en la raíz del almacenamiento de forma predeterminada del clúster Spark.  Edite el siguiente comando para reflejar la configuración.  A continuación, en la sesión SSH abierta para el clúster de HBase, escriba el comando:

| Valor de sintaxis | Nuevo valor|
|---|---|
|[Esquema de URI](hdinsight-hadoop-linux-information.md#URI-and-scheme) | Modificar para reflejar su almacenamiento.  La sintaxis siguiente es para el almacenamiento de blobs con habilitadas para transferencia segura.|
|`SPARK_STORAGE_CONTAINER`|Reemplazar por el nombre del contenedor de almacenamiento predeterminada usado para el clúster de Spark.|
|`SPARK_STORAGE_ACCOUNT`|Reemplazar por el nombre de cuenta de almacenamiento predeterminada usado para el clúster de Spark.|

```
hdfs dfs -copyFromLocal /etc/hbase/conf/hbase-site.xml wasbs://SPARK_STORAGE_CONTAINER@SPARK_STORAGE_ACCOUNT.blob.core.windows.net/
```

## <a name="put-hbase-sitexml-on-your-spark-cluster"></a>Colocar hbase-site.xml en el clúster Spark

1. Conéctese al nodo principal del clúster Spark mediante SSH.

2. Escriba el siguiente comando para copiar `hbase-site.xml` desde almacenamiento de información del clúster Spark de forma predeterminada a la carpeta de configuración de Spark 2 en el almacenamiento del clúster local:

        sudo hdfs dfs -copyToLocal /hbase-site.xml /etc/spark2/conf

## <a name="run-spark-shell-referencing-the-spark-hbase-connector"></a>Ejecutar el shell Spark haciendo referencia al conector Spark HBase

1. En la sesión SSH abierta para el clúster de Spark, escriba el siguiente comando para iniciar un shell de spark:

    ```
    spark-shell --packages com.hortonworks:shc-core:1.1.1-2.1-s_2.11 --repositories https://repo.hortonworks.com/content/groups/public/
    ```  

2. Mantenga abierta esta instancia de shell Spark y continúe con el paso siguiente.

## <a name="define-a-catalog-and-query"></a>Definir un catálogo y una consulta

En este paso, se define un objeto de catálogo que asigna el esquema de Apache Spark a Apache HBase.  

1. En el Shell Spark abierto, escriba lo siguiente `import` instrucciones:

    ```scala
    import org.apache.spark.sql.{SQLContext, _}
    import org.apache.spark.sql.execution.datasources.hbase._
    import org.apache.spark.{SparkConf, SparkContext}
    import spark.sqlContext.implicits._
    ```  

2. Escriba el comando siguiente para definir un catálogo de la tabla de contactos creada en HBase:

    ```scala
    def catalog = s"""{
        |"table":{"namespace":"default", "name":"Contacts"},
        |"rowkey":"key",
        |"columns":{
        |"rowkey":{"cf":"rowkey", "col":"key", "type":"string"},
        |"officeAddress":{"cf":"Office", "col":"Address", "type":"string"},
        |"officePhone":{"cf":"Office", "col":"Phone", "type":"string"},
        |"personalName":{"cf":"Personal", "col":"Name", "type":"string"},
        |"personalPhone":{"cf":"Personal", "col":"Phone", "type":"string"}
        |}
    |}""".stripMargin
    ```

    El código realiza lo siguiente:  

      a. Defina un esquema de catálogo para la tabla HBase denominada `Contacts`.  
     b. Identifique el objeto rowkey como `key` y asigne los nombres de columna usados en Spark a la familia de columna, el nombre de columna y el tipo de columna tal como se usa en HBase.  
     c. El objeto rowkey también tiene que definirse con detalle como una columna con nombre (`rowkey`), que tiene una familia de columna específica `cf` de `rowkey`.  

3. Escriba el comando siguiente para definir un método que proporciona una trama de datos en torno a su `Contacts` tabla de HBase:

    ```scala
    def withCatalog(cat: String): DataFrame = {
        spark.sqlContext
        .read
        .options(Map(HBaseTableCatalog.tableCatalog->cat))
        .format("org.apache.spark.sql.execution.datasources.hbase")
        .load()
     }
    ```

4. Cree una instancia de DataFrame:

    ```scala
    val df = withCatalog(catalog)
    ```  

5. Consulte el elemento DataFrame:

    ```scala
    df.show()
    ```

6. Verá dos filas de datos:

        +------+--------------------+--------------+-------------+--------------+
        |rowkey|       officeAddress|   officePhone| personalName| personalPhone|
        +------+--------------------+--------------+-------------+--------------+
        |  1000|1111 San Gabriel Dr.|1-425-000-0002|    John Dole|1-425-000-0001|
        |  8396|5415 San Gabriel Dr.|  230-555-0191|  Calvin Raji|  230-555-0191|
        +------+--------------------+--------------+-------------+--------------+

7. Registre una tabla temporal para poder consultar la tabla HBase mediante Spark SQL:

    ```scala
    df.createTempView("contacts")
    ```

8. Emita una consulta SQL en la tabla `contacts`:

    ```scala
    val query = spark.sqlContext.sql("select personalName, officeAddress from contacts")
    query.show()
    ```

9. Verá resultados parecidos a los siguientes:

        +-------------+--------------------+
        | personalName|       officeAddress|
        +-------------+--------------------+
        |    John Dole|1111 San Gabriel Dr.|
        |  Calvin Raji|5415 San Gabriel Dr.|
        +-------------+--------------------+

## <a name="insert-new-data"></a>Insertar nuevos datos

1. Para insertar un nuevo registro de contacto, defina una clase `ContactRecord`:

    ```scala
    case class ContactRecord(
        rowkey: String,
        officeAddress: String,
        officePhone: String,
        personalName: String,
        personalPhone: String
        )
    ```

2. Cree una instancia de `ContactRecord` y colóquela en una matriz:

    ```scala
    val newContact = ContactRecord("16891", "40 Ellis St.", "674-555-0110", "John Jackson","230-555-0194")

    var newData = new Array[ContactRecord](1)
    newData(0) = newContact
    ```

3. Guarde la matriz de nuevos datos en HBase:

    ```scala
    sc.parallelize(newData).toDF.write.options(Map(HBaseTableCatalog.tableCatalog -> catalog, HBaseTableCatalog.newTable -> "5")).format("org.apache.spark.sql.execution.datasources.hbase").save()
    ```

4. Examine los resultados:

    ```scala  
    df.show()
    ```

5. La salida debería ser parecida a la que se muestra a continuación:

        +------+--------------------+--------------+------------+--------------+
        |rowkey|       officeAddress|   officePhone|personalName| personalPhone|
        +------+--------------------+--------------+------------+--------------+
        |  1000|1111 San Gabriel Dr.|1-425-000-0002|   John Dole|1-425-000-0001|
        | 16891|        40 Ellis St.|  674-555-0110|John Jackson|  230-555-0194|
        |  8396|5415 San Gabriel Dr.|  230-555-0191| Calvin Raji|  230-555-0191|
        +------+--------------------+--------------+------------+--------------+

## <a name="next-steps"></a>Pasos siguientes

* [Conector HBase de Azure Spark](https://github.com/hortonworks-spark/shc)
