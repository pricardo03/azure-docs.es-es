---
title: Puntos de conexión de Bing News Search | Microsoft Docs
description: Resumen del punto de conexión de News Search API.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: article
ms.date: 11/28/2017
ms.author: v-gedod
ms.openlocfilehash: b8ae99698926a818bf22b0b4027af8610413aaa6
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380098"
---
# <a name="news-search-endpoints"></a>Puntos de conexión de News Search
**News Search API** devuelve artículos de noticias, páginas web, imágenes, vídeos y [entidades](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/search-the-web). Las entidades contienen información de resumen sobre una persona, lugar o tema. 
## <a name="endpoints"></a>Puntos de conexión
Para obtener resultados de News Search con la API de Bing, envíe una solicitud `GET` a uno de los siguientes puntos de conexión. Los encabezados y parámetros de dirección URL definen especificaciones adicionales.

**Punto de conexión 1:** devuelve elementos de noticias según la consulta de búsqueda del usuario. Si la consulta de búsqueda está vacía, la llamada devuelve los artículos de noticias principales. La opción `?q=""` de consulta puede usarse también con la dirección URL `/news`. Para información sobre disponibilidad, consulte los [países y mercados admitidos](supported-countries-markets.md#supported-markets-for-news-search-endpoint).
```
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search 
``` 

**Punto de conexión 2:** devuelve los elementos de noticias principales por categoría. En concreto, puede solicitar los artículos principales sobre negocios, deportes o entretenimiento mediante `category=business`, `category=sports` o `category=entertainment`.  El parámetro `category` solo se puede usar con la dirección URL `/news`. Existen algunos requisitos formales para especificar las categorías; consulte `category` en la documentación de [parámetros de consulta](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#query-parameters). Para información sobre disponibilidad, consulte los [países y mercados admitidos](supported-countries-markets.md#supported-markets-for-news-endpoint). 
```
GET https://api.cognitive.microsoft.com/bing/v7.0/news  
```

**Punto de conexión 3:** devuelve los temas de noticias que actualmente son tendencia en las redes sociales. Cuando se incluye la opción `/trendingtopics`, la búsqueda de Bing omite otros parámetros, como `freshness` y `?q=""`. Para información sobre disponibilidad, consulte los [países y mercados admitidos](supported-countries-markets.md#supported-markets-for-news-trending-endpoint).
```
GET https://api.cognitive.microsoft.com/bing/v7.0/news/trendingtopics 
``` 

Para más información sobre encabezados, parámetros, códigos de mercado, objetos de respuesta, errores etc., consulte la referencia [Bing News Search API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference).
## <a name="response-json"></a>JSON de respuesta
La respuesta a una solicitud de News Search incluye los resultados como objetos JSON. Para analizar los resultados se necesitan procedimientos que controlen los elementos de cada tipo. Consulte el [tutorial](https://docs.microsoft.com/azure/cognitive-services/bing-news-search/tutorial-bing-news-search-single-page-app) y el [código fuente](https://docs.microsoft.com/azure/cognitive-services/bing-news-search/tutorial-bing-news-search-single-page-app-source) para obtener ejemplos.

## <a name="next-steps"></a>Pasos siguientes
Las API de **Bing** admiten acciones de búsqueda que devuelven resultados según su tipo. Todos los puntos de conexión de búsqueda devuelven resultados como objetos de respuesta JSON.  Todos los puntos de conexión admiten consultas que devuelven un idioma o una ubicación en concreto por longitud, latitud y radio de búsqueda.

Para una información completa acerca de los parámetros admitidos por cada punto de conexión, consulte las páginas de referencia de cada tipo.
Para obtener ejemplos de solicitudes básicas con News Search API, consulte los [inicios rápidos de Bing News Search](https://docs.microsoft.com/azure/cognitive-services/bing-news-search).