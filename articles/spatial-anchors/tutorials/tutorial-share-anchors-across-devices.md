---
title: 'Tutorial: Uso compartido de Azure Spatial Anchors entre sesiones y dispositivos | Microsoft Docs'
description: En este tutorial, obtendrá información sobre cómo compartir los identificadores espaciales de Azure Spatial Anchors entre dispositivos Android/iOS de Unity con un servicio de back-end.
author: ramonarguelles
manager: vicenterivera
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: ff9868dd7347812eb6ef566288ec364bc89b6955
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2019
ms.locfileid: "58629302"
---
# <a name="tutorial-share-azure-spatial-anchors-across-sessions-and-devices"></a>Tutorial: Uso compartido de Azure Spatial Anchors entre sesiones y dispositivos

En este tutorial, obtendrá información sobre cómo usar [Azure Spatial Anchors](../overview.md) para crear delimitadores durante una sesión y, a continuación, buscarlos, en el mismo dispositivo o en uno diferente. Estos mismos delimitadores podrían también ser encontrados por varios dispositivos en el mismo lugar y al mismo tiempo.

![Persistencia](./media/persistence.gif)

Azure Spatial Anchors es un servicio multiplataforma para desarrolladores que le permite crear experiencias de realidad mixta mediante objetos cuya ubicación persiste en todos los dispositivos a lo largo del tiempo. Cuando haya terminado, tendrá una aplicación que se puede implementar en dos o más dispositivos. Los delimitadores espaciales creados por una instancia de Azure Spatial Anchors se pueden compartir con las demás.

Aprenderá a:

> [!div class="checklist"]
> * Implementar una aplicación web de ASP.NET Core en Azure que se pueda usar para compartir los delimitadores y almacenarlos en memoria durante un tiempo.
> * Configurar la escena AzureSpatialAnchorsLocalSharedDemo dentro del ejemplo de Unity de nuestros inicios rápidos para aprovechar las ventajas del uso compartido de la aplicación web de delimitadores.
> * Realizar la implementación y ejecución en uno o varios dispositivos.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [Share Anchors Sample Prerequisites](../../../includes/spatial-anchors-share-sample-prereqs.md)]

Es importante destacar que, aunque se va a usar Unity y una aplicación web de ASP.NET Core en este tutorial, es solo para mostrar un ejemplo sobre cómo compartir los identificadores espaciales de Azure Spatial Anchors con otros dispositivos. Se pueden usar otros lenguajes y tecnologías de back-end para lograr el mismo objetivo. Además, la aplicación web de ASP.NET Core usada en este tutorial tiene una dependencia del SDK de .NET Core 2.2. Funciona bien en aplicaciones web de Azure normales (para Windows), pero no funciona en aplicaciones web de Azure para Linux.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project-in-unity"></a>Apertura del proyecto de ejemplo en Unity

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

## <a name="deploy-your-sharing-anchors-service"></a>Implementación del uso compartido del servicio de delimitadores

Abra Visual Studio y abra el proyecto en la carpeta `Sharing\SharingServiceSample`.

[!INCLUDE [Publish Azure](../../../includes/spatial-anchors-publish-azure.md)]

[!INCLUDE [Run Share Anchors Sample](../../../includes/spatial-anchors-run-share-sample.md)]

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha implementado una aplicación web de ASP.NET Core en Azure y ha configurado e implementado una aplicación de Unity. Ha creado delimitadores espaciales con la aplicación y los ha compartido con otros dispositivos mediante la aplicación web de ASP.NET Core.

Para más información sobre cómo mejorar una aplicación web de ASP.NET Core para que use Azure Cosmos DB a fin de almacenar los delimitadores de anclaje espacial, continúe con el tutorial siguiente. Azure Cosmos DB le dará persistencia a la aplicación web de ASP.NET Core. Si lo hace, permitirá que la aplicación cree un delimitador hoy y que días más tarde regrese y pueda encontrarlo de nuevo, con el identificador de delimitador almacenado en la aplicación web.

> [!div class="nextstepaction"]
> [Tutorial: Uso de Azure Cosmos DB para almacenar delimitadores](./tutorial-use-cosmos-db-to-store-anchors.md)
