---
title: 'Guía de inicio rápido: Reconocimiento de voz en C# en .NET Core en Windows mediante el SDK de Voz de Cognitive Services | Microsoft Docs'
titleSuffix: Microsoft Cognitive Services
description: Obtenga información sobre cómo reconocer la voz en C# en .NET Core en Windows mediante el SDK de Voz de Cognitive Services
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/16/2018
ms.author: wolfma
ms.openlocfilehash: 50cf1bbbe529b30da6bfe39281d11eee1c788dd8
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2018
ms.locfileid: "39259167"
---
# <a name="quickstart-recognize-speech-in-c-under-net-core-on-windows-using-the-speech-sdk"></a>Guía de inicio rápido: Reconocimiento de voz en C# en .NET Core en Windows mediante el SDK de Voz

En este artículo, obtendrá información sobre cómo crear una aplicación de la consola de C# para .NET Core en Windows mediante el SDK de Voz de Cognitive Services para transcribir la conversión de voz en texto.
La aplicación se compila con el [paquete NuGet del SDK de Voz de Microsoft Cognitive Services](https://aka.ms/csspeech/nuget) y Microsoft Visual Studio 2017.

> [!NOTE]
> .NET Core es una plataforma de .NET multiplataforma de código abierto que implementa la especificación [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard).

## <a name="prerequisites"></a>Requisitos previos

* Clave de suscripción para Speech Service. Consulte [Pruebe Speech Service gratis](get-started.md).
* Un PC Windows con micrófono operativo.
* [Microsoft Visual Studio 2017](https://www.visualstudio.com/), Community Edition o superior.
* La carga de trabajo de **Desarrollo multiplataforma de .NET Core** en Visual Studio. Puede habilitarlo en **Herramientas** \> **Get Tools and Features** (Obtener herramientas y características).

  ![Habilitación del desarrollo multiplataforma de .NET Core](media/sdk/vs-enable-net-core-workload.png)

## <a name="create-a-visual-studio-project"></a>Creación de un proyecto de Visual Studio

1. En Visual Studio 2017, cree una nueva aplicación de consola de .NET Core en Visual C#. En el cuadro de diálogo **New Project** (Proyecto nuevo), en el panel de la izquierda, expanda **Installed** (Instalado) \> **Visual C#** \> **.NET Core** y seleccione **Console App (.NET Core)** (Aplicación de consola [.NET Core]). Para el nombre del proyecto, escriba *helloworld*.

    ![Creación de aplicación de consola de Visual C# (.NET Core)](media/sdk/qs-csharp-dotnetcore-windows-01-new-console-app.png "Creación de aplicación de consola de Visual C# (.NET Core)")

1. Instale y haga referencia al [paquete NuGet del SDK de Voz](https://aka.ms/csspeech/nuget). En el Explorador de soluciones, haga clic con el botón derecho en la solución y seleccione **Manage NuGet Packages for Solution** (Administrar paquetes de NuGet para la solución).

    ![Hacer clic con el botón derecho en Manage NuGet Packages for Solution (Administrar paquetes NuGet para la solución)](media/sdk/qs-csharp-dotnetcore-windows-02-manage-nuget-packages.png "Manage NuGet Packages for Solution")

1. En la esquina superior derecha, en el campo **Origen del paquete**, elija **Nuget.org**. Busque e instale el paquete `Microsoft.CognitiveServices.Speech` e instálelo en el proyecto **helloworld**.

    ![Instalación del paquete NuGet Microsoft.CognitiveServices.Speech](media/sdk/qs-csharp-dotnetcore-windows-03-nuget-install-0.5.0.png "Install Nuget package")

1. Acepte la licencia en el cuadro de diálogo que aparece.

    ![Aceptación de licencia](media/sdk/qs-csharp-dotnetcore-windows-04-nuget-license.png "Accept the license")

## <a name="add-the-sample-code"></a>Incorporación del código de ejemplo

1. Abra `Program.cs` en el proyecto de Visual Studio y reemplace todo el código de este archivo por lo siguiente.

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-dotnetcore-windows/helloworld/Program.cs#code)]

1. Reemplace la cadena `YourSubscriptionKey` por la clave de suscripción.

1. Reemplace la cadena `YourServiceRegion` por la [región](regions.md) asociada con la suscripción (por ejemplo, `westus` para la suscripción de evaluación gratuita).

1. Guarde los cambios en el proyecto.

## <a name="build-and-run-the-sample"></a>Compilación y ejecución del ejemplo

1. Compile la aplicación. En la barra de menús, elija **Compilar** > **Compilar solución**. El código se debería compilar sin errores ahora.

    ![Compilación correcta](media/sdk/qs-csharp-dotnetcore-windows-05-build.png "Successful build")

1. Inicie la aplicación. En la barra de menús, elija **Depurar** > **Iniciar depuración** o bien presione **F5**.

    ![Inicio de depuración en la aplicación](media/sdk/qs-csharp-dotnetcore-windows-06-start-debugging.png "Start the app into debugging")

1. Aparece una ventana de consola que le pide decir algo (en inglés). El texto reconocido aparecerá en la misma ventana.

    ![Salida de la consola después de un reconocimiento correcto](media/sdk/qs-csharp-dotnetcore-windows-07-console-output.png "Console output after successful recognition")

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Busque este ejemplo en la carpeta `quickstart/csharp-dotnetcore-windows`.

## <a name="next-steps"></a>Pasos siguientes

- [Traducción de voz](how-to-translate-speech.md)
- [Personalización de modelos acústicos](how-to-customize-acoustic-models.md)
- [Personalización de modelos de lenguaje](how-to-customize-language-model.md)
