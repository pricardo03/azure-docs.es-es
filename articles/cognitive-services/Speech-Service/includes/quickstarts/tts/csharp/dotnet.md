---
title: 'Inicio rápido: Síntesis de voz en C# (Windows): servicio de voz'
titleSuffix: Azure Cognitive Services
description: Use esta guía para crear una aplicación de consola de conversión de texto a voz mediante .NET Framework para Windows y el SDK de Voz. Cuando termine, puede sintetizar la voz a partir de texto y escuchar la voz en su altavoz en tiempo real.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 08/27/2019
ms.author: yinhew
ms.openlocfilehash: 38dac73d253245fce86ac419c7a4eb3a106df3f0
ms.sourcegitcommit: 6c01e4f82e19f9e423c3aaeaf801a29a517e97a0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74818324"
---
## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, compruebe lo siguiente:

> [!div class="checklist"]
> * [Ha creado un recurso de Voz de Azure](../../../../get-started.md)
> * [Ha configurado el entorno de desarrollo](../../../../quickstarts/setup-platform.md?tabs=dotnet)
> * [Ha creado un proyecto de ejemplo vacío](../../../../quickstarts/create-project.md?tabs=dotnet)

## <a name="add-sample-code"></a>Incorporación de código de ejemplo

1. Abra **Program.cs** y reemplace el código generado automáticamente por el de este ejemplo:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/text-to-speech/helloworld/Program.cs#code)]

1. Busque la cadena `YourSubscriptionKey` y reemplácela por su clave de suscripción del servicio de voz.

1. Busque la cadena `YourServiceRegion` y reemplácela por la [ región ](~/articles/cognitive-services/Speech-Service/regions.md) asociada a su suscripción. Por ejemplo, si usa la suscripción de evaluación gratuita, la región es `westus`.

1. En la barra de menús, elija **Archivo** > **Guardar todo**.

## <a name="build-and-run-the-application"></a>Compilación y ejecución de la aplicación

1. En la barra de menús, elija **Compilar** > **Compilar solución** para compilar la aplicación. El código se debería compilar sin errores ahora.

1. Elija **Depurar** > **Iniciar depuración** (o seleccione **F5**) para iniciar la aplicación **helloworld**.

1. Escriba una oración o frase en inglés. La aplicación transmite el texto al servicio de voz, que envía la voz sintetizada a la aplicación para que se reproduzca en el altavoz.

   ![Interfaz de usuario de síntesis de voz](~/articles/cognitive-services/Speech-Service/media/sdk/qs-tts-csharp-dotnet-windows-console-output.png)

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [footer](./footer.md)]

## <a name="see-also"></a>Otras referencias

- [Creación de una voz personalizada](~/articles/cognitive-services/Speech-Service/how-to-custom-voice-create-voice.md)
- [Grabación de ejemplos de voz personalizada](~/articles/cognitive-services/Speech-Service/record-custom-voice-samples.md)
