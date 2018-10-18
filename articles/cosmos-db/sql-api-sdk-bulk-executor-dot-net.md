---
title: 'Azure Cosmos DB: API, SDK y recursos de .NET para Bulk Executor | Microsoft Docs'
description: Obtenga toda la información sobre la API y el SDK de .NET para Bulk Executor, incluidas la fechas de lanzamiento, las fechas de retirada y los cambios realizados entre las versiones del SDK de .NET para Bulk Executor de Azure Cosmos DB.
services: cosmos-db
author: tknandu
manager: kfile
editor: cgronlun
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 09/14/2018
ms.author: ramkris
ms.openlocfilehash: ffd8f438429cd8769ac0dbff7f489327166e0000
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2018
ms.locfileid: "46294465"
---
# <a name="net-bulk-executor-library-download-information"></a>Biblioteca Bulk Executor en .Net: información de descarga 

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
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> * [Bulk Executor: .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [Bulk Executor: Java](sql-api-sdk-bulk-executor-java.md)

<table>

<tr><td>**Descripción**</td><td>La biblioteca Bulk Executor permite a las aplicaciones cliente realizar operaciones en masa en las cuentas de Azure Cosmos DB. La biblioteca Bulk Executor proporciona los espacios de nombres BulkImport, BulkUpdate y BulkDelete. El módulo BulkImport puede ingerir documentos en masa de forma optimizada, de tal forma que la capacidad de proceso aprovisionada para una colección se consuma en el máximo nivel posible. El módulo BulkUpdate puede actualizar en masa los datos existentes en los contenedores de Azure Cosmos DB como revisiones. El módulo BulkDelete puede eliminar documentos en masa de forma optimizada, de tal forma que el rendimiento aprovisionado para una colección se consuma en el máximo nivel posible.</td></tr>

<tr><td>**Descarga del SDK**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.BulkExecutor/)</td></tr>

<tr><td>**Biblioteca de BulkExecutor en GitHub**</td><td>[GitHub](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started)</td></tr>

<tr><td>**Documentación de la API**</td><td>[Documentación de referencia de API de .NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor?view=azure-dotnet)</td></tr>

<tr><td>**Primeros pasos**</td><td>[Introducción al SDK de .NET para la biblioteca Bulk Executor](bulk-executor-dot-net.md)</td></tr>

<tr><td>**Plataforma admitida actualmente**</td><td><ul><li>[Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/)(versión >= 2.0.0)</li><li>
[Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/)(versión >= 9.0.1)
</li></ul></td></tr>
</table></br>

## <a name="release-notes"></a>Notas de la versión

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0

* Se agregó compatibilidad con la operación BulkDelete para las cuentas de la API de SQL de Azure Cosmos DB.
* Se agregó compatibilidad con la operación BulkImport para las cuentas de la API de MongoDB de Azure Cosmos DB.
* Se aumentó la dependencia del SDK de .NET de DocumentDB a la versión 2.0.0. 

### <a name="a-name102102"></a><a name="1.0.2"/>1.0.2

* Se agregó compatibilidad con la operación BulkImport para las cuentas de la API de Gremlin de Azure Cosmos DB.

### <a name="a-name101101"></a><a name="1.0.1"/>1.0.1

* Se corrigió un error leve de la operación BulkImport para las cuentas de la API de SQL de Azure Cosmos DB.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0

* Se agregó compatibilidad con las operaciones BulkImport y BulkUpdate de las cuentas de la API de SQL de Azure Cosmos DB.
