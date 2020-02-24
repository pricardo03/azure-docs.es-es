---
title: 'Inicio rápido: Búsqueda de entidades con el SDK de Python: Bing Entity Search'
titleSuffix: Azure Cognitive Services
description: Use este inicio rápido para buscar entidades con el SDK de Bing Entity Search para Python.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: quickstart
ms.date: 02/11/2020
ms.author: aahi
ms.openlocfilehash: a6b62f7ab95f7b2720434c0cf59cce33b0adb1b4
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2020
ms.locfileid: "77201244"
---
# <a name="quickstart-bing-entity-search-sdk-with-python"></a>Inicio rápido: SDK de Bing Entity Search con Python

Use este inicio rápido para empezar a buscar entidades con el SDK de Bing Entity Search para Python. Aunque Bing Entity Search tiene una API REST compatible con la mayoría de los lenguajes de programación, el SDK proporciona una forma sencilla de integrar el servicio en sus aplicaciones. El código fuente de este ejemplo está disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/entity_search_samples.py).

## <a name="prerequisites"></a>Prerrequisitos

* Python [2.x o 3.x](https://www.python.org/)

* [SDK de Bing Entity Search para Python](https://pypi.org/project/azure-cognitiveservices-search-entitysearch/)

Se recomienda utilizar un entorno virtual de Python. Puede instalar e inicializar un entorno virtual con el módulo venv. Puede instalar virtualenv con:

```Console
python -m venv mytestenv
```

Instale el SDK de Bing Entity Search con:

```Console
cd mytestenv
python -m pip install azure-cognitiveservices-search-entitysearch
```

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../../includes/cognitive-services-bing-entity-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Creación e inicialización de la aplicación

1. Cree un archivo de Python en el IDE o editor que prefiera y agregue las siguientes instrucciones de importación. 

    ```python
    from azure.cognitiveservices.search.entitysearch import EntitySearchClient
    from azure.cognitiveservices.search.entitysearch.models import Place, ErrorResponseException
    from msrest.authentication import CognitiveServicesCredentials
    ```

2. Cree una variable para la clave y el punto de conexión de la suscripción. Cree una instancia del cliente mediante la creación de un objeto `CognitiveServicesCredentials` con la clave.
    
    ```python
    subscription_key = "YOUR-SUBSCRIPTION-KEY"
    endpoint = "YOUR-ENDPOINT"
    client = EntitySearchclient(endpoint=endpoint, credentials=CognitiveServicesCredentials(subscription_key))
    ```

## <a name="send-a-search-request-and-receive-a-response"></a>Envío de una solicitud de búsqueda y recepción de la respuesta

1. Envíe una solicitud de búsqueda a Bing Entity Search con `client.entities.search()` y una consulta de búsqueda. 
    
    ```python
    entity_data = client.entities.search(query="Gibralter")
    ```

2. Si se devuelven entidades, convierta `entity_data.entities.value` en una lista e imprima el primer resultado.
    ```python
    if entity_data.entities.value:
    
        main_entities = [entity for entity in entity_data.entities.value
                         if entity.entity_presentation_info.entity_scenario == "DominantEntity"]
    
        if main_entities:
            print(main_entities[0].description)
    ```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Creación de una aplicación web de una sola página](../tutorial-bing-entities-search-single-page-app.md)

* [¿Qué es Bing Entity Search API?](../overview.md )