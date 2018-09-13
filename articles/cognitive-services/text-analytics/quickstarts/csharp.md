---
title: Guía de inicio rápido de C# para Cognitive Services, Text Analytics API | Microsoft Docs
description: Obtenga información y ejemplos de código que le ayuden a empezar a usar rápidamente Text Analytics API en la solución Microsoft Cognitive Services de Azure.
services: cognitive-services
documentationcenter: ''
author: luiscabrer
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 08/30/2018
ms.author: ashmaka
ms.openlocfilehash: 42a682898303b742a17b0a6d4d98c2b9fedf9003
ms.sourcegitcommit: 3d0295a939c07bf9f0b38ebd37ac8461af8d461f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/06/2018
ms.locfileid: "43841701"
---
# <a name="quickstart-for-the-text-analytics-api-with-c"></a>Guía de inicio rápido de Text Analytics API con C# 
<a name="HOLTop"></a>

En este artículo, se explica cómo detectar el idioma, analizar la opinión y extraer frases clave utilizando [Text Analytics API](//go.microsoft.com/fwlink/?LinkID=759711) con C#. El código se ha escrito para que funcione en una aplicación de .NET Core y apenas contiene referencias a bibliotecas externas, por lo que también puede ejecutarse en Linux o MacOS.

Consulte las [definiciones de API](//go.microsoft.com/fwlink/?LinkID=759346) para obtener la documentación técnica de las API.

## <a name="prerequisites"></a>Requisitos previos

Debe tener una [cuenta de Cognitive Services API](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con Text Analytics API. Puede usar el *nivel gratuito de 5000 transacciones al mes* para completar este inicio rápido.

También debe tener [el punto de conexión y la clave de acceso](../How-tos/text-analytics-how-to-access-key.md) que se generaron durante el registro. 


## <a name="install-the-nuget-sdk-package"></a>Instalación del paquete SDK de NuGet
1. Cree una nueva solución de consola en Visual Studio.
1. Haga clic con el botón derecho en la solución y seleccione **Administrar paquetes NuGet para la solución**.
1. Active la casilla **Incluir versión preliminar**.
1. Seleccione la pestaña **Examinar** y busque **Microsoft.Azure.CognitiveServices.Language**.
1. Seleccione el paquete NuGet **Microsoft.Azure.CognitiveServices.Language.TextAnalytics** e instálelo.

> [!Tip]
> Aunque puede llamar a los [puntos de conexión HTTP](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) directamente desde C#, el SDK de Microsoft.Azure.CognitiveServices.Language hace que resulte más fácil llamar al servicio sin tener que preocuparse por la serialización y deserialización de JSON.
>
> A continuación, se incluyen algunos vínculos útiles:
> - [Página del SDK de NuGet](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics)
> - [Código del SDK](https://github.com/Azure/azure-sdk-for-net/tree/psSdkJson6/src/SDKs/CognitiveServices/dataPlane/Language/TextAnalytics)


## <a name="call-the-text-analytics-api-by-using-the-sdk"></a>Llamada a Text Analytics API mediante el SDK
1. Sustituya Program.cs por el código siguiente. En este programa, se muestran las funciones de Text Analytics API en tres secciones (extracción del idioma, extracción de frases clave y análisis de la opinión).
1. Sustituya el valor `Ocp-Apim-Subscription-Key` del encabezado por una clave de acceso que sea válida para la suscripción.
1. Cambie la ubicación de `Endpoint` por el punto de conexión para el que se ha registrado. Encontrará el punto de conexión en el recurso de Azure Portal. Normalmente, el punto de conexión suele empezar del modo siguiente: "https://[región].api.cognitive.microsoft.com". Especifique exclusivamente el protocolo y el nombre del host.
1. Ejecute el programa.

```csharp
using System;
using Microsoft.Azure.CognitiveServices.Language.TextAnalytics;
using Microsoft.Azure.CognitiveServices.Language.TextAnalytics.Models;
using System.Collections.Generic;
using Microsoft.Rest;
using System.Linq;
using System.Net.Http;
using System.Threading;
using System.Threading.Tasks;

namespace ConsoleApp1
{
    class Program
    {
        /// <summary>
        /// Container for subscription credentials. Make sure to enter your valid key.
        /// </summary>
        class ApiKeyServiceClientCredentials : ServiceClientCredentials
        {
            public override Task ProcessHttpRequestAsync(HttpRequestMessage request, CancellationToken cancellationToken)
            {
                request.Headers.Add("Ocp-Apim-Subscription-Key", "ENTER KEY HERE");
                return base.ProcessHttpRequestAsync(request, cancellationToken);
            }
        }

        static void Main(string[] args)
        {

            // Create a client.
            ITextAnalyticsClient client = new TextAnalyticsClient(new ApiKeyServiceClientCredentials())
            {
                Endpoint = "https://westus.api.cognitive.microsoft.com"
            };

            Console.OutputEncoding = System.Text.Encoding.UTF8;
```

## <a name="detect-language"></a>Detectar idioma

Language Detection API detecta el idioma de un documento de texto con el [método Detectar idioma](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7).

```csharp
            // Extracting language.
            Console.WriteLine("===== LANGUAGE EXTRACTION ======");

            var result =  client.DetectLanguageAsync(new BatchInput(
                    new List<Input>()
                        {
                          new Input("1", "This is a document written in English."),
                          new Input("2", "Este es un document escrito en Español."),
                          new Input("3", "这是一个用中文写的文件")
                    })).Result;

            // Printing language results.
            foreach (var document in result.Documents)
            {
                Console.WriteLine("Document ID: {0} , Language: {1}", document.Id, document.DetectedLanguages[0].Name);
            }
```

## <a name="extract-key-phrases"></a>Extracción de frases clave

Key Phrase Extraction API extrae frases clave de un documento de texto con el [método de frases clave](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6).

```csharp
            // Getting key phrases.
            Console.WriteLine("\n\n===== KEY-PHRASE EXTRACTION ======");

            KeyPhraseBatchResult result2 = client.KeyPhrasesAsync(new MultiLanguageBatchInput(
                        new List<MultiLanguageInput>()
                        {
                          new MultiLanguageInput("ja", "1", "猫は幸せ"),
                          new MultiLanguageInput("de", "2", "Fahrt nach Stuttgart und dann zum Hotel zu Fu."),
                          new MultiLanguageInput("en", "3", "My cat is stiff as a rock."),
                          new MultiLanguageInput("es", "4", "A mi me encanta el fútbol!")
                        })).Result;

            // Printing key phrases.
            foreach (var document in result2.Documents)
            {
                Console.WriteLine("Document ID: {0} ", document.Id);

                Console.WriteLine("\t Key phrases:");

                foreach (string keyphrase in document.KeyPhrases)
                {
                    Console.WriteLine("\t\t" + keyphrase);
                }
            }
```

## <a name="analyze-sentiment"></a>Análisis de opinión

Sentiment Analysis API detecta la opinión de un conjunto de registros de texto mediante el [método Sentiment](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9).

```csharp
            // Analyzing sentiment.
            Console.WriteLine("\n\n===== SENTIMENT ANALYSIS ======");

            SentimentBatchResult result3 = client.SentimentAsync(
                    new MultiLanguageBatchInput(
                        new List<MultiLanguageInput>()
                        {
                          new MultiLanguageInput("en", "0", "I had the best day of my life."),
                          new MultiLanguageInput("en", "1", "This was a waste of my time. The speaker put me to sleep."),
                          new MultiLanguageInput("es", "2", "No tengo dinero ni nada que dar..."),
                          new MultiLanguageInput("it", "3", "L'hotel veneziano era meraviglioso. È un bellissimo pezzo di architettura."),
                        })).Result;


            // Printing sentiment results.
            foreach (var document in result3.Documents)
            {
                Console.WriteLine("Document ID: {0} , Sentiment Score: {1:0.00}", document.Id, document.Score);
            }
```

## <a name="identify-linked-entities"></a>Identificación de entidades vinculadas

Entity Linking API identifica entidades conocidas en un documento de texto con el [método de vinculación de entidad](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634).

```csharp
            // Linking entities
            Console.WriteLine("\n\n===== ENTITY LINKING ======");

            EntitiesBatchResult result4 = client.EntitiesAsync(
                    new MultiLanguageBatchInput(
                        new List<MultiLanguageInput>()
                        {
                            new MultiLanguageInput("en", "0", "I really enjoy the new XBox One S. It has a clean look, it has 4K/HDR resolution and it is affordable."),
                            new MultiLanguageInput("en", "1", "The Seattle Seahawks won the Super Bowl in 2014."),
                        })).Result;

            // Printing entity results.
            foreach (var document in result4.Documents)
            {
                Console.WriteLine("Document ID: {0} , Entities: {1}", document.Id, String.Join(", ", document.Entities.Select(entity => entity.Name)));
            }
        }
    }
}
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Text Analytics con Power BI](../tutorials/tutorial-power-bi-key-phrases.md)

## <a name="see-also"></a>Otras referencias 

 [Información general de Text Analytics](../overview.md)  
 [Preguntas más frecuentes](../text-analytics-resource-faq.md)
