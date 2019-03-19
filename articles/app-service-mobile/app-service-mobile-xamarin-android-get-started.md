---
title: Introducción a Azure Mobile Apps para aplicaciones Xamarin.Android
description: Siga este tutorial para empezar a usar Azure Mobile Apps para el desarrollo de Xamarin Android.
services: app-service\mobile
documentationcenter: xamarin
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 81649dd3-544f-40ff-b9b7-60c66d683e60
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: crdun
ms.openlocfilehash: 67bf0499b6dcf4257380f601c802723b2993a092
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/12/2019
ms.locfileid: "57771487"
---
# <a name="create-a-xamarinandroid-app"></a>Creación de una aplicación Xamarin.Android
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Información general
En este tutorial se muestra cómo agregar un servicio de back-end basado en la nube a una aplicación Xamarin.Android. Para obtener más información, consulte [¿Qué es Mobile Apps?](app-service-mobile-value-prop.md)

La siguiente captura de pantalla muestra la aplicación final:

![][0]

Completar este tutorial es un requisito previo para todos los tutoriales de Mobile Apps para aplicaciones Xamarin.Android.

## <a name="prerequisites"></a>Requisitos previos
Para completar este tutorial, debe cumplir los siguientes requisitos previos:

* Una cuenta de Azure activa. Si no dispone de ninguna cuenta, puede registrarse para obtener una versión de evaluación de Azure y conseguir hasta 10 aplicaciones móviles gratuitas. Para obtener más información, consulte [Evaluación gratuita de Azure](https://azure.microsoft.com/pricing/free-trial/).
* Visual Studio con Xamarin. Consulte [Configuración e instalación](https://msdn.microsoft.com/library/mt613162.aspx) para obtener instrucciones.

## <a name="create-an-azure-mobile-app-backend"></a>Creación de un nuevo back-end de Azure Mobile App
Siga estos pasos para crear un back-end de Mobile App.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Ahora ha aprovisionado un back-end de aplicación móvil de Azure que puede usarse por las aplicaciones del cliente móvil. Después, descargue un proyecto de servidor para un back-end de "lista de tareas" sencillo y publíquelo en Azure.

## <a name="configure-the-server-project"></a>Configuración del proyecto de servidor
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinandroid-app"></a>Descarga y ejecución de la aplicación Xamarin.Android
1. En **Descargar y ejecutar el proyecto de Xamarin.Android**, haga clic en el botón **Descargar**.

      Guarde el archivo comprimido del proyecto en el equipo local y anote dónde lo guardó.
2. Presione la tecla **F5** para compilar el proyecto e iniciar la aplicación.
3. En la aplicación, escriba un texto significativo, como *Realizar el tutorial* y luego haga clic en el botón **Agregar**.

    ![][10]

    Los datos de la solicitud se insertan en la tabla TodoItem. El back-end de la aplicación móvil devuelve los elementos almacenados en la tabla y los datos aparecen en la lista.

   > [!NOTE]
   > Puede revisar el código de acceso al back-end de aplicación móvil para consultar e insertar datos; este se encuentra en el archivo de C# ToDoActivity.cs.
   >
   >

## <a name="troubleshooting"></a>solución de problemas
Si tiene problemas al compilar la solución, ejecute el administrador de paquetes NuGet y actualice los paquetes de compatibilidad de `Xamarin.Android`. Puede que los proyectos de inicio rápido no incluyan las versiones más recientes.

Tenga en cuenta que todos los paquetes de soporte a los que se hace referencia en el proyecto deben tener la misma versión. El [paquete NuGet de Azure Mobile Apps](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) tiene la dependencia `Xamarin.Android.Support.CustomTabs` para la plataforma Android, por lo que si el proyecto usa paquetes de soporte más recientes, tiene que instalar este paquete con la versión necesaria directamente para evitar conflictos.

## <a name="next-steps"></a>Pasos siguientes
* [Add Offline Sync to your app](app-service-mobile-xamarin-android-get-started-offline-data.md) (Incorporación de sincronización sin conexión a la aplicación)
* [Incorporación de autenticación a la aplicación](app-service-mobile-xamarin-android-get-started-users.md)
* [Agregar notificaciones push a la aplicación de Xamarin.Android](app-service-mobile-xamarin-android-get-started-push.md)
* [Uso del cliente administrado para Azure Mobile Apps](app-service-mobile-dotnet-how-to-use-client-library.md)

<!-- Images. -->
[0]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-completed-android.png
[6]: ./media/app-service-mobile-xamarin-android-get-started/mobile-portal-quickstart-xamarin.png
[8]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-vs.png
[9]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-xs.png
[10]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-startup-android.png

<!-- URLs. -->
[Azure Portal]: https://azure.portal.com/
[Visual Studio]: https://go.microsoft.com/fwLink/p/?LinkID=534203
