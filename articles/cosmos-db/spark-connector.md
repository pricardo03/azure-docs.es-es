---
title: Conexión de Apache Spark a Azure Cosmos DB
description: Aprenda sobre el conector de Azure Cosmos DB Spark que le permite conectar Apache Spark a Azure Cosmos DB.
author: tknandu
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/21/2019
ms.author: ramkris
ms.openlocfilehash: 60afd8128224050d456699e798d814a259e106ae
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2019
ms.locfileid: "59543828"
---
# <a name="accelerate-big-data-analytics-by-using-the-apache-spark-to-azure-cosmos-db-connector"></a>Aceleración de análisis de macrodatos mediante el conector de Apache Spark a Azure Cosmos DB

Puede ejecutar [Spark](https://spark.apache.org/) trabajos con los datos almacenados en Azure Cosmos DB mediante el conector de Cosmos DB Spark. COSMOS puede usarse para el lote y el procesamiento de flujos y como una capa de servicio para el acceso de baja latencia.

Puede usar el conector con [Azure Databricks](https://azure.microsoft.com/services/databricks) o [HDInsight de Azure](https://azure.microsoft.com/services/hdinsight/), que proporcionan los clústeres de Spark administrados en Azure. La siguiente tabla muestra las versiones de Spark compatible.

| Componente | Versión |
|---------|-------|
| Spark de Apache | 2.4. x, 2.3. x, 2.1 y 2.2. x |
| Scala | 2.11 |
| Versión del runtime de Azure Databricks | > 3.4 |

> [!WARNING]
> Este conector es compatible con la API principal (SQL) de Azure Cosmos DB.
> Para Cosmos DB para MongoDB API, use el [conector MongoDB Spark](https://docs.mongodb.com/spark-connector/master/).
> Para la API de Cassandra de Cosmos DB, utilice el [conector Cassandra Spark](https://github.com/datastax/spark-cassandra-connector).
>

## <a name="quickstart"></a>Guía de inicio rápido

* Siga los pasos de [empezar a trabajar con el SDK de Java](sql-api-async-java-get-started.md) para configurar una cuenta de Cosmos DB y rellenar algunos datos.
* Siga los pasos de [Azure Databricks Introducción](https://docs.azuredatabricks.net/getting-started/index.html) para configurar un área de trabajo de Azure Databricks y el clúster.
* Ahora puede crear nuevos cuadernos e importar la biblioteca del conector de Cosmos DB. Saltar a [trabajar con el conector de Cosmos DB](#bk_working_with_connector) para obtener más información sobre cómo configurar el área de trabajo.
* La siguiente sección tiene fragmentos de código sobre cómo leer y escribir mediante el conector.

### <a name="reading-from-cosmos-db"></a>Lectura desde Cosmos DB

El fragmento de código siguiente muestra cómo crear una trama de datos de Spark para leer desde Cosmos DB en PySpark.

```python
# Read Configuration
readConfig = {
  "Endpoint" : "https://doctorwho.documents.azure.com:443/",
  "Masterkey" : "YOUR-KEY-HERE",
  "Database" : "DepartureDelays",
  "Collection" : "flights_pcoll",
  "query_custom" : "SELECT c.date, c.delay, c.distance, c.origin, c.destination FROM c WHERE c.origin = 'SEA'" // Optional
}

# Connect via azure-cosmosdb-spark to create Spark DataFrame
flights = spark.read.format("com.microsoft.azure.cosmosdb.spark").options(**readConfig).load()
flights.count()
```

Y el mismo fragmento de código de Scala:

```scala
// Import Necessary Libraries
import com.microsoft.azure.cosmosdb.spark.schema._
import com.microsoft.azure.cosmosdb.spark._
import com.microsoft.azure.cosmosdb.spark.config.Config

// Configure connection to your collection
val readConfig = Config(Map(
  "Endpoint" -> "https://doctorwho.documents.azure.com:443/",
  "Masterkey" -> "YOUR-KEY-HERE",
  "Database" -> "DepartureDelays",
  "Collection" -> "flights_pcoll",
  "query_custom" -> "SELECT c.date, c.delay, c.distance, c.origin, c.destination FROM c WHERE c.origin = 'SEA'" // Optional
))

// Connect via azure-cosmosdb-spark to create Spark DataFrame
val flights = spark.read.cosmosDB(readConfig)
flights.count()
```

### <a name="writing-to-cosmos-db"></a>Escribir en Cosmos DB

El fragmento de código siguiente muestra cómo escribir una trama de datos en Cosmos DB en PySpark.

```python
# Write configuration
writeConfig = {
 "Endpoint" : "https://doctorwho.documents.azure.com:443/",
 "Masterkey" : "YOUR-KEY-HERE",
 "Database" : "DepartureDelays",
 "Collection" : "flights_fromsea",
 "Upsert" : "true"
}

# Write to Cosmos DB from the flights DataFrame
flights.write.format("com.microsoft.azure.cosmosdb.spark").options(**writeConfig).save()
```

Y el mismo fragmento de código de Scala:

```scala
// Configure connection to the sink collection
val writeConfig = Config(Map(
  "Endpoint" -> "https://doctorwho.documents.azure.com:443/",
  "Masterkey" -> "YOUR-KEY-HERE",
  "Database" -> "DepartureDelays",
  "Collection" -> "flights_fromsea",
  "Upsert" : "true"
))

// Upsert the dataframe to Cosmos DB
import org.apache.spark.sql.SaveMode
flights.write.mode(SaveMode.Overwrite).cosmosDB(writeConfig)
```

Obtener más fragmentos de código y ejemplos de extremo a extremo, vea [Jupyter](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples/notebooks).

## <a name="bk_working_with_connector"></a> Trabajar con el conector

Puede crear el conector de origen en Github o descargue los archivos de uber JAR de Maven en los vínculos siguientes.

| Spark | Scala | La versión más reciente |
|---|---|---|
| 2.4.0 | 2.11 | [azure-cosmosdb-spark_2.4.0_2.11_1.3.5](https://search.maven.org/artifact/com.microsoft.azure/azure-cosmosdb-spark_2.4.0_2.11/1.3.5/jar)
| 2.3.0 | 2.11 | [azure-cosmosdb-spark_2.3.0_2.11_1.3.3](https://search.maven.org/artifact/com.microsoft.azure/azure-cosmosdb-spark_2.3.0_2.11/1.3.3/jar)
| 2.2.0 | 2.11 | [azure-cosmosdb-spark_2.2.0_2.11_1.1.1](https://search.maven.org/#artifactdetails%7Ccom.microsoft.azure%7Cazure-cosmosdb-spark_2.2.0_2.11%7C1.1.1%7Cjar)
| 2.1.0 | 2.11 | [azure-cosmosdb-spark_2.1.0_2.11_1.2.2](https://search.maven.org/artifact/com.microsoft.azure/azure-cosmosdb-spark_2.1.0_2.11/1.2.2/jar)

### <a name="using-databricks-notebooks"></a>Uso de cuadernos de Databricks

Crear una biblioteca mediante el área de trabajo de Databricks siguiendo las instrucciones de la Guía de Azure Databricks > [usar el conector de Spark para Azure Cosmos DB](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/cosmosdb-connector.html)

> [!NOTE]
> Tenga en cuenta que el **usar el conector de Azure Cosmos DB Spark** página actualmente no está actualizada. En lugar de descargar los archivos JAR independientes seis en seis bibliotecas diferentes, puede descargar el archivo uber-jar de maven en https://search.maven.org/artifact/com.microsoft.azure/azure-cosmosdb-spark_2.4.0_2.11/1.3.5/jar) e instalar esta biblioteca/jar uno.
> 

### <a name="using-spark-cli"></a>Mediante la cli de spark

Para trabajar con el conector mediante la cli de spark (es decir, `spark-shell`, `pyspark`, `spark-submit`), puede usar el `--packages` parámetro con el conector [coordenadas maven](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb-spark_2.4.0_2.11).

```sh
spark-shell --master yarn --packages "com.microsoft.azure:azure-cosmosdb-spark_2.4.0_2.11:1.3.5"

```

### <a name="using-jupyter-notebooks"></a>Uso de Jupyter notebook

Si usa Jupyter notebooks en HDInsight, puede usar spark magic `%%configure` celda para especificar las coordenadas de maven del conector.

```python
{ "name":"Spark-to-Cosmos_DB_Connector",
  "conf": {
    "spark.jars.packages": "com.microsoft.azure:azure-cosmosdb-spark_2.4.0_2.11:1.3.5",
    "spark.jars.excludes": "org.scala-lang:scala-reflect"
   }
   ...
}
```

> Tenga en cuenta, la inclusión de la `spark.jars.excludes` es específico para quitar los conflictos potenciales entre el conector, Apache Spark y Livy.

### <a name="build-the-connector"></a>Crear el conector

Actualmente, este proyecto del conector usa `maven` para compilar sin dependencias, que puede ejecutar:

```sh
mvn clean package
```

## <a name="working-with-our-samples"></a>Trabajar con nuestros ejemplos

El [repositorio de GitHub de Cosmos DB Spark](https://github.com/Azure/azure-cosmosdb-spark) tiene scripts que puede probar los cuadernos de ejemplo siguiente.

* **Rendimiento puntual de vuelos con Spark y Cosmos DB (Seattle)** [ipynb](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/On-Time%20Flight%20Performance%20with%20Spark%20and%20Cosmos%20DB%20-%20Seattle.ipynb) | [html](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/On-Time%20Flight%20Performance%20with%20Spark%20and%20Cosmos%20DB%20-%20Seattle.html): Conectar Spark a Cosmos DB mediante el servicio de HDInsight Jupyter notebook para presentar Spark SQL, GraphFrames y predecir retrasos en vuelos con canalizaciones de aprendizaje automático.
* **[Conexión de Spark con fuente de cambios de Cosmos DB](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Spark%2Band%2BCosmos%2BDB%2BChange%2BFeed.ipynb)**: Una presentación rápida sobre cómo conectar Spark a la fuente de cambios de Cosmos DB.
* **Origen con Apache Spark y Azure Cosmos DB la fuente de cambios de Twitter**: [ipynb](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Twitter%20with%20Spark%20and%20Azure%20Cosmos%20DB%20Change%20Feed.ipynb) | [html](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Twitter%20with%20Spark%20and%20Azure%20Cosmos%20DB%20Change%20Feed.html)
* **Uso de Apache Spark para consultar Cosmos DB gráficos**: [ipynb](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Using%20Apache%20Spark%20to%20query%20Cosmos%20DB%20Graphs.ipynb) | [html](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Using%20Apache%20Spark%20to%20query%20Cosmos%20DB%20Graphs.html)
* **[Conectar Azure Databricks a Azure Cosmos DB](https://docs.databricks.com/spark/latest/data-sources/azure/cosmosdb-connector.html)**  mediante `azure-cosmosdb-spark`.  Vincular aquí también es una versión de Azure Databricks de la [Bloc de notas de rendimiento de vuelos puntuales](https://github.com/dennyglee/databricks/tree/master/notebooks/Users/denny%40databricks.com/azure-databricks).
* **[Arquitectura lambda con Azure Cosmos DB y HDInsight (Apache Spark)](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/readme.md)**: Puede reducir la sobrecarga operativa de mantenimiento de las canalizaciones de macrodatos con Cosmos DB y Spark.

## <a name="more-information"></a>Más información

Contamos con más información el `azure-cosmosdb-spark` [wiki](https://github.com/Azure/azure-cosmosdb-spark/wiki) incluidos:

* [Guía de usuario del conector de Azure Cosmos DB Spark](https://github.com/Azure/azure-documentdb-spark/wiki/Azure-Cosmos-DB-Spark-Connector-User-Guide)
* [Ejemplos de agregaciones](https://github.com/Azure/azure-documentdb-spark/wiki/Aggregations-Examples)

### <a name="configuration-and-setup"></a>Instalación y configuración

* [Configuración del conector de Spark](https://github.com/Azure/azure-cosmosdb-spark/wiki/Configuration-references)
* [Instalación del conector de Cosmos DB Spark](https://github.com/Azure/azure-documentdb-spark/wiki/Spark-to-Cosmos-DB-Connector-Setup) (en curso)
* [Configuración de consulta de Power BI directamente a Azure Cosmos DB a través de Apache Spark (HDI)](https://github.com/Azure/azure-cosmosdb-spark/wiki/Configuring-Power-BI-Direct-Query-to-Azure-Cosmos-DB-via-Apache-Spark-(HDI))

### <a name="troubleshooting"></a>solución de problemas

* [Uso de agregados de Cosmos DB](https://github.com/Azure/azure-documentdb-spark/wiki/Troubleshooting:-Using-Cosmos-DB-Aggregates)
* [Problemas conocidos](https://github.com/Azure/azure-cosmosdb-spark/wiki/Known-Issues)

### <a name="performance"></a>Rendimiento

* [Consejos de rendimiento](https://github.com/Azure/azure-cosmosdb-spark/wiki/Performance-tips)
* [Ejecuciones de pruebas de consulta](https://github.com/Azure/azure-documentdb-spark/wiki/Query-Test-Runs)
* [Ejecuciones de pruebas de escritura](https://github.com/Azure/azure-cosmosdb-spark/wiki/Writing-Test-Runs)

### <a name="change-feed"></a>Fuente de cambios

* [Cambios de procesamiento de Stream con fuente de cambios de Azure Cosmos DB y Apache Spark](https://github.com/Azure/azure-cosmosdb-spark/wiki/Stream-Processing-Changes-using-Azure-Cosmos-DB-Change-Feed-and-Apache-Spark)
* [Demostraciones de fuente de cambios](https://github.com/Azure/azure-cosmosdb-spark/wiki/Change-Feed-demos)
* [Demostraciones de Stream estructurado](https://github.com/Azure/azure-cosmosdb-spark/wiki/Structured-Stream-demos)

### <a name="monitoring"></a>Supervisión

* [Supervisión de trabajos de Spark con application insights](https://github.com/Azure/azure-cosmosdb-spark/tree/2.3/samples/monitoring)

## <a name="next-steps"></a>Pasos siguientes

Si todavía no lo ha hecho, descargue el conector de Spark a Azure Cosmos DB del repositorio de GitHub [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark). Explore los siguientes recursos adicionales en el repositorio:

* [Ejemplos de agregaciones](https://github.com/Azure/azure-cosmosdb-spark/wiki/Aggregations-Examples)
* [Cuadernos y script de ejemplo](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples)

Es posible que también quiera consultar la [guía de Apache Spark SQL, DataFrames y conjuntos de datos](https://spark.apache.org/docs/latest/sql-programming-guide.html) y el artículo [Apache Spark en Azure HDInsight](../hdinsight/spark/apache-spark-jupyter-spark-sql.md).
