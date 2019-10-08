---
title: 'Inicio rápido: Reconocimiento de voz en .NET Framework para Windows (Servicios de voz)'
titleSuffix: Azure Cognitive Services
description: Use esta guía para crear una aplicación de consola de conversión de voz en texto mediante .NET Framework para Windows y el SDK de Voz. Cuando termine, puede usar el micrófono del equipo para realizar la conversión de voz en texto en tiempo real.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 08/28/2019
ms.author: wolfma
ms.openlocfilehash: cb140647394858fbc0a9a00ea125365d5b7a08d5
ms.sourcegitcommit: e9936171586b8d04b67457789ae7d530ec8deebe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71327040"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-net-framework-windows"></a>Inicio rápido: Reconocimiento de voz con el SDK de Voz para .NET Framework (Windows)

También hay guías de inicio rápido para la [síntesis](quickstart-text-to-speech-dotnet-windows.md) y la [traducción de voz](quickstart-translate-speech-dotnetframework-windows.md).

Si lo desea, elija otro lenguaje de programación diferente y entorno:<br/>
[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Use esta guía para crear una aplicación de consola de conversión de voz en texto mediante .NET Framework para Windows y el SDK de Voz. Cuando termine, puede usar el micrófono del equipo para realizar la conversión de voz en texto en tiempo real.

Para ver una demostración rápida (sin tener que compilar el proyecto de Visual Studio, como se ha descrito en este artículo), obtenga los [ejemplos del SDK de Voz de Cognitive Services](https://github.com/Azure-Samples/cognitive-services-speech-sdk) de GitHub.

## <a name="prerequisites"></a>Requisitos previos

Para completar este proyecto necesitará lo siguiente:

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/).
* Una clave de suscripción para el servicio Voz. [Obtenga una gratis](get-started.md).
* Acceso al micrófono del equipo.

## <a name="create-a-visual-studio-project"></a>Creación de un proyecto de Visual Studio

[!INCLUDE [Create project](../../../includes/cognitive-services-speech-service-create-speech-project-vs-csharp.md)]

## <a name="add-sample-code"></a>Incorporación de código de ejemplo

1. Abra **Program.cs** y reemplace el código generado automáticamente por el de este ejemplo:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-dotnet-windows/helloworld/Program.cs#code)]

1. Busque la cadena `YourSubscriptionKey` y reemplácela por su clave de suscripción a Speech Services.

1. Busque la cadena `YourServiceRegion` y reemplácela por la [ región ](regions.md) asociada a su suscripción. Por ejemplo, si usa la suscripción de evaluación gratuita, la región es `westus`.

1. En la barra de menús, elija **Archivo** > **Guardar todo**.

## <a name="build-and-run-the-app"></a>Compilación y ejecución de la aplicación

1. En la barra de menús, elija **Compilar** > **Compilar solución** para compilar la aplicación. El código se debería compilar sin errores ahora.

1. Elija **Depurar** > **Iniciar depuración** (o seleccione **F5**) para iniciar la aplicación **helloworld**.

1. Diga una expresión o frase en inglés en el micrófono del dispositivo. La aplicación transmite su voz a Speech Services, que devuelve el texto transcrito a la aplicación para que se muestre.

   ![Interfaz de usuario del reconocimiento de voz](media/sdk/qs-csharp-dotnet-windows-10-console-output.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Exploración de ejemplos de C# en GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Otras referencias

- [Entrenamiento de un modelo de Custom Speech](how-to-custom-speech-train-model.md)
