---
title: Acerca del SDK de Voz de Cognitive Services | Microsoft Docs
description: Información general sobre los SDK disponibles para el servicio Voz.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: b9b7b8af5ce3d75788fd2c4f5e0309b5ca561a8f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35382869"
---
# <a name="about-the-cognitive-services-speech-sdk"></a>Acerca del SDK de Voz de Cognitive Services

El kit de desarrollo de software (SDK) de Voz de Cognitive Services proporciona a sus aplicaciones acceso a las funciones del servicio de Voz, lo que facilita el desarrollo de software. Actualmente, el SDK proporciona acceso a **conversión de voz en texto**, **traducción de voz** y **reconocimiento de la intención**.

En la tabla se describen los sistemas operativos y lenguajes de programación admitidos actualmente.

|Sistema operativo compatible|Lenguaje de programación|
|-|-|
|Windows|C/C++, C#|
|Linux|C/C++|
|Dispositivos|Java\*|

\* *El SDK de Java es parte del [SDK de dispositivos de voz](speech-devices-sdk.md).*

## <a name="get-the-windows-sdk"></a>Obtención de Windows SDK

La versión de Windows del SDK de Voz incluye bibliotecas cliente de C/C++ de 32 y 64 bits, así como bibliotecas administradas (.NET) para su uso con C#. El SDK se puede instalar en Visual Studio mediante NuGet; busque simplemente `Microsoft.CognitiveServices.Speech`.

## <a name="get-the-linux-sdk"></a>Obtener el SDK de Linux

Asegúrese de que tiene el compilador y las bibliotecas necesarios. Para ello, ejecute los siguientes comandos de shell:

```sh
sudo apt-get update
sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2
```

> [!NOTE]
> En estas instrucciones se supone que se está ejecutando Ubuntu 16.04 en un equipo (x86 o x64). En otra versión de Ubuntu o en una distribución de Linux diferente, adapte estos pasos a su entorno.

A continuación, [descargue el SDK](https://aka.ms/csspeech/linuxbinary) y descomprima los archivos en un directorio de su elección. En esta tabla se muestra la estructura de carpetas del SDK.

|Ruta de acceso|DESCRIPCIÓN|
|-|-|
|`license.md`|Licencia|
|`third-party-notices.md`|Avisos de terceros|
|`include`|Archivos de encabezado para C y C++|
|`lib/x64`|Biblioteca x64 nativa para vincular con la aplicación|
|`lib/x86`|Biblioteca x86 nativa para vincular con la aplicación|

Para crear una aplicación, copie o mueva los binarios (y bibliotecas) necesarios a su entorno de desarrollo e inclúyalos según sea necesario en su proceso de compilación.

## <a name="get-the-java-sdk"></a>Obtener el SDK de Java

El SDK de Java es parte del [SDK de dispositivos de voz](speech-devices-sdk.md).

## <a name="next-steps"></a>Pasos siguientes

* [Obtenga su suscripción de prueba a Speech](https://azure.microsoft.com/try/cognitive-services/)
* [Ver cómo funciona el reconocimiento de voz en C#](quickstart-csharp-windows.md)
