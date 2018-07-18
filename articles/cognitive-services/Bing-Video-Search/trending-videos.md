---
title: Buscar vídeos populares en la Web | Microsoft Docs
description: Se muestra cómo usar Bing Video Search API para buscar vídeos populares en la Web.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 897A28A3-0980-484E-814F-FFE1D5C885E6
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 8db7fcf77042631260b4b165bd3d44053827f3ce
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380035"
---
# <a name="get-trending-videos"></a>Obtener vídeos de tendencia  

Para obtener vídeos populares de hoy en día, envíe la solicitud GET siguiente:  
  
```
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/trending?mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

Los mercados siguientes admiten vídeos populares.  
 
-   en-AU (inglés, Australia)  
-   en-CA (inglés, Canadá)  
-   en-GB (inglés, Gran Bretaña)  
-   en-ID (inglés, Indonesia)  
-   en-IE (inglés, Irlanda)  
-   en-IN (inglés, India)  
-   en-NZ (inglés, Nueva Zelanda)  
-   en-PH (inglés, Filipinas)  
-   en-SG (inglés, Singapur)  
-   en-US (inglés, Estados Unidos)  
-   en-WW (inglés, código agregado internacional)  
-   en-ZA (inglés, Sudáfrica)  
-   zh-CN (chino, China)

  
En el ejemplo siguiente se muestra una respuesta que contiene vídeos populares.  

```  
{  
    "_type" : "TrendingVideos",  
    "bannerTiles" : [
        {  
            "query" : {  
                "text" : "Hello - Smith",  
                "displayText" : "Hello - Smith",  
                "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3E8F5..."
            },  
            "image" : {  
                "description" : "Image from: contosowallpapers.com",  
                "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=RsA%2fdPlTmx4zS...",  
                "headLine" : "\"Hello\" is a song by...",  
                "contentUrl" : "http:\/\/www.contosowallpapers.com\/wp-content..."  
            }  
        },  
        . . .  
    ],  
    "categories" : [
        {  
            "title" : "Music videos",  
            "subcategories" : [
                {  
                    "tiles" : [
                        {  
                            "query" : {  
                                "text" : "Song Title - Artist Name",  
                                "displayText" : "Song Title - Artist Name",  
                                "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3E8F5..."
                            },  
                            "image" : {  
                                "description" : "Image from: contoso.com",  
                                "thumbnailUrl" : "https:\/\/tse2.mm.bing.net\/th?id=...",  
                                "contentUrl" : "http:\/\/images6.contoso.com\/image..."  
                            }  
                        },  
                        . . .  
                    ],
                    "title" : "Top "  
                },
                {
                    "tiles" : [...],
                    "title" : "Trending"
                },
                . . .
            ],  
        },
        {
            "title" : "Viral videos",
            "subcategories" : [
                {
                    "tiles" : [...],
                    "title" : "Trending"
                },
                . . .
            ],  
        },
        . . .  
    ]  
}  
  
```  
La respuesta contiene una lista de vídeos por categoría y subcategoría. Por ejemplo, si la lista de categorías contiene una categoría Music Videos y una de sus subcategorías es Top, puede crear una categoría Top Music Videos en la experiencia de usuario. A continuación, puede usar los campos `thumbnailUrl`, `displayText` y `webSearchUrl` para crear un icono en el que se pueda hacer clic en cada categoría (por ejemplo, Top Music Videos). Cuando el usuario haga clic en un icono, irá a un explorador de vídeos de Bing donde se reproducirá el vídeo.

La respuesta también contiene vídeos de banner, que son los vídeos más populares. Los vídeos de banner pueden proceder de una o varias de las categorías.  
  
