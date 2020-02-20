---
title: 'Acerca del SDK de Voz: servicio de voz'
titleSuffix: Azure Cognitive Services
description: El kit de desarrollo de software (SDK) del servicio Voz proporciona a sus aplicaciones acceso nativo a las funciones del servicio Voz, lo que facilita el desarrollo de software. En este artículo se proporcionan detalles adicionales sobre el SDK para Windows, Linux y Android.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/13/2020
ms.author: dapine
ms.openlocfilehash: 04546883e6742086fa05fc450920b1026702bd08
ms.sourcegitcommit: 0eb0673e7dd9ca21525001a1cab6ad1c54f2e929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2020
ms.locfileid: "77212520"
---
# <a name="about-the-speech-sdk"></a>Acerca del SDK de Voz

El kit de desarrollo de software (SDK) de voz proporciona a sus aplicaciones acceso a las funciones del servicio Voz, lo que facilita el desarrollo de software habilitado para la voz. Actualmente, los SDK proporcionan acceso a **voz a texto**, **texto a voz**, **traducción de voz**, **reconocimiento de intenciones** y al **canal Direct Line Speech de Bot Framework**.

Puede capturar audio fácilmente desde un micrófono, leer de una secuencia o acceder a archivos de audio desde el almacenamiento con el SDK de voz. El SDK de voz admite WAV/PCM de 16 bits, 16 kHz u 8 kHz y audio de un solo canal para el reconocimiento de voz. Los formatos de audio adicionales se admiten mediante el [punto de conexión de REST de voz a texto](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) o el [servicio de transcripción por lotes](https://docs.microsoft.com/azure/cognitive-services/speech-service/batch-transcription#supported-formats).

Puede encontrar información general sobre las funcionalidades y las plataformas admitidas en la [página de entrada](https://aka.ms/csspeech) de la documentación.

[!INCLUDE [Speech SDK Platforms](../../../includes/cognitive-services-speech-service-speech-sdk-platforms.md)]

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

## <a name="get-the-sdk"></a>Obtención del SDK

# <a name="windows"></a>[Windows](#tab/windows)

> [!WARNING]
> El SDK de voz admite Windows 10 o versiones posteriores. Las versiones anteriores de Windows **no se admiten**.

Para Windows, se admiten los siguientes lenguajes:

* C# (UWP y .NET), C++: Puede usar y hacer referencia a la versión más reciente del paquete de NuGet del SDK de Voz. El paquete incluye bibliotecas de cliente de 32 y de 64 bits, así como bibliotecas de cliente y administradas (.NET). El SDK se puede instalar en Visual Studio mediante NuGet, [Microsoft.CognitiveServices.Speech](https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech).

* Java: Puede usar y hacer referencia a la versión más reciente de nuestro paquete de Maven del SDK de Voz, que solo admite Windows x64. En el proyecto de Maven, agregue `https://csspeechstorage.blob.core.windows.net/maven/` como un repositorio adicional y la referencia `com.microsoft.cognitiveservices.speech:client-sdk:1.8.0` como una dependencia.

# <a name="linux"></a>[Linux](#tab/linux)

> [!NOTE]
> Actualmente, solo se admite Ubuntu 16.04, Ubuntu 18.04 y Debian 9 en las siguientes arquitecturas de destino:
> - x86, x64 y ARM64 para desarrollo de C++
> - x64 y ARM64 para Java
> - x64 para .NET Core y Python

Asegúrese de que tiene instaladas las bibliotecas necesarias. Para ello, ejecute los siguientes comandos de shell:

En Ubuntu:

```sh
sudo apt-get update
sudo apt-get install libssl1.0.0 libasound2
```

En Debian 9:

```sh
sudo apt-get update
sudo apt-get install libssl1.0.2 libasound2
```

* C#: Puede usar y hacer referencia a la versión más reciente del paquete de NuGet del SDK de Voz. Para hacer referencia a la SDK, agregue la siguiente referencia de paquete en el proyecto:

  ```xml
  <PackageReference Include="Microsoft.CognitiveServices.Speech" Version="1.8.0" />
  ```

* Java: Puede usar y hacer referencia a la versión más reciente del paquete de Maven del SDK de Voz. En el proyecto de Maven, agregue `https://csspeechstorage.blob.core.windows.net/maven/` como un repositorio adicional y la referencia `com.microsoft.cognitiveservices.speech:client-sdk:1.7.0` como una dependencia.

* C++: Descargue el SDK como un [paquete .tar](https://aka.ms/csspeech/linuxbinary) y descomprima los archivos en el directorio que prefiera. En la tabla siguiente se muestra la estructura de carpetas del SDK:

  |Path|Descripción|
  |-|-|
  |`license.md`|Licencia|
  |`ThirdPartyNotices.md`|Avisos de terceros|
  |`include`|Archivos de encabezado para C y C++|
  |`lib/x64`|Biblioteca x64 nativa para vincular con la aplicación|
  |`lib/x86`|Biblioteca x86 nativa para vincular con la aplicación|

  Para crear una aplicación, copie o mueva los binarios (y bibliotecas) necesarios a su entorno de desarrollo. Inclúyalos según sea necesario en el proceso de compilación.

# <a name="android"></a>[Android](#tab/android)

El SDK de Java para Android está empaquetado como una [biblioteca de Android (AAR)](https://developer.android.com/studio/projects/android-library), que incluye las bibliotecas necesarias, así como los permisos necesarios de Android. Se hospeda en un repositorio de Maven en `https://csspeechstorage.blob.core.windows.net/maven/` como un paquete `com.microsoft.cognitiveservices.speech:client-sdk:1.7.0`.

Para consumir el paquete desde el proyecto de Android Studio, haga los siguientes cambios:

* En el archivo build.gradle de nivel de proyecto, agregue lo siguiente a la sección `repository`:

  ```gradle
  maven { url 'https://csspeechstorage.blob.core.windows.net/maven/' }
  ```

* En el archivo build.gradle de nivel de módulo, agregue lo siguiente a la sección `dependencies`:

  ```gradle
  implementation 'com.microsoft.cognitiveservices.speech:client-sdk:1.7.0'
  ```

El SDK de Java es parte del [SDK de dispositivos de voz](speech-devices-sdk.md).

---

[!INCLUDE [Get the samples](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]

## <a name="next-steps"></a>Pasos siguientes

* [Obtenga su suscripción de prueba a Voz](https://azure.microsoft.com/try/cognitive-services/)
* [Vea cómo funciona el reconocimiento de voz en C#](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=dotnet)
