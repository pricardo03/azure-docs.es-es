---
title: 'Guía de inicio rápido: Búsqueda de imágenes: API de REST Bing Image Search y Python'
titleSuffix: Azure Cognitive Services
description: Use esta guía de inicio rápido para enviar solicitudes de búsqueda de imágenes a la API de REST Bing Image Search mediante Python y reciba respuestas JSON.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: quickstart
ms.date: 8/20/2018
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 018f2b4b3f75ed9cef3fbc63ac4e93b56a756e14
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55174043"
---
# <a name="quickstart-search-for-images-using-the-bing-image-search-rest-api-and-python"></a>Guía de inicio rápido: Búsqueda de imágenes con la API de REST Bing Image Search y Python

Use esta guía de inicio rápido para realizar la primera llamada a Bing Image Search API y recibir una respuesta JSON. Esta sencilla aplicación de Python envía una consulta de búsqueda a la API y muestra los resultados sin formato.

Si bien esta aplicación está escrita en Python, la API es un servicio web RESTful compatible con la mayoría de los lenguajes de programación.

Para ejecutar este ejemplo como Jupyter Notebook en [MyBinder](https://mybinder.org), puede hacer clic en el distintivo launch Binder:

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingImageSearchAPI.ipynb)


Además, el código fuente del ejemplo está disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingImageSearchv7.py).

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

Consulte también [Precios de Cognitive Services - Bing Search API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="running-the-quickstart"></a>Ejecución del inicio rápido

Para empezar a trabajar, establezca `subscription_key` en una clave de suscripción válida para el servicio Bing API.

```python
subscription_key = None
assert subscription_key
```

A continuación, compruebe que el punto de conexión `search_url` sea correcto. En este documento, se utiliza un único punto de conexión Bing Search APIs. Si se producen errores de autorización, vuelva a comprobar este valor frente al punto de conexión de búsqueda de Bing en el panel de Azure.


```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/images/search"
```

Establezca `search_term` para buscar imágenes de “puppies”.


```python
search_term = "puppies"
```

El siguiente bloque usa la biblioteca `requests` de Python para llamar a Bing Search APIs y devolver los resultados como objeto JSON. Observe que se pasa la clave de API a través del diccionario `headers`, y el término de búsqueda a través del diccionario `params`. Para ver la lista completa de opciones que puede usarse para filtrar los resultados de la búsqueda, consulte la documentación de la [API de REST](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference).


```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "license": "public", "imageType": "photo"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

El objeto `search_results` contiene las imágenes reales, junto con metadatos enriquecidos como elementos relacionados. Por ejemplo, la siguiente línea de código puede extraer las direcciones URL de las miniaturas de los 16 primeros resultados.


```python
thumbnail_urls = [img["thumbnailUrl"] for img in search_results["value"][:16]]
```

A continuación, use la biblioteca `PIL` para descargar las imágenes en miniatura y la biblioteca `matplotlib` para representarlas en una cuadrícula de 4 por 4.


```python
%matplotlib inline
import matplotlib.pyplot as plt
from PIL import Image
from io import BytesIO

f, axes = plt.subplots(4, 4)
for i in range(4):
    for j in range(4):
        image_data = requests.get(thumbnail_urls[i+4*j])
        image_data.raise_for_status()
        image = Image.open(BytesIO(image_data.content))        
        axes[i][j].imshow(image)
        axes[i][j].axis("off")
plt.show()
```

## <a name="sample-json-response"></a>Respuesta JSON de ejemplo

Las respuestas de Bing Image Search API se devuelven como JSON. Esta respuesta de ejemplo se ha truncado para mostrar un único resultado.

```json
{
"_type":"Images",
"instrumentation":{
    "_type":"ResponseInstrumentation"
},
"readLink":"images\/search?q=tropical ocean",
"webSearchUrl":"https:\/\/www.bing.com\/images\/search?q=tropical ocean&FORM=OIIARP",
"totalEstimatedMatches":842,
"nextOffset":47,
"value":[
    {
        "webSearchUrl":"https:\/\/www.bing.com\/images\/search?view=detailv2&FORM=OIIRPO&q=tropical+ocean&id=8607ACDACB243BDEA7E1EF78127DA931E680E3A5&simid=608027248313960152",
        "name":"My Life in the Ocean | The greatest WordPress.com site in ...",
        "thumbnailUrl":"https:\/\/tse3.mm.bing.net\/th?id=OIP.fmwSKKmKpmZtJiBDps1kLAHaEo&pid=Api",
        "datePublished":"2017-11-03T08:51:00.0000000Z",
        "contentUrl":"https:\/\/mylifeintheocean.files.wordpress.com\/2012\/11\/tropical-ocean-wallpaper-1920x12003.jpg",
        "hostPageUrl":"https:\/\/mylifeintheocean.wordpress.com\/",
        "contentSize":"897388 B",
        "encodingFormat":"jpeg",
        "hostPageDisplayUrl":"https:\/\/mylifeintheocean.wordpress.com",
        "width":1920,
        "height":1200,
        "thumbnail":{
        "width":474,
        "height":296
        },
        "imageInsightsToken":"ccid_fmwSKKmK*mid_8607ACDACB243BDEA7E1EF78127DA931E680E3A5*simid_608027248313960152*thid_OIP.fmwSKKmKpmZtJiBDps1kLAHaEo",
        "insightsMetadata":{
        "recipeSourcesCount":0,
        "bestRepresentativeQuery":{
            "text":"Tropical Beaches Desktop Wallpaper",
            "displayText":"Tropical Beaches Desktop Wallpaper",
            "webSearchUrl":"https:\/\/www.bing.com\/images\/search?q=Tropical+Beaches+Desktop+Wallpaper&id=8607ACDACB243BDEA7E1EF78127DA931E680E3A5&FORM=IDBQDM"
        },
        "pagesIncludingCount":115,
        "availableSizesCount":44
        },
        "imageId":"8607ACDACB243BDEA7E1EF78127DA931E680E3A5",
        "accentColor":"0050B2"
    }
}
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial de aplicación de una sola página de Bing Image Search](../tutorial-bing-image-search-single-page-app.md)

## <a name="see-also"></a>Otras referencias

* [¿Qué es Bing Image Search?](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [Prueba de una demostración interactiva en línea](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
* [Obtención de una clave de acceso de Cognitive Services gratis](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)  
* [Documentación de Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services)
* [Referencia de Bing Image Search API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)
