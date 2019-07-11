---
title: 'Inicio rápido: Reconocimiento de voz, C# (.NET Core): servicios de Voz'
titleSuffix: Azure Cognitive Services
description: Aprenda a reconocer la voz en C# con .NET Core para Windows o macOS mediante el SDK de Voz
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: wolfma
ms.openlocfilehash: 9c445ea685518cec77089ce644a79305ff7f3e9e
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67606582"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-net-core"></a>Inicio rápido: Reconocimiento de voz con el SDK de Voz para .NET Core

También hay inicios rápidos disponibles para [texto a voz](quickstart-text-to-speech-dotnetcore.md) y [traducción de voz](quickstart-translate-speech-dotnetcore-windows.md).

Si lo desea, elija otro lenguaje de programación diferente o entorno:<br/>
[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

En este artículo, creará una aplicación de consola de C# para .NET Core en Windows o macOS mediante el [SDK de Voz](speech-sdk.md) de Cognitive Services. Transcribirá la conversión de voz en texto en tiempo real desde el micrófono de su PC. La aplicación se compila con el [paquete NuGet del SDK de Voz](https://aka.ms/csspeech/nuget) y Microsoft Visual Studio 2017 (cualquier edición).

> [!NOTE]
> .NET Core es una plataforma de .NET multiplataforma de código abierto que implementa la especificación [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard).

Necesita una clave de suscripción de servicios de voz para completar este Inicio rápido. Puede obtener una gratis. Para más detalles, consulte [Prueba gratuita de los servicios de voz](get-started.md).

## <a name="prerequisites"></a>Requisitos previos

Esta guía de inicio rápido requiere:

* [SDK de .NET Core](https://dotnet.microsoft.com/download)
* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Una clave de suscripción de Azure para el servicio Voz. [Obtenga una gratis](get-started.md).

## <a name="create-a-visual-studio-project"></a>Creación de un proyecto de Visual Studio

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-dotnetcore-create-proj.md)]

## <a name="add-sample-code"></a>Incorporación de código de ejemplo

1. Abra `Program.cs` y reemplace todo el código que contiene por lo siguiente.

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-dotnetcore/helloworld/Program.cs#code)]

1. En el mismo archivo, reemplace la cadena `YourSubscriptionKey` por la clave de suscripción.

1. Además, reemplace la cadena `YourServiceRegion` por la [región](regions.md) asociada a su suscripción (por ejemplo, `westus` para la suscripción de evaluación gratuita).

1. Guarde los cambios en el proyecto.

## <a name="build-and-run-the-app"></a>Compilación y ejecución de la aplicación

1. Compile la aplicación. En la barra de menús, elija **Compilar** > **Compilar solución**. El código se debería compilar sin errores ahora.

    ![Captura de pantalla de la aplicación de Visual Studio, con la opción Generar solución resaltada](media/sdk/qs-csharp-dotnetcore-windows-05-build.png "Compilación correcta")

1. Inicie la aplicación. En la barra de menús, elija **Depurar** > **Iniciar depuración** o bien presione **F5**.

    ![Captura de pantalla de la aplicación de Visual Studio, con la opción Iniciar depuración resaltada](media/sdk/qs-csharp-dotnetcore-windows-06-start-debugging.png "Iniciar la aplicación en depuración")

1. Aparece una ventana de consola que le pide decir algo. Diga una oración o frase en inglés. Lo que diga se transmitirá a los servicios de voz y se transcribirá en texto, que aparece en la misma ventana.

    ![Captura de pantalla de la salida de la consola después de un reconocimiento correcto](media/sdk/qs-csharp-dotnetcore-windows-07-console-output.png "Salida de la consola después de un reconocimiento correcto")

## <a name="next-steps"></a>Pasos siguientes

Se pueden encontrar ejemplos adicionales, por ejemplo, cómo leer voz desde un archivo de audio, en GitHub.

> [!div class="nextstepaction"]
> [Exploración de ejemplos de C# en GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Otras referencias

- [Personalización de modelos acústicos](how-to-customize-acoustic-models.md)
- [Personalización de modelos de lenguaje](how-to-customize-language-model.md)
