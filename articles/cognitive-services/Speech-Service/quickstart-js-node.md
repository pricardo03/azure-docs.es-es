---
title: 'Inicio rápido: Reconocimiento de voz, Node.js: servicios de Voz'
titleSuffix: Azure Cognitive Services
description: Use esta guía para crear una aplicación de consola de conversión de voz en texto mediante el SDK de Voz para Node.js. Cuando termine, puede usar el micrófono del equipo para realizar la conversión de voz en texto en tiempo real.
services: cognitive-services
author: fmegen
manager: cgronlun
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 1/16/2019
ms.author: fmegen
ms.openlocfilehash: 99938f8b64339556c0ee4b9caa9124c609159af6
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2019
ms.locfileid: "55747288"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-nodejs"></a>Inicio rápido: Reconocimiento de voz con el SDK de Voz para Node.js

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

En este artículo, aprenderá a crear un proyecto de Node.js mediante el enlace de JavaScript del SDK de Voz de Azure Cognitive Services para transcribir voz a texto.
La aplicación se basa en el [SDK de Voz para JavaScript](https://aka.ms/csspeech/npmpackage).

## <a name="prerequisites"></a>Requisitos previos

* Una clave de suscripción de Azure para el servicio Voz. [Obtenga una gratis](get-started.md).
* Una versión actual de [Node.js](https://nodejs.org).

## <a name="create-a-new-project"></a>Creación de un nuevo proyecto

Cree una carpeta e inicialice el proyecto:

```sh
npm init -f
```

Este comando inicializa los archivos **package.json** con los valores predeterminados. Probablemente deseará editar este archivo más adelante.

## <a name="install-the-speech-sdk"></a>Instalación de Speech SDK

Agregue el SDK de Voz al proyecto de Node.js:

```
npm install microsoft-cognitiveservices-speech-sdk
```

Este comando descarga e instala la versión más reciente del SDK de Voz y todos los requisitos previos necesarios de **npmjs**. El SDK se instala en el directorio `node_modules` de la carpeta del proyecto.

## <a name="use-the-speech-sdk"></a>Uso del SDK de Voz

Cree un archivo en la carpeta llamado `index.js` y ábralo con un editor de texto.

> [!NOTE]
> En Node.js, el SDK de Voz no admite los tipos de datos de micrófono o **archivo**. Estos solo se admiten en los exploradores web. En su lugar, use la interfaz de **Stream** para el SDK de Voz, ya sea a través de `AudioInputStream.createPushStream()` o `AudioInputStream.createPullStream()`.

En este ejemplo, usaremos la interfaz `PushAudioInputStream`.

Agregue este código de JavaScript:

[!code-javascript[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/js-node/index.js#code)]

## <a name="run-the-sample"></a>Ejecución del ejemplo

Para abrir la aplicación, adapte `YourSubscriptionKey`, `YourServiceRegion` y `YourAudioFile.wav` a su configuración. A continuación, ejecútelo mediante la llamada a este comando:

```sh
node index.js
```

Este comando desencadena un reconocimiento mediante el nombre de archivo proporcionado y presenta la salida en la consola.

Este ejemplo es la salida cuando se ejecuta `index.js` después de actualizar la clave de suscripción y usar el archivo `whatstheweatherlike.wav`:

```json
SpeechRecognitionResult {
  "privResultId": "9E30EEBD41AC4571BB77CF9164441F46",
  "privReason": 3,
  "privText": "What's the weather like?",
  "privDuration": 15900000,
  "privOffset": 300000,
  "privErrorDetails": null,
  "privJson": {
    "RecognitionStatus": "Success",
    "DisplayText": "What's the weather like?",
    "Offset": 300000,
    "Duration": 15900000
  },
  "privProperties": null
}
```

## <a name="install-and-use-the-speech-sdk-with-visual-studio-code"></a>Instalación y uso del SDK de Voz con Visual Studio Code

También puede ejecutar el ejemplo desde Visual Studio Code. Siga estos pasos para instalar, abrir y ejecutar el inicio rápido:

1. Inicie Visual Studio Code. Seleccione **Open Folder** (Abrir carpeta). A continuación, vaya a la carpeta de inicio rápido.

   ![Abrir carpeta](media/sdk/qs-js-node-01-open_project.png)

1. En Visual Studio Code, abra una ventana de terminal.

   ![La ventana de terminal](media/sdk/qs-js-node-02_open_terminal.png)

1. Ejecute `npm` para instalar las dependencias.

   ![npm install](media/sdk/qs-js-node-03-npm_install.png)

1. Ahora ya puede abrir `index.js` y establecer un punto de interrupción.

   ![index.js con un punto de interrupción en la línea 16](media/sdk/qs-js-node-04-setup_breakpoint.png)

1. Para iniciar la depuración, presione F5 o seleccione **Debug/Start Debugging** (Depurar/Iniciar depuración) en el menú.

   ![El menú de depuración](media/sdk/qs-js-node-05-start_debugging.png)

1. Cuando se alcanza un punto de interrupción, puede inspeccionar la pila de llamadas y las variables.

   ![Depurador](media/sdk/qs-js-node-06-hit_breakpoint.png)

1. Las salidas se muestran en la ventana de la consola de depuración.

   ![Consola de depuración](media/sdk/qs-js-node-07-debug_output.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Explorar ejemplos de Node.js en GitHub](https://aka.ms/csspeech/samples)
