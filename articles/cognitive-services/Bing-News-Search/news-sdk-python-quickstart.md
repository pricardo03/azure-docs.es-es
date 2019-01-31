---
title: 'Guía de inicio rápido: Realizar una búsqueda de noticias: SDK de Bing News Search para Python'
titleSuffix: Azure Cognitive Services
description: Use esta guía de inicio rápido para buscar noticias mediante el SDK de Bing News Search para Python y procesar la respuesta.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: quickstart
ms.date: 01/10/2019
ms.author: v-gedod
ms.custom: seodec2018
ms.openlocfilehash: 453da9f5765a0cf9bfdf58026a6d7a2d5a1a2d9f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55197201"
---
# <a name="quickstart-perform-a-news-search-with-the-bing-news-search-sdk-for-python"></a>Guía de inicio rápido: Realizar una búsqueda de noticias con el SDK de Bing News Search para Python

Use este inicio rápido para empezar a buscar noticias con el SDK de Bing News Search para Python. Aunque Bing News Search tiene una API REST compatible con la mayoría de los lenguajes de programación, el SDK proporciona una forma sencilla de integrar el servicio en sus aplicaciones. El código fuente de este ejemplo está disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/news_search_samples.py).

## <a name="prerequisites"></a>Requisitos previos

* [Python](https://www.python.org/) 2.x o 3.x

Se recomienda usar un [entorno virtual](https://docs.python.org/3/tutorial/venv.html) para el desarrollo de Python. Puede instalar e inicializar el entorno virtual con el [módulo venv](https://pypi.python.org/pypi/virtualenv). Debe instalar un módulo virtualenv para Python 2.7. Puede crear un entorno virtual con:

```console
python -m venv mytestenv
```

Puede instalar las dependencias del SDK de Bing News Search con este comando:
    
```console
python -m pip install azure-cognitiveservices-search-newssearch
```

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../includes/cognitive-services-bing-news-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Creación e inicialización de la aplicación

1. Cree un archivo de Python en su IDE o editor favorito e importe las bibliotecas siguientes. Cree una variable para la clave de suscripción y el término de búsqueda.

    ```python
    from azure.cognitiveservices.search.newssearch import NewsSearchAPI
    from msrest.authentication import CognitiveServicesCredentials
    subscription_key = "YOUR-SUBSCRIPTION-KEY"
    search_term = "Quantum Computing"
    ```

## <a name="initialize-the-client-and-send-a-request"></a>Inicialización del cliente y envío de una solicitud

1. Creación de una instancia de `CognitiveServicesCredentials`. Cree una instancia del cliente:
    
    ```python
    client = NewsSearchAPI(CognitiveServicesCredentials(subscription_key))
    ```

2. Envíe una consulta de búsqueda a News Search API y almacene la respuesta.

    ```python
    news_result = client.news.search(query=search_term, market="en-us", count=10)
    ```

## <a name="parse-the-response"></a>Análisis de la respuesta

Si se encuentran resultados de la búsqueda, imprima el primer resultado de la página web:

```python
if news_result.value:
    first_news_result = news_result.value[0]
    print("Total estimated matches value: {}".format(news_result.total_estimated_matches))
    print("News result count: {}".format(len(news_result.value)))
    print("First news name: {}".format(first_news_result.name))
    print("First news url: {}".format(first_news_result.url))
    print("First news description: {}".format(first_news_result.description))
    print("First published time: {}".format(first_news_result.date_published))
    print("First news provider: {}".format(first_news_result.provider[0].name))
else:
    print("Didn't see any news result data..")
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
[Creación de una aplicación web de una sola página](tutorial-bing-news-search-single-page-app.md)
