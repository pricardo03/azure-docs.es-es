---
title: 'Azure Cosmos DB: API, SDK y recursos de .NET Standard para SQL'
description: Obtenga toda la información sobre SQL API y el SDK de .NET, incluidas las fechas de lanzamiento, fechas de retirada y cambios realizados entre las versiones del SDK para .NET de Azure Cosmos DB.
author: deborahc
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 07/12/2019
ms.author: dech
ms.openlocfilehash: 01645e66907e01b2e8fcb771976b50e5bbc8abf4
ms.sourcegitcommit: 920ad23613a9504212aac2bfbd24a7c3de15d549
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/15/2019
ms.locfileid: "68228955"
---
# <a name="azure-cosmos-db-net-standard-sdk-for-sql-api-download-and-release-notes"></a>SDK de Azure Cosmos DB para .NET Standard para SQL API: descarga y notas de la versión
> [!div class="op_single_selector"]
> * [.NET Standard](sql-api-sdk-dotnet-standard.md)
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
> * [BulkExecutor: .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [BulkExecutor: Java](sql-api-sdk-bulk-executor-java.md)

| |  |
|---|---|
|**Descarga del SDK**|[NuGet](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/)|
|**Documentación de la API**|[Documentación de referencia de API de .NET](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet)|
|**Muestras**|[Ejemplos de código de .NET](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/CodeSamples)|
|**Primeros pasos**|[Introducción al SDK para .NET de Azure Cosmos DB](sql-api-get-started.md)|
|**Tutorial de la aplicación web**|[Desarrollo de aplicaciones web con Azure Cosmos DB](sql-api-dotnet-application.md)|
|**Plataforma admitida actualmente**|[Microsoft .NET Standard 2.0](/dotnet/standard/net-standard)|

## <a name="release-notes"></a>Notas de la versión

### <a name="a-name300300"></a><a name="3.0.0"/>3.0.0 
* Disponibilidad general de la [versión 3.0.0](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/) del SDK de .NET
* Tiene .NET Standard 2.0 como destino, que es compatible con .NET Framework 4.6.1+ y .NET Core 2.0+
* Nuevo modelo de objetos, con CosmosClient de nivel superior y métodos divididos entre las clases pertinentes Database y Container
* Nuevas API de flujo de alto rendimiento
* Compatibilidad integrada con las API del procesador de fuente de cambios
* API de compilador fluidas para CosmosClient, Container y el procesador de cambios
* API idiomática de administración de rendimiento
* RequestOptions y ResponseTypes pormenorizados para solicitudes de base de datos, contenedor, elemento, consulta y rendimiento
* Capacidad de escalar contenedores sin particiones 
* Serializador extensible y personalizable
* Canalización de solicitudes extensible compatible con controladores personalizados


## <a name="release--retirement-dates"></a>Fechas de lanzamiento y de retirada
Microsoft notifica la retirada de un SDK con al menos **12 meses** de antelación para facilitar la transición a una versión compatible o más reciente.

Solo se agregan nuevas características, funcionalidad y optimizaciones al SDK actual, por lo que se recomienda actualizar siempre a la última versión del SDK tan pronto como sea posible. 

El servicio rechaza cualquier solicitud realizada a Azure Cosmos DB mediante un SDK retirado.

<br/>

| Versión | Fecha de lanzamiento | Fecha de retirada |
| --- | --- | --- |
| [3.0.0](#3.0.0) |15 de julio de 2019 |--- |

## <a name="faq"></a>Preguntas más frecuentes
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Otras referencias
Para más información sobre Cosmos DB, consulte la página del servicio [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/). 

