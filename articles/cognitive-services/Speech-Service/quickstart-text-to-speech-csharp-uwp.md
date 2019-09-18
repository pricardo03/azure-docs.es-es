---
title: 'Inicio rápido: Síntesis de voz, C# (UWP): Servicios de voz'
titleSuffix: Azure Cognitive Services
description: En este artículo, creará una aplicación para Plataforma universal de Windows (UWP) de C# mediante el SDK de Voz de Cognitive Services. Puede sintetizar la voz a partir de texto en tiempo real en el altavoz del dispositivo. La aplicación se crea con el paquete NuGet del SDK de voz y Microsoft Visual Studio 2019.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 08/19/2019
ms.author: yinhew
ms.openlocfilehash: 65b65c9af377b6a9951f9f328e0732850d3b9c1d
ms.sourcegitcommit: 49c4b9c797c09c92632d7cedfec0ac1cf783631b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2019
ms.locfileid: "70382214"
---
# <a name="quickstart-synthesize-speech-in-a-uwp-app-by-using-the-speech-sdk"></a>Inicio rápido: Síntesis de voz en una aplicación para UWP mediante Speech SDK

También hay disponibles inicios rápidos para el [reconocimiento de voz](quickstart-csharp-uwp.md), la [traducción de voz](quickstart-translate-speech-uwp.md) y el [Asistente virtual por voz](quickstart-virtual-assistant-csharp-uwp.md).

En este artículo, desarrollará una aplicación para la Plataforma universal de Windows (UWP) de C# mediante el [SDK de voz](speech-sdk.md) de Cognitive Services. El programa sintetiza la voz a partir de texto en tiempo real en el altavoz del dispositivo. La aplicación se compila con el [paquete NuGet del SDK de voz](https://aka.ms/csspeech/nuget) y Microsoft Visual Studio 2019 (cualquier edición).

> [!NOTE]
> La Plataforma universal de Windows permite desarrollar aplicaciones que se ejecutan en cualquier dispositivo que admite Windows 10, incluidos PC, Xbox, Surface Hub y otros dispositivos.

## <a name="prerequisites"></a>Requisitos previos

Esta guía de inicio rápido requiere:

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/).
* Una clave de suscripción de Azure para el servicio Voz. [Obtenga una gratis](get-started.md).

## <a name="create-a-visual-studio-project"></a>Creación de un proyecto de Visual Studio

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-uwp-create-proj.md)]

## <a name="add-sample-code"></a>Incorporación de código de ejemplo

Ahora, agregue el código XAML que define la interfaz de usuario de la aplicación y agregue la implementación de código C# subyacente.

1. En el **Explorador de soluciones**, abra `MainPage.xaml`.

1. En la vista XAML del diseñador, inserte el fragmento de código XAML siguiente en la etiqueta **Cuadrícula** (entre `<Grid>` y `</Grid>`):

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/csharp-uwp/helloworld/MainPage.xaml#StackPanel)]

1. En **Explorador de soluciones**, abra el archivo de código fuente subyacente `MainPage.xaml.cs`. (Se agrupa en `MainPage.xaml`).

1. Reemplace todo el código que contiene por el fragmento siguiente:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/csharp-uwp/helloworld/MainPage.xaml.cs#code)]

1. En el controlador `Speak_ButtonClicked` del archivo de código fuente, busque la cadena `YourSubscriptionKey` y reemplácela por su clave de suscripción.

1. En el controlador `Speak_ButtonClicked`, busque la cadena `YourServiceRegion` y reemplácela por la [región](regions.md) asociada a su suscripción. (Por ejemplo, use `westus` para la suscripción de prueba gratuita).

1. En la barra de menús, elija **Archivo** > **Guardar todo** para guardar los cambios.

## <a name="build-and-run-the-application"></a>Compilación y ejecución de la aplicación

Ahora está listo para compilar y probar la aplicación.

1. En la barra de menús, elija **Compilar** > **Compilar solución** para compilar la aplicación. El código se debería compilar sin errores ahora.

1. Elija **Depurar** > **Iniciar depuración** o presione **F5** para iniciar la aplicación. Aparece la ventana **HelloWorld**.

   ![Ejemplo de aplicación de síntesis de voz de UWP en C#: inicio rápido](media/sdk/qs-text-to-speech-uwp-helloworld-window.png)

1. Escriba algún texto en el cuadro de texto y haga clic en **Hablar**. El texto se transmite a los servicios de Voz y se sintetiza en voz, la cual se reproduce en el altavoz.

    ![Interfaz de usuario de síntesis de voz](media/sdk/qs-tts-csharp-uwp-ui-result.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Exploración de ejemplos de C# en GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Otras referencias

- [Creación y uso de modelos de voz personalizados](how-to-custom-voice-create-voice.md)
- [Grabación de ejemplos de voz](record-custom-voice-samples.md)
