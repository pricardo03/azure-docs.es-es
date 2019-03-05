---
title: 'Tutorial: Uso compartido entre sesiones y dispositivos con Azure Spatial Anchors y un back-end de Azure Cosmos DB | Microsoft Docs'
description: En este tutorial, obtendrá información sobre cómo compartir identificadores espaciales de Azure Spatial Anchors entre los dispositivos de Unity con un servicio de back-end y Azure Cosmos DB.
author: ramonarguelles
manager: vicenterivera
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: f0cd42fc37727099ed95a1c6fc2d427b7862412e
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/24/2019
ms.locfileid: "56753490"
---
# <a name="tutorial-sharing-across-sessions-and-devices-with-azure-spatial-anchors-and-an-azure-cosmos-db-back-end"></a>Tutorial: Uso compartido entre sesiones y dispositivos con Azure Spatial Anchors y un back-end de Azure Cosmos DB | Microsoft Docs

En este tutorial se muestra cómo usar [Azure Spatial Anchors](../overview.md) para:

1. Crear delimitadores en una sesión y encontrarlos en otra sesión en el mismo dispositivo u otro. Por ejemplo, en otro momento.
2. Crear delimitadores que puedan encontrar varios dispositivos en el mismo lugar al mismo tiempo.

![Persistencia](./media/persistence.gif)

[Azure Spatial Anchors](../overview.md) es un servicio multiplataforma para desarrolladores que le permite crear experiencias de realidad mixta mediante objetos cuya ubicación persiste en todos los dispositivos a lo largo del tiempo. Cuando haya terminado, tendrá una aplicación que se puede implementar en dos o más dispositivos. Los delimitadores espaciales creados por una instancia de Azure Spatial Anchors compartirán sus identificadores con las demás mediante Cosmos DB.

Aprenderá a:

> [!div class="checklist"]
> * Implementar una aplicación web de ASP.NET Core en Azure que se pueda usar para compartir los delimitadores y almacenarlos en Cosmos DB.
> * Configurar la escena AzureSpatialAnchorsLocalSharedDemo dentro del ejemplo de Unity de nuestros inicios rápidos para aprovechar las ventajas del uso compartido de la aplicación web de delimitadores.
> * Realizar la implementación y ejecución en uno o varios dispositivos.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [Share Anchors Sample Prerequisites](../../../includes/spatial-anchors-share-sample-prereqs.md)]

Es importante destacar que, aunque se va a usar Unity y Azure Cosmos DB en este tutorial, es solo para mostrar un ejemplo sobre cómo compartir los identificadores espaciales de Azure Spatial Anchors con otros dispositivos. Se pueden usar otros lenguajes y tecnologías de back-end para lograr el mismo objetivo. Además, la aplicación web de ASP.NET Core usada en este tutorial tiene una dependencia del SDK de .NET Core 2.2. Funciona bien en aplicaciones web de Azure normales (para Windows), pero no funciona en aplicaciones web de Azure para Linux.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="create-a-database-account"></a>Creación de una cuenta de base de datos

[!INCLUDE [cosmos-db-create-dbaccount-table](../../../includes/cosmos-db-create-dbaccount-table.md)]

Anote el valor de `Connection String` porque lo usaremos más adelante.

## <a name="deploy-your-sharing-anchors-service"></a>Implementación del uso compartido del servicio de delimitadores

Abra Visual Studio y abra el proyecto en la carpeta `Sharing\SharingServiceSample`.

### <a name="configure-the-service-so-that-it-uses-your-cosmos-db"></a>Configuración del servicio para que use Cosmos DB

En el **Explorador de soluciones**, abra `SharingService\Startup.cs`.

Busque la línea `#define INMEMORY_DEMO` en la parte superior del archivo y márquela como comentario. Guarde el archivo.

En el **Explorador de soluciones**, abra `SharingService\appsettings.json`.

Busque la propiedad `StorageConnectionString` y establezca como valor el valor de `Connection String` que anotó en el [paso de creación de la cuenta de base de datos](#create-a-database-account). Guarde el archivo.

[!INCLUDE [Publish Azure](../../../includes/spatial-anchors-publish-azure.md)]

[!INCLUDE [Run Share Anchors Sample](../../../includes/spatial-anchors-run-share-sample.md)]

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha usado Azure Cosmos DB para compartir identificadores de delimitador en todos los dispositivos. Para más información acerca de la biblioteca de Azure Spatial Anchors, consulte nuestra guía sobre cómo crear y localizar los delimitadores.

> [!div class="nextstepaction"]
> [Creación y localización de delimitadores mediante Azure Spatial Anchors](../create-locate-anchors-overview.md)
