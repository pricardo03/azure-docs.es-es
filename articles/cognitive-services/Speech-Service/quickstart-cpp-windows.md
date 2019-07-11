---
title: 'Inicio rápido: Reconocimiento de voz, C++ (Windows): servicios de voz'
titleSuffix: Azure Cognitive Services
description: Aprenda a reconocer la voz en C++ para Windows mediante el SDK de Voz.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: wolfma
ms.openlocfilehash: d63d1c96077642e660e2272cbd8c2ee1250b1471
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67606491"
---
# <a name="quickstart-recognize-speech-in-c-on-windows-by-using-the-speech-sdk"></a>Inicio rápido: Reconocimiento de voz en C++ en Windows mediante el SDK de Voz

También hay inicios rápidos disponibles para [texto a voz](quickstart-text-to-speech-cpp-windows.md) y [traducción de voz](quickstart-translate-speech-cpp-windows.md).

Si lo desea, elija otro lenguaje de programación diferente o entorno:<br/>
[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

En este artículo, creará una aplicación de consola de C++ para Windows. El [SDK de Voz](speech-sdk.md) de Cognitive Services se usa para transcribir la conversión de voz en texto en tiempo real desde el micrófono de un equipo. La aplicación se compila con el [paquete NuGet del SDK de Voz](https://aka.ms/csspeech/nuget) y Microsoft Visual Studio 2017 (cualquier edición).

## <a name="prerequisites"></a>Requisitos previos

Necesita una clave de suscripción de servicios de voz para completar este inicio rápido. Puede obtener una gratis. Para más detalles, consulte [Prueba gratuita de los servicios de voz](get-started.md).

## <a name="create-a-visual-studio-project"></a>Creación de un proyecto de Visual Studio

[!INCLUDE [Quickstart C++ project](../../../includes/cognitive-services-speech-service-quickstart-cpp-create-proj.md)]

## <a name="add-sample-code"></a>Incorporación de código de ejemplo

1. Abra el archivo de origen *helloworld.cpp*. Reemplace todo el código que aparece debajo de la instrucción include inicial (`#include "stdafx.h"` o `#include "pch.h"`) por lo siguiente:

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp-windows/helloworld/helloworld.cpp#code)]

1. En el mismo archivo, reemplace la cadena `YourSubscriptionKey` por la clave de suscripción.

1. Reemplace la cadena `YourServiceRegion` por la [región](regions.md) asociada a sus suscripción (por ejemplo, `westus` para la suscripción de evaluación gratuita).

1. Guarde los cambios en el proyecto.

## <a name="build-and-run-the-app"></a>Compilación y ejecución de la aplicación

1. Compile la aplicación. En la barra de menús, elija **Compilar** > **Compilar solución**. El código se debería compilar sin errores ahora.

   ![Captura de pantalla de la aplicación de Visual Studio, con la opción Compilar solución resaltada](media/sdk/qs-cpp-windows-06-build.png)

1. Inicie la aplicación. En la barra de menús, elija **Depurar** > **Iniciar depuración** o bien presione **F5**.

   ![Captura de pantalla de la aplicación de Visual Studio, con la opción Iniciar depuración resaltada](media/sdk/qs-cpp-windows-07-start-debugging.png)

1. Aparece una ventana de consola que le pide decir algo. Diga una oración o frase en inglés. Lo que diga se transmitirá a los servicios de voz y se transcribirá en texto, que aparece en la misma ventana.

   ![Captura de pantalla de la salida de la consola después de un reconocimiento correcto](media/sdk/qs-cpp-windows-08-console-output-release.png)

## <a name="next-steps"></a>Pasos siguientes

Se pueden encontrar ejemplos adicionales, por ejemplo, cómo leer voz de un archivo de audio, en GitHub.

> [!div class="nextstepaction"]
> [Exploración de ejemplos de C++ en GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Otras referencias

- [Personalización de modelos acústicos](how-to-customize-acoustic-models.md)
- [Personalización de modelos de lenguaje](how-to-customize-language-model.md)
