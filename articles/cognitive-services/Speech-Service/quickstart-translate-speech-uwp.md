---
title: 'Inicio rápido: Traducción de voz, C# (UWP): Servicios Voz'
titleSuffix: Azure Cognitive Services
description: En este inicio rápido, creará una sencilla aplicación con la Plataforma universal de Windows (UWP) para capturar la voz del usuario, traducirla a otro idioma y mostrar el texto en la línea de comandos. Esta guía está diseñada para usuarios de Windows.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 12/13/2018
ms.author: erhopf
ms.openlocfilehash: bb0296098d0717e95e9aa1d73229d59709d13766
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2019
ms.locfileid: "56106093"
---
# <a name="quickstart-translate-speech-with-the-speech-sdk-for-c-uwp"></a>Inicio rápido: Traducción de voz con el SDK de Voz para C# (UWP)

En este inicio rápido, creará una sencilla aplicación con la Plataforma universal de Windows (UWP) que captura la voz del usuario procedente del micrófono del equipo, traduce el contenido de voz y transcribe el texto traducido en la línea de comandos en tiempo real. La aplicación está diseñada para ejecutarse en Windows de 64 bits y se ha creado con el [paquete NuGet del SDK de Voz](https://aka.ms/csspeech/nuget) y Microsoft Visual Studio 2017.

Para obtener una lista completa de los idiomas disponibles para la traducción de voz, consulte la [compatibilidad con idiomas](language-support.md).

> [!NOTE]
> La Plataforma universal de Windows permite desarrollar aplicaciones que se ejecutan en cualquier dispositivo que admita Windows 10, incluidos PC, Xbox, Surface Hub y otros dispositivos.

## <a name="prerequisites"></a>Requisitos previos

Esta guía de inicio rápido requiere:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Una clave de suscripción de Azure para el servicio Voz. [Obtenga una gratis](get-started.md).

## <a name="create-a-visual-studio-project"></a>Creación de un proyecto de Visual Studio

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-uwp-create-proj.md)]

## <a name="add-sample-code"></a>Incorporación de código de ejemplo

1. La interfaz de usuario de la aplicación se define mediante el uso de XAML. Abra `MainPage.xaml` en el Explorador de soluciones. En la vista XAML del diseñador, inserte el fragmento de código XAML siguiente entre `<Grid>` y `</Grid>`.

    [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/speech-translation/csharp-uwp/helloworld/MainPage.xaml#StackPanel)]

1. Abra el archivo de código subyacente `MainPage.xaml.cs` (Búsquelo agrupado en `MainPage.xaml`). Reemplace todo el código que contiene por lo siguiente.

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/speech-translation/csharp-uwp/helloworld/MainPage.xaml.cs#code)]

1. En el controlador `SpeechTranslationFromMicrophone_ButtonClicked` de este archivo, reemplace la cadena `YourSubscriptionKey` por la clave de suscripción.

1. En el controlador `SpeechTranslationFromMicrophone_ButtonClicked`, reemplace la cadena `YourServiceRegion` por la [región](regions.md) asociada con la suscripción (por ejemplo, `westus` para la suscripción de evaluación gratuita).

1. Guarde todos los cambios en el proyecto.

## <a name="build-and-run-the-app"></a>Compilación y ejecución de la aplicación

1. Compile la aplicación. En la barra de menús, seleccione **Compilar** > **Compilar solución**. El código se debería compilar sin errores ahora.

    ![Captura de pantalla de la aplicación de Visual Studio, con la opción Generar solución resaltada](media/sdk/qs-csharp-uwp-08-build.png "Compilación correcta")

1. Inicie la aplicación. En la barra de menús, seleccione **Depurar** > **Iniciar depuración** o bien presione **F5**.

    ![Captura de pantalla de la aplicación de Visual Studio, con la opción Iniciar depuración resaltada](media/sdk/qs-csharp-uwp-09-start-debugging.png "Iniciar la aplicación en depuración")

1. Aparece una ventana. Seleccione **Enable Microphone** (Habilitar teléfono) y confirme la solicitud de permiso emergente.

    ![Captura de pantalla de la solicitud de permiso](media/sdk/qs-csharp-uwp-10-access-prompt.png "Iniciar la aplicación en depuración")

1. Seleccione **Speech recognition with microphone input** (Reconocimiento de voz con entrada de micrófono) y diga una frase en inglés en el micrófono del dispositivo. Lo que diga se transmitirá al servicio de Voz y se transcribe en texto, que aparece en la misma ventana.

    ![Captura de pantalla de la interfaz de usuario de reconocimiento de voz](media/sdk/qs-translate-csharp-uwp-ui-result.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Exploración de ejemplos de C# en GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Otras referencias

- [Personalización de modelos acústicos](how-to-customize-acoustic-models.md)
- [Personalización de modelos de lenguaje](how-to-customize-language-model.md)
