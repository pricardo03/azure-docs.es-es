---
title: Aprenda a administrar la coherencia en Azure Cosmos DB
description: Aprenda a administrar la coherencia en Azure Cosmos DB
services: cosmos-db
author: christopheranderson
ms.service: cosmos-db
ms.topic: sample
ms.date: 10/17/2018
ms.author: chrande
ms.openlocfilehash: be2c68922221af848c9e484d03527d02808c071a
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2018
ms.locfileid: "51283825"
---
# <a name="manage-consistency-levels-in-azure-cosmos-db"></a>Administración de los niveles de coherencia en Azure Cosmos DB

En este artículo se explican las diferentes maneras de establecer la coherencia predeterminada, se anula dicha coherencia en el cliente, se administran manualmente los tokens de sesión y se conoce la métrica de obsolescencia limitada por probabilidades (PBS).

## <a name="configure-the-default-consistency-level"></a>Configuración del nivel de coherencia predeterminado

El nivel de coherencia predeterminado es el que los clientes usarán de forma predeterminada. Los clientes pueden invalidarlo.

### <a name="cli"></a>CLI

```bash
# create with a default consistency
az cosmosdb create --name <name of Cosmos DB Account> --resource-group <resource group name> --default-consistency-level Strong

# update an existing account's default consistency
az cosmosdb update --name <name of Cosmos DB Account> --resource-group <resource group name> --default-consistency-level BoundedStaleness
```

### <a name="powershell"></a>PowerShell

El ejemplo siguiente crea una cuenta de Cosmos DB con una arquitectura multimaestro habilitada en las regiones Este de EE. UU. y Oeste de EE. UU., lo que establece la directiva de coherencia predeterminada como obsolescencia limitada con un intervalo de obsolescencia máximo de 10 segundos y el número máximo de solicitudes obsoletas toleradas en 200.

```azurepowershell-interactive
$locations = @(@{"locationName"="East US"; "failoverPriority"=0},
             @{"locationName"="West US"; "failoverPriority"=1})

$iprangefilter = ""

$consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness";
                       "maxIntervalInSeconds"= "10";
                       "maxStalenessPrefix"="200"}

$CosmosDBProperties = @{"databaseAccountOfferType"="Standard";
                        "locations"=$locations;
                        "consistencyPolicy"=$consistencyPolicy;
                        "ipRangeFilter"=$iprangefilter;
                        "enableMultipleWriteLocations"="true"}

New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
  -ApiVersion "2015-04-08" `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -Name "myCosmosDbAccount" `
  -Properties $CosmosDBProperties
```

### <a name="portal"></a>Portal

Para ver o modificar el nivel de coherencia predeterminado, inicie sesión en Azure Portal. Busque la cuenta de Cosmos DB y abra el panel **Coherencia predeterminada**. Allí, elija el nivel de coherencia que desee como nuevo valor predeterminado y haga clic en Guardar.

![Imagen de menú de coherencia en Azure Portal](./media/how-to-manage-consistency/consistency-settings.png)

## <a name="override-the-default-consistency-level"></a>Invalidación del nivel de coherencia predeterminado

Los clientes pueden invalidar el nivel de coherencia predeterminado establecido por el servicio. Esto puede hacerse para todo el cliente completo o por solicitud.

### <a id="override-default-consistency-dotnet"></a>.NET SDK

```csharp
// Override consistency at the client level
ConsistencyPolicy consistencyPolicy = new ConsistencyPolicy
    {
        DefaultConsistencyLevel = ConsistencyLevel.BoundedStaleness,
        MaxStalenessIntervalInSeconds = 5,
        MaxStalenessPrefix = 100
    };
documentClient = new DocumentClient(new Uri(endpoint), authKey, connectionPolicy, consistencyPolicy);

// Override consistency at the request level via request options
RequestOptions requestOptions = new RequestOptions { ConsistencyLevel = ConsistencyLevel.Strong };

var response = await client.CreateDocumentAsync(collectionUri, document, requestOptions);
```

### <a id="override-default-consistency-java-async"></a>SDK asincrónico para Java

```java
// Override consistency at the client level
ConnectionPolicy policy = new ConnectionPolicy();

AsyncDocumentClient client =
        new AsyncDocumentClient.Builder()
                .withMasterKey(this.accountKey)
                .withServiceEndpoint(this.accountEndpoint)
                .withConsistencyLevel(ConsistencyLevel.Eventual)
                .withConnectionPolicy(policy).build();
```

### <a id="override-default-consistency-java-sync"></a>SDK sincrónico para Java

```java
// Override consistency at the client level
ConnectionPolicy connectionPolicy = new ConnectionPolicy();
DocumentClient client = new DocumentClient(accountEndpoint, accountKey, connectionPolicy, ConsistencyLevel.Strong);
```

### <a id="override-default-consistency-javascript"></a>SDK para Node.js/JavaScript/TypeScript

```javascript
// Override consistency at the client level
const client = new CosmosClient({
  /* other config... */
  consistencyLevel: ConsistencyLevel.Strong
});

// Override consistency at the request level via request options
const { body } = await item.read({ consistencyLevel: ConsistencyLevel.Eventual });
```

### <a id="override-default-consistency-python"></a>SDK para Python

```python
# Override consistency at the client level
connection_policy = documents.ConnectionPolicy()
client = cosmos_client.CosmosClient(self.account_endpoint, {'masterKey': self.account_key}, connection_policy, documents.ConsistencyLevel.Strong)
```

## <a name="utilize-session-tokens"></a>Uso de tokens de sesión

Si desea administrar manualmente tokens de sesión, puede obtenerlos desde las respuestas y establecerlos por solicitud. Si no tiene la necesidad de administrar manualmente los tokens de sesión, no es necesario que utilice los ejemplos siguientes. El SDK realizará un seguimiento de forma automática de los tokens de sesión y usará el token de sesión más reciente, si el usuario no establece el token de sesión.

### <a id="utilize-session-tokens-dotnet"></a>.NET SDK

```csharp
var response = await client.ReadDocumentAsync(
                UriFactory.CreateDocumentUri(databaseName, collectionName, "SalesOrder1"));
string sessionToken = response.SessionToken;

RequestOptions options = new RequestOptions();
options.SessionToken = sessionToken;
var response = await client.ReadDocumentAsync(
                UriFactory.CreateDocumentUri(databaseName, collectionName, "SalesOrder1"), options);
```

### <a id="utilize-session-tokens-java-async"></a>SDK asincrónico para Java

```java
// Get session token from response
RequestOptions options = new RequestOptions();
options.setPartitionKey(new PartitionKey(document.get("mypk")));
Observable<ResourceResponse<Document>> readObservable = client.readDocument(document.getSelfLink(), options);
readObservable.single()           // we know there will be one response
  .subscribe(
      documentResourceResponse -> {
          System.out.println(documentResourceResponse.getSessionToken());
      },
      error -> {
          System.err.println("an error happened: " + error.getMessage());
      });

// Resume the session by setting the session token on RequestOptions
RequestOptions options = new RequestOptions();
requestOptions.setSessionToken(sessionToken);
Observable<ResourceResponse<Document>> readObservable = client.readDocument(document.getSelfLink(), options);
```

### <a id="utilize-session-tokens-java-sync"></a>SDK sincrónico para Java

```java
// Get session token from response
ResourceResponse<Document> response = client.readDocument(documentLink, null);
String sessionToken = response.getSessionToken();

// Resume the session by setting the session token on the RequestOptions
RequestOptions options = new RequestOptions();
options.setSessionToken(sessionToken);
ResourceResponse<Document> response = client.readDocument(documentLink, options);
```

### <a id="utilize-session-tokens-javascript"></a>SDK para Node.js/JavaScript/TypeScript

```javascript
// Get session token from response
const { headers, item } = await container.items.create({ id: "meaningful-id" });
const sessionToken = headers["x-ms-session-token"];

// Immediately or later, you can use that sessionToken from the header to resume that session.
const { body } = await item.read({ sessionToken });
```

### <a id="utilize-session-tokens-python"></a>SDK para Python

```python
// Get the session token from the last response headers
item = client.ReadItem(item_link)
session_token = client.last_response_headers["x-ms-session-token"]

// Resume the session by setting the session token on the options for the request
options = {
    "sessionToken": session_token
}
item = client.ReadItem(doc_link, options)
```

## <a name="monitor-probabilistically-bounded-staleness-pbs-metric"></a>Supervisión de la métrica de obsolescencia limitada de manera probabilística (PBS)

Para ver la métrica de PBS, vaya a la cuenta de Cosmos DB en Azure Portal y abra el panel **Métricas**. Allí, haga clic en la pestaña **Coherencia** y examine el gráfico denominado "**Probability of strongly consistent reads based on your workload (see PBS)**".

![Imagen del gráfico de PBS en Azure Portal](./media/how-to-manage-consistency/pbs-metric.png)

Para ver esta métrica, use el menú de métricas de Cosmos DB. No se mostrará en la experiencia de métricas de Azure Monitor.

## <a name="next-steps"></a>Pasos siguientes

Puede aprender a administrar conflictos de datos o a pasar al siguiente concepto clave de Cosmos DB mediante los siguientes documentos:

* [How to manage conflicts between regions](how-to-manage-conflicts.md) (Administración de conflictos entre regiones)
* [Creación de particiones y distribución de datos](partition-data.md)
