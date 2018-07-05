---
title: 'Guía de inicio rápido: Reconocimiento de voz mediante el SDK de C# de Cognitive Services Speech para Windows | Microsoft Docs'
description: Más información acerca de cómo reconocer la voz mediante el SDK de C# para Speech Service.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: wolfma
ms.openlocfilehash: 64281215d139731b61365936bc1b837798ad8fbf
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2018
ms.locfileid: "37016841"
---
# <a name="quickstart-recognize-speech-using-the-cognitive-services-speech-c-sdk"></a>Guía de inicio rápido: Reconocimiento de voz mediante el SDK de C# de Cognitive Services Speech

En este artículo, aprenderá a crear una aplicación de consola de C# en Windows mediante el SDK de Cognitive Services Speech para convertir voz en texto.

## <a name="prerequisites"></a>requisitos previos

* Clave de suscripción para Speech Service. Consulte [Pruebe Speech Service gratis](get-started.md).
* Visual Studio 2017, Community Edition o superior.
* Carga de trabajo de **Desarrollo de escritorio de .NET** en Visual Studio. Puede habilitarlo en **Herramientas** \> **Get Tools and Features** (Obtener herramientas y características). 

## <a name="create-a-visual-studio-project"></a>Creación de un proyecto de Visual Studio

1. En Visual Studio 2017, cree una nueva aplicación de consola en Visual C#. En el cuadro de diálogo **Nuevo proyecto**, en el panel izquierdo, expanda **Instalado** y, a continuación, seleccione **Console App (.NET Framework)**. Para el nombre del proyecto, escriba *CsharpHelloSpeech*.

    ![Creación de Console App (.NET Framework) de Visual C# (.NET Framework)](media/sdk/speechsdk-05-vs-cs-new-console-app.png "Create Visual C# Console App")

2. Instale y haga referencia al [paquete NuGet del SDK de Voz](https://aka.ms/csspeech/nuget). En el Explorador de soluciones, haga clic con el botón derecho en la solución y seleccione **Manage NuGet Packages for Solution** (Administrar paquetes de NuGet para la solución).

    ![Hacer clic con el botón derecho en Manage NuGet Packages for Solution (Administrar paquetes NuGet para la solución)](media/sdk/speechsdk-06-vs-cs-manage-nuget-packages.png "Manage NuGet Packages for Solution")

3. En la esquina superior derecha, en el campo **Origen del paquete**, elija **Nuget.org**. Busque e instale el paquete `Microsoft.CognitiveServices.Speech` e instálelo en el proyecto **CsharpHelloSpeech**.

    ![Instalación del paquete NuGet Microsoft.CognitiveServices.Speech](media/sdk/speechsdk-08-vs-cs-nuget-install.png "Install Nuget package")

4. En la pantalla de licencia que aparece, acepte la licencia:

    ![Aceptación de licencia](media/sdk/speechsdk-09-vs-cs-nuget-license.png "Accept the license")

## <a name="create-a-platform-configuration-matching-your-pc-architecture"></a>Creación de una configuración de plataforma que coincida con la arquitectura de PC

En esta sección, agregará una nueva plataforma a la configuración que coincida con la arquitectura del procesador.

1. Inicie el Administrador de configuración. Seleccione **Compilar** > **Administrador de configuración**.

    ![Inicio del Administrador de configuración](media/sdk/speechsdk-12-vs-cs-cfg-manager-click.png "Launch the configuration manager")

2. En el cuadro de diálogo **Administrador de configuración**, agregue una nueva plataforma. En la lista desplegable **Plataforma de soluciones activas**, seleccione **Nuevo**.

    ![Incorporación de una nueva plataforma en la ventana del Administrador de configuración](media/sdk/speechsdk-14-vs-cs-cfg-manager-new.png "Add a new platform under the configuration manager window")

3. Si está ejecutando Windows de 64 bits, cree una nueva configuración de plataforma denominada `x64`. Si está ejecutando Windows de 32 bits, cree una nueva configuración de plataforma denominada `x86`. En este artículo, se crea una configuración de plataforma `x64`. 

    ![En Windows de 64 bits, incorporación de una nueva plataforma denominada "x64"](media/sdk/speechsdk-15-vs-cs-cfg-manager-add-x64.png "Add x64 platform")

## <a name="add-the-sample-code"></a>Incorporación del código de ejemplo

1. En el archivo `Program.cs` para el proyecto de Visual Studio, reemplace el cuerpo de la clase `Program` por lo siguiente. Asegúrese de reemplazar la clave de suscripción y la región por una que haya obtenido para el servicio.

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/Windows/quickstart-csharp/Program.cs#code)]

2. Después de pegar el código, el método `Main()` debe ser similar al que se muestra en la captura de pantalla siguiente:

    ![Método principal después de pegar el código](media/sdk/speechsdk-17-vs-cs-paste-code.png "Final Main method")

3. IntelliSense de Visual Studio resalta las referencias a las clases del SDK de Voz que no se han podido resolver. Para corregir este error, agregue la siguiente instrucción `using` al principio del código (ya sea manualmente o mediante [acciones rápidas](https://docs.microsoft.com/visualstudio/ide/quick-actions) de Visual Studio).

    [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/Windows/quickstart-csharp/Program.cs#usingstatement)]

    ![Uso de la acción rápida para agregar lo que falta mediante la instrucción](media/sdk/speechsdk-18-vs-cs-add-using.png "Resolve IntelliSense issues")

4. Asegúrese de que se ha resuelto el resaltado de IntelliSense y guarde los cambios del proyecto.

## <a name="build-and-run-the-sample"></a>Compilación y ejecución del ejemplo

1. Compile la aplicación. En la barra de menús, seleccione **Compilar** > **Compilar solución**. El código se debería compilar sin errores ahora:

    ![Compilación correcta](media/sdk/speechsdk-20-vs-cs-build.png "Successful build")

2. Inicie la aplicación. En la barra de menús, seleccione **Depurar** > **Iniciar depuración** o bien presione **F5**. 

    ![Inicio de depuración en la aplicación](media/sdk/speechsdk-21-vs-cs-f5.png "Start the app into debugging")

3. Aparece una ventana emergente de consola que le pide que diga algo (en inglés).
El resultado del reconocimiento se muestra en la pantalla.

    ![Salida de la consola después de un reconocimiento correcto](media/sdk/speechsdk-22-cs-vs-console-output.png "Console output after successful recognition")

## <a name="download-code"></a>Descarga de código

Para obtener el conjunto más reciente de ejemplos, consulte el [repositorio de GitHub de ejemplos del SDK de Cognitive Services Speech](https://aka.ms/csspeech/samples).

## <a name="next-steps"></a>Pasos siguientes

- [Traducción de voz](how-to-translate-speech.md)
- [Personalizar los modelos de voz](how-to-customize-speech-models.md)
