---
title: 'Inicio rápido: Reconocimiento de voz, C# (Xamarin): Speech Service'
titleSuffix: Azure Cognitive Services
description: En este artículo se crea una aplicación C# Xamarin multiplataforma para Windows UWP, Android e iOS mediante el SDK de Voz de Cognitive Services. Transcriba voz a texto en tiempo real desde el micrófono de su dispositivo o simulador. La aplicación se crea con el paquete NuGet del SDK de voz y Microsoft Visual Studio 2019.
services: cognitive-services
author: jhakulin
manager: robch
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 09/16/2019
ms.author: jhakulin
ms.custom: ''
ms.openlocfilehash: c9467bac8b5998252c021faca4eb4177c42a1736
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72387385"
---
# <a name="quickstart-recognize-speech-using-cross-platform-xamarin-app-by-using-the-speech-sdk"></a>Inicio rápido: Reconocimiento de voz mediante una aplicación Xamarin multiplataforma mediante el SDK de Voz

También hay disponibles inicios rápidos para [conversión de voz en texto](quickstart-csharp-uwp.md), [conversión de texto en voz](quickstart-text-to-speech-csharp-uwp.md) y [traducción de voz](quickstart-translate-speech-uwp.md).

En este artículo, va a desarrollar una aplicación en C# multiplataforma con Xamarin para Plataforma universal de Windows (UWP), Android e iOS mediante el [SDK de Voz](speech-sdk.md). El programa transcribe voz en texto en tiempo real mediante el micrófono del dispositivo. La aplicación se compila con el [paquete NuGet del SDK de voz](https://aka.ms/csspeech/nuget) y Microsoft Visual Studio 2019 (cualquier edición).

## <a name="prerequisites"></a>Requisitos previos

Esta guía de inicio rápido requiere:

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/).
* Una clave de suscripción de Azure para el servicio Voz. [Obtenga una gratis](get-started.md).
* Un PC Windows con Windows 10 Fall Creators Update (10.0; compilación 16299), o cualquier versión posterior, y con un micrófono operativo.
* [Instalación de Xamarin en Visual Studio](https://docs.microsoft.com/xamarin/get-started/installation/?pivots=windows).
* [Instalación de Xamarin Android en Windows](https://docs.microsoft.com/xamarin/android/get-started/installation/windows).
* [Instalación de Xamarin iOS en Windows](https://docs.microsoft.com/xamarin/ios/get-started/installation/windows/?pivots=windows).
* Si el destino es Android: 
   * Un dispositivo Android (ARM32/64, x86; API 23: Android 6.0 Marshmallow o posterior) [habilitado para el desarrollo](https://developer.android.com/studio/debug/dev-options) con un micrófono que funcione.
* Si el destino es iOS: un dispositivo iOS (ARM64) o un simulador de iOS (x64) [habilitado para el desarrollo ](https://docs.microsoft.com/xamarin/ios/get-started/installation/device-provisioning/)con un micrófono en funcionamiento.
* Para que haya compatibilidad con la compilación ARM64, instale las [y el SDK de Windows 10 para ARM/ARM64](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development/).

## <a name="create-a-visual-studio-project"></a>Creación de un proyecto de Visual Studio

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-xamarin-create-proj.md)]

## <a name="add-sample-code-for-the-common-helloworld-project"></a>Adición de código de ejemplo al proyecto `helloworld` común

El proyecto `helloworld` común contiene implementaciones independientes de la plataforma para una aplicación multiplataforma.
Ahora, agregue el código XAML que define la interfaz de usuario de la aplicación y agregue el código C# que está detrás de la implementación.

1. En el **Explorador de soluciones**, en el proyecto `helloworld` común, abra `MainPage.xaml`.

1. En la vista XAML del diseñador, inserte el fragmento de código XAML siguiente en la etiqueta **Cuadrícula** (entre `<StackLayout>` y `</StackLayout>`):

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/csharp-xamarin/helloworld/helloworld/MainPage.xaml)]

1. En **Explorador de soluciones**, abra el archivo de código fuente subyacente `MainPage.xaml.cs`. (Se agrupa en `MainPage.xaml`).

1. Reemplace todo el código que contiene por el fragmento siguiente:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-xamarin/helloworld/helloworld/MainPage.xaml.cs)]

1. En el controlador `OnRecognitionButtonClicked` del archivo de código fuente, busque la cadena `YourSubscriptionKey` y reemplácela por su clave de suscripción.

1. En el controlador `OnRecognitionButtonClicked`, busque la cadena `YourServiceRegion` y reemplácela por la [región](regions.md) asociada a su suscripción. (Por ejemplo, use `westus` para la suscripción de prueba gratuita).

1. A continuación, debe crear un [servicio Xamarin](https://docs.microsoft.com/xamarin/android/app-fundamentals/services/creating-a-service/), que se usa para consultar los permisos de micrófono desde distintos proyectos de plataforma (UWP, Android e iOS). Para ello, agregue una nueva carpeta `Services` en el proyecto `helloworld` y cree un archivo de origen en C# en ella (haga clic con el botón derecho en la carpeta `Services` y en **Agregar** > **Nuevo elemento**  > **Archivo de código**), asígnele el nombre `IMicrophoneService.cs` y coloque todo el código del siguiente fragmento en ese archivo:

[!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-xamarin/helloworld/helloworld/Services/IMicrophoneService.cs)]

## <a name="add-sample-code-for-the-helloworldandroid-project"></a>Adición de código de ejemplo al proyecto `helloworld.Android`

Ahora, agregue el código C# que define la parte específica de Android de la aplicación.

1. En el **Explorador de soluciones**, en el proyecto `helloworld.Android`, abra `MainActivity.cs`.

1. Reemplace todo el código que contiene por el fragmento siguiente:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-xamarin/helloworld/helloworld.Android/MainActivity.cs)]

1. Después, agregue la implementación específica de Android para `MicrophoneService` mediante la creación de una carpeta `Services` en el proyecto `helloworld.Android`. Después, cree un archivo de origen C# en él y cambie su nombre a `MicrophoneService.cs`, y copie y pegue el siguiente fragmento de código en ese archivo.

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-xamarin/helloworld/helloworld.Android/Services/MicrophoneService.cs)]

1. Después, abra `AndroidManifest.xml` en la carpeta `Properties` y agregue el siguiente valor de los permisos de usuario para el micrófono entre `<manifest>` y `</manifest>`.
```xml
   <uses-permission android:name="android.permission.RECORD_AUDIO" />
```

## <a name="add-sample-code-for-the-helloworldios-project"></a>Adición de código de ejemplo al proyecto `helloworld.iOS`

Ahora, agregue el código de C# que define la parte específica de iOS de la aplicación y también crea configuraciones específicas del dispositivo de Apple para el proyecto helloworld.iOS.

1. En el **Explorador de soluciones**, en el proyecto `helloworld.iOS`, abra `AppDelegate.cs`.

1. Reemplace todo el código que contiene por el fragmento siguiente:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-xamarin/helloworld/helloworld.iOS/AppDelegate.cs)]

1. Después, agregue la implementación específica de iOS para `MicrophoneService` mediante la creación de una carpeta `Services` en el proyecto `helloworld.iOS`. Después, cree un archivo de origen C# en él y cambie su nombre a `MicrophoneService.cs`, y copie y pegue el siguiente fragmento de código en ese archivo.

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-xamarin/helloworld/helloworld.iOS/Services/MicrophoneService.cs)]

1. Abra el archivo Info.plist en el proyecto `helloworld.iOS` en un editor de texto de texto y agregue el siguiente par de clave-valor en la sección dict <key>NSMicrophoneUsageDescription</key>
   <string>Esta aplicación de ejemplo requiere acceso al micrófono</string>
   > Nota: En caso de que pretenda compilar para un dispositivo iPhone, asegúrese de que `Bundle Identifier` coincide con el identificador de la aplicación del perfil de aprovisionamiento del dispositivo, ya que, de lo contrario, se producirá un error en la compilación. Con iPhoneSimulator puede dejarlo tal cual.

1. Si va a compilar en un PC Windows, debe establecer conexión con un dispositivo Mac para compilar mediante **Tools** (Herramientas) > **iOS** > **Pair to Mac** (Emparejar con Mac). Siga el Asistente para instrucciones de Visual Studio para habilitar la conexión con el dispositivo Mac.

## <a name="add-sample-code-for-the-helloworlduwp-project"></a>Adición de código de ejemplo al proyecto `helloworld.UWP`

Ahora, agregue el código C# que define la parte específica de UWP de la aplicación.

1. En el **Explorador de soluciones**, en el proyecto `helloworld.UWP`, abra `MainPage.xaml.cs`.

1. Reemplace todo el código que contiene por el fragmento siguiente:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-xamarin/helloworld/helloworld.UWP/MainPage.xaml.cs)]

1. Después, agregue la implementación específica de UWP para `MicrophoneService` mediante la creación de una carpeta `Services` en el proyecto `helloworld.UWP`. Después, cree un archivo de origen C# en él y cambie su nombre a `MicrophoneService.cs`, y copie y pegue el siguiente fragmento de código en ese archivo.

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-xamarin/helloworld/helloworld.UWP/Services/MicrophoneService.cs)]

1. Luego, haga doble clic en el archivo `Package.appxmanifest` del proyecto `helloworld.UWP` en Visual Studio y compruebe que en **Funcionalidades** > **Micrófono** está marcado y guarde el archivo.
   > Nota: Si ve la siguiente advertencia: Certificate file does not exist: helloworld.UWP_TemporaryKey.pfx, please check [speech to text](quickstart-csharp-uwp.md) sample for more information (El archivo de certificado no existe: helloworld.UWP_TemporaryKey.pfx, consulte el ejemplo de conversión de voz en texto para más información).

1. En la barra de menús, elija **Archivo** > **Guardar todo** para guardar los cambios.

## <a name="build-and-run-the-uwp-application"></a>Compilación y ejecución de la aplicación UWP

1. Establezca `helloworld.UWP` como proyecto de inicio y haga clic con el botón derecho en el proyecto `helloworld.UWP` y elija **Compilar** para compilar la aplicación. 

1. Elija **Depurar** > **Iniciar depuración** o presione **F5** para iniciar la aplicación. Aparece la ventana **HelloWorld**.

   ![Ejemplo de aplicación de reconocimiento de voz de UWP en C#: inicio rápido](media/sdk/qs-csharp-xamarin-helloworld-uwp-window.png)

1. Seleccione **Habilitar micrófono** y, cuando aparezca la solicitud de permiso de acceso, seleccione **Sí**.

   ![Solicitud de permiso de acceso al micrófono](media/sdk/qs-csharp-xamarin-uwp-access-prompt.png)

1. Seleccione **Iniciar reconocimiento de voz**  y diga una frase en inglés en el micrófono del dispositivo. Lo que diga se transmitirá a los servicios de voz y se transcribirá en texto, que aparece en la misma ventana.

   ![Interfaz de usuario del reconocimiento de voz](media/sdk/qs-csharp-xamarin-uwp-ui-result.png)

## <a name="build-and-run-the-android-and-ios-applications"></a>Compilación y ejecución de aplicaciones de iOS y Android

La compilación y ejecución de aplicaciones de iOS y Android en el dispositivo o en el simulador se producen de manera similar a como lo hacen en UWP. Es importante asegurarse de que todos los SDK requeridos en la sección `Prerequisites` de este documento están instalados correctamente.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Exploración de ejemplos de C# en GitHub](https://aka.ms/csspeech/samples)
