---
title: Creación de una cuenta de Cassandra API de Azure Cosmos DB, una base de datos y una tabla usando una aplicación de Java
description: En este artículo se muestra cómo crear una cuenta de Cassandra API, así como agregar una base de datos (también conocida como "espacio de claves") y una tabla en esa cuenta usando una aplicación de Java.
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
services: cosmos-db
ms.service: cosmos-db
ms.component: cosmosdb-cassandra
ms.topic: tutorial
ms.date: 09/24/2018
ms.openlocfilehash: af25c52d1300a2daed3542c5cb1db080c505f715
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46953130"
---
# <a name="create-an-azure-cosmos-db-cassandra-api-account-database-and-a-table-by-using-a-java-application"></a>Creación de una cuenta de Cassandra API de Azure Cosmos DB, una base de datos y una tabla usando una aplicación de Java

En este tutorial se describe cómo usar una aplicación de Java para crear una cuenta de Cassandra API de Azure Cosmos DB, así como agregar una base de datos (también conocida como "espacio de claves") y una tabla. La aplicación de Java utiliza el [controlador DataStax Java](https://github.com/datastax/java-driver) para crear una base de datos de usuario que contiene información detallada como el identificador de usuario, el nombre de usuario y la ciudad del usuario.  

En este tutorial se describen las tareas siguientes:

> [!div class="checklist"]
> * Creación de una cuenta de base de datos de Cassandra
> * Obtención de la cadena de conexión de cuenta
> * Creación de un proyecto de Maven y sus dependencias
> * Adición de una base de datos y una tabla
> * Ejecución de la aplicación

## <a name="prerequisites"></a>Requisitos previos 

* Si no tiene una suscripción a Azure, cree una  [cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de empezar. También puede  [probar gratis Azure Cosmos DB](https://azure.microsoft.com/try/cosmosdb/) sin una suscripción de Azure, sin cargos y sin compromiso. 

* Obtenga la última versión de [Kit de desarrollo de Java (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html). 

* [Descargue](http://maven.apache.org/download.cgi) e [instale](http://maven.apache.org/install.html) un archivo binario de [Maven](http://maven.apache.org/). 
  - En Ubuntu, puede ejecutar  `apt-get install maven` para instalar Maven. 

## <a name="create-a-database-account"></a>Creación de una cuenta de base de datos 

1. Inicie sesión en  [Azure Portal](https://portal.azure.com/). 

2. Haga clic en  **Crear un recurso** > **Bases de datos** > **Azure Cosmos DB**. 

3. En el panel  **Nueva cuenta** , especifique la configuración de la nueva cuenta de Azure Cosmos DB. 

   |Configuración   |Valor sugerido  |DESCRIPCIÓN  |
   |---------|---------|---------|
   |ID   |   Escribir un nombre único    | Escriba un nombre único para identificar esta cuenta de Azure Cosmos DB. <br/><br/>Como cassandra.cosmosdb.azure.com se anexa al identificador que proporcione para crear el punto de contacto, debe usar un identificador único pero reconocible.         |
   |API    |  Cassandra   |  La API determina el tipo de cuenta que se va a crear. <br/> Seleccione **Cassandra**, ya que en este artículo se creará una base de datos de columnar que se puede consultar mediante la sintaxis CQL.  |
   |Subscription    |  Su suscripción        |  Seleccione la suscripción de Azure que desea usar para esta cuenta de Azure Cosmos DB.        |
   |Grupo de recursos   | Escribir un nombre    |  Seleccione  **Crear nuevo** y, después, escriba un nombre nuevo de grupo de recursos para la cuenta. Para simplificar, puede usar el mismo nombre del identificador.    |
   |Ubicación    |  Seleccionar la región más cercana a los usuarios    |  Seleccione la ubicación geográfica en la que se va a hospedar la cuenta de Azure Cosmos DB. Use la ubicación más cercana a los usuarios para proporcionarles el acceso más rápido a los datos.    |

   ![Creación de la cuenta con el portal](./media/create-cassandra-api-account-java/create-account.png)

4. A continuación, seleccione **Crear**. <br/>La cuenta tarda unos minutos en crearse. Una vez creado el recurso, puede ver la notificación **Implementación correcta** en la esquina derecha del portal.

## <a name="get-the-connection-details-of-your-account"></a>Obtención de los detalles de conexión de la cuenta  

Obtenga la información de la cadena de conexión desde Azure Portal y cópiela en el archivo de configuración de Java. Esto permite que la aplicación se comunique con la base de datos hospedada. 

1. En  [Azure Portal](http://portal.azure.com/), vaya a la cuenta de Azure Cosmos DB. 

2. Abra el panel  **Cadena de conexión**.  

3. Copie los valores de **PUNTO DE CONTACTO**, **PUERTO**, **NOMBRE DE USUARIO** y **CONTRASEÑA PRINCIPAL** que se usarán en los pasos siguientes.

## <a name="create-maven-project-dependencies-and-utility-classes"></a>Creación del proyecto de Maven, dependencias y clases de utilidad 

El proyecto de ejemplo de Java que se usa en este artículo se hospeda en GitHub. Puede descargarlo del repositorio [azure-cosmos-db-cassandra-java-getting-started](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started). 

Después de descargar los archivos, actualice la información de la cadena de conexión en el archivo `java-examples\src\main\resources\config.properties` y ejecútelo.  

```java
cassandra_host=<FILLME_with_CONTACT POINT> 
cassandra_host = 10350 
cassandra_username=<FILLME_with_USERNAME> 
cassandra_password=<FILLME_with_PRIMARY PASSWORD> 
```

Como alternativa, puede usar el ejemplo desde cero.  

1. En el terminal o símbolo del sistema, cree un nuevo proyecto de Maven denominado "Cassandra-demo". 

   ```bash
   mvn archetype:generate -DgroupId=com.azure.cosmosdb.cassandra -DartifactId=cassandra-demo -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false 
   ```
 
2. Localice la carpeta `cassandra-demo`. Con un editor de texto, abra el archivo `pom.xml` que se generó. 

   Agregue las dependencias de DataStax de Cassandra y cree los complementos necesarios para el proyecto, tal y como se muestra en el archivo [pom.xml](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/pom.xml).  

3. En la carpeta `cassandra-demo\src\main`, cree una carpeta denominada `resources`.  En la carpeta de recursos, agregue los archivos config.properties y log4j.properties:

   - El archivo [config.properties](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/src/main/resources/config.properties) almacena los valores de clave y punto de conexión de Cassandra API de Azure Cosmos DB. 
   
   - El archivo [log4j.properties](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/src/main/resources/log4j.properties) define el nivel de registro necesario al interactuar con Cassandra API.  

4. Después, vaya a la carpeta `src/main/java/com/azure/cosmosdb/cassandra/`. Dentro de la carpeta cassandra, cree otra carpeta denominada `utils`. La nueva carpeta almacena las clases de utilidad necesarias para conectarse a la cuenta de Cassandra API. 

   Agregue la clase [CassandraUtils](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/src/main/java/com/azure/cosmosdb/cassandra/util/CassandraUtils.java) para crear el clúster y abrir y cerrar las sesiones de Cassandra. El clúster se conecta a Cassandra API de Azure Cosmos DB y devuelve una sesión a la que se accede. Use la clase [Configuraciones](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/src/main/java/com/azure/cosmosdb/cassandra/util/Configurations.java) para leer información de la cadena de conexión desde el archivo config.properties. 

5. El ejemplo de Java crea una base de datos con información de usuario como el nombre de usuario, el identificador de usuario y ciudad del usuario. Deberá definir los métodos get y set para acceder a los detalles del usuario en la función principal.
 
   Cree una clase [User.java](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/src/main/java/com/azure/cosmosdb/cassandra/User.java) en la carpeta `src/main/java/com/azure/cosmosdb/cassandra/` con los métodos get y set. 

## <a name="add-a-database-and-a-table"></a>Adición de una base de datos y una tabla  

En esta sección se describe cómo agregar una base de datos (espacio de claves) y una tabla mediante el lenguaje de consulta de Cassandra (CQL). Para obtener información sobre la sintaxis CQL para estos comandos, consulte la sintaxis de consulta [create keyspace](https://docs.datastax.com/en/cql/3.3/cql/cql_reference/cqlCreateKeyspace.html) y [create table](https://docs.datastax.com/en/cql/3.3/cql/cql_reference/cqlCreateTable.html#cqlCreateTable). 

1. En la carpeta `src\main\java\com\azure\cosmosdb\cassandra`, cree una carpeta nueva denominada "`repository`". 

2. A continuación, cree la clase de Java `UserRepository` y agregue el código siguiente en él: 

   ```java
   package com.azure.cosmosdb.cassandra.repository; 
   import java.util.List; 
   import com.datastax.driver.core.BoundStatement; 
   import com.datastax.driver.core.PreparedStatement; 
   import com.datastax.driver.core.Row; 
   import com.datastax.driver.core.Session; 
   import org.slf4j.Logger; 
   import org.slf4j.LoggerFactory; 
   
   /** 
    * Create a Cassandra session 
    */ 
   public class UserRepository { 
   
       private static final Logger LOGGER = LoggerFactory.getLogger(UserRepository.class); 
       private Session session; 
       public UserRepository(Session session) { 
           this.session = session; 
       } 
   
       /** 
       * Create keyspace uprofile in cassandra DB 
        */ 
   
       public void createKeyspace() { 
            final String query = "CREATE KEYSPACE IF NOT EXISTS uprofile WITH REPLICATION = { 'class' : 'NetworkTopologyStrategy', 'datacenter1' : 1 }"; 
           session.execute(query); 
           LOGGER.info("Created keyspace 'uprofile'"); 
       } 
   
       /** 
        * Create user table in cassandra DB 
        */ 
   
       public void createTable() { 
           final String query = "CREATE TABLE IF NOT EXISTS uprofile.user (user_id int PRIMARY KEY, user_name text, user_bcity text)"; 
           session.execute(query); 
           LOGGER.info("Created table 'user'"); 
       } 
   } 
   ```

3. Busque la carpeta `src\main\java\com\azure\cosmosdb\cassandra` y cree una nueva subcarpeta denominada "`examples`".

4. A continuación, cree la clase de Java `UserProfile`. Esta clase contiene el método principal que llama a los métodos createKeyspace y createTable definidos anteriormente: 

   ```java
   package com.azure.cosmosdb.cassandra.examples; 
   import java.io.IOException; 
   import com.azure.cosmosdb.cassandra.repository.UserRepository; 
   import com.azure.cosmosdb.cassandra.util.CassandraUtils; 
   import com.datastax.driver.core.PreparedStatement; 
   import com.datastax.driver.core.Session; 
   import org.slf4j.Logger; 
   import org.slf4j.LoggerFactory; 
   
   /** 
    * Example class which will demonstrate following operations on Cassandra Database on CosmosDB 
    * - Create Keyspace 
    * - Create Table 
    * - Insert Rows 
    * - Select all data from a table 
    * - Select a row from a table 
    */ 
   
   public class UserProfile { 
   
       private static final Logger LOGGER = LoggerFactory.getLogger(UserProfile.class); 
       public static void main(String[] s) throws Exception { 
           CassandraUtils utils = new CassandraUtils(); 
           Session cassandraSession = utils.getSession(); 
   
           try { 
               UserRepository repository = new UserRepository(cassandraSession); 
               //Create keyspace in cassandra database 
               repository.createKeyspace(); 
               //Create table in cassandra database 
               repository.createTable(); 
   
           } finally { 
               utils.close(); 
               LOGGER.info("Please delete your table after verifying the presence of the data in portal or from CQL"); 
           } 
       } 
   } 
   ```
 
## <a name="run-the-app"></a>Ejecución de la aplicación 

1. Abra un símbolo del sistema o una ventana de terminal. Pegue el bloque de código siguiente. 

   Este código cambia el directorio (cd) a la ruta de acceso de la carpeta donde creó el proyecto. A continuación, se ejecuta el comando `mvn clean install` para generar el archivo `cosmosdb-cassandra-examples.jar` en la carpeta de destino. Finalmente, se ejecuta la aplicación de Java.

   ```bash
   cd cassandra-demo

   mvn clean install 

   java -cp target/cosmosdb-cassandra-examples.jar com.azure.cosmosdb.cassandra.examples.UserProfile 
   ```

   En la ventana de terminal se notifica que se ha creado el espacio de claves y la tabla. 
   
2. Ahora, en Azure Portal, abra el **Explorador de datos** para confirmar que el espacio de claves y la tabla se crearon.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a crear una cuenta de Cassandra API de Azure Cosmos DB, así como a agregar una base de datos y una tabla usando una aplicación de Java. Avance al siguiente artículo:

> [!div class="nextstepaction"]
> [Carga de datos de ejemplo en la tabla de Cassandra API](cassandra-api-load-data.md)
