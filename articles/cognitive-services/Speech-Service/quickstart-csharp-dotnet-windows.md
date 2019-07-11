---
title: 'Inicio rápido: Reconocimiento de voz, .NET Framework (Windows): servicios de voz'
titleSuffix: Azure Cognitive Services
description: Use esta guía para crear una aplicación de consola de conversión de voz en texto mediante .NET Framework para Windows y el SDK de Voz. Cuando termine, puede usar el micrófono del equipo para realizar la conversión de voz en texto en tiempo real.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: wolfma
ms.openlocfilehash: d8738357a3bad6626ef6d79248aef1c4d2cb1ead
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67603086"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-net-framework-windows"></a>Inicio rápido: Reconocimiento de voz con el SDK de Voz para .NET Framework (Windows)

También hay disponibles guías de inicio rápido para la conversión [texto a voz](quickstart-text-to-speech-dotnet-windows.md) y [ la traducción de voz](quickstart-translate-speech-dotnetframework-windows.md).

Si lo desea, elija otro lenguaje de programación u otro entorno:<br/>
[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Use esta guía para crear una aplicación de consola de conversión de voz en texto mediante .NET Framework para Windows y el SDK de Voz. Cuando termine, puede usar el micrófono del equipo para realizar la conversión de voz en texto en tiempo real.

Para ver una demostración rápida (sin compilar el proyecto de Visual Studio personalmente como se muestra a continuación):

Obtenga los [ejemplos más recientes del SDK de Voz de Cognitive Services](https://github.com/Azure-Samples/cognitive-services-speech-sdk) de GitHub.

## <a name="prerequisites"></a>Requisitos previos

Para completar este proyecto necesitará lo siguiente:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Una clave de suscripción para el servicio Voz. [Obtenga una gratis](get-started.md).
* Acceso al micrófono del equipo

## <a name="create-a-visual-studio-project"></a>Creación de un proyecto de Visual Studio

[!INCLUDE [Create project](../../../includes/cognitive-services-speech-service-create-speech-project-vs-csharp.md)]

## <a name="add-sample-code"></a>Incorporación de código de ejemplo

1. Abra `Program.cs` y reemplace el código generado automáticamente por el de este ejemplo:

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-dotnet-windows/helloworld/Program.cs#code)]

1. Localice la cadena `YourSubscriptionKey` y reemplácela por su clave de suscripción a los servicios de voz.

1. Localice la cadena `YourServiceRegion` y reemplácela por la [región](regions.md) asociada a sus suscripción. Por ejemplo, si usa la evaluación gratuita, la región es `westus`.

1. Guarde los cambios realizados en el proyecto.

## <a name="build-and-run-the-app"></a>Compilación y ejecución de la aplicación

1. En la barra de menús, seleccione **Compilar** > **Compilar solución**. El código se debería compilar sin errores ahora.

    ![Captura de pantalla de la aplicación de Visual Studio, con la opción Generar solución resaltada](media/sdk/qs-csharp-dotnet-windows-08-build.png "Compilación correcta")

1. En la barra de menús, seleccione **Depurar** > **Iniciar depuración** o presione **F5** para iniciar la depuración.

    ![Captura de pantalla de la aplicación de Visual Studio, con la opción Iniciar depuración resaltada](media/sdk/qs-csharp-dotnet-windows-09-start-debugging.png "Iniciar la aplicación en depuración")

1. Aparecerá una ventana de consola en la que se le solicita que hable. Ahora, diga algo. Lo que diga se transmitirá a los servicios de voz y se traducirá a texto en tiempo real. El resultado de imprime en la consola.

    ![Captura de pantalla de la salida de la consola después de un reconocimiento correcto](media/sdk/qs-csharp-dotnet-windows-10-console-output.png "Salida de la consola después de un reconocimiento correcto")

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Exploración de ejemplos de C# en GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Otras referencias

- [Personalización de modelos acústicos](how-to-customize-acoustic-models.md)
- [Personalización de modelos de lenguaje](how-to-customize-language-model.md)
