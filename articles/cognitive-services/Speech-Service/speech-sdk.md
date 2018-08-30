---
title: Acerca del SDK de Voz de Cognitive Services
description: Información general sobre los SDK disponibles para el servicio Voz.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 08/16/2018
ms.author: v-jerkin
ms.openlocfilehash: c26aeb1d29c3b2c8b5b43d1a1face818295e9d2f
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2018
ms.locfileid: "43122089"
---
# <a name="about-the-cognitive-services-speech-sdk"></a>Acerca del SDK de Voz de Cognitive Services

El kit de desarrollo de software (SDK) de Voz de Cognitive Services proporciona a sus aplicaciones acceso a las funciones del servicio de Voz, lo que facilita el desarrollo de software. Actualmente, el SDK proporciona acceso a **conversión de voz en texto**, **traducción de voz** y **reconocimiento de la intención**.

[!INCLUDE [Speech SDK Platforms](../../../includes/cognitive-services-speech-service-speech-sdk-platforms.md)]

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

## <a name="get-the-sdk"></a>Obtención del SDK

### <a name="windows"></a>Windows

Para Windows se admiten los siguientes lenguajes:

* C# (UWP y. NET), C++: puede usar y hacer referencia a la versión más reciente de nuestro paquete NuGet del SDK de Voz.
  El paquete incluye bibliotecas de cliente de 32 y de 64 bits, así como bibliotecas administradas (.NET).
  El SDK se puede instalar en Visual Studio mediante NuGet; busque simplemente `Microsoft.CognitiveServices.Speech`.

* Java: puede usar y hacer referencia a la versión más reciente de nuestro paquete Maven del SDK de Voz, que solo admite Windows x64.
  En el proyecto de Maven, agregue `https://csspeechstorage.blob.core.windows.net/maven/` como un repositorio adicional y la referencia `com.microsoft.cognitiveservices.speech:client-sdk:0.6.0` como una dependencia. 

### <a name="linux"></a>Linux

> [!NOTE]
> Actualmente, solo se admite Ubuntu 16.04 en un equipo (x86 o x64 para el desarrollo de C++, x64 para .NET Core y Java).

Asegúrese de que tiene instalados el compilador y las bibliotecas necesarios. Para ello, ejecute los siguientes comandos de shell:

```sh
sudo apt-get update
sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2
```

* C#: puede usar y hacer referencia a la versión más reciente de nuestro paquete NuGet del SDK de Voz.
  Para hacer referencia a la SDK, agregue la siguiente referencia de paquete en el proyecto:

  ```xml
  <PackageReference Include="Microsoft.CognitiveServices.Speech" Version="0.6.0" />
  ```

* Java: puede usar y hacer referencia a la versión más reciente de nuestro paquete Maven del SDK de Voz.
  En el proyecto de Maven, agregue `https://csspeechstorage.blob.core.windows.net/maven/` como un repositorio adicional y la referencia `com.microsoft.cognitiveservices.speech:client-sdk:0.6.0` como una dependencia. 

* C++: descargue el SDK como un [paquete .tar](https://aka.ms/csspeech/linuxbinary) y descomprima los archivos en el directorio que prefiera. En la tabla siguiente se muestra la estructura de carpetas del SDK.

  |Ruta de acceso|DESCRIPCIÓN|
  |-|-|
  |`license.md`|Licencia|
  |`ThirdPartyNotices.md`|Avisos de terceros|
  |`include`|Archivos de encabezado para C y C++|
  |`lib/x64`|Biblioteca x64 nativa para vincular con la aplicación|
  |`lib/x86`|Biblioteca x86 nativa para vincular con la aplicación|

  Para crear una aplicación, copie o mueva los binarios (y bibliotecas) necesarios a su entorno de desarrollo e inclúyalos según sea necesario en su proceso de compilación.

### <a name="android"></a>Android

El SDK de Java para Android está empaquetado como una [biblioteca de Android (AAR)](https://developer.android.com/studio/projects/android-library), que incluye las bibliotecas necesarias, así como los permisos necesarios de Android para usarla.
Se hospeda en un repositorio de Maven en `https://csspeechstorage.blob.core.windows.net/maven/` como un paquete `com.microsoft.cognitiveservices.speech:client-sdk:0.6.0`.
Para consumir el paquete desde el proyecto de Android Studio, haga los siguientes cambios:

* En el archivo `build.gradle` de nivel de proyecto, agregue lo siguiente a la sección `repository`:

  ```text
  maven { url 'https://csspeechstorage.blob.core.windows.net/maven/' }
  ```

* En el archivo `build.gradle` de nivel de módulo, agregue lo siguiente a la sección `dependencies`:

  ```text
  implementation 'com.microsoft.cognitiveservices.speech:client-sdk:0.6.0'
  ```

El SDK de Java es parte del [SDK de dispositivos de voz](speech-devices-sdk.md).

[!INCLUDE [Get the samples](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]

## <a name="next-steps"></a>Pasos siguientes

* [Obtenga su suscripción de prueba a Voz](https://azure.microsoft.com/try/cognitive-services/)
* [Vea cómo funciona el reconocimiento de voz en C#](quickstart-csharp-dotnet-windows.md)
