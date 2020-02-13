---
title: 'Inicio rápido: Asistente de voz personalizado en C# (UWP): servicio de voz'
titleSuffix: Azure Cognitive Services
description: En este artículo, creará una aplicación para Plataforma universal de Windows (UWP) de C# mediante el kit de desarrollo de software (SDK) de Voz de Cognitive Services. Conecta su aplicación cliente a un bot de Bot Framework previamente creado y configurado para usar el canal Direct Line Speech. La aplicación se crea con el paquete NuGet del SDK de voz y Microsoft Visual Studio 2019.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 02/10/2020
ms.author: dapine
ms.openlocfilehash: af9395b8f75a9097d4449571fd54be6259a7b9f3
ms.sourcegitcommit: 7c18afdaf67442eeb537ae3574670541e471463d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2020
ms.locfileid: "77119782"
---
# <a name="quickstart-create-a-voice-assistant-with-the-speech-sdk-uwp"></a>Inicio rápido: Creación de un asistente de voz con el SDK de Voz, UWP

También hay guías de inicio rápido para el [reconocimiento](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=uwp), [la síntesis](~/articles/cognitive-services/Speech-Service/quickstarts/text-to-speech.md?pivots=programming-language-csharp&tabs=uwp) y la [traducción de voz](~/articles/cognitive-services/Speech-Service/quickstarts/translate-speech-to-text.md?pivots=programming-language-csharp&tabs=uwp).

En este artículo, desarrollará una aplicación para Plataforma universal de Windows (UWP) de C# mediante el [SDK de Voz](speech-sdk.md). El programa se conectará a un bot previamente creado y configurado para permitir una experiencia de asistente de voz desde la aplicación cliente. La aplicación se compila con el [paquete NuGet del SDK de voz](https://aka.ms/csspeech/nuget) y Microsoft Visual Studio 2019 (cualquier edición).

> [!NOTE]
> La Plataforma universal de Windows permite desarrollar aplicaciones que se ejecutan en cualquier dispositivo que admite Windows 10, incluidos PC, Xbox, Surface Hub y otros dispositivos.

## <a name="prerequisites"></a>Prerrequisitos

Esta guía de inicio rápido requiere:

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/).
* Una clave de suscripción de Azure para el servicio Voz. [Obtenga una gratis](get-started.md) o créela en [Azure Portal](https://portal.azure.com).
* Un bot creado previamente y configurado con el [canal Direct Line Speech](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech).

  > [!NOTE]
  > Consulte [la lista de regiones admitidas para los asistentes de voz ](regions.md#voice-assistants) y asegúrese de que sus recursos se implementan en una de esas regiones.

## <a name="optional-get-started-fast"></a>Opcional: Empiece rápidamente

En este inicio rápido se describirá, paso a paso, cómo hacer que una aplicación cliente se conecte al bot habilitado para voz. Si prefiere sumergirse de lleno, el código fuente completo y listo para compilar utilizado en este inicio rápido está disponible en los [ejemplos del SDK de Voz](https://aka.ms/csspeech/samples) bajo la carpeta `quickstart`.

## <a name="create-a-visual-studio-project"></a>Creación de un proyecto de Visual Studio

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-uwp-create-proj.md)]

## <a name="add-sample-code"></a>Incorporación de código de ejemplo

Ahora, agregue el código XAML que define la interfaz de usuario de la aplicación y agregue la implementación de código C# subyacente.

### <a name="xaml-code"></a>Código XAML

En primer lugar, creará la interfaz de usuario de la aplicación; para ello, agregará el código XAML:

1. En el **Explorador de soluciones**, abra `MainPage.xaml`.

1. En la vista XAML del diseñador, reemplace todo el contenido por el siguiente fragmento de código:

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
            <StackPanel Orientation="Vertical" HorizontalAlignment="Center"  
                        Margin="20,50,0,0" VerticalAlignment="Center" Width="800">
                <Button x:Name="EnableMicrophoneButton" Content="Enable Microphone"  
                        Margin="0,0,10,0" Click="EnableMicrophone_ButtonClicked" 
                        Height="35"/>
                <Button x:Name="ListenButton" Content="Talk to your bot" 
                        Margin="0,10,10,0" Click="ListenButton_ButtonClicked" 
                        Height="35"/>
                <StackPanel x:Name="StatusPanel" Orientation="Vertical" 
                            RelativePanel.AlignBottomWithPanel="True" 
                            RelativePanel.AlignRightWithPanel="True" 
                            RelativePanel.AlignLeftWithPanel="True">
                    <TextBlock x:Name="StatusLabel" Margin="0,10,10,0" 
                               TextWrapping="Wrap" Text="Status:" FontSize="20"/>
                    <Border x:Name="StatusBorder" Margin="0,0,0,0">
                        <ScrollViewer VerticalScrollMode="Auto"  
                                      VerticalScrollBarVisibility="Auto" MaxHeight="200">
                            <!-- Use LiveSetting to enable screen readers to announce 
                                 the status update. -->
                            <TextBlock 
                                x:Name="StatusBlock" FontWeight="Bold" 
                                AutomationProperties.LiveSetting="Assertive"
                                MaxWidth="{Binding ElementName=Splitter, Path=ActualWidth}" 
                                Margin="10,10,10,20" TextWrapping="Wrap"  />
                        </ScrollViewer>
                    </Border>
                </StackPanel>
            </StackPanel>
            <MediaElement x:Name="mediaElement"/>
        </Grid>
    </Page>
    ```

La vista Diseño se actualiza para mostrar la interfaz de usuario de la aplicación.

### <a name="c-code-behind-source"></a>Código subyacente de C#

A continuación, agregue el código subyacente para que la aplicación funcione según lo previsto. El código fuente subyacente incluye:

- Instrucciones `using` para los espacios de nombres `Speech` y `Speech.Dialog`.
- Una implementación sencilla para garantizar el acceso del micrófono, conectado a un controlador de botón
- Asistentes básicos de la interfaz de usuario para presentar los errores y mensajes en la aplicación
- Un punto de aterrizaje para la ruta de acceso del código de inicialización que se rellenará más tarde
- Una asistente para la reproducción de texto a voz (sin compatibilidad con streaming)
- Un controlador de botón vacía para empezar a escuchar que se rellenará más tarde

Para agregar el código fuente subyacente, siga estos pasos:

1. En **Explorador de soluciones**, abra el archivo de código fuente subyacente `MainPage.xaml.cs`. (Se agrupa en `MainPage.xaml`).

1. Reemplace el contenido del archivo por el siguiente fragmento de código:

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
            private DialogServiceConnector connector;

            private enum NotifyType
            {
                StatusMessage,
                ErrorMessage
            };

            public MainPage()
            {
                this.InitializeComponent();
            }

            private async void EnableMicrophone_ButtonClicked(
                object sender, RoutedEventArgs e)
            {
                bool isMicAvailable = true;
                try
                {
                    var mediaCapture = new Windows.Media.Capture.MediaCapture();
                    var settings = 
                        new Windows.Media.Capture.MediaCaptureInitializationSettings();
                    settings.StreamingCaptureMode = 
                        Windows.Media.Capture.StreamingCaptureMode.Audio;
                    await mediaCapture.InitializeAsync(settings);
                }
                catch (Exception)
                {
                    isMicAvailable = false;
                }
                if (!isMicAvailable)
                {
                    await Windows.System.Launcher.LaunchUriAsync(
                        new Uri("ms-settings:privacy-microphone"));
                }
                else
                {
                    NotifyUser("Microphone was enabled", NotifyType.StatusMessage);
                }
            }

            private void NotifyUser(
                string strMessage, NotifyType type = NotifyType.StatusMessage)
            {
                // If called from the UI thread, then update immediately.
                // Otherwise, schedule a task on the UI thread to perform the update.
                if (Dispatcher.HasThreadAccess)
                {
                    UpdateStatus(strMessage, type);
                }
                else
                {
                    var task = Dispatcher.RunAsync(
                        Windows.UI.Core.CoreDispatcherPriority.Normal, 
                        () => UpdateStatus(strMessage, type));
                }
            }

            private void UpdateStatus(string strMessage, NotifyType type)
            {
                switch (type)
                {
                    case NotifyType.StatusMessage:
                        StatusBorder.Background = new SolidColorBrush(
                            Windows.UI.Colors.Green);
                        break;
                    case NotifyType.ErrorMessage:
                        StatusBorder.Background = new SolidColorBrush(
                            Windows.UI.Colors.Red);
                        break;
                }
                StatusBlock.Text += string.IsNullOrEmpty(StatusBlock.Text) 
                    ? strMessage : "\n" + strMessage;

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
                // Raise an event if necessary to enable a screen reader 
                // to announce the status update.
                var peer = Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer.FromElement(StatusBlock);
                if (peer != null)
                {
                    peer.RaiseAutomationEvent(
                        Windows.UI.Xaml.Automation.Peers.AutomationEvents.LiveRegionChanged);
                }
            }

            // Waits for and accumulates all audio associated with a given 
            // PullAudioOutputStream and then plays it to the MediaElement. Long spoken 
            // audio will create extra latency and a streaming playback solution 
            // (that plays audio while it continues to be received) should be used -- 
            // see the samples for examples of this.
            private void SynchronouslyPlayActivityAudio(
                PullAudioOutputStream activityAudio)
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

                var task = Dispatcher.RunAsync(
                    Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
                {
                    mediaElement.SetSource(
                        playbackStreamWithHeader.AsRandomAccessStream(), "audio/wav");
                    mediaElement.Play();
                });
            }

            private void InitializeDialogServiceConnector()
            {
                // New code will go here
            }

            private async void ListenButton_ButtonClicked(
                object sender, RoutedEventArgs e)
            {
                // New code will go here
            }
        }
    }
    ```

1. Agregue el siguiente fragmento de código al cuerpo del método de `InitializeDialogServiceConnector`. Este código crea el `DialogServiceConnector` con la información de la suscripción.

    ```csharp
    // Create a BotFrameworkConfig by providing a Speech service subscription key
    // the RecoLanguage property is optional (default en-US)
    const string speechSubscriptionKey = "YourSpeechSubscriptionKey"; // Your subscription key
    const string region = "YourServiceRegion"; // Your subscription service region.

    var botConfig = BotFrameworkConfig.FromSubscription(speechSubscriptionKey, region);
    botConfig.SetProperty(PropertyId.SpeechServiceConnection_RecoLanguage, "en-US");
    connector = new DialogServiceConnector(botConfig);
    ```

   > [!NOTE]
   > Consulte [la lista de regiones admitidas para los asistentes de voz ](regions.md#voice-assistants) y asegúrese de que sus recursos se implementan en una de esas regiones.

   > [!NOTE]
   > Para obtener información sobre la configuración del bot y la recuperación del secreto de un canal, consulte la documentación de Bot Framework para el [canal Direct Line Speech](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech).

1. Agregue las cadenas `YourChannelSecret`, `YourSpeechSubscriptionKey` y `YourServiceRegion` con sus propios valores para el bot, la suscripción de voz y la [región](regions.md).

1. Anexe el siguiente fragmento de código al final del cuerpo del método de `InitializeDialogServiceConnector`. Este código configura los controladores de los eventos en los que se basa `DialogServiceConnector` comunicar sus actividades de bot, los resultados del reconocimiento de voz y otra información.

    ```csharp
    // ActivityReceived is the main way your bot will communicate with the client 
    // and uses bot framework activities
    connector.ActivityReceived += (sender, activityReceivedEventArgs) =>
    {
        NotifyUser(
            $"Activity received, hasAudio={activityReceivedEventArgs.HasAudio} activity={activityReceivedEventArgs.Activity}");

        if (activityReceivedEventArgs.HasAudio)
        {
            SynchronouslyPlayActivityAudio(activityReceivedEventArgs.Audio);
        }
    };

    // Canceled will be signaled when a turn is aborted or experiences an error condition
    connector.Canceled += (sender, canceledEventArgs) =>
    {
        NotifyUser($"Canceled, reason={canceledEventArgs.Reason}");
        if (canceledEventArgs.Reason == CancellationReason.Error)
        {
            NotifyUser(
                $"Error: code={canceledEventArgs.ErrorCode}, details={canceledEventArgs.ErrorDetails}");
        }
    };

    // Recognizing (not 'Recognized') will provide the intermediate recognized text 
    // while an audio stream is being processed
    connector.Recognizing += (sender, recognitionEventArgs) =>
    {
        NotifyUser($"Recognizing! in-progress text={recognitionEventArgs.Result.Text}");
    };

    // Recognized (not 'Recognizing') will provide the final recognized text 
    // once audio capture is completed
    connector.Recognized += (sender, recognitionEventArgs) =>
    {
        NotifyUser($"Final speech-to-text result: '{recognitionEventArgs.Result.Text}'");
    };

    // SessionStarted will notify when audio begins flowing to the service for a turn
    connector.SessionStarted += (sender, sessionEventArgs) =>
    {
        NotifyUser($"Now Listening! Session started, id={sessionEventArgs.SessionId}");
    };

    // SessionStopped will notify when a turn is complete and 
    // it's safe to begin listening again
    connector.SessionStopped += (sender, sessionEventArgs) =>
    {
        NotifyUser($"Listening complete. Session ended, id={sessionEventArgs.SessionId}");
    };
    ```

1. Agregue el siguiente fragmento de código al cuerpo del método `ListenButton_ButtonClicked` de la clase `MainPage`. Este código configura `DialogServiceConnector` para escuchar, ya que ya ha establecido la configuración y ha registrado los controladores de eventos.

    ```csharp
    if (connector == null)
    {
        InitializeDialogServiceConnector();
        // Optional step to speed up first interaction: if not called, 
        // connection happens automatically on first use
        var connectTask = connector.ConnectAsync();
    }

    try
    {
        // Start sending audio to your speech-enabled bot
        var listenTask = connector.ListenOnceAsync();

        // You can also send activities to your bot as JSON strings -- 
        // Microsoft.Bot.Schema can simplify this
        string speakActivity = 
            @"{""type"":""message"",""text"":""Greeting Message"", ""speak"":""Hello there!""}";
        await connector.SendActivityAsync(speakActivity);

    }
    catch (Exception ex)
    {
        NotifyUser($"Exception: {ex.ToString()}", NotifyType.ErrorMessage);
    }
    ```

1. En la barra de menús, elija **Archivo** > **Guardar todo** para guardar los cambios.

## <a name="build-and-run-the-application"></a>Compilación y ejecución de la aplicación

Ahora está listo para compilar y probar la aplicación.

1. En la barra de menús, elija **Compilar** > **Compilar solución** para compilar la aplicación. El código se debería compilar sin errores ahora.

1. Elija **Depurar** > **Iniciar depuración** o presione **F5** para iniciar la aplicación. Aparece la ventana **HelloWorld**.

   ![Aplicación de asistente de voz para UWP de ejemplo en C#: inicio rápido](media/sdk/qs-voice-assistant-uwp-helloworld-window.png)

1. Seleccione **Habilitar micrófono** y, cuando aparezca la solicitud de permiso de acceso, seleccione **Sí**.

   ![Solicitud de permiso de acceso al micrófono](media/sdk/qs-csharp-uwp-10-access-prompt.png)

1. Seleccione **Talk to your bot** (Hablar con el bot) y diga una expresión o frase en inglés en el micrófono del dispositivo. Lo que diga se transmitirá al canal Direct Line Speech y se transcribirá en texto, que aparece en la misma ventana.
<!--
    ![Successful bot response](media/voice-assistants/quickstart-cs-uwp-bot-successful-turn.png)
-->
## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Creación e implementación de un bot básico](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-basic-deploy?view=azure-bot-service-4.0)

## <a name="see-also"></a>Consulte también

- [Acerca de los asistentes de voz](voice-assistants.md)
- [Obtenga una clave de suscripción gratuita a los servicios de Voz](get-started.md)
- [Palabras clave personalizadas](speech-devices-sdk-create-kws.md)
- [Conexión de Direct Line de Voz al bot](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech)
- [Exploración de ejemplos de C# en GitHub](https://aka.ms/csspeech/samples)
