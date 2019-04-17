---
title: 'Inicio rápido: Reconocimiento de voz, C++ (Linux) (Speech Service)'
titleSuffix: Azure Cognitive Services
description: Aprenda a reconocer la voz en C++ para Linux mediante el SDK de Voz
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 2/20/2019
ms.author: wolfma
ms.openlocfilehash: d504e3abe148f1f607d8ee359e182832ec7d19f6
ms.sourcegitcommit: e43ea344c52b3a99235660960c1e747b9d6c990e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/04/2019
ms.locfileid: "59010318"
---
# <a name="quickstart-recognize-speech-in-c-on-linux-by-using-the-speech-sdk"></a>Inicio rápido: Reconocimiento de voz en C++ en Linux mediante el SDK de Voz

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

En este artículo, va a crear una aplicación de consola de C++ para Ubuntu Linux 16.04 o 18.04. Va a utilizar el [SDK de Voz](speech-sdk.md) de Cognitive Services para transcribir la conversión de voz en texto en tiempo real desde el micrófono de un equipo. La aplicación se crea con el [SDK de Voz para Linux](https://aka.ms/csspeech/linuxbinary) y con el compilador de C++ de la distribución de Linux (por ejemplo, `g++`).

## <a name="prerequisites"></a>Requisitos previos

Necesita una clave de suscripción de servicios de voz para completar este inicio rápido. Puede obtener una gratis. Para más detalles, consulte [Prueba gratuita de los servicios de voz](get-started.md).

## <a name="install-speech-sdk"></a>Instalación del SDK de Voz

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

La versión actual del SDK de Speech de Cognitive Services es `1.4.0`.

El SDK de Voz para Linux se puede usar para crear aplicaciones de 64 bits y 32 bits. Las bibliotecas y los archivos de encabezado necesarios se pueden descargar como un archivo tar desde https://aka.ms/csspeech/linuxbinary.

Descargue e instale el SDK de la forma siguiente:

1. Asegúrese de que se instalan las dependencias del SDK.

   ```sh
   sudo apt-get update
   sudo apt-get install build-essential libssl1.0.0 libasound2 wget
   ```

1. Seleccione el directorio al que desea extraer los archivos del SDK de Voz y configure la variable de entorno `SPEECHSDK_ROOT` para que apunte a ese directorio. Esta variable facilita la referencia al directorio en futuros comandos. Por ejemplo, si desea usar el directorio `speechsdk` en el directorio principal, use un comando similar al siguiente:

   ```sh
   export SPEECHSDK_ROOT="$HOME/speechsdk"
   ```

1. Cree el directorio si aún no existe.

   ```sh
   mkdir -p "$SPEECHSDK_ROOT"
   ```

1. Descargue y extraiga el archivo `.tar.gz` que contienen los archivos binarios del SDK de Voz:

   ```sh
   wget -O SpeechSDK-Linux.tar.gz https://aka.ms/csspeech/linuxbinary
   tar --strip 1 -xzf SpeechSDK-Linux.tar.gz -C "$SPEECHSDK_ROOT"
   ```

1. Valide el contenido del directorio de nivel superior del paquete extraído:

   ```sh
   ls -l "$SPEECHSDK_ROOT"
   ```

   La lista de directorios debe contener los archivos de avisos y licencias de terceros, así como un directorio `include` que contenga archivos de encabezado (`.h`) y un directorio `lib` que contenga bibliotecas.

   [!INCLUDE [Linux Binary Archive Content](../../../includes/cognitive-services-speech-service-linuxbinary-content.md)]

## <a name="add-sample-code"></a>Adición de código de ejemplo

1. Cree un archivo de código fuente C++ denominado `helloworld.cpp` y pegue el siguiente código en él.

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp-linux/helloworld.cpp#code)]

1. En este nuevo archivo, reemplace la cadena `YourSubscriptionKey` por su clave de suscripción de servicios de voz.

1. Reemplace la cadena `YourServiceRegion` por la [región](regions.md) asociada a sus suscripción (por ejemplo, `westus` para la suscripción de evaluación gratuita).

## <a name="build-the-app"></a>Compilación de la aplicación

> [!NOTE]
> Asegúrese de introducir los siguientes comandos como una _única línea de comandos_. La forma más fácil de hacerlo es copiar el comando usando el botón **Copiar** junto a cada comando, y luego pegarlo en el símbolo del shell.

* En un sistema **x64** (64 bits), ejecute el siguiente comando para crear la aplicación.

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x64" -l:libssl.so.1.0.0 -l:libasound.so.2
  ```

* En un sistema **x86** (32 bits), ejecute el siguiente comando para crear la aplicación.

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x86" -l:libssl.so.1.0.0 -l:libasound.so.2
  ```

## <a name="run-the-app"></a>Ejecución de la aplicación

1. Configuración de la ruta de acceso de la biblioteca del cargador para que apunte a la biblioteca del SDK de Voz.

   * En un sistema **x64** (64 bits), escriba el siguiente comando.

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x64"
     ```

   * En un sistema **x86** (32 bits), escriba el siguiente comando.

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x86"
     ```

1. Ejecute la aplicación.

   ```sh
   ./helloworld
   ```

1. En la ventana de consola, aparece un símbolo del sistema que solicita que se diga algo. Diga una oración o frase en inglés. Lo que diga se transmitirá a los servicios de voz y se transcribirá en texto, que aparece en la misma ventana.

   ```text
   Say something...
   We recognized: What's the weather like?
   ```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Exploración de ejemplos de C++ en GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Otras referencias

- [Personalización de modelos acústicos](how-to-customize-acoustic-models.md)
- [Personalización de modelos de lenguaje](how-to-customize-language-model.md)
