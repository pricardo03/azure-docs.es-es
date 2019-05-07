---
title: 'Inicio rápido: Asistente virtual personalizado por voz (versión preliminar), Java (Windows, Linux): servicios de voz'
titleSuffix: Azure Cognitive Services
description: En este inicio rápido, aprenderá a utilizar el kit de desarrollo de software (SDK) de Voz de Cognitive Services en una aplicación de consola Java. Aprenderá a conectar su aplicación cliente a un bot de Bot Framework previamente creado y configurado para usar el canal Direct Line Speech y habilitar una experiencia de asistente virtual por voz.
services: cognitive-services
author: bidishac
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 05/02/2019
ms.author: bidishac
ms.openlocfilehash: 4e9010bed54d0b2a7cb1a95b9e01e5ba02ea9fd5
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65025369"
---
# <a name="quickstart-create-a-voice-first-virtual-assistant-with-the-speech-sdk-java"></a>Inicio rápido: Creación de un asistente virtual por voz con el SDK de Voz, Java

En este artículo creará una aplicación de consola Java mediante el [SDK de Voz de Cognitive Services](speech-sdk.md). La aplicación se conectará a un bot previamente creado y configurado para utilizar el canal Direct Line Speech, enviar una solicitud de voz y devolver una actividad de respuesta de voz (si está configurado). La aplicación se crea con el paquete de Maven del SDK de Voz y el IDE de Java de Eclipse en Windows, Ubuntu Linux y en macOS. Se ejecuta en un entorno de tiempo de ejecución de Java 8 (JRE) de 64 bits.

## <a name="prerequisites"></a>Requisitos previos

Esta guía de inicio rápido requiere:

* Sistema operativo: Windows (64 bits), Ubuntu Linux 16.04/18.04 (64 bits) o macOS 10.13 o superior
* [IDE de Java de Eclipse](https://www.eclipse.org/downloads/)
* [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) o [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* Una clave de suscripción de Azure para el servicio Voz. [Obtenga una gratis](get-started.md).
* Un bot configurado previamente y creado mediante la versión 4.2 o superior de Bot Framework. El bot tendría que suscribirse al nuevo canal "Direct Line Speech" para recibir entradas de voz.

    > [!NOTE]
    > En la versión preliminar, el canal Direct Line Speech actualmente solo admite la región **westus2**.

    > [!NOTE]
    > La prueba de 30 días para el plan de tarifa estándar descrito en [Prueba gratuita de los servicios de voz](get-started.md) está restringida a **westus** (no **westus2**) y por lo tanto no es compatible con el servicio de voz de Direct Line. Las suscripciones del nivel gratis y estándar de **westus2** son compatibles.

Si ejecuta Ubuntu 16.04 o 18.04, asegúrese de que estén instaladas estas dependencias antes de iniciar Eclipse:

```console
sudo apt-get update
sudo apt-get install build-essential libssl1.0.0 libasound2 wget
```

Si está ejecutando Windows (64 bits), asegúrese de que ha instalado Microsoft Visual C++ Redistributable para su plataforma:
* [Descarga de Microsoft Visual C++ Redistributable para Visual Studio 2017](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads)

## <a name="optional-get-started-fast"></a>Opcional: Empiece rápidamente

En este inicio rápido se describirá, paso a paso, cómo hacer que una aplicación cliente simple se conecte al bot habilitado para voz. Si prefiere sumergirse de lleno, el código fuente completo y listo para compilar utilizado en este inicio rápido está disponible en los [ejemplos del SDK de Voz](https://aka.ms/csspeech/samples) bajo la carpeta `quickstart`.

## <a name="create-and-configure-project"></a>Creación y configuración de un proyecto

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-java-create-proj.md)]

Además, para habilitar el registro, actualice el archivo **pom.xml** para incluir la dependencia siguiente.

   ```xml
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-simple</artifactId>
        <version>1.7.5</version>
    </dependency>
   ```

## <a name="add-sample-code"></a>Incorporación de código de ejemplo

1. Para agregar una nueva clase vacía al proyecto de Java, seleccione **File (Archivo)**  >  **New (Nuevo)**  >  **Class (Clase)**.

1. En la ventana **New Java Class** (Nueva clase de Java) escriba **speechsdk.quickstart** en el campo **Package** (Paquete) y **Main** en el campo **Name** (Nombre).

   ![Captura de pantalla de la ventana New Java Class (Nueva clase de Java)](media/sdk/qs-java-jre-06-create-main-java.png)

1. Abra la clase recién creada **Main** y reemplace el contenido del archivo `Main.java` por el código de inicio siguiente.

    ```java
    package speechsdk.quickstart;

    import java.io.IOException;
    import java.io.PipedOutputStream;
    import java.util.HashMap;

    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;

    import com.microsoft.cognitiveservices.speech.ResultReason;
    import com.microsoft.cognitiveservices.speech.audio.AudioConfig;
    import com.microsoft.cognitiveservices.speech.dialog.BotConnectorConfig;
    import com.microsoft.cognitiveservices.speech.dialog.SpeechBotConnector;

    public class Main {
        final Logger log = LoggerFactory.getLogger(Main.class);

        public static void main(String[] args) {
            // New code will go here
        }
    }
    ```

1. En el método **main**, primero configurará el `BotConnectorConfig` y lo utilizará para crear una instancia de `SpeechBotConnector`. Esto se conectará al canal de voz de Direct Line para interactuar con el bot. También se utiliza una instancia de `AudioConfig` para especificar el origen de la entrada de audio. En este ejemplo, se usa el micrófono predeterminado con `AudioConfig.fromDefaultMicrophoneInput()`.

    * Reemplace la cadena `YourSubscriptionKey` por la clave de suscripción, que puede obtener [aquí](get-started.md).
    * Reemplace la cadena `YourServiceRegion` por la [región](regions.md) asociada a su suscripción.
    * Reemplace la cadena `YourChannelSecret` por el secreto del canal de voz de Direct Line.

    > [!NOTE]
    > En la versión preliminar, el canal Direct Line Speech actualmente solo admite la región **westus2**.

    > [!NOTE]
    > La prueba de 30 días para el plan de tarifa estándar descrito en [Prueba gratuita de los servicios de voz](get-started.md) está restringida a **westus** (no **westus2**) y por lo tanto no es compatible con el servicio de voz de Direct Line. Las suscripciones del nivel gratis y estándar de **westus2** son compatibles.

    ```java
    final String channelSecret = "YourChannelSecret"; // Your channel secret
    final String subscriptionKey = "YourSubscriptionKey"; // your subscription key
    final String region = "YourServiceRegion"; // Your service region. Currently assumed to be westus2
    final BotConnectorConfig botConnectorConfig = BotConnectorConfig.fromSecretKey(channelSecret, subscriptionKey, region);

    // Configure audio input from microphone.
    final AudioConfig audioConfig = AudioConfig.fromDefaultMicrophoneInput();

    // Create a SpeechjBotConnector instance
    final SpeechBotConnector botConnector = new SpeechBotConnector(botConnectorConfig, audioConfig);
    ```

1. `SpeechBotConnector` se basa en varios eventos para comunicar sus actividades de bot, los resultados del reconocimiento de voz y otra información. Agregue estos clientes de escucha de eventos a continuación.

    ```java
    // Recognizing will provide the intermediate recognized text while an audio stream is being processed
    botConnector.recognizing.addEventListener((o, speechRecognitionResultEventArgs) -> {
        log.info("Recognizing speech event text: {}", speechRecognitionResultEventArgs.getResult().getText());
    });

    // Recognized will provide the final recognized text once audio capture is completed
    botConnector.recognized.addEventListener((o, speechRecognitionResultEventArgs) -> {
        log.info("Recognized speech event reason text: {}", speechRecognitionResultEventArgs.getResult().getText());
    });

    // SessionStarted will notify when audio begins flowing to the service for a turn
    botConnector.sessionStarted.addEventListener((o, sessionEventArgs) -> {
        log.info("Session Started event id: {} ", sessionEventArgs.getSessionId());
    });

    // SessionStopped will notify when a turn is complete and it's safe to begin listening again
    botConnector.sessionStopped.addEventListener((o, sessionEventArgs) -> {
        log.info("Session stopped event id: {}", sessionEventArgs.getSessionId());
    });

    // Canceled will be signaled when a turn is aborted or experiences an error condition
    botConnector.canceled.addEventListener((o, canceledEventArgs) -> {
        log.info("Canceled event details: {}", canceledEventArgs.getErrorDetails());
        botConnector.disconnectAsync();
    });

    // ActivityReceived is the main way your bot will communicate with the client and uses bot framework activities
    botConnector.activityReceived.addEventListener((o, activityEventArgs) -> {
        String act = activityEventArgs.getActivity().serialize();
        log.info("Received activity: {}", act);
    });
    ```

1. Conecte el `SpeechBotConnector` a Direct Line Speech mediante la llamada al método `connectAsync()`. Para probar el bot, puede invocar el método `listenOnceAsync` para enviar una entrada de audio desde el micrófono. Además, también puede utilizar el método `sendActivityAsync` para enviar una actividad personalizada como una cadena serializada. Estas actividades personalizadas pueden proporcionar datos adicionales que el bot utilizará en la conversación.

    ```java
    botConnector.connectAsync();
    // Start listening.
    System.out.println("Say something ...");
    botConnector.listenOnceAsync();

    // botConnector.sendActivityAsync(...)
    ```

1. Guarde los cambios en el archivo `Main`.

1. Para admitir la reproducción de respuestas, agregará una clase adicional que incluirá métodos de utilidad para admitir audio. Para habilitar el audio, agregue otra nueva clase vacía al proyecto Java: seleccione **File** > **New** > **Class** (Archivo > Nuevo > Clase).

1. En la ventana **New Java Class** (Nueva clase de Java) escriba **speechsdk.quickstart** en el campo **Package** (Paquete) y **AudioPlayer** en el campo **Name** (Nombre).

   ![Captura de pantalla de la ventana New Java Class (Nueva clase de Java)](media/sdk/qs-java-jre-06-create-main-java.png)

1. Abra la clase **AudioPlayer** recién creada y reemplácela por el código que se proporciona a continuación.

    ```java
    import static javax.sound.sampled.AudioFormat.Encoding.PCM_SIGNED;

    import java.io.InputStream;
    import java.io.PipedInputStream;
    import java.io.PipedOutputStream;
    import java.util.concurrent.ExecutorService;
    import java.util.concurrent.Executors;
    import java.util.concurrent.atomic.AtomicBoolean;

    import javax.sound.sampled.AudioFormat;
    import javax.sound.sampled.AudioSystem;
    import javax.sound.sampled.DataLine;
    import javax.sound.sampled.SourceDataLine;

    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;


    public class AudioPlayer {

        public static final int SAMPLE_RATE = 16000; // 16Hz sampling rate
        public static final int SAMPLE_SIZE_IN_BITS = 16; // 16 bit PCM
        public static final int CHANNELS = 1; // Use Mono / Single channel

        public static final int FRAME_RATE = 16000;
        public static final int FRAME_SIZE = 2;

        private static final Logger log = LoggerFactory.getLogger(AudioPlayer.class);
        private AtomicBoolean isPlaying = new AtomicBoolean(false);
        private ExecutorService executorService = Executors.newSingleThreadExecutor();

        public boolean isPlaying() {
            return isPlaying.get();
        }

        public void stopPlaying() {
            isPlaying.set(false);
        }

        public void play(final PipedOutputStream pipedOutputStream) {
            // The current audio supported by the Microsoft Bot framework ~ 16-bit PCM encoding, 16KHz sampling rate.
            final AudioFormat defaultFormat = new AudioFormat(PCM_SIGNED, SAMPLE_RATE, SAMPLE_SIZE_IN_BITS, CHANNELS, FRAME_SIZE, FRAME_RATE, false);
            try {
                final PipedInputStream inputStream = new PipedInputStream(pipedOutputStream);

                executorService.submit(() -> {
                    try {
                        isPlaying.set(true);
                        play(inputStream, defaultFormat);
                        inputStream.close();
                    } catch (Exception e) {
                        log.error("Exception thrown during playback. Message: {}", e.getMessage(), e);
                    }
                });
            } catch (Exception e) {
                log.error("Exception thrown during playback. Message: {}", e.getMessage(), e);
            }
        }

        private void play(final InputStream inputStream, final AudioFormat targetFormat) throws Exception {
            final byte[] buffer = new byte[1024];
            final DataLine.Info info = new DataLine.Info(SourceDataLine.class, targetFormat);
            final SourceDataLine line = (SourceDataLine) AudioSystem.getLine(info);
            line.open();
            if (line != null) {
                line.start();
                int bytesRead = 0;
                while (bytesRead != -1) {
                    bytesRead = inputStream.read(buffer, 0, buffer.length);
                    if (bytesRead != -1) {
                        line.write(buffer, 0, bytesRead);
                    }
                }
                line.drain();
                line.stop();
                line.close();
            }
        }
    }
    ```

1. Guarde los cambios en el archivo `AudioPlayer`.

## <a name="build-and-run-the-app"></a>Compilación y ejecución de la aplicación

Presione F11, o seleccione **Run (Ejecutar)** > **Debug (Depurar)**.
La consola mostrará el mensaje "Say something" (Diga algo). En este punto, puedes decir una expresión o frase en inglés que el bot lo comprenderá. La voz se transmitirá al bot a través del canal Direct Line Speech donde se reconocerá, se procesada por el bot y se devolverá la respuesta como una actividad. Si el bot devuelve la voz como respuesta, el audio se reproducirá mediante la clase `AudioPlayer`.

![Captura de pantalla de la salida de la consola después de un reconocimiento correcto](media/sdk/qs-java-jre-07-console-output.png)

## <a name="next-steps"></a>Pasos siguientes

Se pueden encontrar ejemplos adicionales, por ejemplo, cómo leer voz de un archivo de audio, en GitHub.

> [!div class="nextstepaction"]
> [Exploración de ejemplos de Java en GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Otras referencias

- [Inicio rápido: Conversión de voz, Java (Windows, Linux)](quickstart-translate-speech-java-jre.md)
- [Personalización de modelos acústicos](how-to-customize-acoustic-models.md)
- [Personalización de modelos de lenguaje](how-to-customize-language-model.md)
