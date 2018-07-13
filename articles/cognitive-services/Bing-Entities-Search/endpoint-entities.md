---
title: Punto de conexión de Entity Search| Microsoft Docs
description: Resumen del punto de conexión de Entity Search API.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: article
ms.date: 12/05/2017
ms.author: v-gedod
ms.openlocfilehash: a2557c6000445544b3b47a05d7d356ccaa9928b4
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380026"
---
# <a name="entity-search-endpoint"></a>Punto de conexión de Entity Search
**Entity Search API** incluye un punto de conexión que devuelve las entidades desde la Web basándose en una consulta.

## <a name="endpoint"></a>Punto de conexión
Para obtener resultados de entidades mediante **Bing API**, envíe una solicitud `GET` al punto de conexión siguiente. Use los encabezados y parámetros de dirección URL para definir especificaciones adicionales.

**Punto de conexión**: devuelve entidades relacionadas con la consulta de búsqueda del usuario definida por `?q=""`.
```
 GET https://api.cognitive.microsoft.com/bing/v7.0/entities
```

Para más información sobre encabezados, parámetros, códigos de mercado, objetos de respuesta, errores etc., consulte la referencia [Bing Entity Search API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference).

## <a name="response-json"></a>Respuesta JSON
La respuesta a una solicitud de búsqueda de entidades incluye los resultados como objetos JSON. Para obtener ejemplos de los resultados, consulte [Introducción](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/quick-start).

## <a name="next-steps"></a>Pasos siguientes
Las API de **Bing** admiten acciones de búsqueda que devuelven resultados según su tipo. Todos los puntos de conexión de búsqueda devuelven resultados como objetos de respuesta JSON.  Todos los puntos de conexión admiten consultas que devuelven un idioma o una ubicación en concreto por longitud, latitud y radio de búsqueda.

Para una información completa acerca de los parámetros admitidos por cada punto de conexión, consulte las páginas de referencia de cada tipo.
Para ver ejemplos de uso de Entity Search API, consulte [Introducción](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/quick-start) y [Cambio de tamaño y recorte de imágenes en miniatura](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/resize-and-crop-thumbnails).