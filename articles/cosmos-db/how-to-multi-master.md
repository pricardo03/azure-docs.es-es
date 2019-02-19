---
title: Configuración de la arquitectura multimaestro en Azure Cosmos DB
description: Aprenda a configurar la arquitectura multimaestro en las aplicaciones de Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 2/12/2019
ms.author: mjbrown
ms.openlocfilehash: effe6fa942ce0cabace08e72dba90baf8646680e
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2019
ms.locfileid: "56118863"
---
# <a name="how-to-configure-multi-master-in-your-applications-in-azure-cosmos-db"></a>Configuración de la arquitectura multimaestro en las aplicaciones de Azure Cosmos DB

Para usar las características de la arquitectura multimaestro en las aplicaciones, debe habilitar las escrituras de varias regiones en la aplicación y configurar la funcionalidad de hospedaje múltiple mediante el establecimiento de la región actual en la que está implementada la aplicación.

## <a id="netv2"></a>.NET SDK v2

Para habilitar la arquitectura multimaestro en las aplicaciones, establezca `UseMultipleWriteLocations` en true y configure `SetCurrentLocation` para la región en la que se va a implementar la aplicación y se replica Cosmos DB.

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
