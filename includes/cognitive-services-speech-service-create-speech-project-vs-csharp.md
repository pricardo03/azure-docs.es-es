---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 09/13/2018
ms.author: wolfma
ms.openlocfilehash: 8ff8e8341b6f39f66c2cc8014d41d3d3a2918d2b
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2018
ms.locfileid: "47454540"
---
1. Inicie Visual Studio 2017.
 
1. Asegúrese de que la carga de trabajo del **entorno de escritorio de .NET** esté disponible. Seleccione **Herramientas** \> **Get Tools and Features** (Obtener herramientas y características) desde la barra de menús de Visual Studio para abrir el instalador de Visual Studio. Si esta carga de trabajo ya está habilitada, cierre el cuadro de diálogo. 

    En caso contrario, marque la casilla que se encuentra junto a la opción de **desarrollo de escritorio. NET** y, a continuación, haga clic en el botón **Modificar** situado en la esquina inferior derecha del cuadro de diálogo. La instalación de la nueva característica tardará un momento.

    ![Habilitación del desarrollo de escritorio .NET](~/articles/cognitive-services/speech-service/media/sdk/vs-enable-net-desktop-workload.png)

1. Cree una aplicación de consola nueva de Visual C#. En el cuadro de diálogo **New Project** (Proyecto nuevo), en el panel de la izquierda, expanda **Installed** (Instalado) \> **Visual C#** \> **Windows Desktop** (Escritorio de Windows) y seleccione **Console App (.NET Framework)** (Aplicación de consola [.NET Framework]). Para el nombre del proyecto, escriba *helloworld*.

    ![Creación de aplicación de consola de Visual C# (.NET Framework)](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-01-new-console-app.png "Creación de aplicación de consola de Visual C# (.NET Framework)")

1. Instale y haga referencia al [paquete NuGet del SDK de Voz](https://aka.ms/csspeech/nuget). En el Explorador de soluciones, haga clic con el botón derecho en la solución y seleccione **Manage NuGet Packages for Solution** (Administrar paquetes de NuGet para la solución).

    ![Hacer clic con el botón derecho en Manage NuGet Packages for Solution (Administrar paquetes NuGet para la solución)](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-02-manage-nuget-packages.png "Manage NuGet Packages for Solution")

1. En la esquina superior derecha, en el campo **Origen del paquete**, elija **Nuget.org**. Busque el paquete `Microsoft.CognitiveServices.Speech` e instálelo en el proyecto **helloworld**.

    ![Instalación del paquete NuGet Microsoft.CognitiveServices.Speech](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-03-nuget-install-1.0.0.png "Instalación del paquete Nuget")

1. Acepte la licencia que aparece para comenzar la instalación del paquete de NuGet.

    ![Aceptación de licencia](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-04-nuget-license.png "Accept the license")

    Después de instalar el paquete, aparecerá una confirmación en la consola del Administrador de paquetes.

1. Cree una configuración de plataforma que coincida con la arquitectura del PC mediante Configuration Manager. Seleccione **Compilar** > **Administrador de configuración**.

    ![Inicio del Administrador de configuración](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-05-cfg-manager-click.png "Launch the configuration manager")

1. En el cuadro de diálogo **Administrador de configuración**, agregue una nueva plataforma. En la lista desplegable **Plataforma de soluciones activas**, seleccione **Nuevo**.

    ![Incorporación de una nueva plataforma en la ventana del Administrador de configuración](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-06-cfg-manager-new.png "Add a new platform under the configuration manager window")

1. Si está ejecutando Windows de 64 bits, cree una nueva configuración de plataforma denominada `x64`. Si está ejecutando Windows de 32 bits, cree una nueva configuración de plataforma denominada `x86`.

    ![En Windows de 64 bits, incorporación de una nueva plataforma denominada "x64"](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-07-cfg-manager-add-x64.png "Add x64 platform")


