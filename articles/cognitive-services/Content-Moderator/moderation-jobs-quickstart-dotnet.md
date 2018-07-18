---
title: 'Azure Content Moderator: inicio de trabajos de moderación mediante .NET | Microsoft Docs'
description: Inicio de trabajos de moderación mediante el SDK de Azure Content Moderator para .NET
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/06/2018
ms.author: sajagtap
ms.openlocfilehash: a103875607355993e216ce1ddea02009fc8fa1c4
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380219"
---
# <a name="start-moderation-jobs-using-net"></a>Inicio de trabajos de moderación mediante .NET

En este artículo se proporciona información y ejemplos de código que le ayudarán a empezar a usar el SDK de Content Moderator para .NET con las siguientes finalidades:
 
- Iniciar un trabajo de moderación para examinar y crear revisiones para moderadores humanos
- Obtener el estado de la revisión pendiente
- Realizar un seguimiento de la revisión y obtener su estado final
- Enviar el resultado a la dirección URL de devolución de llamada

En este artículo se da por supuesto que ya está familiarizado con Visual Studio y C#.

## <a name="sign-up-for-content-moderator-services"></a>Suscripción a los servicios de Content Moderator

Antes de poder usar los servicios de Content Moderator mediante la API REST o el SDK, necesita una clave de suscripción.
Consulte el [inicio rápido](quick-start.md) para más información sobre cómo puede obtener la clave.

## <a name="define-a-custom-moderation-workflow"></a>Definición de un flujo de trabajo de moderación personalizado

En un trabajo de moderación, el contenido se examina con las API y se usa un **flujo de trabajo** para determinar si se deben crear o no revisiones.
Si bien la herramienta de revisión contiene un flujo de trabajo predeterminado, en este inicio rápido vamos a [definir un flujo de trabajo personalizado](Review-Tool-User-Guide/Workflows.md).

Usará el nombre del flujo de trabajo en el código que inicia el trabajo de moderación.

## <a name="create-your-visual-studio-project"></a>Creación del proyecto de Visual Studio

1. Agregue un nuevo proyecto de **Aplicación de consola (.NET Framework)** a la solución.

   En el código de ejemplo, asigne el nombre **CreateReviews** al proyecto.

1. Seleccione este proyecto como proyecto de inicio único para la solución.

1. Agregue una referencia al conjunto de proyectos **ModeratorHelper** que creó en [Content Moderator client helper quickstart](content-moderator-helper-quickstart-dotnet.md) (Inicio rápido de la aplicación auxiliar cliente de Content Moderator).

### <a name="install-required-packages"></a>Instalación de los paquetes requeridos

Instale los siguientes paquetes NuGet:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Newtonsoft.Json

### <a name="update-the-programs-using-statements"></a>Actualización de las instrucciones using del programa

Modifique las instrucciones using del programa.

    using Microsoft.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator.Models;
    using ModeratorHelper;
    using Newtonsoft.Json;
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Threading;

### <a name="initialize-application-specific-settings"></a>Inicialización de la configuración específica de la aplicación

Agregue las siguientes constantes y campos estáticos a la clase **Program** en Program.cs.

> [!NOTE]
> Establecerá la constante TeamName en el nombre que usó al crear la suscripción a Content Moderator. Recuperará TeamName del [sitio web de Content Moderator](https://westus.contentmoderator.cognitive.microsoft.com/).
> Una vez que ha iniciado sesión, seleccione **Credentials** (Credenciales) en el menú **Settings** (Configuración) (engranaje).
>
> El nombre del equipo es el valor del campo **ID** de la sección **API**.


    /// <summary>
    /// The moderation job will use this workflow that you defined earlier.
    /// See the quickstart article to learn how to setup custom workflows.
    /// </summary>
    private const string WorkflowName = "OCR";
    
    /// <summary>
    /// The name of the team to assign the job to.
    /// </summary>
    /// <remarks>This must be the team name you used to create your 
    /// Content Moderator account. You can retrieve your team name from
    /// the Conent Moderator web site. Your team name is the Id associated 
    /// with your subscription.</remarks>
    private const string TeamName = "***";

    /// <summary>
    /// The URL of the image to create a review job for.
    /// </summary>
    private const string ImageUrl =
        "https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png";

    /// <summary>
    /// The name of the log file to create.
    /// </summary>
    /// <remarks>Relative paths are ralative the execution directory.</remarks>
    private const string OutputFile = "OutputLog.txt";

    /// <summary>
    /// The number of seconds to delay after a review has finished before
    /// getting the review results from the server.
    /// </summary>
    private const int latencyDelay = 45;

    /// <summary>
    /// The callback endpoint for completed reviews.
    /// </summary>
    /// <remarks>Revies show up for reviewers on your team. 
    /// As reviewers complete reviews, results are sent to the
    /// callback endpoint using an HTTP POST request.</remarks>
    private const string CallbackEndpoint = "";

## <a name="add-code-to-auto-moderate-create-a-review-and-get-the-job-details"></a>Adición de código para moderar automáticamente, crear una revisión y obtener los detalles del trabajo

> [!Note]
> En la práctica, debe establecer la dirección URL de devolución de llamada **CallbackEndpoint** en la dirección URL que recibe los resultados de la revisión manual (mediante una solicitud HTTP POST).

Para comenzar, agregue el siguiente código al método **Main**.

    using (TextWriter writer = new StreamWriter(OutputFile, false))
    {
        using (var client = Clients.NewClient())
        {
            writer.WriteLine("Create review job for an image.");
            var content = new Content(ImageUrl);
        
            // The WorkflowName contains the nameof the workflow defined in the online review tool.
            // See the quickstart article to learn more.
            var jobResult = client.Reviews.CreateJobWithHttpMessagesAsync(
                    TeamName, "image", "contentID", WorkflowName, "application/json", content, CallbackEndpoint);

            // Record the job ID.
            var jobId = jobResult.Result.Body.JobIdProperty;

            // Log just the response body from the returned task.
            writer.WriteLine(JsonConvert.SerializeObject(
                jobResult.Result.Body, Formatting.Indented));

            Thread.Sleep(2000);
            writer.WriteLine();

            writer.WriteLine("Get review job status.");
            var jobDetails = client.Reviews.GetJobDetailsWithHttpMessagesAsync(
                    TeamName, jobId);

            // Log just the response body from the returned task.
            writer.WriteLine(JsonConvert.SerializeObject(
                    jobDetails.Result.Body, Formatting.Indented));

            Console.WriteLine();
            Console.WriteLine("Perform manual reviews on the Content Moderator site.");
            Console.WriteLine("Then, press any key to continue.");
            Console.ReadKey();

            Console.WriteLine();
            Console.WriteLine($"Waiting {latencyDelay} seconds for results to propagate.");
            Thread.Sleep(latencyDelay * 1000);

            writer.WriteLine("Get review details.");
            jobDetails = client.Reviews.GetJobDetailsWithHttpMessagesAsync(
            TeamName, jobId);

            // Log just the response body from the returned task.
            writer.WriteLine(JsonConvert.SerializeObject(
            jobDetails.Result.Body, Formatting.Indented));
        }
        writer.Flush();
        writer.Close();
    }

> [!NOTE]
> La clave de servicio de Content Moderator tiene un límite de frecuencia de solicitudes por segundo (RPS). Si se supera el límite, el SDK produce una excepción con un código de error 429. 
>
> Una clave de un plan gratuito tiene un límite de una solicitud por segundo.

## <a name="run-the-program-and-review-the-output"></a>Ejecución del programa y revisión de la salida

Verá la siguiente salida de ejemplo en la consola:

    Perform manual reviews on the Content Moderator site.
    Then, press any key to continue.

Inicie sesión en la herramienta de revisión de Content Moderator para ver la revisión de imagen pendiente.

Use el botón **Next** (Siguiente) para enviar.

![Revisión de imagen para moderadores humanos](images/ocr-sample-image.PNG)

## <a name="see-the-sample-output-in-the-log-file"></a>Visualización de la salida de ejemplo en el archivo de registro

> [!NOTE]
> En el archivo de salida, las cadenas **Teamname**, **ContentId**, **CallBackEndpoint** y **WorkflowId** reflejan los valores que usó anteriormente.

    Create moderation job for an image.
    {
        "JobId": "2018014caceddebfe9446fab29056fd8d31ffe"
    }

    Get review details.
    {
        "Id": "2018014caceddebfe9446fab29056fd8d31ffe",
        "TeamName": "some team name",
        "Status": "InProgress",
        "WorkflowId": "OCR",
        "Type": "Image",
        "CallBackEndpoint": "",
        "ReviewId": "",
        "ResultMetaData": [],
        "JobExecutionReport": [
        {
            "Ts": "2018-01-07T00:38:26.7714671",
            "Msg": "Successfully got hasText response from Moderator"
        },
        {
            "Ts": "2018-01-07T00:38:26.4181346",
            "Msg": "Getting hasText from Moderator"
        },
        {
            "Ts": "2018-01-07T00:38:25.5122828",
            "Msg": "Starting Execution - Try 1"
        }
        ]
    }


## <a name="your-callback-url-if-provided-receives-this-response"></a>La dirección URL de devolución de llamada, si se proporciona, recibe esta respuesta.

Verá una respuesta similar al siguiente ejemplo:

> [!NOTE]
> En la respuesta de devolución de llamada, las cadenas **ContentId** y **WorkflowId** reflejan los valores que usó anteriormente.

    {
        "JobId": "2018014caceddebfe9446fab29056fd8d31ffe",
        "ReviewId": "201801i28fc0f7cbf424447846e509af853ea54",
        "WorkFlowId": "OCR",
        "Status": "Complete",
        "ContentType": "Image",
        "CallBackType": "Job",
        "ContentId": "contentID",
        "Metadata": {
            "hastext": "True",
            "ocrtext": "IF WE DID \r\nALL \r\nTHE THINGS \r\nWE ARE \r\nCAPABLE \r\nOF DOING, \r\nWE WOULD \r\nLITERALLY \r\nASTOUND \r\nOURSELVE \r\n",
            "imagename": "contentID"
        }
    }


## <a name="next-steps"></a>Pasos siguientes

[Descargue la solución de Visual Studio](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) para este y otros inicios rápidos de Content Moderator para .NET y empiece a trabajar en la integración.
