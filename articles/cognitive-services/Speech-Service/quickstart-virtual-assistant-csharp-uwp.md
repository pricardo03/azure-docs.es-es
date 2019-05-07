---
title: 'Inicio rápido: Asistente virtual personalizado por voz (versión preliminar), C# (UWP): servicios de voz'
titleSuffix: Azure Cognitive Services
description: En este artículo, creará una aplicación para Plataforma universal de Windows (UWP) de C# mediante el kit de desarrollo de software (SDK) de Voz de Cognitive Services. Conecta su aplicación cliente a un bot de Bot Framework previamente creado y configurado para usar el canal Direct Line Speech. La aplicación se crea con el paquete NuGet del SDK de Voz y Microsoft Visual Studio 2017.
services: cognitive-services
author: trrwilson
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 05/02/2019
ms.author: travisw
ms.custom: ''
ms.openlocfilehash: 7dd469b07727e1efa326a1d92ea778bd3fb2cab1
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65025377"
---
# <a name="quickstart-create-a-voice-first-virtual-assistant-with-the-speech-sdk-uwp"></a>Inicio rápido: Creación de un asistente virtual de voz con el SDK de Voz, UWP

En este artículo, desarrollará una aplicación para Plataforma universal de Windows (UWP) de C# mediante el [SDK de Voz](speech-sdk.md). El programa se conectará a un bot previamente creado y configurado para permitir una experiencia de asistente virtual por voz desde la aplicación cliente. La aplicación se compila con el [paquete NuGet del SDK de Voz](https://aka.ms/csspeech/nuget) y Microsoft Visual Studio 2017 (cualquier edición).

> [!NOTE]
> La Plataforma universal de Windows permite desarrollar aplicaciones que se ejecutan en cualquier dispositivo que admite Windows 10, incluidos PC, Xbox, Surface Hub y otros dispositivos.

## <a name="prerequisites"></a>Requisitos previos

Esta guía de inicio rápido requiere:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Una clave de suscripción de Azure para el servicio Voz. [Obtenga una gratis](get-started.md).
* Un bot creado previamente y configurado con el [canal Direct Line Speech](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech.md)

    > [!NOTE]
    > En la versión preliminar, el canal Direct Line Speech actualmente solo admite la región **westus2**.

    > [!NOTE]
    > La prueba de 30 días para el plan de tarifa estándar descrito en [Prueba gratuita de los servicios de voz](get-started.md) está restringida a **westus** (no **westus2**) y por lo tanto no es compatible con el servicio de voz de Direct Line. Las suscripciones del nivel gratis y estándar de **westus2** son compatibles.

## <a name="optional-get-started-fast"></a>Opcional: Empiece rápidamente

En este inicio rápido se describirá, paso a paso, cómo hacer que una aplicación cliente simple se conecte al bot habilitado para voz. Si prefiere sumergirse de lleno, el código fuente completo y listo para compilar utilizado en este inicio rápido está disponible en los [ejemplos del SDK de Voz](https://aka.ms/csspeech/samples) bajo la carpeta `quickstart`.

## <a name="create-a-visual-studio-project"></a>Creación de un proyecto de Visual Studio

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-uwp-create-proj.md)]

## <a name="add-sample-code"></a>Incorporación de código de ejemplo

1. La interfaz de usuario de la aplicación se define mediante el uso de XAML. Abra `MainPage.xaml` en el Explorador de soluciones. En la vista XAML del diseñador, reemplace todo el contenido por el siguiente.

    ```xml
    <Page
        x:Class="helloworld.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:helloworld"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d"
        Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    
        <Grid>
            <StackPanel Orientation="Vertical" HorizontalAlignment="Center"  Margin="20,50,0,0" VerticalAlignment="Center" Width="800">
                <Button x:Name="EnableMicrophoneButton" Content="Enable Microphone"  Margin="0,0,10,0" Click="EnableMicrophone_ButtonClicked" Height="35"/>
                <Button x:Name="ListenButton" Content="Talk to your bot" Margin="0,10,10,0" Click="ListenButton_ButtonClicked" Height="35"/>
                <StackPanel x:Name="StatusPanel" Orientation="Vertical" RelativePanel.AlignBottomWithPanel="True" RelativePanel.AlignRightWithPanel="True" RelativePanel.AlignLeftWithPanel="True">
                    <TextBlock x:Name="StatusLabel" Margin="0,10,10,0" TextWrapping="Wrap" Text="Status:" FontSize="20"/>
                    <Border x:Name="StatusBorder" Margin="0,0,0,0">
                        <ScrollViewer VerticalScrollMode="Auto"  VerticalScrollBarVisibility="Auto" MaxHeight="200">
                            <!-- Use LiveSetting to enable screen readers to announce the status update. -->
                            <TextBlock x:Name="StatusBlock" FontWeight="Bold" AutomationProperties.LiveSetting="Assertive"
                    MaxWidth="{Binding ElementName=Splitter, Path=ActualWidth}" Margin="10,10,10,20" TextWrapping="Wrap"  />
                        </ScrollViewer>
                    </Border>
                </StackPanel>
            </StackPanel>
            <MediaElement x:Name="mediaElement"/>
        </Grid>
    </Page>
        ```

1. Open the code-behind source file `MainPage.xaml.cs`. You'll find it grouped under `MainPage.xaml`. Replace the contents with the code below. Here's what this sample covers: 

    * Using statements for the Speech and Speech.Dialog namespaces
    * A simple implementation to ensure microphone access, wired to a button handler
    * Basic UI helpers to present messages and errors in the application
    * A landing point for the initialization code path that will be populated later
    * A helper to play back text-to-speech (without streaming support)
    * An empty button handler to start listening that will be populated later

    ```csharp
    using Microsoft.CognitiveServices.Speech;
    using Microsoft.CognitiveServices.Speech.Audio;
    using Microsoft.CognitiveServices.Speech.Dialog;
    using System;
    using System.Diagnostics;
    using System.IO;
    using System.Text;
    using Windows.Foundation;
    using Windows.Storage.Streams;
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Controls;
    using Windows.UI.Xaml.Media;

    namespace helloworld
    {
        public sealed partial class MainPage : Page
        {
            private SpeechBotConnector botConnector;

            private enum NotifyType
            {
                StatusMessage,
                ErrorMessage
            };

            public MainPage()
            {
                this.InitializeComponent();
            }

            private async void EnableMicrophone_ButtonClicked(object sender, RoutedEventArgs e)
            {
                bool isMicAvailable = true;
                try
                {
                    var mediaCapture = new Windows.Media.Capture.MediaCapture();
                    var settings = new Windows.Media.Capture.MediaCaptureInitializationSettings();
                    settings.StreamingCaptureMode = Windows.Media.Capture.StreamingCaptureMode.Audio;
                    await mediaCapture.InitializeAsync(settings);
                }
                catch (Exception)
                {
                    isMicAvailable = false;
                }
                if (!isMicAvailable)
                {
                    await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-microphone"));
                }
                else
                {
                    NotifyUser("Microphone was enabled", NotifyType.StatusMessage);
                }
            }

            private void NotifyUser(string strMessage, NotifyType type = NotifyType.StatusMessage)
            {
                // If called from the UI thread, then update immediately.
                // Otherwise, schedule a task on the UI thread to perform the update.
                if (Dispatcher.HasThreadAccess)
                {
                    UpdateStatus(strMessage, type);
                }
                else
                {
                    var task = Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () => UpdateStatus(strMessage, type));
                }
            }

            private void UpdateStatus(string strMessage, NotifyType type)
            {
                switch (type)
                {
                    case NotifyType.StatusMessage:
                        StatusBorder.Background = new SolidColorBrush(Windows.UI.Colors.Green);
                        break;
                    case NotifyType.ErrorMessage:
                        StatusBorder.Background = new SolidColorBrush(Windows.UI.Colors.Red);
                        break;
                }
                StatusBlock.Text += string.IsNullOrEmpty(StatusBlock.Text) ? strMessage : "\n" + strMessage;

                if (!string.IsNullOrEmpty(StatusBlock.Text))
                {
                    StatusBorder.Visibility = Visibility.Visible;
                    StatusPanel.Visibility = Visibility.Visible;
                }
                else
                {
                    StatusBorder.Visibility = Visibility.Collapsed;
                    StatusPanel.Visibility = Visibility.Collapsed;
                }
                // Raise an event if necessary to enable a screen reader to announce the status update.
                var peer = Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer.FromElement(StatusBlock);
                if (peer != null)
                {
                    peer.RaiseAutomationEvent(Windows.UI.Xaml.Automation.Peers.AutomationEvents.LiveRegionChanged);
                }
            }

            // Waits for accumulates all audio associated with a given PullAudioOutputStream and then plays it to the
            // MediaElement. Long spoken audio will create extra latency and a streaming playback solution (that plays
            // audio while it continues to be received) should be used -- see the samples for examples of this.
            private void SynchronouslyPlayActivityAudio(PullAudioOutputStream activityAudio)
            {
                var playbackStreamWithHeader = new MemoryStream();
                playbackStreamWithHeader.Write(Encoding.ASCII.GetBytes("RIFF"), 0, 4); // ChunkID
                playbackStreamWithHeader.Write(BitConverter.GetBytes(UInt32.MaxValue), 0, 4); // ChunkSize: max
                playbackStreamWithHeader.Write(Encoding.ASCII.GetBytes("WAVE"), 0, 4); // Format
                playbackStreamWithHeader.Write(Encoding.ASCII.GetBytes("fmt "), 0, 4); // Subchunk1ID
                playbackStreamWithHeader.Write(BitConverter.GetBytes(16), 0, 4); // Subchunk1Size: PCM
                playbackStreamWithHeader.Write(BitConverter.GetBytes(1), 0, 2); // AudioFormat: PCM
                playbackStreamWithHeader.Write(BitConverter.GetBytes(1), 0, 2); // NumChannels: mono
                playbackStreamWithHeader.Write(BitConverter.GetBytes(16000), 0, 4); // SampleRate: 16kHz
                playbackStreamWithHeader.Write(BitConverter.GetBytes(32000), 0, 4); // ByteRate
                playbackStreamWithHeader.Write(BitConverter.GetBytes(2), 0, 2); // BlockAlign
                playbackStreamWithHeader.Write(BitConverter.GetBytes(16), 0, 2); // BitsPerSample: 16-bit
                playbackStreamWithHeader.Write(Encoding.ASCII.GetBytes("data"), 0, 4); // Subchunk2ID
                playbackStreamWithHeader.Write(BitConverter.GetBytes(UInt32.MaxValue), 0, 4); // Subchunk2Size

                byte[] pullBuffer = new byte[2056];

                uint lastRead = 0;
                do
                {
                    lastRead = activityAudio.Read(pullBuffer);
                    playbackStreamWithHeader.Write(pullBuffer, 0, (int)lastRead);
                }
                while (lastRead == pullBuffer.Length);

                var task = Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
                {
                    mediaElement.SetSource(playbackStreamWithHeader.AsRandomAccessStream(), "audio/wav");
                    mediaElement.Play();
                });
            }

            private void InitializeBotConnector()
            {
                // New code will go here
            }

            private async void ListenButton_ButtonClicked(object sender, RoutedEventArgs e)
            {
                // New code will go here
            }
        }
    }
     ```

1. A continuación, creará el `SpeechBotConnector` con la información de la suscripción. Agregue lo siguiente al cuerpo del método de `InitializeBotConnector`, reemplazando las cadenas `YourChannelSecret`, `YourSpeechSubscriptionKey` y `YourServiceRegion` por sus propios valores para el bot, suscripción de voz y [región](regions.md).

    > [!NOTE]
    > En la versión preliminar, el canal Direct Line Speech actualmente solo admite la región **westus2**.

    > [!NOTE]
    > Para obtener información sobre la configuración del bot y la recuperación del secreto de un canal, consulte la documentación de Bot Framework para el [canal Direct Line Speech](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech.md).

    ```csharp
    // create a BotConnectorConfig by providing a bot secret key and Cognitive Services subscription key
    // the RecoLanguage property is optional (default en-US); note that only en-US is supported in Preview
    const string channelSecret = "YourChannelSecret";
    const string speechSubscriptionKey = "YourSpeechSubscriptionKey";
    const string region = "YourServiceRegion"; // note: this is assumed as westus2 for preview

    var botConnectorConfig = BotConnectorConfig.FromSecretKey(channelSecret, speechSubscriptionKey, region);
    botConnectorConfig.SetProperty(PropertyId.SpeechServiceConnection_RecoLanguage, "en-US");
    botConnector = new SpeechBotConnector(botConnectorConfig);
    ```

1. `SpeechBotConnector` se basa en varios eventos para comunicar sus actividades de bot, los resultados del reconocimiento de voz y otra información. Agregue controladores para estos eventos mediante la anexión de lo siguiente al final del cuerpo del método de `InitializeBotConnector`.

    ```csharp
    // ActivityReceived is the main way your bot will communicate with the client and uses bot framework activities
    botConnector.ActivityReceived += async (sender, activityReceivedEventArgs) =>
    {
        NotifyUser($"Activity received, hasAudio={activityReceivedEventArgs.HasAudio} activity={activityReceivedEventArgs.Activity}");

        if (activityReceivedEventArgs.HasAudio)
        {
            SynchronouslyPlayActivityAudio(activityReceivedEventArgs.Audio);
        }
    };
    // Canceled will be signaled when a turn is aborted or experiences an error condition
    botConnector.Canceled += (sender, canceledEventArgs) =>
    {
        NotifyUser($"Canceled, reason={canceledEventArgs.Reason}");
        if (canceledEventArgs.Reason == CancellationReason.Error)
        {
            NotifyUser($"Error: code={canceledEventArgs.ErrorCode}, details={canceledEventArgs.ErrorDetails}");
        }
    };
    // Recognizing (not 'Recognized') will provide the intermediate recognized text while an audio stream is being processed
    botConnector.Recognizing += (sender, recognitionEventArgs) =>
    {
        NotifyUser($"Recognizing! in-progress text={recognitionEventArgs.Result.Text}");
    };
    // Recognized (not 'Recognizing') will provide the final recognized text once audio capture is completed
    botConnector.Recognized += (sender, recognitionEventArgs) =>
    {
        NotifyUser($"Final speech-to-text result: '{recognitionEventArgs.Result.Text}'");
    };
    // SessionStarted will notify when audio begins flowing to the service for a turn
    botConnector.SessionStarted += (sender, sessionEventArgs) =>
    {
        NotifyUser($"Now Listening! Session started, id={sessionEventArgs.SessionId}");
    };
    // SessionStopped will notify when a turn is complete and it's safe to begin listening again
    botConnector.SessionStopped += (sender, sessionEventArgs) =>
    {
        NotifyUser($"Listening complete. Session ended, id={sessionEventArgs.SessionId}");
    };
    ```

1. Con la configuración establecida y los controladores de eventos registrados, el `SpeechBotConnector` ahora solo necesita escuchar. Agregue lo siguiente al método `ListenButton_ButtonClicked` de la clase `MainPage`.

    ```csharp
    private async void ListenButton_ButtonClicked(object sender, RoutedEventArgs e)
    {
        if (botConnector == null)
        {
            InitializeBotConnector();
            // Optional step to speed up first interaction: if not called, connection happens automatically on first use
            var connectTask = botConnector.ConnectAsync();
        }

        try
        {
            // Start sending audio to your speech-enabled bot
            var listenTask = botConnector.ListenOnceAsync();

            // You can also send activities to your bot as JSON strings -- Microsoft.Bot.Schema can simplify this
            string speakActivity = @"{""type"":""message"",""text"":""Greeting Message"", ""speak"":""Hello there!""}";
            await botConnector.SendActivityAsync(speakActivity);

        }
        catch (Exception ex)
        {
            NotifyUser($"Exception: {ex.ToString()}", NotifyType.ErrorMessage);
        }
    }
    ```

1. Guarde todos los cambios en el proyecto.

## <a name="build-and-run-the-app"></a>Compilación y ejecución de la aplicación

1. Compile la aplicación. En la barra de menús de Visual Studio, seleccione **Compilar** > **Compilar solución**. El código se debería compilar sin errores ahora.

    ![Captura de pantalla de la aplicación de Visual Studio, con la opción Generar solución resaltada](media/sdk/qs-csharp-uwp-08-build.png "Compilación correcta")

1. Inicie la aplicación. En la barra de menús de Visual Studio, seleccione **Depurar** > **Iniciar depuración** o bien presione **F5**.

    ![Captura de pantalla de la aplicación de Visual Studio, con la opción Iniciar depuración resaltada](media/sdk/qs-csharp-uwp-09-start-debugging.png "Iniciar la aplicación en depuración")

1. Aparece una ventana. En la aplicación, seleccione **Enable Microphone** (Habilitar micrófono) y confirme la solicitud de permiso emergente.

    ![Captura de pantalla de la solicitud de permiso](media/sdk/qs-csharp-uwp-10-access-prompt.png "Iniciar la aplicación en depuración")

1. Seleccione **Talk to your bot** (Hablar con el bot) y diga una expresión o frase en inglés en el micrófono del dispositivo. Lo que diga se transmitirá al canal Direct Line Speech y se transcribirá en texto, que aparece en la misma ventana.
quickstart-cs-uwp-bot-successful-turn

    ![Captura de pantalla de un giro de bot correcto](media/voice-first-virtual-assistants/quickstart-cs-uwp-bot-successful-turn.png "Un giro de bot correcto")

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Exploración de ejemplos de C# en GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Otras referencias

- [Traducción de voz](how-to-translate-speech-csharp.md)
- [Personalización de modelos acústicos](how-to-customize-acoustic-models.md)
- [Personalización de modelos de lenguaje](how-to-customize-language-model.md)
