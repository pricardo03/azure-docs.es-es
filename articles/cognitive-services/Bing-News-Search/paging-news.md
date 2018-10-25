---
title: 'Cómo paginar a través de los artículos de noticias disponibles: Bing News Search'
titlesuffix: Azure Cognitive Services
description: Se muestra cómo paginar a través de todos los artículos de noticias que Bing New Search puede devolver.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: conceptual
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: fff1da15df2e690cd0b37bb82654a4d30159325a
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48803399"
---
# <a name="paging-news"></a>Noticias de Bing

Cuando se llama a News Search API, Bing devuelve una lista de resultados. La lista es un subconjunto del número total de resultados que pueden estar relacionados con la consulta. Para obtener el número total estimado de resultados disponibles, consulte el campo [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#news-totalmatches) del objeto de respuesta.  
  
En el siguiente ejemplo se muestra el campo `totalEstimatedMatches` que incluye una respuesta de noticias.  
  
```  
{  
    "_type" : "News",  
    "readLink" : "https:\/\/api.cognitive.microsoft.com\/bing\/v7\/news\/search?q=sailing+dinghies",  
    "totalEstimatedMatches" : 88400,  
    "value" : [...]  
}  
```  
  
Para paginar a través de los artículos disponibles, use los parámetros de consulta [count](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#count) y [offset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#offset).  
  
El parámetro `count` especifica el número de resultados que se van a devolver en la respuesta. El número máximo de resultados que se pueden solicitar en la respuesta es 100. El valor predeterminado es 10. El número real entregado puede ser menor que el solicitado.

El parámetro `offset` especifica el número de resultados que se van a omitir. `offset` está basado en cero y debe ser menor que (`totalEstimatedMatches` - `count`).  

Si quiere mostrar 20 artículos por página, debe establecer `count` en 20 y `offset` en 0 para obtener la primera página de resultados. Para todas las páginas posteriores, se incrementa `offset` en 20 (por ejemplo, 20, 40).  
  
A continuación, se muestra un ejemplo que solicita 20 artículos de noticias, comenzando en el desplazamiento 40.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search?q=sailing+dinghies&count=20&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  
  
Si el valor predeterminado `count` sirve para su implementación, especifique solo el parámetro de consulta `offset`, como se muestra en el ejemplo siguiente:  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search?q=sailing+dinghies&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  
  
> [!NOTE]
> La paginación se aplica solamente a la búsqueda de noticias (/news/search) y no a temas populares (/news/trendingtopics) o categorías de noticias (/news).