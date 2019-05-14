---
title: 'Inicio rápido: Síntesis de voz, .NET Framework (Windows): servicios de Voz'
titleSuffix: Azure Cognitive Services
description: Use esta guía para crear una aplicación de consola de conversión de texto a voz mediante .NET Framework para Windows y el SDK de Voz. Cuando termine, puede sintetizar la voz a partir de texto y escuchar la voz en su altavoz en tiempo real.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 4/03/2019
ms.author: yinhew
ms.openlocfilehash: 3b7addd369d5e26554dd4300e65bee66b8a62768
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2019
ms.locfileid: "65465575"
---
# <a name="quickstart-synthesize-speech-with-the-speech-sdk-for-net-framework-windows"></a>Inicio rápido: Síntesis la voz con el SDK de Voz para .NET Framework (Windows)

También hay inicios rápidos disponibles para el [reconocimiento de voz](quickstart-csharp-dotnet-windows.md) y la [traducción de voz](quickstart-translate-speech-dotnetframework-windows.md).

Use esta guía para crear una aplicación de consola de conversión de texto a voz mediante .NET Framework para Windows y el SDK de Voz. Cuando termine, puede sintetizar la voz a partir de texto y escuchar la voz en su altavoz en tiempo real.

Para ver una demostración rápida (sin compilar el proyecto de Visual Studio personalmente como se muestra a continuación):

Obtenga los [ejemplos más recientes del SDK de Voz de Cognitive Services](https://github.com/Azure-Samples/cognitive-services-speech-sdk) de GitHub.

## <a name="prerequisites"></a>Requisitos previos

Para completar este proyecto necesitará lo siguiente:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Una clave de suscripción para el servicio Voz. [Obtenga una gratis](get-started.md).
* Un altavoz (o auriculares).

## <a name="create-a-visual-studio-project"></a>Creación de un proyecto de Visual Studio

[!INCLUDE [Create project](../../../includes/cognitive-services-speech-service-create-speech-project-vs-csharp.md)]

## <a name="add-sample-code"></a>Incorporación de código de ejemplo

1. Abra `Program.cs` y reemplace el código generado automáticamente por el de este ejemplo:

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/csharp-dotnet-windows/helloworld/Program.cs#code)]

1. Localice la cadena `YourSubscriptionKey` y reemplácela por su clave de suscripción a los servicios de voz.

1. Localice la cadena `YourServiceRegion` y reemplácela por la [región](regions.md) asociada a sus suscripción. Por ejemplo, si usa la evaluación gratuita, la región es `westus`.

1. Guarde los cambios realizados en el proyecto.

## <a name="build-and-run-the-app"></a>Compilación y ejecución de la aplicación

1. En la barra de menús, seleccione **Compilar** > **Compilar solución**. El código se debería compilar sin errores ahora.

    ![Captura de pantalla de la aplicación de Visual Studio, con la opción Generar solución resaltada](media/sdk/qs-csharp-dotnet-windows-08-build.png "Compilación correcta")

1. En la barra de menús, seleccione **Depurar** > **Iniciar depuración** o presione **F5** para iniciar la depuración.

    ![Captura de pantalla de la aplicación de Visual Studio, con la opción Iniciar depuración resaltada](media/sdk/qs-csharp-dotnet-windows-09-start-debugging.png "Iniciar la aplicación en depuración")

1. Aparece una ventana de consola que le pide que escriba texto. Escriba algunas palabras o una frase. El texto que escriba se transmite a los servicios de Voz y se sintetiza en voz, la cual se reproduce en el altavoz.

    ![Captura de pantalla de la salida de la consola después de un reconocimiento correcto](media/sdk/qs-tts-csharp-dotnet-windows-console-output.png "Salida de la consola después de un reconocimiento correcto")

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Exploración de ejemplos de C# en GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Otras referencias

- [Personalizar las fuentes de voz](how-to-customize-voice-font.md)
- [Grabación de ejemplos de voz](record-custom-voice-samples.md)
