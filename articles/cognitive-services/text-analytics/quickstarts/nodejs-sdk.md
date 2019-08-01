---
title: 'Inicio rápido: Uso de Node.js para llamar a Text Analytics API'
titleSuffix: Azure Cognitive Services
description: Obtenga información y ejemplos de código que le ayuden a empezar a usar rápidamente Text Analytics API.
services: cognitive-services
author: raymondl
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 07/30/2019
ms.author: shthowse
ms.openlocfilehash: 9b8a713d58d5753e04de050e0bc961b5e8388123
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2019
ms.locfileid: "68697474"
---
# <a name="quickstart-using-nodejs-to-call-the-text-analytics-cognitive-service"></a>Inicio rápido: Uso de Node.js para llamar a Text Analytics de Cognitive Services
<a name="HOLTop"></a>

Use esta guía de inicio rápido para empezar a analizar el lenguaje con el SDK de Text Analytics para Node.js. Aunque la API de REST [Text Analytics](//go.microsoft.com/fwlink/?LinkID=759711) es compatible con la mayoría de los lenguajes de programación, el SDK proporciona una forma sencilla de integrar el servicio en sus aplicaciones. El código fuente de este ejemplo está disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/textAnalytics.js).

Consulte las [definiciones de API](//go.microsoft.com/fwlink/?LinkID=759346) para obtener la documentación técnica de las API.

## <a name="prerequisites"></a>Requisitos previos

* [Node.js](https://nodejs.org/)
* El [SDK para Node.js](https://www.npmjs.com/package/azure-cognitiveservices-textanalytics) de Text Analytics. Puede instalar el SDK con:

    `npm install azure-cognitiveservices-textanalytics`

[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

También debe tener la [clave de acceso y punto de conexión](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) que se generó automáticamente durante el registro.

## <a name="create-a-nodejs-application-and-install-the-sdk"></a>Creación de una aplicación Node.js e instalación del SDK

Después de instalar Node.js, cree un proyecto de nodo. Cree un directorio para la aplicación y vaya a su directorio.

```mkdir myapp && cd myapp```

Ejecute ```npm init``` para crear una aplicación de nodo con un archivo package.json. Instale los paquetes `ms-rest-azure` y `azure-cognitiveservices-textanalytics` de NPM:

```npm install azure-cognitiveservices-textanalytics ms-rest-azure```

El archivo package.json de la aplicación se actualizará con las dependencias.

## <a name="authenticate-your-credentials"></a>Autenticación con sus credenciales

Cree un nuevo archivo `index.js` en la raíz del proyecto e importe las bibliotecas instaladas.

```javascript
const CognitiveServicesCredentials = require("ms-rest-azure").CognitiveServicesCredentials;
const TextAnalyticsAPIClient = require("azure-cognitiveservices-textanalytics");
```

Cree una variable para la clave de suscripción de Text Analytics.

```javascript
let credentials = new CognitiveServicesCredentials(
    "enter-your-key-here"
);
```

> [!Tip]
> Para una implementación segura de secretos en sistemas de producción, se recomienda usar [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/quick-create-net).
>

## <a name="create-a-text-analytics-client"></a>Creación de un cliente de Text Analytics

Cree un objeto `TextAnalyticsClient` con `credentials` como parámetro. Use la región de Azure correcta para su suscripción de Text Analytics.

```javascript
//Replace 'westus' with the correct region for your Text Analytics subscription
let client = new TextAnalyticsAPIClient(
    credentials,
    "https://westus.api.cognitive.microsoft.com/"
);
```

## <a name="sentiment-analysis"></a>análisis de opiniones

Cree una lista de objetos que contenga los documentos que desea analizar. La carga a la API consta de una lista de `documents`, que contienen un atributo `id`, `language` y `text`. El atributo `text` almacena el texto que se va a analizar, `language` es el idioma del documento y `id` puede ser cualquier valor. 

```javascript
const inputDocuments = {documents:[
    {language:"en", id:"1", text:"I had the best day of my life."},
    {language:"en", id:"2", text:"This was a waste of my time. The speaker put me to sleep."},
    {language:"es", id:"3", text:"No tengo dinero ni nada que dar..."},
    {language:"it", id:"4", text:"L'hotel veneziano era meraviglioso. È un bellissimo pezzo di architettura."}
]}
```

Llame a `client.sentiment` y obtenga el resultado. A continuación, itere por los resultados e imprima el identificador de cada documento y la puntuación de opiniones. Una puntuación cercana a 0 indica una opinión negativa, mientras que una puntuación cercana a 1 indica una opinión positiva.

```javascript
const operation = client.sentiment({multiLanguageBatchInput: inputDocuments})
operation
.then(result => {
    console.log(result.documents);
})
.catch(err => {
    throw err;
});
```

Ejecute el código con `node index.js` en la ventana de la consola.

### <a name="output"></a>Output

```console
[ { id: '1', score: 0.8723785877227783 },
  { id: '2', score: 0.1059873104095459 },
  { id: '3', score: 0.43635445833206177 },
  { id: '4', score: 1 } ]
```

## <a name="language-detection"></a>Detección de idiomas

Cree una lista de objetos que contenga los documentos. La carga a la API consta de una lista de `documents`, que contienen un atributo `id` y `text`. El atributo `text` almacena el texto que se va a analizar y el `id` puede ser cualquier valor.

```javascript
// The documents to be submitted for language detection. The ID can be any value.
const inputDocuments = {
    documents: [
        { id: "1", text: "This is a document written in English." },
        { id: "2", text: "Este es un document escrito en Español." },
        { id: "3", text: "这是一个用中文写的文件" }
    ]
    };
```

Llame a `client.detectLanguage()` y obtenga el resultado. A continuación, itere por los resultados e imprima el identificador de cada documento y el primer lenguaje devuelto.

```javascript
const operation = client.detectLanguage({
    languageBatchInput: inputDocuments
});
operation
    .then(result => {
    result.documents.forEach(document => {
        console.log(`ID: ${document.id}`);
        document.detectedLanguages.forEach(language =>
        console.log(`\tLanguage: ${language.name}`)
        );
    });
    })
    .catch(err => {
    throw err;
    });
```

Ejecute el código con `node index.js` en la ventana de la consola.

### <a name="output"></a>Output

```console
===== LANGUAGE EXTRACTION ======
ID: 1 Language English
ID: 2 Language Spanish
ID: 3 Language Chinese_Simplified
```

## <a name="entity-recognition"></a>Reconocimiento de entidades

Cree una lista de objetos que contenga los documentos. La carga a la API consta de una lista de `documents`, que contienen un atributo `id`, `language` y `text`. El atributo `text` almacena el texto que se va a analizar, `language` es el idioma del documento y `id` puede ser cualquier valor.

```javascript

    const inputDocuments = {documents:[
        {language:"en", id:"1", text:"Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800"},
        {language:"es", id:"2", text:"La sede principal de Microsoft se encuentra en la ciudad de Redmond, a 21 kilómetros de Seattle."},
        ]}

}
```

Llame a `client.entities()` y obtenga el resultado. A continuación, itere por los resultados e imprima el identificador de cada documento. Para cada identidad detectada, imprima su nombre de Wikipedia, el tipo y los subtipos (si existen) y las ubicaciones del texto original.

```javascript
const operation = client.entities({
    multiLanguageBatchInput: inputDocuments
});
operation
    .then(result => {
    result.documents.forEach(document => {
        console.log(`Document ID: ${document.id}`)
        document.entities.forEach(e =>{
        console.log(`\tName: ${e.name} Type: ${e.type} Sub Type: ${e.type}`)
        e.matches.forEach(match => (
            console.log(`\t\tOffset: ${match.offset} Length: ${match.length} Score: ${match.entityTypeScore}`)
        ))
        })
    });
    })
    .catch(err => {
    throw err;
    });
```

Ejecute el código con `node index.js` en la ventana de la consola.

### <a name="output"></a>Output

```console
Document ID: 1
    Name: Microsoft Type: Organization Sub Type: Organization
            Offset: 0 Length: 9 Score: 1
    Name: Bill Gates Type: Person Sub Type: Person
            Offset: 25 Length: 10 Score: 0.999786376953125
    Name: Paul Allen Type: Person Sub Type: Person
            Offset: 40 Length: 10 Score: 0.9988105297088623
    Name: April 4 Type: Other Sub Type: Other
            Offset: 54 Length: 7 Score: 0.8
    Name: April 4, 1975 Type: DateTime Sub Type: DateTime
            Offset: 54 Length: 13 Score: 0.8
    Name: BASIC Type: Other Sub Type: Other
            Offset: 89 Length: 5 Score: 0.8
    Name: Altair 8800 Type: Other Sub Type: Other
            Offset: 116 Length: 11 Score: 0.8
Document ID: 2
    Name: Microsoft Type: Organization Sub Type: Organization
            Offset: 21 Length: 9 Score: 0.999755859375
    Name: Redmond (Washington) Type: Location Sub Type: Location
            Offset: 60 Length: 7 Score: 0.9911284446716309
    Name: 21 kilómetros Type: Quantity Sub Type: Quantity
            Offset: 71 Length: 13 Score: 0.8
    Name: Seattle Type: Location Sub Type: Location
            Offset: 88 Length: 7 Score: 0.9998779296875
```

## <a name="key-phrase-extraction"></a>Extracción de la frase clave

Cree una lista de objetos que contenga los documentos. La carga a la API consta de una lista de `documents`, que contienen un atributo `id`, `language` y `text`. El atributo `text` almacena el texto que se va a analizar, `language` es el idioma del documento y `id` puede ser cualquier valor.

```javascript
    let inputLanguage = {
    documents: [
        {language:"ja", id:"1", text:"猫は幸せ"},
        {language:"de", id:"2", text:"Fahrt nach Stuttgart und dann zum Hotel zu Fu."},
        {language:"en", id:"3", text:"My cat might need to see a veterinarian."},
        {language:"es", id:"4", text:"A mi me encanta el fútbol!"}
    ]
    };
```

Llame a `client.keyPhrases()` y obtenga el resultado. A continuación, itere por los resultados e imprima el identificador de cada documento y todas las frases clave detectadas.

```javascript
    let operation = client.keyPhrases({
    multiLanguageBatchInput: inputLanguage
    });
    operation
    .then(result => {
        console.log(result.documents);
    })
    .catch(err => {
        throw err;
    });
```

Ejecute el código con `node index.js` en la ventana de la consola.

### <a name="output"></a>Output

```console
[ 
    { id: '1', keyPhrases: [ '幸せ' ] },
    { id: '2', keyPhrases: [ 'Stuttgart', 'Hotel', 'Fahrt', 'Fu' ] },
    { id: '3', keyPhrases: [ 'cat', 'veterinarian' ] },
    { id: '4', keyPhrases: [ 'fútbol' ] } 
]
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Text Analytics con Power BI](../tutorials/tutorial-power-bi-key-phrases.md)

## <a name="see-also"></a>Otras referencias

 [Introducción a Text Analytics](../overview.md) [Preguntas más frecuentes](../text-analytics-resource-faq.md)
