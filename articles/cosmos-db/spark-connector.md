---
title: Conexión de Apache Spark a Azure Cosmos DB | Microsoft Docs
description: Use este tutorial para aprender sobre el conector de Azure Cosmos DB Spark que le permite conectar Apache Spark a Azure Cosmos DB para realizar agregaciones distribuidas y ciencias de datos en el sistema de base de datos distribuido globalmente multiinquilino de Microsoft diseñado para la nube.
keywords: apache spark
services: cosmos-db
author: tknandu
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: ramkris
ms.openlocfilehash: cae66a40882231f7762af29cdeaaf658dd2aa968
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39113954"
---
# <a name="accelerate-real-time-big-data-analytics-by-using-the-spark-to-azure-cosmos-db-connector"></a>Aceleración de análisis de macrodatos en tiempo real mediante el conector de Spark a Azure Cosmos DB
 
El conector de Spark a Azure Cosmos DB permite que Azure Cosmos DB actúe como entrada o salida para trabajos de Apache Spark. La conexión de [Spark](http://spark.apache.org/) a [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) acelera la capacidad de resolver problemas de ciencia de datos de avance rápido, donde puede usar Azure Cosmos DB para guardar los datos y consultarlos rápidamente. El conector de Spark a Azure Cosmos DB usa de manera eficiente los índices administrados de forma nativa por Azure Cosmos DB. Los índices permiten columnas actualizables al realizar análisis y aplicar el filtrado de predicados en los datos distribuidos globalmente en rápida evolución, que abarcan Internet de las cosas (IoT), ciencia de datos y escenarios de análisis.

Obtenga más información sobre el conector de Spark a Azure Cosmos DB en este vídeo:

> [!VIDEO https://channel9.msdn.com/Events/Connect/2017/T135/player] 

## <a name="connector-components"></a>Componentes del conector

El conector de Spark a Azure Cosmos DB utiliza los siguientes componentes:

* [Azure Cosmos DB](http://documentdb.com) permite a los clientes aprovisionar y escalar el rendimiento y el almacenamiento de forma elástica a través de cualquier número de regiones geográficas.  

* [Apache Spark](http://spark.apache.org/) es un motor de procesamiento de código abierto eficaz diseñado para ofrecer velocidad, facilidad de uso y análisis sofisticados.  

* [Clúster de Apache Spark en Azure Databricks](https://docs.azuredatabricks.net/getting-started/index.html) para ejecutar trabajos de Spark en el clúster de Spark.

## <a name="connect-apache-spark-to-azure-cosmos-db"></a>Conexión de Apache Spark a Azure Cosmos DB

Existen dos enfoques para conectar Apache Spark y Azure Cosmos DB:

1. Mediante el uso del [SDK de Python para Azure Cosmos DB SQL](https://github.com/Azure/azure-documentdb-python), un conector de Spark a Cosmos DB basado en Python, que también se conoce como "pyDocumentDB".  

2. Con el [SDK de Java para SQL de Azure Cosmos DB](https://github.com/Azure/azure-documentdb-java), un conector de Spark a Cosmos DB basado en Java.


**Versiones compatibles**

| Componente | Versión |
|---------|-------|
|Spark de Apache| 2.1.x, 2.2.x, 2.3.x |
| Scala|2.11|
| Versión en tiempo de ejecución de Databricks | > 3.4 |
| SDK de Java para SQL de Azure Cosmos DB | 1.16.2 |

## <a name="connect-by-using-python-or-pydocumentdb-sdk"></a>Conexión mediante Python o el SDK de pyDocumentDB

La siguiente imagen muestra la arquitectura de la implementación del SDK de pyDocumentDB:

![Flujo de datos de Spark a Azure Cosmos DB a través de pyDocumentDB DB](./media/spark-connector/spark-pydocumentdb.png)


### <a name="data-flow"></a>flujo de datos

El flujo de datos de la implementación de pyDocumentDB es el siguiente:

* El nodo maestro de Spark se conecta al nodo de puerta de enlace de Azure Cosmos DB a través de pyDocumentDB. Un usuario especifica solo las conexiones de Spark y Azure Cosmos DB. Las conexiones a los respectivos nodos maestro y de puerta de enlace son transparentes para el usuario.  

* El nodo de puerta de enlace realiza la consulta en Azure Cosmos DB donde posteriormente se ejecuta la consulta en las particiones de la colección de los nodos de datos. La respuesta a esas consultas se reenvía al nodo de puerta de enlace y ese conjunto de resultados se devuelve al nodo maestro de Spark.  

* Las consultas posteriores (por ejemplo, en un DataFrame de Spark) se envían a los nodos de trabajo de Spark para su procesamiento.  

La comunicación entre Spark y Azure Cosmos DB se limita al nodo maestro de Spark y a los nodos de puerta de enlace de Azure Cosmos DB. Las consultas van tan rápido como permite la capa de transporte entre estos dos nodos.

Ejecute estos pasos para conectar Spark con Azure Cosmos DB mediante el SDK de pyDocumentDB:

1. Cree un [área de trabajo de Azure Databricks](../azure-databricks/quickstart-create-databricks-workspace-portal.md#create-an-azure-databricks-workspace) y un [clúster de Spark](../azure-databricks/quickstart-create-databricks-workspace-portal.md#create-a-spark-cluster-in-databricks) (versión en tiempo de ejecución 4.0 de Databricks (incluye Apache Spark 2.3.0, Scala 2.11) dentro del área de trabajo.  

2. Una vez que se crea el clúster y que está en ejecución, vaya a **Área de trabajo** > **Crear** > **Biblioteca**.  
3. En el cuadro de diálogo New Library (Nueva biblioteca), elija **Upload Python Egg or PyPi** (Cargar Python Egg o PyPi) como el origen, proporcione **pydocumentdb** como nombre y seleccione **Install Library** (Instalar biblioteca). El SDK de PyDocumentdb ya está publicado en los paquetes de PIP para que pueda buscarlo e instalarlo. 

   ![Creación y conexión de una biblioteca](./media/spark-connector/create-library.png)

4. Una vez que instale la biblioteca, adjúntela al clúster que creó anteriormente.  

5. Luego, vaya a **Área de trabajo** > **Crear** > **Cuaderno**.  

6. En el cuadro de diálogo **Create Notebook** (Crear cuaderno), escriba un nombre descriptivo y elija **Python** como lenguaje. En la lista desplegable, seleccione el clúster que creó anteriormente y seleccione **Crear**.  

7. La simplicidad del transporte de comunicación permite que la ejecución de una consulta de Spark a Azure Cosmos DB mediante pyDocumentDB sea relativamente sencilla. Luego ejecutará algunas consultas de Spark con los datos de ejemplo sobre vuelos que están hospedados en la cuenta "doctorwho" de Cosmos DB que es de acceso público. La versión HTML del cuaderno está hospedada en el repositorio [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) de GitHub. Debe descargar los archivos del repositorio e ir a `\samples\Documentation_Samples\Read_Batch_PyDocumentDB.html` y puede importar el cuaderno a la cuenta de Azure Databricks y ejecutarlo. En la sección siguiente se explica detalladamente la funcionalidad de los bloques de código.

El fragmento de código siguiente muestra la importancia del SDK de pyDocumentDB y cómo ejecutar una consulta en el contexto de Spark. Como se indica en el fragmento de código, el SDK de pyDocumentDB contiene los parámetros de conexión necesarios para conectarse a la cuenta de Azure Cosmos DB. Importa las bibliotecas necesarias, configura la clave maestra y el host para crear el cliente de Azure Cosmos DB (pydocumentdb.document_client).


```python
# Import Necessary Libraries
import pydocumentdb
from pydocumentdb import document_client
from pydocumentdb import documents
import datetime

# Configuring the connection policy (allowing for endpoint discovery)
connectionPolicy = documents.ConnectionPolicy()
connectionPolicy.EnableEndpointDiscovery
connectionPolicy.PreferredLocations = ["Central US", "East US 2", "Southeast Asia", "Western Europe","Canada Central"]

# Set keys to connect to Azure Cosmos DB
masterKey = 'le1n99i1w5l7uvokJs3RT5ZAH8dc3ql7lx2CG0h0kK4lVWPkQnwpRLyAN0nwS1z4Cyd1lJgvGUfMWR3v8vkXKA=='
host = 'https://doctorwho.documents.azure.com:443/'
client = document_client.DocumentClient(host, {'masterKey': masterKey}, connectionPolicy)

```

Luego puede ejecutar las consultas, el fragmento de código siguiente se conecta a la colección airports.codes de la cuenta DoctorWho y ejecuta una consulta para extraer las ciudades con aeropuerto del estado de Washington. 

```python
# Configure Database and Collections
databaseId = 'airports'
collectionId = 'codes'

# Configurations the Azure Cosmos DB client will use to connect to the database and collection
dbLink = 'dbs/' + databaseId
collLink = dbLink + '/colls/' + collectionId

# Set query parameter
querystr = "SELECT c.City FROM c WHERE c.State='WA'"

```

Una vez que se ejecuta la consulta, el resultado es "query_iterable.QueryIterable" que se convierte a una lista Python que, a su vez, se convierte en una trama de datos de Spark. 

```python
# Query documents
query = client.QueryDocuments(collLink, querystr, options=None, partition_key=None)

# Query for partitioned collections
# query = client.QueryDocuments(collLink, query, options= { 'enableCrossPartitionQuery': True }, partition_key=None)

# Create `df` Spark DataFrame from `elements` Python list
df = spark.createDataFrame(list(query))

# Show data
df.show()
```

## <a name="considerations-when-using-pydocumentdb-sdk"></a>Consideraciones al usar el SDK de pyDocumentDB
Se recomienda conectar Spark a Azure Cosmos DB mediante el SDK de pyDocumentDB en los escenarios siguientes:

* Quiera usar Python.  

* Desea devolver un conjunto de resultados relativamente pequeño desde Azure Cosmos DB a Spark. Tenga en cuenta que el conjunto de datos subyacente en Azure Cosmos DB puede ser bastante grande y que aplica filtros o ejecuta filtros de predicado en el origen de Azure Cosmos DB.

## <a name="connect-by-using-the-java-sdk"></a>Conexión mediante el SKD de Java

En la imagen siguiente se muestra la arquitectura de la implementación del SDK de Java para SQL de Azure Cosmos DB y los datos se trasladan entre los nodos de trabajo de Spark:

![Flujo de datos del conector de Spark a Azure Cosmos DB](./media/spark-connector/spark-connector.png)

### <a name="data-flow"></a>flujo de datos

El flujo de datos de la implementación del SDK de Java es el siguiente:

* El nodo maestro de Spark se conecta al nodo de puerta de enlace de Azure Cosmos DB para obtener el mapa de particiones. Un usuario especifica solo las conexiones de Spark y Azure Cosmos DB. Las conexiones a los respectivos nodos maestro y de puerta de enlace son transparentes para el usuario.  

* Esta información se proporciona en el nodo principal de Spark. En este momento, debe poder analizar la consulta para determinar las particiones (y sus ubicaciones) dentro de Azure Cosmos DB a las que debe acceder.  

* Esta información se transmite a los nodos de trabajo de Spark.  

* Los nodos de trabajo de Spark se conectan directamente a las particiones de Azure Cosmos DB para extraer los datos y devolverlos a las particiones de Spark de los nodos de trabajo.  

La comunicación entre Spark y Azure Cosmos DB es mucho más rápida porque el movimiento de datos se realiza entre los nodos de trabajo de Spark y los nodos de datos de Azure Cosmos DB (particiones). En este documento, enviará algunos datos de Twitter de ejemplo a la cuenta de Azure Cosmos DB y ejecutará consultas de Spark con esos datos. Use estos pasos para escribir datos de Twitter de ejemplo en Azure Cosmos DB:

1. Cree una [cuenta de API de SQL para Azure Cosmos DB](create-sql-api-dotnet.md#create-a-database-account) y [agregue una colección](create-sql-api-dotnet.md#add-a-collection) a la cuenta.  

2. Descargue el ejemplo [TwitterCosmosDBFeed](https://github.com/tknandu/TwitterCosmosDBFeed) desde GitHub, el que se usa para escribir datos de Twitter de ejemplo en Azure Cosmos DB.  

3. Abra un símbolo del sistema e instale los módulos Tweepy y pyDocumentdb mediante la ejecución de estos comandos:

   ```bash
   pip install tweepy==3.3.0
   pip install pyDocumentDB
   ```

4. Extraiga el contenido del ejemplo de fuente de Twitter y abra el archivo config.py. Actualice los valores de masterKey, host, databaseId, collectionId y preferredLocations.  

5. Vaya a `http://apps.twitter.com/` y registre el script de la fuente de Twitter como una aplicación nueva. Después de elegir un nombre y una aplicación para la aplicación, recibirá una **clave de consumidor, secreto de consumidor, token de acceso y secreto de token de acceso**. Copie estos valores y actualícelos en el archivo config.py para proporcionar acceso mediante programación a la aplicación para Twitter.   

6. Guarde el archivo config.py. Abra el símbolo del sistema y ejecute la aplicación de Python mediante el comando siguiente:

   ```bash
   Python driver.py
   ```

7. Vaya a la colección de Azure Cosmos DB en el portal y compruebe que los datos de Twitter se escriben en la colección.

### <a name="find-and-attach-java-sdk-to-the-spark-cluster"></a>Búsqueda y conexión del SDK de Java al clúster de Spark

1. Cree un [área de trabajo de Azure Databricks](../azure-databricks/quickstart-create-databricks-workspace-portal.md#create-an-azure-databricks-workspace) y un [clúster de Spark](../azure-databricks/quickstart-create-databricks-workspace-portal.md#create-a-spark-cluster-in-databricks) (versión en tiempo de ejecución 4.0 de Databricks (incluye Apache Spark 2.3.0, Scala 2.11) dentro del área de trabajo.  

2. Una vez que se crea el clúster y que está en ejecución, vaya a **Área de trabajo** > **Crear** > **Biblioteca**.  

3. En el cuadro de diálogo New Library (Nueva biblioteca), elija **Maven Coordinate** (Coordenada de Maven) como el origen, proporcione el valor de coordenada **com.microsoft.azure:azure-cosmosdb-spark_2.3.0_2.11:1.2.0** y seleccione **Create Library** (Crear biblioteca). Se resuelven las dependencias de Maven y el paquete se agrega al área de trabajo. En el formato de coordenada de Maven anterior, 2.3.0 representa la versión de Spark, 2.11 representa la versión de Scala y 1.2.0 representa la versión del conector de Azure Cosmos DB. 

4. Una vez que instale la biblioteca, adjúntela al clúster que creó anteriormente. 

En este artículo se muestra el uso del SDK de Java del conector de Spark en los escenarios siguientes:

* Lectura de datos de Twitter desde Azure Cosmos DB  

* Lectura de datos de Twitter que se transmiten en streaming a Azure Cosmos DB  

* Escritura de datos de Twitter en Azure Cosmos DB 

### <a name="read-twitter-data-from-azure-cosmos-db"></a>Lectura de datos de Twitter desde Azure Cosmos DB
 
En esta sección se ejecutan consultas de Spark para leer un lote de datos de Twitter desde Azure Cosmos DB. La versión HTML del cuaderno está hospedada en el repositorio [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) de GitHub. Debe descargar los archivos del repositorio e ir a `\samples\Documentation_Samples\Read_Batch_Twitter_Data.html`, donde puede importar el cuaderno a la cuenta de Azure Databricks, actualizar el URI de la cuenta, la clave maestra, la base de datos, los nombres de conexión y ejecutarlo o puede crear el cuaderno de la siguiente manera:

1. Vaya a la cuenta de Azure Databricks y seleccione **Área de trabajo** > **Crear** > **Cuaderno**. 

2. En el cuadro de diálogo **Create Notebook** (Crear cuaderno), escriba un nombre descriptivo, elija **Python** como lenguaje y, en la lista desplegable, seleccione el clúster que creó anteriormente y seleccione **Crear**.  

3. Actualice los valores de punto de conexión, clave maestra, base de datos y colección para conectarse a la cuenta y leer tweets mediante el comando spark.read.format().

   ```python
   # Configuration Map
   tweetsConfig = {
   "Endpoint" : "<Your Azure Cosmos DB endpoint>",
   "Masterkey" : "<Primary key of your Azure Cosmos DB account>",
   "Database" : "<Your Azure Cosmos DB database name>",
   "Collection" : "<Your Azure Cosmos DB collection name>", 
   "preferredRegions" : "East US",
   "SamplingRatio" : "1.0",
   "schema_samplesize" : "200000",
   "query_custom" : "SELECT c.id, c.created_at, c.user.screen_name, c.user.lang, c.user.location, c.text, c.retweet_count, c.entities.hashtags, c.entities.user_mentions, c.favorited, c.source FROM c"
   }
   # Read Tweets
   tweets = spark.read.format("com.microsoft.azure.cosmosdb.spark").options(**tweetsConfig).load()
   tweets.createOrReplaceTempView("tweets")
   #tweets.cache()

   ```

4. Ejecute la consulta para obtener el recuento de tweets mediante distintos hashtags de los datos en caché. 

   ```python
   %sql
   select hashtags.text, count(distinct id) as tweets
   from (
   select 
     explode(hashtags) as hashtags,
     id
   from tweets
   ) a
   group by hashtags.text
   order by tweets desc
   limit 10
   ```

El SDK de Java admite los valores siguientes para la asignación de configuración: 

|Configuración  |DESCRIPCIÓN  |
|---------|---------|
|query_maxdegreeofparallelism  | Establece el número de operaciones simultáneas que se ejecutan en el cliente durante la ejecución de consultas en paralelo. Si se establece en un valor mayor que 0, limita el número de operaciones simultáneas al valor asignado. Si se establece en un valor menor que 0, el sistema decide automáticamente el número de operaciones simultáneas que se ejecutarán. Cuando el conector asigna cada partición de la colección a un ejecutor, este valor no tendrá ningún efecto en la operación de lectura.        |
|query_maxbuffereditemcount     |    Establece el número máximo de elementos que se pueden almacenar en búfer en el cliente durante la ejecución de consultas en paralelo. Si se establece en un valor mayor que 0, limita el número de elementos almacenados en búfer al valor asignado. Si se establece en un valor menor que 0, el sistema decide automáticamente el número de elementos que se almacenarán en búfer.     |
|query_enablescan    |   Establece la opción de habilitar exámenes en las consultas que no se pudieron atender porque se optó por no recibir el indexado en las rutas de acceso solicitadas.       |
|query_disableruperminuteusage  |  Deshabilita la capacidad de unidades de solicitud (RU)/minuto para atender la consulta si se agotó la capacidad de unidades de solicitud (RU)/segundo aprovisionada habitual.       |
|query_emitverbosetraces   |   Establece la opción de permitir consultas para emitir seguimientos detallados para investigación.      |
|query_pagesize  |   Establece el tamaño de la página de resultados de la consulta para cada solicitud de consulta. Con el fin de optimizar el rendimiento, use un tamaño de página grande para reducir la cantidad de recorridos de ida y vuelta para capturar los resultados de las consultas.      |
|query_custom  |  Establece la consulta de Azure Cosmos DB para invalidar la consulta predeterminada al capturar datos desde Azure Cosmos DB. Observe que, cuando se proporciona este valor, se usará también en lugar de una consulta con predicados transferidos.     |

Según el escenario, se deben usar distintos valores de configuración para optimizar el rendimiento. Tenga en cuenta que la clave de configuración actualmente no distingue mayúsculas de minúsculas y el valor de configuración siempre es una cadena.

### <a name="read-twitter-data-that-is-streaming-to-azure-cosmos-db"></a>Lectura de datos de Twitter que se transmiten en streaming a Azure Cosmos DB

En esta sección, ejecutará consultas de Spark para leer una fuente de cambios de streaming de datos de Twitter. Cuando ejecute las consultas de esta sección, asegúrese de que la aplicación de fuente de Twitter esté en ejecución y que suministre datos a Azure Cosmos DB. La versión HTML del cuaderno está hospedada en el repositorio [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) de GitHub. Debe descargar los archivos del repositorio e ir a `\samples\Documentation_Samples\Read_Stream_Twitter_Data.html`, donde puede importar el cuaderno a la cuenta de Azure Databricks, actualizar el URI de la cuenta, la clave maestra, la base de datos, los nombres de conexión y ejecutarlo o puede crear el cuaderno de la siguiente manera:

1. Vaya a la cuenta de Azure Databricks y seleccione **Área de trabajo** > **Crear** > **Cuaderno**.  

2. En el cuadro de diálogo **Create Notebook** (Crear cuaderno), escriba un nombre descriptivo, elija **Scala** como lenguaje y, en la lista desplegable, seleccione el clúster que creó anteriormente y seleccione **Crear**.  

3. Actualice los valores de punto de conexión, clave maestra, base de datos y colección para conectarse a la cuenta.

   ```scala
   // This script does the following:
   // - creates a structured stream from a Twitter feed CosmosDB collection (on top of change feed)
   // - get the count of tweets
   import com.microsoft.azure.cosmosdb.spark._
   import com.microsoft.azure.cosmosdb.spark.schema._
   import com.microsoft.azure.cosmosdb.spark.config.Config
   import org.codehaus.jackson.map.ObjectMapper
   import com.microsoft.azure.cosmosdb.spark.streaming._
   import java.time._

   val sourceConfigMap = Map(
   "Endpoint" -> "<Your Azure Cosmos DB endpoint>",
   "Masterkey" -> "<Primary key of your Azure Cosmos DB account>",
   "Database" -> "<Your Azure Cosmos DB database name>",
   "Collection" -> "<Your Azure Cosmos DB collection name>", 
   "ConnectionMode" -> "Gateway",
   "ChangeFeedCheckpointLocation" -> "/tmp",
   "changefeedqueryname" -> "Streaming Query from Cosmos DB Change Feed Internal Count")
   ```
4. Empiece a leer la fuente de cambios como secuencia mediante el comando spark.readStream.format():

   ```scala
   var streamData = spark.readStream.format(classOf[CosmosDBSourceProvider].getName).options(sourceConfigMap).load()
   ```
5. Inicie el streaming de la consulta a la consola:

   ```scala
   //**RUN THE ABOVE FIRST AND KEEP BELOW IN SEPARATE CELL
   val query = streamData.withColumn("countcol", streamData.col("id").substr(0,0)).groupBy("countcol").count().writeStream.outputMode("complete").format("console").start()
   ```

El SDK de Java admite los valores siguientes para la asignación de configuración:

|Configuración  |DESCRIPCIÓN  |
|---------|---------|
|readchangefeed   |  Indica que el contenido de la colección se captura de la fuente de cambios de CosmosDB. El valor predeterminado es false.       |
|changefeedqueryname |   Una cadena personalizada para identificar la consulta. El conector realiza un seguimiento de los tokens de continuación de la colección para distintas consultas de la fuente de cambios de manera independiente. Si el valor de readchangefeedis es true, se trata de una configuración necesaria que no puede tener un valor vacío.      |
|changefeedcheckpointlocation  |   Una ruta de acceso al almacenamiento de archivos locales para conservar los tokens de continuación en caso de errores de nodo.      |
|changefeedstartfromthebeginning  |  Establece si la fuente de cambios debe iniciarse desde el principio (true) o desde el punto actual (false). De manera predeterminada, se inicia desde la posición actual (false).       |
|rollingchangefeed  |   Valor booleano que indica si la fuente de cambios debe iniciarse en la última consulta. El valor predeterminado es false, lo que significa que los cambios se van a contar desde la primera lectura de la colección.      |
|changefeedusenexttoken  |   Valor booleano para admitir el procesamiento de escenarios de error. Se usa para indicar que el lote de la fuente de cambios actual se controló correctamente y RDD debe utilizar los tokens de continuación siguientes para obtener el lote de cambios subsiguiente.      |
| InferStreamSchema | Valor booleano que indica si se debe muestrear el esquema de los datos de streaming al principio del streaming. De forma predeterminada, este valor se establece en true. Si este parámetro se establece en true y el esquema de los datos de streaming cambia después de que se muestrean los datos, se quitarán las propiedades recién agregadas en la trama de datos de streaming. <br/><br/> Si desea que la trama de datos de streaming no dependa del esquema, establezca este parámetro en false. De este modo, el cuerpo de los documentos leídos desde la fuente de cambios de Azure Cosmos DB se encapsula en una propiedad "body" en la trama de datos de streaming resultante aparte de los valores de propiedad del sistema.
 |

### <a name="connection-settings"></a>Configuración de conexión

El SDK de Java es compatible con los valores de conexión siguientes:

|Configuración  |DESCRIPCIÓN  |
|---------|---------|
|connectionmode   |  Establece el modo de conexión que debe utilizar el DocumentClient interno para comunicarse con Azure Cosmos DB. Los valores permitidos son **DirectHttps** (valor predeterminado) y **Gateway** (Puerta de enlace). El modo de conexión DirectHttps enruta las solicitudes directamente a las particiones de CosmosDB y proporciona algunas ventajas de latencia.       |
|connectionmaxpoolsize   |  Establece el valor del tamaño del grupo de conexiones que usa el DocumentClient interno. El valor predeterminado es 100.       |
|connectionidletimeout  |  Establece el valor de tiempo de espera para las conexiones inactivas en segundos. El valor predeterminado es 60.       |
|query_maxretryattemptsonthrottledrequests    |  Establece el número máximo de reintentos. Este valor se usa en caso de un error de solicitud debido a la limitación de la velocidad en el cliente. Si no se especifica, el valor predeterminado es 1000 reintentos.       |
|query_maxretrywaittimeinseconds   |  Establece el tiempo de reintentos máximo en segundos. De forma predeterminada, es 1000 segundos.       |

### <a name="write-twitter-data-to-azure-cosmos-db"></a>Escritura de datos de Twitter en Azure Cosmos DB 

En esta sección, se ejecutan consultas de Spark para escribir un lote de datos de Twitter en una nueva colección en la misma base de datos. La versión HTML del cuaderno está hospedada en el repositorio [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) de GitHub. Debe descargar los archivos del repositorio e ir a `\samples\Documentation_Samples\Write_Batch_Twitter_Data.html`, donde puede importar el cuaderno a la cuenta de Azure Databricks, actualizar el URI de la cuenta, la clave maestra, la base de datos, los nombres de conexión y ejecutarlo o puede crear el cuaderno de la siguiente manera:

1. Vaya a la cuenta de Azure Databricks y seleccione **Área de trabajo** > **Crear** > **Cuaderno**.  

2. En el cuadro de diálogo **Create Notebook** (Crear cuaderno), escriba un nombre descriptivo, elija **Scala** como lenguaje y, en la lista desplegable, seleccione el clúster que creó anteriormente y seleccione **Crear**.  

3. Actualice los valores de punto de conexión, clave maestra, base de datos y colección para conectarse a la colección de la base de datos para leer y escribir datos de Twitter.

   ```scala
   %scala
   // Import Necessary Libraries
   import org.joda.time._
   import org.joda.time.format._
   import com.microsoft.azure.cosmosdb.spark.schema._
   import com.microsoft.azure.cosmosdb.spark._
   import com.microsoft.azure.cosmosdb.spark.config.Config

   // Maps
   val readConfigMap = Map(
   "Endpoint" -> "<Your Azure Cosmos DB endpoint>",
   "Masterkey" -> "<Primary key of your Azure Cosmos DB account>",
   "Database" -> "<Your Azure Cosmos DB database name>",
   "Collection" -> "<Your Azure Cosmos DB source collection name>", 
   "preferredRegions" -> "East US",
   "SamplingRatio" -> "1.0",
   "schema_samplesize" -> "200000",
   "query_custom" -> "SELECT c.id, c.created_at, c.user.screen_name, c.user.location, c.text, c.retweet_count, c.entities.hashtags, c.entities.user_mentions, c.favorited, c.source FROM c"
   )
   val writeConfigMap = Map(
   "Endpoint" -> "<Your Azure Cosmos DB endpoint>",
   "Masterkey" -> "<Primary key of your Azure Cosmos DB account>",
   "Database" -> "<Your Azure Cosmos DB database name>",
   "Collection" -> "<Your Azure Cosmos DB destination collection name>", 
   "preferredRegions" -> "East US",
   "SamplingRatio" -> "1.0",
   "schema_samplesize" -> "200000"
   ) 

   // Configs
   // get read
   val readConfig = Config(readConfigMap)
   val tweets = spark.read.cosmosDB(readConfig)
   tweets.createOrReplaceTempView("tweets")
   tweets.cache()

   // get write
   val writeConfig = Config(writeConfigMap)
   ```
4. Ejecute la consulta para obtener el recuento de tweets mediante distintos hashtags de los datos en caché. 

   ```scala
   %sql
   select hashtags.text, count(distinct id) as tweets
   from (
   select 
     explode(hashtags) as hashtags,
     id
   from tweets
   ) a
   group by hashtags.text
   order by tweets desc
   limit 10
   ```

5. Cree una nueva trama de datos de etiquetas de tweets y guarde los datos en la colección nueva. Después de ejecutar el código siguiente, puede volver al portal y comprobar que los datos se escriben en la colección. 

   ```scala
   %scala
   // Import SaveMode so you can Overwrite, Append, ErrorIfExists, Ignore
   import org.apache.spark.sql.{Row, SaveMode, SparkSession}

   // Create new DataFrame of tweets tags
   val tweets_bytags = spark.sql("select '2018-06-12' as currdt, hashtags.text as hashtags, count(distinct id) as tweets from ( select explode(hashtags) as hashtags, id from tweets ) a group by hashtags.text order by tweets desc limit 10")

   // Save to Cosmos DB (using Append in this case)
   // Ensure the baseConfig contains a Read-Write Key
   // The key provided in our examples is a Read-Only Key

   tweets_bytags.write.mode(SaveMode.Overwrite).cosmosDB(writeConfig)
   ```

El SDK de Java admite los valores siguientes para la asignación de configuración:

|Configuración  |DESCRIPCIÓN  |
|---------|---------|
| BulkImport | Valor booleano que indica si se deben importar datos mediante el uso de la biblioteca de BulkExecutor. De forma predeterminada, este valor se establece en true. |
|WritingBatchSize  |   Indica el tamaño del lote que se usará al escribir datos en la colección de Azure Cosmos DB. <br/><br/> Si el parámetro BulkImport se establece en true, el parámetro WritingBatchSize indica el tamaño del lote de documentos proporcionados como entrada a la API importAll de la biblioteca de BulkExecutor. De manera predeterminada, este valor se establece en 100 000. <br/><br/> Si el parámetro BulkImport se establece en false, el parámetro WritingBatchSize indica el tamaño del lote que se usará al escribir en la colección de Azure Cosmos DB. El conector envía de manera asincrónica las solicitudes createDocument/upsertDocument en lote. Cuanto mayor sea el tamaño del lote, mayor es el rendimiento que podemos lograr, siempre que los recursos de clúster estén disponibles. Por otro lado, especifique un tamaño de lote menor para limitar la velocidad y el consumo de RU. De manera predeterminada, el tamaño del lote de escritura se establece en 500.  |
|Upsert   |  Cadena de valor booleano que indica si se debe usar upsertDocument en lugar de CreateDocument al escribir en la colección de CosmosDB.   |
| WriteThroughputBudget |  Cadena de enteros que representa el número de RU\s que quiere asignar al trabajo de Spark de ingesta en bloque con respecto al total de rendimiento asignado a la colección. |


### <a name="write-twitter-data-that-is-streaming-to-azure-cosmos-db"></a>Escritura de datos de Twitter que se transmiten en streaming a Azure Cosmos DB 

En esta sección, se ejecutan consultas de Spark para escribir una fuente de cambios de datos de Twitter de streaming en una nueva colección en la misma base de datos. La versión HTML del cuaderno está hospedada en el repositorio [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) de GitHub. Debe descargar los archivos del repositorio e ir a `\samples\Documentation_Samples\Write_Stream_Twitter_Data.html`, donde puede importar el cuaderno a la cuenta de Azure Databricks, actualizar el URI de la cuenta, la clave maestra, la base de datos, los nombres de conexión y ejecutarlo o puede crear el cuaderno de la siguiente manera:

1. Vaya a la cuenta de Azure Databricks y seleccione **Área de trabajo** > **Crear** > **Cuaderno**.  

2. En el cuadro de diálogo **Create Notebook** (Crear cuaderno), escriba un nombre descriptivo, elija **Scala** como lenguaje y, en la lista desplegable, seleccione el clúster que creó anteriormente y seleccione **Crear**.  

3. Actualice los valores de punto de conexión, clave maestra, base de datos y colección para conectarse a la colección de la base de datos para leer y escribir datos de Twitter.

   ```scala
   import com.microsoft.azure.cosmosdb.spark._
   import com.microsoft.azure.cosmosdb.spark.schema._
   import com.microsoft.azure.cosmosdb.spark.config.Config
   import com.microsoft.azure.cosmosdb.spark.streaming._

   // Configure connection to Azure Cosmos DB Change Feed (Trades)
   val ConfigMap = Map(
   // Account settings
   "Endpoint" -> "<Your Azure Cosmos DB endpoint>",
   "Masterkey" -> "<Primary key of your Azure Cosmos DB account>",
   "Database" -> "<Your Azure Cosmos DB database name>",
   "Collection" -> "<Your Azure Cosmos DB source collection name>", 
   // Change feed settings
   "ReadChangeFeed" -> "true",
   "ChangeFeedStartFromTheBeginning" -> "true",
   "ChangeFeedCheckpointLocation" -> "dbfs:/cosmos-feed",
   "ChangeFeedQueryName" -> "Structured Stream Read",
   "InferStreamSchema" -> "true"
   )
   ```
4. Empiece a leer la fuente de cambios como secuencia mediante el comando spark.readStream.format():
 
   ```scala
   // Start reading change feed of trades as a stream
   var streamdata = spark
     .readStream
     .format(classOf[CosmosDBSourceProvider].getName)
     .options(ConfigMap)
     .load()
   ```

5. Defina la configuración de la colección de destino e inicie el trabajo de streaming con el método writeStream.format():

   ```scala
   val sinkConfigMap = Map(
   "Endpoint" -> "<Your Azure Cosmos DB endpoint>",
   "Masterkey" -> "<Primary key of your Azure Cosmos DB account>",
   "Database" -> "<Your Azure Cosmos DB database name>",
   "Collection" -> "<Your Azure Cosmos DB destination collection name>", 
   "checkpointLocation" -> "streamingcheckpointlocation6",
   "WritingBatchSize" -> "100",
   "Upsert" -> "true")

   // Start the stream writer
   val streamingQueryWriter = streamdata
    .writeStream
    .format(classOf[CosmosDBSinkProvider].getName)
    .outputMode("append")
    .options(sinkConfigMap)
    .start()
 ```

El SDK de Java admite los valores siguientes para la asignación de configuración:

|Configuración  |DESCRIPCIÓN  |
|---------|---------|
|Upsert   |  Cadena de valor booleano que indica si se debe usar upsertDocument en lugar de CreateDocument al escribir en la colección de CosmosDB.   |
|checkpointlocation  |   Una ruta de acceso al almacenamiento de archivos locales para conservar los tokens de continuación en caso de errores de nodo.   |
|WritingBatchSize  |  Indica el tamaño del lote que se usará al escribir datos en la colección de Azure Cosmos DB. El conector envía de manera asincrónica las solicitudes createDocument/upsertDocument en lote. Cuanto mayor sea el tamaño del lote, mayor es el rendimiento que podemos lograr, siempre que los recursos de clúster estén disponibles. Por otro lado, especifique un tamaño de lote menor para limitar la velocidad y el consumo de RU. De manera predeterminada, el tamaño del lote de escritura se establece en 500.  |


## <a name="considerations-when-using-java-sdk"></a>Consideraciones al usar el SDK de Java

Se recomienda conectar Spark a Azure Cosmos DB mediante el SDK de Java en los escenarios siguientes:

* Puede que desee usar Python o Scala.  

* Tiene que transferir una gran cantidad de datos entre Apache Spark y Azure Cosmos DB, el SDK de Java tiene un mayor rendimiento en comparación con pyDocumentDB. Para darle una idea de la diferencia de rendimiento de consultas, consulte la [wiki de ejecuciones de pruebas de consulta](https://github.com/Azure/azure-cosmosdb-spark/wiki/Query-Test-Runs).

## <a name="next-steps"></a>Pasos siguientes

Si aún no lo ha hecho, descargue el conector de Spark a Azure Cosmos DB del repositorio de GitHub [azure-documentdb-spark](https://github.com/Azure/azure-cosmosdb-spark) y explore los recursos adicionales del repositorio:

* [Ejemplos de agregaciones distribuidas](https://github.com/Azure/azure-cosmosdb-spark/wiki/Aggregations-Examples)
* [Cuadernos y script de ejemplo](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples)

Es posible que también quiera consultar la [guía de Apache Spark SQL, DataFrames y conjuntos de datos](http://spark.apache.org/docs/latest/sql-programming-guide.html) y el artículo [Apache Spark en Azure HDInsight](../hdinsight/spark/apache-spark-jupyter-spark-sql.md).
