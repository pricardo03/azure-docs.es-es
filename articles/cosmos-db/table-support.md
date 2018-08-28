---
title: Compatibilidad de Azure Table Storage con Azure Cosmos DB | Microsoft Docs
description: Obtenga información sobre cómo funcionan conjuntamente Table API de Azure Cosmos DB y Azure Table Storage.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-table
ms.devlang: na
ms.topic: overview
ms.date: 11/15/2017
ms.author: sngun
ms.openlocfilehash: a6ac954caa8341d6d949811f0bb9d7e68c0b5aac
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2018
ms.locfileid: "42023258"
---
# <a name="developing-with-azure-cosmos-db-table-api-and-azure-table-storage"></a>Implementación con Table API de Azure Cosmos DB y Azure Table Storage

Table API de Azure Cosmos DB y Azure Table Storage comparten el mismo modelo de datos de tablas y exponen las mismas operaciones de creación, eliminación, actualización y consulta a través de sus SDK. 

[!INCLUDE [storage-table-cosmos-comparison](../../includes/storage-table-cosmos-comparison.md)]

## <a name="developing-with-the-azure-cosmos-db-table-api"></a>Desarrollo con Table API de Azure Cosmos DB

En este momento, [Table API de Azure Cosmos DB](table-introduction.md) tiene cuatro SDK disponibles para el desarrollo: 
- SDK de .NET de [Microsoft.Azure.CosmosDB.Table](https://aka.ms/tableapinuget). Esta biblioteca tiene las mismas firmas de método y clases que el [SDK de Windows Azure Storage](https://www.nuget.org/packages/WindowsAzure.Storage) público, pero también se puede conectar a cuentas de Azure Cosmos DB mediante Table API. Observe que la biblioteca `Microsoft.Azure.CosmosDB.Table` solo está disponible actualmente para .NET Standard, aún no está disponible para .NET Core.
- [SDK de Python](table-sdk-python.md). El nuevo SDK de Python de Azure Cosmos DB es el único SDK que admite Azure Table Storage en Python. Este SDK se conecta con Azure Table Storage y Table API de Azure Cosmos DB.
- [SDK de Java](table-sdk-java.md). Este SDK de Azure Storage tiene la capacidad de conectarse a las cuentas de Azure Cosmos DB mediante Table API.
- [SDK de Node.js](table-sdk-nodejs.md). Este SDK de Azure Storage tiene la capacidad de conectarse a las cuentas de Azure Cosmos DB mediante Table API.

Puede encontrar información adicional sobre el funcionamiento con Table API en el artículo [Preguntas más frecuentes: Desarrollar con Table API](faq.md#develop-with-the-table-api).

## <a name="developing-with-azure-table-storage"></a>Desarrollo con Azure Table Storage

Azure Table Storage tiene las siguientes SDK disponibles para el desarrollo:

- [SDK de WindowsAzure.Storage .NET](https://www.nuget.org/packages/WindowsAzure.Storage/). Esta biblioteca permite trabajar con Storage Table service.
- [SDK de Python](table-sdk-python.md). El SDK de Table de Azure Cosmos DB para Python también admite con Storage Table service.
- [SDK de Azure Storage para Java](https://github.com/azure/azure-storage-java). Este SDK de Azure Storage proporciona una biblioteca cliente en Java que consume Azure Table Storage.
- [SDK de Node.js](table-sdk-nodejs.md). Este SDK proporciona un paquete Node.js y una biblioteca cliente de JavaScript compatible con el explorador que consume Storage Table service.
- [Módulo de PowerShell AzureRmStorageTable](https://www.powershellgallery.com/packages/AzureRmStorageTable/1.0.0.7). Este módulo de PowerShell contiene cmdlets para trabajar con tablas de almacenamiento.
- [Biblioteca cliente de Azure Storage para C++](https://github.com/Azure/azure-storage-cpp/). Esta biblioteca le permite compilar aplicaciones en Azure Storage.
- [Biblioteca cliente de Table de Azure Storage Table para Ruby](https://github.com/azure/azure-storage-ruby/tree/master/table). Este proyecto proporciona un paquete Ruby que facilita el acceso a instancias de Azure Storage Table service.
- [Biblioteca cliente de PHP de Table de Azure Storage Table](https://github.com/Azure/azure-storage-php/tree/master/azure-storage-table). Este proyecto proporciona una biblioteca cliente de PHP que facilita el acceso a instancias de Azure Storage Table service.


   





