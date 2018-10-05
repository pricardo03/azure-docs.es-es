---
title: 'Azure Cognitive Services, documentación de la API de Speech SDK de Cognitive Services: tutoriales y referencia de la API'
description: Obtenga información cómo crear y desarrollar aplicaciones con el SDK de Voz de Cognitive Services.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: 4bfede8df88c64e795e33620650efb579f43ebba
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2018
ms.locfileid: "47404315"
---
# <a name="ship-an-application"></a>Distribuir una aplicación

Respete la [licencia de Speech SDK](https://aka.ms/csspeech/license201809), así como los [avisos de software de terceros](https://csspeechstorage.blob.core.windows.net/drop/1.0.0/ThirdPartyNotices.html), cuando distribuya Speech SDK de Azure Cognitive Services. Además revise la [Declaración de privacidad de Microsoft](https://aka.ms/csspeech/privacy).

En función de la plataforma, existen otras dependencias para ejecutar la aplicación.

## <a name="windows"></a>Windows

El SDK de Voz de Cognitive Services se ha probado en Windows 10 y en Windows Server 2016.

El SDK de Voz de Cognitive Services requiere [Microsoft Visual C++ Redistributable para Visual Studio 2017](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) en el sistema. Puede descargar los instaladores para la versión más reciente de `Microsoft Visual C++ Redistributable for Visual Studio 2017` aquí:

- [Win32](https://aka.ms/vs/15/release/vc_redist.x86.exe)
- [x64](https://aka.ms/vs/15/release/vc_redist.x64.exe)

Si en la aplicación se usa código administrado, se necesita `.NET Framework 4.6.1` o una versión posterior en el equipo de destino.

Para la entrada de micrófono, las bibliotecas de Media Foundation deben estar instaladas. Estas bibliotecas forman parte de Windows 10 y Windows Server 2016. Es posible usar Speech SDK sin estas bibliotecas, siempre y cuando no se use un micrófono como dispositivo de entrada de audio.

Los archivos necesarios del SDK de Voz se pueden implementar en el mismo directorio que la aplicación. De esta forma la aplicación puede acceder directamente a las bibliotecas. Asegúrese de seleccionar la versión correcta (Win32/x64) que coincida con la aplicación.

| NOMBRE | Función
|:-----|:----|
| `Microsoft.CognitiveServices.Speech.core.dll` | SDK básico, necesario para la implementación nativa y administrada
| `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` | Necesario para la implementación administrada
| `Microsoft.CognitiveServices.Speech.csharp.dll` | Necesario para la implementación administrada

## <a name="linux"></a>Linux

Para una aplicación nativa, se debe distribuir la biblioteca de Speech SDK, `libMicrosoft.CognitiveServices.Speech.core.so`.
Asegúrese de seleccionar la versión (x86, x64) que coincida con la aplicación. Según la versión de Linux, es posible que también tenga que incluir las dependencias siguientes:

* Las bibliotecas compartidas de la biblioteca GNU C (incluida la biblioteca de programación de subprocesos de POSIX, `libpthreads`)
* La biblioteca OpenSSL (`libssl.so.1.0.0`)
* La biblioteca cURL (`libcurl.so.4`)
* La biblioteca compartida para las aplicaciones ALSA (`libasound.so.2`)

En Ubuntu 16.04, por ejemplo, las bibliotecas de GNU C ya deberían estar instaladas de forma predeterminada. Las tres últimas se pueden instalar con estos comandos:

```sh
sudo apt-get update
sudo apt-get install libssl1.0.0 libcurl3 libasound2 wget
```

## <a name="next-steps"></a>Pasos siguientes

* [Obtenga su suscripción de prueba a Voz](https://azure.microsoft.com/try/cognitive-services/)
* [Vea cómo funciona el reconocimiento de voz en C#](quickstart-csharp-dotnet-windows.md)
