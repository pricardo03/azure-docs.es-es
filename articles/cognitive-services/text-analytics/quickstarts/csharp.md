---
title: 'Inicio rápido: Llamada al servicio Text Analytics mediante Azure SDK para .NET y C#'
titleSuffix: Azure Cognitive Services
description: Información y ejemplos de código que le ayudarán a empezar a usar el servicio Text Analytics y C#.
services: cognitive-services
author: raymondl
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 05/28/2019
ms.author: assafi
ms.openlocfilehash: 82297842a56930cec2b4de90998b4ffb904543bb
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446963"
---
# <a name="quickstart-use-the-net-sdk-and-c-to-call-the-text-analytics-service"></a>Inicio rápido: Uso de .NET y C# para llamar al servicio Text Analytics
<a name="HOLTop"></a>

Este inicio rápido le ayuda a empezar a usar Azure SDK para .NET y C# para analizar el lenguaje. Aunque la API REST [Text Analytics](//go.microsoft.com/fwlink/?LinkID=759711) es compatible con la mayoría de los lenguajes de programación, el SDK proporciona una forma sencilla de integrar el servicio en sus aplicaciones.

> [!NOTE]
> El código fuente del ejemplo está disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/samples/TextAnalytics).

Para más información técnica, consulte la [referencia de Text Analytics](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/textanalytics?view=azure-dotnet) del SDK para .NET.

## <a name="prerequisites"></a>Requisitos previos

* Cualquier edición de [Visual Studio 2017 o versiones posteriores]
* El [SDK de Text Analytics para .NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics)

[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

También necesitará el [punto de conexión y la clave de acceso](../How-tos/text-analytics-how-to-access-key.md) que se generaron automáticamente durante el registro.

## <a name="create-the-visual-studio-solution-and-install-the-sdk"></a>Creación de la solución de Visual Studio e instalación del SDK

1. Cree un proyecto de aplicación de consola (.NET Core). [Acceda a Visual Studio](https://visualstudio.microsoft.com/vs/).
1. Haga clic con el botón derecho en la solución y seleccione **Administrar paquetes NuGet para la solución**.
1. Seleccione la pestaña **Examinar**. Busque **Microsoft.Azure.CognitiveServices.Language.TextAnalytics**.

## <a name="authenticate-your-credentials"></a>Autenticación con sus credenciales

1. Agregue las siguientes instrucciones `using` al archivo de clase principal (que es Program.cs de forma predeterminada).

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Net.Http;
    using System.Threading;
    using System.Threading.Tasks;

    using Microsoft.Azure.CognitiveServices.Language.TextAnalytics;
    using Microsoft.Azure.CognitiveServices.Language.TextAnalytics.Models;
    using Microsoft.Rest;
    ```

2. Cree una clase `ApiKeyServiceClientCredentials` para almacenar las credenciales y agregarlas a cada solicitud.

    ```csharp
    /// <summary>
    /// Allows authentication to the API by using a basic apiKey mechanism
    /// </summary>
    class ApiKeyServiceClientCredentials : ServiceClientCredentials
    {
        private readonly string subscriptionKey;

        /// <summary>
        /// Creates a new instance of the ApiKeyServiceClientCredentails class
        /// </summary>
        /// <param name="subscriptionKey">The subscription key to authenticate and authorize as</param>
        public ApiKeyServiceClientCredentials(string subscriptionKey)
        {
            this.subscriptionKey = subscriptionKey;
        }

        /// <summary>
        /// Add the Basic Authentication Header to each outgoing request
        /// </summary>
        /// <param name="request">The outgoing request</param>
        /// <param name="cancellationToken">A token to cancel the operation</param>
        public override Task ProcessHttpRequestAsync(HttpRequestMessage request, CancellationToken cancellationToken)
        {
            if (request == null)
            {
                throw new ArgumentNullException("request");
            }

            request.Headers.Add("Ocp-Apim-Subscription-Key", this.subscriptionKey);
            return base.ProcessHttpRequestAsync(request, cancellationToken);
        }
    }
    ```

3. Actualice la clase `Program`. Agregue un miembro de constante para su clave de suscripción de Text Analytics y otro para el punto de conexión de servicio. Recuerde que debe usar la región de Azure correcta para su suscripción de Text Analytics.

    ```csharp
    private const string SubscriptionKey = "enter-your-key-here";

    private const string Endpoint = "enter-your-service-endpoint-here"; // For example: "https://westus.api.cognitive.microsoft.com";
    ```
> [!Tip]
> Para mejorar la seguridad de los secretos en los sistemas de producción, se recomienda usar [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/quick-create-net).
>

## <a name="create-a-text-analytics-client"></a>Creación de un cliente de Text Analytics

En la función `Main` de su proyecto, llame al método de ejemplo que quiere invocar. Pase los parámetros `Endpoint` y `SubscriptionKey` que ha definido.

```csharp
    public static void Main(string[] args)
    {
        var credentials = new ApiKeyServiceClientCredentials(SubscriptionKey);
        var client = new TextAnalyticsClient(credentials)
        {
            Endpoint = Endpoint
        };

        // Change the console encoding to display non-ASCII characters.
        Console.OutputEncoding = System.Text.Encoding.UTF8;
        SentimentAnalysisExample(client).Wait();
        // DetectLanguageExample(client).Wait();
        // RecognizeEntitiesExample(client).Wait();
        // KeyPhraseExtractionExample(client).Wait();
        Console.ReadLine();
    }
```

En las secciones siguientes se describe cómo llamar a cada característica de servicio.

## <a name="perform-sentiment-analysis"></a>Realización de un análisis de opiniones

1. Cree una función `SentimentAnalysisExample()` que tome el cliente que creó anteriormente.
2. Genere una lista de objetos `MultiLanguageInput` que contenga los documentos que quiere analizar.

    ```csharp
    public static async Task SentimentAnalysisExample(TextAnalyticsClient client)
    {
        // The documents to be analyzed. Add the language of the document. The ID can be any value.
        var inputDocuments = new MultiLanguageBatchInput(
            new List<MultiLanguageInput>
            {
                new MultiLanguageInput("en", "1", "I had the best day of my life."),
                new MultiLanguageInput("en", "2", "This was a waste of my time. The speaker put me to sleep."),
                new MultiLanguageInput("es", "3", "No tengo dinero ni nada que dar..."),
                new MultiLanguageInput("it", "4", "L'hotel veneziano era meraviglioso. È un bellissimo pezzo di architettura."),
            });
        //...
    }
    ```

3. En la misma función, llame a `client.SentimentAsync()` y obtenga el resultado. A continuación, recorra en iteración los resultados. Imprima la puntuación de identificador y opinión de cada documento. Una puntuación cercana a 0 indica una opinión negativa, mientras que una puntuación cercana a 1 indica una opinión positiva.

    ```csharp
    var result = await client.SentimentAsync(false, inputDocuments);

    // Printing sentiment results
    foreach (var document in result.Documents)
    {
        Console.WriteLine($"Document ID: {document.Id} , Sentiment Score: {document.Score:0.00}");
    }
    ```

### <a name="output"></a>Output

```console
Document ID: 1 , Sentiment Score: 0.87
Document ID: 2 , Sentiment Score: 0.11
Document ID: 3 , Sentiment Score: 0.44
Document ID: 4 , Sentiment Score: 1.00
```

## <a name="perform-language-detection"></a>Realización de la detección de idioma

1. Cree una función `DetectLanguageExample()` que tome el cliente que creó anteriormente.
2. Genere una lista de objetos `LanguageInput` que contenga los documentos.

    ```csharp
    public static async Task DetectLanguageExample(TextAnalyticsClient client)
    {

        // The documents to be submitted for language detection. The ID can be any value.
        var inputDocuments = new LanguageBatchInput(
                new List<LanguageInput>
                    {
                        new LanguageInput(id: "1", text: "This is a document written in English."),
                        new LanguageInput(id: "2", text: "Este es un document escrito en Español."),
                        new LanguageInput(id: "3", text: "这是一个用中文写的文件")
                    });
        //...
    }
    ```

3. En la misma función, llame a `client.DetectLanguageAsync()` y obtenga el resultado. A continuación, recorra en iteración los resultados. Imprima el identificador y el primer idioma devuelto de cada documento.

    ```csharp
    var langResults = await client.DetectLanguageAsync(false, inputDocuments);

    // Printing detected languages
    foreach (var document in langResults.Documents)
    {
        Console.WriteLine($"Document ID: {document.Id} , Language: {document.DetectedLanguages[0].Name}");
    }
    ```

### <a name="output"></a>Output

```console
===== LANGUAGE EXTRACTION ======
Document ID: 1 , Language: English
Document ID: 2 , Language: Spanish
Document ID: 3 , Language: Chinese_Simplified
```

## <a name="perform-entity-recognition"></a>Realización del reconocimiento de entidades

1. Cree una función `RecognizeEntitiesExample()` que tome el cliente que creó anteriormente.
2. Genere una lista de objetos `MultiLanguageBatchInput` que contenga los documentos.

    ```csharp
    public static async Task RecognizeEntitiesExample(TextAnalyticsClient client)
    {

        // The documents to be submitted for entity recognition. The ID can be any value.
        var inputDocuments = new MultiLanguageBatchInput(
            new List<MultiLanguageInput>
            {
                new MultiLanguageInput("en", "1", "Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800."),
                new MultiLanguageInput("es", "2", "La sede principal de Microsoft se encuentra en la ciudad de Redmond, a 21 kilómetros de Seattle.")
            });
        //...
    }
    ```

3. En la misma función, llame a `client.EntitiesAsync()` y obtenga el resultado. A continuación, recorra en iteración los resultados. Imprima el identificador. de cada documento. Para cada identidad detectada, imprima su nombre de Wikipedia, el tipo y los subtipos (si existen) y las ubicaciones del texto original.

    ```csharp
    var entitiesResult = await client.EntitiesAsync(false, inputDocuments);

    // Printing recognized entities
    foreach (var document in entitiesResult.Documents)
    {
        Console.WriteLine($"Document ID: {document.Id} ");

        Console.WriteLine("\t Entities:");
        foreach (var entity in document.Entities)
        {
            Console.WriteLine($"\t\tName: {entity.Name},\tType: {entity.Type ?? "N/A"},\tSub-Type: {entity.SubType ?? "N/A"}");
            foreach (var match in entity.Matches)
            {
                Console.WriteLine($"\t\t\tOffset: {match.Offset},\tLength: {match.Length},\tScore: {match.EntityTypeScore:F3}");
            }
        }
    }
    ```

### <a name="output"></a>Output

```console
Document ID: 1
         Entities:
                Name: Microsoft,        Type: Organization,     Sub-Type: N/A
                        Offset: 0,      Length: 9,      Score: 1.000
                Name: Bill Gates,       Type: Person,   Sub-Type: N/A
                        Offset: 25,     Length: 10,     Score: 1.000
                Name: Paul Allen,       Type: Person,   Sub-Type: N/A
                        Offset: 40,     Length: 10,     Score: 0.999
                Name: April 4,  Type: Other,    Sub-Type: N/A
                        Offset: 54,     Length: 7,      Score: 0.800
                Name: April 4, 1975,    Type: DateTime, Sub-Type: Date
                        Offset: 54,     Length: 13,     Score: 0.800
                Name: BASIC,    Type: Other,    Sub-Type: N/A
                        Offset: 89,     Length: 5,      Score: 0.800
                Name: Altair 8800,      Type: Other,    Sub-Type: N/A
                        Offset: 116,    Length: 11,     Score: 0.800
Document ID: 2
         Entities:
                Name: Microsoft,        Type: Organization,     Sub-Type: N/A
                        Offset: 21,     Length: 9,      Score: 1.000
                Name: Redmond (Washington),     Type: Location, Sub-Type: N/A
                        Offset: 60,     Length: 7,      Score: 0.991
                Name: 21 kilómetros,    Type: Quantity, Sub-Type: Dimension
                        Offset: 71,     Length: 13,     Score: 0.800
                Name: Seattle,  Type: Location, Sub-Type: N/A
                        Offset: 88,     Length: 7,      Score: 1.000
```

## <a name="perform-key-phrase-extraction"></a>Realización de la extracción de frases clave

1. Cree una función `KeyPhraseExtractionExample()` que tome el cliente que creó anteriormente.
2. Genere una lista de objetos `MultiLanguageBatchInput` que contenga los documentos.

    ```csharp
    public static async Task KeyPhraseExtractionExample(TextAnalyticsClient client)
    {
        var inputDocuments = new MultiLanguageBatchInput(
                    new List<MultiLanguageInput>
                    {
                        new MultiLanguageInput("ja", "1", "猫は幸せ"),
                        new MultiLanguageInput("de", "2", "Fahrt nach Stuttgart und dann zum Hotel zu Fu."),
                        new MultiLanguageInput("en", "3", "My cat might need to see a veterinarian."),
                        new MultiLanguageInput("es", "4", "A mi me encanta el fútbol!")
                    });
        //...
    }
    ```

3. En la misma función, llame a `client.KeyPhrasesAsync()` y obtenga el resultado. A continuación, recorra en iteración los resultados. Imprima el identificador y las frases clave detectadas de cada documento.

    ```csharp
    var kpResults = await client.KeyPhrasesAsync(false, inputDocuments);

    // Printing keyphrases
    foreach (var document in kpResults.Documents)
    {
        Console.WriteLine($"Document ID: {document.Id} ");

        Console.WriteLine("\t Key phrases:");

        foreach (string keyphrase in document.KeyPhrases)
        {
            Console.WriteLine($"\t\t{keyphrase}");
        }
    }
    ```

### <a name="output"></a>Output

```console
Document ID: 1
         Key phrases:
                幸せ
Document ID: 2
         Key phrases:
                Stuttgart
                Hotel
                Fahrt
                Fu
Document ID: 3
         Key phrases:
                cat
                veterinarian
Document ID: 4
         Key phrases:
                fútbol
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Text Analytics con Power BI](../tutorials/tutorial-power-bi-key-phrases.md)


* [Información general de Text Analytics](../overview.md)
* [Preguntas más frecuentes](../text-analytics-resource-faq.md)
