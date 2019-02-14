---
title: Navegación por las páginas de resultados de Bing News Search
titlesuffix: Azure Cognitive Services
description: Obtenga información sobre cómo navegar por las páginas de los artículos de noticias que devuelve Bing News Search API.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: scottwhi
ms.openlocfilehash: 1d344f388b03acb3a81fcfde0e214eb7d82dc9b9
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55885079"
---
# <a name="how-to-page-through-news-search-results"></a>Navegación por las páginas de resultados de búsqueda de noticias

Cuando se llama a News Search API, Bing devuelve una lista de resultados pertinentes para la consulta. Para obtener el número total estimado de resultados disponibles, consulte el campo [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#news-totalmatches) del objeto de respuesta.  
  
En el siguiente ejemplo se muestra el campo `totalEstimatedMatches` que incluye una respuesta de noticias.  

```json
{  
    "_type" : "News",  
    "readLink" : "https:\/\/api.cognitive.microsoft.com\/bing\/v7\/news\/search?q=sailing+dinghies",  
    "totalEstimatedMatches" : 88400,  
    "value" : [...]  
}  
```  
  
Para paginar a través de los artículos disponibles, use los parámetros de consulta [count](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#count) y [offset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#offset).  
 

|Parámetro  |DESCRIPCIÓN  |
|---------|---------|
|`count`     | Especifica el número de resultados que se devolverán en la respuesta. El número máximo de resultados que se pueden solicitar en la respuesta es 100. El valor predeterminado es 10. El número real entregado puede ser menor que el solicitado.        |
|`offset`     | Especifica el número de resultados que se van a omitir. `offset` está basado en cero y debe ser menor que (`totalEstimatedMatches` - `count`).          |

Por ejemplo, si quiere mostrar 20 artículos por página, debe establecer `count` en 20 y `offset` en 0 para obtener la primera página de resultados. Con todas las páginas posteriores, se incrementaría `offset` en 20 (por ejemplo, 20, 40).  
  
En el ejemplo siguiente se solicitan 20 artículos de noticias a partir del desplazamiento 40.  

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

> [!NOTE]
> El campo `TotalEstimatedAnswers` es una estimación del número total de resultados de la búsqueda que puede recuperar de la consulta actual.  Al establecer los parámetros `count` y `offset`, el número `TotalEstimatedAnswers` puede cambiar. 
