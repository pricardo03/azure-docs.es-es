---
title: 'Cómo paginar por los vídeos disponibles: Bing Video Search'
titlesuffix: Azure Cognitive Services
description: Obtenga información sobre cómo paginar a través de todos los vídeos devueltos por Bing Video Search API.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: scottwhi
ms.openlocfilehash: d75a71fe5ba5a275aa8703bc59398cc8a7a5a622
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55867144"
---
# <a name="paging-through-video-search-results"></a>Paginación de los resultados de la búsqueda de vídeos

Bing Video Search API devuelve un subconjunto de todos los resultados de la búsqueda que se encuentren para la consulta. Al paginar estos resultados con las llamadas posteriores a la API, puede obtenerlos y mostrarlos en la aplicación.

> [!NOTE]
> La paginación se aplica solo a la búsqueda de vídeos (/videos/search), y no a la información detallada de vídeos (/videos/details) o a vídeos populares (/videos/trending).

## <a name="total-estimated-matches"></a>Total de coincidencias estimadas

Para obtener el número estimado de resultados de la búsqueda que se encuentran, use el campo [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videos-totalestimatedmatches) en la respuesta JSON.   
  
```json  
{
    "_type" : "Videos",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7545D56...",
    "totalEstimatedMatches" : 1000,
    "value" : [...]
}  
```  
  
## <a name="paging-through-videos"></a>Paginación por los vídeos

Para paginar los vídeos disponibles, use los parámetros de consulta [count](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#count) y [offset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#offset) al enviar la solicitud.  
  

|Parámetro  |DESCRIPCIÓN  |
|---------|---------|
|`count`     | Especifica el número de resultados que se devolverán en la respuesta. El número máximo de resultados que se pueden solicitar en la respuesta es 100. El valor predeterminado es 10. El número real entregado puede ser menor que el solicitado.        |
|`offset`     | Especifica el número de resultados que se van a omitir. `offset` está basado en cero y debe ser menor que (`totalEstimatedMatches` - `count`).          |

Por ejemplo, si quiere mostrar 20 artículos por página, debe establecer `count` en 20 y `offset` en 0 para obtener la primera página de resultados. Con todas las páginas posteriores, se incrementaría `offset` en 20 (por ejemplo, 20, 40).  
  
En el ejemplo siguiente se solicitan 20 vídeos, a partir del desplazamiento 40.  
  
```cURL  
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies&count=20&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

Si usa el valor predeterminado para [count](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#count), tan solo deberá especificar el parámetro de consulta `offset`, como se muestra en el siguiente ejemplo.  
  
```cURL  
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

Si pagina 35 vídeos a la vez, establecería el parámetro de consulta `offset` en 0 en la primera solicitud y, después, incrementaría `offset` en 35 en cada solicitud subsiguiente. Sin embargo, algunos resultados en la siguiente respuesta pueden contener resultados de vídeo duplicados de la respuesta anterior. Por ejemplo, los dos primeros vídeos de una respuesta pueden ser los mismos que los dos últimos vídeos de la respuesta anterior.

Para eliminar resultados duplicados, use el campo [nextOffset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videos-nextoffset) del objeto `Videos`.

Por ejemplo, si desea paginar 30 vídeos a la vez, puede establecer `count` en 30 y `offset` en 0 en la primera solicitud. En la siguiente solicitud, establecería el parámetro de consulta `offset` en el valor `nextOffset`.

> [!NOTE]
> El campo `TotalEstimatedAnswers` es una estimación del número total de resultados de la búsqueda que puede recuperar de la consulta actual.  Al establecer los parámetros `count` y `offset`, el número `TotalEstimatedAnswers` puede cambiar. 
  
## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
[Obtención de información de vídeos](video-insights.md)
