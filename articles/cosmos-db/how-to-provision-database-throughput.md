---
title: Aprovisionamiento del rendimiento de base de datos en Azure Cosmos DB
description: Aprenda a aprovisionar el rendimiento en el nivel de base de datos en Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/06/2018
ms.author: mjbrown
ms.openlocfilehash: 7c253141e0e6e76f845d08e68a1d79949fe811e8
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2019
ms.locfileid: "54034240"
---
# <a name="provision-throughput-for-a-database-in-azure-cosmos-db"></a>Aprovisionamiento del rendimiento de una base de datos en Azure Cosmos DB

En este artículo se explica cómo aprovisionar el rendimiento de una base de datos en Azure Cosmos DB. Puede aprovisionar el rendimiento de un único [contenedor](how-to-provision-container-throughput.md), o bien de una base de datos y compartir el rendimiento entre los contenedores que se incluyen en ella. Para aprovisionar el rendimiento en el nivel de base de datos, se puede usar Azure Portal o el SDK de Cosmos DB.

## <a name="provision-throughput-using-azure-portal"></a>Aprovisionamiento del rendimiento mediante Azure Portal

### <a id="portal-sql"></a>SQL API (Core)

1. Inicie sesión en el [portal de Azure](https://portal.azure.com/).

1. [Cree una cuenta de Cosmos DB](create-sql-api-dotnet.md#create-a-database-account) o seleccione una existente.

1. Abra la hoja **Explorador de datos** y seleccione **Nueva base de datos**. A continuación, rellene el formulario con los siguientes datos:

   * Escriba un identificador de base de datos. 
   * Seleccione Aprovisionar rendimiento.
   * Escriba un rendimiento, por ejemplo 1000 RU.
   * Seleccione **Aceptar**.

![Aprovisionamiento del rendimiento de base de datos de SQL API](./media/how-to-provision-database-throughput/provision-database-throughput-portal-all-api.png)

## <a name="provision-throughput-using-net-sdk"></a>Aprovisionamiento del rendimiento mediante el SDK para .NET

> [!Note]
> Use SQL API para aprovisionar el rendimiento de todas las API. También puede usar el ejemplo siguiente para Cassandra API.

### <a id="dotnet-all"></a>Todas las API

```csharp
//set the throughput for the database
RequestOptions options = new RequestOptions
{
    OfferThroughput = 10000
};

//create the database
await client.CreateDatabaseIfNotExistsAsync(
    new Database {Id = databaseName},  
    options);
```

### <a id="dotnet-cassandra"></a>Cassandra API

```csharp
// Create a Cassandra keyspace and provision throughput of 10000 RU/s
session.Execute(CREATE KEYSPACE IF NOT EXISTS myKeySpace WITH cosmosdb_provisioned_throughput=10000);
```

## <a name="next-steps"></a>Pasos siguientes

Consulte los siguientes artículos para aprender sobre el aprovisionamiento del rendimiento en Cosmos DB:

* [How to provision throughput for a container](how-to-provision-container-throughput.md) (Aprovisionamiento del rendimiento de un contenedor)
* [Rendimiento y unidades de solicitud en Azure Cosmos DB](request-units.md)
