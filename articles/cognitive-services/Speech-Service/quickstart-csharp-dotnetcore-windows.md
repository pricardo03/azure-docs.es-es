---
title: 'Inicio rápido: Reconocimiento de voz en C# en .NET Core en Windows mediante el SDK de Voz de Cognitive Services'
titleSuffix: Microsoft Cognitive Services
description: Obtenga información sobre cómo reconocer la voz en C# en .NET Core en Windows mediante el SDK de Voz de Cognitive Services
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 09/24/2018
ms.author: wolfma
ms.openlocfilehash: 2cb89606986645d567136655d5ab3f07223ba70d
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2018
ms.locfileid: "47434779"
---
# <a name="quickstart-recognize-speech-in-c-under-net-core-on-windows-by-using-the-speech-sdk"></a>Inicio rápido: Reconocimiento de voz en C# en .NET Core en Windows mediante el SDK de Voz

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

En este artículo, creará una aplicación de consola de C# para .NET Core en Windows mediante el [SDK de Voz](speech-sdk.md) de Cognitive Services. Transcribe de voz a texto en tiempo real desde el micrófono de su PC. La aplicación se compila con el [paquete NuGet del SDK de Voz](https://aka.ms/csspeech/nuget) y Microsoft Visual Studio 2017 (cualquier edición).

> [!NOTE]
> .NET Core es una plataforma de .NET multiplataforma de código abierto que implementa la especificación [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard).

Necesita una clave de suscripción del servicio de Voz para completar este inicio rápido. Puede obtener una gratis. Para ver más detalles, consulte [Pruebe Speech Service gratis](get-started.md).


## <a name="create-a-visual-studio-project"></a>Creación de un proyecto de Visual Studio

1. Inicie Visual Studio 2017.

1. Asegúrese de que la carga de trabajo de **Desarrollo multiplataforma de .NET Core** esté disponible. Elija **Herramientas** > **Get Tools and Features (Obtener herramientas y características)** desde la barra de menús de Visual Studio para abrir el instalador de Visual Studio. Si esta carga de trabajo ya está habilitada, cierre el cuadro de diálogo.

    ![Captura de pantalla del instalador de Visual Studio, con la pestaña Workloads (Cargas de trabajo) resaltada](media/sdk/vs-enable-net-core-workload.png)

    En caso contrario, active la casilla junto a **Desarrollo multiplataforma de .NET Core** y elija **Modificar** en la esquina inferior derecha del cuadro de diálogo. La instalación de la nueva característica tardará un momento.

1. Cree una nueva aplicación de consola de .NET Core en Visual C#. En el cuadro de diálogo **Nuevo proyecto**, en el panel izquierdo, expanda **Instalado** > **Visual C#** > **.NET Core**. Luego, seleccione **Aplicación de consola (.NET Framework)**. Para el nombre del proyecto, escriba *helloworld*.

    ![Captura de pantalla del cuadro de diálogo Nuevo proyecto](media/sdk/qs-csharp-dotnetcore-windows-01-new-console-app.png "Crear aplicación de consola de C# de Visual Studio (.NET Core)")

1. Instale y haga referencia al [paquete NuGet del SDK de Voz](https://aka.ms/csspeech/nuget). En el Explorador de soluciones, haga clic con el botón derecho en la solución y seleccione **Manage NuGet Packages for Solution** (Administrar paquetes de NuGet para la solución).

    ![Captura de pantalla del Explorador de soluciones, con la opción Administrar paquetes NuGet para la solución resaltada](media/sdk/qs-csharp-dotnetcore-windows-02-manage-nuget-packages.png "Administrar paquetes NuGet para la solución resaltada")

1. En la esquina superior derecha, en el campo **Origen del paquete**, elija **Nuget.org**. Busque el paquete `Microsoft.CognitiveServices.Speech` e instálelo en el proyecto **helloworld**.

    ![Captura de pantalla del cuadro de diálogo de administrar paquetes para la solución](media/sdk/qs-csharp-dotnetcore-windows-03-nuget-install-1.0.0.png "Instalación de paquete de NuGet")

1. Acepte la licencia que aparece para comenzar la instalación del paquete de NuGet.

    ![Captura de pantalla de cuadro de diálogo de aceptación de licencia](media/sdk/qs-csharp-dotnetcore-windows-04-nuget-license.png "Acepte la licencia")

Después de instalar el paquete, aparecerá una confirmación en la consola del Administrador de paquetes.


## <a name="add-sample-code"></a>Adición de un código de ejemplo

1. Abra `Program.cs` y reemplace todo el código que contiene por lo siguiente.

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-dotnetcore-windows/helloworld/Program.cs#code)]

1. En el mismo archivo, reemplace la cadena `YourSubscriptionKey` por la clave de suscripción.

1. Además, reemplace la cadena `YourServiceRegion` por la [región](regions.md) asociada a sus suscripción (por ejemplo, `westus` para la suscripción de evaluación gratuita).

1. Guarde los cambios en el proyecto.

## <a name="build-and-run-the-app"></a>Compilación y ejecución de la aplicación

1. Compile la aplicación. En la barra de menús, elija **Compilar** > **Compilar solución**. El código se debería compilar sin errores ahora.

    ![Captura de pantalla de la aplicación de Visual Studio, con la opción Generar solución resaltada](media/sdk/qs-csharp-dotnetcore-windows-05-build.png "Compilación correcta")

1. Inicie la aplicación. En la barra de menús, elija **Depurar** > **Iniciar depuración** o bien presione **F5**.

    ![Captura de pantalla de la aplicación de Visual Studio, con la opción Iniciar depuración resaltada](media/sdk/qs-csharp-dotnetcore-windows-06-start-debugging.png "Iniciar la aplicación en depuración")

1. Aparece una ventana de consola que le pide decir algo. Diga una oración o frase en inglés. Lo que diga se transmitirá al servicio de Voz y se transcribe en texto, que aparece en la misma ventana.

    ![Captura de pantalla de la salida de la consola después de un reconocimiento correcto](media/sdk/qs-csharp-dotnetcore-windows-07-console-output.png "Salida de la consola después de un reconocimiento correcto")

[!INCLUDE [Download this sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Busque este ejemplo en la carpeta `quickstart/csharp-dotnetcore-windows`.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Reconocimiento de intenciones a partir de contenido de voz con el SDK de Voz para C#](how-to-recognize-intents-from-speech-csharp.md)

## <a name="see-also"></a>Otras referencias

- [Traducción de voz](how-to-translate-speech-csharp.md)
- [Personalización de modelos acústicos](how-to-customize-acoustic-models.md)
- [Personalización de modelos de lenguaje](how-to-customize-language-model.md)
