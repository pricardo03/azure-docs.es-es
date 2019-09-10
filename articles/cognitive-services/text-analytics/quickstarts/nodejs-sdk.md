---
title: 'Inicio rápido: biblioteca cliente de Text Analytics para Node.js | Microsoft Docs'
titleSuffix: Azure Cognitive Services
description: Obtenga información y ejemplos de código que le ayuden a empezar a usar rápidamente Text Analytics API.
services: cognitive-services
author: raymondl
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 08/28/2019
ms.author: shthowse
ms.openlocfilehash: c3cd64d0a683a60132808bca6a7ceb4aa84db4f1
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2019
ms.locfileid: "70195196"
---
# <a name="quickstart-text-analytics-client-library-for-nodejs"></a>Inicio rápido: biblioteca cliente de Text Analytics para Node.js
<a name="HOLTop"></a>

Introducción a la biblioteca cliente de Text Analytics para Node.js. Siga estos pasos para instalar el paquete y probar el código de ejemplo para realizar tareas básicas. 

Use la biblioteca cliente de Text Analytics para Node.js para realizar estas acciones:

* análisis de opiniones
* Detección de idiomas
* Reconocimiento de entidades
* Extracción de la frase clave

[Documentación de referencia](https://docs.microsoft.com/javascript/api/overview/azure/cognitiveservices/textanalytics?view=azure-node-latest) | [Código fuente de la biblioteca](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/cognitiveServicesTextAnalytics) | [Paquete (NPM)](https://www.npmjs.com/package/azure-cognitiveservices-textanalytics) | [Ejemplos](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/)

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/)
* La versión actual de [Node.js](https://nodejs.org/).

## <a name="setting-up"></a>Instalación

### <a name="create-a-text-analytics-azure-resource"></a>Creación de un recurso de Azure para Text Analytics

Los servicios de Azure Cognitive Services se representan por medio de recursos de Azure a los que se suscribe. Cree un recurso para Text Analytics mediante [Azure Portal](../../cognitive-services-apis-create-account.md) o la [CLI de Azure](../../cognitive-services-apis-create-account-cli.md) en el equipo local. También puede:

* Obtener una [clave de prueba](https://azure.microsoft.com/try/cognitive-services/#decision) válida durante siete días de forma gratuita. Después de registrarse, estará disponible en el [sitio web de Azure](https://azure.microsoft.com/try/cognitive-services/my-apis/).  
* Ver este recurso en [Azure Portal](https://portal.azure.com/).

Después de obtener una clave de la suscripción de evaluación o el recurso, [cree una variable de entorno](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) para ella denominada `TEXTANALYTICS_SUBSCRIPTION_KEY`.

### <a name="create-a-new-nodejs-application"></a>Creación de una aplicación Node.js

En una ventana de la consola (como cmd, PowerShell o Bash), cree un directorio para la aplicación y vaya a él. 

```console
mkdir myapp && cd myapp
```

Ejecute el comando `npm init` para crear una aplicación de nodo con un archivo `package.json`. 

```console
npm init
```

Cree un archivo llamado `index.js` e importe las bibliotecas siguientes:

```javascript
const CognitiveServicesCredentials = require("@azure/ms-rest-js");
const TextAnalyticsAPIClient = require("@azure/cognitiveservices-textanalytics");
```

Cree variables para la clave de suscripción y el punto de conexión de Azure del recurso. Estos valores se obtienen de las variables de entorno TEXT_ANALYTICS_SUBSCRIPTION_KEY y TEXT_ANALYTICS_ENDPOINT. Si creó estas variables de entorno después de haber comenzado a editar la aplicación, deberá cerrar y volver a abrir el editor, el IDE o el shell que usa para acceder a ellas.

[!INCLUDE [text-analytics-find-resource-information](../includes/find-azure-resource-info.md)]

```javascript
const key_var = 'TEXT_ANALYTICS_SUBSCRIPTION_KEY';
if (!process.env[key_var]) {
    throw new Error('please set/export the following environment variable: ' + key_var);
}
const subscription_key = process.env[key_var];

const endpoint_var = 'TEXT_ANALYTICS_ENDPOINT';
if (!process.env[endpoint_var]) {
    throw new Error('please set/export the following environment variable: ' + endpoint_var);
}
const endpoint = process.env[endpoint_var];
```

### <a name="install-the-client-library"></a>Instalación de la biblioteca cliente

Instale los paquetes `@azure/ms-rest-js` y `@azure/cognitiveservices-textanalytics` de NPM:

```console
npm install @azure/cognitiveservices-textanalytics @azure/ms-rest-js
```

el archivo `package.json` de la aplicación se actualizará con las dependencias.

## <a name="object-model"></a>Modelo de objetos

El cliente de Text Analytics es un objeto de [TextAnalyticsClient](https://docs.microsoft.com/javascript/api/azure-cognitiveservices-textanalytics/textanalyticsclient?view=azure-node-latest) que se autentica en Azure mediante su clave. El cliente proporciona varios métodos para analizar texto, como una sola cadena o un lote.

El texto se envía a la API como una lista de `documents`, que son objetos `dictionary` que contienen una combinación de atributos `id`, `text` y `language`, según el método utilizado. El atributo `text` almacena el texto que se va a analizar en el origen `language` y `id` puede ser cualquier valor. 

El objeto de respuesta es una lista que contiene la información de análisis para cada documento. 

## <a name="code-examples"></a>Ejemplos de código

* [Autenticar el cliente](#authenticate-the-client)
* [Análisis de sentimiento](#sentiment-analysis)
* [Detección de idioma](#language-detection)
* [Reconocimiento de entidades](#entity-recognition)
* [Extracción de frases clave](#key-phrase-extraction)


## <a name="authenticate-the-client"></a>Autenticar el cliente

Cree un objeto [TextAnalyticsClient](https://docs.microsoft.com/javascript/api/azure-cognitiveservices-textanalytics/textanalyticsclient?view=azure-node-latest) con `credentials` y `endpoint` como parámetro.

```javascript
const creds = new CognitiveServicesCredentials.ApiKeyCredentials({ inHeader: { 'Ocp-Apim-Subscription-Key': subscription_key } });
const client = new TextAnalyticsAPIClient.TextAnalyticsClient(creds, endpoint);
```

## <a name="sentiment-analysis"></a>análisis de opiniones

Cree una lista de objetos que contenga los documentos que desea analizar.

```javascript
const inputDocuments = {documents:[
    {language:"en", id:"1", text:"I had the best day of my life."}
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
[ { id: '1', score: 0.8723785877227783 } ]
```

## <a name="language-detection"></a>Detección de idiomas

Cree una lista de objetos que contenga los documentos.

```javascript
// The documents to be submitted for language detection. The ID can be any value.
const inputDocuments = {
    documents: [
        { id: "1", text: "This is a document written in English." }
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
```

## <a name="entity-recognition"></a>Reconocimiento de entidades

Cree una lista de objetos que contenga los documentos.

```javascript
const inputDocuments = {
    documents: [
        { language: "en", id: "1", text: "Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800" }
    ]
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
```

## <a name="key-phrase-extraction"></a>Extracción de la frase clave

Cree una lista de objetos que contenga los documentos.

```javascript
    let inputLanguage = {
    documents: [
        {language:"en", id:"1", text:"My cat might need to see a veterinarian."}
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
    { id: '1', keyPhrases: [ 'cat', 'veterinarian' ] }
]
```

## <a name="run-the-application"></a>Ejecución de la aplicación

Ejecute la aplicación con el comando `node` en el archivo de inicio rápido.

```console
node index.js
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Si quiere limpiar y eliminar una suscripción a Cognitive Services, puede eliminar el recurso o grupo de recursos. Al eliminar el grupo de recursos, también se elimina cualquier otro recurso que esté asociado a él.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [CLI de Azure](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Text Analytics con Power BI](../tutorials/tutorial-power-bi-key-phrases.md)


* [Información general de Text Analytics](../overview.md)
* [Análisis de opiniones](../how-tos/text-analytics-how-to-sentiment-analysis.md)
* [Reconocimiento de entidades](../how-tos/text-analytics-how-to-entity-linking.md)
* [Detección de idioma](../how-tos/text-analytics-how-to-keyword-extraction.md)
* [Reconocimiento de idioma](../how-tos/text-analytics-how-to-language-detection.md)
* El código fuente de este ejemplo está disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/textAnalytics.js).
