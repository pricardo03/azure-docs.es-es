---
author: aahill
ms.service: cognitive-services
ms.topic: include
ms.date: 10/28/2019
ms.author: aahi
ms.openlocfilehash: de6601a60bbb65787673d9d3bf5b1cbe9f46804a
ms.sourcegitcommit: d9ec6e731e7508d02850c9e05d98d26c4b6f13e6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2020
ms.locfileid: "76281447"
---
<a name="HOLTop"></a>

[Documentación de referencia](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/textanalytics?view=azure-dotnet-preview) | [Código fuente de la biblioteca](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Language.TextAnalytics) | [Paquete (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics/) | [Ejemplos](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples)

> [!NOTE]
> Por motivos de simplicidad, en el código de este artículo se usan los métodos sincrónicos del SDK de .NET de Text Analytics. En escenarios de producción, se recomienda usar los métodos asincrónicos por lotes para mayor rendimiento y escalabilidad. Por ejemplo, llamar a [SentimentBatchAsync()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.sentimentbatchasync?view=azure-dotnet&viewFallbackFrom=azure-dotnet-preview), en lugar de a [Sentiment()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.sentiment?view=azure-dotnet).

## <a name="prerequisites"></a>Prerequisites

* Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/)
* [IDE de Visual Studio](https://visualstudio.microsoft.com/vs/)

## <a name="setting-up"></a>Instalación

### <a name="create-a-text-analytics-azure-resource"></a>Creación de un recurso de Azure para Text Analytics

[!INCLUDE [text-analytics-resource-creation](../resource-creation.md)]

### <a name="create-a-new-net-core-application"></a>Creación de una aplicación de .NET Core

Utilice el IDE de Visual Studio para crear una aplicación de consola de .NET Core. Así se creará un proyecto "Hola mundo" sencillo, con un solo archivo de origen de C#: *program.cs*.

Instale la biblioteca cliente, para lo que debe hacer clic con el botón derecho en la solución en el **Explorador de soluciones** y seleccionar **Administrar paquetes NuGet**. En el administrador de paquetes que se abre, seleccione **Examinar** y busque `Microsoft.Azure.CognitiveServices.Language.TextAnalytics`. Haga clic en él y, después, en **Instalar**. También puede usar la [Consola del Administrador de paquetes](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-powershell#find-and-install-a-package).

Abra el archivo *program.cs* y agregue las siguientes `using` directivas:

[!code-csharp[Import directives](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=imports)]

En la clase `Program` de la aplicación, cree variables para el punto de conexión y la clave del recurso. 

[!INCLUDE [text-analytics-find-resource-information](../../find-azure-resource-info-v2.md)]

```csharp
private static readonly string key = "<replace-with-your-text-analytics-key-here>";
private static readonly string endpoint = "<replace-with-your-text-analytics-endpoint-here>";
```

Reemplace el método de `Main` de la aplicación. Más adelante definirá los métodos a los que llama aquí.

[!code-csharp[main method](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=main)]

## <a name="object-model"></a>Modelo de objetos

El cliente de Text Analytics es un objeto [TextAnalyticsClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclient?view=azure-dotnet) que se autentica en Azure mediante su clave y proporciona funciones para aceptar texto tanto en forma de cadenas individuales o como en forma de lote. Puede enviar texto a la API de forma sincrónica o asincrónica. El objeto de respuesta contendrá la información del análisis de todos los documentos que envíe. 

## <a name="code-examples"></a>Ejemplos de código

* [Autenticar el cliente](#authenticate-the-client)
* [Análisis de sentimiento](#sentiment-analysis)
* [Detección de idioma](#language-detection)
* [Reconocimiento de entidades](#entity-recognition)
* [Extracción de frases clave](#key-phrase-extraction)

## <a name="authenticate-the-client"></a>Autenticar el cliente

Cree una clase `ApiKeyServiceClientCredentials` para almacenar las credenciales y agregarlas a las solicitudes del cliente. Dentro de ella, cree una invalidación para `ProcessHttpRequestAsync()` que agregue su clave al encabezado `Ocp-Apim-Subscription-Key`.

[!code-csharp[Client class](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=clientClass)]

Cree un método para crear una instancia del objeto [TextAnalyticsClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclient?view=azure-dotnet) con el punto de conexión y un objeto `ApiKeyServiceClientCredentials` que contiene la clave.

[!code-csharp[Client authentication](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=authentication)]

En el método de `main()` del programa, llame al método de autenticación para crear una instancia del cliente.

## <a name="sentiment-analysis"></a>análisis de opiniones

Cree una función denominada `SentimentAnalysisExample()` que tome el cliente que creó anteriormente y llame a su función [Sentiment()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.sentiment?view=azure-dotnet). El objeto [SentimentResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.models.sentimentresult?view=azure-dotnet) que se devuelve contendrá la opinión `Score` si la operación se ha realizado correctamente, y `errorMessage` si no ha sido así. 

Una puntuación cercana a 0 indica una opinión negativa, mientras que una puntuación cercana a 1 indica una opinión positiva.

[!code-csharp[Sentiment analysis](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=sentiment)]

### <a name="output"></a>Output

```console
Sentiment Score: 0.87
```

## <a name="language-detection"></a>Detección de idiomas

Cree una función denominada `languageDetectionExample()` que tome el cliente que creó anteriormente y llame a su función [DetectLanguage()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.detectlanguage?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_TextAnalytics_TextAnalyticsClientExtensions_DetectLanguage_Microsoft_Azure_CognitiveServices_Language_TextAnalytics_ITextAnalyticsClient_System_String_System_String_System_Nullable_System_Boolean__System_Threading_CancellationToken_). El objeto [LanguageResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.models.languageresult?view=azure-dotnet) devuelto contendrá la lista de idiomas detectados en `DetectedLanguages` si la operación se ha realizado correctamente, y `errorMessage` si no ha sido así.  Imprima el primer idioma devuelto.

> [!Tip]
> En algunos casos, puede ser difícil eliminar la ambigüedad de los idiomas en función de la entrada. Puede usar el parámetro `countryHint` para especificar un código de país de dos letras. De forma predeterminada, la API usa "US" como valor de countryHint predeterminado; para quitar este comportamiento, puede restablecer este parámetro y configurarlo como una cadena `countryHint = ""` vacía.

[!code-csharp[Language Detection example](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=languageDetection)]

### <a name="output"></a>Output

```console
Language: English
```

## <a name="entity-recognition"></a>Reconocimiento de entidades

Cree una función denominada `RecognizeEntitiesExample()` que tome el cliente que creó anteriormente y llame a su función [Entities()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.entities?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_TextAnalytics_TextAnalyticsClientExtensions_Entities_Microsoft_Azure_CognitiveServices_Language_TextAnalytics_ITextAnalyticsClient_System_String_System_String_System_Nullable_System_Boolean__System_Threading_CancellationToken_). Recorra en iteración los resultados. El objeto [EntitiesResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.models.entitiesresult?view=azure-dotnet) devuelto contendrá la lista de entidades detectadas en `Entities` si la operación se ha realizado correctamente, y `errorMessage` si no ha sido así. Para cada identidad detectada, imprima su tipo, subtipo y nombre de Wikipedia (si existen) y las ubicaciones del texto original.

[!code-csharp[Entity Recognition example](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=entityRecognition)]


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

## <a name="key-phrase-extraction"></a>Extracción de la frase clave

Cree una función denominada `KeyPhraseExtractionExample()` que tome el cliente que creó anteriormente y llame a su función [KeyPhrases()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.keyphrases?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_TextAnalytics_TextAnalyticsClientExtensions_KeyPhrases_Microsoft_Azure_CognitiveServices_Language_TextAnalytics_ITextAnalyticsClient_System_String_System_String_System_Nullable_System_Boolean__System_Threading_CancellationToken_). El resultado contendrá la lista de frases clave detectadas en `KeyPhrases` si la operación ha tenido éxito, o `errorMessage` si no. Imprima las frases clave detectadas.

[!code-csharp[Key phrase extraction example](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=keyPhraseExtraction)]


### <a name="output"></a>Output

```console
Key phrases:
    cat
    veterinarian
```
