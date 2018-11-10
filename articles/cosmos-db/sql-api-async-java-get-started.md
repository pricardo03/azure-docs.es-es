---
title: Creación de una aplicación Java con el SDK de JavaScript para administrar los datos de SQL API de Azure Cosmos DB | Microsoft Docs
description: Este tutorial muestra cómo usar las cuentas de SQL API de Azure Cosmos DB para almacenar y acceder a los datos mediante una aplicación de Async Java.
keywords: tutorial de nosql, base de datos en línea, aplicación de consola de java
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: java
ms.topic: tutorial
ms.date: 06/29/2018
ms.author: sngun
ms.openlocfilehash: 66e937e92528e2f0a1fca9d9aac78f7265eef4f7
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2018
ms.locfileid: "50741239"
---
# <a name="tutorial-build-a-java-app-with-async-java-sdk-to-manage-azure-cosmos-db-sql-api-data"></a>Tutorial: Creación de una aplicación Java con el SDK de JavaScript para administrar los datos de SQL API de Azure Cosmos DB

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> 

En este tutorial se muestra cómo crear una aplicación Java con el SDK de Async Java para almacenar y acceder a los datos de SQL API de Azure Cosmos DB.

En este tutorial se describen las tareas siguientes:

> [!div class="checklist"]
> * Creación de una cuenta de Azure Cosmos DB y conexión a ella
> * Configuración de la solución
> * Creación de una colección
> * Creación de documentos JSON
> * Consulta de la colección

## <a name="prerequisites"></a>Requisitos previos

Asegúrese de que dispone de los siguientes recursos:

* Una cuenta de Azure activa. Si no tiene una, puede registrarse para obtener una [cuenta gratuita](https://azure.microsoft.com/free/). 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [Git](https://git-scm.com/downloads).
* [Kit de desarrollo de Java (JDK) 8+](https://aka.ms/azure-jdks).
* [Maven](http://maven.apache.org/download.cgi).

## <a name="step-1-create-an-azure-cosmos-db-account"></a>Paso 1: Creación de una cuenta de Azure Cosmos DB
Vamos a crear una cuenta de Azure Cosmos DB. Si ya tiene una cuenta que desea usar, puede ir directamente a [Clonar el proyecto de GitHub](#GitClone). Si está usando el Emulador de Azure Cosmos DB, siga los pasos que se indican en el [Emulador de Azure Cosmos DB](local-emulator.md) para configurar el emulador y vaya directamente a [Clonar el proyecto de GitHub](#GitClone).

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="GitClone"></a>Paso 2: Clonación del repositorio de GitHub

En primer lugar, clone el repositorio de GitHub para [empezar a trabajar con Azure Cosmos DB y Java](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-async-java-getting-started). Por ejemplo, desde un directorio local, ejecute lo siguiente para recuperar el proyecto de ejemplo localmente.

```bash
git clone https://github.com/Azure-Samples/azure-cosmos-db-sql-api-async-java-getting-started.git

cd azure-cosmos-db-sql-api-async-java-getting-started
cd azure-cosmosdb-get-started
```

El directorio contiene una carpeta `pom.xml` y `src/main/java/com/microsoft/azure/cosmosdb/sample` que contiene el código fuente Java, incluido `Main.java`. El proyecto contiene el código necesario para realizar operaciones con Azure Cosmos DB, como la creación de documentos y la consulta de datos dentro de una colección. El archivo `pom.xml` incluye una dependencia en el [SDK de Java para Azure Cosmos DB en Maven](https://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb).

```xml
<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>azure-documentdb</artifactId>
  <version>2.0.0</version>
</dependency>
```

## <a id="Connect"></a>Paso 3: Conexión a una cuenta de Azure Cosmos DB

A continuación, vuelva a [Azure Portal](https://portal.azure.com) para recuperar el punto de conexión y la clave maestra principal. El punto de conexión y la clave principal de Azure Cosmos DB son necesarios para que la aplicación sepa a dónde debe conectarse y para que Azure Cosmos DB confíe en la conexión de la aplicación. El archivo `AccountSettings.java` contiene los valores de URI y la clave principal. 

En Azure Portal, vaya a la cuenta de Azure Cosmos DB y haga clic en **Claves**. Copie la CLAVE PRINCIPAL y el URI del portal, y péguelos en el archivo `AccountSettings.java`. 

```java
public class AccountSettings 
{
  // Replace MASTER_KEY and HOST with values from your Azure Cosmos DB account.
    
  // <!--[SuppressMessage("Microsoft.Security", "CS002:SecretInNextLine")]-->
  public static String MASTER_KEY = System.getProperty("ACCOUNT_KEY", 
          StringUtils.defaultString(StringUtils.trimToNull(
          System.getenv().get("ACCOUNT_KEY")), "<Fill your Azure Cosmos DB account key>"));

  public static String HOST = System.getProperty("ACCOUNT_HOST",
           StringUtils.defaultString(StringUtils.trimToNull(
           System.getenv().get("ACCOUNT_HOST")),"<Fill your Azure Cosmos DB URI>"));
}
```

![Captura de pantalla de la obtención de claves desde el portal][keys]

## <a name="step-4-initialize-the-client-object"></a>Paso 4: Inicialización del objeto de cliente

Inicialice el objeto de cliente con los valores de la clave principal y el URI de host definidos en el archivo "AccountSettings.java".

```java
client = new AsyncDocumentClient.Builder()
         .withServiceEndpoint(AccountSettings.HOST)
         .withMasterKey(AccountSettings.MASTER_KEY)
         .withConnectionPolicy(ConnectionPolicy.GetDefault())
         .withConsistencyLevel(ConsistencyLevel.Session)
         .build();
```

## <a id="CreateDatabase"></a>Paso 5: Creación de una base de datos

Puede crearse una [base de datos](sql-api-resources.md#databases) de Azure Cosmos DB mediante el método createDatabaseIfNotExists() de la clase DocumentClient. Una base de datos es un contenedor lógico de almacenamiento de documentos JSON particionado en recopilaciones.

```java
private void createDatabaseIfNotExists() throws Exception 
{
    
    writeToConsoleAndPromptToContinue("Check if database " + databaseName + " exists.");

    String databaseLink = String.format("/dbs/%s", databaseName);

    Observable<ResourceResponse<Database>> databaseReadObs = client.readDatabase(databaseLink, null);

    Observable<ResourceResponse<Database>> databaseExistenceObs = databaseReadObs
                .doOnNext(x -> {System.out.println("database " + databaseName + " already exists.");}).onErrorResumeNext(e -> {
   // if the database doesn't already exists
   // readDatabase() will result in 404 error
   if (e instanceof DocumentClientException) 
   {
       DocumentClientException de = (DocumentClientException) e;
       // if database 
       if (de.getStatusCode() == 404) {
       // if the database doesn't exist, create it.
       System.out.println("database " + databaseName + " doesn't existed," + " creating it...");
       Database dbDefinition = new Database();
       dbDefinition.setId(databaseName);
       return client.createDatabase(dbDefinition, null);
     }
     }

    // some unexpected failure in reading database happened.
    // pass the error up.
    System.err.println("Reading database " + databaseName + " failed.");
        return Observable.error(e);     
    });

   // wait for completion
   databaseExistenceObs.toCompletable().await();

   System.out.println("Checking database " + databaseName + " completed!\n");
}
```

## <a id="CreateColl"></a>Paso 6: Creación de una colección

> [!WARNING]
> **createCollection** creará una colección con un rendimiento reservado, lo que tiene implicaciones en los precios. Para más información, visite nuestra [página de precios](https://azure.microsoft.com/pricing/details/cosmos-db/).
> 
> 
Se puede crear una colección con el método createDocumentCollectionIfNotExists() de la clase DocumentClient. Una colección es un contenedor de documentos JSON asociado a la lógica de aplicación de JavaScript.

```java
private void createDocumentCollectionIfNotExists() throws Exception 
{
    
    writeToConsoleAndPromptToContinue("Check if collection " + collectionName + " exists.");

    // query for a collection with a given id
    // if it exists nothing else to be done
    // if the collection doesn't exist, create it.

    String databaseLink = String.format("/dbs/%s", databaseName);

    // we know there is only single page of result (empty or with a match)
    client.queryCollections(databaseLink, new SqlQuerySpec("SELECT * FROM r where r.id = @id", 
            new SqlParameterCollection(new SqlParameter("@id", collectionName))), null).single() 
            .flatMap(page -> {
            if (page.getResults().isEmpty()) {
             // if there is no matching collection create the collection.
             DocumentCollection collection = new DocumentCollection();
             collection.setId(collectionName);
             System.out.println("Creating collection " + collectionName);

             return client.createCollection(databaseLink, collection, null);
            } 
            else {
              // collection already exists, nothing else to be done.
              System.out.println("Collection " + collectionName + "already exists");
              return Observable.empty();
            }
      }).toCompletable().await();

 System.out.println("Checking collection " + collectionName + " completed!\n");
    }
```

## <a id="CreateDoc"></a>Paso 7: Creación de documentos JSON

Para crear un [documento](sql-api-resources.md#documents), puede usar el método createDocument de la clase DocumentClient. Los documentos son contenido JSON definido por el usuario (arbitrario). Ahora podemos insertar uno o varios documentos. El archivo "src/main/java/com/microsoft/azure/cosmosdb/sample/Families.java" define los documentos JSON de la familia. 

```java
public static Family getJohnsonFamilyDocument() {
     Family andersenFamily = new Family();
     andersenFamily.setId("Johnson" + System.currentTimeMillis());
     andersenFamily.setLastName("Johnson");

      Parent parent1 = new Parent();
      parent1.setFirstName("John");

      Parent parent2 = new Parent();
      parent2.setFirstName("Lili");

      return andersenFamily;
    }
```

## <a id="Query"></a>Paso 8: Consulta de los recursos de Azure Cosmos DB

Azure Cosmos DB admite [consultas](sql-api-sql-query.md) enriquecidas en los documentos JSON que se almacenan en las colecciones. El código de ejemplo siguiente muestra cómo consultar documentos de Azure Cosmos DB mediante la sintaxis SQL con el método [queryDocuments](/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.querydocuments).

```java
private void executeSimpleQueryAsyncAndRegisterListenerForResult(CountDownLatch completionLatch) 
{
    // Set some common query options
    FeedOptions queryOptions = new FeedOptions();
    queryOptions.setMaxItemCount(100);
    queryOptions.setEnableCrossPartitionQuery(true);

    String collectionLink = String.format("/dbs/%s/colls/%s", databaseName, collectionName);
    Observable<FeedResponse<Document>> queryObservable = client.queryDocuments(collectionLink,
           "SELECT * FROM Family WHERE Family.lastName = 'Andersen'", queryOptions);

    queryObservable.subscribe(queryResultPage -> {
            System.out.println("Got a page of query result with " +
            queryResultPage.getResults().size() + " document(s)"
            + " and request charge of " + queryResultPage.getRequestCharge());
    },
   // terminal error signal
        e -> {
          e.printStackTrace();
          completionLatch.countDown();
        },

   // terminal completion signal
         () -> {
            completionLatch.countDown();
         });
}
```

## <a id="Run"></a>Paso 9: Ejecución de la aplicación de consola de Java

Para ejecutar la aplicación desde la consola, navegue hasta la carpeta del proyecto y realice la compilación con Maven:

```bash
mvn package
```

La ejecución de `mvn package` descarga la biblioteca de Azure Cosmos DB más reciente de Maven y genera `GetStarted-0.0.1-SNAPSHOT.jar`. A continuación, ejecute la aplicación con:

```bash
mvn exec:java -DACCOUNT_HOST=<YOUR_COSMOS_DB_HOSTNAME> -DACCOUNT_KEY= <YOUR_COSMOS_DB_MASTER_KEY>
```

Felicidades. Ha completado este tutorial de NoSQL y tiene una aplicación de consola de Java en funcionamiento.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a crear una aplicación Java con el SDK de Async Java para administrar los datos de SQL API de Azure Cosmos DB. Avance al siguiente artículo:

> [!div class="nextstepaction"]
> [Creación de una aplicación de consola de Node.js con el SDK de JavaScript y Azure Cosmos DB](sql-api-nodejs-get-started.md)

[keys]: media/sql-api-get-started/nosql-tutorial-keys.png
