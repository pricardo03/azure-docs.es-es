---
title: Configuración de la arquitectura multimaestro en Azure Cosmos DB
description: Aprenda a configurar la arquitectura multimaestro en las aplicaciones de Azure Cosmos DB
author: rimman
ms.service: cosmos-db
ms.topic: sample
ms.date: 04/15/2019
ms.author: rimman
ms.openlocfilehash: b862c59002369662d37b6d6a9de28370b0000497
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59682277"
---
# <a name="how-to-configure-multi-master-in-your-applications-that-use-azure-cosmos-db"></a>Configuración de la arquitectura multimaestro en las aplicaciones que usan Azure Cosmos DB

Para usar la característica de la arquitectura multimaestro en la aplicación, deberá habilitar la escritura en varias regiones y configurar la funcionalidad de hospedaje múltiple en Azure Cosmos DB. El hospedaje múltiple se configura mediante el establecimiento de la región donde se implementa la aplicación.

## <a id="netv2"></a>.NET SDK v2

Para habilitar la arquitectura multimaestro en las aplicaciones, establezca `UseMultipleWriteLocations` en true y configure `SetCurrentLocation` para la región en la que se va a implementar la aplicación y se replica Azure Cosmos DB.

```csharp
ConnectionPolicy policy = new ConnectionPolicy
    {
        ConnectionMode = ConnectionMode.Direct,
        ConnectionProtocol = Protocol.Tcp,
        UseMultipleWriteLocations = true
    };
policy.SetCurrentLocation("West US 2");
```

## <a id="netv3"></a>.NET SDK v3 (versión preliminar)

Para habilitar la arquitectura multimaestro en las aplicaciones, configure `UseCurrentRegion` en la región en la que se va a implementar la aplicación y se replica Cosmos DB.

```csharp
CosmosConfiguration config = new CosmosConfiguration("endpoint", "key");
config.UseCurrentRegion("West US");
CosmosClient client = new CosmosClient(config);
```

## <a id="java"></a>SDK asincrónico para Java

Para habilitar la arquitectura multimaestro en las aplicaciones, establezca `policy.setUsingMultipleWriteLocations(true)` y configure `policy.setPreferredLocations` para la región en la que se va a implementar la aplicación y se replica Cosmos DB.

```java
ConnectionPolicy policy = new ConnectionPolicy();
policy.setUsingMultipleWriteLocations(true);
policy.setPreferredLocations(Collections.singletonList(region));

AsyncDocumentClient client =
    new AsyncDocumentClient.Builder()
        .withMasterKeyOrResourceToken(this.accountKey)
        .withServiceEndpoint(this.accountEndpoint)
        .withConsistencyLevel(ConsistencyLevel.Eventual)
        .withConnectionPolicy(policy).build();
```

## <a id="javascript"></a>SDK de Node.js, JavaScript, TypeScript

Para habilitar la arquitectura multimaestro en las aplicaciones, establezca `connectionPolicy.UseMultipleWriteLocations` en true y configure `connectionPolicy.PreferredLocations` para la región en la que se va a implementar la aplicación y se replica Cosmos DB.

```javascript
const connectionPolicy: ConnectionPolicy = new ConnectionPolicy();
connectionPolicy.UseMultipleWriteLocations = true;
connectionPolicy.PreferredLocations = [region];

const client = new CosmosClient({
  endpoint: config.endpoint,
  auth: { masterKey: config.key },
  connectionPolicy,
  consistencyLevel: ConsistencyLevel.Eventual
});
```

## <a id="python"></a>SDK para Python

Para habilitar la arquitectura multimaestro en las aplicaciones, establezca `connection_policy.UseMultipleWriteLocations` en true y configure `connection_policy.PreferredLocations` para la región en la que se va a implementar la aplicación y se replica Cosmos DB.

```python
connection_policy = documents.ConnectionPolicy()
connection_policy.UseMultipleWriteLocations = True
connection_policy.PreferredLocations = [region]

client = cosmos_client.CosmosClient(self.account_endpoint, {'masterKey': self.account_key}, connection_policy, documents.ConsistencyLevel.Session)
```

## <a name="next-steps"></a>Pasos siguientes

Ahora puede pasar a la lectura de los artículos siguientes:

* [Usar tokens de sesión para administrar la coherencia en Azure Cosmos DB](how-to-manage-consistency.md#utilize-session-tokens)
* [Tipos de conflicto y directivas de resolución de conflictos en Azure Cosmos DB](conflict-resolution-policies.md)
* [Alta disponibilidad en Azure Cosmos DB](high-availability.md)
* [Niveles de coherencia en Azure Cosmos DB](consistency-levels.md)
* [Selección del nivel de coherencia adecuado en Azure Cosmos DB](consistency-levels-choosing.md)
* [Inconvenientes de la coherencia, disponibilidad y rendimiento en Azure Cosmos DB](consistency-levels-tradeoffs.md)
* [Availability and performance tradeoffs for various consistency levels](consistency-levels-tradeoffs.md) (Compromisos entre rendimiento y disponibilidad en los distintos niveles de coherencia)
* [Escalado del rendimiento aprovisionado globalmente](scaling-throughput.md)
* [Distribución global en segundo plano](global-dist-under-the-hood.md)
