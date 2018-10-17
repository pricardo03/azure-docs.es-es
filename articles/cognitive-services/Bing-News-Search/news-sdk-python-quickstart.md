---
title: 'Guía de inicio rápido: SDK de Bing News Search con Python'
titleSuffix: Azure Cognitive Services
description: Configure la aplicación de consola del SDK de Bing News Search.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: quickstart
ms.date: 02/14/2018
ms.author: v-gedod
ms.openlocfilehash: 8e4343b053835c0fc2219373ad60f96c7b80636a
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48803348"
---
# <a name="quickstart-bing-news-search-sdk-with-python"></a>Guía de inicio rápido: SDK de Bing News Search con Python

El SDK de News Search contiene la funcionalidad de la API REST para consultas web y análisis de resultados. 

El [código fuente de los ejemplos del SDK de Bing News Search para Python](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/news_search_samples.py) está disponible en GitHub.

## <a name="application-dependencies"></a>Dependencias de aplicaciones
Si aún no lo tiene, instale Python. El SDK es compatible con Python 2.7, 3.3, 3.4, 3.5 y 3.6.

La recomendación general para el desarrollo de Python es usar un [entorno virtual](https://docs.python.org/3/tutorial/venv.html). Instale e inicialice el entorno virtual con el [módulo venv](https://pypi.python.org/pypi/virtualenv). Debe instalar virtualenv para Python 2.7.
```
python -m venv mytestenv
```
Instale las dependencias del SDK de Bing News Search:
```
cd mytestenv
python -m pip install azure-cognitiveservices-search-newssearch
```
## <a name="news-search-client"></a>Cliente de News Search
Obtenga una [clave de acceso de Cognitive Services](https://azure.microsoft.com/try/cognitive-services/) en *Buscar*. Agregar importaciones:
```
from azure.cognitiveservices.search.newssearch import NewsSearchAPI
from msrest.authentication import CognitiveServicesCredentials

subscription_key = "YOUR-SUBSCRIPTION-KEY"
```
Creación de una instancia de `CognitiveServicesCredentials`. Cree una instancia del cliente:
```
client = NewsSearchAPI(CognitiveServicesCredentials(subscription_key))
```
Busque resultados e imprima el primer resultado de la página web:
```
news_result = client.news.search(query="Quantum Computing", market="en-us", count=10)
print("Search news for query \"Quantum Computing\" with market and count")

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
Busque con filtros las noticias más recientes sobre "Inteligencia artificial" con los parámetros `freshness` y `sortBy`. Compruebe el número de resultados e imprima `totalEstimatedMatches`, `name`, `url`, `description`, `published time` y `name of provider` del primer resultado del elemento de noticias.
```
def news_search_with_filtering(subscription_key):

    client = NewsSearchAPI(CognitiveServicesCredentials(subscription_key))

    try:
        news_result = client.news.search(
            query="Artificial Intelligence",
            market="en-us",
            freshness="Week",
            sort_by="Date"
        )
        print("\r\nSearch most recent news for query \"Artificial Intelligence\" with freshness and sortBy")

        if news_result.value:
            first_news_result = news_result.value[0]
            print("News result count: {}".format(len(news_result.value)))
            print("First news name: {}".format(first_news_result.name))
            print("First news url: {}".format(first_news_result.url))
            print("First news description: {}".format(first_news_result.description))
            print("First published time: {}".format(first_news_result.date_published))
            print("First news provider: {}".format(first_news_result.provider[0].name))
        else:
            print("Didn't see any news result data..")

    except Exception as err:
        print("Encountered exception. {}".format(err))

```
Busque entretenimiento de películas y TV en la categoría de noticias con la búsqueda segura. Compruebe el número de resultados e imprima `category`, `name`, `url`, `description`, `published time` y `name of provider` del primer resultado del elemento de noticias.
```
def news_category(subscription_key):

    client = NewsSearchAPI(CognitiveServicesCredentials(subscription_key))

    try:
        news_result = client.news.category(
            category="Entertainment_MovieAndTV",
            market="en-us",
            safe_search="strict"
        )
        print("\r\nSearch category news for movie and TV entertainment with safe search")

        if news_result.value:
            first_news_result = news_result.value[0]
            print("News result count: {}".format(len(news_result.value)))
            print("First news category: {}".format(first_news_result.category))
            print("First news name: {}".format(first_news_result.name))
            print("First news url: {}".format(first_news_result.url))
            print("First news description: {}".format(first_news_result.description))
            print("First published time: {}".format(first_news_result.date_published))
            print("First news provider: {}".format(first_news_result.provider[0].name))
        else:
            print("Didn't see any news result data..")

    except Exception as err:
        print("Encountered exception. {}".format(err))


```
Busque temas de tendencia de noticias de Bing.  Compruebe el número de resultados e imprima `name`, `text of query`, `webSearchUrl`, `newsSearchUrl` y `image Url` del primer resultado de noticias.
```
def news_trending(subscription_key):

    client = NewsSearchAPI(CognitiveServicesCredentials(subscription_key))

    try:
        trending_topics = client.news.trending(market="en-us")
        print("\r\nSearch news trending topics in Bing")

        if trending_topics.value:
            first_topic = trending_topics.value[0]
            print("News result count: {}".format(len(trending_topics.value)))
            print("First topic name: {}".format(first_topic.name))
            print("First topic query: {}".format(first_topic.query.text))
            print("First topic image url: {}".format(first_topic.image.url))
            print("First topic webSearchUrl: {}".format(first_topic.web_search_url))
            print("First topic newsSearchUrl: {}".format(first_topic.news_search_url))
        else:
            print("Didn't see any topics result data..")

    except Exception as err:
        print("Encountered exception. {}".format(err))

```

## <a name="next-steps"></a>Pasos siguientes

[Ejemplos del SDK de Cognitive Services para Python](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)


