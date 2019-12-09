---
title: 'Inicio rápido: Reconocimiento de voz en C# (.NET Core): servicio de voz'
titleSuffix: Azure Cognitive Services
description: Aprenda a sintetizar la voz en C# con .NET Core para Windows mediante el SDK de Voz.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 6/24/2019
ms.author: yinhew
ms.openlocfilehash: 6e732237b22bec6e2a66d44f12ac25e5f9adca3f
ms.sourcegitcommit: 6c01e4f82e19f9e423c3aaeaf801a29a517e97a0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74818288"
---
> [!NOTE]
> .NET Core es una plataforma de .NET multiplataforma de código abierto que implementa la especificación [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard).

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, compruebe lo siguiente:

> [!div class="checklist"]
> * [Ha creado un recurso de Voz de Azure](../../../../get-started.md)
> * [Ha configurado el entorno de desarrollo](../../../../quickstarts/setup-platform.md?tabs=dotnetcore)
> * [Ha creado un proyecto de ejemplo vacío](../../../../quickstarts/create-project.md?tabs=dotnetcore)
## <a name="add-sample-code"></a>Incorporación de código de ejemplo

1. Abra `Program.cs` y reemplace todo el código que contiene por lo siguiente.

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnetcore/text-to-speech/helloworld/Program.cs#code)]

1. En el mismo archivo, reemplace la cadena `YourSubscriptionKey` por la clave de suscripción.

1. Además, reemplace la cadena `YourServiceRegion` por la [región](~/articles/cognitive-services/Speech-Service/regions.md) asociada a su suscripción (por ejemplo, `westus` para la suscripción de evaluación gratuita).

1. Guarde los cambios en el proyecto.

## <a name="build-and-run-the-app"></a>Compilación y ejecución de la aplicación

1. Compile la aplicación. En la barra de menús, elija **Compilar** > **Compilar solución**. El código se debería compilar sin errores ahora.

    ![Captura de pantalla de la aplicación de Visual Studio, con la opción Compilar solución resaltada](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-dotnetcore-windows-05-build.png "Compilación correcta")

1. Inicie la aplicación. En la barra de menús, elija **Depurar** > **Iniciar depuración** o bien presione **F5**.

    ![Captura de pantalla de la aplicación de Visual Studio, con la opción Iniciar depuración resaltada](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-dotnetcore-windows-06-start-debugging.png "Inicio de la aplicación en depuración")

1. Aparece una ventana de consola que le pide que escriba texto. Escriba algunas palabras o una frase. El texto que escriba se transmite al servicio de voz y se sintetiza en una voz que se reproduce en el altavoz.

    ![Captura de pantalla de la salida de consola después de una síntesis correcta](~/articles/cognitive-services/Speech-Service/media/sdk/qs-tts-csharp-dotnet-windows-console-output.png "Salida de la consola después de una síntesis correcta")

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [footer](./footer.md)]

## <a name="see-also"></a>Otras referencias

- [Creación de una voz personalizada](~/articles/cognitive-services/Speech-Service/how-to-custom-voice-create-voice.md)
- [Grabación de ejemplos de voz personalizada](~/articles/cognitive-services/Speech-Service/record-custom-voice-samples.md)
