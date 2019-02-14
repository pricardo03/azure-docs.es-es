---
title: 'Inicio rápido: Inicio de trabajos de moderación mediante .NET: Content Moderator'
titlesuffix: Azure Cognitive Services
description: Cómo iniciar trabajos de moderación mediante el SDK de Azure Content Moderator para .NET.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: quickstart
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 0664e75a299246d9dd2cc14dbab31d22a1bd9c4b
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55878085"
---
# <a name="quickstart-start-moderation-jobs-using-net"></a>Inicio rápido: Inicio de trabajos de moderación mediante .NET

En este artículo se proporciona información y ejemplos de código que le ayudarán a empezar a usar el [SDK de Content Moderator para .NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) para:
 
- Iniciar un trabajo de moderación para examinar y crear revisiones para moderadores humanos
- Obtener el estado de la revisión pendiente
- Realizar un seguimiento de la revisión y obtener su estado final
- Enviar el resultado a la dirección URL de devolución de llamada

En este artículo se da por supuesto que ya está familiarizado con Visual Studio y C#.

## <a name="sign-up-for-content-moderator"></a>Registro en Content Moderator

Antes de poder usar los servicios de Content Moderator mediante la API REST o el SDK, necesita una clave de suscripción. Siga las instrucciones de [Creación de una cuenta de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) para suscribirse a Content Moderator y obtener su clave.

## <a name="sign-up-for-a-review-tool-account-if-not-completed-in-the-previous-step"></a>Registro para obtener una cuenta de la herramienta de revisión, si no lo ha hecho en el paso anterior

Si ha obtenido Content Moderator desde Azure Portal, [regístrese también para obtener la cuenta de la herramienta de revisión](https://contentmoderator.cognitive.microsoft.com/) y cree un equipo de revisión. Necesita el identificador de equipo y la herramienta de revisión para llamar a la API de revisión para iniciar un trabajo y ver las revisiones en la herramienta de revisión.

## <a name="ensure-your-api-key-can-call-the-review-api-for-review-creation"></a>Garantizar que la clave de API puede llamar a la API de revisión para la creación de revisiones

Tras completar los pasos anteriores, puede acabar con dos claves de Content Moderator si ha empezado desde Azure Portal. 

Si tiene pensado usar la clave de API proporcionada por Azure en el ejemplo del SDK, siga los pasos mencionados en la sección [Uso de la clave de Azure con la API de revisión](review-tool-user-guide/credentials.md#use-the-azure-account-with-the-review-tool-and-review-api) para permitir que la aplicación llame a la API de revisión y cree revisiones.

Si usa la clave de prueba gratuita generada por la herramienta de revisión, la cuenta de la herramienta de revisión ya conoce la clave y, por lo tanto, no se requiere ningún paso adicional.

## <a name="define-a-custom-moderation-workflow"></a>Definición de un flujo de trabajo de moderación personalizado

En un trabajo de moderación, el contenido se examina con las API y se usa un **flujo de trabajo** para determinar si se deben crear o no revisiones.
Si bien la herramienta de revisión contiene un flujo de trabajo predeterminado, en este inicio rápido vamos a [definir un flujo de trabajo personalizado](Review-Tool-User-Guide/Workflows.md).

Usará el nombre del flujo de trabajo en el código que inicia el trabajo de moderación.

## <a name="create-your-visual-studio-project"></a>Creación del proyecto de Visual Studio

1. Agregue un nuevo proyecto de **Aplicación de consola (.NET Framework)** a la solución.

   En el código de ejemplo, asigne el nombre **CreateReviews** al proyecto.

1. Seleccione este proyecto como proyecto de inicio único para la solución.

### <a name="install-required-packages"></a>Instalación de los paquetes requeridos

Instale los siguientes paquetes NuGet:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Newtonsoft.Json

### <a name="update-the-programs-using-statements"></a>Actualizar las instrucciones using del programa

Modifique las instrucciones using del programa.

    using Microsoft.Azure.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator.Models;
    using Newtonsoft.Json;
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Threading;

### <a name="create-the-content-moderator-client"></a>Creación del cliente de Content Moderator

Agregue el código siguiente para crear un cliente de Content Moderator para su suscripción.

> [!IMPORTANT]
> Actualice los campos **AzureRegion** y **CMSubscriptionKey** con los valores de su identificador de región y clave de suscripción.


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

### <a name="initialize-application-specific-settings"></a>Inicializar la configuración específica de la aplicación

Agregue las siguientes constantes y campos estáticos a la clase **Program** en Program.cs.

> [!NOTE]
> Establecerá la constante TeamName en el nombre que usó al crear la suscripción a Content Moderator. Recuperará TeamName del [sitio web de Content Moderator](https://westus.contentmoderator.cognitive.microsoft.com/).
> Una vez que ha iniciado sesión, seleccione **Credentials** (Credenciales) en el menú **Settings** (Configuración) (engranaje).
>
> El nombre de equipo es el valor del campo **Id** de la sección **API**.


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
    /// the Content Moderator web site. Your team name is the Id associated 
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
    /// <remarks>Relative paths are relative to the execution directory.</remarks>
    private const string OutputFile = "OutputLog.txt";

    /// <summary>
    /// The number of seconds to delay after a review has finished before
    /// getting the review results from the server.
    /// </summary>
    private const int latencyDelay = 45;

    /// <summary>
    /// The callback endpoint for completed reviews.
    /// </summary>
    /// <remarks>Reviews show up for reviewers on your team. 
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
        
            // The WorkflowName contains the name of the workflow defined in the online review tool.
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

Obtenga el [SDK de Content Moderator para .NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) y la [solución de Visual Studio](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) para este y otros inicios rápidos de Content Moderator para .NET y empiece a trabajar en la integración.
