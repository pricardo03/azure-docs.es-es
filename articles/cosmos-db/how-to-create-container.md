---
title: Creación de un contenedor en Azure Cosmos DB
description: Aprenda a crear un contenedor en Azure Cosmos DB.
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/06/2018
ms.author: mjbrown
ms.openlocfilehash: 5558409c3a3b0aef3757ebb73b2046a7018e4150
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53088194"
---
# <a name="create-a-container-in-azure-cosmos-db"></a>Creación de un contenedor en Azure Cosmos DB

En este artículo se explican las distintas formas de crear un contenedor (colección, tabla, grafo). Un contenedor puede crearse mediante Azure Portal, la CLI de Azure o con los SDK admitidos. En este artículo se muestra cómo crear un contenedor, especificar la clave de partición y aprovisionar el rendimiento.

## <a name="create-a-container-using-azure-portal"></a>Creación de un contenedor mediante Azure Portal

### <a id="portal-sql"></a>SQL API (Core)

1. Inicie sesión en el [portal de Azure](https://portal.azure.com/).

1. [Cree una cuenta de Cosmos DB](create-sql-api-dotnet.md#create-a-database-account) o seleccione una existente.

1. Abra el panel **Explorador de datos** y seleccione **Nueva colección**. A continuación, rellene el formulario con los siguientes datos:

   * Cree una base de datos o use una existente.
   * Escriba un identificador de la colección.
   * Escriba la clave de partición.
   * Escriba un rendimiento, por ejemplo 1000 RU.
   * Seleccione **Aceptar**.

![SQL API crea una colección.](./media/how-to-create-container/partitioned-collection-create-sql.png)

### <a id="portal-mongodb"></a>API de MongoDB

1. Inicie sesión en el [portal de Azure](https://portal.azure.com/).

1. [Cree una cuenta de Cosmos DB](create-mongodb-dotnet.md#create-a-database-account) o seleccione una existente.

1. Abra el panel **Explorador de datos** y seleccione **Nueva colección**. A continuación, rellene el formulario con los siguientes datos:

   * Cree una base de datos o use una existente.
   * Escriba un identificador de la colección.
   * Seleccione la capacidad de almacenamiento **Sin límite**.
   * Escriba una clave de partición.
   * Escriba un rendimiento, por ejemplo 1000 RU.
   * Seleccione **Aceptar**.

![La API de MongoDB crea una colección](./media/how-to-create-container/partitioned-collection-create-mongodb.png)

### <a id="portal-cassandra"></a>Cassandra API

1. Inicie sesión en el [portal de Azure](https://portal.azure.com/).

1. [Cree una cuenta de Cosmos DB](create-cassandra-dotnet.md#create-a-database-account) o seleccione una existente.

1. Abra el panel **Explorador de datos** y seleccione **Nueva tabla**. A continuación, rellene el formulario con los siguientes datos:

   * Cree un nuevo espacio de claves o use uno ya existente.
   * Escriba un nombre de tabla.
   * Especifique las propiedades y una clave principal.
   * Escriba un rendimiento, por ejemplo 1000 RU.
   * Seleccione **Aceptar**.

![Cassandra API crea una colección](./media/how-to-create-container/partitioned-collection-create-cassandra.png)

> [!NOTE]
> Con Cassandra API, la clave principal se usa como clave de partición.

### <a id="portal-gremlin"></a>Gremlin API

1. Inicie sesión en el [portal de Azure](https://portal.azure.com/).

1. [Cree una cuenta de Cosmos DB](create-graph-dotnet.md#create-a-database-account) o seleccione una existente.

1. Abra el panel **Explorador de datos** y seleccione **New Graph** (Nuevo grafo). A continuación, rellene el formulario con los siguientes datos:

   * Cree una base de datos o use una existente.
   * Escriba un identificador de grafo.
   * Seleccione la capacidad de almacenamiento **Sin límite**.
   * Escriba una clave de partición para los vértices.
   * Escriba un rendimiento, por ejemplo 1000 RU.
   * Seleccione **Aceptar**.

![Gremlin API crea una colección](./media/how-to-create-container/partitioned-collection-create-gremlin.png)

### <a id="portal-table"></a>Table API

1. Inicie sesión en el [portal de Azure](https://portal.azure.com/).

1. [Cree una cuenta de Cosmos DB](create-table-dotnet.md#create-a-database-account) o seleccione una existente.

1. Abra el panel **Explorador de datos** y seleccione **Nueva tabla**. A continuación, rellene el formulario con los siguientes datos:

   * Escriba un identificador de tabla.
   * Seleccione la capacidad de almacenamiento **Sin límite**.
   * Escriba un rendimiento, por ejemplo 1000 RU.
   * Seleccione **Aceptar**.

![Table API crea una colección](./media/how-to-create-container/partitioned-collection-create-table.png)

> [!Note]
> Con Table API, la clave de partición se especifica cada vez que se agrega una nueva fila.

## <a name="create-a-container-using-azure-cli"></a>Creación de un contenedor mediante la CLI de Azure

### <a id="cli-sql"></a>SQL API (Core)

```azurecli-interactive
# Create a container with a partition key and provision 1000 RU/s throughput.

az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $containerName \
    --name $accountName \
    --db-name $databaseName \
    --partition-key-path /myPartitionKey \
    --throughput 1000
```

### <a id="cli-mongodb">API de MongoDB</a>

```azurecli-interactive
# Create a collection with a shard key and provision 1000 RU/s throughput.
az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $collectionName \
    --name $accountName \
    --db-name $databaseName \
    --partition-key-path /myShardKey \
    --throughput 1000
```

### <a id="cli-cassandra"></a>Cassandra API

```azurecli-interactive
# Create a table with a partition/primary key and provision 1000 RU/s throughput.
az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $tableName \
    --name $accountName \
    --db-name $keyspaceName \
    --partition-key-path /myPrimaryKey \
    --throughput 1000
```

### <a id="cli-gremlin"></a>Gremlin API

```azurecli-interactive
# Create a graph with a partition key and provision 1000 RU/s throughput.
az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $graphName \
    --name $accountName \
    --db-name $databaseName \
    --partition-key-path /myPartitionKey \
    --throughput 1000
```

### <a id="cli-table"></a>Table API

```azurecli-interactive
# Create a table with 1000 RU/s
# Note: you don't need to specify partition key in the following command because the partition key is set on each row.
az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $tableName \
    --name $accountName \
    --db-name $databaseName \
    --throughput 1000
```

## <a name="create-a-container-using-net-sdk"></a>Creación de un contenedor mediante el SDK para .NET

### <a id="dotnet-sql-graph"></a>SQL API y Gremlin API

```csharp
// Create a container with a partition key and provision 1000 RU/s throughput.
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "myContainerName";
myCollection.PartitionKey.Paths.Add("/myPartitionKey");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("myDatabaseName"),
    myCollection,
    new RequestOptions { OfferThroughput = 1000 });
```

### <a id="dotnet-mongodb"></a>API de MongoDB

```csharp
// Create a collection with a partition key by using Mongo Shell:
db.runCommand( { shardCollection: "myDatabase.myCollection", key: { myShardKey: "hashed" } } )
```

> [!Note]
> MongoDB no tiene un concepto de unidades de solicitud. Para crear una colección con rendimiento, use Azure Portal o SQL API como se muestra en los ejemplos anteriores.

### <a id="dotnet-cassandra"></a>Cassandra API

```csharp
// Create a Cassandra table with a partition/primary key and provision 1000 RU/s throughput.
session.Execute(CREATE TABLE myKeySpace.myTable(
    user_id int PRIMARY KEY,
    firstName text,
    lastName text) WITH cosmosdb_provisioned_throughput=1000);
```

## <a name="next-steps"></a>Pasos siguientes

Consulte los siguientes artículos para información sobre la creación de particiones en Cosmos DB:

- [Creación de particiones en Azure Cosmos DB](partitioning-overview.md)
