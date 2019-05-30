---
title: Introducción a Mobile Apps mediante Xamarin.Forms
description: Siga este tutorial para empezar a usar Mobile Apps para el desarrollo de Xamarin.Forms.
services: app-service\mobile
documentationcenter: xamarin
author: conceptdev
manager: crdun
ms.assetid: 5e692220-cc89-4548-96c8-35259722acf5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 05/09/2019
ms.author: crdun
ms.openlocfilehash: b0719f6ac2f99f9e665b1265665752dd53ccbaf0
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66242654"
---
# <a name="create-a-xamarinforms-app-with-azure"></a>Creación de una aplicación Xamarin.Forms con Azure

[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

En este tutorial se muestra cómo agregar un servicio de back-end basado en la nube a una aplicación móvil Xamarin.Forms mediante la característica Mobile Apps de Azure App Service como back-end. Va a crear tanto un back-end de Mobile Apps nuevo como una aplicación Xamarin.Forms simple de la lista de tareas pendientes que almacene los datos de la aplicación en Azure.

Completar este tutorial es un requisito previo para todos los tutoriales de Mobile Apps para aplicaciones Xamarin.Forms.

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, necesitará lo siguiente:

* Una cuenta de Azure activa. Si no dispone de ninguna cuenta, puede registrarse para obtener una versión de evaluación de Azure y conseguir hasta 10 aplicaciones móviles gratuitas que podrá seguir usando incluso después de que finalice la evaluación. Para más información, consulte [Obtener una evaluación gratuita de Azure](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio Tools para Xamarin en Visual Studio 2017 o posterior, o Visual Studio para Mac. Consulte la [página de instalación de Xamarin][Install Xamarin] para obtener instrucciones.

* (opcional) Para crear una aplicación iOS, es necesario un equipo Mac con Xcode 9.0 o posterior. Visual Studio para Mac se puede usar para desarrollar aplicaciones de iOS o Visual Studio 2017 o versiones posteriores puede usarse (siempre y cuando el equipo Mac está disponible en la red).

## <a name="create-a-new-mobile-apps-back-end"></a>Creación de un back-end de Mobile Apps
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="create-a-database-connection-and-configure-the-client-and-server-project"></a>Crear una conexión de base de datos y configurar el proyecto de cliente y servidor
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="run-the-xamarinforms-solution"></a>Ejecutar la solución de Xamarin.Forms

Para abrir la solución, se necesita Visual Studio Tools para Xamarin (consulte las [instrucciones de instalación de Xamarin][Install Xamarin]). Si las herramientas ya están instaladas, siga estos pasos para descargar y abrir la solución:

### <a name="visual-studio"></a>Visual Studio

1. Vaya a [Azure Portal](https://portal.azure.com/).

2. En la hoja Configuración de la aplicación móvil, haga clic en **Inicio rápido** (en Implementación) > **Xamarin.Forms**. En el paso 3, haga clic en **Crear una nueva aplicación** en caso de que no esté seleccionado.  A continuación, haga clic en el botón **Descargar** .

   Esta acción permite descargar un proyecto que contiene la aplicación cliente que está conectada a la aplicación móvil. Guarde el archivo comprimido del proyecto en el equipo local y anote dónde lo guardó.

3. Extraiga el proyecto que ha descargado y luego ábralo en Visual Studio.

4. Siga estas instrucciones para ejecutar los proyectos de Android o Windows y, si hay un equipo Mac en red disponible, el proyecto de iOS.

### <a name="visual-studio-for-mac"></a>Visual Studio para Mac

1. Vaya a la [portal Azure](https://portal.azure.com/) y vaya a la aplicación móvil que ha creado. En el `Overview` hoja, busque la dirección URL que es el punto de conexión público para la aplicación móvil. Por ejemplo, será el nombre del sitio para el nombre de mi aplicación "test123" https://test123.azurewebsites.net.

2. Abra el archivo `Constants.cs` en esta carpeta - xamarin.forms/ZUMOAPPNAME. Es el nombre de la aplicación `ZUMOAPPNAME`.

3. En `Constants.cs` class, reemplace `ZUMOAPPURL` variable con el punto de conexión público anterior.

    `public static string ApplicationURL = @"ZUMOAPPURL";`

    se convierte en

    `public static string ApplicationURL = @"https://test123.azurewebsites.net";`
    
4. Siga estas instrucciones para ejecutar los proyectos de Android o Windows y, si hay un equipo Mac en red disponible, el proyecto de iOS.

## <a name="optional-run-the-android-project"></a>(Opcional) Ejecución del proyecto de Android

En esta sección, ejecutará el proyecto de Xamarin.Android. Puede omitir esta sección si no está trabajando con dispositivos Android.

### <a name="visual-studio"></a>Visual Studio

1. Haga clic con el botón derecho en el proyecto de Android (Droid) y, después, seleccione **Establecer como proyecto de inicio**.

2. En el menú **Compilar**, seleccione **Administrador de configuración**.

3. En el cuadro de diálogo **Administrador de configuración**, active las casillas **Compilar** e **Implementar** junto al proyecto de Android, y asegúrese de que el proyecto de código compartido tenga activada la casilla **Compilar**.

4. Para compilar el proyecto e iniciar la aplicación en un emulador de Android, presione la tecla **F5** o haga clic en el botón **Iniciar**.

### <a name="visual-studio-for-mac"></a>Visual Studio para Mac

1. Haga clic con el botón derecho en el proyecto de Android y, después, seleccione **Establecer como proyecto de inicio**.

2. Para compilar el proyecto e iniciar la aplicación en un emulador de Android, seleccione el menú **Ejecutar** y, luego, **Iniciar depuración**.

En la aplicación, escriba un texto significativo, como *Aprender Xamarin*, y seleccione el botón del signo más ( **+** ).

![Aplicación de tareas pendientes de Android][11]

Esta acción envía una solicitud POST al nuevo back-end Mobile Apps hospedado en Azure. Los datos de la solicitud se insertan en la tabla TodoItem. El back-end de Mobile Apps devuelve los elementos almacenados en la tabla y los datos se muestran en la lista.

> [!NOTE]
> El código al que accede el back-end de Mobile Apps se encuentra en el archivo de C# **TodoItemManager.cs** del proyecto de código compartido de la solución.
>

## <a name="optional-run-the-ios-project"></a>(Opcional) Ejecución del proyecto de iOS

En esta sección, ejecutará el proyecto de Xamarin.iOS para dispositivos iOS. Puede omitir esta sección si no está trabajando con dispositivos iOS.

### <a name="visual-studio"></a>Visual Studio

1. Haga clic con el botón derecho en el proyecto de iOS y, después, seleccione **Establecer como proyecto de inicio**.

2. En el menú **Compilar**, seleccione **Administrador de configuración**.

3. En el cuadro de diálogo **Administrador de configuración**, active las casillas **Compilar** e **Implementar** junto al proyecto de iOS, y asegúrese de que el proyecto de código compartido tenga activada la casilla **Compilar**.

4. Para compilar el proyecto e iniciar la aplicación en el emulador de iPhone, seleccione la tecla **F5**.

### <a name="visual-studio-for-mac"></a>Visual Studio para Mac

1. Haga clic con el botón derecho en el proyecto de iOS y, después, seleccione **Establecer como proyecto de inicio**.

2. En el menú **Ejecutar**, seleccione **Iniciar depuración** para compilar el proyecto e iniciar la aplicación en el emulador de iPhone.

En la aplicación, escriba un texto significativo, como *Aprender Xamarin*, y seleccione el botón del signo más ( **+** ).

![Aplicación de tareas pendientes de iOS][10]

Esta acción envía una solicitud POST al nuevo back-end Mobile Apps hospedado en Azure. Los datos de la solicitud se insertan en la tabla TodoItem. El back-end de Mobile Apps devuelve los elementos almacenados en la tabla y los datos se muestran en la lista.

> [!NOTE]
> El código al que accede el back-end de Mobile Apps se encuentra en el archivo de C# **TodoItemManager.cs** del proyecto de código compartido de la solución.
>

## <a name="optional-run-the-windows-project"></a>(Opcional) Ejecución del proyecto de Windows

En esta sección, ejecutará el proyecto de Plataforma universal de Windows (UWP) de Xamarin.Forms para dispositivos Windows. Puede omitir esta sección si no está trabajando con dispositivos Windows.

### <a name="visual-studio"></a>Visual Studio

1. Haga clic con el botón derecho en cualquier proyecto de UWP y, luego, seleccione **Establecer como proyecto de inicio**.

2. En el menú **Compilar**, seleccione **Administrador de configuración**.

3. En el cuadro de diálogo **Administrador de configuración**, active las casillas **Compilar** e **Implementar** junto al proyecto de Windows, y asegúrese de que el proyecto de código compartido tenga activada la casilla **Compilar**.

4. Para compilar el proyecto e iniciar la aplicación en un emulador de Windows, presione la tecla **F5** o haga clic en el botón **Iniciar** (que debe poner **Equipo Local**).

> [!NOTE]
> El proyecto de Windows no se puede ejecutar en macOS.

En la aplicación, escriba un texto significativo, como *Aprender Xamarin*, y seleccione el botón del signo más ( **+** ).

Esta acción envía una solicitud POST al nuevo back-end Mobile Apps hospedado en Azure. Los datos de la solicitud se insertan en la tabla TodoItem. El back-end de Mobile Apps devuelve los elementos almacenados en la tabla y los datos se muestran en la lista.

![Aplicación de tareas pendientes de UWP][12]

> [!NOTE]
> Encontrará el código con el que se accede al back-end de Mobile Apps en el archivo de C# **TodoItemManager.cs** del proyecto de biblioteca de clases portable de la solución.
>

## <a name="troubleshooting"></a>solución de problemas

Si tiene problemas para compilar la solución, ejecute el administrador de paquetes NuGet y actualice a la versión más reciente de `Xamarin.Forms` y, en el proyecto de Android, actualice los paquetes de compatibilidad de `Xamarin.Android`. Puede que los proyectos de inicio rápido no incluyan las versiones más recientes.

Tenga en cuenta que todos los paquetes de soporte a los que se hace referencia en el proyecto Android deben tener la misma versión. El [paquete NuGet para Azure Mobile Apps](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) tiene una dependencia `Xamarin.Android.Support.CustomTabs` para la plataforma Android, por lo que si el proyecto usa paquetes de compatibilidad más recientes, debe instalar este paquete con la versión requerida directamente para evitar conflictos.

<!-- Images. -->
[10]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-android.png
[12]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-windows.png

<!-- URLs. -->
[Install Xamarin]: https://docs.microsoft.com/xamarin/cross-platform/get-started/installation/