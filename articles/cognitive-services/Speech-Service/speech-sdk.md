---
title: Acerca del SDK de Voz - Servicios de voz
titleSuffix: Azure Cognitive Services
description: El kit de desarrollo de software (SDK) del servicio Voz proporciona a sus aplicaciones acceso nativo a las funciones del servicio Voz, lo que facilita el desarrollo de software. En este artículo se proporcionan detalles adicionales sobre el SDK para Windows, Linux y Android.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: wolfma
ms.openlocfilehash: 120dd4a6a342732f3e07d87786009779d23be829
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67606423"
---
# <a name="about-the-speech-sdk"></a>Acerca del SDK de Voz

El kit de desarrollo de software (SDK) de voz proporciona a sus aplicaciones acceso a las funciones del servicio Voz, lo que facilita el desarrollo de software habilitado para la voz. Actualmente, los SDK proporcionan acceso a **voz a texto**, **texto a voz**, **traducción de voz**, **reconocimiento de intenciones** y al **canal Direct Line Speech de Bot Framework**. Puede encontrar información general sobre las funcionalidades y las plataformas admitidas en la [página de entrada](https://aka.ms/csspeech) de la documentación.

[!INCLUDE [Speech SDK Platforms](../../../includes/cognitive-services-speech-service-speech-sdk-platforms.md)]

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

## <a name="get-the-sdk"></a>Obtención del SDK

### <a name="windows"></a>Windows

Para Windows, se admiten los siguientes lenguajes:

* C# (UWP y .NET), C++: Puede usar y hacer referencia a la versión más reciente del paquete de NuGet del SDK de Voz. El paquete incluye bibliotecas de cliente de 32 y de 64 bits, así como bibliotecas de cliente y administradas (.NET). El SDK se puede instalar en Visual Studio mediante NuGet. Busque **Microsoft.CognitiveServices.Speech**.

* Java: Puede usar y hacer referencia a la versión más reciente de nuestro paquete de Maven del SDK de Voz, que solo admite Windows x64. En el proyecto de Maven, agregue `https://csspeechstorage.blob.core.windows.net/maven/` como un repositorio adicional y la referencia `com.microsoft.cognitiveservices.speech:client-sdk:1.6.0` como una dependencia.

### <a name="linux"></a>Linux

> [!NOTE]
> Actualmente, solo se admite Ubuntu 16.04, Ubuntu 18.04 y Debian 9 en un equipo (x86 o x64 para el desarrollo en C++, y x64 para .NET Core, Java y Python).

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
  <PackageReference Include="Microsoft.CognitiveServices.Speech" Version="1.6.0" />
  ```

* Java: Puede usar y hacer referencia a la versión más reciente del paquete de Maven del SDK de Voz. En el proyecto de Maven, agregue `https://csspeechstorage.blob.core.windows.net/maven/` como un repositorio adicional y la referencia `com.microsoft.cognitiveservices.speech:client-sdk:1.6.0` como una dependencia.

* C++: Descargue el SDK como un [paquete .tar](https://aka.ms/csspeech/linuxbinary) y descomprima los archivos en el directorio que prefiera. En la tabla siguiente se muestra la estructura de carpetas del SDK:

  |Path|DESCRIPCIÓN|
  |-|-|
  |`license.md`|Licencia|
  |`ThirdPartyNotices.md`|Avisos de terceros|
  |`include`|Archivos de encabezado para C y C++|
  |`lib/x64`|Biblioteca x64 nativa para vincular con la aplicación|
  |`lib/x86`|Biblioteca x86 nativa para vincular con la aplicación|

  Para crear una aplicación, copie o mueva los binarios (y bibliotecas) necesarios a su entorno de desarrollo. Inclúyalos según sea necesario en el proceso de compilación.

### <a name="android"></a>Android

El SDK de Java para Android está empaquetado como una [biblioteca de Android (AAR)](https://developer.android.com/studio/projects/android-library), que incluye las bibliotecas necesarias, así como los permisos necesarios de Android. Se hospeda en un repositorio de Maven en `https://csspeechstorage.blob.core.windows.net/maven/` como un paquete `com.microsoft.cognitiveservices.speech:client-sdk:1.6.0`.

Para consumir el paquete desde el proyecto de Android Studio, haga los siguientes cambios:

* En el archivo build.gradle de nivel de proyecto, agregue lo siguiente a la sección `repository`:

  ```gradle
  maven { url 'https://csspeechstorage.blob.core.windows.net/maven/' }
  ```

* En el archivo build.gradle de nivel de módulo, agregue lo siguiente a la sección `dependencies`:

  ```gradle
  implementation 'com.microsoft.cognitiveservices.speech:client-sdk:1.6.0'
  ```

El SDK de Java es parte del [SDK de dispositivos de voz](speech-devices-sdk.md).

[!INCLUDE [Get the samples](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]

## <a name="next-steps"></a>Pasos siguientes

* [Obtenga su suscripción de prueba a Voz](https://azure.microsoft.com/try/cognitive-services/)
* [Vea cómo funciona el reconocimiento de voz en C#](quickstart-csharp-dotnet-windows.md)
