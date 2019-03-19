---
title: Navegación por las páginas de las imágenes que devuelve Bing Image Search API
titleSuffix: Azure Cognitive Services
description: Navegue por distintas páginas de las imágenes que devuelve Bing Image Search API.
services: cognitive-services
author: swhite-msft
manager: cgonlun
ms.assetid: 3C8423F8-41E0-4F89-86B6-697E840610A7
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: scottwhi
ms.custom: seodec2018
ms.openlocfilehash: 38b2244d68de25f53d59dd4eb0a6beba03f0e51d
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2019
ms.locfileid: "57339545"
---
# <a name="page-through-the-images-results"></a>Navegue por las páginas de resultados de imagen.

Cuando se llama a Bing Image Search API, Bing devuelve una lista de resultados. La lista es un subconjunto del número total de resultados que están relacionados con la consulta. Para obtener el número total estimado de resultados disponibles, consulte el campo [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#totalestimatedmatches) del objeto de respuesta.  

En el siguiente ejemplo se muestra el campo `totalEstimatedMatches` que incluye una respuesta de imágenes.  

```json
{
    "_type" : "Images",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=73118C8...",
    "totalEstimatedMatches" : 838,
    "value" : [...]  
}  
```  

Para paginar por las imágenes disponibles, use los parámetros de consulta [count](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#count) y [offset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#offset).  

El parámetro `count` especifica el número de resultados que se van a devolver en la respuesta. El número máximo de resultados que se pueden solicitar en la respuesta es 150. El valor predeterminado es 35. El número real entregado puede ser menor que el solicitado.

El parámetro `offset` especifica el número de resultados que se van a omitir. `offset` está basado en cero y debe ser menor que (`totalEstimatedMatches` - `count`).  

Si quiere mostrar 20 imágenes por página, debe establecer `count` en 20 y `offset` en 0 para obtener la primera página de resultados. A continuación, se muestra un ejemplo que solicita 20 imágenes, comenzando en el desplazamiento 40.  

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies&count=20&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

Si el valor predeterminado `count` es válido para su implementación, solo debe especificar el parámetro de consulta `offset`.  

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

En general, si pagina 35 imágenes a la vez, establece el parámetro de consulta `offset` en 0 en la primera solicitud y, después, incrementa `offset` en 35 en cada solicitud subsiguiente. Sin embargo, algunos de los resultados de la respuesta subsiguiente pueden ser duplicados de la respuesta anterior. Por ejemplo, las dos primeras imágenes de la respuesta pueden ser las mismas que las dos últimas imágenes de la respuesta anterior.

Para eliminar resultados duplicados, use el campo [nextOffset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#nextoffset) del objeto `Images`. El campo `nextOffset` le indica el objeto `offset` que debe usar para la solicitud siguiente. Por ejemplo, si desea paginar 30 imágenes a la vez, establece `count` en 30 y `offset` en 0 en la primera solicitud. En la siguiente solicitud, establece `count` en 30 y `offset` en el valor `nextOffset` de la respuesta anterior. Para paginar hacia atrás, se recomienda mantener una pila de los desplazamientos anteriores y retirar el más reciente.

> [!NOTE]
> La paginación se aplica solo a la búsqueda de imágenes (/images/search), y no a la información detallada de imágenes o a imágenes populares (/images/trending).

> [!NOTE]
> El campo `TotalEstimatedAnswers` es una estimación del número total de resultados de la búsqueda que puede recuperar de la consulta actual.  Al establecer los parámetros `count` y `offset`, el número `TotalEstimatedAnswers` puede cambiar. 
