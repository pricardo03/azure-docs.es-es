---
title: 'Guía de inicio rápido: Reconocimiento y traducción de voz, .NET Framework (Windows) - servicio Voz'
titleSuffix: Azure Cognitive Services
description: Use esta guía para crear una aplicación de consola de conversión de voz en texto mediante .NET Framework para Windows y el SDK de Voz. Cuando termine, puede usar el micrófono del equipo para realizar la conversión de voz en texto en tiempo real.
services: cognitive-services
author: wolfma61
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 11/05/2018
ms.author: wolfma
ms.openlocfilehash: 502f59c88808480f26e6ea5a6d2c5b362c78869b
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "51683509"
---
# <a name="quickstart-recognize-and-transcribe-speech-using-the-speech-sdk-and-net-framework-windows"></a>Guía de inicio rápido: Reconocimiento y traducción de voz mediante el SDK de Voz y .NET Framework (Windows)

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Use esta guía para crear una aplicación de consola de conversión de voz en texto mediante .NET Framework para Windows y el SDK de Voz. Cuando termine, puede usar el micrófono del equipo para realizar la conversión de voz en texto en tiempo real.

En esta guía de inicio rápido, se requiere una [cuenta de Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con el servicio Voz de Microsoft habilitado. Si no tiene una cuenta, puede usar la [evaluación gratuita](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started) para obtener una clave de suscripción.

## <a name="prerequisites"></a>Requisitos previos

Para completar este proyecto necesitará lo siguiente:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Una clave de suscripción para el servicio Voz
* Acceso al micrófono del equipo

## <a name="create-a-visual-studio-project"></a>Creación de un proyecto de Visual Studio

[!INCLUDE [Create project ](../../../includes/cognitive-services-speech-service-create-speech-project-vs-csharp.md)]

## <a name="add-sample-code"></a>Incorporación de código de ejemplo

1. Abra `Program.cs` y reemplace el código generado automáticamente por el de este ejemplo:

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-dotnet-windows/helloworld/Program.cs#code)]

1. Localice la cadena `YourSubscriptionKey` y reemplácela por su clave de suscripción del servicio Voz.

1. Localice la cadena `YourServiceRegion` y reemplácela por la [región](regions.md) asociada a sus suscripción. Por ejemplo, si usa la evaluación gratuita, la región es `westus`.

1. Guarde los cambios realizados en el proyecto.

## <a name="build-and-run-the-app"></a>Compilación y ejecución de la aplicación

1. En la barra de menús, seleccione **Compilar** > **Compilar solución**. El código se debería compilar sin errores ahora.

    ![Captura de pantalla de la aplicación de Visual Studio, con la opción Generar solución resaltada](media/sdk/qs-csharp-dotnet-windows-08-build.png "Compilación correcta")

1. En la barra de menús, seleccione **Depurar** > **Iniciar depuración** o presione **F5** para iniciar la depuración.

    ![Captura de pantalla de la aplicación de Visual Studio, con la opción Iniciar depuración resaltada](media/sdk/qs-csharp-dotnet-windows-09-start-debugging.png "Iniciar la aplicación en depuración")

1. Aparecerá una ventana de consola en la que se le solicita que hable. Ahora, diga algo. Lo que diga se transmitirá al servicio de Voz y se traducirá a texto en tiempo real. El resultado de imprime en la consola.

    ![Captura de pantalla de la salida de la consola después de un reconocimiento correcto](media/sdk/qs-csharp-dotnet-windows-10-console-output.png "Salida de la consola después de un reconocimiento correcto")

[!INCLUDE [Download this sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
El código está disponible en la carpeta `quickstart/csharp-dotnet-windows`.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Reconocimiento de intenciones a partir de contenido de voz con el SDK de Voz para C#](how-to-recognize-intents-from-speech-csharp.md)

## <a name="see-also"></a>Otras referencias

- [Traducción de voz](how-to-translate-speech-csharp.md)
- [Personalización de modelos acústicos](how-to-customize-acoustic-models.md)
- [Personalización de modelos de lenguaje](how-to-customize-language-model.md)
