---
title: 'Inicio rápido: Biblioteca cliente v3 de Text Analytics para Java | Microsoft Docs'
description: Introducción a la biblioteca cliente v3 de Text Analytics para Java.
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 02/11/2020
ms.author: aahi
ms.reviewer: tasharm, assafi
ms.openlocfilehash: eeaef4a9970609c43c03cd784436796a6f8af174
ms.sourcegitcommit: 2823677304c10763c21bcb047df90f86339e476a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2020
ms.locfileid: "77211411"
---
<a name="HOLTop"></a>

[Documentación de referencia](https://aka.ms/azsdk-java-textanalytics-ref-docs) | [Código fuente de la biblioteca](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/textanalytics/azure-ai-textanalytics) | [Paquete (Maven)](https://oss.sonatype.org/#nexus-search;quick~com.azure) | [Ejemplos](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/textanalytics/azure-ai-textanalytics/src/samples/java/com/azure/ai/textanalytics)

## <a name="prerequisites"></a>Prerrequisitos

* Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/)
* [Kit de desarrollo de Java](https://www.oracle.com/technetwork/java/javase/downloads/index.html) (JDK), versión 8 o posterior


[!INCLUDE [text-analytics-resource-creation](resource-creation.md)]

## <a name="setting-up"></a>Instalación

### <a name="create-a-new-maven-project"></a>Creación de un nuevo proyecto de Maven

Agregue la siguiente dependencia de Text Analytics al proyecto. Esta versión de la dependencia usa la versión `3.0-preview` de Text Analytics API. 

```xml
<dependencies>
    <dependency>
        <groupId>com.azure</groupId>
        <artifactId>azure-ai-textanalytics</artifactId>
        <version>1.0.0-beta.2</version>
    </dependency>
</dependencies>
```

Cree un archivo Java en el siguiente directorio: `\src\main\java`.

Abra el archivo Java y agregue las siguientes instrucciones `import`:

```java
import com.azure.ai.textanalytics.models.*;
import com.azure.ai.textanalytics.TextAnalyticsClientBuilder;
import com.azure.ai.textanalytics.TextAnalyticsClient;
```

En el archivo Java, agregue una nueva clase y agregue la clave y el punto de conexión del recurso de Azure, tal y como se muestra a continuación.

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]

```java
public class TextAnalyticsSamples {
    private static String KEY = "<replace-with-your-text-analytics-key-here>";
    private static String ENDPOINT = "<replace-with-your-text-analytics-endpoint-here>";
}
```

Agregue el siguiente método principal a la clase. Más adelante definirá los métodos a los que llama aquí.

```java
public static void main(String[] args) {

    TextAnalyticsClient client = authenticateClient(KEY, ENDPOINT);

    sentimentAnalysisExample(client);
    detectLanguageExample(client);
    recognizeEntitiesExample(client);
    recognizePIIEntitiesExample(client);
    recognizeLinkedEntitiesExample(client);
    extractKeyPhrasesExample(client);
}
```

## <a name="object-model"></a>Modelo de objetos

El cliente de Text Analytics es un objeto `TextAnalyticsClient` que se autentica en Azure mediante su clave y que proporciona funciones para aceptar texto como cadenas individuales o como un lote. Puede enviar texto a la API de forma sincrónica o asincrónica. El objeto de respuesta contendrá la información del análisis de todos los documentos que envíe. 

## <a name="code-examples"></a>Ejemplos de código

* [Autenticar el cliente](#authenticate-the-client)
* [Análisis de sentimiento](#sentiment-analysis) 
* [Detección de idioma](#language-detection)
* [Reconocimiento de entidades con nombre](#named-entity-recognition-ner) 
* [Vinculación de entidad](#entity-linking)
* [Extracción de frases clave](#key-phrase-extraction)

## <a name="authenticate-the-client"></a>Autenticar el cliente

Cree un método para crear una instancia del objeto `TextAnalyticsClient` con los objetos `KEY` y `ENDPOINT` creados anteriormente.

```java
static TextAnalyticsClient authenticateClient(String key, String endpoint) {
    return new TextAnalyticsClientBuilder()
        .apiKey(new TextAnalyticsApiKeyCredential(key))
        .endpoint(endpoint)
        .buildClient();
}
```

En el método de `main()` del programa, llame al método de autenticación para crear una instancia del cliente.

## <a name="sentiment-analysis"></a>análisis de opiniones

Cree una función denominada `sentimentAnalysisExample()` que tome el cliente que creó anteriormente y llame a su función `analyzeSentiment()`. El objeto `AnalyzeSentimentResult` devuelto contendrá `documentSentiment` y `sentenceSentiments` si la operación se realiza correctamente, o `errorMessage` si no. 

```java
static void sentimentAnalysisExample(TextAnalyticsClient client)
{
    // The text that need be analyzed.
    String text = "I had the best day of my life. I wish you were there with me.";

    DocumentSentiment documentSentiment = client.analyzeSentiment(text);
        System.out.printf(
            "Recognized document sentiment: %s, positive score: %.2f, neutral score: %.2f, negative score: %.2f.%n",
            documentSentiment.getSentiment(),
            documentSentiment.getSentimentScores().getPositive(),
            documentSentiment.getSentimentScores().getNeutral(),
            documentSentiment.getSentimentScores().getNegative());

        for (SentenceSentiment sentenceSentiment : documentSentiment.getSentences()) {
            System.out.printf(
                "Recognized sentence sentiment: %s, positive score: %.2f, neutral score: %.2f, negative score: %.2f.%n",
                sentenceSentiment.getSentiment(),
                sentenceSentiment.getSentimentScores().getPositive(),
                sentenceSentiment.getSentimentScores().getNeutral(),
                sentenceSentiment.getSentimentScores().getNegative());
        }
}
```

### <a name="output"></a>Output

```console
Recognized document sentiment: positive, Positive Score: 1.00, Neutral Score: 0.00, Negative Score: 0.00.
Recognized sentence sentiment: positive, positive score: 1.00, neutral score: 0.00, negative score: 0.00.
Recognized sentence sentiment: neutral, positive score: 0.21, neutral score: 0.77, negative score: 0.02.
```
## <a name="language-detection"></a>Detección de idiomas

Cree una función denominada `detectLanguageExample()` que tome el cliente que creó anteriormente y llame a su función `detectLanguage()`. El objeto `DetectLanguageResult` devuelto contendrá un idioma principal detectado, una lista de otros idiomas detectados si la operación se realiza correctamente o `errorMessage` si no.

> [!Tip]
> En algunos casos, puede ser difícil eliminar la ambigüedad de los idiomas en función de la entrada. Puede usar el parámetro `countryHint` para especificar un código de país de dos letras. De forma predeterminada, la API usa "US" como valor predeterminado de countryHint; para eliminar este comportamiento, puede restablecer este parámetro y configurarlo como una cadena `countryHint = ""` vacía. Para establecer un valor predeterminado diferente, establezca la propiedad `TextAnalyticsClientOptions.DefaultCountryHint` y pásela durante la inicialización del cliente.

```java
static void detectLanguageExample(TextAnalyticsClient client)
{
    // The text that need be analyzed.
    String text = "Ce document est rédigé en Français.";

    DetectedLanguage detectedLanguage = client.detectLanguage(text);
    System.out.printf("Detected primary language: %s, ISO 6391 name: %s, score: %.2f.%n",
        detectedLanguage.getName(),
        detectedLanguage.getIso6391Name(),
        detectedLanguage.getScore());
}
```

### <a name="output"></a>Output

```console
Detected primary language: French, ISO 6391 name: fr, score: 1.00.
```
## <a name="named-entity-recognition-ner"></a>Reconocimiento de entidades con nombre (NER)

> [!NOTE]
> En la versión `3.0-preview`:
> * NER incluye métodos independientes para detectar información personal. 
> * La vinculación de entidad es una solicitud independiente distinta a la de NER.

Cree una función denominada `recognizeEntitiesExample()` que tome el cliente que creó anteriormente y llame a su función `recognizeEntities()`. El objeto devuelto `RecognizeEntitiesResult` contendrá una lista de `NamedEntity` si la operación se realiza correctamente o `errorMessage` si no.

```java
static void recognizeEntitiesExample(TextAnalyticsClient client)
{
    // The text that need be analyzed.
    String text = "I had a wonderful trip to Seattle last week.";

    for (CategorizedEntity entity : client.recognizeEntities(text)) {
        System.out.printf(
            "Recognized entity: %s, entity category: %s, entity sub-category: %s, offset: %s, length: %s, score: %.2f.%n",
            entity.getText(),
            entity.getCategory(),
            entity.getSubCategory() == null || entity.getSubCategory().isEmpty() ? "N/A" : entity.getSubCategory(),
            entity.getOffset(),
            entity.getLength(),
            entity.getScore());
    }
}
```

### <a name="output"></a>Output

```console
Recognized entity: Seattle, entity category: Location, entity sub-category: GPE, offset: 26, length: 7, score: 0.92.
Recognized entity: last week, entity category: DateTime, entity sub-category: DateRange, offset: 34, length: 9, score: 0.80.
```

## <a name="using-ner-to-recognize-personal-information"></a>Uso de NER para reconocer información personal

Cree una función denominada `recognizePIIEntitiesExample()` que tome el cliente que creó anteriormente y llame a su función `recognizePiiEntities()`. El objeto devuelto `RecognizePiiEntitiesResult` contendrá una lista de `NamedEntity` si la operación se realiza correctamente o `errorMessage` si no. 

```java
static void recognizePIIEntitiesExample(TextAnalyticsClient client)
{
    // The text that need be analyzed.
    String text = "Insurance policy for SSN on file 123-12-1234 is here by approved.";

    for (PiiEntity entity : client.recognizePiiEntities(text)) {
        System.out.printf(
            "Recognized personal identifiable information entity: %s, entity category: %s, entity sub-category: %s, offset: %s, length: %s, score: %.2f.%n",
            entity.getText(),
            entity.getCategory(),
            entity.getSubCategory() == null || entity.getSubCategory().isEmpty() ? "N/A" : entity.getSubCategory(),
            entity.getOffset(),
            entity.getLength(),
            entity.getScore());
    }
}
```

### <a name="output"></a>Output

```console
Recognized personal identifiable information entity: 123-12-1234, entity category: U.S. Social Security Number (SSN), entity sub-category: N/A, offset: 33, length: 11, score: 0.85.
```

## <a name="entity-linking"></a>Vinculación de entidad

Cree una función denominada `recognizeLinkedEntitiesExample()` que tome el cliente que creó anteriormente y llame a su función `recognizeLinkedEntities()`. El objeto devuelto `RecognizeLinkedEntitiesResult` contendrá una lista de `LinkedEntity` si la operación se realiza correctamente o `errorMessage` si no. Puesto que las entidades vinculadas se identifican de forma única, las apariciones de la misma entidad se agrupan bajo un objeto `LinkedEntity` como una lista de objetos `LinkedEntityMatch`.

```java
static void recognizeLinkedEntitiesExample(TextAnalyticsClient client)
{
    // The text that need be analyzed.
    String text = "Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, " +
            "to develop and sell BASIC interpreters for the Altair 8800. " +
            "During his career at Microsoft, Gates held the positions of chairman, " +
            "chief executive officer, president and chief software architect, " +
            "while also being the largest individual shareholder until May 2014.";

    System.out.printf("Linked Entities:%n");
    for (LinkedEntity linkedEntity : client.recognizeLinkedEntities(text)) {
        System.out.printf("Name: %s, ID: %s, URL: %s, Data Source: %s.%n",
                linkedEntity.getName(),
                linkedEntity.getId(),
                linkedEntity.getUrl(),
                linkedEntity.getDataSource());
        System.out.printf("Matches:%n");
        for (LinkedEntityMatch linkedEntityMatch : linkedEntity.getLinkedEntityMatches()) {
            System.out.printf("Text: %s, Offset: %s, Length: %s, Score: %.2f.%n",
                    linkedEntityMatch.getText(),
                    linkedEntityMatch.getOffset(),
                    linkedEntityMatch.getLength(),
                    linkedEntityMatch.getScore());
        }
    }
}
```

### <a name="output"></a>Output

```console
Linked Entities:
Name: Altair 8800, ID: Altair 8800, URL: https://en.wikipedia.org/wiki/Altair_8800, Data Source: Wikipedia.
Matches:
Text: Altair 8800, Offset: 11, Length: 116, Score: 0.78.
Name: Bill Gates, ID: Bill Gates, URL: https://en.wikipedia.org/wiki/Bill_Gates, Data Source: Wikipedia.
Matches:
Text: Bill Gates, Offset: 10, Length: 25, Score: 0.55.
Text: Gates, Offset: 5, Length: 161, Score: 0.55.
Name: Paul Allen, ID: Paul Allen, URL: https://en.wikipedia.org/wiki/Paul_Allen, Data Source: Wikipedia.
Matches:
Text: Paul Allen, Offset: 10, Length: 40, Score: 0.53.
Name: Microsoft, ID: Microsoft, URL: https://en.wikipedia.org/wiki/Microsoft, Data Source: Wikipedia.
Matches:
Text: Microsoft, Offset: 9, Length: 0, Score: 0.47.
Text: Microsoft, Offset: 9, Length: 150, Score: 0.47.
Name: April 4, ID: April 4, URL: https://en.wikipedia.org/wiki/April_4, Data Source: Wikipedia.
Matches:
Text: April 4, Offset: 7, Length: 54, Score: 0.25.
Name: BASIC, ID: BASIC, URL: https://en.wikipedia.org/wiki/BASIC, Data Source: Wikipedia.
Matches:
Text: BASIC, Offset: 5, Length: 89, Score: 0.28.
```
## <a name="key-phrase-extraction"></a>Extracción de la frase clave

Cree una función denominada `extractKeyPhrasesExample()` que tome el cliente que creó anteriormente y llame a su función `extractKeyPhrases()`. El objeto `ExtractKeyPhraseResult` devuelto contendrá una lista de frases clave si la operación se realiza correctamente o `errorMessage` si no.

```java
static void extractKeyPhrasesExample(TextAnalyticsClient client)
{
    // The text that need be analyzed.
    String text = "My cat might need to see a veterinarian.";

    System.out.printf("Recognized phrases: %n");
    for (String keyPhrase : client.extractKeyPhrases(text)) {
        System.out.printf("%s%n", keyPhrase);
    }
}
```

### <a name="output"></a>Output

```console
Recognized phrases: 
cat
veterinarian
```
