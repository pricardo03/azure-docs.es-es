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
ms.date: 08/17/2018
ms.author: crdun
ms.openlocfilehash: 289d50fc9d71ee5ea8960fd077e91ef6f68027cb
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62127988"
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
* [Visual Studio Community 2017].
* Debe estar familiarizado con el desarrollo de aplicaciones de UWP. Visite la [documentación de UWP](https://docs.microsoft.com/windows/uwp/) para aprender a [configurar](https://docs.microsoft.com/windows/uwp/get-started/get-set-up) para compilar aplicaciones de UWP.

## <a name="create-a-new-azure-mobile-app-backend"></a>Creación de un nuevo back-end de Aplicaciones móviles de Azure

Siga estos pasos para crear un nuevo back-end de aplicación móvil.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Ahora ha aprovisionado un back-end de aplicación móvil de Azure que puede usarse por las aplicaciones del cliente móvil. Después, descargará un proyecto de servidor para un back-end de "lista de tareas" sencillo y lo publicará en Azure.

## <a name="configure-the-server-project"></a>Configuración del proyecto de servidor

[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-client-project"></a>Descarga y ejecución del proyecto de cliente

Una vez configurado el back-end de aplicación móvil, puede crear una nueva aplicación cliente o modificar una aplicación existente para conectarse a Azure. En esta sección, descargará un proyecto de una aplicación de ejemplo de UWP que se personaliza para conectarse al back-end de aplicación móvil.

1. De nuevo en la hoja **Inicio rápido** para el back-end de aplicación móvil, haga clic en **Crear una nueva aplicación** > **Descargar** y extraiga los archivos de proyecto comprimidos en el equipo local.

    ![Descargar el proyecto de inicio rápido de Windows](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-app-windows-quickstart.png)

2. Abra el proyecto de UWP y presione la tecla F5 para implementar y ejecutar la aplicación.
3. En la aplicación, escriba texto significativo, como *Realice el tutorial*, en el cuadro de texto **Insertar un TodoItem** y, a continuación, haga clic en **Guardar**.

    ![Inicio rápido de Windows completo - Escritorio](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-startup.png)

    Esta acción envía una solicitud POST al nuevo back-end de aplicación móvil hospedado en Azure.

> [!TIP]
> Si usa el back-end de .NET, puede agregar el proyecto de la aplicación de UWP a la misma solución que el proyecto de servidor. Así se facilita la depuración y prueba tanto de la aplicación como del back-end en la misma solución de Visual Studio. Para agregar un proyecto de una aplicación de UWP a la solución de back-end, debe usar Visual Studio 2017.

## <a name="next-steps"></a>Pasos siguientes

* [Incorporación de autenticación a la aplicación](app-service-mobile-windows-store-dotnet-get-started-users.md)  
   Aprenda a autenticar a los usuarios de su aplicación con un proveedor de identidades.
* [Incorporación de notificaciones push a su aplicación](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  : aprenda a agregar a la aplicación compatibilidad con notificaciones push y a configurar su back-end de aplicación móvil para usar Azure Notification Hubs para enviar notificaciones push.
* [Activación de la sincronización sin conexión para la aplicación de Windows](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Aprenda a agregar compatibilidad sin conexión a una aplicación con un back-end de aplicación móvil. La sincronización sin conexión permite a los usuarios finales interactuar con una aplicación móvil (ver, agregar o modificar datos) aun cuando no haya conexión de red.

<!-- Anchors. -->
<!-- Images. -->
<!-- URLs. -->
[Mobile App SDK]: https://go.microsoft.com/fwlink/?LinkId=257545
[Azure portal]: https://portal.azure.com/
[Visual Studio Community 2017]: https://go.microsoft.com/fwLink/p/?LinkID=534203
