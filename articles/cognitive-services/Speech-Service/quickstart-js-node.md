---
title: 'Inicio rápido: Reconocimiento de voz, Node.js: servicios de Voz'
titleSuffix: Azure Cognitive Services
description: Use esta guía para crear una aplicación de consola de conversión de voz en texto mediante el SDK de Voz para Node.js. Cuando termine, puede usar el micrófono del equipo para realizar la conversión de voz en texto en tiempo real.
services: cognitive-services
author: fmegen
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 1/16/2019
ms.author: fmegen
ms.openlocfilehash: e0ae916687ca32835dd8daf6e5059b8f6eea0ff6
ms.sourcegitcommit: ba9f95cf821c5af8e24425fd8ce6985b998c2982
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2019
ms.locfileid: "54382165"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-nodejs"></a>Inicio rápido: Reconocimiento de voz con el SDK de Voz para Node.js

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

En este artículo, aprenderá a crear un proyecto de Node.js mediante el enlace de JavaScript del SDK de Voz de Cognitive Services para transcribir voz a texto.
La aplicación se basa en el [SDK de Voz de Microsoft Cognitive Services](https://aka.ms/csspeech/npmpackage).

## <a name="prerequisites"></a>Requisitos previos

* Una clave de suscripción de Azure para el servicio Voz. [Obtenga una gratis](get-started.md).
* Una versión actual de [Node.js](https://nodejs.org).

## <a name="create-a-new-project"></a>Creación de un nuevo proyecto

Cree una carpeta e inicialice el proyecto.

```sh
npm init -f
```

Esto inicializará los archivos package.json con valores predeterminados. Probablemente deseará editar este archivo más adelante.

## <a name="install-the-speech-sdk"></a>Instalación de Speech SDK

Agregue el SDK de Voz al proyecto de Node.js.

```
npm install microsoft-cognitiveservices-speech-sdk
```

Esto descarga e instala la versión más reciente del SDK de Voz y todos los requisitos previos necesarios de npmjs. El SDK se instalará en el directorio `node_modules` de la carpeta del proyecto.

## <a name="use-the-speech-sdk"></a>Uso del SDK de Voz

Cree un nuevo archivo llamado `index.js` en la carpeta y ábralo con un editor de texto.

> [!NOTE]
> Tenga en cuenta que en Node.js el SDK de Voz no admite los tipos de datos de micrófono o archivo. Estos solo se admiten en los exploradores web. En su lugar, utilice la interfaz de Stream para el SDK de Voz, ya sea a través de `AudioInputStream.createPushStream()` o `AudioInputStream.createPullStream()`.

En este ejemplo, usaremos la interfaz `PushAudioInputStream`.

Agregue el siguiente código JavaScript:

[!code-javascript[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/js-node/index.js#code)]

## <a name="run-the-sample"></a>Ejecución del ejemplo

Para iniciar la aplicación, adapte `YourSubscriptionKey`, `YourServiceRegion` y `YourAudioFile.wav` a la configuración. A continuación, puede ejecutarla mediante una llamada al siguiente comando:

```sh
node index.js
```

Esto desencadenará un reconocimiento mediante el nombre de archivo proporcionado y presentará la salida en la consola.

Este es un ejemplo de salida que resulta de ejecutar `index.js` después de actualizar la clave de suscripción y de usar el archivo `whatstheweatherlike.wav`.

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

1. Inicie Visual Studio Code y haga clic en "Abrir carpeta" y, a continuación, desplácese a la carpeta de inicio rápido

   ![Captura de pantalla de Abrir carpeta](media/sdk/qs-js-node-01-open_project.png)

1. En Visual Studio Code, abra una ventana de terminal.

   ![Captura de pantalla de la ventana de terminal](media/sdk/qs-js-node-02_open_terminal.png)

1. Ejecute npm para instalar las dependencias

   ![Captura de pantalla de instalación de npm](media/sdk/qs-js-node-03-npm_install.png)

1. Ahora ya puede abrir `index.js` y establecer un punto de interrupción

   ![Captura de pantalla de index.js con un punto de interrupción en la línea 16](media/sdk/qs-js-node-04-setup_breakpoint.png)

1. Para iniciar la depuración, presione la tecla F5 o seleccione Depurar/Iniciar depuración en el menú

   ![Captura de pantalla del menú de depuración](media/sdk/qs-js-node-05-start_debugging.png)

1. Cuando se alcanza un punto de interrupción, puede inspeccionar la pila de llamadas y las variables

   ![Captura de pantalla del depurador](media/sdk/qs-js-node-06-hit_breakpoint.png)

1. Se mostrarán todas las salidas en la ventana de la consola de depuración

   ![Captura de pantalla de la consola de depuración](media/sdk/qs-js-node-07-debug_output.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Explorar ejemplos de Node.js en GitHub](https://aka.ms/csspeech/samples)
