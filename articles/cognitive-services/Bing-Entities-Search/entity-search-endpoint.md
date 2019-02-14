---
title: Punto de conexión de Bing Entity Search API
titlesuffix: Azure Cognitive Services
description: Más información sobre el punto de conexión de Bing Entity Search API y envío de solicitudes a este.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: v-gedod
ms.openlocfilehash: 3c2aa4b22c8e679f73692978d9e1f8009f11a46b
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55875236"
---
# <a name="bing-entity-search-api-endpoint"></a>Punto de conexión de Bing Entity Search API


Bing Entity Search API incluye un punto de conexión que devuelve las entidades desde la Web basándose en una consulta. Estos resultados de búsqueda se devuelven en formato JSON.

## <a name="get-entity-results-from-the-endpoint"></a>Obtención de resultados de entidad a partir del punto de conexión

Para obtener resultados de entidades mediante **Bing API**, envíe una solicitud `GET` al punto de conexión siguiente. Use [encabezados](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#headers) y [parámetros de consulta](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#query-parameters) para personalizar la solicitud de búsqueda. Se pueden enviar solicitudes de búsqueda mediante el parámetro `?q=`.

```cURL
 GET https://api.cognitive.microsoft.com/bing/v7.0/entities
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [¿Qué es Bing Entity Search API?](overview.md)

## <a name="see-also"></a>Otras referencias 

Para más información sobre encabezados, parámetros, códigos de mercado, objetos de respuesta, errores etc, consulte la referencia sobre [Bing Entity Search API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference).
