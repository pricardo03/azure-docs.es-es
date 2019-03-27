---
title: 'Tutorial: Uso compartido entre sesiones y dispositivos con Azure Spatial Anchors y un back-end de Azure Cosmos DB | Microsoft Docs'
description: En este tutorial, aprenderá a compartir identificadores de Azure Spatial Anchors entre los dispositivos Android o iOS de Unity con un servicio de back-end y Azure Cosmos DB.
author: ramonarguelles
manager: vicenterivera
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: 0e7011b9778221869940b137a2b87239f2d8db9b
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58286404"
---
# <a name="tutorial-sharing-across-sessions-and-devices-with-azure-spatial-anchors-and-an-azure-cosmos-db-back-end"></a>Tutorial: Uso compartido entre sesiones y dispositivos con Azure Spatial Anchors y un back-end de Azure Cosmos DB

En este tutorial aprenderá a usar [Azure Spatial Anchors](../overview.md) para:

- Crear delimitadores en una sesión y encontrarlos en otra sesión, en el mismo dispositivo o en otro. Por ejemplo, la segunda sesión podría ser otro día.
- Crear delimitadores que puedan encontrar varios dispositivos en el mismo lugar y al mismo tiempo.

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

En este tutorial, ha usado Azure Cosmos DB para compartir identificadores de delimitador en todos los dispositivos. Para más información acerca de la biblioteca de Azure Spatial Anchors, consulte nuestra guía sobre cómo crear y localizar los delimitadores.

> [!div class="nextstepaction"]
> [Creación y localización de delimitadores mediante Azure Spatial Anchors](../create-locate-anchors-overview.md)
