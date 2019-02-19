---
title: Aprovisionamiento del rendimiento de contenedor en Azure Cosmos DB
description: Aprenda a aprovisionar el rendimiento en el nivel de contenedor en Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/06/2018
ms.author: mjbrown
ms.openlocfilehash: 4df8a12581b5d71a76964ca1e3d40c6c53185f67
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55860327"
---
# <a name="provision-throughput-on-an-azure-cosmos-container"></a>Aprovisionamiento del rendimiento en un contenedor de Azure Cosmos

En este artículo se explica cómo aprovisionar el rendimiento de un contenedor (colección, grafo, tabla) en Azure Cosmos DB. Puede aprovisionar el rendimiento de un único contenedor o [aprovisionar el de una base de datos](how-to-provision-database-throughput.md) y compartirlo entre los contenedores incluidos en ella. Para aprovisionar el rendimiento de un contenedor, use Azure Portal, la CLI de Azure o los SDK de Azure Cosmos DB.

## <a name="provision-throughput-by-using-azure-portal"></a>Aprovisionamiento del rendimiento mediante Azure Portal

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).

1. [Cree una nueva cuenta de Azure Cosmos DB](create-sql-api-dotnet.md#create-a-database-account) o seleccione una existente.

1. Abra el panel **Data Explorer** y seleccione **Nueva colección**. Después, proporcione los detalles siguientes:

   * Indique si va a crear una nueva base de datos o a usar una existente.
   * Escriba un identificador de colección (o de tabla o grafo).
   * Escriba un valor de la clave de partición (por ejemplo, `/userid`).
   * Escriba un rendimiento, por ejemplo 1000 RU.
   * Seleccione **Aceptar**.

![Captura de pantalla de Data Explorer, con Nueva colección resaltado](./media/how-to-provision-container-throughput/provision-container-throughput-portal-all-api.png)

## <a name="provision-throughput-by-using-azure-cli"></a>Aprovisionamiento del rendimiento mediante la CLI de Azure

```azurecli-interactive
# Create a container with a partition key and provision throughput of 1000 RU/s
az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $containerName \
    --name $accountName \
    --db-name $databaseName \
    --partition-key-path /myPartitionKey \
    --throughput 1000
```

Si va a aprovisionar el rendimiento para una cuenta de Azure Cosmos DB configurada con la API de Azure Cosmos DB para MongoDB, utilice `/myShardKey` para la ruta de acceso de clave de partición. Si va a aprovisionar el rendimiento para una cuenta de Azure Cosmos DB configurada para Cassandra API, utilice `/myPrimaryKey` para la ruta de acceso de la clave de partición.

## <a name="provision-throughput-by-using-net-sdk"></a>Aprovisionamiento del rendimiento mediante el SDK para .NET

> [!Note]
> Use SQL API para el aprovisionamiento del rendimiento de todas las API, excepto Cassandra API.

### <a id="dotnet-most"></a>API de MongoDB y versiones de SQL, Gremlin y Table API

```csharp
// Create a container with a partition key and provision throughput of 1000 RU/s
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "myContainerName";
myCollection.PartitionKey.Paths.Add("/myPartitionKey");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("myDatabaseName"),
    myCollection,
    new RequestOptions { OfferThroughput = 1000 });
```

### <a id="dotnet-cassandra"></a>Cassandra API

```csharp
// Create a Cassandra table with a partition (primary) key and provision throughput of 1000 RU/s
session.Execute(CREATE TABLE myKeySpace.myTable(
    user_id int PRIMARY KEY,
    firstName text,
    lastName text) WITH cosmosdb_provisioned_throughput=1000);
```

## <a name="next-steps"></a>Pasos siguientes

Consulte los siguientes artículos para aprender sobre el aprovisionamiento del rendimiento en Azure Cosmos DB:

* [How to provision throughput for a database](how-to-provision-database-throughput.md) (Aprovisionamiento del rendimiento de una base de datos)
* [Rendimiento y unidades de solicitud en Azure Cosmos DB](request-units.md)
