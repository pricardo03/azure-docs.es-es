---
title: Creación de una aplicación de Android en Azure App Service Mobile Apps | Microsoft Docs
description: Siga este tutorial para aprender a usar back-ends de aplicación móvil de Azure para el desarrollo de Android
services: app-service\mobile
documentationcenter: android
author: elamalani
manager: crdun
editor: ''
ms.assetid: 355f0959-aa7f-472c-a6c7-9eecea3a34a9
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: conceptual
ms.date: 5/6/2019
ms.author: emalani
ms.openlocfilehash: 0f79d2e7112847dbc5553e60f12bc872a5c0b5d5
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2019
ms.locfileid: "72027644"
---
# <a name="create-an-android-app"></a>Creación de una aplicación de Android
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

> [!NOTE]
> Visual Studio App Center admite servicios integrados de un extremo a otro fundamentales para el desarrollo de aplicaciones móviles. Los desarrolladores pueden usar los servicios de **compilación**, **prueba** y **distribución** para configurar la canalización de integración y entrega continuas. Una vez que se ha implementado la aplicación, los desarrolladores pueden supervisar el estado y el uso de su aplicación con los servicios de **análisis** y **diagnóstico**, e interactuar con los usuarios que utilizan el servicio de **Push** (inserción). Además, los desarrolladores pueden aprovechar **Auth** para autenticar a los usuarios y el servicio de **datos** para almacenar y sincronizar los datos de la aplicación en la nube.
> Si está pensando en integrar Cloud Services en una aplicación móvil, regístrese en App Center [App Center](https://appcenter.ms/signup?utm_source=zumo&utm_medium=Azure&utm_campaign=zumo%20doc) hoy mismo.

## <a name="overview"></a>Información general
En este tutorial se muestra cómo agregar un servicio back-end basado en la nube a una aplicación móvil de Android con un back-end de la aplicación móvil de Azure.  Creará tanto un back-end de aplicación móvil nuevo como una aplicación de Android simple de la *lista de tareas pendientes* que almacene los datos de la aplicación en Azure.

Completar este tutorial es un requisito previo para todos los demás tutoriales de Android sobre cómo usar la característica Mobile Apps en Azure App Service.

## <a name="prerequisites"></a>Requisitos previos
Para completar este tutorial, necesitará lo siguiente:

* [Herramientas para desarrolladores de Android](https://developer.android.com/sdk/index.html), que incluyen el entorno de desarrollo integrado de Android Studio y la plataforma de Android más reciente.
* Android SDK de Azure Mobile.
* Una [cuenta de Azure activa](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-a-new-azure-mobile-app-backend"></a>Creación de un nuevo back-end de Aplicaciones móviles de Azure
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="create-a-database-connection-and-configure-the-client-and-server-project"></a>Creación de una conexión de base de datos y configuración del proyecto de cliente y servidor
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="run-the-android-app"></a>Ejecución de la aplicación Android
[!INCLUDE [app-service-mobile-android-run-app](../../includes/app-service-mobile-android-run-app.md)]

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
