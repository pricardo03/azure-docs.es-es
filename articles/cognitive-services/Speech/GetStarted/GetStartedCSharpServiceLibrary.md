---
title: Introducción a Microsoft Speech Recognition API con la biblioteca de servicios de C#| Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Utilice la biblioteca del servicio de reconocimiento de Bing Speech para convertir el idioma hablado en texto.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ms.openlocfilehash: e9b3d3207f5aca6cba3555ba2578b5c66b3bd193
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "49343698"
---
# <a name="quickstart-use-the-bing-speech-recognition-service-library-in-c35-for-net-windows"></a>Guía de inicio rápido: Uso de la biblioteca del servicio de reconocimiento de Bing Speech en C&#35; para Windows .NET

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

La biblioteca de servicios está pensada para los desarrolladores que tienen su propio servicio en la nube y quieren llamar al servicio de voz desde su servicio. Si quiere llamar al servicio de reconocimiento de voz desde aplicaciones vinculadas a dispositivo, no utilice este SDK. (Use otras bibliotecas cliente o las API de REST para esto).

Para usar la biblioteca de servicios de C#, instale el [paquete NuGet Microsoft.Bing.Speech](https://www.nuget.org/packages/Microsoft.Bing.Speech/). Para la referencia de API de biblioteca, vea la [biblioteca de servicios de C# de Microsoft Speech](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-ServiceLibrary/master/docs/index.html).

En las secciones siguientes se describe cómo instalar, compilar y ejecutar la aplicación de ejemplo de C# mediante el uso de la biblioteca de servicios de C#.

## <a name="prerequisites"></a>Requisitos previos

### <a name="platform-requirements"></a>Requisitos de la plataforma

El siguiente ejemplo se desarrolló para Windows 8+ y .NET 4.5+ Framework con [Visual Studio 2015, Community Edition](https://www.visualstudio.com/products/visual-studio-community-vs).

### <a name="get-the-sample-application"></a>Obtener la aplicación de ejemplo

Clone el ejemplo del repositorio del [ejemplo de biblioteca de servicios de C# de voz](https://github.com/Microsoft/Cognitive-Speech-STT-ServiceLibrary).

### <a name="subscribe-to-the-speech-recognition-api-and-get-a-free-trial-subscription-key"></a>Suscribirse a Speech Recognition API y obtener una clave de suscripción de prueba gratuita

Speech API forma parte de Cognitive Services (anteriormente Project Oxford). Puede obtener las claves de la suscripción de prueba gratuita en la página [Suscripción a Cognitive Services](https://azure.microsoft.com/try/cognitive-services/). Después de seleccionar Speech API, seleccione **Obtener clave de API** para obtener la clave. Devuelve una clave principal y una clave secundaria. Las dos claves están asociadas a la misma cuota, por lo que puede usar cualquiera de las claves.

> [!IMPORTANT]
> * Obtenga una clave suscripción. Para poder usar las bibliotecas cliente de voz, debe tener una [clave de suscripción](https://azure.microsoft.com/try/cognitive-services/).
>
> * Use la clave de su suscripción. Con la aplicación de ejemplo proporcionada de biblioteca de servicios de C#, debe proporcionar la clave de suscripción como uno de los parámetros de línea de comandos. Para obtener más información, consulte [Ejecutar la aplicación de ejemplo](#step-3-run-the-sample-application).

## <a name="step-1-install-the-sample-application"></a>Paso 1: Instalar la aplicación de ejemplo

1. Inicie Visual Studio 2015 y seleccione **Archivo** > **Abrir** > **Proyecto/Solución**.

2. Haga doble clic para abrir el archivo de Visual Studio 2015 Solution (.sln) denominado SpeechClient.sln. La solución se abre en Visual Studio.

## <a name="step-2-build-the-sample-application"></a>Paso 2: Compilar la aplicación de ejemplo

Presione Ctrl+Mayús+B, o seleccione **Compilar** en el menú de la cinta de opciones. Después, seleccione **Compilar solución**.

## <a name="step-3-run-the-sample-application"></a>Paso 3: Ejecutar la aplicación de ejemplo

1. Una vez completada la compilación, presione F5 o seleccione **Iniciar** en el menú de la cinta de opciones para ejecutar el ejemplo.

2. Abra el directorio de salida para el ejemplo, por ejemplo, SpeechClientSample\bin\Debug. Presione Mayús+haga clic con el botón derecho y seleccione **Abrir ventana de comandos aquí**.

3. Ejecute `SpeechClientSample.exe` con los argumentos siguientes:

   * Arg[0]: especifique un archivo WAV de audio de entrada.
   * Arg[1]: especifique la configuración regional del audio.
   * Arg[2]: especifique los modos de reconocimiento: *Corto* para el modo `ShortPhrase` y *Largo* para el modo `LongDictation`.
   * Arg[3]: especifique la clave de suscripción para tener acceso al servicio de reconocimiento de voz.

## <a name="samples-explained"></a>Ejemplos explicados

### <a name="recognition-modes"></a>Modos de reconocimiento

* Modo `ShortPhrase`: una expresión de hasta 15 segundos de duración. A medida que los datos se envían al servidor, el cliente recibe varios resultados parciales y un resultado final óptimo.
* Modo `LongDictation`: una expresión de hasta 10 minutos de duración. A medida que los datos se envían al servidor, el cliente recibe varios resultados parciales y varios resultados finales, en función de las pausas que indique el servidor en la oración.

### <a name="supported-audio-formats"></a>Formatos de audio compatibles

Speech API es compatible con audio/WAV mediante el uso de los códecs siguientes:

* Canal único PCM
* Siren
* SirenSR

### <a name="preferences"></a>Preferencias

Para crear un elemento SpeechClient, debe crear primero un objeto de preferencias. El objeto de preferencias es un conjunto de parámetros que configura el comportamiento del servicio de voz. Consta de los siguientes campos:

* `SpeechLanguage`: la configuración regional del audio que se envía al servicio de voz.
* `ServiceUri`: el punto de conexión usado para llamar al servicio de voz.
* `AuthorizationProvider`: una implementación de IAuthorizationProvider usada para capturar tokens con el fin de obtener acceso al servicio de voz. Aunque el ejemplo proporciona un proveedor de autorización de Cognitive Services, se recomienda encarecidamente crear su propia implementación para controlar el almacenamiento en caché de tokens.
* `EnableAudioBuffering`: una opción avanzada. Vea [Administración de conexiones](#connection-management).

### <a name="speech-input"></a>Entrada de voz

El objeto SpeechInput consta de dos campos:

* **Audio**: una implementación de secuencias de su elección de la que el SDK extrae audio. Puede ser cualquier [secuencia](https://msdn.microsoft.com/library/system.io.stream(v=vs.110).aspx) que admita operaciones de lectura.
   > [!NOTE]
   > El SDK detecta el final de la secuencia cuando esta devuelve **0** en la lectura.

* **RequestMetadata**: metadatos sobre la solicitud de voz. Para obtener más información, consulte la [referencia](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-ServiceLibrary/master/docs/index.html).

### <a name="speech-request"></a>Solicitud de voz

Una vez haya creado la instancia de los objetos SpeechClient y SpeechInput, use RecognizeAsync para realizar una solicitud al servicio de voz.

```cs
    var task = speechClient.RecognizeAsync(speechInput);
```

Una vez finalizada la solicitud, termina la tarea que RecognizeAsync devuelve. El último elemento RecognitionResult es el final del reconocimiento. La tarea puede producir un error si se genera un error inesperado en el servicio o el SDK.

### <a name="speech-recognition-events"></a>Eventos de reconocimiento de voz

#### <a name="partial-results-event"></a>Evento de resultados parciales

Este evento se llamará cada vez que el servicio de voz prediga lo que puede estar diciendo, incluso antes de que termine de hablar (si usa `MicrophoneRecognitionClient`) o termine de enviar datos (si usa `DataRecognitionClient`). Puede suscribirse al evento mediante `SpeechClient.SubscribeToPartialResult()`. O puede usar el método genérico de suscripción de eventos `SpeechClient.SubscribeTo<RecognitionPartialResult>()`.

**Formato de devolución** | DESCRIPCIÓN |
------|------
**LexicalForm** | Este formato es óptimo para su uso por aplicaciones que necesiten los resultados del reconocimiento de voz sin formato y sin procesar.
**DisplayText** | La frase reconocida después de aplicar la normalización de texto inverso, las mayúsculas y minúsculas, los signos de puntuación y el enmascaramiento de palabras soeces. Las palabras soeces se enmascaran con asteriscos después del carácter inicial, por ejemplo, "d***". Este formato es óptimo para su uso por aplicaciones que muestren los resultados del reconocimiento de voz al usuario.
**Confidence** | El nivel de confianza que la frase reconocida representa para el audio asociado, tal como se define por el servidor de reconocimiento de voz.
**MediaTime** | La hora actual en relación con el inicio de la secuencia de audio (en unidades de tiempo de 100 nanosegundos).
**MediaDuration** | La duración o longitud de la frase actual en relación con el segmento de audio (en unidades de tiempo de 100 nanosegundos).

#### <a name="result-event"></a>Evento de resultado
Cuando termina de hablar (en modo `ShortPhrase`), se llama a este evento. Se le ofrecen las n opciones mejores para el resultado. En modo `LongDictation`, el evento puede llamarse varias veces, en función de dónde indica el servidor las pausas en la frase. Puede suscribirse al evento mediante `SpeechClient.SubscribeToRecognitionResult()`. O puede usar el método genérico de suscripción de eventos `SpeechClient.SubscribeTo<RecognitionResult>()`.

**Formato de devolución** | DESCRIPCIÓN |
------|------|
**RecognitionStatus** | El estado de cómo se produjo el reconocimiento. Por ejemplo, se produjo como resultado del reconocimiento correcto, como resultado de la cancelación de la conexión, etc.
**Frases** | El conjunto de las n frases mejores reconocidas con la confianza de reconocimiento.

Para obtener más información sobre los resultados del reconocimiento, consulte [Output format](../Concepts.md#output-format) (Formato de salida).

### <a name="speech-recognition-response"></a>Respuesta de reconocimiento de voz

Ejemplo de respuesta de voz:
```
--- Partial result received by OnPartialResult  
---what  
--- Partial result received by OnPartialResult  
--what's  
--- Partial result received by OnPartialResult  
---what's the web  
--- Partial result received by OnPartialResult   
---what's the weather like  
---***** Phrase Recognition Status = [Success]   
***What's the weather like? (Confidence:High)  
What's the weather like? (Confidence:High) 
```

## <a name="connection-management"></a>Administración de conexiones

La API utiliza una sola conexión WebSocket por solicitud. Para la experiencia de usuario óptima, el SDK intenta volver a conectarse al servicio de voz e iniciar el reconocimiento del último elemento RecognitionResult que recibió. Por ejemplo, si la solicitud de audio dura dos minutos, el SDK recibe un elemento RecognitionEvent en la marca de un minuto y se produce un error de red después de cinco segundos, el SDK inicia una nueva conexión desde la marca de un minuto.

>[!NOTE]
>El SDK no busca más atrás de la marca de un minuto, porque la secuencia puede que no admita operaciones de búsqueda. En su lugar, el SDK mantiene un búfer interno que usa para almacenar en búfer el audio y borra el búfer al recibir eventos RecognitionResult.

## <a name="buffering-behavior"></a>Comportamiento de almacenamiento en búfer

De forma predeterminada, el SDK almacena en búfer el audio para que se pueda recuperar si se produce una interrupción de red. En un escenario donde es preferible descartar el audio que se pierde durante la desconexión de red y reiniciar la conexión, es mejor deshabilitar el almacenamiento en búfer del audio estableciendo `EnableAudioBuffering` del objeto de preferencias en `false`.

## <a name="related-topics"></a>Temas relacionados

[Referencia de la biblioteca de servicios de C# de voz de Microsoft](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-ServiceLibrary/master/docs/index.html)
