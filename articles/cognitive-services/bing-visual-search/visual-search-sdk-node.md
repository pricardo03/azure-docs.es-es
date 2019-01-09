---
title: 'Inicio rápido: Obtención de información sobre imágenes con el SDK de Bing Visual Search para Node.js'
titleSuffix: Azure Cognitive Services
description: Cargue una imagen utilizando el SDK de Bing Visual Search y obtenga conclusiones sobre ella.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: quickstart
ms.date: 05/18/2018
ms.author: v-gedod
ms.openlocfilehash: 1dba0f3aacd0e51a9e4d8017a93f18928fd6b2ea
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2018
ms.locfileid: "53744300"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-sdk-for-nodejs"></a>Inicio rápido: Obtención de información sobre imágenes con el SDK de Bing Visual Search para Node.js

Use este artículo de inicio rápido para empezar a obtener información sobre las imágenes desde el servicio Bing Visual Search con el SDK de Node.js. Aunque Bing Visual Search tiene una API de REST compatible con la mayoría de los lenguajes de programación, el SDK proporciona una forma sencilla de integrar el servicio en sus aplicaciones. El código fuente correspondiente a este ejemplo se encuentra disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/visualSearch.js). 

## <a name="prerequisites"></a>Requisitos previos
* [Node.js](https://www.nodejs.org/)
* El SDK de Bing Visual Search para Node.js
    * Para configurar una aplicación de consola mediante el SDK de Bing Visual Search, ejecute los siguientes comandos:
        1. `npm install ms-rest-azure`
        2. `npm install azure-cognitiveservices-search-visualSearch`.


[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

<a name="client"></a>

## <a name="create-and-initialize-the-application"></a>Creación e inicialización de la aplicación

1. Cree un archivo JavaScript en el editor o el IDE que prefiera y agregue los siguientes requisitos. Después, cree variables para la clave de suscripción, el identificador de configuración personalizado y la ruta de acceso al archivo para imagen que quiere cargar. 

    ```javascript
    const os = require("os");
    const async = require('async');
    const fs = require('fs');
    const Search = require('azure-cognitiveservices-visualsearch');
    const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
    
    let keyVar = 'YOUR-VISUAL-SEARCH-ACCESS-KEY';
    let credentials = new CognitiveServicesCredentials(keyVar);
    let filePath = "../Data/image.jpg";
    ```

2. Cree una instancia del cliente.

    ```javascript
    let visualSearchApiClient = new Search.VisualSearchAPIClient(credentials);
    ```

## <a name="search-for-images"></a>Búsqueda de imágenes

1. Use `fs.createReadStream()` para leer en el archivo de imagen y cree variables para la solicitud de búsqueda y los resultados. Luego, use el cliente para buscar imágenes.

    ```javascript
    let fileStream = fs.createReadStream(filePath);
    let visualSearchRequest = JSON.stringify({});
    let visualSearchResults;
    try {
        visualSearchResults = await visualSearchApiClient.images.visualSearch({
            image: fileStream,
            knowledgeRequest: visualSearchRequest
        });
        console.log("Search visual search request with binary of image");
    } catch (err) {
        console.log("Encountered exception. " + err.message);
    }
    ```

2. Analice los resultados de la consulta anterior:

    ```javascript
    // Visual Search results
    if (visualSearchResults.image.imageInsightsToken) {
        console.log(`Uploaded image insights token: ${visualSearchResults.image.imageInsightsToken}`);
    }
    else {
        console.log("Couldn't find image insights token!");
    }
    
    // List of tags
    if (visualSearchResults.tags.length > 0) {
        let firstTagResult = visualSearchResults.tags[0];
        console.log(`Visual search tag count: ${visualSearchResults.tags.length}`);
    
        // List of actions in first tag
        if (firstTagResult.actions.length > 0) {
            let firstActionResult = firstTagResult.actions[0];
            console.log(`First tag action count: ${firstTagResult.actions.length}`);
            console.log(`First tag action type: ${firstActionResult.actionType}`);
        }
        else {
            console.log("Couldn't find tag actions!");
        }
    
    }
    else {
        console.log("Couldn't find image tags!");
    }
    
    ```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Compilar una aplicación web de una sola página](tutorial-bing-visual-search-single-page-app.md)
