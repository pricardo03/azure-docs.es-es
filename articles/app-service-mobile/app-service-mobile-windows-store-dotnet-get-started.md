---
title: Creación de una Plataforma universal de Windows (UWP) que se use en Azure Mobile Apps | Microsoft Docs
description: Siga este tutorial para aprender a usar back-ends de aplicación móvil de Azure para el desarrollo de aplicaciones para Plataforma universal de Windows (UWP) en C#, Visual Basic o JavaScript.
services: app-service\mobile
documentationcenter: windows
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 47124296-2908-4d92-85e0-05c4aa6db916
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 05/09/2019
ms.author: crdun
ms.openlocfilehash: be579b631fd910c56f2c360d6aace5b8d35c22e5
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66235991"
---
# <a name="create-a-windows-app-with-an-azure-backend"></a>Creación de una aplicación de Windows con un back-end de Azure

[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Información general

En este tutorial se muestra cómo agregar un servicio back-end basado en la nube a una aplicación para Plataforma universal de Windows (UWP). Para obtener más información, consulte [¿Qué es Mobile Apps?](app-service-mobile-value-prop.md) A continuación se muestran las capturas de pantalla de la aplicación completada:

![Aplicación de escritorio completada](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed-desktop.png)

Completar este tutorial es un requisito previo para todos los tutoriales de aplicaciones móviles para aplicaciones para UWP.

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, necesitará lo siguiente:

* Una cuenta de Azure activa. Si no dispone de ninguna cuenta, puede registrarse para obtener una versión de evaluación de Azure y conseguir hasta 10 aplicaciones móviles gratuitas que podrá seguir usando incluso después de que finalice la evaluación. Para obtener más información, consulte [Evaluación gratuita de Azure](https://azure.microsoft.com/pricing/free-trial/).
* Windows 10
* Visual Studio Community 2017.
* Debe estar familiarizado con el desarrollo de aplicaciones de UWP. Visite la [documentación de UWP](https://docs.microsoft.com/windows/uwp/) para aprender a [configurar](https://docs.microsoft.com/windows/uwp/get-started/get-set-up) para compilar aplicaciones de UWP.

## <a name="create-a-new-azure-mobile-app-backend"></a>Creación de un nuevo back-end de Aplicaciones móviles de Azure

Siga estos pasos para crear un nuevo back-end de aplicación móvil.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="create-a-database-connection-and-configure-the-client-and-server-project"></a>Crear una conexión de base de datos y configurar el proyecto de cliente y servidor
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="run-the-client-project"></a>Ejecute el proyecto de cliente

1. Abra el proyecto UWP.

2. Vaya a la [portal Azure](https://portal.azure.com/) y vaya a la aplicación móvil que ha creado. En el `Overview` hoja, busque la dirección URL que es el punto de conexión público para la aplicación móvil. Por ejemplo, será el nombre del sitio para el nombre de mi aplicación "test123" https://test123.azurewebsites.net.

3. Abra el archivo `App.xaml.cs` en esta carpeta - windows-uwp-cs/ZUMOAPPNAME /. Es el nombre de la aplicación `ZUMOAPPNAME`.

4. En `App` class, reemplace `ZUMOAPPURL` parámetro con el punto de conexión público anterior.

    `public static MobileServiceClient MobileService = new MobileServiceClient("ZUMOAPPURL");`

    se convierte en
    
    `public static MobileServiceClient MobileService = new MobileServiceClient("https://test123.azurewebsites.net");`

5. Presione la tecla F5 para implementar y ejecutar la aplicación.

6. En la aplicación, escriba texto significativo, como *Realice el tutorial*, en el cuadro de texto **Insertar un TodoItem** y, a continuación, haga clic en **Guardar**.

    ![Inicio rápido de Windows completo - Escritorio](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-startup.png)

    Esta acción envía una solicitud POST al nuevo back-end de aplicación móvil hospedado en Azure.
