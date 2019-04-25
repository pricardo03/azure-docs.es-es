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
ms.openlocfilehash: d955654eee1d02994f303b6270b156eb9a61c29b
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "58915262"
---
# <a name="tutorial-share-azure-spatial-anchors-across-sessions-and-devices-with-an-azure-cosmos-db-back-end"></a>Tutorial: Uso compartido de Azure Spatial Anchors entre sesiones y dispositivos con un back-end de Azure Cosmos DB

En este tutorial, obtendrá información sobre cómo usar [Azure Spatial Anchors](../overview.md) para crear delimitadores durante una sesión y, a continuación, buscarlos, durante otra sesión en el mismo dispositivo o en uno diferente. Por ejemplo, la segunda sesión podría ser otro día. Estos mismos delimitadores podrían también ser encontrados por varios dispositivos en el mismo lugar y al mismo tiempo.

![GIF que ilustra la persistencia de objetos](./media/persistence.gif)

[Azure Spatial Anchors](../overview.md) es un servicio multiplataforma para desarrolladores que se puede usar para crear experiencias de realidad mixta con objetos cuya ubicación persiste en todos los dispositivos a lo largo del tiempo. Cuando haya terminado, tendrá una aplicación que se puede implementar en dos o más dispositivos. Los delimitadores espaciales creados por una instancia de Spatial Anchors compartirán sus identificadores con las demás mediante Azure Cosmos DB.

Aprenderá a:

> [!div class="checklist"]
> * Implementar una aplicación web de ASP.NET Core en Azure que se pueda usar para compartir los delimitadores y almacenarlos en Azure Cosmos DB.
> * Configurar la escena AzureSpatialAnchorsLocalSharedDemo del ejemplo de Unity de los tutoriales de inicio rápido de Azure para aprovechar las ventajas del uso compartido de la aplicación web Sharing Anchors.
> * Implementar una aplicación en uno o varios dispositivos y ejecutarla.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [Share Anchors Sample Prerequisites](../../../includes/spatial-anchors-share-sample-prereqs.md)]

Es importante destacar que, aunque en este tutorial se van a usar Unity y Azure Cosmos DB, es solo para proporcionar un ejemplo de cómo compartir los identificadores de Spatial Anchors entre dispositivos. Se pueden usar otros lenguajes y tecnologías de back-end para lograr el mismo objetivo. Además, la aplicación web ASP.NET Core que se usa en este tutorial requiere el SDK de .NET Core 2.2. Funciona bien en Web Apps para Windows, pero actualmente no funciona en Web Apps para Linux.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="create-a-database-account"></a>Creación de una cuenta de base de datos

[!INCLUDE [cosmos-db-create-dbaccount-table](../../../includes/cosmos-db-create-dbaccount-table.md)]

Copie `Connection String`, ya que lo va a necesitar.

## <a name="open-the-sample-project-in-unity"></a>Apertura del proyecto de ejemplo en Unity

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

## <a name="deploy-the-sharing-anchors-service"></a>Implementación del servicio de delimitadores de uso compartido

Abra Visual Studio y, después, abra el proyecto de la carpeta `Sharing\SharingServiceSample`.

### <a name="configure-the-service-to-use-your-azure-cosmos-db-database"></a>Configuración del servicio para que use una base de datos de Azure Cosmos DB

En el **Explorador de soluciones**, abra `SharingService\Startup.cs`.

Busque `#define INMEMORY_DEMO` al principio del archivo y marque la línea como comentario. Guarde el archivo.

En el **Explorador de soluciones**, abra `SharingService\appsettings.json`.

Busque la propiedad `StorageConnectionString` y establezca que el valor sea el mismo que el de `Connection String`, que copió en el [paso de creación de la cuenta de base de datos](#create-a-database-account). Guarde el archivo.

[!INCLUDE [Publish Azure](../../../includes/spatial-anchors-publish-azure.md)]

[!INCLUDE [Run Share Anchors Sample](../../../includes/spatial-anchors-run-share-sample.md)]

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha usado Azure Cosmos DB para compartir identificadores de delimitador en todos los dispositivos. Para más información acerca de cómo usar Azure Spatial Anchors en una nueva aplicación Android, continúe con el siguiente tutorial.

> [!div class="nextstepaction"]
> [Inicio de una nueva aplicación Android](./tutorial-new-android-app.md)
