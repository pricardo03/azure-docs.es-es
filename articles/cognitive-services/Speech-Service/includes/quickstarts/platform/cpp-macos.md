---
title: 'Inicio rápido: Configuración de la plataforma del SDK de Voz para C++ (macOS): servicio Voz'
titleSuffix: Azure Cognitive Services
description: Use esta guía para configurar la plataforma para utilizar C++ en macOS con el SDK de servicios de voz.
services: cognitive-services
author: markamos
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 10/14/2019
ms.author: erhopf
ms.openlocfilehash: 2c6a186eef631372f08b876083dacede31cf1077
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73504471"
---
En esta guía se muestra cómo instalar el [SDK de Voz](~/articles/cognitive-services/speech-service/speech-sdk.md) para C++ en macOS 10.13 y versiones superiores.

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

## <a name="system-requirements"></a>Requisitos del sistema

macOS 10.13 y versiones superiores

## <a name="install-speech-sdk"></a>Instalación del SDK de Voz

1. Seleccione el directorio al que desea extraer los archivos del SDK de Voz y configure la variable de entorno `SPEECHSDK_ROOT` para que apunte a ese directorio. Esta variable facilita la referencia al directorio en futuros comandos. Por ejemplo, si desea usar el directorio `speechsdk` en el directorio principal, use un comando similar al siguiente:

   ```sh
   export SPEECHSDK_ROOT="$HOME/speechsdk"
   ```

1. Cree el directorio si aún no existe.

   ```sh
   mkdir -p "$SPEECHSDK_ROOT"
   ```

1. Descargue y extraiga el archivo `.zip` que contiene el marco del SDK de Voz:

   ```sh
   wget -O SpeechSDK-macOS.zip https://aka.ms/csspeech/macosbinary
   unzip SpeechSDK-macOS.zip -d "$SPEECHSDK_ROOT"
   ```

1. Valide el contenido del directorio de nivel superior del paquete extraído:

   ```sh
   ls -l "$SPEECHSDK_ROOT"
   ```

   La lista de directorios debe contener los archivos de licencia y aviso de otro fabricante, así como un directorio `MicrosoftCognitiveServicesSpeech.framework`.

Ahora puede continuar con la sección [Pasos siguientes](#next-steps).

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [windows](../quickstart-list.md)]
