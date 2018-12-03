---
title: 'Guía de inicio rápido: Bing Entity Search API con Python'
titlesuffix: Azure Cognitive Services
description: Obtenga información y ejemplos de código que le ayuden a empezar a usar rápidamente Bing Entity Search API.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: quickstart
ms.date: 11/28/2017
ms.author: aahi
ms.openlocfilehash: 65a1800a4cc07105d2a8d09dae6596c6171a7fb9
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2018
ms.locfileid: "52313510"
---
# <a name="quickstart-for-bing-entity-search-api-with-python"></a>Guía de inicio rápido para Bing Entity Search API con Python

En este artículo se muestra cómo utilizar [Bing Entity Search](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/search-the-web) API con Python.

## <a name="prerequisites"></a>Requisitos previos

Debe tener [Python.js 3.x](https://www.python.org/downloads/) para ejecutar este código.

Debe tener una [cuenta de Cognitive Services API](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con **Bing Entity Search API**. La [cuenta de evaluación gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-entity-search-api) es suficiente para esta guía de inicio rápido. Necesita la clave de acceso que se le proporciona al activar la versión de evaluación gratuita, o puede usar una clave de suscripción de pago desde su panel de Azure.   Consulte también [Precios de Cognitive Services - Bing Search API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="search-entities"></a>Entidades de búsqueda

Para ejecutar esta aplicación, siga estos pasos.

1. Cree un nuevo proyecto de Python en su IDE favorito.
2. Agregue el código que se proporciona a continuación.
3. Reemplace el valor `key` por una clave de acceso válida para la suscripción.
4. Ejecute el programa.

```python
# -*- coding: utf-8 -*-

import http.client, urllib.parse
import json

# **********************************************
# *** Update or verify the following values. ***
# **********************************************

# Replace the subscriptionKey string value with your valid subscription key.
subscriptionKey = 'ENTER KEY HERE'

host = 'api.cognitive.microsoft.com'
path = '/bing/v7.0/entities'

mkt = 'en-US'
query = 'italian restaurants near me'

params = '?mkt=' + mkt + '&q=' + urllib.parse.quote (query)

def get_suggestions ():
    headers = {'Ocp-Apim-Subscription-Key': subscriptionKey}
    conn = http.client.HTTPSConnection (host)
    conn.request ("GET", path + params, None, headers)
    response = conn.getresponse ()
    return response.read ()

result = get_suggestions ()
print (json.dumps(json.loads(result), indent=4))
```

**Respuesta**

Se devuelve una respuesta correcta en JSON, como se muestra en el siguiente ejemplo: 

```json
{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "italian restaurant near me",
    "askUserForLocation": true
  },
  "places": {
    "value": [
      {
        "_type": "LocalBusiness",
        "webSearchUrl": "https://www.bing.com/search?q=sinful+bakery&filters=local...",
        "name": "Liberty's Delightful Sinful Bakery & Cafe",
        "url": "https://www.contoso.com/",
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness"
          ]
        },
        "address": {
          "addressLocality": "Seattle",
          "addressRegion": "WA",
          "postalCode": "98112",
          "addressCountry": "US",
          "neighborhood": "Madison Park"
        },
        "telephone": "(800) 555-1212"
      },

      . . .
      {
        "_type": "Restaurant",
        "webSearchUrl": "https://www.bing.com/search?q=Pickles+and+Preserves...",
        "name": "Munson's Pickles and Preserves Farm",
        "url": "http://www.princi.com/",
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness",
            "Restaurant"
          ]
        },
        "address": {
          "addressLocality": "Seattle",
          "addressRegion": "WA",
          "postalCode": "98101",
          "addressCountry": "US",
          "neighborhood": "Capitol Hill"
        },
        "telephone": "(800) 555-1212"
      },
      
      . . .
    ]
  }
}
```

[Volver arriba](#HOLTop)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial de Bing Entity Search](../tutorial-bing-entities-search-single-page-app.md)
> [Introducción a Bing Entity Search](../search-the-web.md )
> [Referencias de API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference)
