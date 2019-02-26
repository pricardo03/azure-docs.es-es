---
title: Configuración de la arquitectura multimaestro en Azure Cosmos DB
description: Aprenda a configurar la arquitectura multimaestro en las aplicaciones de Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 2/12/2019
ms.author: mjbrown
ms.openlocfilehash: 84c8e2921602bb653c0b1ef0adffd3d89e91bd78
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/15/2019
ms.locfileid: "56312147"
---
# <a name="how-to-configure-multi-master-in-your-applications-that-use-azure-cosmos-db"></a>Configuración de la arquitectura multimaestro en las aplicaciones que usan Azure Cosmos DB

Para usar las características de la arquitectura multimaestro en las aplicaciones, deberá habilitar la escritura de varias regiones y configurar la funcionalidad de host múltiple. El hospedaje múltiple se configura mediante el establecimiento de la región actual donde se implementa la aplicación.

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

Para habilitar la arquitectura multimaestro en las aplicaciones, establezca `policy.setUsingMultipleWriteLocations(true)` en true y configure `policy.setPreferredLocations` para la región en la que se va a implementar la aplicación y se replica Cosmos DB.

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

Más información sobre la arquitectura multimaestro, la distribución global y la coherencia en Azure Cosmos DB. Consulte los artículos siguientes:

* [Usar tokens de sesión para administrar la coherencia en Azure Cosmos DB](how-to-manage-consistency.md#utilize-session-tokens)

* [Tipos de conflicto y directivas de resolución de conflictos en Azure Cosmos DB](conflict-resolution-policies.md)

* [Alta disponibilidad en Azure Cosmos DB](high-availability.md)

* [Selección del nivel de coherencia adecuado en Azure Cosmos DB](consistency-levels-choosing.md)

* [Inconvenientes de la coherencia, disponibilidad y rendimiento en Azure Cosmos DB](consistency-levels-tradeoffs.md)
