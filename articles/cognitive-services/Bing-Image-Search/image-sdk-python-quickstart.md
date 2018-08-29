---
title: 'Guía de inicio rápido: Solicitud y filtrado de imágenes mediante el SDK en Python'
description: En esta guía de inicio rápido, solicita y filtra las imágenes devueltas por Bing Image Search, con Python.
titleSuffix: Azure Image Search SDK Python quickstart
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 02/14/2018
ms.author: v-gedod
ms.openlocfilehash: 4729f103bb9b50d4ff039907db8eb677f3dc290a
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2018
ms.locfileid: "41936748"
---
 # <a name="quickstart-request-and-filter-images-using-the-sdk-and-python"></a>Guía de inicio rápido: Solicitud y filtrado de imágenes mediante el SDK y Python

El SDK de Bing Image Search contiene la funcionalidad de la API REST para consultas de vídeos y análisis de resultados. 

El [código fuente de los ejemplos del SDK de Bing Image Search para Python](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/image_search_samples.py) está disponible en GitHub.

## <a name="application-dependencies"></a>Dependencias de aplicaciones
Si aún no lo tiene, instale Python. El SDK es compatible con Python 2.7, 3.3, 3.4, 3.5 y 3.6.

La recomendación general para el desarrollo de Python es usar un [entorno virtual](https://docs.python.org/3/tutorial/venv.html). Instale e inicialice el entorno virtual con el [módulo venv](https://pypi.python.org/pypi/virtualenv). Debe instalar virtualenv para Python 2.7.
```
python -m venv mytestenv
```
Instale las dependencias del SDK de Bing Image Search:
```
cd mytestenv
python -m pip install azure-cognitiveservices-search-imagesearch
```
## <a name="image-search-client"></a>Cliente de Image Search
Obtenga una [clave de acceso de Cognitive Services](https://azure.microsoft.com/try/cognitive-services/) en *Buscar*. Agregar importaciones:
```
from azure.cognitiveservices.search.imagesearch import ImageSearchAPI
from azure.cognitiveservices.search.imagesearch.models import ImageType, ImageAspect, ImageInsightModule
from msrest.authentication import CognitiveServicesCredentials

subscription_key = "YOUR-SUBSCRIPTION-KEY"
```
Cree una instancia de la `CognitiveServicesCredentials` y una instancia del cliente:
```
client = ImageSearchAPI(CognitiveServicesCredentials(subscription_key))
```
Busque imágenes en la consulta (Yosemite) filtradas para imágenes GIF animadas y formato panorámico. Compruebe el número de resultados e imprima insightsToken, la dirección URL en miniatura y la URL web del primer resultado.
```
image_results = client.images.search(
        query="Yosemite",
        image_type=ImageType.animated_gif, # Could be the str "AnimatedGif"
        aspect=ImageAspect.wide # Could be the str "Wide"
    )
    print("\r\nSearch images for \"Yosemite\" results that are animated gifs and wide aspect")

    if image_results.value:
        first_image_result = image_results.value[0]
        print("Image result count: {}".format(len(image_results.value)))
        print("First image insights token: {}".format(first_image_result.image_insights_token))
        print("First image thumbnail url: {}".format(first_image_result.thumbnail_url))
        print("First image web search url: {}".format(first_image_result.web_search_url))
    else:
        print("Couldn't find image results!")

```
Busque imágenes para (Yosemite) filtradas para imágenes GIF animadas y formato panorámico.  Compruebe el número de resultados.  Imprima `insightsToken`, `thumbnail url` y `web url` del primer resultado.
```
image_results = client.images.search(
    query="Yosemite",
    image_type=ImageType.animated_gif, # Could be the str "AnimatedGif"
    aspect=ImageAspect.wide # Could be the str "Wide"
)
print("\r\nSearch images for \"Yosemite\" results that are animated gifs and wide aspect")

if image_results.value:
    first_image_result = image_results.value[0]
    print("Image result count: {}".format(len(image_results.value)))
    print("First image insights token: {}".format(first_image_result.image_insights_token))
    print("First image thumbnail url: {}".format(first_image_result.thumbnail_url))
    print("First image web search url: {}".format(first_image_result.web_search_url))
else:
    print("Couldn't find image results!")

```

Obtención de resultados de tendencia:
```
trending_result = client.images.trending()
print("\r\nSearch trending images")

# Categorires
if trending_result.categories:
    first_category = trending_result.categories[0]
    print("Category count: {}".format(len(trending_result.categories)))
    print("First category title: {}".format(first_category.title))
    if first_category.tiles:
        first_tile = first_category.tiles[0]
        print("Subcategory tile count: {}".format(len(first_category.tiles)))
        print("First tile text: {}".format(first_tile.query.text))
        print("First tile url: {}".format(first_tile.query.web_search_url))
    else:
        print("Couldn't find subcategory tiles!")
    else:
        print("Couldn't find categories!")

```

## <a name="next-steps"></a>Pasos siguientes

[Ejemplos del SDK de Cognitive Services para Python](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)


