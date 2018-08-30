---
title: 'Guía de inicio rápido: Reconocimiento de voz en C# en .NET Framework en Windows mediante Speech SDK de Cognitive Services'
titleSuffix: Microsoft Cognitive Services
description: Obtenga información sobre cómo reconocer la voz en C# en .NET Framework en Windows mediante el SDK de Voz de Cognitive Services
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/16/2018
ms.author: wolfma
ms.openlocfilehash: 20d0f4736d3aa5f23d9061b4482da7a623862344
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2018
ms.locfileid: "43128228"
---
# <a name="quickstart-recognize-speech-in-c-under-net-framework-on-windows-using-the-speech-sdk"></a>Guía de inicio rápido: Reconocimiento de voz en C# en .NET Framework en Windows mediante el SDK de Voz

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

En este artículo, obtendrá información sobre cómo crear una aplicación de la consola de C# para .NET Framework en Windows mediante el SDK de Voz de Cognitive Services para transcribir la conversión de voz en texto.
La aplicación se compila con el [paquete NuGet del SDK de Voz de Microsoft Cognitive Services](https://aka.ms/csspeech/nuget) y Microsoft Visual Studio 2017.

## <a name="prerequisites"></a>Requisitos previos

* Clave de suscripción para Speech Service. Consulte [Pruebe Speech Service gratis](get-started.md).
* Un PC Windows con micrófono operativo.
* Visual Studio 2017, Community Edition o superior.
* Carga de trabajo de **Desarrollo de escritorio de .NET** en Visual Studio. Puede habilitarlo en **Herramientas** \> **Get Tools and Features** (Obtener herramientas y características).

  ![Habilitación del desarrollo de escritorio .NET](media/sdk/vs-enable-net-desktop-workload.png)

  ![Habilitación del desarrollo multiplataforma de .NET Core](media/sdk/vs-enable-net-desktop-workload.png)

## <a name="create-a-visual-studio-project"></a>Creación de un proyecto de Visual Studio

1. En Visual Studio 2017, cree una nueva aplicación de consola en Visual C#. En el cuadro de diálogo **New Project** (Proyecto nuevo), en el panel de la izquierda, expanda **Installed** (Instalado) \> **Visual C#** \> **Windows Desktop** (Escritorio de Windows) y seleccione **Console App (.NET Framework)** (Aplicación de consola [.NET Framework]). Para el nombre del proyecto, escriba *helloworld*.

    ![Creación de aplicación de consola de Visual C# (.NET Framework)](media/sdk/qs-csharp-dotnet-windows-01-new-console-app.png "Creación de aplicación de consola de Visual C# (.NET Framework)")

1. Instale y haga referencia al [paquete NuGet del SDK de Voz](https://aka.ms/csspeech/nuget). En el Explorador de soluciones, haga clic con el botón derecho en la solución y seleccione **Manage NuGet Packages for Solution** (Administrar paquetes de NuGet para la solución).

    ![Hacer clic con el botón derecho en Manage NuGet Packages for Solution (Administrar paquetes NuGet para la solución)](media/sdk/qs-csharp-dotnet-windows-02-manage-nuget-packages.png "Manage NuGet Packages for Solution")

1. En la esquina superior derecha, en el campo **Origen del paquete**, elija **Nuget.org**. Busque el paquete `Microsoft.CognitiveServices.Speech` e instálelo en el proyecto **helloworld**.

    ![Instalación del paquete NuGet Microsoft.CognitiveServices.Speech](media/sdk/qs-csharp-dotnet-windows-03-nuget-install-0.5.0.png "Install Nuget package")

1. Acepte la licencia mostrada.

    ![Aceptación de licencia](media/sdk/qs-csharp-dotnet-windows-04-nuget-license.png "Accept the license")

1. La línea de salida siguiente aparece en la consola del Administrador de paquetes.

   ```text
   Successfully installed 'Microsoft.CognitiveServices.Speech 0.6.0' to helloworld
   ```

## <a name="create-a-platform-configuration-matching-your-pc-architecture"></a>Creación de una configuración de plataforma que coincida con la arquitectura de PC

En esta sección, agregará una nueva plataforma a la configuración que coincida con la arquitectura del procesador.

1. Inicie el Administrador de configuración. Seleccione **Compilar** > **Administrador de configuración**.

    ![Inicio del Administrador de configuración](media/sdk/qs-csharp-dotnet-windows-05-cfg-manager-click.png "Launch the configuration manager")

1. En el cuadro de diálogo **Administrador de configuración**, agregue una nueva plataforma. En la lista desplegable **Plataforma de soluciones activas**, seleccione **Nuevo**.

    ![Incorporación de una nueva plataforma en la ventana del Administrador de configuración](media/sdk/qs-csharp-dotnet-windows-06-cfg-manager-new.png "Add a new platform under the configuration manager window")

1. Si está ejecutando Windows de 64 bits, cree una nueva configuración de plataforma denominada `x64`. Si está ejecutando Windows de 32 bits, cree una nueva configuración de plataforma denominada `x86`. En este artículo, se crea una configuración de plataforma `x64`.

    ![En Windows de 64 bits, incorporación de una nueva plataforma denominada "x64"](media/sdk/qs-csharp-dotnet-windows-07-cfg-manager-add-x64.png "Add x64 platform")

## <a name="add-the-sample-code"></a>Incorporación del código de ejemplo

1. Abra `Program.cs` y reemplace todo el código que contiene por lo siguiente.

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-dotnet-windows/helloworld/Program.cs#code)]

1. Reemplace la cadena `YourSubscriptionKey` por la clave de suscripción.

1. Reemplace la cadena `YourServiceRegion` por la [región](regions.md) asociada con la suscripción (por ejemplo, `westus` para la suscripción de evaluación gratuita).

1. Guarde los cambios en el proyecto.

## <a name="build-and-run-the-sample"></a>Compilación y ejecución del ejemplo

1. Compile la aplicación. En la barra de menús, seleccione **Compilar** > **Compilar solución**. El código se debería compilar sin errores ahora.

    ![Compilación correcta](media/sdk/qs-csharp-dotnet-windows-08-build.png "Successful build")

1. Inicie la aplicación. En la barra de menús, seleccione **Depurar** > **Iniciar depuración** o bien presione **F5**.

    ![Inicio de depuración en la aplicación](media/sdk/qs-csharp-dotnet-windows-09-start-debugging.png "Start the app into debugging")

1. Aparece una ventana de consola que le pide decir algo (en inglés). El texto reconocido aparecerá en la misma ventana.

    ![Salida de la consola después de un reconocimiento correcto](media/sdk/qs-csharp-dotnet-windows-10-console-output.png "Console output after successful recognition")

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Busque este ejemplo en la carpeta `quickstart/csharp-dotnet-windows`.

## <a name="next-steps"></a>Pasos siguientes

- [Traducción de voz](how-to-translate-speech-csharp.md)
- [Personalización de modelos acústicos](how-to-customize-acoustic-models.md)
- [Personalización de modelos de lenguaje](how-to-customize-language-model.md)
