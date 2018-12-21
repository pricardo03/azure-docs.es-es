---
title: Consulta de contenedores en Azure Cosmos DB
description: Aprenda a consultar contenedores de Azure Cosmos DB.
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/06/2018
ms.author: mjbrown
ms.openlocfilehash: 11c68b61802f6c7b3755da71c176ea777f171e4c
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2018
ms.locfileid: "53409843"
---
# <a name="query-containers-in-azure-cosmos-db"></a>Consulta de contenedores en Azure Cosmos DB

En este artículo se explica cómo consultar un contenedor (colección, grafo, tabla) en Azure Cosmos DB.

## <a name="in-partition-query"></a>Consulta en la partición

Al consultar datos desde contenedores,si la consulta tiene especificado un filtro de clave de partición, Azure Cosmos DB enruta automáticamente la consulta a las particiones correspondientes a los valores de clave de partición especificados en el filtro. Por ejemplo, la siguiente consulta se enruta a la partición de DeviceId que contiene todos los documentos correspondientes al valor de clave de partición "XMS-0001".

```csharp
// Query using partition key into a class called, DeviceReading
IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("myDatabaseName", "myCollectionName"))
    .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");
```

## <a name="cross-partition-query"></a>Consulta entre particiones

La consulta siguiente no tiene filtro en la clave de partición (DeviceId) y por ello se despliega por todas las particiones, en las que se ejecuta según el índice de la partición. Para ejecutar una consulta entre particiones, establezca **EnableCrossPartitionQuery** en true (o x-ms-documentdb-query-enablecrosspartition en la API REST).

```csharp
// Query across partition keys into a class called, DeviceReading
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("myDatabaseName", "myCollectionName"),
    new FeedOptions { EnableCrossPartitionQuery = true })
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);
```

Cosmos DB admite las funciones de agregado COUNT, MIN, MAX y AVG a través de contenedores que usan SQL. Las funciones de agregado a través de contenedores comienzan a partir de la versión 1.12.0 o superior del SDK. Las consultas deben incluir un único operador de agregado y un valor individual en la proyección.

## <a name="parallel-cross-partition-query"></a>Consulta entre particiones en paralelo

Los SDK 1.9.0 y posterior de Cosmos DB admiten opciones de ejecución de consultas en paralelo.  Las consultas entre particiones en paralelo permiten realizar consultas de baja latencia entre particiones. Por ejemplo, la siguiente consulta está configurada para ejecutarse en paralelo en particiones.

```csharp
// Cross-partition Order By Query with parallel execution
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("myDatabaseName", "myCollectionName"),  
    new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
    .OrderBy(m => m.MetricValue);
```

Puede administrar la ejecución de consultas en paralelo ajustando los parámetros siguientes:

- **MaxDegreeOfParallelism**: establece el número máximo de conexiones de red simultáneas a las particiones del contenedor. Si esta propiedad se establece en -1, el SDK administra el grado de paralelismo. Si el parámetro MaxDegreeOfParallelism no se especifica o está establecido en 0, que es el valor predeterminado, habrá una única conexión de red a las particiones del contenedor.

- **MaxBufferedItemCount**: equilibra la latencia de las consultas frente al uso de memoria del lado cliente. Si se omite esta opción o se establece en -1, el SDK administra el número de elementos almacenados en búfer durante la ejecución de consultas en paralelo.

Con el mismo estado de la colección, una consulta en paralelo devolverá resultados en el mismo orden que la ejecución en serie. Al realizar una consulta entre particiones que incluye operadores de ordenación (ORDER BY o TOP), el SDK de Azure Cosmos DB emite la consulta en paralelo entre las particiones y combina los resultados ordenados parcialmente en el lado cliente para generar resultados ordenados globalmente.

## <a name="next-steps"></a>Pasos siguientes

Consulte los siguientes artículos para información sobre la creación de particiones en Cosmos DB:

- [Creación de particiones en Azure Cosmos DB](partitioning-overview.md)
- [Synthetic partition keys in Azure Cosmos DB](synthetic-partition-keys.md) (Claves de partición sintéticas en Azure Cosmos DB)
