---
title: SDK y recursos de .NET Standard para Table API de Azure Cosmos DB
description: Obtenga toda la información sobre Table API de Azure Cosmos DB y el SDK de .NET Standard, incluidas la fechas de lanzamiento, las fechas de retirada y los cambios realizados entre las versiones.
author: wmengmsft
ms.author: wmeng
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.devlang: dotnet
ms.topic: reference
ms.date: 10/18/2018
ms.openlocfilehash: 7ccc9793bd1c4a345098892e66be15228efcc265
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2019
ms.locfileid: "54844575"
---
# <a name="azure-cosmos-db-table-net-standard-api-download-and-release-notes"></a>Table API de .NET Standard de Azure Cosmos DB: descarga y notas de la versión
> [!div class="op_single_selector"]

> * [.NET](table-sdk-dotnet.md)
> * [.NET Standard](table-sdk-dotnet-standard.md)
> * [Java](table-sdk-java.md)
> * [Node.js](table-sdk-nodejs.md)
> * [Python](table-sdk-python.md)

|   |   |
|---|---|
|**Descarga del SDK**|[NuGet](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table)|
|**Plataforma admitida actualmente**|[Microsoft .NET Standard 2.0](https://www.nuget.org/packages/NETStandard.Library)|

## <a name="release-notes"></a>Notas de la versión

### <a name="a-name0101-preview0101-preview"></a><a name="0.10.1-preview"/>0.10.1-preview
* Agregue compatibilidad con el token de SAS, las operaciones de TablePermissions, ServiceProperties y ServiceStats sobre los puntos de conexión de Azure Storage. 
   > [!NOTE] Algunas funcionalidades de los SDK anteriores de tabla de Azure Storage todavía no se admiten, como el cifrado del lado cliente.

### <a name="a-name0100-preview0100-preview"></a><a name="0.10.0-preview"/>0.10.0 (versión preliminar)
* Se ha agregado compatibilidad con operaciones básicas de CRUD, por lotes y consultas sobre los puntos de conexión de tabla de Azure Storage. 
   > [!NOTE] Algunas funcionalidades de los SDK anteriores de tabla de Azure Storage todavía no se admiten, como el cifrado del lado cliente.

### <a name="a-name091-preview091-preview"></a><a name="0.9.1-preview"/>0.9.1 (versión preliminar)
* El SDK de .NET Standard para Table API de Azure Cosmos DB es una biblioteca de .NET multiplataforma que ofrece acceso eficaz al modelo de datos de tabla en Cosmos DB. Esta versión inicial es compatible con el conjunto completo de CRUD de tabla y entidad + funcionalidades de consulta con API similares como el [SDK de Table API de Cosmos DB para .NET Framework](table-sdk-dotnet.md). 
   > [!NOTE] Los puntos de conexión de tabla de Azure Storage aún no son compatibles con la versión preliminar 0.9.1.

## <a name="release-and-retirement-dates"></a>Fechas de lanzamiento y de retirada
Microsoft notifica la retirada de un SDK con al menos **12 meses** de antelación para facilitar la transición a una versión compatible o más reciente.

| Versión | Fecha de lanzamiento | Fecha de retirada |
| --- | --- | --- |
| [0.10.1-preview](#0.10.1-preview) |22 de enero de 2019 |--- |
| [0.10.0 (versión preliminar)](#0.10.0-preview) |18 de diciembre de 2018 |--- |
| [0.9.1 (versión preliminar)](#0.9.1-preview) |18 de octubre de 2018 |--- |


## <a name="faq"></a>Preguntas más frecuentes

[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Otras referencias
Para obtener más información sobre Table API de Azure Cosmos DB, consulte [Introducción a Table API de Azure Cosmos DB](table-introduction.md). 
