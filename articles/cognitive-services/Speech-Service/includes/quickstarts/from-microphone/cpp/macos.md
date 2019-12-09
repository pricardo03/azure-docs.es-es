---
title: 'Inicio rápido: Reconocimiento de la voz a través de un micrófono en C++ (macOS): servicio de voz'
titleSuffix: Azure Cognitive Services
description: Aprenda a reconocer la voz en C++ para macOS mediante el SDK de Voz.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: wolfma
ms.openlocfilehash: 885e853528eb0b20079e6a7739450687ed001b8e
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74818701"
---
## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar:

> [!div class="checklist"]
> * [Ha creado un recurso de Voz de Azure](../../../../get-started.md)
> * [Ha configurado el entorno de desarrollo](../../../../quickstarts/setup-platform.md?tabs=macos)
> * [Ha creado un proyecto de ejemplo vacío](../../../../quickstarts/create-project.md?tabs=macos)
> * Asegúrese de que tiene acceso a un micrófono para capturar el audio.

## <a name="add-sample-code"></a>Incorporación de código de ejemplo

1. Cree un archivo de código fuente C++ denominado `helloworld.cpp` y pegue el siguiente código en él.

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp/macos/from-microphone/helloworld.cpp#code)]

1. En este nuevo archivo, reemplace la cadena `YourSubscriptionKey` por su clave de suscripción del servicio Voz.

1. Reemplace la cadena `YourServiceRegion` por la [región](~/articles/cognitive-services/Speech-Service/regions.md) asociada a sus suscripción (por ejemplo, `westus` para la suscripción de evaluación gratuita).

> [!NOTE]
> El SDK de Voz se usará de forma predeterminada para reconocer el uso de en-us como idioma. Para más información sobre cómo elegir el idioma de origen, consulte [Especificación del idioma de origen para la conversión de voz a texto](../../../../how-to-specify-source-language.md).

## <a name="build-the-app"></a>Compilación de la aplicación

> [!NOTE]
> Asegúrese de introducir los siguientes comandos como una _única línea de comandos_. La forma más fácil de hacerlo es copiar el comando usando el botón **Copiar** junto a cada comando, y luego pegarlo en el símbolo del shell.

* Ejecute el siguiente comando para compilar la aplicación.

  ```sh
  g++ helloworld.cpp -o helloworld --std=c++14 -F${SPEECHSDK_ROOT} -framework MicrosoftCognitiveServicesSpeech
  ```

## <a name="run-the-app"></a>Ejecución de la aplicación

1. Configuración de la ruta de acceso de la biblioteca del cargador para que apunte a la biblioteca del SDK de Voz.

    ```sh
    export DYLD_FRAMEWORK_PATH="$DYLD_FRAMEWORK_PATH:$SPEECHSDK_ROOT"
    ```

1. Ejecute la aplicación.

   ```sh
   ./helloworld
   ```

1. En la ventana de consola, aparece un símbolo del sistema que solicita que se diga algo. Diga una oración o frase en inglés. Lo que diga se transmitirá al servicio de Voz y se transcribirá en texto, que aparece en la misma ventana.

   ```text
   Say something...
   We recognized: What's the weather like?
   ```

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [footer](./footer.md)]