---
title: Configuración de la arquitectura multimaestro en Azure Cosmos DB
description: Aprenda a configurar la arquitectura multimaestro en las aplicaciones de Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 07/02/2019
ms.author: mjbrown
ms.openlocfilehash: 73b4ada713e264aaa2504fe4d4f504e07ae45181
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538092"
---
# <a name="configure-multi-master-in-your-applications-that-use-azure-cosmos-db"></a>Configuración de la arquitectura multimaestro en las aplicaciones que usan Azure Cosmos DB

Una vez creada una cuenta con varias regiones de escritura habilitadas, debe realizar dos cambios en la aplicación en ConnectionPolicy para DocumentClient para habilitar las funcionalidades de hospedaje múltiple y arquitectura multimaestro en Azure Cosmos DB. En ConnectionPolicy, establezca UseMultipleWriteLocations en true y pase el nombre de la región donde se implementa la aplicación a SetCurrentLocation. Se rellena la propiedad PreferredLocations según la proximidad geográfica de la ubicación pasada. Si más adelante se agrega una nueva región a la cuenta, la aplicación no tiene que actualizarse ni volver a implementarse; se detecta automáticamente la región más cercana y se hospeda por sí sola en ella si se produce un evento regional.

> [!TIP]
> Las cuentas de Cosmos creadas no configuradas inicialmente para arquitectura multimaestro se pueden migrar con cero tiempo de inactividad. Para más información, consulte, [Configuración de varias regiones de escritura](how-to-manage-database-account.md#configure-multiple-write-regions).

## <a id="netv2"></a>.NET SDK v2

Para habilitar la arquitectura multimaestro en la aplicación, establezca `UseMultipleWriteLocations` en `true`. Además, establezca `SetCurrentLocation` en la región en que se va a implementar la aplicación y donde se va a replicar Azure Cosmos DB:

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

Para habilitar la arquitectura multimaestro en la aplicación, establezca `UseCurrentRegion` en la región en la que se va a implementar la aplicación y donde se va a replicar Cosmos DB.

```csharp
CosmosConfiguration config = new CosmosConfiguration("endpoint", "key");
config.UseCurrentRegion("West US");
CosmosClient client = new CosmosClient(config);
```

## <a id="java"></a>SDK asincrónico para Java

Para habilitar la arquitectura multimaestro en la aplicación, establezca `policy.setUsingMultipleWriteLocations(true)` y establezca `policy.setPreferredLocations` en la región en la que se va a implementar la aplicación y donde se va a replicar Cosmos DB:

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

## <a id="javascript"></a>SDK de Node.js, JavaScript y TypeScript

Para habilitar la arquitectura multimaestro en la aplicación, establezca `connectionPolicy.UseMultipleWriteLocations` en `true`. Además, establezca `connectionPolicy.PreferredLocations` en la región en que se va a implementar la aplicación y donde se va a replicar Cosmos DB:

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

Para habilitar la arquitectura multimaestro, establezca `connection_policy.UseMultipleWriteLocations` en `true`. Además, establezca `connection_policy.PreferredLocations` en la región en que se va a implementar la aplicación y donde se va a replicar Cosmos DB.

```python
connection_policy = documents.ConnectionPolicy()
connection_policy.UseMultipleWriteLocations = True
connection_policy.PreferredLocations = [region]

client = cosmos_client.CosmosClient(self.account_endpoint, {'masterKey': self.account_key}, connection_policy, documents.ConsistencyLevel.Session)
```

## <a name="next-steps"></a>Pasos siguientes

Lea los siguientes artículos:

* [Uso de tokens de sesión para administrar la coherencia en Azure Cosmos DB](how-to-manage-consistency.md#utilize-session-tokens)
* [Tipos de conflicto y directivas de resolución de conflictos en Azure Cosmos DB](conflict-resolution-policies.md)
* [Alta disponibilidad en Azure Cosmos DB](high-availability.md)
* [Niveles de coherencia en Azure Cosmos DB](consistency-levels.md)
* [Selección del nivel de coherencia adecuado en Azure Cosmos DB](consistency-levels-choosing.md)
* [Inconvenientes de la coherencia, disponibilidad y rendimiento en Azure Cosmos DB](consistency-levels-tradeoffs.md)
* [Availability and performance tradeoffs for various consistency levels](consistency-levels-tradeoffs.md) (Compromisos entre rendimiento y disponibilidad en los distintos niveles de coherencia)
* [Escalado del rendimiento aprovisionado globalmente](scaling-throughput.md)
* [Distribución global: En segundo plano](global-dist-under-the-hood.md)
