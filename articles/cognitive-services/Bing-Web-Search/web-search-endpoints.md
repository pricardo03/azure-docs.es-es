---
title: Punto de conexión de Web Search
titleSuffix: Azure Cognitive Services
description: Resumen del punto de conexión de Web Search API.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 11/28/2017
ms.author: v-gedod
ms.openlocfilehash: 3058ca6cf0eb99486dd4c269d43b274fb367f7a9
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "46125466"
---
# <a name="web-search-endpoint"></a>Punto de conexión de Web Search
**Web Search API** devuelve páginas web, noticias, imágenes, vídeos y [entidades](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/search-the-web). Las entidades contienen información de resumen sobre una persona, lugar o tema.
## <a name="endpoint"></a>Punto de conexión
Para obtener resultados de búsqueda web mediante la API de Bing, envíe una solicitud `GET` al punto de conexión siguiente. Los encabezados y parámetros de dirección URL definen especificaciones adicionales.

**Punto de conexión**: devuelve resultados web relacionados con la consulta de búsqueda del usuario definida por `?q=""`.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/search
```
Punto de conexión: para más información sobre encabezados, parámetros, códigos de mercado, objetos de respuesta, errores etc., consulte la referencia [Bing Web API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference).

## <a name="response-json"></a>JSON de respuesta
La respuesta a una solicitud de búsqueda web incluye todos los resultados como objetos JSON. Para analizar el resultado se necesitan procedimientos que controlen los elementos de cada tipo. Consulte el [tutorial](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/tutorial-bing-web-search-single-page-app) y el [código fuente](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/tutorial-bing-web-search-single-page-app-source) para obtener ejemplos.

## <a name="next-steps"></a>Pasos siguientes
Las API de **Bing** admiten acciones de búsqueda que devuelven resultados según su tipo. Todos los puntos de conexión de búsqueda devuelven resultados como objetos de respuesta JSON.  Todos los puntos de conexión admiten consultas que devuelven un idioma o una ubicación en concreto por longitud, latitud y radio de búsqueda.

Para una información completa acerca de los parámetros admitidos por cada punto de conexión, consulte las páginas de referencia de cada tipo.
Para obtener ejemplos de solicitudes básicas mediante Web Search API, consulte los [inicios rápidos de búsqueda en la Web](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/search-the-web).
