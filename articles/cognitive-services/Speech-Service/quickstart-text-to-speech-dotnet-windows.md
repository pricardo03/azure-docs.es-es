---
title: 'Inicio rápido: Síntesis de voz en .NET Framework para Windows (Servicios de voz)'
titleSuffix: Azure Cognitive Services
description: Use esta guía para crear una aplicación de consola de conversión de texto a voz mediante .NET Framework para Windows y el SDK de Voz. Cuando termine, puede sintetizar la voz a partir de texto y escuchar la voz en su altavoz en tiempo real.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 08/27/2019
ms.author: yinhew
ms.openlocfilehash: 1a411455e4a6dea22e092cdfc8e70ee23b656435
ms.sourcegitcommit: e9936171586b8d04b67457789ae7d530ec8deebe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71327451"
---
# <a name="quickstart-synthesize-speech-with-the-speech-sdk-for-net-framework-windows"></a>Inicio rápido: Síntesis la voz con el SDK de Voz para .NET Framework (Windows)

También hay guías de inicio rápido para el [reconocimiento](quickstart-csharp-dotnet-windows.md) y la [traducción de voz](quickstart-translate-speech-dotnetframework-windows.md).

Use esta guía para crear una aplicación de consola de conversión de texto a voz mediante .NET Framework para Windows y el SDK de Voz. Cuando termine, puede sintetizar la voz a partir de texto y escuchar la voz en su altavoz en tiempo real.

Para ver una demostración rápida (sin tener que compilar el proyecto de Visual Studio, como se ha descrito en este artículo), obtenga los [ejemplos del SDK de Voz de Cognitive Services](https://github.com/Azure-Samples/cognitive-services-speech-sdk) de GitHub.

## <a name="prerequisites"></a>Requisitos previos

Para completar este proyecto necesitará lo siguiente:

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/).
* Una clave de suscripción para el servicio Voz. [Obtenga una gratis](get-started.md).
* Un altavoz (o auriculares).

## <a name="create-a-visual-studio-project"></a>Creación de un proyecto de Visual Studio

[!INCLUDE [Create project](../../../includes/cognitive-services-speech-service-create-speech-project-vs-csharp.md)]

## <a name="add-sample-code"></a>Incorporación de código de ejemplo

1. Abra **Program.cs** y reemplace el código generado automáticamente por el de este ejemplo:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/csharp-dotnet-windows/helloworld/Program.cs#code)]

1. Busque la cadena `YourSubscriptionKey` y reemplácela por su clave de suscripción a Speech Services.

1. Busque la cadena `YourServiceRegion` y reemplácela por la [ región ](regions.md) asociada a su suscripción. Por ejemplo, si usa la suscripción de evaluación gratuita, la región es `westus`.

1. En la barra de menús, elija **Archivo** > **Guardar todo**.

## <a name="build-and-run-the-application"></a>Compilación y ejecución de la aplicación

1. En la barra de menús, elija **Compilar** > **Compilar solución** para compilar la aplicación. El código se debería compilar sin errores ahora.

1. Elija **Depurar** > **Iniciar depuración** (o seleccione **F5**) para iniciar la aplicación **helloworld**.

1. Escriba una oración o frase en inglés. La aplicación transmite el texto a Speech Services, que envía la voz sintetizada a la aplicación para que se reproduzca en el altavoz.

   ![Interfaz de usuario de síntesis de voz](media/sdk/qs-tts-csharp-dotnet-windows-console-output.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Exploración de ejemplos de C# en GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Otras referencias

- [Creación de una voz personalizada](how-to-custom-voice-create-voice.md)
- [Grabación de ejemplos de voz personalizada](record-custom-voice-samples.md)
