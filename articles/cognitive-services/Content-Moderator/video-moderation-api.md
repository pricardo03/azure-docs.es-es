---
title: 'Moderación de vídeo: Content Moderator'
titlesuffix: Azure Cognitive Services
description: Use la moderación de vídeos para buscar contenido para adultos e inapropiado.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: conceptual
ms.date: 02/02/2018
ms.author: sajagtap
ms.openlocfilehash: 9b6240ef0883e1a523c50c9b641065deb2669d53
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47226542"
---
# <a name="video-moderation"></a>Moderación de vídeo

En la actualidad, los visitantes en línea reproducen miles de millones de vídeos tanto en redes sociales como en sitios los web regionales más populares, y esta cifra va en aumento. Al aplicar los servicios basados en el aprendizaje automático para detectar el posible contenido para adultos o que sea inapropiado, puede reducir el costo de sus esfuerzos de moderación.

## <a name="sign-up-for-the-content-moderator-media-processor-preview"></a>Registro en el procesador de multimedia de Content Moderator (versión preliminar)

### <a name="create-a-free-azure-account"></a>Crear una cuenta de Azure gratuita

Si no tiene ninguna, [haga clic aquí](https://azure.microsoft.com/free/) para crear una cuenta gratuita de Azure.

### <a name="create-an-azure-media-services-account"></a>Creación de una cuenta de Azure Media Services

La funcionalidad de vídeo de Content Moderator está disponible de forma gratuita como un **procesador de multimedia** (en versión preliminar pública) para Azure Media Services (AMS).

[Cree una cuenta de Azure Media Services](https://docs.microsoft.com/azure/media-services/media-services-portal-create-account) en su suscripción de Azure.

### <a name="get-azure-active-directory-credentials"></a>Obtener las credenciales de Azure Active Directory

   1. Lea el [artículo del portal de Azure Media Services](https://docs.microsoft.com/azure/media-services/media-services-portal-get-started-with-aad) para obtener información sobre cómo usar Azure Portal para obtener sus credenciales de autenticación de Azure AD.
   1. Lea el [artículo de .NET de Azure Media Services](https://docs.microsoft.com/azure/media-services/media-services-dotnet-get-started-with-aad) para aprender a usar sus credenciales de Azure Active Directory con .NET SDK.

   > [!NOTE]
   > El código de ejemplo de esta guía de inicio rápido usa el método de **autenticación de la entidad de servicio** descrito en ambos artículos.

Una vez que obtenga sus credenciales de AMS, hay dos maneras de probar el procesador de multimedia de Content Moderator.

## <a name="use-azure-media-services-explorer"></a>Usar el explorador de Azure Media Services

Use el [explorador interactivo de Azure Media Services (AMS)](https://azure.microsoft.com/blog/managing-media-workflows-with-the-new-azure-media-services-explorer-tool/) para explorar su cuenta de AMS, cargar vídeos y realizar exámenes con el procesador de multimedia de Content Moderator. [Descárguelo e instálelo](https://github.com/Azure/Azure-Media-Services-Explorer/releases) desde GitHub y [examine el código fuente](http://github.com/Azure/Azure-Media-Services-Explorer) para obtener más información acerca del uso del SDK de AMS.

![Explorador de Azure Media Services con Content Moderator](images/ams-explorer-content-moderator.PNG)

## <a name="net-quickstart-with-visual-studio-and-c"></a>Guía de inicio rápido de .NET con Visual Studio y C#

1. Agregue un nuevo proyecto denominado **Aplicación de consola (.NET Framework)** a la solución.

   En el código de ejemplo, póngale al proyecto el nombre **VideoModeration**.

1. Seleccione este proyecto como proyecto de inicio único para la solución.

### <a name="install-required-packages"></a>Instalación de los paquetes requeridos

Instale los siguientes paquetes NuGet que encontrará en [NuGet](https://www.nuget.org/):

- windowsazure.mediaservices
- windowsazure.mediaservices.extensions

### <a name="update-the-programs-using-statements"></a>Actualizar las instrucciones "using" del programa

Modifique las instrucciones using del programa.

    using System;
    using System.Linq;
    using System.Threading.Tasks;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.IO;
    using System.Threading;
    using Microsoft.WindowsAzure.Storage.Blob;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using System.Collections.Generic;


### <a name="initialize-application-specific-settings"></a>Inicializar la configuración específica de la aplicación

Agregue los siguientes campos estáticos a la clase **Program** en Program.cs.

    // declare constants and globals
    private static CloudMediaContext _context = null;
    private static CloudStorageAccount _StorageAccount = null;

    // Azure Media Services (AMS) associated Storage Account, Key, and the Container that has 
    // a list of Blobs to be processed.
    static string STORAGE_NAME = "YOUR AMS ASSOCIATED BLOB STORAGE NAME";
    static string STORAGE_KEY = "YOUR AMS ASSOCIATED BLOB STORAGE KEY";
    static string STORAGE_CONTAINER_NAME = "YOUR BLOB CONTAINER FOR VIDEO FILES";

    private static StorageCredentials _StorageCredentials = null;

    // Azure Media Services authentication. See the quickstart for how to get these.
    private const string AZURE_AD_TENANT_NAME = "microsoft.onmicrosoft.com";
    private const string CLIENT_ID = "YOUR CLIENT ID"
    private const string CLIENT_SECRET = "YOUR CLIENT SECRET";

    // REST API endpoint, for example "https://accountname.restv2.westcentralus.media.azure.net/API".      
    private const string REST_API_ENDPOINT = "YOUR API ENDPOINT";

    // Content Moderator Media Processor Nam
    private const string MEDIA_PROCESSOR = "Azure Media Content Moderator";

    // Input and Output files in the current directory of the executable
    private const string INPUT_FILE = "VIDEO FILE NAME";
    private const string OUTPUT_FOLDER = "";

### <a name="create-a-preset-file-json"></a>Crear un archivo de valores preestablecidos (json)

Cree un archivo JSON en el directorio actual con el número de versión.

    private static readonly string CONTENT_MODERATOR_PRESET_FILE = "preset.json";
    //Example file content:
    //        {
    //             "version": "2.0"
    //        }
    private static readonly string CONTENT_MODERATOR_PRESET_FILE = "preset.json";

### <a name="add-the-following-code-to-the-main-method"></a>Agregar el siguiente código al método principal

El método principal primero crea un contexto de Azure Media y, en caso de que sus vídeos estén en el almacenamiento de blobs, creará a continuación un contexto de Azure Storage.
El código restante examina un vídeo de una carpeta local, blob o varios blobs dentro de un contenedor de almacenamiento de Azure.
Puede probar todas las opciones si decide comentar las otras líneas de código.

    // Create Azure Media Context
    CreateMediaContext();

    // Create Storage Context
    CreateStorageContext();

    // Use a file as the input.
    // IAsset asset = CreateAssetfromFile();
    
    // -- OR ---
    
    // Or a blob as the input
    // IAsset asset = CreateAssetfromBlob((CloudBlockBlob)GetBlobsList().First());

    // Then submit the asset to Content Moderator
    // RunContentModeratorJob(asset);

    //-- OR ----

    // Just run the content moderator on all blobs in a list (from a Blob Container)
    RunContentModeratorJobOnBlobs();

### <a name="add-the-code-to-create-an-azure-media-context"></a>Agregar el código para crear un contexto de Azure Media

    /// <summary>
    /// Creates a media context from azure credentials
    /// </summary>
    static void CreateMediaContext()
    {
        // Get Azure AD credentials
        var tokenCredentials = new AzureAdTokenCredentials(AZURE_AD_TENANT_NAME,
            new AzureAdClientSymmetricKey(CLIENT_ID, CLIENT_SECRET),
            AzureEnvironments.AzureCloudEnvironment);

        // Initialize an Azure AD token
        var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

        // Create a media context
        _context = new CloudMediaContext(new Uri(REST_API_ENDPOINT), tokenProvider);
    }

### <a name="add-the-code-to-create-an-azure-storage-context"></a>Agregar el código para crear un contexto de Azure Storage
Use el contexto de almacenamiento que se creó a partir de sus credenciales de Storage, para acceder al almacenamiento de blobs.

    /// <summary>
    /// Creates a storage context from the AMS associated storage name and key
    /// </summary>
    static void CreateStorageContext()
    {
        // Get a reference to the storage account associated with a Media Services account. 
        if (_StorageCredentials == null)
        {
            _StorageCredentials = new StorageCredentials(STORAGE_NAME, STORAGE_KEY);
        }
        _StorageAccount = new CloudStorageAccount(_StorageCredentials, false);
    }

### <a name="add-the-code-to-create-azure-media-assets-from-local-file-and-blob"></a>Agregar el código para crear recursos de Azure Media a partir de un archivo local y un blob
El procesador de multimedia de Content Moderator ejecuta trabajos en la opción **Recursos** de la plataforma Azure Media Services.
Estos métodos crean los recursos de un archivo local o un blob asociado.

    /// <summary>
    /// Creates an Azure Media Services Asset from the video file
    /// </summary>
    /// <returns>Asset</returns>
    static IAsset CreateAssetfromFile()
    {
        return _context.Assets.CreateFromFile(INPUT_FILE, AssetCreationOptions.None); ;
    }

    /// <summary>
    /// Creates an Azure Media Services asset from your blog storage
    /// </summary>
    /// <param name="Blob"></param>
    /// <returns>Asset</returns>
    static IAsset CreateAssetfromBlob(CloudBlockBlob Blob)
    {
        // Create asset from the FIRST blob in the list and return it
        return _context.Assets.CreateFromBlob(Blob, _StorageCredentials, AssetCreationOptions.None);
    }

### <a name="add-the-code-to-scan-a-collection-of-videos-as-blobs-within-a-container"></a>Agregar el código para examinar una colección de vídeos (como blobs) en un contenedor

    /// <summary>
    /// Runs the Content Moderator Job on all Blobs in a given container name
    /// </summary>
    static void RunContentModeratorJobOnBlobs()
    {
        // Get the reference to the list of Blobs. See the following method.
        var blobList = GetBlobsList();

        // Iterate over the Blob list items or work on specific ones as needed
        foreach (var sourceBlob in blobList)
        {
            // Create an Asset
            IAsset asset = _context.Assets.CreateFromBlob((CloudBlockBlob)sourceBlob,
                               _StorageCredentials, AssetCreationOptions.None);
            asset.Update();

            // Submit to Content Moderator
            RunContentModeratorJob(asset);
        }
    }

    /// <summary>
    /// Get all blobs in your container
    /// </summary>
    /// <returns></returns>
    static IEnumerable<IListBlobItem> GetBlobsList()
    {
        // Get a reference to the Container within the Storage Account
        // that has the files (blobs) for moderation
        CloudBlobClient CloudBlobClient = _StorageAccount.CreateCloudBlobClient();
        CloudBlobContainer MediaBlobContainer = CloudBlobClient.GetContainerReference(STORAGE_CONTAINER_NAME);

        // Get the reference to the list of Blobs 
        var blobList = MediaBlobContainer.ListBlobs();
        return blobList;
    }

### <a name="add-the-method-to-run-the-content-moderator-job"></a>Agregar el método para ejecutar el trabajo de Content Moderator

    /// <summary>
    /// Run the Content Moderator job on the designated Asset from local file or blob storage
    /// </summary>
    /// <param name="asset"></param>
    static void RunContentModeratorJob(IAsset asset)
    {
        // Grab the presets
        string configuration = File.ReadAllText(CONTENT_MODERATOR_PRESET_FILE);

        // grab instance of Azure Media Content Moderator MP
        IMediaProcessor mp = _context.MediaProcessors.GetLatestMediaProcessorByName(MEDIA_PROCESSOR);

        // create Job with Content Moderator task
        IJob job = _context.Jobs.Create(String.Format("Content Moderator {0}",
                asset.AssetFiles.First() + "_" + Guid.NewGuid()));

        ITask contentModeratorTask = job.Tasks.AddNew("Adult and racy classifier task",
                mp, configuration,
                TaskOptions.None);
        contentModeratorTask.InputAssets.Add(asset);
        contentModeratorTask.OutputAssets.AddNew("Adult and racy classifier output",
            AssetCreationOptions.None);

        job.Submit();


        // Create progress printing and querying tasks
        Task progressPrintTask = new Task(() =>
        {
            IJob jobQuery = null;
            do
            {
                var progressContext = _context;
                jobQuery = progressContext.Jobs
                .Where(j => j.Id == job.Id)
                    .First();
                    Console.WriteLine(string.Format("{0}\t{1}",
                    DateTime.Now,
                    jobQuery.State));
                    Thread.Sleep(10000);
             }
             while (jobQuery.State != JobState.Finished &&
             jobQuery.State != JobState.Error &&
             jobQuery.State != JobState.Canceled);
        });
        progressPrintTask.Start();

        Task progressJobTask = job.GetExecutionProgressTask(
        CancellationToken.None);
        progressJobTask.Wait();

        // If job state is Error, the event handling 
        // method for job progress should log errors.  Here we check 
        // for error state and exit if needed.
        if (job.State == JobState.Error)
        {
            ErrorDetail error = job.Tasks.First().ErrorDetails.First();
            Console.WriteLine(string.Format("Error: {0}. {1}",
            error.Code,
            error.Message));
        }

        DownloadAsset(job.OutputMediaAssets.First(), OUTPUT_FOLDER);
    }

### <a name="add-a-couple-of-helper-functions"></a>Agregar un par de funciones auxiliares

Estos métodos descargan el archivo de salida (JSON) de Content Moderator del recurso de Azure Media Services, y le permiten hacer un seguimiento del estado del trabajo de moderación para que el programa pueda registrar el estado de ejecución en la consola.

    static void DownloadAsset(IAsset asset, string outputDirectory)
    {
        foreach (IAssetFile file in asset.AssetFiles)
        {
            file.Download(Path.Combine(outputDirectory, file.Name));
        }
    }

    // event handler for Job State
    static void StateChanged(object sender, JobStateChangedEventArgs e)
    {
        Console.WriteLine("Job state changed event:");
        Console.WriteLine("  Previous state: " + e.PreviousState);
        Console.WriteLine("  Current state: " + e.CurrentState);
        switch (e.CurrentState)
        {
            case JobState.Finished:
                Console.WriteLine();
                Console.WriteLine("Job finished.");
                break;
            case JobState.Canceling:
            case JobState.Queued:
            case JobState.Scheduled:
            case JobState.Processing:
                Console.WriteLine("Please wait...\n");
                break;
            case JobState.Canceled:
                Console.WriteLine("Job is canceled.\n");
                break;
            case JobState.Error:
                Console.WriteLine("Job failed.\n");
                break;
            default:
                break;
        }
    }

### <a name="run-the-program-and-review-the-output"></a>Ejecutar el programa y revisar la salida

Una vez completado el trabajo de moderación de contenido, analice la respuesta JSON. Esta consta de estos elementos:

- Resumen de información del vídeo
- **Capturas**  denominadas "**fragmentos**".
- **Fotogramas clave**  denominados "**eventos**" con una marca **reviewRecommended" (= true o false)"** basada en puntuaciones de tipo **Adultos** e **Inapropiado**.
- Los valores **start**, **duration** , **totalDuration** y **timestamp** están entrecomillados. Divida con el valor **timescale** para obtener el número en segundos.
 
> [!NOTE]

> - `adultScore` representa la posible presencia y la puntuación de predicción de contenido que se pueda considerar sexualmente explícito o para adultos en ciertas situaciones.
> - `racyScore` representa la posible presencia y la puntuación de predicción de contenido que se pueda considerar sexualmente insinuante o para adultos en ciertas situaciones.
> - `adultScore` y `racyScore` están entre 0 y 1. Cuanto más alta sea la puntuación, mayor será la predicción del modelo sobre la aplicabilidad de la categoría. Esta versión preliminar usa un modelo estadístico, en lugar de resultados codificados manualmente. Se recomienda realizar pruebas con contenido propio para determinar cómo se alinea cada categoría en función de sus propios requisitos.
> - `reviewRecommended` es true o false según los umbrales de puntuación internos. Los clientes deben evaluar si quieren usar este valor o decidir si desean umbrales personalizados basados en sus directivas de contenido.
>

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

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo generar [reseñas de vídeos](video-reviews-quickstart-dotnet.md) desde la salida de moderación.

Agregue la [moderación de transcripciones](video-transcript-moderation-review-tutorial-dotnet.md) a sus reseñas de vídeos.

Consulte el tutorial detallado sobre cómo crear una [solución completa de moderación de vídeos y transcripciones ](video-transcript-moderation-review-tutorial-dotnet.md).

[Descargue la solución de Visual Studio](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) para este y otros inicios rápidos de Content Moderator para .NET.
