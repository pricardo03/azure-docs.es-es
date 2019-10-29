---
title: 'Inicio rápido: Reconocimiento de voz, C# (Xamarin): Speech Service'
titleSuffix: Azure Cognitive Services
description: En este artículo se crea una aplicación C# Xamarin multiplataforma para la Plataforma universal de Windows (UWP), Android e iOS mediante el SDK de Voz de Cognitive Services. Transcriba voz a texto en tiempo real desde el micrófono de su dispositivo o simulador. La aplicación se crea con el paquete NuGet del SDK de voz y Microsoft Visual Studio 2019.
services: cognitive-services
author: jhakulin
manager: robch
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 09/16/2019
ms.author: jhakulin
ms.custom: ''
ms.openlocfilehash: ad1c6f7d17d1e04fcfa5b187a29dadd2fa61edd8
ms.sourcegitcommit: 9a4296c56beca63430fcc8f92e453b2ab068cc62
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/20/2019
ms.locfileid: "72675625"
---
# <a name="quickstart-recognize-speech-by-using-a-cross-platform-xamarin-app-that-uses-the-speech-sdk"></a>Inicio rápido: Reconocimiento de voz mediante una aplicación Xamarin multiplataforma que usa el SDK de Voz

También hay disponibles inicios rápidos para [conversión de voz en texto](quickstart-csharp-uwp.md), [conversión de texto en voz](quickstart-text-to-speech-csharp-uwp.md) y [traducción de voz](quickstart-translate-speech-uwp.md).

En este artículo desarrollará una aplicación C# multiplataforma mediante Xamarin para la Plataforma universal de Windows (UWP), Android e iOS que usa el [SDK de Voz](speech-sdk.md) de Azure Cognitive Services. El programa transcribe voz en texto en tiempo real mediante el micrófono del dispositivo. La aplicación se compila con el [paquete NuGet del SDK de Voz](https://aka.ms/csspeech/nuget) y cualquier edición de Microsoft Visual Studio 2019.

## <a name="prerequisites"></a>Requisitos previos

Esta guía de inicio rápido requiere:

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/).
* Una clave de suscripción de Azure para el servicio Voz. [Obtenga una gratis](get-started.md).
* Un PC Windows con Windows 10 Fall Creators Update (10.0; compilación 16299), o cualquier versión posterior, y con un micrófono operativo.
* Una [instalación de Xamarin en Visual Studio](https://docs.microsoft.com/xamarin/get-started/installation/?pivots=windows).
* Una [instalación de Xamarin Android en Windows](https://docs.microsoft.com/xamarin/android/get-started/installation/windows).
* Una [instalación de Xamarin iOS en Windows](https://docs.microsoft.com/xamarin/ios/get-started/installation/windows/?pivots=windows).
* Si el destino es Android: 
   * Un dispositivo Android (ARM32/64, x86; API 23: Android 6.0 Marshmallow o posterior) [habilitado para el desarrollo](https://developer.android.com/studio/debug/dev-options) con un micrófono que funcione.
* Si el destino es iOS: 
    * un dispositivo iOS (ARM64) o un simulador de iOS (x64) [habilitado para el desarrollo](https://docs.microsoft.com/xamarin/ios/get-started/installation/device-provisioning/) con un micrófono en funcionamiento.
* Para que haya compatibilidad con la compilación ARM64 de Windows, instale las [herramientas de compilación opcionales y el SDK de Windows 10 para ARM/ARM64](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development/).

## <a name="create-a-visual-studio-project"></a>Creación de un proyecto de Visual Studio

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-xamarin-create-proj.md)]

## <a name="add-sample-code-for-the-common-helloworld-project"></a>Adición de código de ejemplo al proyecto común "helloworld"

El proyecto "helloworld" común contiene implementaciones independientes de la plataforma para una aplicación multiplataforma. Ahora, agregue el código XAML que define la interfaz de usuario de la aplicación y agregue el código C# que está detrás de la implementación.

1. En el **Explorador de soluciones**, en el proyecto "helloworld" común, abra `MainPage.xaml`.

1. En la vista XAML del diseñador, inserte el fragmento de código XAML siguiente en la etiqueta **Cuadrícula** entre `<StackLayout>` y `</StackLayout>`:

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/csharp-xamarin/helloworld/helloworld/MainPage.xaml)]

1. En **Explorador de soluciones**, abra el archivo de código fuente subyacente `MainPage.xaml.cs`. Se agrupa en `MainPage.xaml`.

1. Reemplace todo el código que contiene por el fragmento siguiente:

   [!code-csharp[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-xamarin/helloworld/helloworld/MainPage.xaml.cs)]

1. En el controlador `OnRecognitionButtonClicked` del archivo de código fuente, busque la cadena `YourSubscriptionKey` y reemplácela por su clave de suscripción.

1. En el controlador `OnRecognitionButtonClicked`, busque la cadena `YourServiceRegion` y reemplácela por la [región](regions.md) asociada a su suscripción. Por ejemplo, use `westus` para la suscripción de prueba gratuita.

1. A continuación, debe crear un [servicio Xamarin](https://docs.microsoft.com/xamarin/android/app-fundamentals/services/creating-a-service/), que se usa para consultar los permisos de micrófono desde proyectos de distintas plataformas como UWP, Android e iOS. Para ello, agregue una nueva carpeta denominada *Services* en el proyecto "helloworld" y cree un nuevo archivo de origen C# en ella. Puede hacer clic con el botón derecho en la carpeta *Services* y seleccionar **Agregar** > **Nuevo elemento** > **Archivo de código**. Cambie el nombre del archivo a `IMicrophoneService.cs` y coloque todo el código del siguiente fragmento en ese archivo:

   [!code-csharp[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-xamarin/helloworld/helloworld/Services/IMicrophoneService.cs)]

## <a name="add-sample-code-for-the-helloworldandroid-project"></a>Adición de código de ejemplo al proyecto helloworld.Android

Ahora, agregue el código C# que define la parte específica de Android de la aplicación.

1. En el **Explorador de soluciones**, en el proyecto helloworld.Android, abra `MainActivity.cs`.

1. Reemplace todo el código que contiene por el fragmento siguiente:

   [!code-csharp[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-xamarin/helloworld/helloworld.Android/MainActivity.cs)]

1. Después, agregue la implementación específica de Android para `MicrophoneService` mediante la creación de una carpeta *Services* en el proyecto helloworld.Android. Después, cree un nuevo archivo de código fuente C# en ella. Cambie el nombre del archivo a `MicrophoneService.cs`. Copie y pegue el siguiente fragmento de código en ese archivo:

   [!code-csharp[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-xamarin/helloworld/helloworld.Android/Services/MicrophoneService.cs)]

1. Después, abra `AndroidManifest.xml` en la carpeta *Properties*. Agregue el siguiente valor de permisos de usuario para el micrófono entre `<manifest>` y `</manifest>`:

   ```xml
   <uses-permission android:name="android.permission.RECORD_AUDIO" />
   ```

## <a name="add-sample-code-for-the-helloworldios-project"></a>Adición de código de ejemplo al proyecto helloworld.iOS

Ahora, agregue el código C# que define la parte específica de iOS de la aplicación. Cree también configuraciones específicas del dispositivo de Apple en el proyecto helloworld.iOS.

1. En el **Explorador de soluciones**, en el proyecto helloworld.iOS, abra `AppDelegate.cs`.

1. Reemplace todo el código que contiene por el fragmento siguiente:

   [!code-csharp[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-xamarin/helloworld/helloworld.iOS/AppDelegate.cs)]

1. Después, agregue la implementación específica de iOS para `MicrophoneService` mediante la creación de una carpeta *Services* en el proyecto helloworld.iO. Después, cree un nuevo archivo de código fuente C# en ella. Cambie el nombre del archivo a `MicrophoneService.cs`. Copie y pegue el siguiente fragmento de código en ese archivo:

   [!code-csharp[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-xamarin/helloworld/helloworld.iOS/Services/MicrophoneService.cs)]

1. Abra `Info.plist` en el proyecto helloworld.iOS en el editor de texto. Agregue el siguiente par clave-valor en la sección dict:

   <key>NSMicrophoneUsageDescription</key>
   <string>Esta aplicación de ejemplo requiere acceso al micrófono</string>

   > [!NOTE] 
   > Si va a compilar para un dispositivo iPhone, asegúrese de que `Bundle Identifier` coincida con el identificador de la aplicación del perfil de aprovisionamiento del dispositivo. En caso contrario, la compilación dará error. Con iPhoneSimulator puede dejarlo tal cual.

1. Si va a compilar en un PC Windows, debe establecer una conexión con el dispositivo Mac para compilar mediante **Tools** (Herramientas) > **iOS** > **Pair to Mac** (Emparejar con Mac). Siga el Asistente para instrucciones de Visual Studio para habilitar la conexión con el dispositivo Mac.

## <a name="add-sample-code-for-the-helloworlduwp-project"></a>Adición de código de ejemplo al proyecto helloworld.UWP

Ahora, agregue el código C# que define la parte específica de UWP de la aplicación.

1. En el **Explorador de soluciones**, en el proyecto helloworld.UWP, abra `MainPage.xaml.cs`.

1. Reemplace todo el código que contiene por el fragmento siguiente:

   [!code-csharp[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-xamarin/helloworld/helloworld.UWP/MainPage.xaml.cs)]

1. Después, agregue la implementación específica de UWP para `MicrophoneService` mediante la creación de una carpeta *Services* en el proyecto helloworld.UWP. Después, cree un nuevo archivo de código fuente C# en ella. Cambie el nombre del archivo a `MicrophoneService.cs`. Copie y pegue el siguiente fragmento de código en ese archivo:

   [!code-csharp[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-xamarin/helloworld/helloworld.UWP/Services/MicrophoneService.cs)]

1. A continuación, haga doble clic en el archivo `Package.appxmanifest` en el proyecto helloworld.UWP de Visual Studio. En **Funcionalidades**, asegúrese de que está seleccionado **Micrófono** y guarde el archivo.

   > [!NOTE] 
   > Si ve esta advertencia: "Certificate file does not exist: helloworld.UWP_TemporaryKey.pfx," (El archivo de certificado no existe: helloworld.UWP_TemporaryKey.pfx), consulte el ejemplo de conversión de [voz en texto](quickstart-csharp-uwp.md) para más información.

1. En la barra de menús, seleccione **Archivo** > **Guardar todo** para guardar los cambios.

## <a name="build-and-run-the-uwp-application"></a>Compilación y ejecución de la aplicación UWP

1. Establezca helloworld.UWP como proyecto de inicio. Haga clic con el botón derecho en el proyecto helloworld.UWP y seleccione **Crear** para crear la aplicación. 

1. Seleccione **Depurar** > **Iniciar depuración** (o seleccione **F5**) para iniciar la aplicación. Aparece la ventana **HelloWorld**.

   ![Ejemplo de aplicación de reconocimiento de voz de UWP en C#: inicio rápido](media/sdk/qs-csharp-xamarin-helloworld-uwp-window.png)

1. Seleccione **Habilitar micrófono**. Cuando aparezca la solicitud de permiso de acceso, seleccione **Sí**.

   ![Solicitud de permiso de acceso al micrófono](media/sdk/qs-csharp-xamarin-uwp-access-prompt.png)

1. Seleccione **Iniciar reconocimiento de voz**  y diga una frase en inglés en el micrófono del dispositivo. Lo que diga se transmitirá a los servicios de voz y se transcribirá en texto que aparece en la misma ventana.

   ![Interfaz de usuario del reconocimiento de voz](media/sdk/qs-csharp-xamarin-uwp-ui-result.png)

## <a name="build-and-run-the-android-and-ios-applications"></a>Compilación y ejecución de aplicaciones de iOS y Android

La compilación y ejecución de aplicaciones de iOS y Android en el dispositivo o en el simulador se producen de manera similar a como lo hacen en UWP. Asegúrese de que todos los SDK requeridos en la sección "Requisitos previos" de este artículo están instalados correctamente.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Exploración de ejemplos de C# en GitHub](https://aka.ms/csspeech/samples)
