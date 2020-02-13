---
title: 'Inicio rápido: Asistente de voz personalizado en Java (Android): servicio de voz'
titleSuffix: Azure Cognitive Services
description: Aprenda a crear una aplicación de asistente de voz de Java en Android mediante el SDK de Voz.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 02/10/2020
ms.author: dapine
ms.openlocfilehash: d4f84bad6139210dcff3d3f3726d8b7a17d92a5b
ms.sourcegitcommit: 7c18afdaf67442eeb537ae3574670541e471463d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2020
ms.locfileid: "77119733"
---
# <a name="quickstart-create-a-voice-assistant-in-java-on-android-by-using-the-speech-sdk"></a>Inicio rápido: Creación de un asistente de voz de Java en Android con el SDK de Voz

También hay un inicio rápido disponible para la [conversión de voz en texto ](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-java&tabs=android) y [de texto en voz](~/articles/cognitive-services/Speech-Service/quickstarts/text-to-speech.md?pivots=programming-language-java&tabs=android).

En este artículo, va a crear un asistente de voz con Java para Android mediante el [SDK de Voz](speech-sdk.md). Esta aplicación se conectará a un bot que ya se haya creado y configurado con el [canal Direct Line Speech](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech). A continuación, enviará una solicitud de voz al bot y presentará una actividad de respuesta habilitada para voz.

Esta aplicación se basa en el paquete de Maven de Speech SDK y en Android Studio 3.3. El SDK de Voz es compatible actualmente con dispositivos Android que tienen procesadores ARM de 32 o 64 bits o Intel x86 o x64.

> [!NOTE]
> Para el SDK de dispositivos de Voz y el dispositivo Roobo, consulte [SDK de dispositivos de voz](speech-devices-sdk.md).

## <a name="prerequisites"></a>Prerrequisitos

- Una clave de suscripción de Azure para el servicio Voz. [Obtenga una gratis](get-started.md) o créela en [Azure Portal](https://portal.azure.com).
- Un bot creado previamente y configurado con el [canal Direct Line Speech](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech)
- [Android Studio](https://developer.android.com/studio/) v3.3 o versiones posteriores

    > [!NOTE]
    > Consulte [la lista de regiones admitidas para los asistentes de voz ](regions.md#voice-assistants) y asegúrese de que sus recursos se implementan en una de esas regiones.

## <a name="create-and-configure-a-project"></a>Creación y configuración de un proyecto

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-java-android-create-proj.md)]

## <a name="create-user-interface"></a>Interfaz de Create user (Crear usuario)

En esta sección, se va a crear una interfaz de usuario básica para la aplicación. Comencemos por abrir la actividad principal: `activity_main.xml`. La plantilla básica incluye una barra de título con el nombre de la aplicación y un `TextView` con el mensaje "Hello world!".

A continuación, reemplace el contenido de `activity_main.xml` por el código siguiente:

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity">

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:onClick="onBotButtonClicked"
        android:text="Talk to your bot" />

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Recognition Data"
        android:textSize="18dp"
        android:textStyle="bold" />

    <TextView
        android:id="@+id/recoText"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="  \n(Recognition goes here)\n" />

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Activity Data"
        android:textSize="18dp"
        android:textStyle="bold" />

    <TextView
        android:id="@+id/activityText"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:scrollbars="vertical"
        android:text="  \n(Activities go here)\n" />

   </LinearLayout>
   ```

Este XML define una interfaz de usuario sencilla para interactuar con el bot.

- El elemento `button` inicia una interacción e invoca al método `onBotButtonClicked` cuando se hace clic.
- El elemento `recoText` mostrará los resultados de la conversión de voz a texto cuando hable al bot.
- El elemento `activityText` mostrará la carga de JSON para la actividad de Bot Framework más reciente desde el bot.

El texto y la representación gráfica de la interfaz de usuario ahora deben tener este aspecto:

![](media/sdk/qs-java-android-assistant-designer-ui.png)

## <a name="add-sample-code"></a>Incorporación de código de ejemplo

1. Abra `MainActivity.java` y reemplace el contenido por el código siguiente:

   ```java
    package samples.speech.cognitiveservices.microsoft.com;

    import android.media.AudioFormat;
    import android.media.AudioManager;
    import android.media.AudioTrack;
    import android.support.v4.app.ActivityCompat;
    import android.support.v7.app.AppCompatActivity;
    import android.os.Bundle;
    import android.text.method.ScrollingMovementMethod;
    import android.view.View;
    import android.widget.TextView;

    import com.microsoft.cognitiveservices.speech.audio.AudioConfig;
    import com.microsoft.cognitiveservices.speech.audio.PullAudioOutputStream;
    import com.microsoft.cognitiveservices.speech.dialog.DialogServiceConfig;
    import com.microsoft.cognitiveservices.speech.dialog.DialogServiceConnector;

    import org.json.JSONException;
    import org.json.JSONObject;

    import static android.Manifest.permission.*;

    public class MainActivity extends AppCompatActivity {
        // Replace below with your own speech subscription key
        private static String speechSubscriptionKey = "YourSpeechSubscriptionKey";
        // Replace below with your own speech service region
        private static String serviceRegion = "YourSpeechServiceRegion";

        private DialogServiceConnector connector;

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);

            TextView recoText = (TextView) this.findViewById(R.id.recoText);
            TextView activityText = (TextView) this.findViewById(R.id.activityText);
            recoText.setMovementMethod(new ScrollingMovementMethod());
            activityText.setMovementMethod(new ScrollingMovementMethod());

            // Note: we need to request permissions for audio input and network access
            int requestCode = 5; // unique code for the permission request
            ActivityCompat.requestPermissions(MainActivity.this, new String[]{RECORD_AUDIO, INTERNET}, requestCode);
        }

        public void onBotButtonClicked(View v) {
            // Recreate the DialogServiceConnector on each button press, ensuring that the existing one is closed
            if (connector != null) {
                connector.close();
                connector = null;
            }

            // Create the DialogServiceConnector from speech subscription information
            BotFrameworkConfig config = BotFrameworkConfig.fromSubscription(speechSubscriptionKey, serviceRegion);
            connector = new DialogServiceConnector(config, AudioConfig.fromDefaultMicrophoneInput());

            // Optional step: preemptively connect to reduce first interaction latency
            connector.connectAsync();

            // Register the DialogServiceConnector's event listeners
            registerEventListeners();

            // Begin sending audio to your bot
            connector.listenOnceAsync();
        }

        private void registerEventListeners() {
            TextView recoText = (TextView) this.findViewById(R.id.recoText); // 'recoText' is the ID of your text view
            TextView activityText = (TextView) this.findViewById(R.id.activityText); // 'activityText' is the ID of your text view

            // Recognizing will provide the intermediate recognized text while an audio stream is being processed
            connector.recognizing.addEventListener((o, recoArgs) -> {
                recoText.setText("  Recognizing: " + recoArgs.getResult().getText());
            });

            // Recognized will provide the final recognized text once audio capture is completed
            connector.recognized.addEventListener((o, recoArgs) -> {
                recoText.setText("  Recognized: " + recoArgs.getResult().getText());
            });

            // SessionStarted will notify when audio begins flowing to the service for a turn
            connector.sessionStarted.addEventListener((o, sessionArgs) -> {
                recoText.setText("Listening...");
            });

            // SessionStopped will notify when a turn is complete and it's safe to begin listening again
            connector.sessionStopped.addEventListener((o, sessionArgs) -> {
            });

            // Canceled will be signaled when a turn is aborted or experiences an error condition
            connector.canceled.addEventListener((o, canceledArgs) -> {
                recoText.setText("Canceled (" + canceledArgs.getReason().toString() + ") error details: {}" + canceledArgs.getErrorDetails());
                connector.disconnectAsync();
            });

            // ActivityReceived is the main way your bot will communicate with the client and uses bot framework activities.
            connector.activityReceived.addEventListener((o, activityArgs) -> {
                try {
                    // Here we use JSONObject only to "pretty print" the condensed Activity JSON
                    String rawActivity = activityArgs.getActivity().serialize();
                    String formattedActivity = new JSONObject(rawActivity).toString(2);
                    activityText.setText(formattedActivity);
                } catch (JSONException e) {
                    activityText.setText("Couldn't format activity text: " + e.getMessage());
                }

                if (activityArgs.hasAudio()) {
                    // Text-to-speech audio associated with the activity is 16 kHz 16-bit mono PCM data
                    final int sampleRate = 16000;
                    int bufferSize = AudioTrack.getMinBufferSize(sampleRate, AudioFormat.CHANNEL_OUT_MONO, AudioFormat.ENCODING_PCM_16BIT);

                    AudioTrack track = new AudioTrack(
                            AudioManager.STREAM_MUSIC,
                            sampleRate,
                            AudioFormat.CHANNEL_OUT_MONO,
                            AudioFormat.ENCODING_PCM_16BIT,
                            bufferSize,
                            AudioTrack.MODE_STREAM);

                    track.play();

                    PullAudioOutputStream stream = activityArgs.getAudio();

                    // Audio is streamed as it becomes available. Play it as it arrives.
                    byte[] buffer = new byte[bufferSize];
                    long bytesRead = 0;

                    do {
                        bytesRead = stream.read(buffer);
                        track.write(buffer, 0, (int) bytesRead);
                    } while (bytesRead == bufferSize);

                    track.release();
                }
            });
        }
    }
   ```

   * El método `onCreate` incluye código que solicita permisos para micrófono e Internet.

   * El método `onBotButtonClicked` es, como se indicó anteriormente, el controlador de clic de botón. Al presionar un botón, se desencadena una única interacción ("turno") con el bot.

   * El método `registerEventListeners` muestra los eventos utilizados por `DialogServiceConnector`, así como el control básico de las actividades entrantes.

1. En el mismo archivo, reemplace las cadenas de configuración para que coincidan con los recursos:

    * Reemplace `YourChannelSecret` por el secreto de canal Direct Line Speech para el bot.

    * Reemplace `YourSpeechSubscriptionKey` por la clave de suscripción.

    * Reemplace `YourServiceRegion` por la [región](regions.md) asociada con la suscripción. Actualmente solo se admite un subconjunto de regiones del servicio de voz con Direct Line Speech. Para más información, consulte [Regiones](regions.md#voice-assistants).

## <a name="build-and-run-the-app"></a>Compilación y ejecución de la aplicación

1. Conecte el dispositivo Android al equipo de desarrollo. Asegúrese de tener [habilitado el modo de desarrollo y la depuración USB](https://developer.android.com/studio/debug/dev-options) en el dispositivo.

1. Para compilar la aplicación, presione Ctrl+F9 o elija **Build (Compilar)**  > **Make Project (Crear proyecto)** desde la barra de menús.

1. Para iniciar la aplicación, presione Mayús+F10 o elija **Run (Ejecutar)**  > **Run 'app' (Ejecutar "aplicación")** .

1. En la ventanas de destino de implementación que aparece, elija el dispositivo Android.

   ![Captura de pantalla de la ventana Select Deployment Target (Seleccionar destino de implementación)](media/sdk/qs-java-android-12-deploy.png)

Cuando se hayan iniciado la aplicación y su actividad, haga clic en el botón para comenzar a hablar con el bot. El texto transcrito aparecerá mientras habla y la última actividad que haya recibido del bot aparecerá cuando se reciba. Si el bot está configurado para proporcionar respuestas de voz, la conversión de voz a texto se reproducirá automáticamente.

![Captura de pantalla de la aplicación Android](media/sdk/qs-java-android-assistant-completed-turn.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Creación e implementación de un bot básico](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-basic-deploy?view=azure-bot-service-4.0)

## <a name="see-also"></a>Consulte también
- [Acerca de los asistentes de voz](voice-assistants.md)
- [Obtenga una clave de suscripción gratuita a los servicios de Voz](get-started.md)
- [Palabras clave personalizadas](speech-devices-sdk-create-kws.md)
- [Conexión de Direct Line de Voz al bot](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech)
- [Exploración de ejemplos de Java en GitHub](https://aka.ms/csspeech/samples)
