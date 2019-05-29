---
title: Compatibilidad de Azure Table Storage con Azure Cosmos DB
description: Obtenga información sobre cómo funcionan conjuntamente Table API de Azure Cosmos DB y Azure Table Storage.
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: overview
ms.date: 11/15/2017
author: wmengmsft
ms.author: wmeng
ms.reviewer: sngun
ms.openlocfilehash: c7c9caad6c454b90a6d50932450922a03bbc1944
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/14/2019
ms.locfileid: "65604157"
---
# <a name="developing-with-azure-cosmos-db-table-api-and-azure-table-storage"></a>Implementación con Table API de Azure Cosmos DB y Azure Table Storage

Table API de Azure Cosmos DB y Azure Table Storage comparten el mismo modelo de datos de tablas y exponen las mismas operaciones de creación, eliminación, actualización y consulta a través de sus SDK. 

[!INCLUDE [storage-table-cosmos-comparison](../../includes/storage-table-cosmos-comparison.md)]

## <a name="developing-with-the-azure-cosmos-db-table-api"></a>Desarrollo con Table API de Azure Cosmos DB

En este momento, [Table API de Azure Cosmos DB](table-introduction.md) tiene cuatro SDK disponibles para el desarrollo: 

* [Microsoft.Azure.Cosmos.Table](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table): .NET SDK. Esta biblioteca va destinada a .NET Standard y tiene las mismas clases y firmas de método que el [SDK de Azure Storage de Windows](https://www.nuget.org/packages/WindowsAzure.Storage) público, pero también se puede conectar a cuentas de Azure Cosmos DB mediante Table API. Se recomienda que los usuarios de la biblioteca de .NET Framework [Microsoft.Azure.CosmosDB.Table](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.Table/) actualicen a [Microsoft.Azure.Cosmos.Table](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table), ya que está en modo de mantenimiento y pronto quedará en desuso.

* [SDK de Python](table-sdk-python.md): El nuevo SDK de Python de Azure Cosmos DB es el único SDK que admite Azure Table Storage en Python. Este SDK se conecta con Azure Table Storage y Table API de Azure Cosmos DB.

* [SDK de Java](table-sdk-java.md): Este SDK de Azure Storage tiene la capacidad de conectarse a las cuentas de Azure Cosmos DB mediante Table API.

* [SDK de Node.js](table-sdk-nodejs.md): Este SDK de Azure Storage tiene la capacidad de conectarse a las cuentas de Azure Cosmos DB mediante Table API.


Puede encontrar información adicional sobre el funcionamiento con Table API en el artículo [Preguntas más frecuentes: desarrollo con Table API en .NET](faq.md#table).

## <a name="developing-with-azure-table-storage"></a>Desarrollo con Azure Table Storage

Azure Table Storage tiene las siguientes SDK disponibles para el desarrollo:

- [SDK de WindowsAzure.Storage .NET](https://www.nuget.org/packages/WindowsAzure.Storage/). Esta biblioteca permite trabajar con Storage Table service.
- [SDK de Python](table-sdk-python.md). El SDK de Table de Azure Cosmos DB para Python también admite con Storage Table service.
- [SDK de Azure Storage para Java](https://github.com/azure/azure-storage-java). Este SDK de Azure Storage proporciona una biblioteca cliente en Java que consume Azure Table Storage.
- [SDK de Node.js](table-sdk-nodejs.md). Este SDK proporciona un paquete Node.js y una biblioteca cliente de JavaScript compatible con el explorador que consume Storage Table service.
- [Módulo de PowerShell AzureRmStorageTable](https://www.powershellgallery.com/packages/AzureRmStorageTable). Este módulo de PowerShell contiene cmdlets para trabajar con tablas de almacenamiento.
- [Biblioteca cliente de Azure Storage para C++](https://github.com/Azure/azure-storage-cpp/). Esta biblioteca le permite compilar aplicaciones en Azure Storage.
- [Biblioteca cliente de Table de Azure Storage Table para Ruby](https://github.com/azure/azure-storage-ruby/tree/master/table). Este proyecto proporciona un paquete Ruby que facilita el acceso a instancias de Azure Storage Table service.
- [Biblioteca cliente de PHP de Table de Azure Storage Table](https://github.com/Azure/azure-storage-php/tree/master/azure-storage-table). Este proyecto proporciona una biblioteca cliente de PHP que facilita el acceso a instancias de Azure Storage Table service.


   





