---
title: Introducción a Bing Speech Recognition API con la biblioteca de escritorio de C#| Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Desarrollo de aplicaciones básicas de Windows que usan Bing Speech Recognition API para convertir audio hablado en texto.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ROBOTS: NOINDEX
ms.openlocfilehash: 5b85a548d692b06a6da009432678d9c02242cb50
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46996832"
---
# <a name="quickstart-use-the-bing-speech-recognition-api-in-c35-for-net-on-windows"></a>Guía de inicio rápido: Uso de Bing Speech Recognition API en C&#35; para .NET en Windows

Esta página muestra cómo desarrollar una aplicación básica de Windows que usa Speech Recognition API para convertir audio hablado en texto. El uso de la biblioteca cliente permite streaming en tiempo real, lo que significa que cuando la aplicación cliente envía el audio al servicio, al mismo tiempo y de forma asincrónica recibe los resultados parciales del reconocimiento.

Los desarrolladores que quieran usar el servicio Voz desde aplicaciones que se ejecutan en cualquier dispositivo pueden usar la biblioteca de escritorio de C#. Para usar la biblioteca, instale el [paquete NuGet Microsoft.ProjectOxford.SpeechRecognition-x86](https://www.nuget.org/packages/Microsoft.ProjectOxford.SpeechRecognition-x86/) para las plataformas de 32 bits y el [paquete de NuGet Microsoft.ProjectOxford.SpeechRecognition-x64](https://www.nuget.org/packages/Microsoft.ProjectOxford.SpeechRecognition-x64/) para las plataformas de 64 bits. Para ver la referencia de API de la biblioteca cliente, consulte el artículo sobre la [biblioteca de escritorio de C# del servicio Voz de Microsoft](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-Windows/master/docs/SpeechSDK/index.html).

En las secciones siguientes se describe cómo instalar, compilar y ejecutar la aplicación de ejemplo de C# mediante el uso de la biblioteca de escritorio de C#.

## <a name="prerequisites"></a>Requisitos previos

### <a name="platform-requirements"></a>Requisitos de la plataforma

El siguiente ejemplo se desarrolló para Windows 8+ y .NET Framework 4.5+ con [Visual Studio 2015, Community Edition](https://www.visualstudio.com/products/visual-studio-community-vs).

### <a name="get-the-sample-application"></a>Obtención de la aplicación de ejemplo

Clone el ejemplo del repositorio de [ejemplos de biblioteca de escritorio de C# de voz](https://github.com/microsoft/cognitive-speech-stt-windows).

### <a name="subscribe-to-the-speech-recognition-api-and-get-a-free-trial-subscription-key"></a>Suscripción a Speech Recognition API y obtención de una clave de suscripción de prueba gratuita

Speech API forma parte de Cognitive Services (anteriormente Project Oxford). Puede obtener las claves de la suscripción de prueba gratuita en la página [Suscripción a Cognitive Services](https://azure.microsoft.com/try/cognitive-services/). Después de seleccionar Speech API, seleccione **Obtener clave de API** para obtener la clave. Devuelve una clave principal y una clave secundaria. Las dos claves están asociadas a la misma cuota, por lo que puede usar cualquiera de las claves.

> [!IMPORTANT]
> * Obtenga una clave suscripción. Antes de usar las bibliotecas de cliente de voz, debe tener una [clave de suscripción](https://azure.microsoft.com/try/cognitive-services/).
>
> * Use la clave de su suscripción Con la aplicación de ejemplo de escritorio de C# que se proporciona, pegue la clave de suscripción en el cuadro de texto al ejecutar el ejemplo. Para más información, consulte [Ejecución de la aplicación de ejemplo](#step-3-run-the-sample-application).

## <a name="step-1-install-the-sample-application"></a>Paso 1: Instalar la aplicación de ejemplo

1. Inicie Visual Studio 2015 y seleccione **Archivo** > **Abrir** > **Proyecto/Solución**.

2. Vaya a la carpeta donde guardó los archivos descargados de Speech Recognition API. Seleccione **Speech** > **Windows** y, a continuación, seleccione la carpeta de Sample-WP.

3. Haga doble clic para abrir el archivo de solución de Visual Studio 2015 (.sln) llamado SpeechToText-WPF-Samples.sln. La solución se abre en Visual Studio.

## <a name="step-2-build-the-sample-application"></a>Paso 2: Compilación de la aplicación de ejemplo

1. Si desea usar el *reconocimiento con intenciones*, primero debe suscribirse a [Language Understanding Intelligent Service (LUIS)](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/). A continuación, use la dirección URL del punto de conexión de la aplicación de LUIS para establecer el valor de la clave `LuisEndpointUrl` en el archivo app.config, en la carpeta samples/SpeechRecognitionServiceExample. Para más información sobre la dirección URL del punto de conexión de la aplicación de LUIS, consulte [Publicación de la aplicación](../../luis/luis-get-started-create-app.md#publish-your-app).

   > [!TIP]
   > Reemplace el carácter `&` en el punto de conexión de dirección URL de la aplicación de LUIS por `&amp;` para asegurarse de que la dirección URL se interpreta correctamente el analizador XML.

2. Presione Ctrl+Mayús+B o seleccione **Compilar** en el menú de la cinta de opciones. Después, seleccione **Compilar solución**.

## <a name="step-3-run-the-sample-application"></a>Paso 3: Ejecución de la aplicación de ejemplo

1. Una vez finalizada la compilación, presione F5 o seleccione **Iniciar** en el menú de la cinta de opciones para ejecutar el ejemplo.

2. Vaya a la ventana **Project Oxford Speech to Text Sample**. Pegue la clave de suscripción en el cuadro de texto **Paste your subscription key here to start** (pegue la clave de suscripción aquí para comenzar), tal y como se muestra. Para conservar su clave de suscripción en su equipo portátil o PC, seleccione **Save Key** (Guardar clave). Para eliminar la clave de suscripción del sistema, seleccione **Eliminar clave** (Eliminar clave) para quitarla del equipo de escritorio o portátil.

   ![Pegar la clave de reconocimiento de voz](../Images/SpeechRecog_paste_key.PNG)

3. En **Speech Recognition Source** (Origen de reconocimiento de voz), elija uno de los seis orígenes de voz, que se dividen en dos categorías de entrada principales:

   * Usar el micrófono del equipo o un micrófono conectado para capturar la voz.
   * Reproducir un archivo de audio.

   Cada categoría tiene tres modos de reconocimiento:

    * **Modo ShortPhrase**: una expresión de hasta 15 segundos de duración. A medida que los datos se envían al servidor, el cliente recibe varios resultados parciales y un resultado final con las n opciones mejores.
    * **Modo LongDictation**: una expresión hasta dos minutos de duración. A medida que los datos se envían al servidor, el cliente recibe varios resultados parciales y varios resultados finales, en función de las pausas que indique el servidor en la oración.
    * **Detección de intenciones**: el servidor devuelve información adicional estructurada sobre la entrada de voz. Para utilizar la detección de intenciones, debe primero entrenar un modelo con [LUIS](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/).

Use archivos de audio de ejemplo con esta aplicación de ejemplo. Busque los archivos en el repositorio que se descargó con este ejemplo, en la carpeta samples/SpeechRecognitionServiceExample. Estos archivos de audio de ejemplo se ejecutan automáticamente si no se elige otro archivo al seleccionar **Use wav file for Shortphrase mode** (Usar archivo wav para el modo Shortphrase) o **Use wav file for Longdictation mode** (Usar archivo wav para el modo Longdictation) como entrada de voz. Actualmente, solo se admite el formato de audio WAV.

![Interfaz de Speech to Text](../Images/HelloJones.PNG)

## <a name="samples-explained"></a>Ejemplos explicados

### <a name="recognition-events"></a>Eventos de reconocimiento

* **Eventos de resultados parciales**: se llama a este evento cada vez que el servicio de voz predice lo que puede estar diciendo, incluso antes de que termine de hablar (si usa `MicrophoneRecognitionClient`) o termine de enviar datos (si usa `DataRecognitionClient`).
* **Eventos de error**: se llaman cuando el servicio detecta un error.
* **Eventos de intención**: se llaman en los clientes "WithIntent" (solo en el modo ShortPhrase) después de analizar el resultado final del reconocimiento en una intención JSON estructurada.
* **Eventos de resultados**:
  * En modo `ShortPhrase`, se llama a este evento y devuelve los n resultados mejores cuando termina de hablar.
  * En modo `LongDictation`, se llama al controlador de eventos varias veces, en función de dónde identifica el servicio las pausas en la frase.
  * **Para cada una de las n opciones mejores**, se devuelve un valor de confianza y unos cuantos formatos diferentes del texto reconocido. Para más información, consulte [Formato de salida](../Concepts.md#output-format).

Ya se apunta a los controladores de eventos en el código, en forma de comentarios de código.

## <a name="related-topics"></a>Temas relacionados

* [Referencia de la biblioteca de escritorio de voz de Microsoft](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-Windows/master/docs/SpeechSDK/index.html)
* [Introducción a Microsoft Speech Recognition API en Java en Android](GetStartedJavaAndroid.md)
* [Introducción a Microsoft Speech Recognition API en Objective-C en iOS](Get-Started-ObjectiveC-iOS.md)
* [Introducción a Microsoft Speech Recognition API en Javascript](GetStartedJSWebsockets.md)
* [Introducción a Microsoft Speech Recognition API con REST](GetStartedREST.md)
