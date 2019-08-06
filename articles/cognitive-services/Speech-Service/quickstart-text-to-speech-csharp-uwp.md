---
title: 'Inicio rápido: Síntesis de voz, C# (UWP): Servicios de voz'
titleSuffix: Azure Cognitive Services
description: En este artículo, creará una aplicación para Plataforma universal de Windows (UWP) de C# mediante el SDK de Voz de Cognitive Services. Puede sintetizar la voz a partir de texto en tiempo real en el altavoz del dispositivo. La aplicación se crea con el paquete NuGet del SDK de Voz y Microsoft Visual Studio 2017.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 6/24/2019
ms.author: yinhew
ms.openlocfilehash: 5e1f8aea1b00cbba7fec6c7ca416a965458ab526
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2019
ms.locfileid: "68607746"
---
# <a name="quickstart-synthesize-speech-in-a-uwp-app-by-using-the-speech-sdk"></a>Inicio rápido: Síntesis de voz en una aplicación para UWP mediante Speech SDK

También hay disponibles inicios rápidos para el [reconocimiento de voz](quickstart-csharp-uwp.md), la [traducción de voz](quickstart-translate-speech-uwp.md) y el [Asistente virtual por voz](quickstart-virtual-assistant-csharp-uwp.md).

En este artículo, desarrollará una aplicación de la Plataforma universal de Windows (UWP; versión de Windows 1709 o posterior) para C# mediante el [SDK de Voz](speech-sdk.md) de Cognitive Services. El programa sintetizará la voz a partir de texto en tiempo real en el altavoz del dispositivo. La aplicación se compila con el [paquete NuGet del SDK de Voz](https://aka.ms/csspeech/nuget) y Microsoft Visual Studio 2017 o una versión posterior (cualquier edición).

> [!NOTE]
> La Plataforma universal de Windows permite desarrollar aplicaciones que se ejecutan en cualquier dispositivo que admite Windows 10, incluidos PC, Xbox, Surface Hub y otros dispositivos.

## <a name="prerequisites"></a>Requisitos previos

Esta guía de inicio rápido requiere:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) o versiones posteriores
* Una clave de suscripción de Azure para el servicio Voz. [Obtenga una gratis](get-started.md).

## <a name="create-a-visual-studio-project"></a>Creación de un proyecto de Visual Studio

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-uwp-create-proj.md)]

## <a name="add-sample-code"></a>Incorporación de código de ejemplo

1. La interfaz de usuario de la aplicación se define mediante el uso de XAML. Abra `MainPage.xaml` en el Explorador de soluciones. En la vista XAML del diseñador, inserte el fragmento de código XAML siguiente en la etiqueta de cuadrícula (entre `<Grid>` y `</Grid>`).

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/csharp-uwp/helloworld/MainPage.xaml#StackPanel)]

1. Abra el archivo de código subyacente `MainPage.xaml.cs` (Búsquelo agrupado en `MainPage.xaml`). Reemplace todo el código que contiene por lo siguiente.

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/csharp-uwp/helloworld/MainPage.xaml.cs#code)]

1. En el controlador `Speak_ButtonClicked` de este archivo, reemplace la cadena `YourSubscriptionKey` por la clave de suscripción.

1. En el controlador `Speak_ButtonClicked`, reemplace la cadena `YourServiceRegion` por la [región](regions.md) asociada con la suscripción (por ejemplo, `westus` para la suscripción de evaluación gratuita).

1. Guarde todos los cambios en el proyecto.

## <a name="build-and-run-the-app"></a>Compilación y ejecución de la aplicación

1. Compile la aplicación. En la barra de menús, seleccione **Compilar** > **Compilar solución**. El código se debería compilar sin errores ahora.

    ![Captura de pantalla de la aplicación de Visual Studio, con la opción Generar solución resaltada](media/sdk/qs-csharp-uwp-08-build.png "Compilación correcta")

1. Inicie la aplicación. En la barra de menús, seleccione **Depurar** > **Iniciar depuración** o bien presione **F5**.

    ![Captura de pantalla de la aplicación de Visual Studio, con la opción Iniciar depuración resaltada](media/sdk/qs-csharp-uwp-09-start-debugging.png "Iniciar la aplicación en depuración")

1. Escriba algún texto en el cuadro de texto y haga clic en **Hablar**. El texto se transmite a los servicios de Voz y se sintetiza en voz, la cual se reproduce en el altavoz.

    ![Captura de pantalla de la interfaz de usuario de síntesis de voz](media/sdk/qs-tts-csharp-uwp-ui-result.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Exploración de ejemplos de C# en GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Otras referencias

- [Personalizar las fuentes de voz](how-to-customize-voice-font.md)
- [Grabación de ejemplos de voz](record-custom-voice-samples.md)
