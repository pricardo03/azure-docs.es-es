---
title: 'Inicio rápido: Síntesis de voz, C++ (Windows): Servicios de voz'
titleSuffix: Azure Cognitive Services
description: Aprenda a sintetizar la voz en C++ para el escritorio de Windows mediante el SDK de Voz.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 08/24/2019
ms.author: yinhew
ms.openlocfilehash: 686b21d3e02266af77687778c32f0d1ca6d55154
ms.sourcegitcommit: 49c4b9c797c09c92632d7cedfec0ac1cf783631b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2019
ms.locfileid: "70383065"
---
# <a name="quickstart-synthesize-speech-in-c-on-windows-by-using-the-speech-sdk"></a>Inicio rápido: Síntesis de voz en C++ en Windows mediante el SDK de Voz

También hay guías de inicio rápido para el [reconocimiento](quickstart-cpp-windows.md) y la [traducción de voz](quickstart-translate-speech-cpp-windows.md).

En este artículo, creará una aplicación de consola de C++ para Windows. Usará el [SDK de Voz](speech-sdk.md) de Cognitive Services para sintetizar la voz a partir de texto en tiempo real y reproducirla en los altavoces de su PC. La aplicación se compila con el [paquete NuGet del SDK de voz](https://aka.ms/csspeech/nuget) y Microsoft Visual Studio 2019 (cualquier edición).

Para obtener una lista completa de los idiomas o voces disponibles para la síntesis de voz, consulte la [compatibilidad con idiomas](language-support.md#text-to-speech).

## <a name="prerequisites"></a>Requisitos previos

Necesita una clave de suscripción de servicios de voz para completar este inicio rápido. Puede obtener una gratis. Para más detalles, consulte [Prueba gratuita de los servicios de voz](get-started.md).

## <a name="create-a-visual-studio-project"></a>Creación de un proyecto de Visual Studio

[!INCLUDE [Quickstart C++ project](../../../includes/cognitive-services-speech-service-quickstart-cpp-create-proj.md)]

## <a name="add-sample-code"></a>Incorporación de código de ejemplo

1. Abra el archivo de origen **helloworld.cpp**.

1. Reemplace todo el código por el fragmento siguiente:

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/cpp-windows/helloworld/helloworld.cpp#code)]

1. En el mismo archivo, reemplace la cadena `YourSubscriptionKey` por la clave de suscripción.

1. Reemplace la cadena `YourServiceRegion` por la [región](regions.md) asociada a sus suscripción (por ejemplo, `westus` para la suscripción de evaluación gratuita).

1. En la barra de menús, elija **Archivo** > **Guardar todo**.

## <a name="build-and-run-the-application"></a>Compilación y ejecución de la aplicación

1. En la barra de menús, seleccione **Compilar** > **Compilar solución** para compilar la aplicación. El código se debería compilar sin errores ahora.

1. Elija **Depurar** > **Iniciar depuración** o presione **F5** para iniciar la aplicación **HelloWorld**.

1. Diga una oración o frase en inglés. La aplicación transmite el texto a Speech Services, que envía la voz sintetizada a la aplicación para que se reproduzca en el altavoz.

   ![Salida de la consola después de una síntesis correcta](media/sdk/qs-tts-cpp-windows-console-output.png)

## <a name="next-steps"></a>Pasos siguientes

Se pueden encontrar ejemplos adicionales, por ejemplo, cómo guardar voz en un archivo de audio, en GitHub.

> [!div class="nextstepaction"]
> [Exploración de ejemplos de C++ en GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Otras referencias

- [Creación de una voz personalizada](how-to-custom-voice-create-voice.md)
- [Grabación de ejemplos de voz personalizada](record-custom-voice-samples.md)
