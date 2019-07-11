---
title: 'Inicio rápido: Reconocimiento de voz, C# (.NET Core): Servicios de Voz'
titleSuffix: Azure Cognitive Services
description: Aprenda a sintetizar la voz en C# con .NET Core para Windows mediante el SDK de Voz.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 6/24/2019
ms.author: yinhew
ms.openlocfilehash: 7b4a018e38ca625e38dc1658a95d3ce0e677f711
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67467197"
---
# <a name="quickstart-synthesize-speech-with-the-speech-sdk-for-net-core"></a>Inicio rápido: Síntesis de voz con el SDK de Voz para .NET Core

También hay inicios rápidos disponibles para el [reconocimiento de voz](quickstart-csharp-dotnetcore-windows.md) y la [traducción de voz](quickstart-translate-speech-dotnetcore-windows.md).

En este artículo, creará una aplicación de consola de C# para .NET Core en Windows mediante el [SDK de Voz](speech-sdk.md) de Cognitive Services. Puede sintetizar la voz a partir de texto en tiempo real en el altavoz del equipo. La aplicación se compila con el [paquete NuGet del SDK de Voz](https://aka.ms/csspeech/nuget) y Microsoft Visual Studio 2017 (cualquier edición).

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

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/csharp-dotnetcore/helloworld/Program.cs#code)]

1. En el mismo archivo, reemplace la cadena `YourSubscriptionKey` por la clave de suscripción.

1. Además, reemplace la cadena `YourServiceRegion` por la [región](regions.md) asociada a su suscripción (por ejemplo, `westus` para la suscripción de evaluación gratuita).

1. Guarde los cambios en el proyecto.

## <a name="build-and-run-the-app"></a>Compilación y ejecución de la aplicación

1. Compile la aplicación. En la barra de menús, elija **Compilar** > **Compilar solución**. El código se debería compilar sin errores ahora.

    ![Captura de pantalla de la aplicación de Visual Studio, con la opción Generar solución resaltada](media/sdk/qs-csharp-dotnetcore-windows-05-build.png "Compilación correcta")

1. Inicie la aplicación. En la barra de menús, elija **Depurar** > **Iniciar depuración** o bien presione **F5**.

    ![Captura de pantalla de la aplicación de Visual Studio, con la opción Iniciar depuración resaltada](media/sdk/qs-csharp-dotnetcore-windows-06-start-debugging.png "Iniciar la aplicación en depuración")

1. Aparece una ventana de consola que le pide que escriba texto. Escriba algunas palabras o una frase. El texto que escriba se transmite a los servicios de Voz y se sintetiza en voz, la cual se reproduce en el altavoz.

    ![Captura de pantalla de la salida de la consola después de una síntesis correcta](media/sdk/qs-tts-csharp-dotnet-windows-console-output.png "Salida de la consola después de una síntesis correcta")

## <a name="next-steps"></a>Pasos siguientes

Se pueden encontrar ejemplos adicionales, por ejemplo, cómo sintetizar voz en un archivo de audio, en GitHub.

> [!div class="nextstepaction"]
> [Exploración de ejemplos de C# en GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Otras referencias

- [Personalizar las fuentes de voz](how-to-customize-voice-font.md)
- [Grabación de ejemplos de voz](record-custom-voice-samples.md)
