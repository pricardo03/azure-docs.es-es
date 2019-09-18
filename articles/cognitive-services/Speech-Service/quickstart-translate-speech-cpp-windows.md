---
title: 'Inicio rápido: Traducción de voz, C++ (Windows): Servicios de voz'
titleSuffix: Azure Cognitive Services
description: En este inicio rápido, creará una sencilla aplicación de C++ para capturar la voz del usuario, traducirla a otro idioma y mostrar el texto en la línea de comandos. Esta guía está diseñada para usuarios de Windows.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 08/24/2019
ms.author: erhopf
ms.openlocfilehash: 6ba4e44efc7ff24aa48f9f16840b2248423f7241
ms.sourcegitcommit: 49c4b9c797c09c92632d7cedfec0ac1cf783631b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2019
ms.locfileid: "70382674"
---
# <a name="quickstart-translate-speech-in-c-on-windows-by-using-the-speech-sdk"></a>Inicio rápido: Traducción de voz en C++ en Windows mediante el SDK de voz

También hay guías de inicio rápido para el [reconocimiento](quickstart-cpp-windows.md) y la [síntesis de voz](quickstart-text-to-speech-cpp-windows.md).

En este inicio rápido, creará una aplicación de C++ que captura la voz del usuario procedente del micrófono del equipo, traduce el contenido de voz y transcribe el texto traducido en la línea de comandos en tiempo real. La aplicación se compila con el [paquete NuGet del SDK de voz](https://aka.ms/csspeech/nuget) y Microsoft Visual Studio 2019 (cualquier edición).

Para obtener una lista completa de los idiomas disponibles para la traducción de voz, consulte la [compatibilidad con idiomas](language-support.md).

## <a name="prerequisites"></a>Requisitos previos

Necesita una clave de suscripción de servicios de voz para completar este inicio rápido. Puede obtener una gratis. Para más detalles, consulte [Prueba gratuita de los servicios de voz](get-started.md).

## <a name="create-a-visual-studio-project"></a>Creación de un proyecto de Visual Studio

[!INCLUDE [Quickstart C++ project](../../../includes/cognitive-services-speech-service-quickstart-cpp-create-proj.md)]

## <a name="add-sample-code"></a>Incorporación de código de ejemplo

1. Abra el archivo de origen **helloworld.cpp**.

1. Reemplace todo el código por el fragmento siguiente:

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/speech-translation/cpp-windows/helloworld/helloworld.cpp?range=2-#code)]

1. En el mismo archivo, reemplace la cadena `YourSubscriptionKey` por la clave de suscripción.

1. Reemplace la cadena `YourServiceRegion` por la [región](regions.md) asociada a sus suscripción (por ejemplo, `westus` para la suscripción de evaluación gratuita).

1. En la barra de menús, elija **Archivo** > **Guardar todo**.

## <a name="build-and-run-the-application"></a>Compilación y ejecución de la aplicación

1. En la barra de menús, seleccione **Compilar** > **Compilar solución** para compilar la aplicación. El código se debería compilar sin errores ahora.

1. Elija **Depurar** > **Iniciar depuración** o presione **F5** para iniciar la aplicación **HelloWorld**.

1. Diga una oración o frase en inglés. La aplicación transmite su voz a Speech Services, que la traduce y la transcribe a texto, en este caso, a francés y alemán. Después, Speech Services envían el texto de nuevo a la aplicación para que se muestren.

   ![Salida de la consola después de una traducción de voz correcta](media/sdk/qs-translate-cpp-windows-output.png)

## <a name="next-steps"></a>Pasos siguientes

En GitHub se pueden encontrar ejemplos adicionales, para, por ejemplo, leer voz desde un archivo de audio y convertir el texto traducido en voz sintetizada.

> [!div class="nextstepaction"]
> [Exploración de ejemplos de C++ en GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Otras referencias

- [Entrenamiento de un modelo de Custom Speech](how-to-custom-speech-train-model.md)
