---
title: 'Inicio rápido: Envío de consultas de búsqueda con la API REST de Bing Image Search API en Python'
description: En esta guía de inicio rápido, va a utilizar Python para enviar consultas de búsqueda a Bing Search API y obtener una lista de imágenes pertinentes.
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 9/21/2017
ms.author: v-jerkin
ms.openlocfilehash: bc527ba39b580935f113f56aa63f7bdd283ba304
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2018
ms.locfileid: "41931336"
---
# <a name="quickstart-send-search-queries-using-the-rest-api-and-python"></a>Guía de inicio rápido: Envío de consultas de búsqueda con la API REST y Python

Bing Image Search API proporciona una experiencia similar a Bing.com/Images al permitir enviar una consulta de búsqueda de usuario a Bing y obtener una lista de imágenes apropiadas.

Este tutorial muestra un ejemplo sencillo de llamada a Bing Image Search API y procesamiento posterior del objeto JSON resultante. Para obtener más información, consulte la [documentación de Bing Image Search](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference).

Para ejecutar este ejemplo como Jupyter Notebook en [MyBinder](https://mybinder.org), puede hacer clic en el distintivo launch Binder: 

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingImageSearchAPI.ipynb)

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="running-the-walkthrough"></a>Ejecución del tutorial
Para continuar con el tutorial, establezca `subscription_key` en la clave de la API para el servicio de la API de Bing.


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

A continuación, se puede usar la biblioteca `PIL` para descargar las imágenes en miniatura y la biblioteca `matplotlib` para representarlas en una cuadrícula de 4 por 4.


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
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial de aplicación de una sola página de Bing Image Search](../tutorial-bing-image-search-single-page-app.md)

## <a name="see-also"></a>Otras referencias 

[¿Qué es Bing Image Search?](../overview.md)  
[Pruébelo](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
[Obtenga una clave de acceso para la evaluación gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)  
[Referencia de Bing Image Search API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)
