---
title: 'Guía de inicio rápido: Búsqueda de imágenes: SDK de Bing Image Search para Python'
titleSuffix: Azure Cognitive Services
description: Use este inicio rápido para buscar su primera imagen con el SDK de Bing Image Search, un contenedor de la API y que contiene las mismas características. Esta sencilla aplicación de Python envía una consulta de búsqueda de imagen, analiza la respuesta de JSON y muestra la dirección URL de la primera imagen devuelta.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: quickstart
ms.date: 12/06/2019
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 85aa1bda395240d0f11b0654ee48b9f1a0401eaa
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2019
ms.locfileid: "74930592"
---
# <a name="quickstart-search-for-images-with-the-bing-image-search-sdk-for-python"></a>Guía de inicio rápido: Búsqueda de imágenes con el SDK de Bing Image Search para Python

Use este inicio rápido para buscar su primera imagen con el SDK de Bing Image Search, un contenedor de la API y que contiene las mismas características. Esta sencilla aplicación de Python envía una consulta de búsqueda de imagen, analiza la respuesta de JSON y muestra la dirección URL de la primera imagen devuelta.

El código fuente de este ejemplo está disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/image-search-quickstart.py) con anotaciones y control de errores adicionales.

## <a name="prerequisites"></a>Requisitos previos

* [Python 2.7 o 3.4](https://www.python.org/) y versiones posteriores.

* El [SDK de Azure Image Search](https://pypi.org/project/azure-cognitiveservices-search-imagesearch/) para Python
    * Instalación mediante `pip install azure-cognitiveservices-search-imagesearch`

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Creación e inicialización de la aplicación

1. Cree un script de Python en su IDE o editor favorito y realice las siguientes importaciones:

    ```python
    from azure.cognitiveservices.search.imagesearch import ImageSearchAPI
    from msrest.authentication import CognitiveServicesCredentials
    ```

2. Cree variables para el término de búsqueda y la clave de suscripción.

    ```python
    subscription_key = "Enter your key here"
    search_term = "canadian rockies"
    ```

## <a name="create-the-image-search-client"></a>Creación del cliente de Image Search

1. Cree una instancia de `CognitiveServicesCredentials` y úsela para crear una instancia del cliente:

    ```python
    client = ImageSearchAPI(CognitiveServicesCredentials(subscription_key))
    ```
1. Envíe una consulta de búsqueda a Bing Image Search API:
    ```python
    image_results = client.images.search(query=search_term)
    ```
   ## <a name="process-and-view-the-results"></a>Procese y visualice los resultados

Analice los resultados de la imagen devueltos en la respuesta.


Si la respuesta contiene resultados de la búsqueda, almacene el primer resultado e imprima sus detalles, como la dirección URL de una miniatura, la dirección URL original, además del número total de imágenes devueltas.  

```python
if image_results.value:
    first_image_result = image_results.value[0]
    print("Total number of images returned: {}".format(len(image_results.value)))
    print("First image thumbnail url: {}".format(
        first_image_result.thumbnail_url))
    print("First image content url: {}".format(first_image_result.content_url))
else:
    print("No image results returned!")
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial de aplicación de una sola página de Bing Image Search](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app)

## <a name="see-also"></a>Consulte también

* [¿Qué es Bing Image Search?](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [Prueba de una demostración interactiva en línea](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
* [Obtención de una clave de acceso de Cognitive Services gratis](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)
* [Ejemplos de Python para el SDK de Azure Cognitive Services](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)  
* [Documentación de Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services)
* [Referencia de Bing Image Search API](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference)
