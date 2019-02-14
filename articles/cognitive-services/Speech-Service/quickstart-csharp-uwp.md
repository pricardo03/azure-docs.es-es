---
title: 'Guía de inicio rápido: Reconocimiento de voz, C# (UWP) (Servicios Voz)'
titleSuffix: Azure Cognitive Services
description: En este artículo, creará una aplicación para Plataforma universal de Windows (UWP) de C# mediante el SDK de Voz de Cognitive Services. Transcribe de voz a texto en tiempo real desde el micrófono de su dispositivo. La aplicación se crea con el paquete NuGet del SDK de Voz y Microsoft Visual Studio 2017.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 12/06/2018
ms.author: wolfma
ms.custom: seodec18
ms.openlocfilehash: f29848098a99352e4e9e7a3caac4d074ebcb24b0
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55878517"
---
# <a name="quickstart-recognize-speech-in-a-uwp-app-by-using-the-speech-sdk"></a>Guía de inicio rápido: Reconocimiento de voz en una aplicación para UWP mediante el SDK de Voz

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

En este artículo, desarrollará una aplicación de la Plataforma universal de Windows (UWP; versión de Windows 1709 o posterior) para C# mediante el [SDK de Voz](speech-sdk.md) de Cognitive Services. El programa transcribe voz a texto en tiempo real mediante el micrófono del dispositivo. La aplicación se compila con el [paquete NuGet del SDK de Voz](https://aka.ms/csspeech/nuget) y Microsoft Visual Studio 2017 (cualquier edición).

> [!NOTE]
> La Plataforma universal de Windows permite desarrollar aplicaciones que se ejecutan en cualquier dispositivo que admite Windows 10, incluidos PC, Xbox, Surface Hub y otros dispositivos.

## <a name="prerequisites"></a>Requisitos previos

Esta guía de inicio rápido requiere:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Una clave de suscripción de Azure para el servicio Voz. [Obtenga una gratis](get-started.md).

## <a name="create-a-visual-studio-project"></a>Creación de un proyecto de Visual Studio

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-uwp-create-proj.md)]

## <a name="add-sample-code"></a>Incorporación de código de ejemplo

1. La interfaz de usuario de la aplicación se define mediante el uso de XAML. Abra `MainPage.xaml` en el Explorador de soluciones. En la vista XAML del diseñador, inserte el fragmento de código XAML siguiente en la etiqueta de cuadrícula (entre `<Grid>` y `</Grid>`).

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/csharp-uwp/helloworld/MainPage.xaml#StackPanel)]

1. Abra el archivo de código subyacente `MainPage.xaml.cs` (Búsquelo agrupado en `MainPage.xaml`). Reemplace todo el código que contiene por lo siguiente.

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-uwp/helloworld/MainPage.xaml.cs#code)]

1. En el controlador `SpeechRecognitionFromMicrophone_ButtonClicked` de este archivo, reemplace la cadena `YourSubscriptionKey` por la clave de suscripción.

1. En el controlador `SpeechRecognitionFromMicrophone_ButtonClicked`, reemplace la cadena `YourServiceRegion` por la [región](regions.md) asociada con la suscripción (por ejemplo, `westus` para la suscripción de evaluación gratuita).

1. Guarde todos los cambios en el proyecto.

## <a name="build-and-run-the-app"></a>Compilación y ejecución de la aplicación

1. Compile la aplicación. En la barra de menús, seleccione **Compilar** > **Compilar solución**. El código se debería compilar sin errores ahora.

    ![Captura de pantalla de la aplicación de Visual Studio, con la opción Generar solución resaltada](media/sdk/qs-csharp-uwp-08-build.png "Compilación correcta")

1. Inicie la aplicación. En la barra de menús, seleccione **Depurar** > **Iniciar depuración** o bien presione **F5**.

    ![Captura de pantalla de la aplicación de Visual Studio, con la opción Iniciar depuración resaltada](media/sdk/qs-csharp-uwp-09-start-debugging.png "Iniciar la aplicación en depuración")

1. Aparece una ventana. Seleccione **Enable Microphone** (Habilitar teléfono) y confirme la solicitud de permiso emergente.

    ![Captura de pantalla de la solicitud de permiso](media/sdk/qs-csharp-uwp-10-access-prompt.png "Iniciar la aplicación en depuración")

1. Seleccione **Speech recognition with microphone input** (Reconocimiento de voz con entrada de micrófono) y diga una frase en inglés en el micrófono del dispositivo. Lo que diga se transmitirá al servicio de Voz y se transcribe en texto, que aparece en la misma ventana.

    ![Captura de pantalla de la interfaz de usuario de reconocimiento de voz](media/sdk/qs-csharp-uwp-11-ui-result.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Exploración de ejemplos de C# en GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Otras referencias

- [Traducción de voz](how-to-translate-speech-csharp.md)
- [Personalización de modelos acústicos](how-to-customize-acoustic-models.md)
- [Personalización de modelos de lenguaje](how-to-customize-language-model.md)
