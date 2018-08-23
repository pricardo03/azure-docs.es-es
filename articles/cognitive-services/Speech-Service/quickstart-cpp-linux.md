---
title: 'Guía de inicio rápido: Reconocimiento de voz en C++ en Linux con Speech SDK de Cognitive Services'
titleSuffix: Microsoft Cognitive Services
description: Aprenda a reconocer la voz en C++ en Linux con el SDK de Voz de Cognitive Services
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 07/16/2018
ms.author: wolfma
ms.openlocfilehash: b8bc24123fe75e876f607b07e37423ae68d92ee4
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2018
ms.locfileid: "41929739"
---
# <a name="quickstart-recognize-speech-in-c-on-linux-using-the-speech-sdk"></a>Guía de inicio rápido: Reconocimiento de voz en C++ en Linux mediante el SDK de Voz

[!include[Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

En este artículo, aprenderá a crear una aplicación de consola de C++ en Linux (Ubuntu 16.04) mediante el SDK de Voz de Cognitive Services para convertir voz en texto.

## <a name="prerequisites"></a>Requisitos previos

* Clave de suscripción para Speech Service. Consulte [Pruebe Speech Service gratis](get-started.md).
* Un equipo Ubuntu 16.04 con un micrófono.
* Para instalar los paquetes necesarios para compilar y ejecutar este ejemplo, ejecute lo siguiente:

  ```sh
  sudo apt-get update
  sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2 wget
  ```

## <a name="get-the-speech-sdk"></a>Obtención del SDK de Voz

[!include[License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

La versión actual del SDK de Speech de Cognitive Services es `0.6.0`.

Cognitive Services Speech SDK para Linux está disponible para la compilación de aplicaciones de 64 y 32 bits.
Se pueden descargar los archivos necesarios como un archivo tar de https://aka.ms/csspeech/linuxbinary.
Descargue e instale el SDK de la forma siguiente:

1. Seleccione un directorio (ruta de acceso absoluta) donde desea colocar los archivos binarios y los encabezados de Speech SDK.
   Por ejemplo, seleccione la ruta de acceso `speechsdk` en su directorio particular:

   ```sh
   export SPEECHSDK_ROOT="$HOME/speechsdk"
   ```

1. Cree el directorio si aún no existe:

   ```sh
   mkdir -p "$SPEECHSDK_ROOT"
   ```

1. Descargue y extraiga el archivo `.tar.gz` con los archivos binarios de Speech SDK:

   ```sh
   wget -O SpeechSDK-Linux.tar.gz https://aka.ms/csspeech/linuxbinary
   tar --strip 1 -xzf SpeechSDK-Linux.tar.gz -C "$SPEECHSDK_ROOT"
   ```

1. Valide el contenido del directorio de nivel superior del paquete extraído:

   ```sh
   ls -l "$SPEECHSDK_ROOT"
   ```

   Debe mostrar el aviso de terceros y los archivos de licencia, así como un directorio `include` para los encabezados y un directorio `lib` para las bibliotecas.

   [!include[Linux Binary Archive Content](../../../includes/cognitive-services-speech-service-linuxbinary-content.md)]

## <a name="add-the-sample-code"></a>Incorporación del código de ejemplo

1. Agregue el código siguiente en un archivo denominado `helloworld.cpp`:

  [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp-linux/helloworld.cpp#code)]

1. Reemplace la cadena `YourSubscriptionKey` por la clave de suscripción.

1. Reemplace la cadena `YourServiceRegion` por la [región](regions.md) asociada a sus suscripción (por ejemplo, `westus` para la suscripción de evaluación gratuita).

## <a name="building"></a>Compilación

> [!NOTE]
> Asegúrese de copiar y pegar los comandos de compilación siguientes en una _única línea_.

* En una máquina **x64**, ejecute el siguiente comando para compilar la aplicación:

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x64" -l:libssl.so.1.0.0 -l:libcurl.so.4 -l:libasound.so.2
  ```

* En una máquina **x86**, ejecute el siguiente comando para compilar la aplicación:

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x86" -l:libssl.so.1.0.0 -l:libcurl.so.4 -l:libasound.so.2
  ```

## <a name="run-the-sample"></a>Ejecución del ejemplo

1. Configure la ruta de acceso de la biblioteca del cargador para que apunte a la biblioteca del SDK de Voz.

   * En una máquina **x64**, ejecute:

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x64"
     ```

   * En una máquina **x86**, ejecute:

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x86"
     ```

1. Ejecute la aplicación de la manera siguiente:

   ```sh
   ./helloworld
   ```

1. Debería mostrarse una salida similar a esta:

   ```text
   Say something...
   We recognized: What's the weather
   ```

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Busque este ejemplo en la carpeta `quickstart/cpp-linux`.

## <a name="next-steps"></a>Pasos siguientes

* [Obtener nuestros ejemplos](speech-sdk.md#get-the-samples)
