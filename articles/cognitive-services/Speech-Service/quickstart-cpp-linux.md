---
title: Inicio rápido de Speech SDK para C++ y Linux | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Obtenga información y ejemplos de código que le ayuden a empezar a usar Speech SDK con Linux y C++ en Cognitive Services rápidamente.
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: cee70ba585f93dda3249fc5b39f25fb613b57a45
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/25/2018
ms.locfileid: "36753631"
---
# <a name="quickstart-for-c-and-linux"></a>Inicio rápido para C++ y Linux

La versión actual de Cognitive Services Speech SDK es `0.4.0`.

Cognitive Services Speech SDK para Linux está disponible para la compilación de aplicaciones de 64 y 32 bits. Se pueden descargar los archivos necesarios como un archivo tar de https://aka.ms/csspeech/linuxbinary.

> [!NOTE]
> Si busca un inicio rápido para C++ y Windows, vaya [aquí](quickstart-cpp-windows.md).
> Si busca un inicio rápido para C# y Windows, vaya [aquí](quickstart-csharp-windows.md).

[!include[Get a Subscription Key](includes/get-subscription-key.md)]

> [!NOTE]
> En estas instrucciones se supone que la ejecución se realiza en Ubuntu 16.04 en un equipo (x86 o x64).
> En otra versión de Ubuntu o en una distribución de Linux diferente, tendrá que adaptar los pasos necesarios.

## <a name="prerequisites"></a>requisitos previos

[!include[Ubuntu Prerequisites](includes/ubuntu1604-prerequisites.md)]

## <a name="getting-the-binary-package"></a>Obtención del paquete binario

[!include[License Notice](includes/license-notice.md)]

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

   [!include[Linux Binary Archive Content](includes/linuxbinary-content.md)]

## <a name="sample-code"></a>Código de ejemplo

El código siguiente reconoce el inglés del micrófono.
Colóquelo en un archivo denominado `quickstart-linux.cpp`:

[!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/Linux/quickstart-linux/quickstart-linux.cpp#code)]

Reemplace la clave de suscripción en el código por la que haya obtenido.

## <a name="building"></a>Compilación

> [!NOTE]
> Asegúrese de copiar y pegar los comandos de compilación siguientes en una _única línea_.

* Ejecute el siguiente comando para compilar la aplicación en un equipo x64:

  ```sh
  g++ quickstart-linux.cpp -o quickstart-linux -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x64" -l:libssl.so.1.0.0 -l:libcurl.so.4 -l:libasound.so.2
  ```

* Ejecute el siguiente comando para compilar la aplicación en un equipo x86:

  ```sh
  g++ quickstart-linux.cpp -o quickstart-linux -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x86" -l:libssl.so.1.0.0 -l:libcurl.so.4 -l:libasound.so.2
  ```

## <a name="running"></a>En ejecución

Para ejecutar la aplicación, deberá configurar la ruta de acceso de la biblioteca del cargador para que apunte a la biblioteca de Speech SDK.

* En un equipo x64, ejecute:

  ```sh
  export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x64"
  ```

* En un equipo x86, ejecute:

  ```sh
  export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x86"
  ```

Ejecute la aplicación de la manera siguiente:

```sh
./quickstart-linux
```

Si todo es correcto, debería mostrarse una salida similar a esta:

```text
Say something...
We recognized: What's the weather
```

## <a name="downloading-the-sample"></a>Descarga del ejemplo

Para obtener el conjunto más reciente de ejemplos, vea el [repositorio de GitHub de ejemplos de Cognitive Services Speech SDK](https://aka.ms/csspeech/samples).

## <a name="next-steps"></a>Pasos siguientes

* Visite la [página de ejemplos](samples.md) para obtener más ejemplos.
