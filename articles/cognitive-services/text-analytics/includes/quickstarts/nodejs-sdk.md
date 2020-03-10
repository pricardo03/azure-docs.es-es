---
title: 'Inicio rápido: Biblioteca cliente v3 de Text Analytics para Node.js | Microsoft Docs'
description: Introducción a la biblioteca cliente v3 de Text Analytics para Node.js.
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 02/26/2020
ms.author: aahi
ms.reviewer: sumeh, assafi
ms.openlocfilehash: 9f4c5e8d491bcff420e354657ff7302a22cd7583
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/28/2020
ms.locfileid: "78155452"
---
<a name="HOLTop"></a>

#### <a name="version-30-preview"></a>[Versión 3.0 (versión preliminar)](#tab/version-3)

[Documentación de referencia de v3](https://aka.ms/azsdk-js-textanalytics-ref-docs) | [Código fuente de la biblioteca v3](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/textanalytics/ai-text-analytics) | [Paquete v3 (NPM)](https://www.npmjs.com/package/@azure/ai-text-analytics) | [Ejemplos de v3](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/textanalytics/ai-text-analytics/samples)


#### <a name="version-21"></a>[Versión 2.1](#tab/version-2)

[Documentación de referencia de v2](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics) | [Código fuente de la biblioteca v2](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/cognitiveServicesTextAnalytics) | [Paquete v2 (NPM)](https://www.npmjs.com/package/@azure/cognitiveservices-textanalytics) | [Ejemplos de v2](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/)

---

## <a name="prerequisites"></a>Prerrequisitos

* Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/)
* La versión actual de [Node.js](https://nodejs.org/).
* Una vez que tenga la suscripción de Azure, <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics"  title="cree un recurso de Text Analytics"  target="_blank">cree un recurso de Text Analytics <span class="docon docon-navigate-external x-hidden-focus"></span></a> en Azure Portal para obtener la clave y el punto de conexión. 
    * Necesitará la clave y el punto de conexión del recurso que cree para conectar la aplicación a API Text Analytics. Lo hará más adelante en el inicio rápido.
    * Puede usar el plan de tarifa gratuito para probar el servicio y actualizarlo más adelante a un nivel de pago para producción.

## <a name="setting-up"></a>Instalación

### <a name="create-a-new-nodejs-application"></a>Creación de una aplicación Node.js

> [!NOTE]
> Esta versión de la biblioteca cliente de Text Analytics también se puede ejecutar en el [explorador](https://github.com/Azure/azure-sdk-for-js/blob/master/documentation/Bundling.md).

En una ventana de la consola (como cmd, PowerShell o Bash), cree un directorio para la aplicación y vaya a él. 

```console
mkdir myapp && cd myapp
```

Ejecute el comando `npm init` para crear una aplicación de nodo con un archivo `package.json`. 

```console
npm init
```
### <a name="install-the-client-library"></a>Instalación de la biblioteca cliente

#### <a name="version-30-preview"></a>[Versión 3.0 (versión preliminar)](#tab/version-3)

Instale los paquetes NPM `@azure/ai-text-analytics`:

```console
npm install --save @azure/ai-text-analytics
```

#### <a name="version-21"></a>[Versión 2.1](#tab/version-2)

Instale los paquetes NPM `@azure/cognitiveservices-textanalytics`:

```console
npm install --save @azure/cognitiveservices-textanalytics
```

el archivo `package.json` de la aplicación se actualizará con las dependencias.

Cree un archivo llamado `index.js` y agregue las bibliotecas siguientes:

---

el archivo `package.json` de la aplicación se actualizará con las dependencias.
Cree un archivo llamado `index.js` y agregue las bibliotecas siguientes:

#### <a name="version-30-preview"></a>[Versión 3.0 (versión preliminar)](#tab/version-3)

```javascript
"use strict";

const { TextAnalyticsClient, TextAnalyticsApiKeyCredential } = require("@azure/ai-text-analytics");
```

#### <a name="version-21"></a>[Versión 2.1](#tab/version-2)

```javascript
"use strict";

const { TextAnalyticsClient, CognitiveServicesCredential } = require("@azure/cognitiveservices-textanalytics");
```
---

Cree variables para el punto de conexión y la clave de Azure del recurso.

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]

```javascript
const key = '<paste-your-text-analytics-key-here>';
const endpoint = `<paste-your-text-analytics-endpoint-here>`;
```

## <a name="object-model"></a>Modelo de objetos

El cliente de Text Analytics es un objeto `TextAnalyticsClient` que se autentica en Azure mediante la clave. El cliente proporciona varios métodos para analizar texto, como una sola cadena o un lote.

El texto se envía a la API como una lista de `documents`, que son objetos `dictionary` que contienen una combinación de atributos `id`, `text` y `language`, según el método utilizado. El atributo `text` almacena el texto que se va a analizar en el origen `language` y `id` puede ser cualquier valor. 

El objeto de respuesta es una lista que contiene la información de análisis para cada documento. 

## <a name="code-examples"></a>Ejemplos de código

* [Autenticación de cliente](#client-authentication)
* [Análisis de sentimiento](#sentiment-analysis) 
* [Detección de idioma](#language-detection)
* [Reconocimiento de entidades con nombre](#named-entity-recognition-ner)
* [Vinculación de entidad](#entity-linking)
* [Extracción de frases clave](#key-phrase-extraction)

## <a name="client-authentication"></a>Autenticación de clientes

#### <a name="version-30-preview"></a>[Versión 3.0 (versión preliminar)](#tab/version-3)

Cree un objeto `TextAnalyticsClient` con el punto de conexión y la clave como parámetros.

```javascript
const textAnalyticsClient = new TextAnalyticsClient(endpoint,  new TextAnalyticsApiKeyCredential(key));
```

#### <a name="version-21"></a>[Versión 2.1](#tab/version-2)

Cree un objeto [TextAnalyticsClient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics/textanalyticsclient) con `credentials` y `endpoint` como parámetro.

[!code-javascript[Authentication and client creation](~/cognitive-services-node-sdk-samples/Samples/textAnalytics.js?name=authentication)]

---

## <a name="sentiment-analysis"></a>análisis de opiniones

#### <a name="version-30-preview"></a>[Versión 3.0 (versión preliminar)](#tab/version-3)

Cree una matriz de cadenas que contenga el documento que quiere analizar. Llame al método `analyzeSentiment()` del cliente y obtenga el objeto `SentimentBatchResult` devuelto. Recorra en iteración la lista de resultados e imprima el identificador de cada documento y la opinión de nivel de documento con puntuaciones de confianza. El resultado contiene, para cada documento, un sentimiento de nivel de oración junto con las puntuaciones de desplazamientos, longitud y confianza.

```javascript
async function sentimentAnalysis(client){

    const sentimentInput = [
        "I had the best day of my life. I wish you were there with me."
    ];
    const sentimentResult = await client.analyzeSentiment(sentimentInput);

    sentimentResult.forEach(document => {
        console.log(`ID: ${document.id}`);
        console.log(`\tDocument Sentiment: ${document.sentiment}`);
        console.log(`\tDocument Scores:`);
        console.log(`\t\tPositive: ${document.sentimentScores.positive.toFixed(2)} \tNegative: ${document.sentimentScores.negative.toFixed(2)} \tNeutral: ${document.sentimentScores.neutral.toFixed(2)}`);
        console.log(`\tSentences Sentiment(${document.sentences.length}):`);
        document.sentences.forEach(sentence => {
            console.log(`\t\tSentence sentiment: ${sentence.sentiment}`)
            console.log(`\t\tSentences Scores:`);
            console.log(`\t\tPositive: ${sentence.sentimentScores.positive.toFixed(2)} \tNegative: ${sentence.sentimentScores.negative.toFixed(2)} \tNeutral: ${sentence.sentimentScores.neutral.toFixed(2)}`);
            console.log(`\t\tLength: ${sentence.length}, Offset: ${sentence.offset}`);
        })
    });
}
sentimentAnalysis(textAnalyticsClient)
```

Ejecute el código con `node index.js` en la ventana de la consola.

### <a name="output"></a>Output

```console
ID: 0
        Document Sentiment: positive
        Document Scores:
                Positive: 1.00  Negative: 0.00  Neutral: 0.00
        Sentences Sentiment(2):
                Sentence sentiment: positive
                Sentences Scores:
                Positive: 1.00  Negative: 0.00  Neutral: 0.00
                Length: 30, Offset: 0
                Sentence sentiment: neutral
                Sentences Scores:
                Positive: 0.21  Negative: 0.02  Neutral: 0.77
                Length: 30, Offset: 31
```

#### <a name="version-21"></a>[Versión 2.1](#tab/version-2)

Cree una lista de objetos de diccionario, que contenga los documentos que quiere analizar. Llame al método [sentiment()](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics/textanalyticsclient#sentiment-models-textanalyticsclientsentimentoptionalparams-) del cliente y obtenga el objeto [SentimentBatchResult](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics/sentimentbatchresult) devuelto. Itere la lista de resultados e imprima el identificador de cada documento y la puntuación de opiniones. Una puntuación cercana a 0 indica una opinión negativa, mientras que una puntuación cercana a 1 indica una opinión positiva.

[!code-javascript[Sentiment analysis](~/cognitive-services-node-sdk-samples/Samples/textAnalytics.js?name=sentimentAnalysis)]

Ejecute el código con `node index.js` en la ventana de la consola.

### <a name="output"></a>Output

```console
[ { id: '1', score: 0.87 } ]
[ { id: '2', score: 0.11 } ]
[ { id: '3', score: 0.44 } ]
[ { id: '4', score: 1.00 } ]
```

---

## <a name="language-detection"></a>Detección de idiomas

#### <a name="version-30-preview"></a>[Versión 3.0 (versión preliminar)](#tab/version-3)

Cree una matriz de cadenas que contenga el documento que quiere analizar. Llame al método `detectLanguage()` del cliente y obtenga el objeto `DetectLanguageResultCollection` devuelto. Luego, recorra en iteración los resultados e imprima el identificador de cada documento y el idioma principal respectivo.

```javascript
async function languageDetection(client) {

    const languageInputArray = [
        "Ce document est rédigé en Français."
    ];
    const languageResult = await client.detectLanguage(languageInputArray);

    languageResult.forEach(document => {
        console.log(`ID: ${document.id}`);
        console.log(`\tPrimary Language ${document.primaryLanguage.name}`)
    });
}
languageDetection(textAnalyticsClient);
```

Ejecute el código con `node index.js` en la ventana de la consola.

### <a name="output"></a>Output

```console
ID: 0
        Primary Language French
```

#### <a name="version-21"></a>[Versión 2.1](#tab/version-2)

Cree una lista de objetos de diccionario que contenga los documentos. Llame al método [detectLanguage()](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics/textanalyticsclient#detectlanguage-models-textanalyticsclientdetectlanguageoptionalparams-) del cliente y obtenga el objeto [LanguageBatchResult](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics/languagebatchresult) devuelto. A continuación, itere por los resultados e imprima el identificador de cada documento y el idioma.

[!code-javascript[Language detection](~/cognitive-services-node-sdk-samples/Samples/textAnalytics.js?name=languageDetection)]

Ejecute el código con `node index.js` en la ventana de la consola.

### <a name="output"></a>Output

```console
Document ID: 1 , Language: English
Document ID: 2 , Language: Spanish
Document ID: 3 , Language: Chinese_Simplified
```

---

## <a name="named-entity-recognition-ner"></a>Reconocimiento de entidades con nombre (NER)

#### <a name="version-30-preview"></a>[Versión 3.0 (versión preliminar)](#tab/version-3)

> [!NOTE]
> En la versión `3.0-preview`:
> * NER incluye métodos independientes para detectar información personal. 
> * La vinculación de entidad es una solicitud independiente distinta a la de NER.

Cree una matriz de cadenas que contenga el documento que quiere analizar. Llame al método `recognizeEntities()` del cliente y obtenga el objeto `RecognizeEntitiesResult`. Recorra en iteración la lista de resultados e imprima el nombre, el tipo, el subtipo, el desplazamiento, la longitud y la puntuación de la entidad.

```javascript
async function entityRecognition(client){

    const entityInputs = [
        "Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800",
        "La sede principal de Microsoft se encuentra en la ciudad de Redmond, a 21 kilómetros de Seattle."
    ];
    const entityResults = await client.recognizeEntities(entityInputs);

    entityResults.forEach(document => {
        console.log(`Document ID: ${document.id}`);
        document.entities.forEach(entity => {
            console.log(`\tName: ${entity.text} \tCategory: ${entity.category} \tSubcategory: ${entity.subCategory ? entity.subCategory : "N/A"}`);
            console.log(`\tOffset: ${entity.offset}, Length: ${entity.length} \tScore: ${entity.score}`);
        });
    });
}
entityRecognition(textAnalyticsClient);
```

Ejecute el código con `node index.js` en la ventana de la consola.

### <a name="output"></a>Output

```console
Document ID: 0
        Name: Microsoft         Category: Organization  Subcategory: N/A
        Offset: 0, Length: 9    Score: 1
        Name: Bill Gates        Category: Person        Subcategory: N/A
        Offset: 25, Length: 10  Score: 0.67
        Name: Paul Allen        Category: Person        Subcategory: N/A
        Offset: 40, Length: 10  Score: 0.81
        Name: April 4, 1975     Category: DateTime      Subcategory: Date
        Offset: 54, Length: 13  Score: 0.8
        Name: interpreters      Category: PersonType    Subcategory: N/A
        Offset: 95, Length: 12  Score: 0.6
        Name: 8800      Category: Quantity      Subcategory: Number
        Offset: 123, Length: 4  Score: 0.8
Document ID: 1
        Name: Microsoft         Category: Organization  Subcategory: N/A
        Offset: 21, Length: 9   Score: 0.96
        Name: Redmond   Category: Location      Subcategory: GPE
        Offset: 60, Length: 7   Score: 0.09
        Name: 21        Category: Quantity      Subcategory: Number
        Offset: 71, Length: 2   Score: 0.8
        Name: Seattle   Category: Location      Subcategory: GPE
```

## <a name="using-ner-to-detect-personal-information"></a>Uso de NER para detectar información personal

Cree una matriz de cadenas que contenga el documento que quiere analizar. Llame al método `recognizePiiEntities()` del cliente y obtenga el objeto `EntitiesBatchResult`. Recorra en iteración la lista de resultados e imprima el nombre, el tipo, el subtipo, el desplazamiento, la longitud y la puntuación de la entidad.


```javascript
async function entityPiiRecognition(client){

    const entityPiiInput = [
        "Insurance policy for SSN on file 123-12-1234 is here by approved."
    ];
    const entityPiiResults = await client.recognizePiiEntities(entityPiiInput);

    entityPiiResults.forEach(document => {
        console.log(`Document ID: ${document.id}`);
        document.entities.forEach(entity => {
            console.log(`\tName: ${entity.text} \tCategory: ${entity.category} \tSubcategory: ${entity.subCategory ? entity.subCategory : "N/A"}`);
            console.log(`\tOffset: ${entity.offset}, Length: ${entity.length} \tScore: ${entity.score}`);
        });
    });
}
entityPiiRecognition(textAnalyticsClient);
```

Ejecute el código con `node index.js` en la ventana de la consola.

### <a name="output"></a>Output

```console
Document ID: 0
        Name: 123-12-1234       Category: U.S. Social Security Number (SSN)     Subcategory: N/A
        Offset: 33, Length: 11  Score: 0.85
```

## <a name="entity-linking"></a>Entity Linking

Cree una matriz de cadenas que contenga el documento que quiere analizar. Llame al método `recognizeLinkedEntities()` del cliente y obtenga el objeto `RecognizeLinkedEntitiesResult`. Recorra en iteración la lista de resultados e imprima el nombre, el identificador, el origen de datos, la dirección URL y las coincidencias de la entidad. Cada objeto de la matriz `matches` contendrá el desplazamiento, la longitud y la puntuación de esa coincidencia.

```javascript
async function linkedEntityRecognition(client){

    const linkedEntityInput = [
        "Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800. During his career at Microsoft, Gates held the positions of chairman, chief executive officer, president and chief software architect, while also being the largest individual shareholder until May 2014."
    ];
    const entityResults = await client.recognizeLinkedEntities(linkedEntityInput);

    entityResults.forEach(document => {
        console.log(`Document ID: ${document.id}`);
        document.entities.forEach(entity => {
            console.log(`\tName: ${entity.name} \tID: ${entity.id} \tURL: ${entity.url} \tData Source: ${entity.dataSource}`);
            console.log(`\tMatches:`)
            entity.matches.forEach(match => {
                console.log(`\t\tText: ${match.text}`);
                console.log(`\t\tOffset: ${match.offset}, Length: ${match.length} \tScore: ${match.score.toFixed(3)}`);
            });
        });
    });
}
linkedEntityRecognition(textAnalyticsClient);
```

Ejecute el código con `node index.js` en la ventana de la consola.

### <a name="output"></a>Output

```console
Document ID: 0
        Name: Altair 8800       ID: Altair 8800         URL: https://en.wikipedia.org/wiki/Altair_8800  Data Source: Wikipedia
        Matches:
                Text: Altair 8800
                Offset: 116, Length: 11         Score: 0.777
        Name: Bill Gates        ID: Bill Gates  URL: https://en.wikipedia.org/wiki/Bill_Gates   Data Source: Wikipedia
        Matches:
                Text: Bill Gates
                Offset: 25, Length: 10  Score: 0.555
                Text: Gates
                Offset: 161, Length: 5  Score: 0.555
        Name: Paul Allen        ID: Paul Allen  URL: https://en.wikipedia.org/wiki/Paul_Allen   Data Source: Wikipedia
        Matches:
                Text: Paul Allen
                Offset: 40, Length: 10  Score: 0.533
        Name: Microsoft         ID: Microsoft   URL: https://en.wikipedia.org/wiki/Microsoft    Data Source: Wikipedia
        Matches:
                Text: Microsoft
                Offset: 0, Length: 9    Score: 0.469
                Text: Microsoft
                Offset: 150, Length: 9  Score: 0.469
        Name: April 4   ID: April 4     URL: https://en.wikipedia.org/wiki/April_4      Data Source: Wikipedia
        Matches:
                Text: April 4
                Offset: 54, Length: 7   Score: 0.248
        Name: BASIC     ID: BASIC       URL: https://en.wikipedia.org/wiki/BASIC        Data Source: Wikipedia
        Matches:
                Text: BASIC
                Offset: 89, Length: 5   Score: 0.281
```

#### <a name="version-21"></a>[Versión 2.1](#tab/version-2)

> [!NOTE]
> En la versión 2.1, la vinculación de entidad se incluye en la respuesta de NER.

Cree una lista de objetos que contenga los documentos. Llame al método [entities()](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics/textanalyticsclient#entities-models-textanalyticscliententitiesoptionalparams-) del cliente y obtenga el objeto [EntitiesBatchResult](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics/entitiesbatchresult). Itere la lista de resultados e imprima el identificador de cada documento. Para cada identidad detectada, imprima su nombre de Wikipedia, el tipo y los subtipos (si existen) y las ubicaciones del texto original.

[!code-javascript[Entity recognition](~/cognitive-services-node-sdk-samples/Samples/textAnalytics.js?name=entityRecognition)]

Ejecute el código con `node index.js` en la ventana de la consola.

### <a name="output"></a>Output

```console
Document ID: 1
    Name: Microsoft,        Type: Organization,     Sub-Type: N/A
    Offset: 0, Length: 9,   Score: 1.0
    Name: Bill Gates,       Type: Person,   Sub-Type: N/A
    Offset: 25, Length: 10, Score: 0.999847412109375
    Name: Paul Allen,       Type: Person,   Sub-Type: N/A
    Offset: 40, Length: 10, Score: 0.9988409876823425
    Name: April 4,  Type: Other,    Sub-Type: N/A
    Offset: 54, Length: 7,  Score: 0.8
    Name: April 4, 1975,    Type: DateTime, Sub-Type: Date
    Offset: 54, Length: 13, Score: 0.8
    Name: BASIC,    Type: Other,    Sub-Type: N/A
    Offset: 89, Length: 5,  Score: 0.8
    Name: Altair 8800,      Type: Other,    Sub-Type: N/A
    Offset: 116, Length: 11,        Score: 0.8

Document ID: 2
    Name: Microsoft,        Type: Organization,     Sub-Type: N/A
    Offset: 21, Length: 9,  Score: 0.999755859375
    Name: Redmond (Washington),     Type: Location, Sub-Type: N/A
    Offset: 60, Length: 7,  Score: 0.9911284446716309
    Name: 21 kilómetros,    Type: Quantity, Sub-Type: Dimension
    Offset: 71, Length: 13, Score: 0.8
    Name: Seattle,  Type: Location, Sub-Type: N/A
    Offset: 88, Length: 7,  Score: 0.9998779296875
```

---

## <a name="key-phrase-extraction"></a>Extracción de la frase clave

#### <a name="version-30-preview"></a>[Versión 3.0 (versión preliminar)](#tab/version-3)

Cree una matriz de cadenas que contenga el documento que quiere analizar. Llame al método `extractKeyPhrases()` del cliente y obtenga el objeto `ExtractKeyPhrasesResult` devuelto. Itere los resultados e imprima el identificador de cada documento y todas las frases clave detectadas.

```javascript
async function keyPhraseExtraction(client){

    const keyPhrasesInput = [
        "My cat might need to see a veterinarian.",
    ];
    const keyPhraseResult = await client.extractKeyPhrases(keyPhrasesInput);
    
    keyPhraseResult.forEach(document => {
        console.log(`ID: ${document.id}`);
        console.log(`\tDocument Key Phrases: ${document.keyPhrases}`);
    });
}
keyPhraseExtraction(textAnalyticsClient);
```

Ejecute el código con `node index.js` en la ventana de la consola.

### <a name="output"></a>Output

```console
ID: 0
        Document Key Phrases: cat,veterinarian
```

#### <a name="version-21"></a>[Versión 2.1](#tab/version-2)

Cree una lista de objetos que contenga los documentos. Llame al método de [keyPhrases()](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics/textanalyticsclient#keyphrases-models-textanalyticsclientkeyphrasesoptionalparams-) del cliente y obtenga el objeto [KeyPhraseBatchResult](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics/keyphrasebatchresult) devuelto. Itere los resultados e imprima el identificador de cada documento y todas las frases clave detectadas.

[!code-javascript[Key phrase extraction](~/cognitive-services-node-sdk-samples/Samples/textAnalytics.js?name=keyPhraseExtraction)]

Ejecute el código con `node index.js` en la ventana de la consola.

### <a name="output"></a>Output

```console
[
    { id: '1', keyPhrases: [ '幸せ' ] }
    { id: '2', keyPhrases: [ 'Stuttgart', "hotel", "Fahrt", "Fu" ] }
    { id: '3', keyPhrases: [ 'cat', 'veterinarian' ] }
    { id: '3', keyPhrases: [ 'fútbol' ] }
]
```

---

## <a name="run-the-application"></a>Ejecución de la aplicación

Ejecute la aplicación con el comando `node` en el archivo de inicio rápido.

```console
node index.js
```
