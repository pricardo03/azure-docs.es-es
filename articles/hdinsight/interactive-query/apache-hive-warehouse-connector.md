---
title: Integración de Apache Spark y Apache Hive con el conector de almacén de Hive
description: Obtenga información sobre cómo integrar Apache Spark y Apache Hive con el conector del almacén de Hive en HDInsight de Azure.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 04/18/2019
ms.openlocfilehash: b450fe763104adbbd08e4b5f362bd51ffbf82c81
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62126552"
---
# <a name="integrate-apache-spark-and-apache-hive-with-the-hive-warehouse-connector"></a>Integración de Apache Spark y Apache Hive con el conector de almacén de Hive

El conector de almacenamiento de Hive de Apache (HWC) es una biblioteca que le permite trabajar más fácilmente con Apache Spark y Apache Hive al admitir tareas como mover datos entre las tablas de Hive y Spark DataFrames y dirigir también Spark streaming de datos en tablas de Hive. Hive Warehouse Connector funciona como un puente entre Spark y Hive. Admite Scala, Java y Python para el desarrollo.

El conector de almacenamiento de Hive le permite aprovechar las ventajas de las características exclusivas de Hive y Spark para crear eficaces aplicaciones de big data. Apache Hive ofrece compatibilidad con transacciones de base de datos que son atómica, coherente, aislado y durables (ACID). Para obtener más información sobre ACID y transacciones en Hive, consulte [Hive transacciones](https://cwiki.apache.org/confluence/display/Hive/Hive+Transactions). Hive también ofrece controles de seguridad detallada a través de Apache Ranger y baja latencia de procesamiento analítico no está disponible en Apache Spark.

Apache Spark, tiene una API de Streaming estructurado que proporciona la funcionalidad de streaming no está disponible en Apache Hive. Comenzando con Hortonworks Data Platform (HDP) 3.0, Apache Spark y Apache Hive tienen las tiendas de metadatos independiente, lo que pueden dificultar la interoperabilidad. El conector de almacenamiento de Hive resulta más fácil usar Spark y Hive juntos. La biblioteca HWC carga los datos desde los demonios de LLAP en ejecutores de Spark en paralelo, lo que más eficaz y escalable que usa una conexión estándar de JDBC de Spark a Hive.

![Arquitectura](./media/apache-hive-warehouse-connector/hive-warehouse-connector-architecture.png)

Algunas de las operaciones admitidas por el conector de almacenamiento de Hive son:

* Que describe una tabla
* Creación de una tabla de datos en formato ORC
* Seleccionar datos de Hive y recuperar una trama de datos
* Escribir una trama de datos en Hive en lote
* Ejecutar una instrucción update de Hive
* Leer datos de la tabla de Hive, transformarlos en Spark y escribirlos en una nueva tabla de Hive
* Escribir una secuencia de trama de datos o de Spark a Hive mediante HiveStreaming

## <a name="hive-warehouse-connector-setup"></a>Instalación del conector de almacén de Hive

Siga estos pasos para configurar el conector de almacenamiento de Hive entre un clúster de Spark e Interactive Query en HDInsight de Azure:

1. Cree un clúster de HDInsight Spark 4.0 mediante el portal de Azure con una cuenta de almacenamiento y una red virtual de Azure personalizada. Para obtener información sobre cómo crear un clúster en una red virtual de Azure, consulte [agregar HDInsight a una red virtual existente](../../hdinsight/hdinsight-extend-hadoop-virtual-network.md#existingvnet).
1. Crear un clúster de HDInsight Interactive Query (LLAP) 4.0 mediante el portal de Azure con la misma cuenta de almacenamiento y red virtual de Azure que el clúster de Spark.
1. Copie el contenido de la `/etc/hosts` archivo en headnode0 del clúster Interactive Query a la `/etc/hosts` archivo en el nodo principal headnode0 del clúster Spark. Este paso permitirá que el clúster de Spark resolver las direcciones IP de los nodos de clúster de Interactive Query. Ver el contenido del archivo actualizado con `cat /etc/hosts`. La salida debe parecerse a lo que se muestra en la captura de pantalla siguiente.

    ![ver el archivo de hosts](./media/apache-hive-warehouse-connector/hive-warehouse-connector-hosts-file.png)

1. Configure las opciones de clúster de Spark realizando los pasos siguientes: 
    1. Vaya a Azure portal, seleccione clústeres de HDInsight y, a continuación, haga clic en el nombre del clúster.
    1. En el lado derecho, bajo **paneles de clúster**, seleccione **Ambari principal**.
    1. En la interfaz de usuario de Ambari web, haga clic en **SPARK2** > **CONFIGS** > **personalizado spark2-defaults**.

        ![Configuración de Ambari Spark2](./media/apache-hive-warehouse-connector/hive-warehouse-connector-spark2-ambari.png)

    1. Establecer `spark.hadoop.hive.llap.daemon.service.hosts` en el mismo valor que la propiedad **nombre de la aplicación LLAP** en **Advanced hive interactivo env**. Por ejemplo: `@llap0`

    1. Establecer `spark.sql.hive.hiveserver2.jdbc.url` a la cadena de conexión JDBC que se conecta a Hiveserver2 en el clúster de Interactive Query. La cadena de conexión para el clúster se parecerá al siguiente de URI. `CLUSTERNAME` es el nombre del clúster Spark y el `user` y `password` parámetros se establecen en los valores correctos para el clúster.

        ```
        jdbc:hive2://LLAPCLUSTERNAME.azurehdinsight.net:443/;user=admin;password=PWD;ssl=true;transportMode=http;httpPath=/hive2
        ```

        >[!Note] 
        > La dirección URL de JDBC debe contener credenciales para conectarse a Hiveserver2 incluido un nombre de usuario y una contraseña.

    1. Establecer `spark.datasource.hive.warehouse.load.staging.dir` en un directorio de almacenamiento provisional adecuado de compatible con HDFS. Si tiene dos clústeres diferentes, el directorio de almacenamiento provisional debe ser una carpeta en el directorio de almacenamiento provisional de la cuenta de almacenamiento del clúster LLAP para que HiveServer2 tenga acceso a él. Por ejemplo, `wasb://STORAGE_CONTAINER_NAME@STORAGE_ACCOUNT_NAME.blob.core.windows.net/tmp` donde `STORAGE_ACCOUNT_NAME` es el nombre de la cuenta de almacenamiento utilizada por el clúster, y `STORAGE_CONTAINER_NAME` es el nombre del contenedor de almacenamiento.

    1. Establecer `spark.datasource.hive.warehouse.metastoreUri` con el valor de la tienda de metadatos URI del clúster Interactive Query. Para buscar el metastoreUri para el clúster LLAP, busque el **hive.metastore.uris** del clúster en la propiedad en la UI de Ambari para su LLAP **Hive** > **avanzadas**  >  **General**. El valor debe ser similar el siguiente URI:

        ```
        thrift://hn0-hwclla.0iv2nyrmse1uvp2caa4e34jkmf.cx.internal.cloudapp.net:9083,
        thrift://hn1-hwclla.0iv2nyrmse1uvp2caa4e34jkmf.cx.internal.cloudapp.net:9083
        ```

    1. Establecer `spark.security.credentials.hiveserver2.enabled` a `false` implementar el modo de cliente YARN.
    1. Establecer `spark.hadoop.hive.zookeeper.quorum` para el cuórum de Zookeeper del clúster de LLAP. Para buscar el cuórum de Zookeeper para el clúster LLAP, busque el **hive.zookeeper.quorum** del clúster en la propiedad en la UI de Ambari para su LLAP **Hive** > **Avanzadas**  >  **Advanced hive-site**. El valor debe ser similar a la siguiente cadena:

        ```
        zk1-nkhvne.0iv2nyrmse1uvp2caa4e34jkmf.cx.internal.cloudapp.net:2181,
        zk4-nkhvne.0iv2nyrmse1uvp2caa4e34jkmf.cx.internal.cloudapp.net:2181,
        zk6-nkhvne.0iv2nyrmse1uvp2caa4e34jkmf.cx.internal.cloudapp.net:2181
        ```

Para probar la configuración del conector de almacén de Hive, siga los pasos descritos en la sección [conexión y ejecución de consultas](#connecting-and-running-queries).

## <a name="using-the-hive-warehouse-connector"></a>Uso del conector de almacén de Hive

### <a name="connecting-and-running-queries"></a>Conexión y la ejecución de consultas

Puede elegir entre varios métodos diferentes para conectarse a su clúster de Interactive Query y ejecutar consultas mediante el conector de almacenamiento de Hive. Métodos admitidos incluyen las siguientes herramientas:

* [spark-shell](../spark/apache-spark-shell.md)
* PySpark
* Spark-submit
* [Zeppelin](../spark/apache-spark-zeppelin-notebook.md)
* [Livy](../spark/apache-spark-livy-rest-interface.md)

Todos los ejemplos proporcionados en este artículo se ejecutará a través del shell de spark.

Para iniciar una sesión de shell de spark, realice los pasos siguientes:

1. SSH en el nodo principal del clúster. Para obtener más información sobre cómo conectarse a su clúster con SSH, consulte [conectar con HDInsight (Apache Hadoop) mediante SSH](../../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).
1. Cambie al directorio correcto escribiendo `cd /usr/hdp/current/hive_warehouse_connector` o proporcionar la ruta de acceso completo a todos los archivos jar que se usan como parámetros en el comando de shell de spark.
1. Escriba el siguiente comando para iniciar el shell de spark:

    ```bash
    spark-shell --master yarn \
    --jars /usr/hdp/3.0.1.0-183/hive_warehouse_connector/hive-warehouse-connector-assembly-1.0.0.3.0.1.0-183.jar \
    --conf spark.security.credentials.hiveserver2.enabled=false
    ```

1. Verá un mensaje de bienvenida y un `scala>` donde puede escribir comandos de símbolo del sistema.

1. Después de iniciar el shell de spark, se puede iniciar una instancia del conector de Hive de almacén mediante los siguientes comandos:

    ```scala
    import com.hortonworks.hwc.HiveWarehouseSession
    val hive = HiveWarehouseSession.session(spark).build()
    ```

### <a name="connecting-and-running-queries-on-enterprise-security-package-esp-clusters"></a>Conectar y ejecutar consultas en los clústeres de paquete de seguridad de la empresa (ESP)

El paquete de seguridad de la empresa (ESP) proporciona funcionalidades de nivel empresarial, como la autenticación basada en Active Directory, compatibilidad con varios usuario y control de acceso basado en roles para los clústeres de Apache Hadoop en HDInsight de Azure. Para obtener más información sobre la ESP, consulte [una introducción a la seguridad de Apache Hadoop con Enterprise Security Package](../domain-joined/apache-domain-joined-introduction.md).

1. Siga los pasos iniciales de 1 y 2 en [conexión y ejecución de consultas](#connecting-and-running-queries).
1. Tipo `kinit` e inicie sesión con un usuario de dominio.
1. Shell de spark inicio con la lista completa de los parámetros de configuración como se muestra a continuación. Todos los valores de todas las letras mayúsculas entre corchetes angulares deben especificarse en función de su clúster. Si necesita averiguar los valores de entrada para cualquiera de los parámetros siguientes, vea la sección sobre [instalación del conector de Hive de almacén](#hive-warehouse-connector-setup).:

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

### <a name="creating-spark-dataframes-from-hive-queries"></a>Creación de tramas de datos de Spark desde consultas de Hive

Los resultados de todas las consultas mediante la biblioteca HWC se devuelven como una trama de datos. Los ejemplos siguientes muestran cómo crear una consulta básica.

```scala
hive.setDatabase("default")
val df = hive.executeQuery("select * from hivesampletable")
df.filter("state = 'Colorado'").show()
```

Los resultados de la consulta son DataFrames de Spark, que puede usarse con bibliotecas de Spark como MLIB y SparkSQL.

### <a name="writing-out-spark-dataframes-to-hive-tables"></a>Escriben DataFrames de Spark en tablas de Hive

Spark no admite la escritura en tablas ACID administradas de Hive de forma nativa. Con HWC, sin embargo, se puede escribir cualquier trama de datos en una tabla de Hive. Puede ver esta funcionalidad en el trabajo en el ejemplo siguiente:

1. Cree una tabla denominada `sampletable_colorado` y especifique sus columnas con el comando siguiente:

    ```scala
    hive.createTable("sampletable_colorado").column("clientid","string").column("querytime","string").column("market","string").column("deviceplatform","string").column("devicemake","string").column("devicemodel","string").column("state","string").column("country","string").column("querydwelltime","double").column("sessionid","bigint").column("sessionpagevieworder","bigint").create()
    ```

2. Filtrar la tabla `hivesampletable` donde la columna `state` es igual a `Colorado`. Esta consulta de la tabla de Hive se devuelve como un DataFrame de Spark. A continuación, la trama de datos se guarda en la tabla de Hive `sampletable_colorado` utilizando el `write` función.
    
    ```scala
    hive.table("hivesampletable").filter("state = 'Colorado'").write.format(HiveWarehouseSession.HIVE_WAREHOUSE_CONNECTOR).option("table","sampletable_colorado").save()
    ```

Puede ver la tabla resultante en la captura de pantalla siguiente.

![Mostrar tabla resultante](./media/apache-hive-warehouse-connector/hive-warehouse-connector-show-hive-table.png)

### <a name="structured-streaming-writes"></a>Escrituras de flujo estructurado

Mediante el conector de Hive de almacén, puede usar Spark streaming para escribir datos en tablas de Hive.

Siga estos pasos para crear un ejemplo de conector de Hive de almacén que ingiere datos de un flujo de Spark en el puerto localhost 9999 en una tabla de Hive.

1. Abra un terminal en su clúster de Spark.
1. Empezar a spark Streaming con el siguiente comando:

    ```scala
    val lines = spark.readStream.format("socket").option("host", "localhost").option("port",9988).load()
    ```

1. Generar datos para el flujo de Spark que creó siguiendo los pasos siguientes:
    1. Abra otro terminal en el mismo clúster de Spark.
    1. En el símbolo del sistema, escriba `nc -lk 9999`. Este comando usa la utilidad netcat para enviar datos desde la línea de comandos para el puerto especificado.
    1. Escriba las palabras que desea que el flujo de Spark a introducir, seguido por un retorno de carro.
        ![datos de entrada de flujo de spark](./media/apache-hive-warehouse-connector/hive-warehouse-connector-spark-stream-data-input.png)
1. Crear una nueva tabla de Hive para almacenar los datos de transmisión por secuencias. En el shell de spark, escriba los siguientes comandos:

    ```scala
    hive.createTable("stream_table").column("value","string").create()
    ```

1. Escribir los datos de streaming en la tabla recién creada con el comando siguiente:

    ```scala
    lines.filter("value = 'HiveSpark'").writeStream.format(HiveWarehouseSession.STREAM_TO_STREAM).option("database", "default").option("table","stream_table").option("metastoreUri",spark.conf.get("spark.datasource.hive.warehouse.metastoreUri")).option("checkpointLocation","/tmp/checkpoint1").start()
    ```

    >[!Important]
    > El `metastoreUri` y `database` opciones actualmente deben establecerse manualmente debido a un problema conocido en Apache Spark. Para obtener más información sobre este problema, consulte [SPARK 25460](https://issues.apache.org/jira/browse/SPARK-25460).

1. Puede ver los datos insertados en la tabla con el siguiente comando:

    ```scala
    hive.table("stream_table").show()
    ```

### <a name="securing-data-on-spark-esp-clusters"></a>Protección de datos en clústeres de Spark ESP

1. Crear una tabla `demo` con algunos datos de ejemplo mediante los comandos siguientes:

    ```scala
    create table demo (name string);
    INSERT INTO demo VALUES ('HDinsight');
    INSERT INTO demo VALUES ('Microsoft');
    INSERT INTO demo VALUES ('InteractiveQuery');
    ```

1. Ver el contenido de la tabla con el siguiente comando. Antes de aplicar la directiva, el `demo` tabla muestra la columna completa.

    ```scala
    hive.executeQuery("SELECT * FROM demo").show()
    ```

    ![tabla de demostración antes de aplicar la directiva de ranger](./media/apache-hive-warehouse-connector/hive-warehouse-connector-table-before-ranger-policy.png)

1. Aplicar una directiva que solo muestra los cuatro últimos caracteres de la columna de enmascaramiento de columna.  
    1. Vaya a la interfaz de usuario del Administrador de Ranger en `https://CLUSTERNAME.azurehdinsight.net/ranger/`.
    1. Haga clic en el servicio de Hive para el clúster en **Hive**.
        ![tabla de demostración antes de aplicar la directiva de ranger](./media/apache-hive-warehouse-connector/hive-warehouse-connector-ranger-service-manager.png)
    1. Haga clic en el **enmascaramiento** ficha y, a continuación, **agregar nueva directiva** ![lista de directivas](./media/apache-hive-warehouse-connector/hive-warehouse-connector-ranger-hive-policy-list.png)
    1. Proporcione un nombre de directiva que desee. Seleccione la base de datos: **Default**, tabla de Hive: **demostración**, columna de Hive: **nombre**, usuario: **rsadmin2**, tipos de acceso: **seleccione**y **Máscara parcial: mostrar último 4** desde el **Seleccionar opción de enmascaramiento** menú. Haga clic en **Agregar**.
                ![lista de directivas](./media/apache-hive-warehouse-connector/hive-warehouse-connector-ranger-create-policy.png)
1. Ver contenido de la tabla de nuevo. Después de aplicar la directiva de ranger, podemos ver solo los cuatro últimos caracteres de la columna.

    ![tabla de demostración después de aplicar la directiva de ranger](./media/apache-hive-warehouse-connector/hive-warehouse-connector-table-after-ranger-policy.png)

## <a name="next-steps"></a>Pasos siguientes

* [Uso de Interactive Query con HDInsight](https://docs.microsoft.com/azure/hdinsight/interactive-query/apache-interactive-query-get-started)
* [Ejemplos de la interacción con el conector de almacén de Hive mediante Zeppelin, Livy, spark-submit y pyspark](https://community.hortonworks.com/articles/223626/integrating-apache-hive-with-apache-spark-hive-war.html)
