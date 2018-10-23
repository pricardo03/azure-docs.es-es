---
title: 'Guía de inicio rápido: comprobación del contenido de la imagen en C# con Content Moderator'
titlesuffix: Azure Cognitive Services
description: Comprobación del contenido de la imagen con el SDK de Content Moderator para C#
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: quickstart
ms.date: 10/10/2018
ms.author: sajagtap
ms.openlocfilehash: 4973d78eac02aed42689bf5742155c375d5f78ae
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2018
ms.locfileid: "49309304"
---
# <a name="quickstart-check-image-content-in-c"></a>Guía de inicio rápido: comprobación del contenido de la imagen en C# 

En este artículo se proporcionan información y ejemplos de código que le ayudarán a empezar a usar el [SDK de Content Moderator para .NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) para comprobar simultáneamente una imagen: 

- el contenido para adultos o subido de tono
- el texto extraíble
- los rostros de personas

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar. 

## <a name="sign-up-for-content-moderator-services"></a>Suscribirse a los servicios de Content Moderator

Antes de usar los servicios de Content Moderator mediante API REST o del SDK, necesita una clave de API y la región de la cuenta de la API. Suscríbase a Content Moderator en [Azure Portal](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator) para obtenerlas.

## <a name="create-your-visual-studio-project"></a>Crear un proyecto de Visual Studio

1. Agregue un nuevo proyecto de **Aplicación de consola (.NET Framework)** a la solución.

   En el código de ejemplo, póngale al proyecto el nombre **ImageModeration**.

1. Seleccione este proyecto como proyecto de inicio único para la solución.


### <a name="install-required-packages"></a>Instalación de los paquetes requeridos

Instale los siguientes paquetes NuGet:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Newtonsoft.Json

### <a name="update-the-programs-using-statements"></a>Actualizar las instrucciones using del programa

Agregue las siguientes instrucciones `using`.

```csharp
using Microsoft.Azure.CognitiveServices.ContentModerator;
using Microsoft.CognitiveServices.ContentModerator;
using Microsoft.CognitiveServices.ContentModerator.Models;
using Newtonsoft.Json;
using System;
using System.Collections.Generic;
using System.IO;
using System.Threading;
```

### <a name="create-the-content-moderator-client"></a>Creación del cliente de Content Moderator

Agregue el código siguiente para crear un cliente de Content Moderator para su suscripción.

> [!IMPORTANT]
> Actualice los campos **AzureRegion** y **CMSubscriptionKey** con los valores de su identificador de región y clave de suscripción.

```csharp
/// <summary>
/// Wraps the creation and configuration of a Content Moderator client.
/// </summary>
/// <remarks>This class library contains insecure code. If you adapt this 
/// code for use in production, use a secure method of storing and using
/// your Content Moderator subscription key.</remarks>
public static class Clients
{
    /// <summary>
    /// The region/location for your Content Moderator account, 
    /// for example, westus.
    /// </summary>
    private static readonly string AzureRegion = "YOUR API REGION";

    /// <summary>
    /// The base URL fragment for Content Moderator calls.
    /// </summary>
    private static readonly string AzureBaseURL =
        $"https://{AzureRegion}.api.cognitive.microsoft.com";

    /// <summary>
    /// Your Content Moderator subscription key.
    /// </summary>
    private static readonly string CMSubscriptionKey = "YOUR API KEY";

    /// <summary>
    /// Returns a new Content Moderator client for your subscription.
    /// </summary>
    /// <returns>The new client.</returns>
    /// <remarks>The <see cref="ContentModeratorClient"/> is disposable.
    /// When you have finished using the client,
    /// you should dispose of it either directly or indirectly. </remarks>
    public static ContentModeratorClient NewClient()
    {
        // Create and initialize an instance of the Content Moderator API wrapper.
        ContentModeratorClient client = new ContentModeratorClient(new ApiKeyServiceClientCredentials(CMSubscriptionKey));

        client.Endpoint = AzureBaseURL;
        return client;
    }
}
```

### <a name="initialize-application-specific-settings"></a>Inicializar la configuración específica de la aplicación

Agregue los siguientes campos estáticos a la clase **Program** en Program.cs.

```csharp
///<summary>
///The name of the file that contains the image URLs to evaluate.
///</summary>
///<remarks>You will need to create an input file and update 
///this path accordingly. Paths are relative to the execution directory.
///</remarks>
private static string ImageUrlFile = "ImageFiles.txt";

///<summary>
///The name of the file to contain the output from the evaluation.
///</summary>
///<remarks>Paths are relative to the execution directory.
///</remarks>
private static string OutputFile = "ModerationOutput.json";
```

Cree un archivo de entrada, _ImageFiles.txt_, y agregue las direcciones URL de las imágenes que desea analizar. Esta guía de inicio rápido usa las dos direcciones URL siguientes para generar la salida de ejemplo.
- https://moderatorsampleimages.blob.core.windows.net/samples/sample2.jpg
- https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png

## <a name="create-a-class-to-handle-results"></a>Creación de una clase para controlar los resultados

Agregue la clase siguiente a la clase **Program**. Se utilizará una instancia de esta clase para registrar los resultados de la moderación de las imágenes revisadas.

```csharp
/// <summary>
/// Contains the image moderation results for an image, 
/// including text and face detection results.
/// </summary>
public class EvaluationData
{
    /// <summary>
    /// The URL of the evaluated image.
    /// </summary>
    public string ImageUrl;

    /// <summary>
    /// The image moderation results.
    /// </summary>
    public Evaluate ImageModeration;

    /// <summary>
    /// The text detection results.
    /// </summary>
    public OCR TextDetection;

    /// <summary>
    /// The face detection results;
    /// </summary>
    public FoundFaces FaceDetection;
}
```

## <a name="create-the-image-evaluation-method"></a>Creación del método de evaluación de la imagen

Agregue el método siguiente a la clase **Program**. Este método evalúa una sola imagen y devuelve los resultados de la evaluación.

> [!NOTE]
> La clave de servicio de Content Moderator tiene un límite de solicitudes por segundo (RPS) y, si se supera, el SDK emite una excepción con un código de error 429. Una clave de un plan gratuito tiene un límite de una solicitud por segundo.

```csharp
/// <summary>
/// Evaluates an image using the Image Moderation APIs.
/// </summary>
/// <param name="client">The Content Moderator API wrapper to use.</param>
/// <param name="imageUrl">The URL of the image to evaluate.</param>
/// <returns>Aggregated image moderation results for the image.</returns>
/// <remarks>This method throttles calls to the API.
/// Your Content Moderator service key will have a requests per second (RPS)
/// rate limit, and the SDK will throw an exception with a 429 error code 
/// if you exceed that limit. A free tier key has a 1 RPS rate limit.
/// </remarks>
private static EvaluationData EvaluateImage(
  ContentModeratorClient client, string imageUrl)
{
    var url = new ImageUrl("URL", imageUrl.Trim());

    var imageData = new EvaluationData();

    imageData.ImageUrl = url.Value;

  // Evaluate for adult and racy content.
    imageData.ImageModeration =
        client.ImageModeration.EvaluateUrlInput("application/json", url, true);
    Thread.Sleep(1000);

    // Detect and extract text.
    imageData.TextDetection =
        client.ImageModeration.OCRUrlInput("eng", "application/json", url, true);
    Thread.Sleep(1000);

    // Detect faces.
    imageData.FaceDetection =
        client.ImageModeration.FindFacesUrlInput("application/json", url, true);
    Thread.Sleep(1000);

    return imageData;
}
```

El método **EvaluateUrlInput** es un contenedor para Image Moderation REST API.
El valor devuelto contiene el objeto devuelto por la llamada API.

El método **OCRUrlInput** es un contenedor para Image OCR REST API.
El valor devuelto contiene el objeto devuelto por la llamada API.

El método **FindFacesUrlInput** es un contenedor para Image Find Faces REST API.
El valor devuelto contiene el objeto devuelto por la llamada API.

## <a name="evaluate-the-images-in-your-code"></a>Evaluación de las imágenes del código

Agregue el siguiente código al método **Main**.

```csharp
// Create an object to store the image moderation results.
List<EvaluationData> evaluationData = new List<EvaluationData>();

// Create an instance of the Content Moderator API wrapper.
using (var client = Clients.NewClient())
{
    // Read image URLs from the input file and evaluate each one.
    using (StreamReader inputReader = new StreamReader(ImageUrlFile))
    {
        while (!inputReader.EndOfStream)
        {
            string line = inputReader.ReadLine().Trim();
            if (line != String.Empty)
            {
                EvaluationData imageData = EvaluateImage(client, line);
                evaluationData.Add(imageData);
            }
        }
    }
}

// Save the moderation results to a file.
using (StreamWriter outputWriter = new StreamWriter(OutputFile, false))
{
    outputWriter.WriteLine(JsonConvert.SerializeObject(
        evaluationData, Formatting.Indented));

    outputWriter.Flush();
    outputWriter.Close();
}
```

## <a name="run-the-program-and-review-the-output"></a>Ejecutar el programa y revisar la salida

Abra el archivo _ModerationOutput.json_ para ver el contenido de salida. Debe tener un aspecto similar al siguiente contenido. Tenga en cuenta que cada imagen tiene secciones diferentes para `ImageModeration`, `FaceDetection`, y `TextDetection`, que corresponden a las tres llamadas de API del método **EvaluateImage**.

```json
[{
  "ImageUrl": "https://moderatorsampleimages.blob.core.windows.net/samples/sample2.jpg",
  "ImageModeration": {
    "cacheID": "7733c303-3b95-4710-a41e-7a322ae81a15_636488005858745661",
    "result": false,
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_687c356d-0f00-4aeb-ae5f-c7555af80247",
    "adultClassificationScore": 0.019196987152099609,
    "isImageAdultClassified": false,
    "racyClassificationScore": 0.032390203326940536,
    "isImageRacyClassified": false,
    "advancedInfo": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "116"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "273405"
      }
    ],
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    }
  },
  "TextDetection": {
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    },
    "metadata": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "12"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "273405"
      }
    ],
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_814fa162-c5d6-4ca6-997b-30ed0686ca83",
    "cacheId": "3fb69496-c64b-4de9-affd-6dd6d23f3e78_636488005876558920",
    "language": "eng",
    "text": "IF WE DID \r\nALL \r\nTHE THINGS \r\nWE ARE \r\nCAPABLE \r\nOF DOING, \r\nWE WOULD \r\nLITERALLY \r\nASTOUND \r\nOURSELVE \r\n",
    "candidates": []
  },
  "FaceDetection": {
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    },
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_a2c40dbe-609d-4eb8-b01c-9988388804ea",
    "cacheId": "e4c0b500-ea8e-4a31-8fb3-35f98c4fbd65_636488005889528303",
    "result": false,
    "count": 0,
    "advancedInfo": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "11"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "273405"
      }
    ],
    "faces": []
  }
},
{
  "ImageUrl": "https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png",
  "ImageModeration": {
    "cacheID": "b4866aa2-5e69-44ed-806a-f9a5d618c8ae_636488005930693926",
    "result": false,
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_fdce5510-f689-4791-b081-c2ad54dcfe78",
    "adultClassificationScore": 0.0035635426174849272,
    "isImageAdultClassified": false,
    "racyClassificationScore": 0.021369094029068947,
    "isImageRacyClassified": false,
    "advancedInfo": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "109"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "2278902"
      }
    ],
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    }
  },
  "TextDetection": {
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    },
    "metadata": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "46"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "2278902"
      }
    ],
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_08a4bc19-6010-41bb-a440-a77278e167d8",
    "cacheId": "28b37471-41b3-4f79-bd23-965498bcff51_636488005950851288",
    "language": "eng",
    "text": "",
    "candidates": []
  },
  "FaceDetection": {
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    },
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_40f2ce07-14ba-47cd-ba09-58b557a89854",
    "cacheId": "ec9c1be3-99b7-4bd9-8bc4-dc958c74459f_636488005964914299",
    "result": true,
    "count": 6,
    "advancedInfo": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "60"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "2278902"
      }
    ],
    "faces": [
      {
        "bottom": 598,
        "left": 44,
        "right": 268,
        "top": 374
      },
      {
        "bottom": 620,
        "left": 308,
        "right": 532,
        "top": 396
      },
      {
        "bottom": 575,
        "left": 594,
        "right": 773,
        "top": 396
      },
      {
        "bottom": 563,
        "left": 812,
        "right": 955,
        "top": 420
      },
      {
        "bottom": 611,
        "left": 972,
        "right": 1151,
        "top": 432
      },
      {
        "bottom": 510,
        "left": 1232,
        "right": 1456,
        "top": 286
      }
    ]
  }
}]
```

## <a name="next-steps---get-the-source-code"></a>Pasos siguientes: obtención del código fuente

Obtenga el [SDK de Content Moderator para .NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) y la [solución de Visual Studio](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) para este y otros inicios rápidos de Content Moderator para .NET y empiece a trabajar en la integración.
