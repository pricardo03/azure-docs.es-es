---
title: 'Azure Cosmos DB: API, SDK y recursos de .NET para Bulk Executor'
description: Obtenga toda la información sobre la API y el SDK de .NET para Bulk Executor, incluidas la fechas de lanzamiento, las fechas de retirada y los cambios realizados entre las versiones del SDK de .NET para Bulk Executor de Azure Cosmos DB.
author: tknandu
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 11/19/2018
ms.author: ramkris
ms.openlocfilehash: 74eddadd7fd967daa1eebb9d7cb223fdc708025f
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66471427"
---
# <a name="net-bulk-executor-library-download-information"></a>Biblioteca BulkExecutor para .NET: Información de descarga 

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
| **Descripción**| La biblioteca Bulk Executor permite a las aplicaciones cliente realizar operaciones en bloque en las cuentas de Azure Cosmos DB. La biblioteca Bulk Executor proporciona los espacios de nombres BulkImport, BulkUpdate y BulkDelete. El módulo BulkImport puede ingerir documentos en masa de forma optimizada, de tal forma que la capacidad de proceso aprovisionada para una colección se consuma en el máximo nivel posible. El módulo BulkUpdate puede actualizar en masa los datos existentes en los contenedores de Azure Cosmos DB como revisiones. El módulo BulkDelete puede eliminar documentos en masa de forma optimizada, de tal forma que el rendimiento aprovisionado para una colección se consuma en el máximo nivel posible.|
|**Descarga del SDK**| [NuGet](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.BulkExecutor/) |
| **Biblioteca de BulkExecutor en GitHub**| [GitHub](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started)|
|**Documentación de la API**|[Documentación de referencia de API de .NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor?view=azure-dotnet)|
|**Primeros pasos**|[Introducción al SDK de .NET para la biblioteca Bulk Executor](bulk-executor-dot-net.md)|
| **Plataforma admitida actualmente**| Microsoft .NET Framework 4.5.2, 4.6.1 y .NET Standard 2.0 |

## <a name="release-notes"></a>Notas de la versión

### <a name="a-name230-preview2230-preview2"></a><a name="2.3.0-preview2"/>2.3.0-preview2

* Se agregó compatibilidad para el ejecutor en masa de graph Aceptar el ttl de vértices y bordes

### <a name="a-name220-preview2220-preview2"></a><a name="2.2.0-preview2"/>2.2.0-preview2

* Se ha corregido un problema, que han provocado excepciones durante el escalado elástico de Azure Cosmos DB cuando se ejecuta en modo de puerta de enlace. Esta corrección facilita funcionalmente equivalente a 1.4.1 de versión.

### <a name="a-name210-preview2210-preview2"></a><a name="2.1.0-preview2"/>2.1.0-preview2

* BulkDelete se ha agregado compatibilidad para que las cuentas de API de SQL Aceptar la clave de partición, las tuplas de Id. de documento para eliminar. Este cambio facilita funcionalmente equivalente a la versión 1.4.0 de versión.

### <a name="a-name200-preview2200-preview2"></a><a name="2.0.0-preview2"/>2.0.0-preview2

* Incluye MongoBulkExecutor para la compatibilidad con .NET Standard 2.0. Esta característica tiene una funcionalidad similar a la de la versión 1.3.0, pero se ha agregado la compatibilidad con .NET Standard 2.0 como plataforma de destino.

### <a name="a-name200-preview200-preview"></a><a name="2.0.0-preview"/>2.0.0-preview

* Se ha agregado .NET Standard 2.0 como una de las plataformas de destino admitidas para que la biblioteca BulkExecutor funcione con las aplicaciones .NET Core.

### <a name="a-name160160"></a><a name="1.6.0"/>1.6.0

* Actualizado el ejecutor en masa para usar la versión más reciente del SDK de .NET de Azure Cosmos DB (2.4.0).

### <a name="a-name150150"></a><a name="1.5.0"/>1.5.0

* Se agregó compatibilidad para el ejecutor en masa de graph Aceptar el ttl de vértices y bordes

### <a name="a-name141141"></a><a name="1.4.1"/>1.4.1

* Se ha corregido un problema, que han provocado excepciones durante el escalado elástico de Azure Cosmos DB cuando se ejecuta en modo de puerta de enlace.

### <a name="a-name140140"></a><a name="1.4.0"/>1.4.0

* BulkDelete se ha agregado compatibilidad para que las cuentas de API de SQL Aceptar la clave de partición, las tuplas de Id. de documento para eliminar.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0

* Se ha corregido un problema que generaba un error de formato en el agente de usuario utilizado por BulkExecutor.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0

* Se han mejorado las API de importación y actualización de BulkExecutor para adaptarlas de forma transparente al escalado elástico del contenedor de Cosmos DB cuando el espacio de almacenamiento sobrepase la capacidad actual sin generar excepciones.

### <a name="a-name112112"></a><a name="1.1.2"/>1.1.2

* Se ha incrementado la independencia de .NET SDK para DocumentDB con la versión 2.1.3.

### <a name="a-name111111"></a><a name="1.1.1"/>1.1.1

* Se ha corregido un problema que hacía que BulkExecutor generara un error de JSRT al realizar la importación en colecciones fijas.

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
