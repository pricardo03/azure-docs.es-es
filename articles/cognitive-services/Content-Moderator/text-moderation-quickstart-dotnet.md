---
title: 'Moderación de texto: Content Moderator, .NET'
titlesuffix: Azure Cognitive Services
description: Cómo moderar texto con el SDK de Content Moderator para .NET
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: sajagtap
ms.openlocfilehash: 6db97231b2dc8905f1a3b3a6bc63580e9f371f84
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47227272"
---
# <a name="moderate-text-using-net"></a>Moderación de texto con .NET

En este artículo se proporciona información y ejemplos de código que le ayudarán a empezar a usar el [SDK de Content Moderator para .NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) con las siguientes finalidades:

- Detectar posibles palabras soeces en texto con filtrado basado en términos
- Usar modelos basados en aprendizaje automático para [clasificar el texto](text-moderation-api.md#classification) en tres categorías.
- Detectar información de identificación personal (PII) como números de teléfono de Estados Unidos y Reino Unido, direcciones de correo electrónico y direcciones postales de Estados Unidos.
- Normalizar el texto y corregir los errores tipográficos automáticamente

En este artículo se da por supuesto que ya está familiarizado con Visual Studio y C#.

## <a name="sign-up-for-content-moderator-services"></a>Suscribirse a los servicios de Content Moderator

Antes de poder usar los servicios de Content Moderator mediante la API REST o el SDK, necesita una clave de suscripción.
Consulte el [inicio rápido](quick-start.md) para más información sobre cómo puede obtener la clave.

## <a name="create-your-visual-studio-project"></a>Crear un proyecto de Visual Studio

1. Agregue un nuevo proyecto de **Aplicación de consola (.NET Framework)** a la solución.

   En el código de ejemplo, asigne el nombre **TextModeration** al proyecto.

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

Agregue los siguientes campos estáticos a la clase **Program** en Program.cs.

    /// <summary>
    /// The name of the file that contains the text to evaluate.
    /// </summary>
    /// <remarks>You will need to create an input file and update this path
    /// accordingly. Relative paths are relative to the execution directory.</remarks>
    private static string TextFile = "TextFile.txt";

    /// <summary>
    /// The name of the file to contain the output from the evaluation.
    /// </summary>
    /// <remarks>Relative paths are relative to the execution directory.</remarks>
    private static string OutputFile = "TextModerationOutput.txt";

En esta guía de inicio rápido hemos usado el texto siguiente para generar la salida.

> [!NOTE]
> El número de la seguridad social no válido del texto de ejemplo siguiente es intencionado. El propósito es transmitir la entrada de ejemplo y el formato de salida.

    Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052.
    These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 
    0800 820 3300. Also, 999-99-9999 looks like a social security number (SSN).

## <a name="add-code-to-load-and-evaluate-the-input-text"></a>Adición de código para cargar y evaluar el texto de entrada

Agregue el siguiente código al método **Main**.

    // Load the input text.
    string text = File.ReadAllText(TextFile);
    Console.WriteLine("Screening {0}", TextFile);

    text = text.Replace(System.Environment.NewLine, " ");

    // Save the moderation results to a file.
    using (StreamWriter outputWriter = new StreamWriter(OutputFile, false))
    {
        // Create a Content Moderator client and evaluate the text.
        using (var client = Clients.NewClient())
        {
            // Screen the input text: check for profanity, classify the text into three categories,
                // do autocorrect text, and check for personally identifying
                // information (PII)
                outputWriter.WriteLine("Autocorrect typos, check for matching terms, PII, and classify.");
                var screenResult =
                    client.TextModeration.ScreenText("eng", "text/plain", text, true, true, null, true);
                outputWriter.WriteLine(
                        JsonConvert.SerializeObject(screenResult, Formatting.Indented));
        }
        outputWriter.Flush();
        outputWriter.Close();
    }

> [!NOTE]
> La clave de servicio de Content Moderator tiene un límite de solicitudes por segundo (RPS) y, si se supera el límite, el SDK emite una excepción con un código de error 429.
>
> Cuando se usa una clave de nivel gratis, la tasa de solicitudes está limitada a una solicitud por segundo.

## <a name="run-the-program-and-review-the-output"></a>Ejecutar el programa y revisar la salida

La salida del ejemplo para el programa, escrita en el archivo de registro, es:

    Autocorrect typos, check for matching terms, PII, and classify.
    {
    "OriginalText": "\"Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052. These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300. Also, 999-99-9999 looks like a social security number (SSN).\"",
    "NormalizedText": "\" Is this a garbage or crap email abide@ abed. com, phone: 6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, Redmond, WA 98052. These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300. Also, 999-99-9999 looks like a social security number ( SSN) . \"",
    "AutoCorrectedText": "\" Is this a garbage or crap email abide@ abed. com, phone: 6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, Redmond, WA 98052. These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300. Also, 999-99-9999 looks like a social security number ( SSN) . \"",
    "Misrepresentation": null,
    
    "Classification": {
            "Category1": {
            "Score": 1.5113095059859916E-06
            },
            "Category2": {
            "Score": 0.12747249007225037
            },
            "Category3": {
            "Score": 0.98799997568130493
            },
            "ReviewRecommended": true
    },
    "Status": {
        "Code": 3000,
        "Description": "OK",
        "Exception": null
    },
    "PII": {
            "Email": [
                {
                "Detected": "abcdef@abcd.com",
                "SubType": "Regular",
                "Text": "abcdef@abcd.com",
                "Index": 33
                }
                ],
            "IPA": [
                {
                "SubType": "IPV4",
                "Text": "255.255.255.255",
                "Index": 73
                }
                ],
            "Phone": [
                {
                "CountryCode": "US",
                "Text": "6657789887",
                "Index": 57
                },
                {
                "CountryCode": "US",
                "Text": "870 608 4000",
                "Index": 211
                },
                {
                "CountryCode": "UK",
                "Text": "+44 870 608 4000",
                "Index": 207
                },
                {
                "CountryCode": "UK",
                "Text": "0344 800 2400",
                "Index": 227
                },
                {
            "   CountryCode": "UK",
                "Text": "0800 820 3300",
                "Index": 244
                }
                ],
             "Address": [{
                 "Text": "1 Microsoft Way, Redmond, WA 98052",
                 "Index": 89
                    }]
        },
    "Language": "eng",
    "Terms": [
        {
            "Index": 22,
            "OriginalIndex": 22,
            "ListId": 0,
            "Term": "crap"
        }
    ],
    "TrackingId": "9392c53c-d11a-441d-a874-eb2b93d978d3"
    }

## <a name="next-steps"></a>Pasos siguientes

Obtenga el [SDK de Content Moderator para .NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) y la [solución de Visual Studio](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) para este y otros inicios rápidos de Content Moderator para .NET y empiece a trabajar en la integración.
