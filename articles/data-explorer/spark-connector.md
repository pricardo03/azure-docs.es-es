---
title: Usar el conector de explorador de datos de Azure para Apache Spark para mover datos entre clústeres de Spark y de explorador de datos de Azure.
description: Este tema muestra cómo mover datos entre clústeres de Apache Spark y de explorador de datos de Azure.
author: orspod
ms.author: orspodek
ms.reviewer: michazag
ms.service: data-explorer
ms.topic: conceptual
ms.date: 4/29/2019
ms.openlocfilehash: 8cb1489a0663556f9dd9e6026a036df2468d656d
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64928338"
---
# <a name="azure-data-explorer-connector-for-apache-spark-preview"></a>Conector de datos de Azure Explorer para Apache Spark (versión preliminar)

[Apache Spark](https://spark.apache.org/) es un motor de análisis unificada para el procesamiento de datos a gran escala. El Explorador de datos de Azure es un servicio de análisis de datos rápido, totalmente administrado para análisis en tiempo real en grandes volúmenes de datos. 

Conector del explorador de datos de Azure para Spark implementa el origen de datos y el receptor de datos para mover los datos en el Explorador de datos de Azure y Spark clústeres a usar ambos de sus capacidades. Con Azure Data Explorer y Apache Spark, se pueden crear aplicaciones rápidas y escalables orientadas a escenarios controlados por datos, tales como el aprendizaje automático, la extracción, transformación y carga de datos (ETL), y Log Analytics. Escribir en el Explorador de datos de Azure puede realizarse en modo de transmisión por secuencias y por lotes.
Lectura desde el Explorador de datos de Azure admite la eliminación de columnas y la aplicación del predicado, lo que reduce el volumen de datos transferidos mediante el filtrado de datos en el Explorador de datos de Azure.

Conector de Spark del explorador de datos de Azure es un [proyecto de código abierto](https://github.com/Azure/azure-kusto-spark) que se pueden ejecutar en cualquier clúster de Spark.

> [!NOTE]
> Aunque algunos de los ejemplos siguientes se refieren a una [Azure Databricks](https://docs.azuredatabricks.net/) clúster de Spark, conector de Spark de explorador de datos de Azure no tiene dependencias directas en Databricks o cualquier otra distribución de Spark.

## <a name="prerequisites"></a>Requisitos previos

* [Creación de un clúster y de la base de datos de Azure Data Explorer](/azure/data-explorer/create-cluster-database-portal) 
* Creación de un clúster de Spark
* Instalar biblioteca de conectores del explorador de datos de Azure y las bibliotecas que aparecen en [dependencias](https://github.com/Azure/azure-kusto-spark#dependencies) incluidos los siguientes [SDK de Java de Kusto](/azure/kusto/api/java/kusto-java-client-library) bibliotecas:
    * [Kusto Data Client](https://mvnrepository.com/artifact/com.microsoft.azure.kusto/kusto-data)
    * [Kusto Ingest Client](https://mvnrepository.com/artifact/com.microsoft.azure.kusto/kusto-ingest)
* Bibliotecas para incorporadas [Spark 2.4, Scala 2.11](https://github.com/Azure/azure-kusto-spark/releases)

## <a name="how-to-build-the-spark-connector"></a>Cómo crear el conector de Spark

Conector de Spark se pueden compilar desde [orígenes](https://github.com/Azure/azure-kusto-spark) tal como se detalla a continuación.

> [!NOTE]
> Este paso es opcional. Si estás usando bibliotecas pregeneradas vaya a [el programa de instalación de clúster de Spark](#spark-cluster-setup).

### <a name="build-prerequisites"></a>Requisitos previos de compilación

* SDK de Java 1.8 instalado
* [Maven 3.x](https://maven.apache.org/download.cgi) instalado
* Versión de Apache Spark 2.4.0 o superior

> [!TIP]
> las versiones 2.3. x también se admiten, pero pueden requerir algunos cambios en pom.xml dependencias.

Para aplicaciones de Java/Scala con las definiciones del proyecto de Maven, vincule la aplicación con el siguiente artefacto (última versión puede variar):

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

Para obtener más información, consulte [uso del conector](https://github.com/Azure/azure-kusto-spark#usage).

## <a name="spark-cluster-setup"></a>Instalación de clúster de Spark

> [!NOTE]
> Se recomienda usar la última versión del conector de Spark de explorador de datos de Azure al realizar los pasos siguientes:

1. Establecer configuración de clúster, en función de clúster de Azure Databricks mediante Spark 2.4 y Scala 2.11 de Spark siguiente: 

    ![Configuración de clúster de Databricks](media/spark-connector/databricks-cluster.png)

1. Importación de la biblioteca del conector de explorador de datos de Azure:

    ![Biblioteca de importación Explorador de datos de Azure](media/spark-connector/db-create-library.png)

1. Agregar dependencias adicionales:

    ![Adición de dependencias](media/spark-connector/db-dependencies.png)

    > [!TIP]
    > Se encuentra la versión correcta de java para cada versión de Spark [aquí](https://github.com/Azure/azure-kusto-spark#dependencies).

1. Compruebe que todos los necesarios se instalan las bibliotecas:

    ![Comprobar las bibliotecas instaladas](media/spark-connector/db-libraries-view.png)

## <a name="authentication"></a>Authentication

Conector de Spark del explorador de datos de Azure le permite autenticarse con Azure Active Directory (Azure AD) mediante un [aplicación de Azure AD](#azure-ad-application-authentication), [token de acceso de Azure AD](https://github.com/Azure/azure-kusto-spark/blob/dev/docs/Authentication.md#direct-authentication-with-access-token), [autenticación de dispositivos ](https://github.com/Azure/azure-kusto-spark/blob/dev/docs/Authentication.md#device-authentication) (para escenarios que no sea de producción), o [Azure Key Vault](https://github.com/Azure/azure-kusto-spark/blob/dev/docs/Authentication.md#key-vault). El usuario debe instalar el paquete azure-keyvault y proporcionar credenciales de la aplicación para tener acceso al recurso de Key Vault.

### <a name="azure-ad-application-authentication"></a>Autenticación de aplicación de Azure AD

Método de autenticación sencillo y común mayoría. Se recomienda este método para el uso del conector de Spark de explorador de datos de Azure.

|Properties (Propiedades)  |DESCRIPCIÓN  |
|---------|---------|
|**KUSTO_AAD_CLIENT_ID**     |   Identificador de aplicación (cliente) de Azure AD.      |
|**KUSTO_AAD_AUTHORITY_ID**     |  Autoridad de autenticación de Azure AD. Identificador de directorio de AD (inquilino de) Azure.        |
|**KUSTO_AAD_CLIENT_PASSWORD**    |    Clave de aplicación de Azure AD para el cliente.     |

### <a name="azure-data-explorer-privileges"></a>Privilegios de explorador de datos de Azure

Deben tener los privilegios siguientes en un clúster de explorador de datos de Azure:

* Para leer (origen de datos), debe tener la aplicación de Azure AD *Visor* privilegios en la base de datos de destino, o *admin* privilegios en la tabla de destino.
* Para escribir (receptor de datos), debe tener la aplicación de Azure AD *introductor* privilegios en la base de datos de destino. También debe tener *usuario* privilegios en la base de datos de destino para crear nuevas tablas. Si la tabla de destino ya existe, *admin* privilegios en la tabla de destino se pueden configurar.
 
Para obtener más información sobre los roles principales del explorador de datos de Azure, consulte [autorización basada en roles](/azure/kusto/management/access-control/role-based-authorization). Para administrar roles de seguridad, consulte [administración de roles de seguridad](/azure/kusto/management/security-roles).

## <a name="spark-sink-writing-to-azure-data-explorer"></a>Receptor de Spark: Escribir en el Explorador de datos de Azure

1. Configurar los parámetros de receptor:

     ```scala
    val KustoSparkTestAppId = dbutils.secrets.get(scope = "KustoDemos", key = "KustoSparkTestAppId")
    val KustoSparkTestAppKey = dbutils.secrets.get(scope = "KustoDemos", key = "KustoSparkTestAppKey")
 
    val appId = KustoSparkTestAppId
    val appKey = KustoSparkTestAppKey
    val authorityId = "72f988bf-86f1-41af-91ab-2d7cd011db47"
    val cluster = "Sparktest.eastus2"
    val database = "TestDb"
    val table = "StringAndIntTable"
    ```

1. Escribir DataFrame de Spark en clústeres del explorador de datos de Azure como lote:

    ```scala
    df.write
      .format("com.microsoft.kusto.spark.datasource")
      .option(KustoOptions.KUSTO_CLUSTER, cluster)
      .option(KustoOptions.KUSTO_DATABASE, database)
      .option(KustoOptions.KUSTO_TABLE, table)
      .option(KustoOptions.KUSTO_AAD_CLIENT_ID, appId)
      .option(KustoOptions.KUSTO_AAD_CLIENT_PASSWORD, appKey) 
      .option(KustoOptions.KUSTO_AAD_AUTHORITY_ID, authorityId)
      .save()
    ```

1. Escribir datos de streaming:

    ```scala    
    import org.apache.spark.sql.streaming.Trigger
    import java.util.concurrent.TimeUnit
    
    // Set up a checkpoint and disable codeGen. Set up a checkpoint and disable codeGen as a workaround for an known issue 
    spark.conf.set("spark.sql.streaming.checkpointLocation", "/FileStore/temp/checkpoint")
    spark.conf.set("spark.sql.codegen.wholeStage","false")
    
    // Write to a Kusto table fro streaming source
    val kustoQ = csvDf
          .writeStream
          .format("com.microsoft.kusto.spark.datasink.KustoSinkProvider")
          .options(Map(
            KustoOptions.KUSTO_CLUSTER -> cluster,
            KustoOptions.KUSTO_TABLE -> table,
            KustoOptions.KUSTO_DATABASE -> database,
            KustoOptions.KUSTO_AAD_CLIENT_ID -> appId,
            KustoOptions.KUSTO_AAD_CLIENT_PASSWORD -> appKey,
            KustoOptions.KUSTO_AAD_AUTHORITY_ID -> authorityId))
          .trigger(Trigger.Once)
    
    kustoQ.start().awaitTermination(TimeUnit.MINUTES.toMillis(8))
    ```

## <a name="spark-source-reading-from-azure-data-explorer"></a>Código fuente de Spark: Lectura desde el Explorador de datos de Azure

1. Al leer pequeñas cantidades de datos, defina la consulta de datos:

    ```scala
    val conf: Map[String, String] = Map(
          KustoOptions.KUSTO_AAD_CLIENT_ID -> appId,
          KustoOptions.KUSTO_AAD_CLIENT_PASSWORD -> appKey,
          KustoOptions.KUSTO_QUERY -> s"$table | where (ColB % 1000 == 0) | distinct ColA"      
        )
    
    // Simplified syntax flavor
    import org.apache.spark.sql._
    import com.microsoft.kusto.spark.sql.extension.SparkExtension._
    import org.apache.spark.SparkConf
    
    val df = spark.read.kusto(cluster, database, "", conf)
    display(df)
    ```

1. Cuando se leen grandes cantidades de datos, se debe proporcionar el almacenamiento de blobs transitorios. Proporcione la clave SAS del contenedor de almacenamiento, o nombre de la cuenta de almacenamiento, clave de cuenta y nombre del contenedor. 

    ```scala
    // Use either container/account-key/account name, or container SaS
    val container = dbutils.secrets.get(scope = "KustoDemos", key = "blobContainer")
    val storageAccountKey = dbutils.secrets.get(scope = "KustoDemos", key = "blobStorageAccountKey")
    val storageAccountName = dbutils.secrets.get(scope = "KustoDemos", key = "blobStorageAccountName")
    // val storageSas = dbutils.secrets.get(scope = "KustoDemos", key = "blobStorageSasUrl")
    ```

    En el ejemplo anterior, no se acceder a Key Vault mediante la interfaz del conector. Como alternativa, usamos un método más sencillo de usar los secretos de Databricks.

1. Lea desde el Explorador de datos de Azure:

    ```scala
    val df2 = spark.read.kusto(cluster, database, "ReallyBigTable", conf3)
    
    val dfFiltered = df2
      .where(df2.col("ColA").startsWith("row-2"))
      .filter("ColB > 12")
      .filter("ColB <= 21")
      .select("ColA")
    
    display(dfFiltered)
    ```
