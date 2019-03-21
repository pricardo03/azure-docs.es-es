---
title: Creación de un contenedor en Azure Cosmos DB
description: Aprenda a crear un contenedor en Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/06/2018
ms.author: mjbrown
ms.openlocfilehash: 8ce890500c31c1966254e5bca9d23c8fcdd7bb67
ms.sourcegitcommit: aa3be9ed0b92a0ac5a29c83095a7b20dd0693463
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58258299"
---
# <a name="create-an-azure-cosmos-container"></a>Creación de un contenedor de Azure Cosmos

En este artículo se explican las distintas formas de crear un contenedor (colección, tabla, grafo). Use Azure Portal, la CLI de Azure o SDK admitidos. En este artículo se muestra cómo crear un contenedor, especificar la clave de partición y aprovisionar el rendimiento.

## <a name="create-a-container-by-using-azure-portal"></a>Creación de un contenedor mediante Azure Portal

### <a id="portal-sql"></a>SQL API

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).

1. [Cree una nueva cuenta de Azure Cosmos DB](create-sql-api-dotnet.md#create-account) o seleccione una existente.

1. Abra el panel **Data Explorer** y seleccione **Nueva colección**. Después, proporcione los detalles siguientes:

   * Indique si va a crear una nueva base de datos o a usar una existente.
   * Escriba el identificador de la colección.
   * Escriba la clave de partición.
   * Escriba un rendimiento, por ejemplo 1000 RU.
   * Seleccione **Aceptar**.

![Panel de la captura de pantalla de Data Explorer, con Nueva colección resaltado](./media/how-to-create-container/partitioned-collection-create-sql.png)

### <a id="portal-mongodb"></a>Azure Cosmos DB API para MongoDB

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).

1. [Cree una nueva cuenta de Azure Cosmos DB](create-mongodb-dotnet.md#create-a-database-account) o seleccione una existente.

1. Abra el panel **Data Explorer** y seleccione **Nueva colección**. Después, proporcione los detalles siguientes:

   * Indique si va a crear una nueva base de datos o a usar una existente.
   * Escriba el identificador de la colección.
   * Seleccione la capacidad de almacenamiento **Sin límite**.
   * Escriba una clave de partición.
   * Escriba un rendimiento, por ejemplo 1000 RU.
   * Seleccione **Aceptar**.

![Captura de pantalla del cuadro de diálogo Agregar colección de Azure Cosmos DB API para MongoDB](./media/how-to-create-container/partitioned-collection-create-mongodb.png)

### <a id="portal-cassandra"></a>Cassandra API

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).

1. [Cree una nueva cuenta de Azure Cosmos DB](create-cassandra-dotnet.md#create-a-database-account) o seleccione una existente.

1. Abra el panel **Data Explorer** y seleccione **Nueva tabla**. Después, proporcione los detalles siguientes:

   * Indique si va a crear un nuevo espacio de claves o a usar uno existente.
   * Escriba un nombre de tabla.
   * Especifique las propiedades y una clave principal.
   * Escriba un rendimiento, por ejemplo 1000 RU.
   * Seleccione **Aceptar**.

![Captura de pantalla del cuadro de diálogo Agregar tabla de Cassandra API](./media/how-to-create-container/partitioned-collection-create-cassandra.png)

> [!NOTE]
> Con Cassandra API, la clave principal se usa como clave de partición.

### <a id="portal-gremlin"></a>Gremlin API

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).

1. [Cree una nueva cuenta de Azure Cosmos DB](create-graph-dotnet.md#create-a-database-account) o seleccione una existente.

1. Abra el panel de **Data Explorer** y seleccione **New Graph** (Nuevo grafo). Después, proporcione los detalles siguientes:

   * Indique si va a crear una nueva base de datos o a usar una existente.
   * Escriba un identificador de grafo.
   * Seleccione la capacidad de almacenamiento **Sin límite**.
   * Escriba una clave de partición para los vértices.
   * Escriba un rendimiento, por ejemplo 1000 RU.
   * Seleccione **Aceptar**.

![Captura de pantalla del cuadro de diálogo Agregar grafo de la API Gremlin](./media/how-to-create-container/partitioned-collection-create-gremlin.png)

### <a id="portal-table"></a>Table API

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).

1. [Cree una nueva cuenta de Azure Cosmos DB](create-table-dotnet.md#create-a-database-account) o seleccione una existente.

1. Abra el panel **Data Explorer** y seleccione **Nueva tabla**. Después, proporcione los detalles siguientes:

   * Escriba un identificador de tabla.
   * Seleccione la capacidad de almacenamiento **Sin límite**.
   * Escriba un rendimiento, por ejemplo 1000 RU.
   * Seleccione **Aceptar**.

![Captura de pantalla del cuadro de diálogo Agregar tabla de Table API](./media/how-to-create-container/partitioned-collection-create-table.png)

> [!Note]
> Con Table API, la clave de partición se especifica cada vez que se agrega una nueva fila.

## <a name="create-a-container-by-using-azure-cli"></a>Creación de un contenedor mediante la CLI de Azure

### <a id="cli-sql"></a>SQL API

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

### <a id="cli-mongodb"></a>Azure Cosmos DB API para MongoDB

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

## <a name="create-a-container-by-using-net-sdk"></a>Creación de un contenedor mediante el SDK para .NET

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

### <a id="dotnet-mongodb"></a>Azure Cosmos DB API para MongoDB

```csharp
// Create a collection with a partition key by using Mongo Shell:
db.runCommand( { shardCollection: "myDatabase.myCollection", key: { myShardKey: "hashed" } } )
```

> [!Note]
> El protocolo de conexión MongoDB no solicita unidades. Para crear una colección con rendimiento, use Azure Portal o SQL API.

### <a id="dotnet-cassandra"></a>Cassandra API

```csharp
// Create a Cassandra table with a partition/primary key and provision 1000 RU/s throughput.
session.Execute(CREATE TABLE myKeySpace.myTable(
    user_id int PRIMARY KEY,
    firstName text,
    lastName text) WITH cosmosdb_provisioned_throughput=1000);
```

## <a name="next-steps"></a>Pasos siguientes

- [Creación de particiones en Azure Cosmos DB](partitioning-overview.md)
