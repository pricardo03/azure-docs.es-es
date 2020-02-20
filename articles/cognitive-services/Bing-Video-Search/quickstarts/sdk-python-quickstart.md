---
title: 'Inicio rápido: Búsqueda de vídeos mediante el SDK de Python: Bing Video Search'
titleSuffix: Azure Cognitive Services
description: Use este artículo de inicio rápido para enviar solicitudes de búsqueda de vídeo mediante el SDK de Bing Video Search para Python.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: quickstart
ms.date: 02/11/2020
ms.author: aahi
ms.openlocfilehash: 9d7b2a8950134e530e042e862a1c19abe89fd78d
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2020
ms.locfileid: "77201227"
---
# <a name="quickstart-perform-a-video-search-with-the-bing-video-search-sdk-for-python"></a>Inicio rápido: Realización de una búsqueda de vídeo con el SDK de Bing Video Search para Python

Use este artículo de inicio rápido para empezar a buscar vídeos con el SDK de Bing Video Search para Python. Aunque Bing Video Search tiene una API de REST compatible con la mayoría de los lenguajes de programación, el SDK proporciona una forma sencilla de integrar el servicio en sus aplicaciones. El código fuente de este ejemplo está disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/video_search_samples.py) con anotaciones adicionales y características.

[!INCLUDE [cognitive-services-bing-video-search-signup-requirements](../../../../includes/cognitive-services-bing-video-search-signup-requirements.md)]

## <a name="prerequisites"></a>Prerrequisitos

- [Python](https://www.python.org/) 2.x o 3.x
- SDK de Bing Video Search para Python

Se recomienda utilizar un [entorno virtual](https://docs.python.org/3/tutorial/venv.html) de Python. Puede instalar e inicializar un entorno virtual con el [módulo venv](https://pypi.python.org/pypi/virtualenv). Instale virtualenv para Python 2.7 con:

```console
python -m venv mytestenv
```

Instale el SDK de Bing Video Search con:

```console
cd mytestenv
python -m pip install azure-cognitiveservices-search-videosearch
```

## <a name="create-and-initialize-the-application"></a>Creación e inicialización de la aplicación

1. Cree un archivo de Python en el IDE o editor que prefiera y agregue las siguientes instrucciones de importación. 

    ```python
    from azure.cognitiveservices.search.videosearch import VideoSearchClient
    from azure.cognitiveservices.search.videosearch.models import VideoPricing, VideoLength, VideoResolution, VideoInsightModule
    from msrest.authentication import CognitiveServicesCredentials
    ```

2. Cree una variable para la clave de suscripción. 

    ```python
    subscription_key = "YOUR-SUBSCRIPTION-KEY"
    endpoint = "YOUR-ENDPOINT"
    ```

## <a name="create-the-search-client"></a>Creación del cliente de búsqueda

Cree una instancia de la `CognitiveServicesCredentials` y una instancia del cliente:

```python
client = VideoSearchAPI(endpoint, CognitiveServicesCredentials(subscription_key))
```

## <a name="send-a-search-request-and-get-a-response"></a>Envío de una solicitud de búsqueda y obtención de una respuesta

1. Use `client.videos.search()` con la consulta de búsqueda para enviar una solicitud a Bing Video Search API y obtener una respuesta.

    ```python
    video_result = client.videos.search(query="SwiftKey")
    ```

2. Si la respuesta contiene resultados de búsqueda, obtenga el primero de ellos e imprima su identificador, nombre y dirección URL.

    ```python
    if video_result.value:
        first_video_result = video_result.value[0]
        print("Video result count: {}".format(len(video_result.value)))
        print("First video id: {}".format(first_video_result.video_id))
        print("First video name: {}".format(first_video_result.name))
        print("First video url: {}".format(first_video_result.content_url))
    else:
        print("Didn't see any video result data..")
    ```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Creación de una aplicación web de una sola página](../tutorial-bing-video-search-single-page-app.md)

## <a name="see-also"></a>Consulte también 

- [¿Qué es Bing Video Search API?](../overview.md)
- [Ejemplos del SDK de Cognitive Services para .NET](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
