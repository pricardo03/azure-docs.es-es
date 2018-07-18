---
title: Puntos de conexión de Video Search | Microsoft Docs
description: Resumen del punto de conexión de Video Search API.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: article
ms.date: 12/04/2017
ms.author: v-gedod
ms.openlocfilehash: 9836d9928362ab37b0a81ff5043d99f9bf353f22
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380090"
---
# <a name="video-search-endpoints"></a>Puntos de conexión de Video Search
**Video Search API** incluye tres puntos de conexión.  El punto de conexión 1 devuelve vídeos de la web en función de una consulta. El punto de conexión 2 devuelve información detallada sobre un vídeo en función del parámetro de dirección URL `modules`.  El punto de conexión 3 devuelve imágenes de tendencia.

## <a name="endpoints"></a>Puntos de conexión
Para obtener resultados de vídeo con la API de Bing, envíe una solicitud `GET` a uno de los siguientes puntos de conexión. Use los encabezados y parámetros de dirección URL para definir especificaciones adicionales.

**Punto de conexión 1:** devuelve vídeos relacionados con la consulta de búsqueda del usuario definida por `?q=""`.
``` 
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search
```

**Punto de conexión 2:** devuelve información detallada sobre un vídeo, como los vídeos relacionados. Incluya el [parámetro de consulta](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#query-parameters) `modules`, que es una lista delimitada por comas de información detallada para solicitar.
``` 
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/details
```

**Punto de conexión 3:** devuelve vídeos que son tendencia en función de las solicitudes de búsqueda que realizan otros usuarios. Los vídeos se dividen en categorías diferentes, por ejemplo, en función de personas o eventos destacables.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/trending
```

Para más información sobre encabezados, parámetros, códigos de mercado, objetos de respuesta, errores etc., consulte la referencia [Bing Video Search API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference).
## <a name="response-json"></a>JSON de respuesta
La respuesta a una solicitud de búsqueda de vídeos incluye los resultados como objetos JSON. Para obtener ejemplos de análisis de los resultados, consulte el [tutorial](https://docs.microsoft.com/azure/cognitive-services/bing-video-search/tutorial-bing-video-search-single-page-app) y el [código fuente](https://docs.microsoft.com/azure/cognitive-services/bing-video-search/tutorial-bing-video-search-single-page-app-source).

## <a name="next-steps"></a>Pasos siguientes
Las API de **Bing** admiten acciones de búsqueda que devuelven resultados según su tipo. Todos los puntos de conexión de búsqueda devuelven resultados como objetos de respuesta JSON.  Todos los puntos de conexión admiten consultas que devuelven un idioma o una ubicación en concreto por longitud, latitud y radio de búsqueda.

Para una información completa acerca de los parámetros admitidos por cada punto de conexión, consulte las páginas de referencia de cada tipo.
Para obtener ejemplos de solicitudes básicas mediante Video Search API, consulte los [inicios rápidos de Video Search](https://docs.microsoft.com/azure/cognitive-services/bing-video-search).