---
title: 'Tutorial: Translator Speech API, C#'
titleSuffix: Azure Cognitive Services
description: Use Translator Speech API para convertir texto en tiempo real.
services: cognitive-services
author: v-jerkin
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-speech
ms.topic: tutorial
ms.date: 3/5/2018
ms.author: v-jerkin
ROBOTS: NOINDEX
ms.openlocfilehash: 2de56366c3204e77eb2e6775ddd88b6fc4f0c219
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46993874"
---
# <a name="tutorial-translator-speech-application-in-c"></a>Tutorial: aplicación Translator Speech en C#

En este tutorial se realiza un recorrido por una herramienta interactiva de traducción de voz que utiliza Translator Speech API, que forma parte de Azure Cognitive Services. Aprenderá a:

> [!div class="checklist"]
> * Solicitar una lista de los idiomas admitidos por el servicio.
> * Capturar audio y transmitirlo al servicio.
> * Recibir y mostrar traducciones de voz como texto.
> * Reproducir, si lo desea, una versión hablada (conversión texto a voz) de la traducción.

Está disponible un archivo de la solución de Visual Studio para esta aplicación en [GitHub](https://github.com/MicrosoftTranslator/SpeechTranslator).

## <a name="prerequisites"></a>Requisitos previos

Para este tutorial, necesita cualquier edición de Visual Studio 2017, incluida Community Edition. 

La solución de Visual Studio compila también un instalador para la aplicación. Necesita [WiX Toolset](http://wixtoolset.org/) y [WiX Toolset Visual Studio Extension](https://marketplace.visualstudio.com/items?itemName=RobMensching.WixToolsetVisualStudio2017Extension) para la compatibilidad de esta funcionalidad.

También necesita una clave de suscripción para el servicio Translator Speech, que puede obtener en el panel de Microsoft Azure. Hay un plan de precios gratuito que permite traducir hasta 10 horas de voz al mes sin cargo. Este plan es suficiente para este tutorial.

También se requiere la [biblioteca JSON.Net](https://www.newtonsoft.com/json) de terceros (de Newtonsoft). NuGet instala automáticamente este ensamblado si se habilitan ambas casillas de la restauración de paquetes en las opciones de Visual Studio.

## <a name="trying-the-translation-app"></a>Prueba de la aplicación de traducción

Después de abrir la solución Translator Speech (`SpeechTranslator.sln`) en Visual STudio, presione F5 para compilar e iniciar la aplicación.  Aparece la ventana principal del programa.

![[Ventana principal del traductor de voz]](media/speech-translator-main-window.png)

En la primera ejecución, elija **Configuración de la cuenta** en el menú **Configuración** para abrir la ventana que se muestra aquí.

![[Ventana principal del traductor de voz]](media/speech-translator-settings-window.png)

Pegue la clave de suscripción de Translator Speech en esta ventana y haga clic en **Guardar.** La clave se guarda entre ejecuciones.

De nuevo en la ventana principal, elija los dispositivos de entrada y salida de audio que quiera usar, así como los idiomas de origen y destino. Si desea oír el audio de la traducción, asegúrese de que la opción **TTS** (conversión de texto a voz) esté marcada. Si desea ver traducciones parciales especulativas mientras habla, habilite la opción **Resultados parciales**.

Por último, haga clic en **Iniciar** para comenzar la traducción. Diga algo que quiera traducir y observe que el texto reconocido y la traducción aparecen en la ventana. Si ha activado la opción TTS, también se oye la traducción.

## <a name="obtaining-supported-languages"></a>Obtención de idiomas admitidos

En el momento de escribir este documento, el servicio Translator Speech admite más de cinco docenas de idiomas para la traducción de texto. Para la traducción de voz, admite un número de idiomas menor. Estos idiomas requieren compatibilidad tanto para la transcripción (reconocimiento de voz) como para la salida de texto a voz, síntesis.

Es decir, para la traducción de voz, el idioma de origen debe ser compatible para la transcripción. El idioma de salida puede ser cualquiera de los idiomas admitidos para la traducción de texto, suponiendo que quiere un resultado de texto. Si desea salida de voz, solo puede traducir a un idioma admitido para la conversión de texto a voz.

Microsoft puede agregar compatibilidad para nuevos idiomas de vez en cuando. Por este motivo, no debe codificar ninguna información de los idiomas admitidos en la aplicación. En su lugar, Translator Speech API proporciona un punto de conexión de idiomas que permite recuperar los idiomas admitidos en tiempo de ejecución. Puede optar por recibir una o varias listas de idiomas: 

| | |
|-|-|
|`speech`|Los idiomas admitidos para la transcripción de voz. Pueden ser idiomas de origen para la traducción de voz.|
|`text`|Los idiomas admitidos para la traducción de texto a texto. Pueden ser idiomas de destino para la traducción de voz cuando se utiliza la salida de texto.|
|`tts`|Las voces compatibles para la síntesis de voz, cada una de ellas asociada a un idioma determinado. Pueden ser idiomas de destino para la traducción de voz cuando se utiliza la conversión de texto a voz. Un idioma determinado puede admitirse en más de una voz.|

El punto de conexión de idiomas no requiere una clave de suscripción y su utilización no se tiene en cuenta para la cuota. Su URI es `https://dev.microsofttranslator.com/languages` y devuelve sus resultados en formato JSON.

El método `UpdateLanguageSettingsAsync()` de `MainWindow.xaml.cs`, mostrado a continuación, llama al punto de conexión de idiomas para obtener la lista de los idiomas admitidos. 

```csharp
private async Task UpdateLanguageSettingsAsync()
{
    Uri baseUri = new Uri("https://" + baseUrl);
    string fullUriString = "/Languages?api-version=1.0&scope=text,speech,tts";
    if (MenuItem_Experimental.IsChecked) fullUriString += "&flight=experimental";            
    Uri fullUri = new Uri(baseUri, fullUriString);

    using (HttpClient client = new HttpClient()) //'client' is the var - using statement ensures the dispose method is used even after an exception.
    {
        HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, fullUri);

        // get language names for current UI culture:
        request.Headers.Add("Accept-Language", CultureInfo.CurrentUICulture.TwoLetterISOLanguageName);

        // add a client-side trace Id. In case of issues, one can contact support and provide this:
        //string traceId = "SpeechTranslator" + Guid.NewGuid().ToString();
        //request.Headers.Add("X-ClientTraceId", traceId);
        //Debug.Print("TraceId: {0}", traceId);

        client.Timeout = TimeSpan.FromMilliseconds(10000);
        HttpResponseMessage response = await client.SendAsync(request); //make the async call to the web using the client var and passing the built up URI
        response.EnsureSuccessStatusCode(); //causes exception if the return is false

        Debug.Print("Request Id returned: {0}", GetRequestId(response));

        //create dictionaries to hold the language specific data
        spokenLanguages = new Dictionary<string, string>();
        fromLanguages = new Dictionary<string, string>();
        textLanguages = new Dictionary<string, string>();
        isLTR = new Dictionary<string, bool>();
        voices = new Dictionary<string, List<TTsDetail>>();

        JObject jResponse = JObject.Parse(await response.Content.ReadAsStringAsync()); //get the json from the async call with the response var created above, parse it and put it in a var called jResponse - JObject is a newton class

        //Gather the set of TTS voices
        foreach (JProperty jTts in jResponse["tts"])
        {
            JObject ttsDetails = (JObject)jTts.Value;

            string code = jTts.Name;
            string language = ttsDetails["language"].ToString();
            string displayName = ttsDetails["displayName"].ToString();
            string gender = ttsDetails["gender"].ToString();

            if (!voices.ContainsKey(language)) //check dictionary for a specific key value
            {
                voices.Add(language, new List<TTsDetail>()); //add to the dictionary the locale key and a ttsDetail object
            }

            voices[language].Add(new TTsDetail() { Code = code, DisplayName = string.Format("{0} ({1})", displayName, gender) });
        }

        // Gather the set of speech translation languages
        foreach (JProperty jSpeech in jResponse["speech"])
        {
            JObject languageDetails = (JObject)jSpeech.Value;
            string code = jSpeech.Name;
            string simplecode = languageDetails["language"].ToString();
            string displayName = languageDetails["name"].ToString();
            spokenLanguages.Add(code, displayName);
            fromLanguages.Add(code,simplecode);
        }

        spokenLanguages = spokenLanguages.OrderBy(x => x.Value).ToDictionary(x => x.Key, x => x.Value);
        FromLanguage.Items.Clear();
        foreach (var language in spokenLanguages)
        {
            FromLanguage.Items.Add(new ComboBoxItem() { Content = language.Value, Tag = language.Key});
        }

        // Gather the set of text translation languages
        foreach (JProperty jText in jResponse["text"])
        {
            JObject languageDetails = (JObject)jText.Value;
            string code = jText.Name;
            string displayName = languageDetails["name"].ToString();
            textLanguages.Add(code, displayName);

            string direction = languageDetails["dir"].ToString().ToLowerInvariant();
            bool LTR = true;
            if (direction.ToLowerInvariant() == "rtl") LTR = false;
            isLTR.Add(code, LTR);
        }

        textLanguages = textLanguages.OrderBy(x => x.Value).ToDictionary(x => x.Key, x => x.Value);
        ToLanguage.Items.Clear();
        foreach (var language in textLanguages)
        {
            ToLanguage.Items.Add(new ComboBoxItem() { Content = language.Value, Tag = language.Key });
        }

        if (Properties.Settings.Default.FromLanguageIndex >= 0) FromLanguage.SelectedIndex = Properties.Settings.Default.FromLanguageIndex;
        else
        {
            for(int i=0; i < FromLanguage.Items.Count; ++i)
            {
                ComboBoxItem item = (ComboBoxItem)FromLanguage.Items[i];
                if(CultureInfo.CurrentUICulture.Name.Equals((string)item.Tag, StringComparison.OrdinalIgnoreCase))
                {
                    FromLanguage.SelectedIndex = i;
                }
            }
        }
        if (Properties.Settings.Default.ToLanguageIndex >= 0) ToLanguage.SelectedIndex = Properties.Settings.Default.ToLanguageIndex;
        else
        {
            Random rnd = new Random(DateTime.Now.Millisecond);
            ToLanguage.SelectedIndex = (rnd.Next() % textLanguages.Count);
        }
    }
}
```

Este método primero construye una solicitud HTTP al punto de conexión de idiomas, que solicita las tres listas de idiomas (`text`, `speech` y `tts`).

El punto de conexión de idiomas usa el encabezado `Accept-Languages` de la solicitud para determinar el idioma en el que se representan los nombres de los idiomas. Por ejemplo, el idioma que los angloparlantes conocen como "German" se denomina "Deutsch" en alemán y "Alemán" en español. La lista de idiomas refleja estas diferencias. El idioma predeterminado del sistema se utiliza para este encabezado.

Tras enviar la solicitud y recibir la respuesta de JSON, esta se analiza en estructuras de datos internas. Estas estructuras se utilizan luego para construir los menús de idioma de origen y destino. 

Puesto que las voces disponibles dependen del idioma elegido por el usuario, aún no es posible configurar el menú de voz. En su lugar, se almacenan las voces disponibles para cada idioma para su uso posterior. El controlador `ToLanguage_SelectionChanged` (en el mismo archivo de origen) actualiza más tarde el menú de voz mediante una llamada a `UpdateVoiceComboBox()` cuando el usuario elige un idioma de destino. 

Como diversión, se selecciona aleatoriamente un idioma de destino si el usuario no ha ejecutado antes la aplicación. (Las opciones de menú se almacenan entre sesiones).

## <a name="authenticating-requests"></a>Autenticación de solicitudes

Para autenticarse en el servicio Microsoft Translator Speech debe enviar la clave de suscripción de Azure en el encabezado como el valor de `Ocp-Apim-Subscription-Key` en la solicitud de conexión.

## <a name="translation-overview"></a>Información general de la traducción

Translate API (punto de conexión WebSockets `wss://dev.microsofttranslator.com/speech/translate`) acepta la traducción del audio en formato WAVE firmado, monofónico, de 16 bits, a 16 kHz. El servicio devuelve una o más respuestas de JSON que contienen tanto el texto reconocido como el traducido. Si se ha solicitado la conversión de texto a voz, se envía un archivo de audio.

El usuario elige el origen de audio en el menú de entrada de micrófono o archivo. El audio puede proceder de un dispositivo de audio (por ejemplo, un micrófono) o de un archivo `.WAV`.

Se invoca el método `StartListening_Click` cuando el usuario hace clic en el botón Iniciar. Este controlador de eventos llama, a su vez, a `Connect()` para comenzar el proceso de envío de audio al punto de conexión de la API de servicio. El método `Connect()` realiza las siguientes tareas:


> [!div class="checklist"]
> * Obtención de la configuración del usuario de la ventana principal y su validación
> * Inicialización de la entrada de audio y secuencias de salida
> * Llamada a `ConnectAsync()` para controlar el resto del trabajo

`ConnectAsync()`, a su vez, controla las tareas siguientes:

> [!div class="checklist"]
> * Autenticación con clave de suscripción de Azure en el encabezado `Ocp-Apim-Subscription-Key`
> * Creación de una instancia de `SpeechClient` (se encuentra en `SpeechClient.cs`) para comunicarse con el servicio
> * Inicialización de las instancias de `TextMessageDecoder` y `BinaryMessageDecoder` (vea `SpeechResponseDecoder.cs`) para controlar las respuestas
> * Envío del audio a través de la instancia de `SpeechClient` al servicio Translator Speech
> * Recepción y proceso de los resultados de la traducción

Las responsabilidades de `SpeechClient` son menos:

> [!div class="checklist"]
> * Establecimiento de una conexión WebSocket con el servicio Translator Speech
> * Envío de datos de audio y recepción de respuestas a través del socket

## <a name="a-closer-look"></a>Un examen más profundo

Debe haberse aclarado ahora cómo funcionan las partes de la aplicación conjuntamente para realizar la solicitud de traducción. Eche un vistazo a parte del código, centrándose en las partes relevantes.

Esta es una versión parcial de `Connect()` que muestra cómo configurar las secuencias de audio:

```csharp
private void Connect()
{
    if (this.currentState != UiState.ReadyToConnect) return;

    Stopwatch watch = Stopwatch.StartNew();
    UpdateUiState(UiState.Connecting);

    // Omitted: code to validate UI settings

    string tag = ((ComboBoxItem)Mic.SelectedItem).Tag as string;
    string audioFileInputPath = null;
    if (tag == "File")
    {
        audioFileInputPath = this.AudioFileInput.Text;
        foreach (string currFile in audioFileInputPath.Split('|'))
        {
            if (!File.Exists(currFile))
            {
                SetMessage(String.Format($"Invalid audio source: selected file {currFile} does not exist."), "", MessageKind.Error);
                UpdateUiState(UiState.ReadyToConnect);
                return;
            }
        }
    }
    bool shouldSuspendInputAudioDuringTTS = this.CutInputAudioCheckBox.IsChecked.HasValue ? this.CutInputAudioCheckBox.IsChecked.Value : false;

    correlationId = Guid.NewGuid().ToString("D").Split('-')[0].ToUpperInvariant();

    // Setup speech translation client options
    SpeechClientOptions options;

    string voicename = "";
    if (this.Voice.SelectedItem != null)
    {
        voicename = ((ComboBoxItem)this.Voice.SelectedItem).Tag.ToString();
    }
    options = new SpeechTranslateClientOptions()
    {
        TranslateFrom = ((ComboBoxItem)this.FromLanguage.SelectedItem).Tag.ToString(),
        TranslateTo = ((ComboBoxItem)this.ToLanguage.SelectedItem).Tag.ToString(),
        Voice = voicename,
    };
    
    options.Hostname = baseUrl;
    options.AuthHeaderKey = "Authorization";
    options.AuthHeaderValue = ""; // set later in ConnectAsync.
    options.ClientAppId = new Guid("EA66703D-90A8-436B-9BD6-7A2707A2AD99");
    options.CorrelationId = this.correlationId;
    options.Features = GetFeatures().ToString().Replace(" ", "");
    options.Profanity = ((SpeechClient.ProfanityFilter)Enum.Parse(typeof(SpeechClient.ProfanityFilter), GetProfanityLevel(), true)).ToString();
    options.Experimental = MenuItem_Experimental.IsChecked;

    // Setup player and recorder but don't start them yet.
    WaveFormat waveFormat = new WaveFormat(16000, 16, 1);

    // WaveProvider for incoming TTS
    // We use a rather large BufferDuration because we need to be able to hold an entire utterance.
    // TTS audio is received in bursts (faster than real-time).
    textToSpeechBytes = 0;
    playerTextToSpeechWaveProvider = new BufferedWaveProvider(waveFormat);
    playerTextToSpeechWaveProvider.BufferDuration = TimeSpan.FromMinutes(5);

    ISampleProvider sampleProvider = null;
    if (audioFileInputPath != null)
    {
        // Setup mixing of audio from input file and from TTS
        playerAudioInputWaveProvider = new BufferedWaveProvider(waveFormat);
        var srce1 = new Pcm16BitToSampleProvider(playerTextToSpeechWaveProvider);
        var srce2 = new Pcm16BitToSampleProvider(playerAudioInputWaveProvider);
        var mixer = new MixingSampleProvider(srce1.WaveFormat);
        mixer.AddMixerInput(srce1);
        mixer.AddMixerInput(srce2);
        sampleProvider = mixer;
    }
    else
    {
        recorder = new WaveIn();
        recorder.DeviceNumber = (int)((ComboBoxItem)Mic.SelectedItem).Tag;
        recorder.WaveFormat = waveFormat;
        recorder.DataAvailable += OnRecorderDataAvailable;
        sampleProvider = playerTextToSpeechWaveProvider.ToSampleProvider();
    }

    if (!batchMode)
    {
        player = new WaveOut();
        player.DeviceNumber = (int)((ComboBoxItem)Speaker.SelectedItem).Tag;
        player.Init(sampleProvider);
    }

    this.audioBytesSent = 0;

    string logAudioFileName = null;
    if (LogSentAudio.IsChecked|| LogReceivedAudio.IsChecked)
    {
        string logAudioPath = System.IO.Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData), Properties.Settings.Default.OutputDirectory);
        try
        {
            Directory.CreateDirectory(logAudioPath);
        }
        catch
        {
            this.AddItemToLog(string.Format("Could not create folder {0}", logAudioPath));
        }

        if (LogSentAudio.IsChecked)
        {
            logAudioFileName = System.IO.Path.Combine(logAudioPath, string.Format("audiosent_{0}.wav", this.correlationId));
        }

        if (LogReceivedAudio.IsChecked)
        {
            string fmt = System.IO.Path.Combine(logAudioPath, string.Format("audiotts_{0}_{{0}}.wav", this.correlationId));
            this.audioReceived = new BinaryMessageDecoder(fmt);
        }
    }
}
```

Una parte considerable de `Connect()` implica la creación de una instancia de `SpeechClientOptions` (vea `SpeechClientOptions.cs`) para retener las opciones de traducción. Las opciones incluyen la información necesaria para conectarse al servicio (por ejemplo, la clave de autenticación y el nombre de host) y las funciones usadas para la traducción. Los campos se asignan a los campos de encabezado y los parámetros HTTP expuestos por [Translator Speech API](https://docs.microsoft.com/azure/cognitive-services/translator-speech/reference).

`Connect()` también crea e inicializa el dispositivo de entrada de audio (variable `sampleProvider`) que actúa como el origen de la voz que se debe traducir. Este dispositivo puede ser un dispositivo de entrada de hardware, como un micrófono, o un archivo que contenga datos de audio WAVE.

Este es el método `ConnectAsync()` que crea una instancia de la clase `speechClient` y enlaza funciones anónimas para controlar el texto y las respuestas binarias del servicio.

```csharp
private async Task ConnectAsync(SpeechClientOptions options, bool suspendInputAudioDuringTTS)
{
    await ADMAuthenticate(options);
    
    TextMessageDecoder textDecoder;
    
    s2smtClient = new SpeechClient((SpeechTranslateClientOptions)options, CancellationToken.None);
    
    s2smtClient.OnBinaryData += (c, a) => { AddSamplesToPlay(a, suspendInputAudioDuringTTS); };
    s2smtClient.OnEndOfBinaryData += (c, a) => { AddSamplesToPlay(a, suspendInputAudioDuringTTS); };
    s2smtClient.OnTextData += (c, a) => { textDecoder.AppendData(a); lastReceivedPacketTick = DateTime.Now.Ticks; };
    s2smtClient.OnEndOfTextData += (c, a) =>
    {
        textDecoder.AppendData(a);
        lastReceivedPacketTick = DateTime.Now.Ticks;
        textDecoder
            .Decode()
            .ContinueWith(t =>
            {
                if (t.IsFaulted)
                {
                    Log(t.Exception, "E: Failed to decode incoming text message.");
                }
                else
                {
                    object msg = t.Result;
                    if (msg.GetType() == typeof(FinalResultMessage))
                    {
                        // omitted: code to process final binary result
                    }
                    if (msg.GetType() == typeof(PartialResultMessage))
                    {
                        // omitted: code to process partial binary result
                    }
                }
            });
    };
    s2smtClient.Failed += (c, ex) =>
    {
        Log(ex, "E: SpeechTranslation client reported an error.");
    };
    s2smtClient.Disconnected += (c, ea) =>
    {
        SafeInvoke(() =>
        {
            // We only care to react to server disconnect when our state is Connected. 
            if (currentState == UiState.Connected)
            {
                Log("E: Connection has been lost.");
                Log($"E: Errors (if any): \n{string.Join("\n", s2smtClient.Errors)}");
                Disconnect();
            }
        });
    };
    await s2smtClient.Connect();
}
```

Tras la autenticación, el método crea la instancia de `SpeechClient`. La clase `SpeechClient` (en `SpeechClient.cs`) invoca a los controladores de eventos al recibir los datos de texto y binarios. Se invocan controladores adicionales cuando la conexión genera un error o se desconecta.

Los datos binarios son audio (salida de texto a voz) que el servicio envía cuando se habilita TTS. Los datos de texto son una traducción parcial o completa del texto hablado. Por lo que después de crear una instancia, el método enlaza funciones para controlar estos mensajes: el audio mediante el almacenamiento para su posterior reproducción y el texto mediante la presentación en la ventana.

## <a name="next-steps"></a>Pasos siguientes

Este ejemplo de código es una aplicación con numerosas funciones que muestra el uso de Translator Speech API. Por lo tanto, hay un número razonable de partes que se deben comprender. Ya ha visto las porciones más importantes. Para el resto, puede ser instructivo establecer unos puntos de interrupción en Visual Studio y completar el proceso de traducción. Una vez que comprenda la aplicación de ejemplo, estará equipado para utilizar el servicio de Translator Speech en sus propias aplicaciones.

> [!div class="nextstepaction"]
> [Referencia de Microsoft Translator Speech API](https://docs.microsoft.com/azure/cognitive-services/translator-speech/reference)
