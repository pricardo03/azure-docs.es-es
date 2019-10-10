---
title: 'Azure Cosmos DB: API, SDK y recursos de .NET para Bulk Executor'
description: Obtenga toda la información sobre la API y el SDK de .NET para BulkExecutor, incluidas la fechas de lanzamiento, fechas de retirada y cambios realizados entre las versiones del SDK de .NET para BulkExecutor de Azure Cosmos DB.
author: tknandu
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 11/19/2018
ms.author: ramkris
ms.openlocfilehash: 447f52a08c9e833808508852c4619f6ce1d947e1
ms.sourcegitcommit: 6fe40d080bd1561286093b488609590ba355c261
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703368"
---
# <a name="net-bulk-executor-library-download-information"></a>Biblioteca Bulk Executor para .NET: Información de descarga 

> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [Fuente de cambios de .NET](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Async Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [Proveedor de recursos de REST](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [Bulk Executor: .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [Bulk Executor: Java](sql-api-sdk-bulk-executor-java.md)

| |  |
|---|---|
| **Descripción**| La biblioteca Bulk Executor .NET permite a las aplicaciones cliente realizar operaciones en bloque en las cuentas de Azure Cosmos DB. Esta biblioteca proporciona los espacios de nombres BulkImport, BulkUpdate y BulkDelete. El módulo BulkImport puede ingerir documentos en masa de forma optimizada, de tal forma que la capacidad de proceso aprovisionada para una colección se consuma en el máximo nivel posible. El módulo BulkUpdate puede actualizar en masa los datos existentes en los contenedores de Azure Cosmos como revisiones. El módulo BulkDelete puede eliminar documentos en masa de forma optimizada, de tal forma que el rendimiento aprovisionado para una colección se consuma en el máximo nivel posible.|
|**Descarga del SDK**| [NuGet](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.BulkExecutor/) |
| **Biblioteca BulkExecutor en GitHub**| [GitHub](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started)|
|**Documentación de la API**|[Documentación de referencia de API de .NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor?view=azure-dotnet)|
|**Primeros pasos**|[Introducción al SDK de .NET para la biblioteca BulkExecutor](bulk-executor-dot-net.md)|
| **Plataforma admitida actualmente**| Microsoft .NET Framework 4.5.2, 4.6.1 y .NET Standard 2.0 |

## <a name="release-notes"></a>Notas de la versión

### <a name="a-name230-preview2230-preview2"></a><a name="2.3.0-preview2"/>2.3.0-preview2

* Se ha agregado compatibilidad para que el ejecutor en masa acepte TTL en vértices y bordes.

### <a name="a-name220-preview2220-preview2"></a><a name="2.2.0-preview2"/>2.2.0-preview2

* Se ha corregido un problema que provocaba excepciones durante el escalado elástico de Azure Cosmos DB cuando se ejecutaba en modo de puerta de enlace. Esta corrección la hace funcionalmente equivalente a la versión 1.4.1.

### <a name="a-name210-preview2210-preview2"></a><a name="2.1.0-preview2"/>2.1.0-preview2

* Se ha agregado compatibilidad con la operación BulkDelete para que las cuentas de la API de SQL acepten la clave de partición y las tuplas de identificación de documentos que se van a eliminar. Este cambio la hace funcionalmente equivalente a la versión 1.4.0.

### <a name="a-name200-preview2200-preview2"></a><a name="2.0.0-preview2"/>2.0.0-preview2

* Incluye MongoBulkExecutor para la compatibilidad con .NET Standard 2.0. Esta característica tiene una funcionalidad similar a la de la versión 1.3.0, pero se ha agregado la compatibilidad con .NET Standard 2.0 como plataforma de destino.

### <a name="a-name200-preview200-preview"></a><a name="2.0.0-preview"/>2.0.0-preview

* Se ha agregado .NET Standard 2.0 como una de las plataformas de destino admitidas para que la biblioteca Bulk Executor funcione con las aplicaciones .NET Core.

### <a name="a-name182182"></a><a name="1.8.2"/>1.8.2

* Se ha corregido el consumo elevado de CPU en ciertos escenarios.
* El seguimiento ahora utiliza TraceSource. Los usuarios pueden definir clientes de escucha para el origen `BulkExecutorTrace`.
* Se ha corregido un escenario poco frecuente que podía provocar un bloqueo al enviar documentos de aproximadamente 2 MB de tamaño.

### <a name="a-name160160"></a><a name="1.6.0"/>1.6.0

* Se ha actualizado Bulk Executor para usar la versión más reciente del SDK de .NET de Azure Cosmos DB (2.4.0).

### <a name="a-name150150"></a><a name="1.5.0"/>1.5.0

* Se ha agregado compatibilidad para que el ejecutor en masa acepte TTL en vértices y bordes.

### <a name="a-name141141"></a><a name="1.4.1"/>1.4.1

* Se ha corregido un problema que provocaba excepciones durante el escalado elástico de Azure Cosmos DB cuando se ejecutaba en modo de puerta de enlace.

### <a name="a-name140140"></a><a name="1.4.0"/>1.4.0

* Se ha agregado compatibilidad con la operación BulkDelete para que las cuentas de la API de SQL acepten la clave de partición y las tuplas de identificación de documentos que se van a eliminar.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0

* Se ha corregido un problema que generaba un error de formato en el agente de usuario utilizado por Bulk Executor.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0

* Se han mejorado las API de importación y actualización de Bulk Executor para adaptarlas de forma transparente al escalado elástico del contenedor de Cosmos cuando el espacio de almacenamiento sobrepase la capacidad actual sin generar excepciones.

### <a name="a-name112112"></a><a name="1.1.2"/>1.1.2

* Se ha incrementado la independencia de .NET SDK para DocumentDB con la versión 2.1.3.

### <a name="a-name111111"></a><a name="1.1.1"/>1.1.1

* Se ha corregido un problema que hacía que Bulk Executor generara un error de JSRT al realizar la importación en colecciones fijas.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0

* Se agregó compatibilidad con la operación BulkDelete para las cuentas de la API de SQL de Azure Cosmos DB.
* Se agregó compatibilidad con la operación BulkImport para las cuentas con la API de Azure Cosmos DB para MongoDB.
* Se aumentó la dependencia del SDK de .NET de DocumentDB a la versión 2.0.0. 

### <a name="a-name102102"></a><a name="1.0.2"/>1.0.2

* Se agregó compatibilidad con la operación BulkImport para las cuentas de la API de Gremlin de Azure Cosmos DB.

### <a name="a-name101101"></a><a name="1.0.1"/>1.0.1

* Se corrigió un error leve de la operación BulkImport para las cuentas de la API de SQL de Azure Cosmos DB.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0

* Se agregó compatibilidad con las operaciones BulkImport y BulkUpdate de las cuentas de la API de SQL de Azure Cosmos DB.

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de la biblioteca Bulk Executor de Java, consulte el artículo siguiente:

[SDK e información sobre la versión de la biblioteca Bulk Executor para Java](sql-api-sdk-bulk-executor-java.md)
