---
title: 'Inicio rápido: Traducción de voz en C# para .NET Framework y Windows (Servicios de voz)'
titleSuffix: Azure Cognitive Services
description: En este inicio rápido, creará una aplicación de .NET Framework para capturar la voz del usuario, traducirla a otro idioma y mostrar el texto en la línea de comandos. Esta guía está diseñada para usuarios de Windows.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 08/28/2019
ms.author: erhopf
ms.openlocfilehash: bb710a3e3adb13aa3999c13043c8bb93f6b885f1
ms.sourcegitcommit: e9936171586b8d04b67457789ae7d530ec8deebe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71327340"
---
# <a name="quickstart-translate-speech-with-the-speech-sdk-for-net-framework-windows"></a>Inicio rápido: Traducción de voz con el SDK de Voz para .NET Framework (Windows)

También hay guías de inicio rápido para el [reconocimiento](quickstart-csharp-dotnet-windows.md) y la [síntesis de voz](quickstart-text-to-speech-dotnet-windows.md).

En este inicio rápido, creará una aplicación de .NET Framework que captura la voz del usuario procedente del micrófono del equipo, la traduce y transcribe el texto traducido en la línea de comandos en tiempo real. La aplicación se puede ejecutar en Windows de 32 bits o de 64 bits, y se ha creado con el [paquete NuGet del SDK de Voz](https://aka.ms/csspeech/nuget) y Microsoft Visual Studio 2019.

Para obtener una lista completa de los idiomas disponibles para la traducción de voz, consulte la [compatibilidad con idiomas](language-support.md).

## <a name="prerequisites"></a>Requisitos previos

Esta guía de inicio rápido requiere:

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/).
* Una clave de suscripción de Azure para el servicio Voz. [Obtenga una gratis](get-started.md).

## <a name="create-a-visual-studio-project"></a>Creación de un proyecto de Visual Studio

[!INCLUDE [Create project](../../../includes/cognitive-services-speech-service-create-speech-project-vs-csharp.md)]

## <a name="add-sample-code"></a>Incorporación de código de ejemplo

1. Abra el archivo **Program.cs** y sustituya todo el código existente por el siguiente.

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/speech-translation/csharp-dotnet-windows/helloworld/Program.cs#code)]

1. Busque la cadena `YourSubscriptionKey` y reemplácela por su clave de suscripción.

1. Busque la cadena `YourServiceRegion` y reemplácela por la [ región ](regions.md) asociada a su suscripción. Por ejemplo, si usa la suscripción de evaluación gratuita, la región es `westus`.

1. En la barra de menús, elija **Archivo** > **Guardar todo**.

## <a name="build-and-run-the-application"></a>Compilación y ejecución de la aplicación

1. En la barra de menús, elija **Compilar** > **Compilar solución** para compilar la aplicación. El código se debería compilar sin errores ahora.

1. Elija **Depurar** > **Iniciar depuración** (o seleccione **F5**) para iniciar la aplicación **helloworld**.

1. Diga una expresión o frase en inglés en el micrófono del dispositivo. La aplicación transmite su voz al servicio de voz, que la traduce y la transcribe a texto en otro idioma, en este caso, a alemán. El servicio de voz envía el texto traducido de nuevo a la aplicación, que muestra la traducción en la ventana.

   ![Interfaz de usuario de la traducción de voz](media/sdk/qs-translate-csharp-dotnetcore-windows-output.png)

## <a name="next-steps"></a>Pasos siguientes

Se pueden encontrar ejemplos adicionales como, por ejemplo, leer voz desde un archivo de audio y proporcionar una salida del texto traducido como voz sintetizada, en GitHub.

> [!div class="nextstepaction"]
> [Exploración de ejemplos de C# en GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Otras referencias

- [Entrenamiento de un modelo de Custom Speech](how-to-custom-speech-train-model.md)
