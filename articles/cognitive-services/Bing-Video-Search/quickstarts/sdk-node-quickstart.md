---
title: 'Inicio rápido: Búsqueda de vídeos mediante el SDK de Bing Video Search para Node.js'
titleSuffix: Azure Cognitive Services
description: Use este artículo de inicio rápido para enviar solicitudes de búsqueda de vídeo mediante el SDK de Bing Video Search para Node.js.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: quickstart
ms.date: 01/31/2019
ms.author: rosh
ms.openlocfilehash: ffb04634b41d14e151c5a14d052d0d9f70b34d70
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2019
ms.locfileid: "55569284"
---
# <a name="quickstart-perform-a-video-search-with-the-bing-video-search-sdk-for-nodejs"></a>Inicio rápido: Realización de una búsqueda de vídeo con el SDK de Bing Video Search para Node.js

Use este artículo de inicio rápido para empezar a buscar vídeos con el SDK de Bing Video Search para Node.js. Aunque Bing Video Search tiene una API de REST compatible con la mayoría de los lenguajes de programación, el SDK proporciona una forma sencilla de integrar el servicio en sus aplicaciones. El código fuente de este ejemplo está disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/videoSearch.js). Contiene más anotaciones y características.

## <a name="prerequisites"></a>Requisitos previos

- [Node.js](https://www.nodejs.org/)

Para configurar una aplicación de consola mediante el SDK de Bing Video Search:
* Ejecute `npm install ms-rest-azure` en el entorno de desarrollo.
* Ejecute `npm install azure-cognitiveservices-videosearch` en el entorno de desarrollo.

[!INCLUDE [cognitive-services-bing-video-search-signup-requirements](../../../../includes/cognitive-services-bing-video-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Creación e inicialización de la aplicación

1. Cree un archivo JavaScript en el editor o el IDE que prefiera y añada una instrucción `require()` al SDK de Bing Video Search y al módulo `CognitiveServicesCredentials`. Cree una variable para la clave de suscripción. 
    
    ```javascript
    const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
    const VideoSearchAPIClient = require('azure-cognitiveservices-videosearch');
    ```

2. Cree una instancia de `CognitiveServicesCredentials` con su clave. A continuación, utilícela para crear una instancia del cliente de búsqueda de vídeo.

    ```javascript
    let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
    let client = new VideoSearchAPIClient(credentials);
    ```

## <a name="send-the-search-request"></a>Envío de la solicitud de búsqueda

1. Use `client.videosOperations.search()` para enviar una solicitud de búsqueda a Bing Video Search API. Cuando se devuelvan los resultados de búsqueda, utilice `.then()` para registrar el resultado.
    
    ```javascript
    client.videosOperations.search('Interstellar Trailer').then((result) => {
        console.log(result.value);
    }).catch((err) => {
        throw err;
    });
    ```

<!-- Remove until the response can be replace with a sanitized version.
The code prints `result.value` items to the console without parsing any text. The results will be:
- _type: 'VideoObjectElementType'

![Video results](media/video-search-sdk-node-results.png)
-->

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Creación de una aplicación web de una sola página](../tutorial-bing-video-search-single-page-app.md)

## <a name="see-also"></a>Otras referencias 

* [¿Qué es Bing Video Search API?](../overview.md)
* [Ejemplos del SDK de Cognitive Services para .NET](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)