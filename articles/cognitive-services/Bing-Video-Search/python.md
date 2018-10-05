---
title: 'Guía de inicio rápido: Bing Video Search, Python'
titlesuffix: Azure Cognitive Services
description: Obtenga información y ejemplos de código que le ayuden a empezar a usar rápidamente Bing Video Search API.
services: cognitive-services
author: v-jerkin
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: quickstart
ms.date: 9/21/2017
ms.author: v-jerkin
ms.openlocfilehash: 797eb476aa3386949b08efb957edf48a97e40d6b
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47220029"
---
# <a name="quickstart-bing-video-search-api-with-python"></a>Guía de inicio rápido: Bing Video Search API con Python

En este tutorial se detalla cómo usar Bing Video Search API, que forma parte de Microsoft Cognitive Services en Azure. Consulte la [referencia de la API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference) para obtener detalles técnicos acerca de las API.

Para ejecutar este ejemplo como Jupyter Notebook en [MyBinder](https://mybinder.org), puede hacer clic en el distintivo launch Binder: 

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingVideoSearchAPI.ipynb)


## <a name="prerequisites"></a>Requisitos previos
Debe tener una [cuenta de Cognitive Services API](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con **Bing Search APIs**. La [cuenta de evaluación gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) es suficiente para esta guía de inicio rápido. Necesita la clave de acceso que se le proporciona al activar la versión de evaluación gratuita, o puede usar una clave de suscripción de pago desde su panel de Azure.

## <a name="running-the-walkthrough"></a>Ejecución del tutorial

En primer lugar, establezca `subscription_key` como la clave de API para el servicio de la API de Bing.


```python
subscription_key = None
assert subscription_key
```

A continuación, compruebe que el punto de conexión `search_url` sea correcto. En este documento, se utiliza un único punto de conexión Bing Search APIs. Si se producen errores de autorización, vuelva a comprobar este valor frente al punto de conexión de búsqueda de Bing en el panel de Azure.


```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/videos/search"
```

Configuración de `search_term` para buscar vídeos de gatitos


```python
search_term = "kittens"
```

El siguiente bloque usa la biblioteca `requests` de Python para llamar a Bing Search APIs y devolver los resultados como objeto JSON. Observe que se pasa la clave de API a través del diccionario `headers`, y el término de búsqueda a través del diccionario `params`. Para ver la lista completa de opciones que puede usarse para filtrar los resultados de la búsqueda, consulte la documentación de la [API REST](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference).


```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "count":5, "pricing": "free", "videoLength":"short"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

El objeto `search_results` contiene los vídeos pertinentes junto con eficaces metadatos. Para ver uno de los vídeos, use su propiedad `embedHtml` e insértela en un `IFrame`.


```python
from IPython.display import HTML
HTML(search_results["value"][0]["embedHtml"].replace("autoplay=1","autoplay=0"))
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Paginación de vídeos](paging-videos.md)
> [Cambio de tamaño y recorte de miniaturas](resize-and-crop-thumbnails.md)

## <a name="see-also"></a>Otras referencias 

 [Búsqueda de vídeos en la web](search-the-web.md) [Probarlo](https://azure.microsoft.com/services/cognitive-services/bing-video-search-api/)
