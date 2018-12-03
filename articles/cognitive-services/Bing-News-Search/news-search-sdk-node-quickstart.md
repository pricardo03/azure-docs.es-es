---
title: 'Guía de inicio rápido: SDK de Bing News Search con Node'
titleSuffix: Azure Cognitive Services
description: Configuración de la aplicación de consola del SDK de Bing News Search
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: quickstart
ms.date: 02/12/2018
ms.author: v-gedod
ms.openlocfilehash: 075ce771b6fa92663f172734aabf57b089b294a9
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2018
ms.locfileid: "52316946"
---
# <a name="quickstart-bing-news-search-sdk-with-node"></a>Guía de inicio rápido: SDK de Bing News Search con Node

El SDK de Bing News Search contiene la funcionalidad de la API REST para consultas de noticias y análisis de resultados. 

El [código fuente de los ejemplos del SDK de Bing News Search para Node](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/newsSearch.js) está disponible en GitHub.

## <a name="application-dependencies"></a>Dependencias de aplicaciones
Obtenga una [clave de acceso de Cognitive Services](https://azure.microsoft.com/try/cognitive-services/) en **Buscar**.  Consulte también [Precios de Cognitive Services - Bing Search API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/). 

Para configurar una aplicación de consola mediante el SDK de Bing News Search:
* Ejecute `npm install ms-rest-azure` en el entorno de desarrollo.
* Ejecute `npm install azure-cognitiveservices-newssearch` en el entorno de desarrollo.

## <a name="news-search-client"></a>Cliente de News Search
Obtenga una [clave de acceso de Cognitive Services](https://azure.microsoft.com/try/cognitive-services/) en *Buscar*. Creación de una instancia de `CognitiveServicesCredentials`:
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
Después, cree una instancia del cliente:
```
const NewsSearchAPIClient = require('azure-cognitiveservices-newssearch');
let client = new NewsSearchAPIClient(credentials);
```
Utilice el cliente para buscar con un texto de consulta, en este caso "Juegos Olímpicos de Invierno":
```
client.newsOperations.search('Winter Olympics').then((result) => {
    console.log(result.value);
}).catch((err) => {
    throw err;
});

```
El código imprime `result.value` elementos en la consola sin analizar ningún texto. Los resultados, si hubiera por categoría, incluyen:
- _type: "NewsArticle"
- _type: "WebPage"
- _type: "VideoObject"
- _type: "ImageObject"

<!-- Remove until we can replace with santized version
![News results](media/node-sdk-quickstart-results.png)
-->

## <a name="next-steps"></a>Pasos siguientes

[Ejemplos del SDK de Cognitive Services para Node.js](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)
