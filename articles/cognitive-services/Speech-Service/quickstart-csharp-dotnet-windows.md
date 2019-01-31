---
title: 'Inicio rápido: Reconocimiento de voz, .NET Framework (Windows): servicios de voz'
titleSuffix: Azure Cognitive Services
description: Use esta guía para crear una aplicación de consola de conversión de voz en texto mediante .NET Framework para Windows y el SDK de Voz. Cuando termine, puede usar el micrófono del equipo para realizar la conversión de voz en texto en tiempo real.
services: cognitive-services
author: wolfma61
manager: cgronlun
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 12/13/2018
ms.author: wolfma
ms.openlocfilehash: 9d01a6658a62abdc84a18c3e5dbec7cb88aabc98
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55208167"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-net-framework-windows"></a>Inicio rápido: Reconocimiento de voz con el SDK de Voz para .NET Framework (Windows)

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Use esta guía para crear una aplicación de consola de conversión de voz en texto mediante .NET Framework para Windows y el SDK de Voz. Cuando termine, puede usar el micrófono del equipo para realizar la conversión de voz en texto en tiempo real.

## <a name="prerequisites"></a>Requisitos previos

Para completar este proyecto necesitará lo siguiente:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Una clave de suscripción para el servicio Voz. [Obtenga una gratis](get-started.md).
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

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Exploración de ejemplos de C# en GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Otras referencias

- [Personalización de modelos acústicos](how-to-customize-acoustic-models.md)
- [Personalización de modelos de lenguaje](how-to-customize-language-model.md)
