---
title: 'Inicio rápido: Asistente de voz personalizado en Java (Windows y Linux): servicio de voz'
titleSuffix: Azure Cognitive Services
description: En este inicio rápido, aprenderá a utilizar el SDK de Voz de Cognitive Services en una aplicación de consola Java. Aprenderá a conectar su aplicación cliente a un bot de Bot Framework previamente creado y configurado para usar el canal Direct Line Speech y habilitar una experiencia de asistente de voz.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 02/10/2020
ms.author: dapine
ms.openlocfilehash: 6baa98d50c50146e93b4832053f63f3bead90a6d
ms.sourcegitcommit: 021ccbbd42dea64d45d4129d70fff5148a1759fd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2020
ms.locfileid: "78330843"
---
# <a name="quickstart-create-a-voice-assistant-with-the-speech-sdk-java-preview"></a>Inicio rápido: Creación de un asistente de voz con el SDK de Voz, Java (versión preliminar)

También hay disponibles inicios rápidos para [conversión de voz en texto](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-java&tabs=jre), [conversión de texto en voz](~/articles/cognitive-services/Speech-Service/quickstarts/text-to-speech.md?pivots=programming-language-java&tabs=jre) y [traducción de voz](~/articles/cognitive-services/Speech-Service/quickstarts/translate-speech-to-text.md?pivots=programming-language-java&tabs=jre).

En este artículo creará una aplicación de consola Java mediante el [SDK de Voz de Azure Cognitive Services](speech-sdk.md). La aplicación se conectará a un bot previamente creado y configurado para utilizar el canal Direct Line Speech, enviar una solicitud de voz y devolver una actividad de respuesta de voz (si se ha configurado). La aplicación se crea con el paquete de Maven del SDK de Voz y el IDE de Java de Eclipse en Windows, Linux y en macOS. Se ejecuta en un entorno de tiempo de ejecución de Java 8 (JRE) de 64 bits.

## <a name="prerequisites"></a>Prerrequisitos

Esta guía de inicio rápido requiere:

- Sistema operativo: Windows (64 bits), Ubuntu Linux 16.04/18.04 (64 bits), RHEL/CentOS 8 (x64) o macOS 10.13 o posterior.
- [IDE de Java de Eclipse](https://www.eclipse.org/downloads/).
- [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) o [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html).
- Una clave de suscripción de Azure para el servicio Voz. [Obtenga una gratis](get-started.md) o créela en [Azure Portal](https://portal.azure.com).
- Un bot configurado previamente y creado mediante la versión 4.2 o superior de Bot Framework. El bot tendría que suscribirse al nuevo canal Direct Line Speech para recibir entradas de voz.

  > [!NOTE]
  > Consulte [la lista de regiones admitidas para los asistentes de voz ](regions.md#voice-assistants) y asegúrese de que sus recursos se implementan en una de esas regiones.

Si ejecuta Ubuntu 16.04 o 18.04, asegúrese de que estén instaladas estas dependencias antes de iniciar Eclipse:

```sh
sudo apt-get update
sudo apt-get install build-essential libssl1.0.0 libasound2 wget
```

En On RHEL/CentOS 8:

```sh
sudo yum update
sudo yum groupinstall "Development tools"
sudo yum install alsa-lib java-1.8.0-openjdk-devel openssl wget
```

> [!NOTE]
> En RHEL/CentOS 8, siga las instrucciones sobre [cómo configurar OpenSSL para Linux](~/articles/cognitive-services/speech-service/how-to-configure-openssl-linux.md).

Si está ejecutando Windows (64 bits), asegúrese de que ha instalado Microsoft Visual C++ Redistributable para su plataforma:

- [Descarga de Microsoft Visual C++ Redistributable para Visual Studio 2017](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads)

## <a name="optional-get-started-fast"></a>Opcional: Empiece rápidamente

En este inicio rápido se describe, paso a paso, cómo hacer que una aplicación cliente simple se conecte al bot habilitado para voz. Si desea sumergirse de lleno, el código fuente completo y listo para compilar utilizado en este inicio rápido está disponible en los [ejemplos del SDK de Voz](https://aka.ms/csspeech/samples) bajo la carpeta `quickstart`.

## <a name="create-and-configure-project"></a>Creación y configuración de un proyecto

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-java-create-proj.md)]

Además, para habilitar el registro, actualice el archivo _pom.xml_ para incluir la dependencia siguiente:

```xml
 <dependency>
     <groupId>org.slf4j</groupId>
     <artifactId>slf4j-simple</artifactId>
     <version>1.7.5</version>
 </dependency>
```

## <a name="add-sample-code"></a>Incorporación de código de ejemplo

1. Para agregar una nueva clase vacía al proyecto de Java, seleccione **File (Archivo)**  > **New (Nuevo)**  > **Class (Clase)** .

1. En la ventana **New Java Class** (Nueva clase de Java) escriba _speechsdk.quickstart_ en el campo **Package** (Paquete) y _Main_ en el campo **Name** (Nombre).

   ![Captura de pantalla de la ventana New Java Class (Nueva clase de Java)](media/sdk/qs-java-jre-06-create-main-java.png)

1. Abra la clase recién creada `Main` y reemplace el contenido del archivo `Main.java` por el código de inicio siguiente:

   ```java
   package speechsdk.quickstart;

   import com.microsoft.cognitiveservices.speech.audio.AudioConfig;
   import com.microsoft.cognitiveservices.speech.audio.PullAudioOutputStream;
   import com.microsoft.cognitiveservices.speech.dialog.DialogServiceConfig;
   import com.microsoft.cognitiveservices.speech.dialog.DialogServiceConnector;
   import org.slf4j.Logger;
   import org.slf4j.LoggerFactory;

   import javax.sound.sampled.AudioFormat;
   import javax.sound.sampled.AudioSystem;
   import javax.sound.sampled.DataLine;
   import javax.sound.sampled.SourceDataLine;
   import java.io.InputStream;

   public class Main {
       final Logger log = LoggerFactory.getLogger(Main.class);

       public static void main(String[] args) {
           // New code will go here
       }

       private void playAudioStream(PullAudioOutputStream audio) {
           ActivityAudioStream stream = new ActivityAudioStream(audio);
           final ActivityAudioStream.ActivityAudioFormat audioFormat = stream.getActivityAudioFormat();
           final AudioFormat format = new AudioFormat(
                   AudioFormat.Encoding.PCM_SIGNED,
                   audioFormat.getSamplesPerSecond(),
                   audioFormat.getBitsPerSample(),
                   audioFormat.getChannels(),
                   audioFormat.getFrameSize(),
                   audioFormat.getSamplesPerSecond(),
                   false);
           try {
               int bufferSize = format.getFrameSize();
               final byte[] data = new byte[bufferSize];

               SourceDataLine.Info info = new DataLine.Info(SourceDataLine.class, format);
               SourceDataLine line = (SourceDataLine) AudioSystem.getLine(info);
               line.open(format);

               if (line != null) {
                   line.start();
                   int nBytesRead = 0;
                   while (nBytesRead != -1) {
                       nBytesRead = stream.read(data);
                       if (nBytesRead != -1) {
                           line.write(data, 0, nBytesRead);
                       }
                   }
                   line.drain();
                   line.stop();
                   line.close();
               }
               stream.close();

           } catch (Exception e) {
               e.printStackTrace();
           }
       }

   }
   ```

1. En el método `main`, primero configurará el `DialogServiceConfig` y lo utilizará para crear una instancia de `DialogServiceConnector`. Esta instancia se conecta al canal de Direct Line Speech para interactuar con el bot. También se utiliza una instancia de `AudioConfig` para especificar el origen de la entrada de audio. En este ejemplo, se usa el micrófono predeterminado con `AudioConfig.fromDefaultMicrophoneInput()`.

   - Reemplace la cadena `YourSubscriptionKey` por la clave de suscripción, que puede obtener en [este sitio web](get-started.md).
   - Reemplace la cadena `YourServiceRegion` por la [región](regions.md) asociada a su suscripción.

   > [!NOTE]
   > Consulte [la lista de regiones admitidas para los asistentes de voz ](regions.md#voice-assistants) y asegúrese de que sus recursos se implementan en una de esas regiones.

   ```java
   final String subscriptionKey = "YourSubscriptionKey"; // Your subscription key
   final String region = "YourServiceRegion"; // Your speech subscription service region
   final BotFrameworkConfig botConfig = BotFrameworkConfig.fromSubscription(subscriptionKey, region);

   // Configure audio input from a microphone.
   final AudioConfig audioConfig = AudioConfig.fromDefaultMicrophoneInput();

   // Create a DialogServiceConnector instance.
   final DialogServiceConnector connector = new DialogServiceConnector(botConfig, audioConfig);
   ```

1. El conector `DialogServiceConnector` se basa en varios eventos para comunicar sus actividades de bot, los resultados del reconocimiento de voz y otra información. Agregue estos clientes de escucha de eventos a continuación.

   ```java
   // Recognizing will provide the intermediate recognized text while an audio stream is being processed.
   connector.recognizing.addEventListener((o, speechRecognitionResultEventArgs) -> {
       log.info("Recognizing speech event text: {}", speechRecognitionResultEventArgs.getResult().getText());
   });

   // Recognized will provide the final recognized text once audio capture is completed.
   connector.recognized.addEventListener((o, speechRecognitionResultEventArgs) -> {
       log.info("Recognized speech event reason text: {}", speechRecognitionResultEventArgs.getResult().getText());
   });

   // SessionStarted will notify when audio begins flowing to the service for a turn.
   connector.sessionStarted.addEventListener((o, sessionEventArgs) -> {
       log.info("Session Started event id: {} ", sessionEventArgs.getSessionId());
   });

   // SessionStopped will notify when a turn is complete and it's safe to begin listening again.
   connector.sessionStopped.addEventListener((o, sessionEventArgs) -> {
       log.info("Session stopped event id: {}", sessionEventArgs.getSessionId());
   });

   // Canceled will be signaled when a turn is aborted or experiences an error condition.
   connector.canceled.addEventListener((o, canceledEventArgs) -> {
       log.info("Canceled event details: {}", canceledEventArgs.getErrorDetails());
       connector.disconnectAsync();
   });

   // ActivityReceived is the main way your bot will communicate with the client and uses Bot Framework activities.
   connector.activityReceived.addEventListener((o, activityEventArgs) -> {
       final String act = activityEventArgs.getActivity().serialize();
           log.info("Received activity {} audio", activityEventArgs.hasAudio() ? "with" : "without");
           if (activityEventArgs.hasAudio()) {
               playAudioStream(activityEventArgs.getAudio());
           }
       });
   ```

1. Conecte el `DialogServiceConnector` a Direct Line Speech mediante la llamada al método `connectAsync()`. Para probar el bot, puede invocar el método `listenOnceAsync` para enviar una entrada de audio desde el micrófono. Además, también puede utilizar el método `sendActivityAsync` para enviar una actividad personalizada como una cadena serializada. Estas actividades personalizadas pueden proporcionar datos adicionales que el bot utilizará en la conversación.

   ```java
   connector.connectAsync();
   // Start listening.
   System.out.println("Say something ...");
   connector.listenOnceAsync();

   // connector.sendActivityAsync(...)
   ```

1. Guarde los cambios en el archivo `Main`.

1. Para admitir la reproducción de respuestas, puede agregar una clase adicional que se transforma el objeto PullAudioOutputStream devuelto desde la API de getAudio() a un elemento InputStream de Java para facilitar el control. Esta `ActivityAudioStream` es una clase especializada que controla la respuesta de audio desde el canal de Direct Line Speech. Proporcionará descriptores de acceso para capturar la información del formato de audio que se requiere para controlar la reproducción. Para ello, seleccione **Archivo** > **Nuevo** > **Clase**.

1. En la ventana **New Java Class** (Nueva clase Java), escriba _speechsdk.quickstart_ en el campo **Package** (Paquete) y _ActivityAudioStream_ en el campo **Name** (Nombre).

1. Abra la clase `ActivityAudioStream` que se creó recientemente y reemplace el contenido por el código siguiente:

   ```java
   package com.speechsdk.quickstart;

   import com.microsoft.cognitiveservices.speech.audio.PullAudioOutputStream;

   import java.io.IOException;
   import java.io.InputStream;

    public final class ActivityAudioStream extends InputStream {
        /**
         * The number of samples played per second (16 kHz).
         */
        public static final long SAMPLE_RATE = 16000;
        /**
         * The number of bits in each sample of a sound that has this format (16 bits).
         */
        public static final int BITS_PER_SECOND = 16;
        /**
         * The number of audio channels in this format (1 for mono).
         */
        public static final int CHANNELS = 1;
        /**
         * The number of bytes in each frame of a sound that has this format (2).
         */
        public static final int FRAME_SIZE = 2;

        /**
         * Reads up to a specified maximum number of bytes of data from the audio
         * stream, putting them into the given byte array.
         *
         * @param b   the buffer into which the data is read
         * @param off the offset, from the beginning of array <code>b</code>, at which
         *            the data will be written
         * @param len the maximum number of bytes to read
         * @return the total number of bytes read into the buffer, or -1 if there
         * is no more data because the end of the stream has been reached
         */
        @Override
        public int read(byte[] b, int off, int len) {
            byte[] tempBuffer = new byte[len];
            int n = (int) this.pullStreamImpl.read(tempBuffer);
            for (int i = 0; i < n; i++) {
                if (off + i > b.length) {
                    throw new ArrayIndexOutOfBoundsException(b.length);
                }
                b[off + i] = tempBuffer[i];
            }
            if (n == 0) {
                return -1;
            }
            return n;
        }

        /**
         * Reads the next byte of data from the activity audio stream if available.
         *
         * @return the next byte of data, or -1 if the end of the stream is reached
         * @see #read(byte[], int, int)
         * @see #read(byte[])
         * @see #available
         * <p>
         */
        @Override
        public int read() {
            byte[] data = new byte[1];
            int temp = read(data);
            if (temp <= 0) {
                // we have a weird situation if read(byte[]) returns 0!
                return -1;
            }
            return data[0] & 0xFF;
        }

        /**
         * Reads up to a specified maximum number of bytes of data from the activity audio stream,
         * putting them into the given byte array.
         *
         * @param b the buffer into which the data is read
         * @return the total number of bytes read into the buffer, or -1 if there
         * is no more data because the end of the stream has been reached
         */
        @Override
        public int read(byte[] b) {
            int n = (int) pullStreamImpl.read(b);
            if (n == 0) {
                return -1;
            }
            return n;
        }

        /**
         * Skips over and discards a specified number of bytes from this
         * audio input stream.
         *
         * @param n the requested number of bytes to be skipped
         * @return the actual number of bytes skipped
         * @throws IOException if an input or output error occurs
         * @see #read
         * @see #available
         */
        @Override
        public long skip(long n) {
            if (n <= 0) {
                return 0;
            }
            if (n <= Integer.MAX_VALUE) {
                byte[] tempBuffer = new byte[(int) n];
                return read(tempBuffer);
            }
            long count = 0;
            for (long i = n; i > 0; i -= Integer.MAX_VALUE) {
                int size = (int) Math.min(Integer.MAX_VALUE, i);
                byte[] tempBuffer = new byte[size];
                count += read(tempBuffer);
            }
            return count;
        }

        /**
         * Closes this audio input stream and releases any system resources associated
         * with the stream.
         */
        @Override
        public void close() {
            this.pullStreamImpl.close();
        }

        /**
         * Fetch the audio format for the ActivityAudioStream. The ActivityAudioFormat defines the sample rate, bits per sample, and the # channels.
         *
         * @return instance of the ActivityAudioFormat associated with the stream
         */
        public ActivityAudioStream.ActivityAudioFormat getActivityAudioFormat() {
            return activityAudioFormat;
        }

        /**
         * Returns the maximum number of bytes that can be read (or skipped over) from this
         * audio input stream without blocking.
         *
         * @return the number of bytes that can be read from this audio input stream without blocking.
         * As this implementation does not buffer, this will be defaulted to 0
         */
        @Override
        public int available() {
            return 0;
        }

        public ActivityAudioStream(final PullAudioOutputStream stream) {
            pullStreamImpl = stream;
            this.activityAudioFormat = new ActivityAudioStream.ActivityAudioFormat(SAMPLE_RATE, BITS_PER_SECOND, CHANNELS, FRAME_SIZE, AudioEncoding.PCM_SIGNED);
        }

        private PullAudioOutputStream pullStreamImpl;

        private ActivityAudioFormat activityAudioFormat;

        /**
         * ActivityAudioFormat is an internal format which contains metadata regarding the type of arrangement of
         * audio bits in this activity audio stream.
         */
        static class ActivityAudioFormat {

            private long samplesPerSecond;
            private int bitsPerSample;
            private int channels;
            private int frameSize;
            private AudioEncoding encoding;

            public ActivityAudioFormat(long samplesPerSecond, int bitsPerSample, int channels, int frameSize, AudioEncoding encoding) {
                this.samplesPerSecond = samplesPerSecond;
                this.bitsPerSample = bitsPerSample;
                this.channels = channels;
                this.encoding = encoding;
                this.frameSize = frameSize;
            }

            /**
             * Fetch the number of samples played per second for the associated audio stream format.
             *
             * @return the number of samples played per second
             */
            public long getSamplesPerSecond() {
                return samplesPerSecond;
            }

            /**
             * Fetch the number of bits in each sample of a sound that has this audio stream format.
             *
             * @return the number of bits per sample
             */
            public int getBitsPerSample() {
                return bitsPerSample;
            }

            /**
             * Fetch the number of audio channels used by this audio stream format.
             *
             * @return the number of channels
             */
            public int getChannels() {
                return channels;
            }

            /**
             * Fetch the default number of bytes in a frame required by this audio stream format.
             *
             * @return the number of bytes
             */
            public int getFrameSize() {
                return frameSize;
            }

            /**
             * Fetch the audio encoding type associated with this audio stream format.
             *
             * @return the encoding associated
             */
            public AudioEncoding getEncoding() {
                return encoding;
            }
        }

        /**
         * Enum defining the types of audio encoding supported by this stream.
         */
        public enum AudioEncoding {
            PCM_SIGNED("PCM_SIGNED");

            String value;

            AudioEncoding(String value) {
                this.value = value;
            }
        }
    }

   ```

1. Guarde los cambios en el archivo `ActivityAudioStream`.

## <a name="build-and-run-the-app"></a>Compilación y ejecución de la aplicación

Seleccione F11, o seleccione **Run (Ejecutar)**  > **Debug (Depurar)** .
La consola muestra el mensaje: "Say something" (Diga algo).
En ese momento, diga una frase en inglés que el bot pueda entender. La voz se transmite al bot a través del canal de Direct Line Speech y, una vez allí, el bot la reconoce y la procesa. La respuesta se devuelve como una actividad. Si el bot devuelve la voz como respuesta, el audio se reproducirá mediante la clase `AudioPlayer`.

![Captura de pantalla de la salida de la consola después de un reconocimiento correcto](media/sdk/qs-java-jre-08-console-output.png)

## <a name="next-steps"></a>Pasos siguientes

Se pueden encontrar ejemplos adicionales, por ejemplo, cómo leer voz de un archivo de audio, en GitHub.

> [!div class="nextstepaction"]
> [Creación e implementación de un bot básico](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-basic-deploy?view=azure-bot-service-4.0)

## <a name="see-also"></a>Consulte también

- [Acerca de los asistentes de voz](voice-assistants.md)
- [Obtenga una clave de suscripción gratuita a los servicios de Voz](get-started.md)
- [Palabras clave personalizadas](speech-devices-sdk-create-kws.md)
- [Conexión de Direct Line de Voz al bot](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech)
- [Exploración de ejemplos de Java en GitHub](https://aka.ms/csspeech/samples)
