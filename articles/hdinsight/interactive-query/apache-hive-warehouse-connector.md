---
title: Integración de Apache Spark y Apache Hive con el conector de Hive Warehouse
description: Aprenda a integrar Apache Spark y Apache Hive con el conector de Hive Warehouse en Azure HDInsight.
ms.service: hdinsight
author: nakhanha
ms.author: nakhanha
ms.reviewer: hrasheed
ms.topic: conceptual
ms.date: 04/29/2019
ms.openlocfilehash: 98b97abf9fe0d8e71f93022dcbfeff0a5339d5a1
ms.sourcegitcommit: 6d2a147a7e729f05d65ea4735b880c005f62530f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2019
ms.locfileid: "69983079"
---
# <a name="integrate-apache-spark-and-apache-hive-with-the-hive-warehouse-connector"></a>Integración de Apache Spark y Apache Hive con el conector de Hive Warehouse

El conector de Apache Hive Warehouse (HWC) es una biblioteca que le permite trabajar más fácilmente con Apache Spark y Apache Hive, dado que admite tareas como mover datos entre DataFrames de Spark y tablas de Hive, y también dirigir datos de streaming de Spark a tablas de Hive. El conector de Hive Warehouse funciona como un puente entre Spark y Hive. Admite Scala, Java y Python para el desarrollo.

El conector de Hive Warehouse le permite aprovechar las ventajas de las características exclusivas de Hive y Spark para crear eficaces aplicaciones de macrodatos. Apache Hive ofrece compatibilidad con las transacciones de base de datos de atomicidad, coherencia, aislamiento, durabilidad (ACID). Para obtener más información sobre ACID y las transacciones en Hive, consulte [Hive Transactions](https://cwiki.apache.org/confluence/display/Hive/Hive+Transactions) (Transacciones de Hive). Hive también ofrece controles de seguridad detallados a través de Apache Ranger y el procesamiento analítico de baja latencia que no está disponible en Apache Spark.

Apache Spark tiene una API de streaming estructurada que proporciona funcionalidades de streaming que no están disponibles en Apache Hive. A partir de la versión 4.0 de HDInsight, Apache Spark 2.3.1 y Apache Hive 3.1.0 tienen tiendas de metadatos independientes, lo que puede dificultar la interoperabilidad. El conector de Hive Warehouse simplifica el uso conjunto de Spark y Hive. La biblioteca HWC carga los datos de los demonios LLAP a los ejecutores de Spark en paralelo, por lo que resulta más eficiente y escalable que el uso de una conexión JDBC estándar desde Spark a Hive.

![Arquitectura](./media/apache-hive-warehouse-connector/hive-warehouse-connector-architecture.png)

Algunas de las operaciones compatibles con el conector de Hive Warehouse son:

* Descripción de una tabla
* Creación de una tabla de datos en formato ORC
* Selección de datos de Hive y recuperación de un elemento DataFrame
* Escritura de un elemento DataFrame en Hive en un lote
* Ejecución de una instrucción de actualización de Hive
* Lectura de datos de la tabla de Hive, transformación en Spark y escritura en una nueva tabla de Hive
* Escritura de un flujo de Spark o DataFrame en Hive mediante Hive Streaming

## <a name="hive-warehouse-connector-setup"></a>Configuración del conector de Hive Warehouse

Siga estos pasos para configurar el conector de Hive Warehouse entre un clúster de Spark e Interactive Query en Azure HDInsight:

1. Cree un clúster de HDInsight Spark 4.0 mediante Azure Portal con una cuenta de almacenamiento y una red virtual de Azure personalizada. Para obtener información sobre cómo crear un clúster en una red virtual de Azure, consulte [Add HDInsight to an existing virtual network](../../hdinsight/hdinsight-plan-virtual-network-deployment.md#existingvnet) (Adición de HDInsight a una red virtual existente).
1. Cree un clúster de Interactive Query de HDInsight (LLAP) 4.0 mediante Azure Portal con la misma cuenta de almacenamiento y la red virtual de Azure que el clúster de Spark.
1. Copie el contenido del archivo `/etc/hosts` en headnode0 de su clúster de Interactive Query en el archivo `/etc/hosts` de headnode0 de su clúster de Spark. Este paso permitirá al clúster de Spark resolver las direcciones IP de los nodos del clúster de Interactive Query. Vea el contenido del archivo actualizado con `cat /etc/hosts`. La salida debe tener un aspecto similar a lo que se muestra en la captura de pantalla siguiente.

    ![ver el archivo de hosts](./media/apache-hive-warehouse-connector/hive-warehouse-connector-hosts-file.png)

1. Para establecer la configuración del clúster de Spark, realice los pasos siguientes: 
    1. Vaya a Azure Portal, seleccione clústeres de HDInsight y, a continuación, haga clic en el nombre del clúster.
    1. En el lado derecho, bajo **Paneles de clúster**, seleccione **Inicio de Ambari**.
    1. En la interfaz de usuario web de Ambari, haga clic en **SPARK2** > **CONFIGS** > **Custom spark2-defaults** (Valores predeterminados personalizados de Spark2).

        ![Configuración de Spark2 en Ambari](./media/apache-hive-warehouse-connector/hive-warehouse-connector-spark2-ambari.png)

    1. Establezca `spark.hadoop.hive.llap.daemon.service.hosts` en el mismo valor que la propiedad **hive.llap.daemon.service.hosts** de ** Advanced hive-interactive-site** (hive-interactive-site avanzado). Por ejemplo: `@llap0`

    1. Establezca `spark.sql.hive.hiveserver2.jdbc.url` en la cadena de conexión JDBC, que se conecta a Hiveserver2 en el clúster de Interactive Query. La cadena de conexión para el clúster se parecerá al siguiente URI. `CLUSTERNAME` es el nombre de su clúster de Spark y los parámetros `user` y `password` están establecidos en los valores correctos para su clúster.

        ```
        jdbc:hive2://LLAPCLUSTERNAME.azurehdinsight.net:443/;user=admin;password=PWD;ssl=true;transportMode=http;httpPath=/hive2
        ```

        >[!Note] 
        > La dirección URL de JDBC debe contener credenciales para conectarse a Hiveserver2 incluido un nombre de usuario y una contraseña.

    1. Establezca `spark.datasource.hive.warehouse.load.staging.dir` en un directorio de almacenamiento provisional adecuado y compatible con HDFS. Si tiene dos clústeres diferentes, el directorio de almacenamiento provisional debe ser una carpeta del directorio de almacenamiento provisional de la cuenta de almacenamiento del clúster de LLAP para que HiveServer2 tenga acceso a ella. Por ejemplo, `wasb://STORAGE_CONTAINER_NAME@STORAGE_ACCOUNT_NAME.blob.core.windows.net/tmp` en que `STORAGE_ACCOUNT_NAME` es el nombre de la cuenta de almacenamiento que usa el clúster, y `STORAGE_CONTAINER_NAME` es el nombre del contenedor de almacenamiento.

    1. Establezca `spark.datasource.hive.warehouse.metastoreUri` con el valor del URI de la tienda de metadatos del clúster de Interactive Query. Para buscar el valor de metastoreUri de su clúster LLAP, busque la propiedad **hive.metastore.uris** en la interfaz de usuario de Ambari de su clúster de LLAP en **Hive** > **AVANZADO** > **General**. El valor tendrá un aspecto similar al siguiente URI:

        ```
        thrift://hn0-hwclla.0iv2nyrmse1uvp2caa4e34jkmf.cx.internal.cloudapp.net:9083,
        thrift://hn1-hwclla.0iv2nyrmse1uvp2caa4e34jkmf.cx.internal.cloudapp.net:9083
        ```

    1. Establezca `spark.security.credentials.hiveserver2.enabled` en `false` para el modo de implementación del cliente YARN.
    1. Establezca `spark.hadoop.hive.zookeeper.quorum` en el cuórum de Zookeeper del clúster de LLAP. Para buscar el cuórum de Zookeeper de su clúster de LLAP, busque la propiedad **hive.zookeeper.quorum** en la interfaz de usuario de Ambari de su clúster de LLAP en **Hive** > **AVANZADO** > **Advanced hive-site** (hive-site avanzado). El valor tendrá un aspecto similar a la cadena siguiente:

        ```
        zk1-nkhvne.0iv2nyrmse1uvp2caa4e34jkmf.cx.internal.cloudapp.net:2181,
        zk4-nkhvne.0iv2nyrmse1uvp2caa4e34jkmf.cx.internal.cloudapp.net:2181,
        zk6-nkhvne.0iv2nyrmse1uvp2caa4e34jkmf.cx.internal.cloudapp.net:2181
        ```

Para probar la configuración del conector de Hive Warehouse, siga los pasos descritos en la sección [Conexión y ejecución de consultas](#connecting-and-running-queries).

## <a name="using-the-hive-warehouse-connector"></a>Uso del conector de Hive Warehouse

### <a name="connecting-and-running-queries"></a>Conexión y ejecución de consultas

Puede elegir entre varios métodos diferentes para conectarse a su clúster de Interactive Query y ejecutar consultas mediante el conector de Hive Warehouse. Entre los métodos admitidos se incluyen las herramientas siguientes:

* [spark-shell](../spark/apache-spark-shell.md)
* PySpark
* spark-submit
* [Zeppelin](../spark/apache-spark-zeppelin-notebook.md)
* [Livy](../spark/apache-spark-livy-rest-interface.md)

Todos los ejemplos proporcionados en este artículo se ejecutarán a través de spark-shell.

Para iniciar una sesión de spark-shell, realice los pasos siguientes:

1. Use SSH en el nodo principal del clúster. Para obtener información acerca de cómo conectarse al clúster con SSH, consulte [Conexión a HDInsight (Apache Hadoop) mediante SSH](../../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).
1. Para cambiar al directorio correcto, escriba `cd /usr/hdp/current/hive_warehouse_connector` o proporcione la ruta de acceso completa a todos los archivos .jar que se usan como parámetros en el comando spark-shell.
1. Ejecute el siguiente comando para iniciar el shell de Spark:

    ```bash
    spark-shell --master yarn \
    --jars /usr/hdp/3.0.1.0-183/hive_warehouse_connector/hive-warehouse-connector-assembly-1.0.0.3.0.1.0-183.jar \
    --conf spark.security.credentials.hiveserver2.enabled=false
    ```

1. Verá un mensaje de bienvenida y una solicitud `scala>` en que puede escribir comandos.

1. Después de iniciar spark-shell, se puede iniciar una instancia del conector de Hive Warehouse mediante los siguientes comandos:

    ```scala
    import com.hortonworks.hwc.HiveWarehouseSession
    val hive = HiveWarehouseSession.session(spark).build()
    ```

### <a name="connecting-and-running-queries-on-enterprise-security-package-esp-clusters"></a>Conexión y ejecución de consultas en los clústeres de Enterprise Security Package (ESP)

Enterprise Security Package (ESP) le proporciona funcionalidades de nivel empresarial, como la autenticación basada en Active Directory, compatibilidad con varios usuarios y control de acceso basado en rol para los clústeres de Apache Hadoop en Azure HDInsight. Para más información, consulte [Uso de Enterprise Security Package en HDInsight](../domain-joined/apache-domain-joined-architecture.md).

1. Siga los pasos iniciales de 1 y 2 de la sección [Conexión y ejecución de consultas](#connecting-and-running-queries).
1. Escriba `kinit` e inicie sesión con un usuario de dominio.
1. Inicie spark-shell con la lista completa de parámetros de configuración como se muestra a continuación. Todos los valores en letras mayúsculas que se encuentran entre corchetes angulares deben especificarse en función de su clúster. Si necesita averiguar los valores de entrada para cualquiera de los parámetros siguientes, vea la sección [Configuración del conector de Hive Warehouse](#hive-warehouse-connector-setup).

    ```bash
    spark-shell --master yarn \
    --jars /usr/hdp/3.0.1.0-183/hive_warehouse_connector/hive-warehouse-connector-assembly-1.0.0.3.0.1.0-183.jar \
    --conf spark.security.credentials.hiveserver2.enabled=false
    --conf spark.hadoop.hive.llap.daemon.service.hosts='<LLAP_APP_NAME>'
    --conf spark.sql.hive.hiveserver2.jdbc.url='jdbc:hive2://<ZOOKEEPER_QUORUM>;serviceDiscoveryMode=zookeeper;zookeeperNamespace=hiveserver2-interactive'
    --conf spark.datasource.hive.warehouse.load.staging.dir='<STAGING_DIR>'
    --conf spark.datasource.hive.warehouse.metastoreUri='<METASTORE_URI>'
    --conf spark.hadoop.hive.zookeeper.quorum='<ZOOKEEPER_QUORUM>'
   ```

### <a name="creating-spark-dataframes-from-hive-queries"></a>Creación de DataFrames de Spark a partir de consultas de Hive

Los resultados de todas las consultas mediante la biblioteca HWC se devuelven como un elemento DataFrame. En los ejemplos siguientes se muestra cómo crear una consulta básica.

```scala
hive.setDatabase("default")
val df = hive.executeQuery("select * from hivesampletable")
df.filter("state = 'Colorado'").show()
```

Los resultados de la consulta son DataFrames de Spark, que se pueden usar con bibliotecas de Spark como MLIB y SparkSQL.

### <a name="writing-out-spark-dataframes-to-hive-tables"></a>Escritura de DataFrames de Spark en tablas de Hive

Spark no admite la escritura en tablas ACID administradas de Hive de forma nativa. Con HWC, sin embargo, puede escribir cualquier elemento DataFrame en una tabla de Hive. Puede ver como funciona esta funcionalidad en el ejemplo siguiente:

1. Cree una tabla denominada `sampletable_colorado` y especifique sus columnas con el comando siguiente:

    ```scala
    hive.createTable("sampletable_colorado").column("clientid","string").column("querytime","string").column("market","string").column("deviceplatform","string").column("devicemake","string").column("devicemodel","string").column("state","string").column("country","string").column("querydwelltime","double").column("sessionid","bigint").column("sessionpagevieworder","bigint").create()
    ```

2. Filtre la tabla `hivesampletable` en que la columna `state` es igual a `Colorado`. Esta consulta de la tabla de Hive se devuelve como un elemento DataFrame de Spark. A continuación, el elemento DataFrame se guarda en la tabla de Hive `sampletable_colorado` utilizando la función `write`.
    
    ```scala
    hive.table("hivesampletable").filter("state = 'Colorado'").write.format(HiveWarehouseSession.HIVE_WAREHOUSE_CONNECTOR).option("table","sampletable_colorado").save()
    ```

Puede ver la tabla resultante en la captura de pantalla siguiente.

![mostrar tabla resultante](./media/apache-hive-warehouse-connector/hive-warehouse-connector-show-hive-table.png)

### <a name="structured-streaming-writes"></a>Escrituras de flujos estructurados

Al usar el conector de Hive Warehouse, puede emplear los flujos de Spark para escribir datos en las tablas de Hive.

Siga los pasos siguientes para crear un ejemplo del conector de Hive Warehouse en que ingiere datos de un flujo de Spark en el puerto localhost 9999 en una tabla de Hive.

1. Abra un terminal en el clúster de Spark.
1. Inicie el flujo de Spark con el siguiente comando:

    ```scala
    val lines = spark.readStream.format("socket").option("host", "localhost").option("port",9988).load()
    ```

1. Genere datos para el flujo de Spark que creó. Para ello, siga estos pasos:
    1. Abra otro terminal en el mismo clúster de Spark.
    1. En el símbolo del sistema, escriba `nc -lk 9999`. Este comando usa la utilidad netcat para enviar datos desde la línea de comandos al puerto especificado.
    1. Escriba las palabras que quiera que el flujo de Spark ingiera, seguidas de un retorno de carro.
        ![datos de entrada del flujo de Spark](./media/apache-hive-warehouse-connector/hive-warehouse-connector-spark-stream-data-input.png)
1. Cree una nueva tabla de Hive para mantener los datos de flujos. En spark-shell, escriba los siguientes comandos:

    ```scala
    hive.createTable("stream_table").column("value","string").create()
    ```

1. Escriba los datos de flujos en la tabla recién creada usando el siguiente comando:

    ```scala
    lines.filter("value = 'HiveSpark'").writeStream.format(HiveWarehouseSession.STREAM_TO_STREAM).option("database", "default").option("table","stream_table").option("metastoreUri",spark.conf.get("spark.datasource.hive.warehouse.metastoreUri")).option("checkpointLocation","/tmp/checkpoint1").start()
    ```

    >[!Important]
    > Actualmente, las opciones `metastoreUri` y `database` deben establecerse de forma manual debido a un problema conocido en Apache Spark. Para obtener más información acerca de este problema, consulte  [SPARK-25460](https://issues.apache.org/jira/browse/SPARK-25460).

1. Puede ver los datos insertados en la tabla con el siguiente comando:

    ```scala
    hive.table("stream_table").show()
    ```

### <a name="securing-data-on-spark-esp-clusters"></a>Protección de datos en los clústeres ESP de Spark

1. Cree una tabla `demo` con algunos datos de ejemplo mediante los comandos siguientes:

    ```scala
    create table demo (name string);
    INSERT INTO demo VALUES ('HDinsight');
    INSERT INTO demo VALUES ('Microsoft');
    INSERT INTO demo VALUES ('InteractiveQuery');
    ```

1. Ver el contenido de la tabla con el siguiente comando. Antes de aplicar la directiva, en la tabla `demo` se muestra la columna completa.

    ```scala
    hive.executeQuery("SELECT * FROM demo").show()
    ```

    ![tabla de demostración antes de aplicar la directiva de Ranger](./media/apache-hive-warehouse-connector/hive-warehouse-connector-table-before-ranger-policy.png)

1. Aplique una directiva de enmascaramiento de columnas que solo muestre los últimos cuatro caracteres de la columna.  
    1. Abra la interfaz de usuario de administrador de Ranger en `https://CLUSTERNAME.azurehdinsight.net/ranger/`.
    1. Haga clic en el servicio de Hive de su clúster en **Hive**.
        ![tabla de demostración antes de aplicar la directiva de Ranger](./media/apache-hive-warehouse-connector/hive-warehouse-connector-ranger-service-manager.png)
    1. Haga clic en la pestaña **Enmascaramiento** y, a continuación, **Agregar nueva directiva** ![lista de directivas](./media/apache-hive-warehouse-connector/hive-warehouse-connector-ranger-hive-policy-list.png).
    1. Proporcione el nombre que quiera para la directiva y seleccione las opciones siguientes. Para la base de datos: **valor predeterminado**; tabla de Hive: **demostración**; columna de Hive: **nombre**; usuario: **rsadmin2**; tipos de acceso: **select** (determinados) y, en el menú **Select Masking Option** (Seleccionar opción de enmascaramiento), **Partial mask: show last 4** (Máscara parcial: mostrar últimos 4). Haga clic en **Agregar**.
                ![lista de directivas](./media/apache-hive-warehouse-connector/hive-warehouse-connector-ranger-create-policy.png)
1. Vuelva a consultar el contenido de la tabla. Después de aplicar la directiva de Ranger, solo podemos ver los cuatro últimos caracteres de la columna.

    ![tabla de demostración antes de aplicar la directiva de Ranger](./media/apache-hive-warehouse-connector/hive-warehouse-connector-table-after-ranger-policy.png)

## <a name="next-steps"></a>Pasos siguientes

* [Uso de Interactive Query con HDInsight](https://docs.microsoft.com/azure/hdinsight/interactive-query/apache-interactive-query-get-started)
* [Ejemplos de la interacción con el conector de Hive Warehouse mediante Zeppelin, Livy, spark-submit y pyspark](https://community.hortonworks.com/articles/223626/integrating-apache-hive-with-apache-spark-hive-war.html)
