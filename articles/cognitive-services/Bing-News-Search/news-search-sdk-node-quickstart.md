---
title: 'Inicio rápido: Realizar una búsqueda de noticias con el SDK de Bing News Search para Node.js'
titleSuffix: Azure Cognitive Services
description: Use esta guía de inicio rápido para buscar noticias mediante el SDK de Bing News Search para Node.js y procesar la respuesta.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: quickstart
ms.date: 06/18/2019
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: c71d76e93eb1a121163d40fe61dcd5a8546d63f8
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67203358"
---
# <a name="quickstart-perform-a-news-search-with-the-bing-news-search-sdk-for-nodejs"></a>Guía de inicio rápido: Realizar una búsqueda de noticias con el SDK de Bing News Search para Node.js

Use este inicio rápido para empezar a buscar noticias con el SDK de Bing News Search para Node.js. Aunque Bing News Search tiene una API REST compatible con la mayoría de los lenguajes de programación, el SDK proporciona una forma sencilla de integrar el servicio en sus aplicaciones. El código fuente de este ejemplo está disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/newsSearch.js).

## <a name="prerequisites"></a>Requisitos previos

* [Node.js](https://nodejs.org/en/)

Para configurar una aplicación de consola mediante el SDK de Bing News Search:
1. Ejecute `npm install ms-rest-azure` en el entorno de desarrollo.
2. Ejecute `npm install azure-cognitiveservices-newssearch` en el entorno de desarrollo.


[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../includes/cognitive-services-bing-news-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Creación e inicialización de la aplicación

1. Creación de una instancia de `CognitiveServicesCredentials`. Cree variables para el término de búsqueda y la clave de suscripción.

    ```javascript
    const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
    let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
    let search_term = 'Winter Olympics'
    ```

2. Cree una instancia del cliente:
    
    ```javascript
    const NewsSearchAPIClient = require('azure-cognitiveservices-newssearch');
    let client = new NewsSearchAPIClient(credentials);
    ```

## <a name="send-a-search-query"></a>Envío de una consulta de búsqueda

1. Utilice el cliente para buscar un término de consulta, en este caso "Juegos olímpicos de invierno":
    
    ```javascript
    client.newsOperations.search(search_term).then((result) => {
        console.log(result.value);
    }).catch((err) => {
        throw err;
    });
    ```

El código imprime `result.value` elementos en la consola sin analizar ningún texto. Los resultados, si hubiera por categoría, incluyen:

- `_type: 'NewsArticle'`
- `_type: 'WebPage'`
- `_type: 'VideoObject'`
- `_type: 'ImageObject'`

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Creación de una aplicación web de una sola página](tutorial-bing-news-search-single-page-app.md)
