---
title: Use el conector de Azure Data Explorer para Apache Spark para mover datos entre clústeres de Azure Data Explorer y Spark.
description: En este tema se muestra cómo mover datos entre clústeres de Azure Data Explorer y Apache Spark.
author: orspod
ms.author: orspodek
ms.reviewer: michazag
ms.service: data-explorer
ms.topic: conceptual
ms.date: 1/14/2020
ms.openlocfilehash: 868e9e068244af91e218d906bee115b58906152f
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2020
ms.locfileid: "76027927"
---
# <a name="azure-data-explorer-connector-for-apache-spark-preview"></a>Conector de Azure Data Explorer para Apache Spark (versión preliminar)

[Apache Spark](https://spark.apache.org/) es un motor de análisis unificado para el procesamiento de datos a gran escala. Azure Data Explorer es un servicio de análisis de datos rápido y totalmente administrado que permite analizar grandes volúmenes de datos en tiempo real. 

El conector de Azure Data Explorer para Spark implementa el origen y el receptor de datos para mover datos entre clústeres de Azure Data Explorer y Spark y utilizar sus capacidades. Con Azure Data Explorer y Apache Spark, se pueden crear aplicaciones rápidas y escalables orientadas a escenarios controlados por datos, tales como el aprendizaje automático, la extracción, transformación y carga de datos (ETL), y Log Analytics. La escritura en Azure Data Explorer puede realizarse en modo de transmisión por secuencias y por lotes.
La lectura desde Azure Data Explorer admite la eliminación de columnas y la aplicación del predicado, lo que reduce el volumen de datos transferidos mediante el filtrado de datos en Azure Data Explorer.

El conector de Spark de Azure Data Explorer es un [proyecto de código abierto](https://github.com/Azure/azure-kusto-spark) que se puede ejecutar en cualquier clúster de Spark. El conector de Spark de Azure Data Explorer convierte a Azure Data Explorer en un almacén de datos válido para las operaciones estándar de origen y receptor de Spark, como escritura, lectura y writeStream. 

> [!NOTE]
> Aunque algunos de los ejemplos siguientes se refieren a un clúster de Spark de [Azure Databricks](https://docs.azuredatabricks.net/), el conector de Spark de Azure Data Explorer no tiene dependencias directas en Databricks ni en ninguna otra distribución de Spark.

## <a name="prerequisites"></a>Prerequisites

* [Creación de un clúster y de la base de datos de Azure Data Explorer](/azure/data-explorer/create-cluster-database-portal) 
* Creación de un clúster de Spark
* Instale la biblioteca de conectores de Azure Data Explorer y las bibliotecas que aparecen en las [dependencias](https://github.com/Azure/azure-kusto-spark#dependencies), incluidas las siguientes bibliotecas de [SDK de Kusto Java](/azure/kusto/api/java/kusto-java-client-library):
    * [Cliente de datos de Kusto](https://mvnrepository.com/artifact/com.microsoft.azure.kusto/kusto-data)
    * [Cliente de ingesta de Kusto](https://mvnrepository.com/artifact/com.microsoft.azure.kusto/kusto-ingest)
* Bibliotecas pregeneradas para [Spark 2.4, Scala 2.11](https://github.com/Azure/azure-kusto-spark/releases) y el [repositorio de Maven](https://mvnrepository.com/artifact/com.microsoft.azure.kusto/spark-kusto-connector)

## <a name="how-to-build-the-spark-connector"></a>Cómo crear el conector de Spark

El conector de Spark se puede crear desde [orígenes](https://github.com/Azure/azure-kusto-spark), tal como se detalla a continuación.

> [!NOTE]
> Este paso es opcional. Si utiliza bibliotecas pregeneradas, vaya a [Instalación de clúster de Spark](#spark-cluster-setup).

### <a name="build-prerequisites"></a>Requisitos previos de compilación

* SDK de Java 1.8 instalado
* [Maven 3.x](https://maven.apache.org/download.cgi) instalado
* Apache Spark versión 2.4.0 o posterior

> [!TIP]
> Las versiones 2.3.x también se admiten, pero pueden requerir algunos cambios en las dependencias pom.xml.

Para las aplicaciones Scala y Java que utilizan definiciones de proyecto Maven, vincule la aplicación con el siguiente artefacto (la última versión puede ser diferente):

```Maven
   <dependency>
     <groupId>com.microsoft.azure</groupId>
     <artifactId>spark-kusto-connector</artifactId>
     <version>1.0.0-Beta-02</version>
   </dependency>
```

### <a name="build-commands"></a>Comandos de compilación

Para compilar el archivo jar y ejecutar todas las pruebas:

```
mvn clean package
```

Para compilar el archivo jar, ejecute todas las pruebas e instale jar en el repositorio de Maven local:

```
mvn clean install
```

Para obtener más información, consulte [Uso de conectores](https://github.com/Azure/azure-kusto-spark#usage).

## <a name="spark-cluster-setup"></a>Instalación del clúster de Spark

> [!NOTE]
> Se recomienda usar la última versión del conector de Spark de Azure Data Explorer al realizar los pasos siguientes:

1. Establezca la siguiente configuración del clúster de Spark, en función del clúster de Azure Databricks, mediante Spark 2.4.4 y Scala 2.11: 

    ![Configuración del clúster de Databricks](media/spark-connector/databricks-cluster.png)
    
1. Instale la biblioteca spark-kusto-connector más reciente de Maven:

    ![Importación de la biblioteca de Azure Data Explorer](media/spark-connector/db-create-library.png)

1. Compruebe que están instaladas todas las bibliotecas necesarias:

    ![Comprobación de las bibliotecas instaladas](media/spark-connector/db-libraries-view.png)

## <a name="authentication"></a>Authentication

El conector de Spark de Azure Data Explorer le permite autenticarse con Azure Active Directory (Azure AD) mediante una [aplicación de Azure AD](#azure-ad-application-authentication), un [token de acceso de Azure AD](https://github.com/Azure/azure-kusto-spark/blob/dev/docs/Authentication.md#direct-authentication-with-access-token), una [autenticación de dispositivos ](https://github.com/Azure/azure-kusto-spark/blob/dev/docs/Authentication.md#device-authentication) (para escenarios que no sean de producción) o [Azure Key Vault](https://github.com/Azure/azure-kusto-spark/blob/dev/docs/Authentication.md#key-vault). El usuario debe instalar el paquete azure-keyvault y proporcionar credenciales de la aplicación para tener acceso al recurso de Key Vault.

### <a name="azure-ad-application-authentication"></a>Autenticación de la aplicación de Azure AD

El método de autenticación más sencillo y habitual. Se recomienda este método para usar el conector de Spark de Azure Data Explorer.

|Propiedades  |Descripción  |
|---------|---------|
|**KUSTO_AAD_CLIENT_ID**     |   Identificador de la aplicación de Azure AD (cliente).      |
|**KUSTO_AAD_AUTHORITY_ID**     |  Autoridad de autenticación de Azure AD. Identificador de Azure AD Directory (inquilino).        |
|**KUSTO_AAD_CLIENT_PASSWORD**    |    Clave de aplicación de Azure AD para el cliente.     |

### <a name="azure-data-explorer-privileges"></a>Privilegios de Azure Data Explorer

Un clúster de Azure Data Explorer debe tener los privilegios siguientes:

* Para lectura (origen de datos), la aplicación de Azure AD debe tener privilegios de *visor* en la base de datos de destino o de *administrador* en la tabla de destino.
* Para escritura (receptor de datos), la aplicación de Azure AD debe tener privilegios de *agente de ingesta* en la base de datos de destino. También debe tener privilegios de *usuario* en la base de datos de destino para crear nuevas tablas. Si la tabla de destino ya existe, se pueden configurar privilegios de *administrador* en la tabla de destino.
 
Para obtener más información sobre los roles principales de Azure Data Explorer, consulte [Autorización basada en roles](/azure/kusto/management/access-control/role-based-authorization). Para administrar los roles de seguridad, consulte [Administración de los roles de seguridad](/azure/kusto/management/security-roles).

## <a name="spark-sink-writing-to-azure-data-explorer"></a>Receptor de Spark: escritura en Azure Data Explorer

1. Configuración de los parámetros del receptor:

     ```scala
    val KustoSparkTestAppId = dbutils.secrets.get(scope = "KustoDemos", key = "KustoSparkTestAppId")
    val KustoSparkTestAppKey = dbutils.secrets.get(scope = "KustoDemos", key = "KustoSparkTestAppKey")
 
    val appId = KustoSparkTestAppId
    val appKey = KustoSparkTestAppKey
    val authorityId = "72f988bf-86f1-41af-91ab-2d7cd011db47" // Optional - defaults to microsoft.com
    val cluster = "Sparktest.eastus2"
    val database = "TestDb"
    val table = "StringAndIntTable"
    ```

1. Escriba DataFrame de Spark en clústeres de Azure Data Explorer como lote:

    ```scala
    import com.microsoft.kusto.spark.datasink.KustoSinkOptions
    import org.apache.spark.sql.{SaveMode, SparkSession}

    df.write
      .format("com.microsoft.kusto.spark.datasource")
      .option(KustoSinkOptions.KUSTO_CLUSTER, cluster)
      .option(KustoSinkOptions.KUSTO_DATABASE, database)
      .option(KustoSinkOptions.KUSTO_TABLE, "Demo3_spark")
      .option(KustoSinkOptions.KUSTO_AAD_CLIENT_ID, appId)
      .option(KustoSinkOptions.KUSTO_AAD_CLIENT_PASSWORD, appKey)
      .option(KustoSinkOptions.KUSTO_AAD_AUTHORITY_ID, authorityId)
      .option(KustoSinkOptions.KUSTO_TABLE_CREATE_OPTIONS, "CreateIfNotExist")
      .mode(SaveMode.Append)
      .save()  
    ```
    
   O bien, use la sintaxis simplificada:
   
    ```scala
         import com.microsoft.kusto.spark.datasink.SparkIngestionProperties
         import com.microsoft.kusto.spark.sql.extension.SparkExtension._
         
         val sparkIngestionProperties = Some(new SparkIngestionProperties()) // Optional, use None if not needed
         df.write.kusto(cluster, database, table, conf, sparkIngestionProperties)
    ```
   
1. Escriba datos de streaming:

    ```scala    
    import org.apache.spark.sql.streaming.Trigger
    import java.util.concurrent.TimeUnit
    import java.util.concurrent.TimeUnit
    import org.apache.spark.sql.streaming.Trigger

    // Set up a checkpoint and disable codeGen. Set up a checkpoint and disable codeGen as a workaround for an known issue 
    spark.conf.set("spark.sql.streaming.checkpointLocation", "/FileStore/temp/checkpoint")
    spark.conf.set("spark.sql.codegen.wholeStage","false") // Use in case a NullPointerException is thrown inside codegen iterator
    
    // Write to a Kusto table from a streaming source
    val kustoQ = df
          .writeStream
          .format("com.microsoft.kusto.spark.datasink.KustoSinkProvider")
          .options(conf) 
          .option(KustoSinkOptions.KUSTO_WRITE_ENABLE_ASYNC, "true") // Optional, better for streaming, harder to handle errors
          .trigger(Trigger.ProcessingTime(TimeUnit.SECONDS.toMillis(10))) // Sync this with the ingestionBatching policy of the database
          .start()
    ```

## <a name="spark-source-reading-from-azure-data-explorer"></a>Origen de Spark: lectura desde Azure Data Explorer

1. Al leer pequeñas cantidades de datos, defina la consulta de datos:

    ```scala
    import com.microsoft.kusto.spark.datasource.KustoSourceOptions
    import org.apache.spark.SparkConf
    import org.apache.spark.sql._
    import com.microsoft.azure.kusto.data.ClientRequestProperties

    val query = s"$table | where (ColB % 1000 == 0) | distinct ColA"
    val conf: Map[String, String] = Map(
          KustoSourceOptions.KUSTO_AAD_CLIENT_ID -> appId,
          KustoSourceOptions.KUSTO_AAD_CLIENT_PASSWORD -> appKey
        )

    val df = spark.read.format("com.microsoft.kusto.spark.datasource").
      options(conf).
      option(KustoSourceOptions.KUSTO_QUERY, query).
      option(KustoSourceOptions.KUSTO_DATABASE, database).
      option(KustoSourceOptions.KUSTO_CLUSTER, cluster).
      load()

    // Simplified syntax flavor
    import com.microsoft.kusto.spark.sql.extension.SparkExtension._
    
    val cpr: Option[ClientRequestProperties] = None // Optional
    val df2 = spark.read.kusto(cluster, database, query, conf, cpr)
    display(df2)
    ```

1. Al leer grandes cantidades de datos, se debe proporcionar almacenamiento de blobs transitorio. Proporcione la clave SAS del contenedor de almacenamiento, o el nombre de la cuenta de almacenamiento, la clave de la cuenta y el nombre del contenedor. Este paso solo es necesario para la versión preliminar actual del conector de Spark.

    ```scala
    // Use either container/account-key/account name, or container SaS
    val container = dbutils.secrets.get(scope = "KustoDemos", key = "blobContainer")
    val storageAccountKey = dbutils.secrets.get(scope = "KustoDemos", key = "blobStorageAccountKey")
    val storageAccountName = dbutils.secrets.get(scope = "KustoDemos", key = "blobStorageAccountName")
    // val storageSas = dbutils.secrets.get(scope = "KustoDemos", key = "blobStorageSasUrl")
    ```

    En el ejemplo anterior, no se accede a Key Vault mediante la interfaz del conector. Como alternativa, se utiliza un método más sencillo de usar los secretos de Databricks.

1. Lectura desde Azure Data Explorer:

    ```scala
     val conf3 = Map(
          KustoSourceOptions.KUSTO_AAD_CLIENT_ID -> appId,
          KustoSourceOptions.KUSTO_AAD_CLIENT_PASSWORD -> appKey
          KustoSourceOptions.KUSTO_BLOB_STORAGE_SAS_URL -> storageSas)
    val df2 = spark.read.kusto(cluster, database, "ReallyBigTable", conf3)
    
    val dfFiltered = df2
      .where(df2.col("ColA").startsWith("row-2"))
      .filter("ColB > 12")
      .filter("ColB <= 21")
      .select("ColA")
    
    display(dfFiltered)
    ```

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre el [conector de Azure Data Explorer para Spark](https://github.com/Azure/azure-kusto-spark/tree/master/docs)
* [Código de ejemplo](https://github.com/Azure/azure-kusto-spark/tree/master/samples/src/main)

