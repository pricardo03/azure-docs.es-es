---
title: 'Cómo filtrar los resultados de búsqueda: Bing Web Search API'
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo filtrar y mostrar los resultados de búsqueda de Bing Web Search API.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.assetid: 8B837DC2-70F1-41C7-9496-11EDFD1A888D
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: conceptual
ms.date: 01/12/2017
ms.author: scottwhi
ms.openlocfilehash: 700fae4e206e547037406d4f15f32cb167fbe6b9
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "46123596"
---
# <a name="filtering-the-answers-that-the-search-response-includes"></a>Filtrado de los resultados incluidos en la respuesta de búsqueda  

Al consultar la web, Bing devuelve todo el contenido que cree que es pertinente para la búsqueda. Por ejemplo, si la consulta de búsqueda es "navegar+barco", la respuesta podría contener los resultados siguientes:

```json
{
    "_type" : "SearchResponse",
    "webPages" : {
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3A43C...",
        "totalEstimatedMatches" : 262000,
        "value" : [...]
    },
    "images" : {
        "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#Images",
        "readLink" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=sail...",
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3A43CA5CA6464E5D...",
        "isFamilyFriendly" : true,
        "value" : [...]
    },
    "rankingResponse" : {
        "mainline" : {
            "items" : [...]
        }
    }
}    
```

Si está interesado en determinados tipos de contenido, como imágenes, vídeos o noticias, puede solicitar solo esos resultados con el parámetro de consulta [responseFilter](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#responsefilter). Si Bing busca contenido pertinente para los resultados especificados, Bing lo devuelve. El filtro de respuesta es una lista de resultados delimitados por comas. A continuación se muestra cómo utilizar `responseFilter` para solicitar imágenes, vídeos y noticias de barcos de navegación. Cuando se codifica la cadena de consulta, las comas cambian a %2c.  

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&responseFilter=images%2Cvideos%2Cnews&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location:  47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

A continuación se muestra la respuesta de la consulta anterior. Como puede ver, Bing no encontró resultados pertinentes de vídeo y de noticias, y no se incluyen en la respuesta.

```json
{
    "_type" : "SearchResponse",
    "images" : {
        "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#Images",
        "readLink" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=sail...",
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3AD78B183C56456C...",
        "isFamilyFriendly" : true,
        "value" : [...]
    },
    "rankingResponse" : {
        "mainline" : {
            "items" : [{
                "answerType" : "Images",
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#Images"
                }
            }]
        }
    }
}
```

Si desea excluir determinados tipos de contenido (por ejemplo, imágenes) de la respuesta, puede excluirlos agregando un prefijo con un guion (signo menos) al valor responseFilter. Separe los tipos excluidos con comas:

```
&responseFilter=-images,-videos
```

Aunque Bing no devolvió resultados de vídeo y de noticias en la respuesta anterior, no significa que no exista contenido de vídeo y de noticias. Solo quiere decir que la página no los incluyó. Sin embargo, si [página](./paging-webpages.md) más resultados, probablemente las páginas siguientes los incluyan. Además, si se llama directamente a los puntos de conexión de [Video Search API](../bing-video-search/search-the-web.md) y [News Search API](../bing-news-search/search-the-web.md), es probable que la respuesta contenga resultados.

No recomienda no utilizar `responseFilter` para obtener los resultados de una sola API. Si desea contenido de una sola API de Bing, llame directamente a dicha API. Por ejemplo, para recibir solo las imágenes, envíe una solicitud al punto de conexión de Image Search API, `https://api.cognitive.microsoft.com/bing/v7.0/images/search` u otros de los puntos de conexión de [imágenes](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#endpoints). Llamar a una sola API es importante no solo por motivos de rendimiento, también porque las API específicas de contenido ofrecen mejores resultados. Por ejemplo, puede utilizar filtros que no están disponibles en Web Search API para filtrar los resultados.  

Para obtener resultados de búsqueda de un dominio específico, incluya el operador de consulta `site:` en la cadena de consulta.  

```
https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies+site:contososailing.com&mkt=en-us
```

> [!NOTE]
> En función de la consulta, si usa el operador de consulta `site:`, existe la posibilidad de que la respuesta incluya contenido para adultos sin tener en cuenta la configuración de [safeSearch](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#safesearch) (búsqueda segura). Debería usar `site:` solo si es consciente del contenido del sitio y el escenario admite la posibilidad de contenido para adultos.

## <a name="limiting-the-number-of-answers-in-the-response"></a>Limitación del número de resultados en la respuesta

Bing incluye resultados en la respuesta en función de la clasificación. Por ejemplo, si consulta *navegar + barco*, Bing devuelve `webpages`, `images`, `videos`, y `relatedSearches`.

```json
{
    "_type" : "SearchResponse",
    "queryContext" : {
        "originalQuery" : "sailing dinghies"
    },
    "webPages" : {...},
    "images" : {...},
    "relatedSearches" : {...},
    "videos" : {...},
    "rankingResponse" : {...}
}
```

Para limitar el número de resultados que Bing devuelve a los dos primeros resultados (páginas web e imágenes), establezca el parámetro de consulta [answerCount](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#answercount) en 2.

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&answerCount=2&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location:  47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

La respuesta solo incluye `webPages` y `images`.

```json
{
    "_type" : "SearchResponse",
    "queryContext" : {
        "originalQuery" : "sailing dinghies"
    },
    "webPages" : {...},
    "images" : {...},
    "rankingResponse" : {...}
}
```

Si agrega el parámetro de consulta `responseFilter` a la consulta anterior y lo establece en páginas web y noticias, la respuesta contiene solo páginas web porque no se han clasificado las noticias.

```json
{
    "_type" : "SearchResponse",
    "queryContext" : {
        "originalQuery" : "sailing dinghies"
    },
    "webPages" : {...},
    "rankingResponse" : {...}
}
```

## <a name="promoting-answers-that-are-not-ranked"></a>Promoción de resultados no clasificados

Si los principales resultados que Bing devuelve para una consulta son páginas web, imágenes, vídeos y búsquedas relacionadas, la respuesta incluirá dichos resultados. Si establece [answerCount](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#answercount) en dos (2), Bing devuelve los dos primeros resultados clasificados: páginas web e imágenes. Si desea que Bing incluya imágenes y vídeos en la respuesta, especifique el parámetro de consulta [promote](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#promote) y establézcalo en images y videos.

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&answerCount=2&promote=images%2Cvideos&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location:  47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

La siguiente es la respuesta a la solicitud anterior. Bing devuelve los dos principales resultados, páginas web e imágenes, y promociona los vídeos en la respuesta.

```json
{
    "_type" : "SearchResponse",
    "queryContext" : {
        "originalQuery" : "sailiing dinghies"
    },
    "webPages" : {...},
    "images" : {...},
    "videos" : {...},
    "rankingResponse" : {...}
}
```

Si establece `promote` en news, la respuesta no incluye los resultados de noticias porque no es un resultado clasificado. Solo se pueden promocionar resultados clasificados.

Los resultados que desea promocionar no cuentan para el límite de `answerCount`. Por ejemplo, si los resultados clasificados son noticias, imágenes y vídeos, y establece `answerCount` en 1 y `promote` en news, la respuesta contiene noticias e imágenes. O bien, si los resultados clasificados son vídeos, imágenes y noticias, la respuesta contiene vídeos y noticias.

Puede usar `promote` únicamente si se especifica el parámetro de consulta `answerCount`.
