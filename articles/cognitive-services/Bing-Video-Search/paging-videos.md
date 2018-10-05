---
title: 'Cómo paginar por los vídeos disponibles: Bing Video Search'
titlesuffix: Azure Cognitive Services
description: Se muestra cómo paginar por todos los vídeos que puede devolver Bing.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: conceptual
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 9b030312c562d1c0a6cbacfc7f424289dee2e8de
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47225572"
---
# <a name="paging-videos"></a>Paginación de vídeos

Cuando se llama a Video Search API, Bing devuelve una lista de resultados. La lista es un subconjunto del número total de resultados que están relacionados con la consulta. Para obtener el número total estimado de resultados disponibles, consulte el campo [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videos-totalestimatedmatches) del objeto de respuesta.  
  
En el siguiente ejemplo se muestra el campo `totalEstimatedMatches` que incluye una respuesta de vídeo.  
  
```  
{
    "_type" : "Videos",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7545D56...",
    "totalEstimatedMatches" : 1000,
    "value" : [...]
}  
```  
  
Para paginar por los vídeos disponibles, use los parámetros de consulta [count](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#count) y [offset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#offset).  
  
El parámetro `count` especifica el número de resultados que se van a devolver en la respuesta. El número máximo de resultados que se pueden solicitar en la respuesta es 105. El valor predeterminado es 35. El número real entregado puede ser menor que el solicitado.

El parámetro `offset` especifica el número de resultados que se van a omitir. `offset` está basado en cero y debe ser menor que (`totalEstimatedMatches` - `count`).  
  
Si quiere mostrar 20 vídeos por página, debe establecer `count` en 20 y `offset` en 0 para obtener la primera página de resultados. Con todas las páginas posteriores, se incrementaría `offset` en 20 (por ejemplo, 20, 40).  

A continuación, se muestra un ejemplo que solicita 20 vídeos, comenzando en el desplazamiento 40.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies&count=20&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

Si el valor predeterminado `count` es válido para su implementación, solo debe especificar el parámetro de consulta `offset`.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

Por lo general, si pagina 35 vídeos a la vez, establece el parámetro de consulta `offset` en 0 en la primera solicitud y, después, incrementa `offset` en 35 en cada solicitud subsiguiente. Sin embargo, algunos de los resultados de la respuesta subsiguiente pueden ser duplicados de la respuesta anterior. Por ejemplo, los dos primeros vídeos de la respuesta pueden ser los mismos que los dos últimos vídeos de la respuesta anterior.

Para eliminar resultados duplicados, use el campo [nextOffset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videos-nextoffset) del objeto `Videos`.

Por ejemplo, si desea paginar 30 vídeos a la vez, establece `count` en 30 y `offset` en 0 en la primera solicitud. En la siguiente solicitud, establece el parámetro de consulta `offset` en el valor `nextOffset`.


> [!NOTE]
> La paginación se aplica solo a la búsqueda de vídeos (/videos/search), y no a la información detallada de vídeos (/videos/details) o a vídeos populares (/videos/trending).