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
ms.openlocfilehash: 0368e99135dd6e377dd2820b3e673c55182319b9
ms.sourcegitcommit: e3b0fb00b27e6d2696acf0b73c6ba05b74efcd85
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2019
ms.locfileid: "68663803"
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
> * [Bulk Executor: .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [Bulk Executor: Java](sql-api-sdk-bulk-executor-java.md)

| |  |
|---|---|
|**Descarga del SDK**|[NuGet](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/)|
|**Documentación de la API**|[Documentación de referencia de API de .NET](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet)|
|**Muestras**|[Ejemplos de código de .NET](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/CodeSamples)|
|**Primeros pasos**|[Introducción al SDK para .NET de Azure Cosmos DB](sql-api-get-started.md)|
|**Tutorial de la aplicación web**|[Desarrollo de aplicaciones web con Azure Cosmos DB](sql-api-dotnet-application.md)|
|**Plataforma admitida actualmente**|[Microsoft .NET Standard 2.0](/dotnet/standard/net-standard)|

## <a name="release-notes"></a>Notas de la versión
### <a name="a-name310310"></a><a name="3.1.0"/>3.1.0
#### <a name="added"></a>Se agregó
- [#541](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/541) Se agregó el nivel de coherencia a las opciones de cliente y consulta.
- [#544](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/544) Se agregó compatibilidad de token de continuación para LINQ.
- [#557](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/557) Se agregaron opciones de desencadenador a las opciones de solicitud de elemento.
- [#571](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/571) Se agregó un serializador Json.Net predeterminado con una configuración opcional.
- [#572](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/572) Se agregó la validación de la clave de partición en CreateContainerIfNotExistsAsync.
- [#581](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/581) Se agregó LINQ a QueryDefinition API.
- [#592](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/592) Se agregó CreateIfNotExistsAsync al generador de contenedores.
- [#597](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/597) Se agregó la propiedad de token de continuación a ResponseMessage.
- [#604](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/604) Se agregó el método de extensión ToStreamIterator de LINQ.

#### <a name="fixed"></a>Corregido
- [#548](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/548) Se corrigió el mensaje mal escrito en CosmosException.ToString();.
- [#558](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/558) Se corrigió la contención de bloqueo LocationCache ConcurrentDict.
- [#561](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/561) GetItemLinqQueryable ahora funciona con una consulta null.
- [#567](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/567) La consulta controla correctamente las distintas referencias culturales de idioma.
- [#574](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/574) Se corrigió el mensaje de error vacío si se produce un error en el análisis de excepción inesperada.
- [#576](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/576) La consulta serializa correctamente la entrada en una secuencia.

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
| [3.1.0](#3.1.0) |29 de julio de 2019 |--- |
| [3.0.0](#3.0.0) |15 de julio de 2019 |--- |


## <a name="faq"></a>Preguntas más frecuentes
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Otras referencias
Para más información sobre Cosmos DB, consulte la página del servicio [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/). 

