---
title: 'Tutorial: Uso compartido de Azure Spatial Anchors entre sesiones y dispositivos con un back-end de Azure Cosmos DB | Microsoft Docs'
description: En este tutorial, aprenderá a compartir identificadores de Azure Spatial Anchors entre los dispositivos Android o iOS de Unity con un servicio de back-end y Azure Cosmos DB.
author: ramonarguelles
manager: vicenterivera
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: 7ddbff563f79992f21aef5182177f4fb60c61dab
ms.sourcegitcommit: 7efb2a638153c22c93a5053c3c6db8b15d072949
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2019
ms.locfileid: "72882158"
---
# <a name="tutorial-sharing-azure-spatial-anchors-across-sessions-and-devices-with-an-azure-cosmos-db-back-end"></a>Tutorial: Uso compartido de Azure Spatial Anchors entre sesiones y dispositivos con un back-end de Azure Cosmos DB

Este tutorial es una continuación del tutorial para [compartir anclajes espaciales de Azure entre sesiones y dispositivos](../../../articles/spatial-anchors/tutorials/tutorial-share-anchors-across-devices.md). Le guiará en el proceso de agregar algunas funcionalidades más para que Azure Cosmos DB sirva como almacenamiento de back-end al mismo tiempo que comparten los delimitadores espaciales de Azure entre sesiones y dispositivos.

![GIF que ilustra la persistencia de objetos](./media/persistence.gif)

Es importante destacar que, aunque en este tutorial se van a usar Unity y Azure Cosmos DB, es solo para proporcionar un ejemplo de cómo compartir los identificadores de Spatial Anchors entre dispositivos. Se pueden usar otros lenguajes y tecnologías de back-end para lograr el mismo objetivo. Además, la aplicación web ASP.NET Core que se usa en este tutorial requiere el SDK de .NET Core 2.2. Funciona bien en Web Apps para Windows, pero actualmente no funciona en Web Apps para Linux.

## <a name="create-a-database-account"></a>Creación de una cuenta de base de datos

Agregue una base de datos de Azure Cosmos al grupo de recursos que creó anteriormente. 

[!INCLUDE [cosmos-db-create-dbaccount-table](../../../includes/cosmos-db-create-dbaccount-table.md)]

Copie `Connection String`, ya que lo va a necesitar.

## <a name="make-minor-changes-to-the-sharingservice-files"></a>Realización de cambios menores en los archivos SharingService

En el **Explorador de soluciones**, abra `SharingService\Startup.cs`.

Busque `#define INMEMORY_DEMO` al principio del archivo y marque la línea como comentario. Guarde el archivo.

En el **Explorador de soluciones**, abra `SharingService\appsettings.json`.

Busque la propiedad `StorageConnectionString` y establezca que el valor sea el mismo que el de `Connection String`, que copió en el [paso de creación de la cuenta de base de datos](#create-a-database-account). Guarde el archivo.

Puede publicar el servicio de uso compartido y ejecutar la aplicación de ejemplo.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha usado Azure Cosmos DB para compartir identificadores de delimitador en todos los dispositivos. Para más información sobre cómo usar Azure Spatial Anchors en una nueva aplicación de Unity HoloLens, continúe con el siguiente tutorial.

> [!div class="nextstepaction"]
> [Inicio de una nueva aplicación de Unity HoloLens](./tutorial-new-unity-hololens-app.md)
