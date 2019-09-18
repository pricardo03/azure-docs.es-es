---
title: Tokens de recursos de Azure Cosmos DB con Gremlin
description: Aprenda a crear tokens de recursos y a usarlos para acceder a la base de datos de grafos
author: olignat
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: overview
ms.date: 09/06/2019
ms.author: olignat
ms.openlocfilehash: fcb18fb14cf787713735da07ca2048d0853fa46c
ms.sourcegitcommit: b8578b14c8629c4e4dea4c2e90164e42393e8064
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/09/2019
ms.locfileid: "70807016"
---
# <a name="azure-cosmos-db-resource-tokens-with-gremlin"></a>Tokens de recursos de Azure Cosmos DB con Gremlin
En este artículo se explica cómo usar los [tokens de recursos de Cosmos DB](secure-access-to-data.md) para acceder a la base de datos de grafos mediante el SDK de Gremlin.

## <a name="create-a-resource-token"></a>Creación de un token de recursos

El SDK de TinkerPop Gremlin no tiene una API para crear tokens de recursos. Un token de recurso es un concepto de Cosmos DB. Para crear tokens de recursos, descargue el [SDK de Azure Cosmos DB](sql-api-sdk-dotnet.md). Si la aplicación necesita crear tokens de recursos y usarlos para tener acceso a la base de datos de grafos, necesita dos SDK independientes.

Jerarquía del modelo de objetos por encima de los tokens de recursos:
- **Cuenta de Cosmos DB**:entidad de nivel superior con DNS asociado, por ejemplo, `contoso.gremlin.cosmos.azure.com`
  - **Base de datos de Cosmos DB**
    - **User**
      - **Permiso**
        - *Token*: propiedad del objeto **Permission** que denota las acciones permitidas o denegadas.

El token de recurso tiene formato `"type=resource&ver=1&sig=<base64 string>;<base64 string>;"`. Esta cadena es opaca para los clientes y debe usarse tal cual, sin modificación ni interpretación.

```csharp
// Notice that document client is created against .NET SDK end-point rather than Gremlin.
DocumentClient client = new DocumentClient(
  new Uri("https://contoso.documents.azure.com:443/"), 
  "<master key>", 
  new ConnectionPolicy 
  {
    EnableEndpointDiscovery = false, 
    ConnectionMode = ConnectionMode.Direct 
  });

  // Read specific permission to obtain a token.
  // Token will not be returned during ReadPermissionReedAsync() call.
  // This call will succeed only if database id, user id and permission id already exist. 
  // Note that <database id> is not a database name, it is a base64 string that represents database identifier, for example "KalVAA==".
  // Similar comment applies to <user id> and <permission id>
  Permission permission = await client.ReadPermissionAsync(UriFactory.CreatePermissionUri("<database id>", "<user id>", "<permission id>"));

  Console.WriteLine("Obtained token {0}", permission.Token);
}
```

## <a name="use-a-resource-token"></a>Uso de un token de recursos
Los tokens de recursos se pueden usar directamente como propiedad "password" al construir la clase `GremlinServer`.

```csharp
// Gremlin application needs to be given a resource token. It can't discover the token on its own.
// Token can be obtained for a given permission using Cosmos DB SDK or passed into the application as command line argument or configuration value.
string resourceToken = GetResourceToken();

// Configure gremlin servier to use resource token rather than master key
GremlinServer server = new GremlinServer(
  "contoso.gremlin.cosmosdb.azure.com",
  port: 443,
  enableSsl: true,
  username: "/dbs/<database name>/colls/<collection name>",

  // Format of the token is "type=resource&ver=1&sig=<base64 string>;<base64 string>;"
  password: resourceToken);

  using (GremlinClient gremlinClient = new GremlinClient(server, new GraphSON2Reader(), new GraphSON2Writer(), GremlinClient.GraphSON2MimeType))
  {
      await gremlinClient.SubmitAsync("g.V().limit(1)");
  }
```

La misma estrategia funciona en todos los SDK de TinkerPop Gremlin.

```java
Cluster.Builder builder = Cluster.build();

AuthProperties authenticationProperties = new AuthProperties();
authenticationProperties.with(AuthProperties.Property.USERNAME,
    String.format("/dbs/%s/colls/%s", "<database name>", "<collection name>"));

// Format of the token is "type=resource&ver=1&sig=<base64 string>;<base64 string>;"
authenticationProperties.with(AuthProperties.Property.PASSWORD, resourceToken);

builder.authProperties(authenticationProperties);
```

## <a name="limit"></a>Límite

Una sola cuenta de Gremlin puede emitir un número ilimitado de tokens, pero solo se pueden usar simultáneamente hasta **100** tokens en **1 hora**. Si la aplicación supera el límite de tokens por hora, se denegará la solicitud de autenticación con el mensaje de error `"Exceeded allowed resource token limit of 100 that can be used concurrently"`. El cierre de las conexiones activas con tokens específicos para liberar ranuras para nuevos tokens no surtirá efecto. El motor de base de datos para Gremlin de Cosmos DB realiza el seguimiento de los distintos tokens en la hora anterior a la solicitud de autenticación.

## <a name="permission"></a>Permiso

Las aplicaciones de error comunes se encuentran cuando al usar tokens de recursos aparece el mensaje `"Insufficient permissions provided in the authorization header for the corresponding request. Please retry with another authorization header."`. Este error se devuelve cuando el recorrido a través de Gremlin intenta escribir un borde o un vértice, pero el token de recurso solo concede permisos `Read`. Inspeccione el recorrido por si contiene alguno de los siguientes pasos: `.addV()`, `.addE()`, `.drop()`o `.property()`.

## <a name="next-steps"></a>Pasos siguientes
* [Control de acceso basado en rol](role-based-access-control.md) en Azure Cosmos DB
* [Información sobre la protección del acceso a los datos](secure-access-to-data.md) en Azure Cosmos DB
