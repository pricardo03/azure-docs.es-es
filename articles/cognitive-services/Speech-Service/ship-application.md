---
title: 'Desarrollo de aplicaciones con el SDK de Voz: servicios de Voz'
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo crear aplicaciones con el SDK de Voz.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: wolfma
ms.custom: seodec18
ms.openlocfilehash: d8330ff663a7adc2f1cae3ed6bdf1cec2fa972c5
ms.sourcegitcommit: f9448a4d87226362a02b14d88290ad6b1aea9d82
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66808458"
---
# <a name="ship-an-application"></a>Distribuir una aplicación

Respete la [licencia de Speech SDK](https://aka.ms/csspeech/license201809), así como los [avisos de software de terceros](https://csspeechstorage.blob.core.windows.net/drop/1.0.0/ThirdPartyNotices.html), cuando distribuya Speech SDK de Azure Cognitive Services. Además revise la [Declaración de privacidad de Microsoft](https://aka.ms/csspeech/privacy).

En función de la plataforma, existen otras dependencias para ejecutar la aplicación.

## <a name="windows"></a>Windows

El SDK de Voz de Cognitive Services se ha probado en Windows 10 y en Windows Server 2016.

Cognitive Services Speech SDK requiere la [Microsoft Visual C++ Redistributable para Visual Studio de 2019](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) en el sistema. Puede descargar los instaladores para la versión más reciente de `Microsoft Visual C++ Redistributable for Visual Studio 2019` aquí:

- [Win32](https://aka.ms/vs/16/release/vc_redist.x86.exe)
- [x64](https://aka.ms/vs/16/release/vc_redist.x64.exe)

Si en la aplicación se usa código administrado, se necesita `.NET Framework 4.6.1` o una versión posterior en el equipo de destino.

Para la entrada de micrófono, las bibliotecas de Media Foundation deben estar instaladas. Estas bibliotecas forman parte de Windows 10 y Windows Server 2016. Es posible usar Speech SDK sin estas bibliotecas, siempre y cuando no se use un micrófono como dispositivo de entrada de audio.

Los archivos necesarios del SDK de Voz se pueden implementar en el mismo directorio que la aplicación. De esta forma la aplicación puede acceder directamente a las bibliotecas. Asegúrese de seleccionar la versión correcta (Win32/x64) que coincida con la aplicación.

| Name | Función
|:-----|:----|
| `Microsoft.CognitiveServices.Speech.core.dll` | SDK básico, necesario para la implementación nativa y administrada
| `Microsoft.CognitiveServices.Speech.csharp.dll` | Necesario para la implementación administrada

>[!NOTE]
> Comenzando con la versión 1.3.0 el archivo `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` (incluido en versiones anteriores) ya no es necesario. La funcionalidad está ahora integrada en el SDK de core.

## <a name="linux"></a>Linux

Speech SDK es compatible actualmente con las distribuciones de Ubuntu 16.04, 18.04 Ubuntu y Debian 9.
Para una aplicación nativa, se debe distribuir la biblioteca de Speech SDK, `libMicrosoft.CognitiveServices.Speech.core.so`.
Asegúrese de seleccionar la versión (x86, x64) que coincida con la aplicación. Según la versión de Linux, es posible que también tenga que incluir las dependencias siguientes:

* Las bibliotecas compartidas de la biblioteca GNU C (incluida la biblioteca de programación de subprocesos de POSIX, `libpthreads`)
* La biblioteca OpenSSL (`libssl.so.1.0.0` o `libssl.so.1.0.2`)
* La biblioteca compartida para las aplicaciones ALSA (`libasound.so.2`)

En Ubuntu, las bibliotecas de GNU C ya deberían estar instaladas de forma predeterminada. Las tres últimas se pueden instalar con estos comandos:

```sh
sudo apt-get update
sudo apt-get install libssl1.0.0 libasound2
```

En Debian 9 instale estos paquetes:

```sh
sudo apt-get update
sudo apt-get install libssl1.0.2 libasound2
```

## <a name="next-steps"></a>Pasos siguientes

* [Obtenga su suscripción de prueba a Voz](https://azure.microsoft.com/try/cognitive-services/)
* [Vea cómo funciona el reconocimiento de voz en C#](quickstart-csharp-dotnet-windows.md)
