---
title: 'Inicio rápido: Síntesis de voz, C++ (Windows): servicios de Voz'
titleSuffix: Azure Cognitive Services
description: Aprenda a sintetizar la voz en C++ para el escritorio de Windows mediante el SDK de Voz.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 4/03/2019
ms.author: yinhew
ms.openlocfilehash: 732816ce10836d5828e7f325a3eb3fe31627d4f4
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59012376"
---
# <a name="quickstart-synthesize-speech-in-c-on-windows-by-using-the-speech-sdk"></a>Inicio rápido: Síntesis de voz en C++ en Windows mediante el SDK de Voz

En este artículo, creará una aplicación de consola de C++ para Windows. Usará el [SDK de Voz](speech-sdk.md) de Cognitive Services para sintetizar la voz a partir de texto en tiempo real y reproducirla en los altavoces de su PC. La aplicación se compila con el [paquete NuGet del SDK de Voz](https://aka.ms/csspeech/nuget) y Microsoft Visual Studio 2017 (cualquier edición).

La característica descrita en este artículo está disponible en el [SDK de Voz 1.4.0](https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech/1.4.0).

Para obtener una lista completa de los idiomas o voces disponibles para la síntesis de voz, consulte la [compatibilidad con idiomas](language-support.md#text-to-speech).

## <a name="prerequisites"></a>Requisitos previos

Necesita una clave de suscripción de servicios de voz para completar este inicio rápido. Puede obtener una gratis. Para más detalles, consulte [Prueba gratuita de los servicios de voz](get-started.md).

## <a name="create-a-visual-studio-project"></a>Creación de un proyecto de Visual Studio

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-cpp-create-proj.md)]

## <a name="add-sample-code"></a>Incorporación de código de ejemplo

1. Abra el archivo de origen *helloworld.cpp*. Reemplace todo el código que aparece debajo de la instrucción include inicial (`#include "stdafx.h"` o `#include "pch.h"`) por lo siguiente:

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/cpp-windows/helloworld/helloworld.cpp#code)]

1. En el mismo archivo, reemplace la cadena `YourSubscriptionKey` por la clave de suscripción.

1. Reemplace la cadena `YourServiceRegion` por la [región](regions.md) asociada a sus suscripción (por ejemplo, `westus` para la suscripción de evaluación gratuita).

1. Guarde los cambios en el proyecto.

## <a name="build-and-run-the-app"></a>Compilación y ejecución de la aplicación

1. Compile la aplicación. En la barra de menús, elija **Compilar** > **Compilar solución**. El código se debería compilar sin errores ahora.

   ![Captura de pantalla de la aplicación de Visual Studio, con la opción Compilar solución resaltada](media/sdk/qs-cpp-windows-06-build.png)

1. Inicie la aplicación. En la barra de menús, elija **Depurar** > **Iniciar depuración** o bien presione **F5**.

   ![Captura de pantalla de la aplicación de Visual Studio, con la opción Iniciar depuración resaltada](media/sdk/qs-cpp-windows-07-start-debugging.png)

1. Aparece una ventana de consola que le pide que escriba texto. Escriba algunas palabras o una frase. El texto que escriba se transmite a los servicios de Voz y se sintetiza en voz, la cual se reproduce en el altavoz.

   ![Captura de pantalla de la salida de consola después de una síntesis correcta](media/sdk/qs-tts-cpp-windows-console-output.png)

## <a name="next-steps"></a>Pasos siguientes

Se pueden encontrar ejemplos adicionales, por ejemplo, cómo guardar voz en un archivo de audio, en GitHub.

> [!div class="nextstepaction"]
> [Exploración de ejemplos de C++ en GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Otras referencias

- [Personalizar las fuentes de voz](how-to-customize-voice-font.md)
- [Grabación de ejemplos de voz](record-custom-voice-samples.md)
