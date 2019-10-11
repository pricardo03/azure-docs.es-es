---
title: Creación de una aplicación de iOS en Azure App Service Mobile Apps | Microsoft Docs
description: Siga este tutorial para aprender a usar back-ends de la aplicación móvil de Azure para el desarrollo de iOS en Objective-C o Swift.
services: app-service\mobile
documentationcenter: ios
author: elamalani
manager: crdun
editor: ''
ms.assetid: 6461a899-9340-42dd-b118-ffc5ba00e846
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: emalani
ms.openlocfilehash: cdfe395bca7ac73f00506da0180146310bd71d3d
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2019
ms.locfileid: "72027234"
---
# <a name="create-an-ios-app"></a>Creación de una aplicación iOS

[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

> [!NOTE]
> Visual Studio App Center admite servicios integrados de un extremo a otro fundamentales para el desarrollo de aplicaciones móviles. Los desarrolladores pueden usar los servicios de **compilación**, **prueba** y **distribución** para configurar la canalización de integración y entrega continuas. Una vez que se ha implementado la aplicación, los desarrolladores pueden supervisar el estado y el uso de su aplicación con los servicios de **análisis** y **diagnóstico**, e interactuar con los usuarios que utilizan el servicio de **Push** (inserción). Además, los desarrolladores pueden aprovechar **Auth** para autenticar a los usuarios y el servicio de **datos** para almacenar y sincronizar los datos de la aplicación en la nube.
> Si está pensando en integrar Cloud Services en una aplicación móvil, regístrese en App Center [App Center](https://appcenter.ms/?utm_source=zumo&utm_medium=Azure&utm_campaign=zumo%20doc) hoy mismo.

## <a name="overview"></a>Información general

En este tutorial se muestra cómo agregar [Azure App Service Mobile Apps](app-service-mobile-value-prop.md), un servicio back-end basado en la nube, a una aplicación de iOS. El primer paso es crear un nuevo back-end para dispositivos móviles en Azure. Después, es preciso descargar una sencilla aplicación iOS de *lista de tareas* de ejemplo que almacena datos en Azure.

Para completar este tutorial, es preciso tener un Mac y una [cuenta de Azure](https://azure.microsoft.com/pricing/free-trial/)

## <a name="create-a-new-azure-mobile-app-backend"></a>Creación de un nuevo back-end de Aplicaciones móviles de Azure

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="create-a-database-connection-and-configure-the-client-and-server-project"></a>Creación de una conexión de base de datos y configuración del proyecto de cliente y servidor
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="run-the-ios-app"></a>Ejecución de la aplicación iOS

[!INCLUDE [app-service-mobile-ios-run-app](../../includes/app-service-mobile-ios-run-app.md)]

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
