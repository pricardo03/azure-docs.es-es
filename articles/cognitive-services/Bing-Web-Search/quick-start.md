---
title: Inicio rápido de Web Search API
titleSuffix: Azure Cognitive Services
description: Aquí se muestra cómo empezar a usar Bing Web Search API.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.assetid: 27B4B51A-D017-44C8-8E4E-9684DC553886
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: quickstart
ms.date: 03/17/2019
ms.author: scottwhi
ms.openlocfilehash: e4076b042642df3cd232ff52769d790da5a1bcc4
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58163810"
---
# <a name="your-first-bing-search-query"></a>Esta será su primera consulta de búsqueda en Bing

Para poder realizar la primera llamada, debe obtener una clave de suscripción de Cognitive Services. Para obtener una clave, consulte [Pruebe Cognitive Services](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api). Consulte también [Precios de Cognitive Services - Bing Search API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

Para obtener los resultados de búsqueda web, envíe una solicitud GET al siguiente punto de conexión:  

```
https://api.cognitive.microsoft.com/bing/v7.0/search
```  

La solicitud debe usar el protocolo HTTPS.

Se recomienda que todas las solicitudes procedan de un servidor. Al distribuir la clave como parte de una aplicación cliente, existe una mayor probabilidad de que un tercero malintencionado acceda a ella. Además, la realización de llamadas desde un servidor proporciona un único punto de actualización para futuras versiones de la API.  

La solicitud debe especificar el parámetro de consulta [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#query), que contiene el término de búsqueda del usuario. Aunque es opcional, la solicitud debe especificar también el parámetro de consulta [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#mkt), que identifica el mercado de donde desea que procedan los resultados. Para ver una lista de parámetros de consulta opcionales, como `responseFilter` y `textDecorations`, consulte [Query Parameters](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#query-parameters) (Parámetros de consulta). Todos los valores de parámetro de consulta deben estar codificados con una dirección URL.  

La solicitud debe especificar el encabezado [Ocp-Apim-Subscription-Key](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#subscriptionkey). Aunque es opcional, se recomienda especificar los encabezados siguientes:  

-   [User-Agent](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#useragent)  
-   [X-MSEdge-ClientID](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#clientid)  
-   [X-Search-ClientIP](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#clientip)  
-   [X-Search-Location](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#location)  

Los encabezados de IP y ubicación del cliente son importantes para devolver contenido relativo a la ubicación. Por ejemplo, si la consulta del usuario es *clases+vela*, lo más probable es que estén interesados en clases cerca de su ubicación. Si desea que los resultados muestren clases particulares que estén disponibles cerca de la ubicación del usuario, deberá incluir el encabezado de ubicación y, opcionalmente, el encabezado de dirección IP del cliente. Es menos importante si el término de la consulta hace referencia explícitamente a una ubicación (por ejemplo, clases+vela+cayos+florida).

Para ver una lista de todos los encabezados de solicitud y respuesta, consulte [Encabezados](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#headers).

## <a name="the-request"></a>Solicitud

A continuación se muestra una solicitud de búsqueda que incluye todos los encabezados y parámetros de consulta sugeridos. Si es la primera vez que llama a cualquiera de las API de Bing, no incluya el encabezado de identificador de cliente. Solo debe incluir el identificador de cliente si se ha llamado previamente a una API de Bing y Bing ha devuelto un identificador de cliente para esa combinación de usuario y dispositivo.

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+lessons+seattle&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

A continuación se muestra la respuesta a la solicitud anterior. En el ejemplo también se muestran los encabezados de respuesta específicos de Bing.

```
BingAPIs-TraceId: 76DD2C2549B94F9FB55B4BD6FEB6AC
X-MSEdge-ClientID: 1C3352B306E669780D58D607B96869
BingAPIs-Market: en-US

{
    "_type" : "SearchResponse",
    "webPages" : {
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=70BE289346...",
        "totalEstimatedMatches" : 982000,
        "value" : [{
            "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.0",
            "name" : "Seattle Sailing Club | Lessons, Membership & More",
            "url" : "https:\/\/www.bing.com\/cr?IG=70BE289346ED4594874FE...",
            "displayUrl" : "https:\/\/seattlesailing.com",
            "snippet" : "Sailing is fun! Seattle Sailing Club makes it accessible and affordable...",
            "deepLinks" : [{
                "name" : "Learn to Sail",
                "url" : "https:\/\/www.bing.com\/cr?IG=70BE289346ED4594874FEFDAD..."
            },
            {
                "name" : "Fleet",
                "url" : "https:\/\/www.bing.com\/cr?IG=70BE289346ED4594874FEFDAD38...",
                "snippet" : "The Seattle Sailing Club is nothing without our fleet of excellent..."
            },
            {
                "name" : "Sailing Lessons",
                "url" : "http:\/\/www.bing.com\/cr?IG=70BE289346ED4594874FEFDAD38...",
                "snippet" : "Sailing Lessons. Taking the first step into the sailing community..."
            },
            {
                "name" : "Membership",
                "url" : "https:\/\/www.bing.com\/cr?IG=70BE289346ED4594874FEFDAD3...",
                "snippet" : "Seattle sailboat rental for less than the cost of moorage!"
            },
            {
                "name" : "Hours – Directions",
                "url" : "https:\/\/www.bing.com\/cr?IG=70BE289346ED4594874...",
                "snippet" : "Contact the Seattle Sailing Club by phone or email..."
            },
            {
                "name" : "Overnight Cruising",
                "url" : "https:\/\/www.bing.com\/cr?IG=70BE289346ED4594874FEFDAD...",
                "snippet" : "Overnight Cruising. Setting sail for a multi-day adventure..."
            }],
            "dateLastCrawled" : "2017-04-07T02:25:00"
        },
        {
            "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.1",
            "name" : "Best Sailing lessons in Seattle, WA - yelp.com",
            "url" : "https:\/\/www.bing.com\/cr?IG=70BE289346ED4594874FEFDAD38AA...",
            "displayUrl" : "https:\/\/www.yelp.com\/search?find_desc=sailing+lessons&find_loc...",
            "snippet" : "Reviews on Sailing lessons in Seattle, WA - Seattle Sailing...",
            "dateLastCrawled" : "2017-04-01T10:49:00"
        },
        {
            "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.2",
            "name" : "Seattle Sailing with Windworks Sailing Club - Sailing ...",
            "url" : "https:\/\/www.bing.com\/cr?IG=70BE289346ED4594874FEFDAD3...",
            "displayUrl" : "https:\/\/www.windworkssailing.com",
            "snippet" : "Seattle sailing club for sailing lessons, sailboat charters...",
            "dateLastCrawled" : "2017-04-09T03:33:00"
        },
        {
            "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.3",
            "name" : "Sailing Lessons in Downtown Seattle - SheSails Seattle",
            "url" : "http:\/\/www.bing.com\/cr?IG=70BE289346ED4594874FEFDAD38...",
            "displayUrl" : "shesailsseattle.com",
            "snippet" : "SheSails Seattle is sailing lessons and sailboat charters in a fun...",
            "dateLastCrawled" : "2017-04-05T07:15:00"
        },
        {
            "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.4",
            "name" : "Seattle Sailing Club - 19 Photos & 27 Reviews - Boating ...",
            "url" : "https:\/\/www.bing.com\/cr?IG=70BE289346ED4594874FE...",
            "displayUrl" : "https:\/\/www.yelp.com\/biz\/seattle-sailing-club-seattle",
            "snippet" : "27 reviews of Seattle Sailing Club \"Love the Seattle Sailing...",
            "dateLastCrawled" : "2017-04-08T14:40:00"
        },
        {
            "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.5",
            "name" : "Puget Sound Sailing - Seattle and Tacoma - PUGET SOUND ...",
            "url" : "http:\/\/www.bing.com\/cr?IG=70BE289346ED4594874FEFDA...",
            "displayUrl" : "www.pugetsoundsailing.com",
            "snippet" : "Puget Sound Sailing Institute is an American Sailing Association...",
            "dateLastCrawled" : "2017-04-09T01:39:00"
        },
        {
            "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.6",
            "name" : "Seattle Yacht Club - Official Site",
            "url" : "http:\/\/www.bing.com\/cr?IG=70BE289346ED4594874FE...",
            "displayUrl" : "www.seattleyachtclub.org",
            "snippet" : "Seattle Yacht Club Awarded Joe Prosser Trophy for Excellence...",
            "dateLastCrawled" : "2017-04-09T14:30:00"
        }],
        "someResultsRemoved" : true
    },
    "relatedSearches" : {
        "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#RelatedSearches",
        "value" : [{
            "text" : "sailing lessons",
            "displayText" : "sailing lessons",
            "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=70BE289346E..."
        },
        {
            "text" : "sailing schools seattle",
            "displayText" : "sailing schools seattle",
            "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=70BE289346ED4..."
        },
        {
            "text" : "seattle boating lessons",
            "displayText" : "seattle boating lessons",
            "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=70BE289346ED4..."
        },
        {
            "text" : "adult sailing lessons seattle",
            "displayText" : "adult sailing lessons seattle",
            "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=70BE289346ED..."
        },
        {
            "text" : "seattle sailing club",
            "displayText" : "seattle sailing club",
            "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=70BE289346ED459..."
        },
        {
            "text" : "seattle sailing club lessons",
            "displayText" : "seattle sailing club lessons",
            "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=70BE289..."
        },
        {
            "text" : "sailing in seattle area",
            "displayText" : "sailing in seattle area",
            "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=70BE289346ED45948..."
        },
        {
            "text" : "seattle sailing lessons lake union",
            "displayText" : "seattle sailing lessons lake union",
            "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=70BE289346ED459..."
        }]
    },
    "rankingResponse" : {
        "mainline" : {
            "items" : [{
                "answerType" : "WebPages",
                "resultIndex" : 0,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.0"
                }
            },
            {
                "answerType" : "WebPages",
                "resultIndex" : 1,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.1"
                }
            },
            {
                "answerType" : "WebPages",
                "resultIndex" : 2,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.2"
                }
            },
            {
                "answerType" : "WebPages",
                "resultIndex" : 3,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.3"
                }
            },
            {
                "answerType" : "WebPages",
                "resultIndex" : 4,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.4"
                }
            },
            {
                "answerType" : "WebPages",
                "resultIndex" : 5,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.5"
                }
            },
            {
                "answerType" : "WebPages",
                "resultIndex" : 6,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.6"
                }
            }]
        },
        "sidebar" : {
            "items" : [{
                "answerType" : "RelatedSearches",
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#RelatedSearches"
                }
            }]
        }
    }
}
```

## <a name="next-steps"></a>Pasos siguientes

Pruebe la API. Vaya a la [consola de pruebas de Web Search API](https://dev.cognitive.microsoft.com/docs/services/56b43eeccf5ff8098cef3807/operations/56b4447dcf5ff8098cef380d).

Para más información sobre cómo consumir los objetos de respuesta, consulte [Searching the Web](./search-the-web.md) (Búsqueda en la Web).
