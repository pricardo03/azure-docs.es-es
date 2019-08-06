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
ms.date: 07/30/2019
ms.author: assafi
ms.openlocfilehash: 6bd3907392dad626c1eeb1823c929f1a35d544dd
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2019
ms.locfileid: "68697669"
---
# <a name="quickstart-use-the-net-sdk-and-c-to-call-the-text-analytics-service"></a>Inicio rápido: Uso de .NET y C# para llamar al servicio Text Analytics
<a name="HOLTop"></a>

Este inicio rápido le ayuda a empezar a usar Azure SDK para .NET y C# para analizar el lenguaje. Aunque la API REST [Text Analytics](//go.microsoft.com/fwlink/?LinkID=759711) es compatible con la mayoría de los lenguajes de programación, el SDK proporciona una forma sencilla de integrar el servicio en sus aplicaciones.

> [!NOTE]
> Por motivos de simplicidad, en el código de demostración de este artículo se usan los métodos sincrónicos del SDK de .NET de Text Analytics. De todas formas, para escenarios de producción, se recomienda usar métodos asincrónicos por lotes en sus propias aplicaciones para mantener su escalabilidad y capacidad de respuesta. Por ejemplo, podría usar `SentimentBatchAsync` en lugar de `Sentiment`.
>
> Se puede encontrar una versión asincrónica por lotes de este ejemplo en [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/samples/TextAnalytics).

Para más información técnica, consulte la [referencia de Text Analytics](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/textanalytics?view=azure-dotnet) del SDK para .NET.

## <a name="prerequisites"></a>Requisitos previos

* Cualquier edición de Visual Studio 2017 o versiones posteriores.
* El [SDK de Text Analytics para .NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics)

[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

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
    class ApiKeyServiceClientCredentials : ServiceClientCredentials
    {
        private readonly string apiKey;

        public ApiKeyServiceClientCredentials(string apiKey)
        {
            this.apiKey = apiKey;
        }

        public override Task ProcessHttpRequestAsync(HttpRequestMessage request, CancellationToken cancellationToken)
        {
            if (request == null)
            {
                throw new ArgumentNullException("request");
            }
            request.Headers.Add("Ocp-Apim-Subscription-Key", this.apiKey);
            return base.ProcessHttpRequestAsync(request, cancellationToken);
        }
    }
    ```

3. Actualice la clase `Program`. Agregue un miembro de constante para su clave de API de Text Analytics y otro para el punto de conexión de servicio. Recuerde que debe usar la ubicación de Azure correcta para el recurso de Text Analytics.

    ```csharp
    //Enter your Text Analytics (TA) API Key (available in Azure Portal -> your TA resource -> Keys)
    private const string ApiKey = "enter-your-textanalytics-api-key-here";
    //You can get the resource location from Azure Portal -> your TA resource -> Overview
    private const string Endpoint = "enter-your-service-endpoint-here"; // For example: "https://<your-location>.api.cognitive.microsoft.com";
    ```

> [!Tip]
> Para mejorar la seguridad de los secretos en los sistemas de producción, se recomienda usar [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/quick-create-net).
>

## <a name="create-a-text-analytics-client"></a>Creación de un cliente de Text Analytics

En la función `Main` de su proyecto, llame al método de ejemplo que quiere invocar. Pase los parámetros `Endpoint` y `ApiKey` que ha definido.

```csharp
    public static void Main(string[] args)
    {
        var credentials = new ApiKeyServiceClientCredentials(ApiKey);
        var client = new TextAnalyticsClient(credentials)
        {
            Endpoint = Endpoint
        };

        // Change the console encoding to display non-ASCII characters.
        Console.OutputEncoding = System.Text.Encoding.UTF8;
        SentimentAnalysisExample(client);
        // DetectLanguageExample(client);
        // RecognizeEntitiesExample(client);
        // KeyPhraseExtractionExample(client);
        Console.ReadLine();
    }
```

En las secciones siguientes se describe cómo llamar a cada característica de servicio.

## <a name="perform-sentiment-analysis"></a>Realización de un análisis de opiniones

1. Cree una función `SentimentAnalysisExample()` que tome el cliente que creó anteriormente.
2. En la misma función, llame a `client.Sentiment()` y obtenga el resultado. El resultado contendrá la opinión `Score` si la operación ha tenido éxito, o `errorMessage` si no. Una puntuación cercana a 0 indica una opinión negativa, mientras que una puntuación cercana a 1 indica una opinión positiva.

    ```csharp
    var result = client.Sentiment("I had the best day of my life.", "en");

    // Printing sentiment results
    Console.WriteLine($"Sentiment Score: {result.Score:0.00}");
    ```

### <a name="output"></a>Output

```console
Sentiment Score: 0.87
```

## <a name="perform-language-detection"></a>Realización de la detección de idioma

1. Cree una función `DetectLanguageExample()` que tome el cliente que creó anteriormente.
2. En la misma función, llame a `client.DetectLanguage()` y obtenga el resultado. El resultado contendrá la lista de idiomas detectados en `DetectedLanguages` si la operación ha tenido éxito, o `errorMessage` si no. A continuación, imprima el primer idioma devuelto.

    ```csharp
    var result = client.DetectLanguage("This is a document written in English.");

    // Printing detected languages
    Console.WriteLine($"Language: {result.DetectedLanguages[0].Name}");
    ```

> [!Tip]
> En algunos casos, puede ser difícil eliminar la ambigüedad de los idiomas en función de la entrada. Puede usar el parámetro `countryHint` para especificar un código de país de dos letras. De forma predeterminada, la API usa "US" como valor de countryHint predeterminado; para quitar este comportamiento, puede restablecer este parámetro y configurarlo como una cadena `countryHint = ""` vacía.

### <a name="output"></a>Output

```console
Language: English
```

## <a name="perform-entity-recognition"></a>Realización del reconocimiento de entidades

1. Cree una función `RecognizeEntitiesExample()` que tome el cliente que creó anteriormente.
2. En la misma función, llame a `client.Entities()` y obtenga el resultado. A continuación, recorra en iteración los resultados. El resultado contendrá la lista de entidades detectadas `Entities` si la operación ha tenido éxito, o `errorMessage` si no. Para cada identidad detectada, imprima su tipo, subtipo y nombre de Wikipedia (si existen) y las ubicaciones del texto original.

    ```csharp
    var result = client.Entities("Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800.");

    // Printing recognized entities
    Console.WriteLine("Entities:");
    foreach (var entity in result.Entities)
    {
        Console.WriteLine($"\tName: {entity.Name},\tType: {entity.Type ?? "N/A"},\tSub-Type: {entity.SubType ?? "N/A"}");
        foreach (var match in entity.Matches)
        {
            Console.WriteLine($"\t\tOffset: {match.Offset},\tLength: {match.Length},\tScore: {match.EntityTypeScore:F3}");
        }
    }
    ```

### <a name="output"></a>Output

```console
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
```

## <a name="perform-key-phrase-extraction"></a>Realización de la extracción de frases clave

1. Cree una función `KeyPhraseExtractionExample()` que tome el cliente que creó anteriormente.
2. En la misma función, llame a `client.KeyPhrases()` y obtenga el resultado. El resultado contendrá la lista de frases clave detectadas en `KeyPhrases` si la operación ha tenido éxito, o `errorMessage` si no. A continuación, imprima las frases clave detectadas.

    ```csharp
    var result = client.KeyPhrases("My cat might need to see a veterinarian.");

    // Printing key phrases
    Console.WriteLine("Key phrases:");

    foreach (string keyphrase in result.KeyPhrases)
    {
        Console.WriteLine($"\t{keyphrase}");
    }
    ```

### <a name="output"></a>Output

```console
Key phrases:
    cat
    veterinarian
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Text Analytics con Power BI](../tutorials/tutorial-power-bi-key-phrases.md)


* [Información general de Text Analytics](../overview.md)
* [Preguntas más frecuentes](../text-analytics-resource-faq.md)
