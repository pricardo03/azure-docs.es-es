---
title: 'Tutorial: Uso compartido entre sesiones y dispositivos con Azure Spatial Anchors | Microsoft Docs'
description: En este tutorial, obtendrá información sobre cómo compartir los identificadores espaciales de Azure Spatial Anchors entre dispositivos Android/iOS de Unity con un servicio de back-end.
author: ramonarguelles
manager: vicenterivera
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: 7d9fe58b7db60513eed81aae628ebd7ca754a53a
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "57901311"
---
# <a name="tutorial-sharing-across-sessions-and-devices-with-azure-spatial-anchors"></a>Tutorial: Uso compartido entre sesiones y dispositivos con Azure Spatial Anchors

En este tutorial se muestra cómo usar [Azure Spatial Anchors](../overview.md) para:

1. Crear delimitadores en una sesión y encontrarlos en otra sesión en el mismo dispositivo u otro. Por ejemplo, en otro momento.
2. Crear delimitadores que puedan encontrar varios dispositivos en el mismo lugar al mismo tiempo.

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

## <a name="open-the-sample-project-in-unity"></a>Apertura del proyecto de ejemplo en Unity

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

[!INCLUDE [Run Share Anchors Sample](../../../includes/spatial-anchors-run-share-sample.md)]

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha implementado una aplicación web de ASP.NET Core en Azure y ha configurado e implementado una aplicación de Unity. Ha creado delimitadores espaciales con la aplicación y los ha compartido con otros dispositivos mediante la aplicación web de ASP.NET Core.

Para más información sobre cómo mejorar su aplicación web de ASP.NET Core para que use Azure Cosmos DB para almacenar los delimitadores espaciales compartidos, continúe con el tutorial siguiente.

> [!div class="nextstepaction"]
> [Tutorial: Uso de Azure Cosmos DB para almacenar delimitadores](./tutorial-use-cosmos-db-to-store-anchors.md)
