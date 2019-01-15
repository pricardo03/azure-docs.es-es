---
title: 'Tutorial: Moderación de vídeos y transcripciones en .NET: Content Moderator'
titlesuffix: Azure Cognitive Services
description: Se describe cómo usar Content Moderator para moderar vídeos y transcripciones en. NET.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: tutorial
ms.date: 1/27/2018
ms.author: sajagtap
ms.openlocfilehash: e5958eeb4aca4cdc41bbc263a4cc38378927874b
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2019
ms.locfileid: "54118581"
---
# <a name="tutorial-video-and-transcript-moderation"></a>Tutorial: Moderación de vídeos y transcripciones

La API de vídeo de Content Moderator le permite moderar vídeos y crear revisiones de vídeo en la herramienta de revisión humana. 

Este detallado tutorial le servirá para aprender a crear una solución completa de moderación de vídeo y transcripción con moderación asistida por máquina y la creación de la revisión humana en bucle.

Descargue la [aplicación de consola de C#](https://github.com/MicrosoftContentModerator/VideoReviewConsoleApp) para este tutorial. La aplicación de consola usa el SDK y los paquetes relacionados para realizar las siguientes tareas:

- Comprimir los vídeos dE entrada para un procesamiento más rápido
- Moderar el vídeo para obtener capturas y fotogramas con información
- Utilizar las marcas de tiempo de los fotogramas para crear miniaturas (imágenes)
- Enviar marcas de tiempo y miniaturas para crear revisiones de vídeos
- Convertir la voz del vídeo en texto (transcripción) con la API de Media Indexer
- Moderar la transcripción con el servicio de moderación de texto
- Agregar la transcripción moderada a la revisión del vídeo

## <a name="sample-program-outputs"></a>Salidas del programa de ejemplo

Antes de continuar, echemos un vistazo a las siguientes salidas de ejemplo del programa:

- [Salida de consola](#program-output)
- [Revisión de vídeo](#video-review-default-view)
- [Vista de transcripción](#video-review-transcript-view)

## <a name="prerequisites"></a>Requisitos previos

1. Regístrese en el sitio web de la [herramienta de revisión de Content Moderator](https://contentmoderator.cognitive.microsoft.com/) y [cree etiquetas personalizadas](Review-Tool-User-Guide/tags.md) que la aplicación de consola de C# asigna desde dentro del código. La siguiente pantalla muestra las etiquetas personalizadas.

  ![Etiquetas personalizadas de moderación de vídeo](images/video-tutorial-custom-tags.png)

1. Para ejecutar la aplicación de ejemplo, necesitará una cuenta de Azure y una cuenta de Azure Media Services. Además, debe tener acceso a la versión preliminar privada de Content Moderator. Por último, necesita credenciales de autenticación de Azure Active Directory. Para obtener más información acerca de cómo obtener esta información, consulte [el tutorial rápido de la API de moderación de vídeo](video-moderation-api.md).

1. Edite el archivo `App.config` y agregue el nombre del inquilino de Active Directory, los puntos de conexión de servicio y las claves de la suscripción que indica `#####`. Necesita la siguiente información:

|Clave|DESCRIPCIÓN|
|-|-|
|`AzureMediaServiceRestApiEndpoint`|Punto de conexión para la API de Azure Media Services|
|`ClientSecret`|Clave de suscripción de Azure Media Services|
|`ClientId`|Identificador de cliente para Azure Media Services|
|`AzureAdTenantName`|Nombre de inquilino de Active Directory que representa su organización|
|`ContentModeratorReviewApiSubscriptionKey`|Clave de suscripción para la API de revisión de Content Moderator|
|`ContentModeratorApiEndpoint`|Punto de conexión para Content Moderator API|
|`ContentModeratorTeamId`|Identificador de equipo del moderador de contenido|

## <a name="getting-started"></a>Introducción

La clase `Program` en `Program.cs` es el punto de entrada principal para la aplicación de moderación de vídeo.

### <a name="methods-of-class-program"></a>Métodos de la clase Program

|Método|DESCRIPCIÓN|
|-|-|
|`Main`|Analiza la línea de comandos, recopila la información del usuario y empieza a procesar.|
|`ProcessVideo`|Comprime, carga, modera y crea las revisiones del vídeo.|
|`CreateVideoStreamingRequest`|Crea una secuencia para cargar un vídeo|
|`GetUserInputs`|Recopila información proporcionada por el usuario; se usa cuando no hay ninguna opción de línea de comandos|
|`Initialize`|Inicializa los objetos necesarios para el proceso de moderación|

### <a name="the-main-method"></a>El método Main

`Main()` es donde comienza la ejecución, por lo que es el lugar para empezar a entender el proceso de moderación de vídeo.

    static void Main(string[] args)
    {
        if (args.Length == 0)
        {
            string videoPath = string.Empty;
            GetUserInputs(out videoPath);
            Initialize();
            AmsConfigurations.logFilePath = Path.Combine(Path.GetDirectoryName(videoPath), "log.txt");
            try
            {
                ProcessVideo(videoPath).Wait();
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }
        else
        {
            DirectoryInfo directoryInfo = new DirectoryInfo(args[0]);
            if (args.Length == 2)
                bool.TryParse(args[1], out generateVtt);
            Initialize();
            AmsConfigurations.logFilePath = Path.Combine(args[0], "log.txt");
            var files = directoryInfo.GetFiles("*.mp4", SearchOption.AllDirectories);
            foreach (var file in files)
            {
                try
                {
                    ProcessVideo(file.FullName).Wait();
                }
                catch (Exception ex)
                {
                    Console.WriteLine(ex.Message);
                }
            }
        }
    }

`Main()` controla los argumentos de línea de comandos siguientes:

- La ruta de acceso a un directorio que contiene archivos de vídeo MPEG-4 que deben enviarse para moderación. Todos los archivos `*.mp4` de este directorio y sus subdirectorios se envían para moderación.
- Si lo desea, un marcador booleano (true/false) que indica si deben generarse transcripciones de texto con el fin de moderar audio.

Si no hay argumentos de línea de comandos, `Main()` llama a `GetUserInputs()`. Este método solicita al usuario que escriba la ruta de acceso a un único archivo de vídeo y especifique si se debe generar una transcripción de texto.

> [!NOTE]
> La aplicación de consola utiliza la [API de Azure Media Indexer](https://docs.microsoft.com/azure/media-services/media-services-process-content-with-indexer2) para generar las transcripciones a partir de la pista de audio del vídeo cargado. Los resultados se muestran en formato WebVTT. Para obtener más información sobre este formato, vea [Web Video Text Tracks Format](https://developer.mozilla.org/en-US/docs/Web/API/WebVTT_API) (Formato de pistas de texto de vídeo web).

### <a name="initialize-and-processvideo-methods"></a>Métodos Initialize y ProcessVideo

Independientemente de si las opciones del programa proceden de la línea de comandos o de la entrada interactiva del usuario, `Main()` llama a `Initialize()` para crear las instancias siguientes:

|Clase|DESCRIPCIÓN|
|-|-|
|`AMSComponent`|Comprime los archivos de vídeo antes de enviarlos para moderación.|
|`AMSconfigurations`|Interfaz para los datos de configuración de la aplicación, que se encuentra en `App.config`.|
|`VideoModerator`| Carga, codificación, cifrado y moderación con el SDK de Azure Media Services|
|`VideoReviewApi`|Administra las revisiones de vídeo en el servicio Content Moderator.|

Estas clases (además de `AMSConfigurations`, que es sencilla) se tratan con más detalle en las próximas secciones de este tutorial.

Por último, los archivos de vídeo se procesan uno a uno mediante una llamada a `ProcessVideo()` para cada uno.

    private static async Task ProcessVideo(string videoPath)
    {
        Stopwatch sw = new Stopwatch();
        sw.Start();
        Console.ForegroundColor = ConsoleColor.White;
        Console.WriteLine("\nVideo compression process started...");

        var compressedVideoPath = amsComponent.CompressVideo(videoPath);
        if (string.IsNullOrWhiteSpace(compressedVideoPath))
        {
            Console.ForegroundColor = ConsoleColor.Red;
            Console.WriteLine("Video Compression failed.");
        }

        Console.WriteLine("\nVideo compression process completed...");

        UploadVideoStreamRequest uploadVideoStreamRequest = CreateVideoStreamingRequest(compressedVideoPath);
        UploadAssetResult uploadResult = new UploadAssetResult();

        if (generateVtt)
        {
            uploadResult.GenerateVTT = generateVtt;
        }
        Console.WriteLine("\nVideo moderation process started...");

        if (!videoModerator.CreateAzureMediaServicesJobToModerateVideo(uploadVideoStreamRequest, uploadResult))
        {
            Console.ForegroundColor = ConsoleColor.Red;
            Console.WriteLine("Video Review process failed.");
        }

        Console.WriteLine("\nVideo moderation process completed...");
        Console.WriteLine("\nVideo review process started...");
        string reviewId = await videoReviewApi.CreateVideoReviewInContentModerator(uploadResult);
        Console.WriteLine("\nVideo review successfully completed...");
        sw.Stop();
        Console.WriteLine("\nTotal Elapsed Time: {0}", sw.Elapsed);
        using (var stw = new StreamWriter(AmsConfigurations.logFilePath, true))
        {
            stw.WriteLine("Video File Name: " + Path.GetFileName(videoPath));
            stw.WriteLine($"ReviewId: {reviewId}");
            stw.WriteLine("Total Elapsed Time: {0}", sw.Elapsed);
        }
    }


El método `ProcessVideo()` es bastante sencillo. Realiza las operaciones siguientes en orden:

- Comprime el vídeo
- Carga el vídeo en un recurso de Azure Media Services
- Crea un trabajo de Azure Media Services para moderar el vídeo
- Crea una revisión del vídeo en Content Moderator

En las siguientes secciones se examinan con más detalle algunos de los procesos individuales invocados por `ProcessVideo()`. 

## <a name="compressing-the-video"></a>Compresión del vídeo

Para minimizar el tráfico de red, la aplicación convierte archivos de vídeo a formato H.264 (MPEG-4 AVC) y les aplica una escala de un ancho máximo de 640 píxeles. Se recomienda el códec H.264 por su alta eficacia (velocidad de compresión). La compresión se realiza mediante la herramienta de línea de comandos `ffmpeg` gratuita, que se incluye en la carpeta `Lib` de la solución de Visual Studio. Los archivos de entrada pueden ser de cualquier formato admitido por `ffmpeg`, incluidos los códecs y formatos de archivo de vídeo más usados.

> [!NOTE]
> Cuando se inicia el programa con las opciones de línea de comandos, se especifica un directorio que contiene los archivos de vídeo que se envían para moderación. Se procesan todos los archivos de este directorio con la extensión de nombre de archivo `.mp4`. Para procesar otras extensiones de nombre de archivo, actualice el método `Main()` en `Program.cs` para incluir las extensiones deseadas.

El código que comprime un solo archivo de vídeo es la clase `AmsComponent` en `AMSComponent.cs`. El método responsable de esta funcionalidad es `CompressVideo()`, que se muestra aquí.

    public string CompressVideo(string videoPath)
    {
        string ffmpegBlobUrl;
        if (!ValidatePreRequisites())
        {
            Console.WriteLine("Configurations check failed. Please cross check the configurations!");
            throw new Exception();
        }

        if (File.Exists(_configObj.FfmpegExecutablePath))
        {
            ffmpegBlobUrl = this._configObj.FfmpegExecutablePath;
        }
        else
        {
            Console.WriteLine("ffmpeg.exe is missing. Please check the Lib folder");
            throw new Exception();
        }

        string videoFilePathCom = videoPath.Split('.')[0] + "_c.mp4";
        ProcessStartInfo processStartInfo = new ProcessStartInfo();
        processStartInfo.WindowStyle = ProcessWindowStyle.Hidden;
        processStartInfo.FileName = ffmpegBlobUrl;
        processStartInfo.Arguments = "-i \"" + videoPath + "\" -vcodec libx264 -n -crf 32 -preset veryfast -vf scale=640:-1 -c:a aac -aq 1 -ac 2 -threads 0 \"" + videoFilePathCom + "\"";
        var process = Process.Start(processStartInfo);
        process.WaitForExit();
        process.Close();
        return videoFilePathCom;
    }

El código sigue estos pasos:

- Realiza comprobaciones para asegurarse de que la configuración de `App.config` contiene todos los datos necesarios
- Realiza comprobaciones para asegurarse de que el binario `ffmpeg` está presente
- Genera el nombre de archivo de salida mediante la anexión de `_c.mp4` al nombre base del archivo (como `Example.mp4` -> `E>xample_c.mp4`)
- Genera una cadena de línea de comandos para llevar a cabo la conversión
- Inicia un proceso `ffmpeg` mediante la línea de comandos
- Espera a que el vídeo se procese

> [!NOTE]
> Si sabe con certeza que sus vídeos ya se han comprimido con H.264 y tienen las dimensiones adecuadas, puede volver a escribir `CompressVideo()` para omitir la compresión.

El método devuelve el nombre del archivo de salida comprimido.

## <a name="uploading-and-moderating-the-video"></a>Carga y moderación del vídeo

El vídeo debe almacenarse en Azure Media Services para que el servicio Content Moderation lo pueda procesar. La clase `Program` en `Program.cs` tiene un método breve `CreateVideoStreamingRequest()` que devuelve un objeto que representa la solicitud de streaming usada para cargar el vídeo.

    private static UploadVideoStreamRequest CreateVideoStreamingRequest(string compressedVideoFilePath)
    {
        return
            new UploadVideoStreamRequest
            {
                VideoStream = File.ReadAllBytes(compressedVideoFilePath),
                VideoName = Path.GetFileName(compressedVideoFilePath),
                EncodingRequest = new EncodingRequest()
                {
                    EncodingBitrate = AmsEncoding.AdaptiveStreaming
                },
                VideoFilePath = compressedVideoFilePath
            };
    }

El objeto `UploadVideoStreamRequest` resultante se define en `UploadVideoStreamRequest.cs` (y su elemento primario, `UploadVideoRequest`, en `UploadVideoRequest.cs`). Estas clases no se muestran aquí; son breves y solo sirven para almacenar los datos de vídeo comprimidos y la información sobre estos. Otra clase de solo datos, `UploadAssetResult` (`UploadAssetResult.cs`) se utiliza para almacenar los resultados del proceso de carga. Ahora es posible comprender estas líneas en `ProcessVideo()`:

    UploadVideoStreamRequest uploadVideoStreamRequest = CreateVideoStreamingRequest(compressedVideoPath);
    UploadAssetResult uploadResult = new UploadAssetResult();

    if (generateVtt)
    {
        uploadResult.GenerateVTT = generateVtt;
    }
    Console.WriteLine("\nVideo moderation process started...");

    if (!videoModerator.CreateAzureMediaServicesJobToModerateVideo(uploadVideoStreamRequest, uploadResult))
    {
        Console.ForegroundColor = ConsoleColor.Red;
        Console.WriteLine("Video Review process failed.");
    }

Estas líneas realizan las siguientes tareas:

- Crean un `UploadVideoStreamRequest` para cargar el vídeo comprimido
- Establecen la marca `GenerateVTT` de la solicitud si el usuario ha solicitado una transcripción de texto
- Llaman a `CreateAzureMediaServicesJobToModerateVideo()` para realizar la carga y recibir el resultado

## <a name="deep-dive-into-video-moderation"></a>Profundización en la moderación de vídeo

El método `CreateAzureMediaServicesJobToModerateVideo()` está en `VideoModerator.cs`, que contiene la mayor parte del código que interactúa con Azure Media Services. El código fuente del método se muestra en el siguiente extracto.

    public bool CreateAzureMediaServicesJobToModerateVideo(UploadVideoStreamRequest uploadVideoRequest, UploadAssetResult uploadResult)
    {
        asset = CreateAsset(uploadVideoRequest);
        uploadResult.VideoName = uploadVideoRequest.VideoName;
        // Encoding the asset , Moderating the asset, Generating transcript in parallel
        IAsset encodedAsset = null;
        //Creates the job for the tasks.
        IJob job = this._mediaContext.Jobs.Create("AMS Review Job");

        //Adding encoding task to job.
        ConfigureEncodeAssetTask(uploadVideoRequest.EncodingRequest, job);

        ConfigureContentModerationTask(job);

        //adding transcript task to job.
        if (uploadResult.GenerateVTT)
        {
            ConfigureTranscriptTask(job);
        }

        Stopwatch timer = new Stopwatch();
        timer.Start();
        //submit and execute job.
        job.Submit();
        job.GetExecutionProgressTask(new CancellationTokenSource().Token).Wait();
        timer.Stop();
        using (var sw = new StreamWriter(AmsConfigurations.logFilePath, true))
        {
            sw.WriteLine("AMS Job Elapsed Time: {0}", timer.Elapsed);
        }

        if (job.State == JobState.Error)
        {
            throw new Exception("Video moderation has failed due to AMS Job error.");
        }

        UploadAssetResult result = uploadResult;
        encodedAsset = job.OutputMediaAssets[0];
        result.ModeratedJson = GetCmDetail(job.OutputMediaAssets[1]);
        // Check for valid Moderated JSON
        var jsonModerateObject = JsonConvert.DeserializeObject<VideoModerationResult>(result.ModeratedJson);

        if (jsonModerateObject == null)
        {
            return false;
        }
        if (uploadResult.GenerateVTT)
        {
            GenerateTranscript(job.OutputMediaAssets.Last());
        }

        uploadResult.StreamingUrlDetails = PublishAsset(encodedAsset);
        string downloadUrl = GenerateDownloadUrl(asset, uploadVideoRequest.VideoName);
        uploadResult.StreamingUrlDetails.DownloadUri = downloadUrl;
        uploadResult.VideoName = uploadVideoRequest.VideoName;
        uploadResult.VideoFilePath = uploadVideoRequest.VideoFilePath;
        return true;
    }

Este código realiza las tareas siguientes:

- Crea un trabajo de AMS para que se realice el procesamiento
- Agrega tareas para codificar el archivo de vídeo, moderándolo y generando una transcripción de texto
- Envía el trabajo, cargando el archivo e iniciando el procesamiento
- Recupera los resultados de la moderación, la transcripción de texto (si se solicita) y otra información

## <a name="sample-video-moderation-response"></a>Respuesta de moderación de vídeo de ejemplo

El resultado del trabajo de moderación de vídeo (vea el [inicio rápido de moderación de vídeo](video-moderation-api.md) es una estructura de datos JSON que contiene los resultados de la moderación. Estos resultados incluyen un desglose de los fragmentos (capturas) en el vídeo, y cada uno de ellos contiene eventos (clips) con fotogramas clave que se han marcado para su revisión. Cada fotograma clave se puntúa sobre la probabilidad de que tenga contenido para adultos. En el ejemplo siguiente se muestra una respuesta JSON:

    {
        "version": 2,
        "timescale": 90000,
        "offset": 0,
        "framerate": 50,
        "width": 1280,
        "height": 720,
        "totalDuration": 18696321,
        "fragments": [
        {
            "start": 0,
            "duration": 18000
        },
        {
            "start": 18000,
            "duration": 3600,
            "interval": 3600,
            "events": [
            [
            {
                "reviewRecommended": false,
                "adultScore": 0.00001,
                "racyScore": 0.03077,
                "index": 5,
                "timestamp": 18000,
                "shotIndex": 0
            }
            ]
        ]
        },
        {
            "start": 18386372,
            "duration": 119149,
            "interval": 119149,
            "events": [
            [
            {
                "reviewRecommended": true,
                "adultScore": 0.00000,
                "racyScore": 0.91902,
                "index": 5085,
                "timestamp": 18386372,
                "shotIndex": 62
            }
        ]
        ]
        }
    ]
    }

También se produce una transcripción de audio del vídeo cuando está configurada la marca `GenerateVTT`.

> [!NOTE]
> La aplicación de consola utiliza la [API de Azure Media Indexer](https://docs.microsoft.com/azure/media-services/media-services-process-content-with-indexer2) para generar las transcripciones a partir de la pista de audio del vídeo cargado. Los resultados se muestran en formato WebVTT. Para obtener más información sobre este formato, vea [Web Video Text Tracks Format](https://developer.mozilla.org/en-US/docs/Web/API/WebVTT_API) (Formato de pistas de texto de vídeo web).


## <a name="creating-the-human-in-the-loop-review"></a>Creación de la revisión humana en bucle

El proceso de moderación devuelve una lista de fotogramas clave del vídeo, junto con una transcripción de sus pistas de audio. El siguiente paso es crear una revisión en la herramienta de revisión de Content Moderator para moderadores humanos. Volviendo al método `ProcessVideo()` en `Program.cs`, verá la llamada al método `CreateVideoReviewInContentModerator()`. Este método está en la clase `videoReviewApi`, que se encuentra en `VideoReviewAPI.cs` y se muestra aquí.

    public async Task<string> CreateVideoReviewInContentModerator(UploadAssetResult uploadAssetResult)
    {
    
        string reviewId = string.Empty;
        List<ProcessedFrameDetails> frameEntityList = framegenerator.CreateVideoFrames(uploadAssetResult);
        string path = uploadAssetResult.GenerateVTT == true ? this._amsConfig.FfmpegFramesOutputPath + Path.GetFileNameWithoutExtension (uploadAssetResult.VideoName) + "_aud_SpReco.vtt" : "";
        TranscriptScreenTextResult screenTextResult = new TranscriptScreenTextResult();
        
    if (File.Exists(path))
        {
            screenTextResult = await GenerateTextScreenProfanity(reviewId, path, frameEntityList);
            uploadAssetResult.Category1TextScore = screenTextResult.Category1Score;
            uploadAssetResult.Category2TextScore = screenTextResult.Category2Score;
            uploadAssetResult.Category3TextScore = screenTextResult.Category3Score;
            uploadAssetResult.Category1TextTag = screenTextResult.Category1Tag;
            uploadAssetResult.Category2TextTag = screenTextResult.Category2Tag;
            uploadAssetResult.Category3TextTag = screenTextResult.Category3Tag;
        }
        
        var reviewVideoRequestJson = CreateReviewRequestObject(uploadAssetResult, frameEntityList);
        if (string.IsNullOrWhiteSpace(reviewVideoRequestJson))
        {
            throw new Exception("Video review process failed in CreateVideoReviewInContentModerator");
        }
        
        reviewId = JsonConvert.DeserializeObject<List<string>>(ExecuteCreateReviewApi(reviewVideoRequestJson).Result).FirstOrDefault();
        frameEntityList = framegenerator.GenerateFrameImages(frameEntityList, uploadAssetResult, reviewId);
        await CreateAndPublishReviewInContentModerator(uploadAssetResult, frameEntityList, reviewId, path, screenTextResult);
        return reviewId;
    
    }

`CreateVideoReviewInContentModerator()` llama a otros métodos para llevar a cabo las siguientes tareas:

> [!NOTE]
> La aplicación de consola utiliza la biblioteca [FFmpeg](https://ffmpeg.org/) para generar miniaturas. Estas miniaturas (imágenes) se corresponden con las marcas de tiempo de los fotogramas en la [salida de moderación de vídeo](#sample-video-moderation-response).

|Tarea|Métodos|Archivo|
|-|-|-|
|Extraer los fotogramas clave del vídeo y crear imágenes en miniatura a partir de ellos|`CreateVideoFrames()`<br>`GenerateFrameImages()`|`FrameGeneratorServices.cs`|
|Analizar la transcripción de texto, si está disponible, para buscar el audio para adultos|`GenerateTextScreenProfanity()`| `VideoReviewAPI.cs`|
|Preparar y enviar una solicitud de revisión de vídeo para su inspección humana|`CreateReviewRequestObject()`<br> `ExecuteCreateReviewApi()`<br>`CreateAndPublishReviewInContentModerator()`|`VideoReviewAPI.cs`|

## <a name="video-review-default-view"></a>Vista predeterminada de la revisión de vídeo

La captura de pantalla siguiente muestra los resultados de los pasos anteriores.

![Vista predeterminada de la revisión de vídeo](images/video-tutorial-default-view.PNG)

## <a name="transcript-generation"></a>Generación de transcripción

Hasta ahora, el código presentado en este tutorial se ha centrado en el contenido visual. La revisión del contenido de voz es un proceso independiente y opcional que, como se ha mencionado, usa una transcripción generada a partir del audio. Ahora es el momento de revisar cómo se crean las transcripciones de texto y cómo se usan en el proceso de revisión. La tarea de generar la transcripción recae en el servicio [Azure Media Indexer](https://docs.microsoft.com/azure/media-services/media-services-index-content).

La aplicación realiza las tareas siguientes:

|Tarea|Métodos|Archivo|
|-|-|-|
|Determinar si se deben generar transcripciones de texto|`Main()`<br>`GetUserInputs()`|`Program.cs`|
|Si es así, enviar un trabajo de transcripción como parte de la moderación|`ConfigureTranscriptTask()`|`VideoModerator.cs`|
|Obtener una copia local de la transcripción|`GenerateTranscript()`|`VideoModerator.cs`|
|Marcar los fotogramas del vídeo que contienen audio inapropiado|`GenerateTextScreenProfanity()`<br>`TextScreen()`|`VideoReviewAPI.cs`|
|Agregar los resultados a la revisión|`UploadScreenTextResult()`<br>`ExecuteAddTranscriptSupportFile()`|`VideoReviewAPI.cs`|

### <a name="task-configuration"></a>Configuración de tareas

Vamos a ver cómo se envía el trabajo de transcripción. `CreateAzureMediaServicesJobToModerateVideo()` (ya descrito) llama a `ConfigureTranscriptTask()`.

    private void ConfigureTranscriptTask(IJob job)
    {
        string mediaProcessorName = _amsConfigurations.MediaIndexer2MediaProcessor;
        IMediaProcessor processor = _mediaContext.MediaProcessors.GetLatestMediaProcessorByName(mediaProcessorName);

        string configuration = File.ReadAllText(_amsConfigurations.MediaIndexerConfigurationJson);
        ITask task = job.Tasks.AddNew("AudioIndexing Task", processor, configuration, TaskOptions.None);
        task.InputAssets.Add(asset);
        task.OutputAssets.AddNew("AudioIndexing Output Asset", AssetCreationOptions.None);
    }

La configuración de la tarea de transcripción se lee desde el archivo `MediaIndexerConfig.json` en la carpeta `Lib` de la solución. Se crean recursos de AMS para el archivo de configuración y para la salida del proceso de transcripción. Cuando se ejecuta el trabajo de AMS, esta tarea crea una transcripción de texto a partir de la pista de audio del archivo de vídeo.

> [!NOTE]
> La aplicación de ejemplo reconoce solo voz en inglés de Estados Unidos.

### <a name="transcript-generation"></a>Generación de transcripción

La transcripción se publica como un recurso de AMS. Para examinar si existe contenido inapropiado en la transcripción, la aplicación descarga el recurso de Azure Media Services. `CreateAzureMediaServicesJobToModerateVideo()` llama a `GenerateTranscript()`, como se muestra aquí, para recuperar el archivo.

    public bool GenerateTranscript(IAsset asset)
    {
        try
        {
            var outputFolder = this._amsConfigurations.FfmpegFramesOutputPath;
            IAsset outputAsset = asset;
            IAccessPolicy policy = null;
            ILocator locator = null;
            policy = _mediaContext.AccessPolicies.Create("My 30 days readonly policy", TimeSpan.FromDays(360), AccessPermissions.Read);
            locator = _mediaContext.Locators.CreateLocator(LocatorType.Sas, outputAsset, policy, DateTime.UtcNow.AddMinutes(-5));
            DownloadAssetToLocal(outputAsset, outputFolder);
            locator.Delete();
            return true;
        }
        catch
        {   //TODO:  Logging
            Console.WriteLine("Exception occurred while generating index for video.");
            throw;
        }
    }

Después de algunas configuraciones necesarias de Azure Media Services, la descarga se realiza efectivamente llamando a `DownloadAssetToLocal()`, una función genérica que copia un recurso de Azure Media Services en un archivo local.

## <a name="transcript-moderation"></a>Moderación de la transcripción

Con la transcripción a mano, se analiza y se utiliza en la revisión. La creación de la revisión pertenece al ámbito de `CreateVideoReviewInContentModerator()`, que llama a `GenerateTextScreenProfanity()` para realizar el trabajo. A su vez, este método llama a `TextScreen()`, que contiene la mayor parte de la funcionalidad. 

`TextScreen()` realiza las siguientes tareas:

- Analizar la transcripción para ver si hay marcas de tiempo y títulos
- Enviar cada título a la moderación de texto
- Marcar los fotogramas que pueden tener contenido de voz inapropiado

Vamos a examinar cada una estas tareas con más detalle:

### <a name="initialize-the-code"></a>Inicialización del código

En primer lugar, inicialice todas las variables y colecciones.

    private async Task<TranscriptScreenTextResult> TextScreen(string filepath, List<ProcessedFrameDetails> frameEntityList)
    {
        List<TranscriptProfanity> profanityList = new List<TranscriptProfanity>();
        string responseContent = string.Empty;
        HttpResponseMessage response;
        bool category1Tag = false;
        bool category2Tag = false;
        bool category3Tag = false;
        double category1Score = 0;
        double category2Score = 0;
        double category3Score = 0;
        List<string> vttLines = File.ReadAllLines(filepath).Where(line => !line.Contains("NOTE Confidence:") && line.Length > 0).ToList();
        StringBuilder sb = new StringBuilder();
        List<CaptionScreentextResult> csrList = new List<CaptionScreentextResult>();
        CaptionScreentextResult captionScreentextResult = new CaptionScreentextResult() { Captions = new List<string>() };

        // Code from the next sections in the tutorial
    

### <a name="parse-the-transcript-for-captions"></a>Análisis de la transcripción para detectar títulos

A continuación, analice la transcripción en formato VTT para ver si contiene títulos y marcas de tiempo. La herramienta de revisión muestra estos títulos en la pestaña de transcripción en la pantalla de revisión de vídeo. Las marcas de tiempo se usan para sincronizar los títulos con los fotogramas de vídeo correspondientes.

        // Code from the previous section(s) in the tutorial

        //
        // Parse the transcript
        //
        foreach (var line in vttLines.Skip(1))
        {
                if (line.Contains("-->"))
                {
                    if (sb.Length > 0)
                    {
                        captionScreentextResult.Captions.Add(sb.ToString());
                        sb.Clear();
                    }
                    if (captionScreentextResult.Captions.Count > 0)
                    {
                        csrList.Add(captionScreentextResult);
                        captionScreentextResult = new CaptionScreentextResult() { Captions = new List<string>() };
                    }
                    string[] times = line.Split(new string[] { "-->" }, StringSplitOptions.RemoveEmptyEntries);
                    string startTimeString = times[0].Trim();
                    string endTimeString = times[1].Trim();
                    int startTime = (int)TimeSpan.ParseExact(startTimeString, @"hh\:mm\:ss\.fff", CultureInfo.InvariantCulture).TotalMilliseconds;
                    int endTime = (int)TimeSpan.ParseExact(endTimeString, @"hh\:mm\:ss\.fff", CultureInfo.InvariantCulture).TotalMilliseconds;
                    captionScreentextResult.StartTime = startTime;
                    captionScreentextResult.EndTime = endTime;
                }
                else
                {
                    sb.Append(line);
                }
                if (sb.Length + line.Length > 1024)
                {
                    captionScreentextResult.Captions.Add(sb.ToString());
                    sb.Clear();
                }
            }
            if (sb.Length > 0)
            {
                captionScreentextResult.Captions.Add(sb.ToString());
            }
            if (captionScreentextResult.Captions.Count > 0)
            {
                csrList.Add(captionScreentextResult);
            }

            // Code from the following section in the quickstart

### <a name="moderate-captions-with-the-text-moderation-service"></a>Moderación de los títulos con el servicio de moderación de texto

A continuación, se analizan los títulos de texto analizados con la API de texto de Content Moderator.

> [!NOTE]
> La clave de servicio de Content Moderator tiene un límite de frecuencia de solicitudes por segundo (RPS). Si se supera el límite, el SDK produce una excepción con un código de error 429. 
>
> Una clave de plan gratuito tiene un límite de frecuencia de RPS.

    //
    // Moderate the captions or cues
    //
    int waitTime = 1000;
    foreach (var csr in csrList)
    {
                bool captionAdultTextTag = false;
                bool captionRacyTextTag = false;
                bool captionOffensiveTextTag = false;
                bool retry = true;

                foreach (var caption in csr.Captions)
                {
                    while (retry)
                    {
                        try
                        {
                            System.Threading.Thread.Sleep(waitTime);
                            var lang = await CMClient.TextModeration.DetectLanguageAsync("text/plain", caption);
                            var oRes = await CMClient.TextModeration.ScreenTextWithHttpMessagesAsync(lang.DetectedLanguageProperty, "text/plain", caption, null, null, null, true);
                            response = oRes.Response;
                            responseContent = await response.Content.ReadAsStringAsync();
                            retry = false;
                        }
                        catch (Exception e)
                        {
                            if (e.Message.Contains("429"))
                            {
                                Console.WriteLine($"Moderation API call failed. Message: {e.Message}");
                                waitTime = (int)(waitTime * 1.5);
                                Console.WriteLine($"wait time: {waitTime}");
                            }
                            else
                            {
                                retry = false;
                                Console.WriteLine($"Moderation API call failed. Message: {e.Message}");
                            }
                        }
                    }
                    var jsonTextScreen = JsonConvert.DeserializeObject<TextScreen>(responseContent);
                    if (jsonTextScreen != null)
                    {
                        TranscriptProfanity transcriptProfanity = new TranscriptProfanity();
                        transcriptProfanity.TimeStamp = "";
                        List<Terms> transcriptTerm = new List<Terms>();
                        if (jsonTextScreen.Terms != null)
                        {
                            foreach (var term in jsonTextScreen.Terms)
                            {
                                var profanityobject = new Terms
                                {
                                    Term = term.Term,
                                    Index = term.Index
                                };
                                transcriptTerm.Add(profanityobject);
                            }
                            transcriptProfanity.Terms = transcriptTerm;
                            profanityList.Add(transcriptProfanity);
                        }
                        if (jsonTextScreen.Classification.Category1.Score > _amsConfig.Category1TextThreshold) captionAdultTextTag = true;
                        if (jsonTextScreen.Classification.Category2.Score > _amsConfig.Category2TextThreshold) captionRacyTextTag = true;
                        if (jsonTextScreen.Classification.Category3.Score > _amsConfig.Category3TextThreshold) captionOffensiveTextTag = true;
                        if (jsonTextScreen.Classification.Category1.Score > _amsConfig.Category1TextThreshold) category1Tag = true;
                        if (jsonTextScreen.Classification.Category2.Score > _amsConfig.Category2TextThreshold) category2Tag = true;
                        if (jsonTextScreen.Classification.Category3.Score > _amsConfig.Category3TextThreshold) category3Tag = true;
                        category1Score = jsonTextScreen.Classification.Category1.Score > category1Score ? jsonTextScreen.Classification.Category1.Score : category1Score;
                        category2Score = jsonTextScreen.Classification.Category2.Score > category2Score ? jsonTextScreen.Classification.Category2.Score : category2Score;
                        category3Score = jsonTextScreen.Classification.Category3.Score > category3Score ? jsonTextScreen.Classification.Category3.Score : category3Score;
                    }
                    foreach (var frame in frameEntityList.Where(x => x.TimeStamp >= csr.StartTime && x.TimeStamp <= csr.EndTime))
                    {
                        frame.IsAdultTextContent = captionAdultTextTag;
                        frame.IsRacyTextContent = captionRacyTextTag;
                        frame.IsOffensiveTextContent = captionOffensiveTextTag;
                    }
                }
            }
            TranscriptScreenTextResult screenTextResult = new TranscriptScreenTextResult()
            {
                TranscriptProfanity = profanityList,
                Category1Tag = category1Tag,
                Category2Tag = category2Tag,
                Category3Tag = category3Tag,
                Category1Score = category1Score,
                Category2Score = category2Score,
                Category3Score = category3Score
            };
            return screenTextResult;
    }

### <a name="breaking-down-the-text-moderation-step"></a>Desglose del paso de moderación de texto

`TextScreen()` es un método considerable, así que vamos a verlo por partes.

1. En primer lugar, el método lee el archivo de transcripción línea por línea. Omite las líneas en blanco y las líneas que contienen un `NOTE` con una puntuación de confianza. Extrae las marcas de tiempo y los elementos de texto de las *indicaciones* del archivo. Una indicación representa el texto de la pista de audio e incluye las horas de inicio y finalización. Una indicación comienza con la línea de la marca de tiempo con la cadena `-->`. Le sigue una o más líneas de texto.

1. Las instancias de `CaptionScreentextResult` (definidas en `TranscriptProfanity.cs`) se utilizan para contener la información que se analiza de cada indicación.  Cuando se detecta una nueva línea de marca de tiempo o se alcanza una longitud máxima de texto de 1024 caracteres, se agrega un nuevo `CaptionScreentextResult` a `csrList`. 

1. A continuación, el método envía cada indicación a la API de moderación de texto. Llama a `ContentModeratorClient.TextModeration.DetectLanguageAsync()` y `ContentModeratorClient.TextModeration.ScreenTextWithHttpMessagesAsync()`, que se definen en el ensamblado `Microsoft.Azure.CognitiveServices.ContentModerator`. Para evitar la limitación de velocidad, el método hace una pausa de un segundo antes de enviar cada indicación.

1. Después de recibir los resultados del servicio de moderación de texto, el método los analiza para ver si cumplen los umbrales de confianza. Estos valores se establecen en `App.config` como `OffensiveTextThreshold`, `RacyTextThreshold` y `AdultTextThreshold`. Por último, también se almacenan los propios términos inapropiados. Todos los fotogramas dentro de intervalo de tiempo de la indicación se marcan como que contienen material ofensivo o para adultos.

1. `TextScreen()` devuelve una instancia de `TranscriptScreenTextResult` que contiene el resultado de moderación de texto a partir del vídeo como un todo. Este objeto incluye marcas y puntuaciones para los distintos tipos de contenido inapropiado, junto con una lista de todos los términos inapropiados. El autor de la llamada, `CreateVideoReviewInContentModerator()`, llama a `UploadScreenTextResult()` para asociar esta información a la revisión para que esté disponible para los revisores humanos.
 
## <a name="video-review-transcript-view"></a>Vista de transcripción de la revisión de vídeo

La siguiente pantalla muestra el resultado de los pasos de generación y moderación de la transcripción.

![Vista de la transcripción de la moderación de vídeo](images/video-tutorial-transcript-view.PNG)

## <a name="program-output"></a>Salida del programa

La siguiente salida de línea de comandos del programa muestra las distintas tareas a medida que se completan. Además, el resultado de moderación (en formato JSON) y la transcripción de voz están disponibles en el mismo directorio que los archivos de vídeo originales.

    Microsoft.ContentModerator.AMSComponentClient
    Enter the fully qualified local path for Uploading the video :
    "Your File Name.MP4"
    Generate Video Transcript? [y/n] : y
    
    Video compression process started...
    Video compression process completed...
    
    Video moderation process started...
    Video moderation process completed...
    
    Video review process started...
    Video Frames Creation inprogress...
    Frames(83) created successfully.
    Review Created Successfully and the review Id 201801va8ec2108d6e043229ba7a9e6373edec5
    Video review successfully completed...
    
    Total Elapsed Time: 00:05:56.8420355


## <a name="next-steps"></a>Pasos siguientes

[Descargue la solución de Visual Studio](https://github.com/MicrosoftContentModerator/VideoReviewConsoleApp), los archivos de ejemplo y las bibliotecas necesarias para este tutorial y empiece a trabajar en la integración.
