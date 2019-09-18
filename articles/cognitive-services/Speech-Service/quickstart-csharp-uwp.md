---
title: 'Inicio rápido: Reconocimiento de voz, C# (UWP): Servicios de voz'
titleSuffix: Azure Cognitive Services
description: En este artículo, creará una aplicación para Plataforma universal de Windows (UWP) de C# mediante el SDK de Voz de Cognitive Services. Transcribe de voz a texto en tiempo real desde el micrófono de su dispositivo. La aplicación se crea con el paquete NuGet del SDK de voz y Microsoft Visual Studio 2019.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 08/19/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: fe5ff376a7895e2ca5246c0b9eb575752b07c7a1
ms.sourcegitcommit: 49c4b9c797c09c92632d7cedfec0ac1cf783631b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2019
ms.locfileid: "70382262"
---
# <a name="quickstart-recognize-speech-in-a-uwp-app-by-using-the-speech-sdk"></a>Inicio rápido: Reconocimiento de voz en una aplicación para UWP mediante el SDK de Voz

También hay disponibles guías de inicio rápido para la [síntesis de voz](quickstart-text-to-speech-csharp-uwp.md), la [traducción de voz](quickstart-translate-speech-uwp.md) y el [asistente virtual por voz](quickstart-virtual-assistant-csharp-uwp.md).

Si lo desea, elija otro lenguaje de programación diferente y entorno:<br/>
[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

En este artículo, desarrollará una aplicación para la Plataforma universal de Windows (UWP) de C# mediante el [SDK de voz](speech-sdk.md) de Cognitive Services. El programa transcribe voz en texto en tiempo real mediante el micrófono del dispositivo. La aplicación se compila con el [paquete NuGet del SDK de voz](https://aka.ms/csspeech/nuget) y Microsoft Visual Studio 2019 (cualquier edición).

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

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/csharp-uwp/helloworld/MainPage.xaml#StackPanel)]

1. En **Explorador de soluciones**, abra el archivo de código fuente subyacente `MainPage.xaml.cs`. (Se agrupa en `MainPage.xaml`).

1. Reemplace todo el código que contiene por el fragmento siguiente:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-uwp/helloworld/MainPage.xaml.cs#code)]

1. En el controlador `SpeechRecognitionFromMicrophone_ButtonClicked` del archivo de código fuente, busque la cadena `YourSubscriptionKey` y reemplácela por su clave de suscripción.

1. En el controlador `SpeechRecognitionFromMicrophone_ButtonClicked`, busque la cadena `YourServiceRegion` y reemplácela por la [región](regions.md) asociada a su suscripción. (Por ejemplo, use `westus` para la suscripción de prueba gratuita).

1. En la barra de menús, elija **Archivo** > **Guardar todo** para guardar los cambios.

## <a name="build-and-run-the-application"></a>Compilación y ejecución de la aplicación

Ahora está listo para compilar y probar la aplicación.

1. En la barra de menús, elija **Compilar** > **Compilar solución** para compilar la aplicación. El código se debería compilar sin errores ahora.

1. Elija **Depurar** > **Iniciar depuración** o presione **F5** para iniciar la aplicación. Aparece la ventana **HelloWorld**.

   ![Ejemplo de aplicación de reconocimiento de voz de UWP en C#: inicio rápido](media/sdk/qs-csharp-uwp-helloworld-window.png)

1. Seleccione **Habilitar micrófono** y, cuando aparezca la solicitud de permiso de acceso, seleccione **Sí**.

   ![Solicitud de permiso de acceso al micrófono](media/sdk/qs-csharp-uwp-10-access-prompt.png)

1. Seleccione **Speech recognition with microphone input** (Reconocimiento de voz con entrada de micrófono) y diga una frase en inglés en el micrófono del dispositivo. Lo que diga se transmitirá a los servicios de voz y se transcribirá en texto, que aparece en la misma ventana.

   ![Interfaz de usuario del reconocimiento de voz](media/sdk/qs-csharp-uwp-11-ui-result.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Exploración de ejemplos de C# en GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Otras referencias

- [Inicio rápido: Traducción de voz con el SDK de voz para C# (UWP)](quickstart-translate-speech-uwp.md)
- [Entrenamiento de un modelo de Custom Speech](how-to-custom-speech-train-model.md)
