---
title: Creación de una Plataforma universal de Windows (UWP) que se use en Azure Mobile Apps | Microsoft Docs
description: Siga este tutorial para aprender a usar back-ends de aplicación móvil de Azure para el desarrollo de aplicaciones para Plataforma universal de Windows (UWP) en C#, Visual Basic o JavaScript.
services: app-service\mobile
documentationcenter: windows
author: elamalani
manager: crdun
editor: ''
ms.assetid: 47124296-2908-4d92-85e0-05c4aa6db916
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: emalani
ms.openlocfilehash: 2b718a000bb5805cc615a59aebb2d5dcca6906a4
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67440203"
---
# <a name="create-a-windows-app-with-an-azure-backend"></a>Creación de una aplicación de Windows con un back-end de Azure

[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

> [!NOTE]
> Visual Studio App Center está invirtiendo en servicios nuevos e integrados fundamentales para el desarrollo de aplicaciones móviles. Los desarrolladores pueden usar los servicios de **compilación**, **prueba** y **distribución** para configurar la canalización de integración y entrega continuas. Una vez que se ha implementado la aplicación, los desarrolladores pueden supervisar el estado y el uso de su aplicación con los servicios de **análisis** y **diagnóstico**, e interactuar con los usuarios con el servicio de **inserción**. Además, los desarrolladores pueden aprovechar **Auth** para autenticar a los usuarios y el servicio de **datos** para almacenar y sincronizar los datos de la aplicación en la nube. Consulte [App Center](https://appcenter.ms/?utm_source=zumo&utm_campaign=app-service-mobile-windows-store-dotnet-get-started) hoy mismo.
>

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

## <a name="run-the-client-project"></a>Ejecutar el proyecto de cliente

1. Abra el proyecto de UWP.

2. Vaya a [Azure Portal](https://portal.azure.com/) y diríjase a la aplicación móvil que ha creado. En la hoja `Overview`, busque la dirección URL que es el punto de conexión público de la aplicación móvil. Por ejemplo, el nombre de sitio para el nombre de mi aplicación "test123" será https://test123.azurewebsites.net.

3. Abra el archivo `App.xaml.cs` en esta carpeta - windows-uwp-cs/ZUMOAPPNAME/. El nombre de la aplicación es `ZUMOAPPNAME`.

4. En la clase `App`, reemplace el parámetro `ZUMOAPPURL` por el punto de conexión público anterior.

    `public static MobileServiceClient MobileService = new MobileServiceClient("ZUMOAPPURL");`

    se convierte en
    
    `public static MobileServiceClient MobileService = new MobileServiceClient("https://test123.azurewebsites.net");`

5. Presione la tecla F5 para implementar y ejecutar la aplicación.

6. En la aplicación, escriba texto significativo, como *Realice el tutorial*, en el cuadro de texto **Insertar un TodoItem** y, a continuación, haga clic en **Guardar**.

    ![Inicio rápido de Windows completo - Escritorio](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-startup.png)

    Esta acción envía una solicitud POST al nuevo back-end de aplicación móvil hospedado en Azure.
