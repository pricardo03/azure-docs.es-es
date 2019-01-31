---
title: 'Inicio rápido: SDK de Bing Visual Search, Python'
titleSuffix: Azure Cognitive Services
description: Configuración de la aplicación de consola del SDK de Video Search para Python.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 06/11/2018
ms.author: v-gedod
ms.openlocfilehash: 372b7719fd77d1fe52f020a419a756bc769a676d
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55187852"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-sdk-for-python"></a>Inicio rápido: Obtención de información detallada de imágenes con el SDK de Bing Visual Search para Python

Use este artículo de inicio rápido para empezar a obtener información detallada de imágenes desde el servicio Bing Visual Search con el SDK de Python. Aunque Bing Visual Search tiene una API REST compatible con la mayoría de los lenguajes de programación, el SDK ofrece una forma fácil de integrar el servicio en sus aplicaciones. El código fuente correspondiente a este ejemplo se encuentra disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/visual_search_samples.py). 

## <a name="prerequisites"></a>Requisitos previos

* [Python](https://www.python.org/) 2.x o 3.x
* Se recomienda utilizar un [entorno virtual](https://docs.python.org/3/tutorial/venv.html). Instale e inicialice el entorno virtual con el [módulo venv](https://pypi.python.org/pypi/virtualenv). Instale virtualenv para Python 2.7.
* El SDK de Bing Visual Search para Python. Puede instalarlo con los siguientes comandos:
    1. `cd mytestenv`
    2. `python -m pip install azure-cognitiveservices-search-visualsearch`



[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]


## <a name="create-and-initialize-the-application"></a>Creación e inicialización de la aplicación

1. Cree un archivo de Python en su IDE o editor favorito y agregue las siguientes instrucciones de importación. 

    ```python
    import http.client, urllib.parse
    import json
    import os.path
    from azure.cognitiveservices.search.visualsearch import VisualSearchAPI
    from azure.cognitiveservices.search.visualsearch.models import (
        VisualSearchRequest,
        CropArea,
        ImageInfo,
        Filters,
        KnowledgeRequest,
    )
    ```
2. Cree variables para la clave de suscripción, el identificador de configuración personalizada y la imagen que quiere cargar. 
    
    ```python
    subscription_key = 'YOUR-VISUAL-SEARCH-ACCESS-KEY'
    PATH = 'C:\\Users\\USER\\azure-cognitive-samples\\mytestenv\\TestImages\\'
    image_path = os.path.join(PATH, "image.jpg")
    
    ```

3. Creación de una instancia del cliente

    ```python
    var client = new WebSearchAPI(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"))
    ```

## <a name="send-the-search-request"></a>Envío de la solicitud de búsqueda

1. Con el archivo de imagen abierto, serialice `VisualSearchRequest()` y páselo como parámetro `knowledge_request` para el elemento `visual_search()`.

    ```python
    with open(image_path, "rb") as image_fd:
        # You need to pass the serialized form of the model
        knowledge_request = json.dumps(VisualSearchRequest().serialize())
    
        print("\r\nSearch visual search request with binary of dog image")
        result = client.images.visual_search(image=image_fd, knowledge_request=knowledge_request)
    ```

2. Si se devolvieron resultados, imprímalos junto con las etiquetas y las acciones de la primera etiqueta.

    ```python
    if not result:
            print("No visual search result data.")
    
            # Visual Search results
        if result.image.image_insights_token:
            print("Uploaded image insights token: {}".format(result.image.image_insights_token))
        else:
            print("Couldn't find image insights token!")
    
        # List of tags
        if result.tags:
            first_tag = result.tags[0]
            print("Visual search tag count: {}".format(len(result.tags)))
    
            # List of actions in first tag
            if first_tag.actions:
                first_tag_action = first_tag.actions[0]
                print("First tag action count: {}".format(len(first_tag.actions)))
                print("First tag action type: {}".format(first_tag_action.action_type))
            else:
                print("Couldn't find tag actions!")
        else:
            print("Couldn't find image tags!")
    ```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Compilar una aplicación web de una sola página](tutorial-bing-visual-search-single-page-app.md)
