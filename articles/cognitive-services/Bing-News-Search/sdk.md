---
title: SDK de Bing Search | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: SDK de Bing Search para las aplicaciones que buscan en la web.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.assetid: ''
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: article
ms.date: 1/24/2018
ms.author: v-gedod
ms.openlocfilehash: 4a40ea665e153536d2322706b455598902ce41eb
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35382603"
---
# <a name="bing-search-sdk"></a>SDK de Bing Search
Los ejemplos de Bing News Search API incluyen los escenarios en los que:
1. Se consultan noticias para los términos de búsqueda con los parámetros `market` y `count`, se verifica el número de resultados y se imprime `totalEstimatedMatches`, el nombre, la dirección URL, la descripción, la hora de publicación y el nombre del proveedor del primer resultado de noticias.
2. Se consultan las noticias más recientes para los términos de búsqueda con los parámetros `freshness` y `sortBy`, se verifica el número de resultados y se imprime `totalEstimatedMatches`, la dirección URL, la descripción, la hora de publicación y el nombre del proveedor del primer resultado de noticias.
3. Se consultan las noticias de la categoría para `movie` y `TV entertainment` con la búsqueda segura, se verifica el número de resultados y se imprime la categoría, el nombre, la dirección URL, la descripción, la hora de publicación y el nombre del proveedor del primer resultado de noticias.
4. Se consultan los temas de tendencia de noticias de Bing, se verifica el número de resultados y se imprime el nombre, el texto de consulta, `webSearchUrl`, `newsSearchUrl` y la dirección URL del primer resultado de noticias.

Los SDK de Bing Search hacen que se pueda acceder de inmediato a la funcionalidad de búsqueda web en los siguientes lenguajes de programación:
* Introducción a [ejemplos de .NET](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
    * [Paquete NuGet](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.NewsSearch/1.2.0)
    * Consulte también [bibliotecas de .NET](https://github.com/Azure/azure-sdk-for-net/tree/psSdkJson6/src/SDKs/CognitiveServices/dataPlane/Search/BingNewsSearch) para las definiciones y dependencias.
* Introducción a [ejemplos de Node.js](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples) 
    * Consulte también [bibliotecas de Node.js](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/newsSearch) para las definiciones y dependencias.
* Introducción a [ejemplos de Java](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples) 
    * Consulte también [bibliotecas de Java](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master/Search/BingNewsSearch) para las definiciones y dependencias.
* Introducción a [ejemplos de Python](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples) 
    * Consulte también [bibliotecas de Python](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-cognitiveservices-search-newssearch) para las definiciones y dependencias.

Entre los ejemplos de SDK para cada lenguaje se incluye un archivo Léame con detalles sobre los requisitos previos y la instalación y ejecución de los ejemplos.