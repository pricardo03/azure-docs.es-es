---
title: 'Inicio rápido: Llamada al punto de conexión de Bing Custom Search con Python | Microsoft Docs'
titlesuffix: Azure Cognitive Services
description: Use este documento de inicio rápido para comenzar a solicitar los resultados de búsqueda de la instancia de Bing Custom Search con Python.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 05/07/2018
ms.author: aahi
ms.openlocfilehash: 2399cdfcfbc6be30dc013d52760ec4c355d106e4
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55868317"
---
# <a name="quickstart-call-your-bing-custom-search-endpoint-using-python"></a>Inicio rápido: Llamada al punto de conexión de Bing Custom Search con Python

Use este documento de inicio rápido para comenzar a solicitar los resultados de búsqueda de la instancia de Bing Custom Search. Si bien esta aplicación está escrita en Python, Bing Custom Search API es un servicio web RESTful compatible con la mayoría de los lenguajes de programación. El código fuente del ejemplo está disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingCustomSearchv7.py).

## <a name="prerequisites"></a>Requisitos previos

- Una instancia de Bing Custom Search. Consulte [Quickstart: Creación de la primera instancia de Bing Custom Search](quick-start.md) para más información.
- [Python](https://www.python.org/) 2.x o 3.x

[!INCLUDE [cognitive-services-bing-custom-search-prerequisites](../../../includes/cognitive-services-bing-custom-search-signup-requirements.md)]


## <a name="create-and-initialize-the-application"></a>Creación e inicialización de la aplicación

1. Cree un archivo de Python en el IDE o editor que prefiera y agregue las siguientes instrucciones de importación. Cree variables para la clave de suscripción, el identificador de configuración personalizada y un término de búsqueda. 

    ```python
    import json
    import requests
    
    subscriptionKey = "YOUR-SUBSCRIPTION-KEY"
    customConfigId = "YOUR-CUSTOM-CONFIG-ID"
    searchTerm = "microsoft"
    ```

## <a name="send-and-receive-a-search-request"></a>Envío y recepción de una solicitud de búsqueda 

1. Construya la dirección URL de solicitud anexando el término de búsqueda al parámetro de consulta `q=`, y el identificador de configuración personalizada de la instancia de la búsqueda a `customconfig=`. Separe los parámetros con un carácter `&`. 

    ```python
    url = 'https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?' + 'q=' + searchTerm + '&' + 'customconfig=' + customConfigId
    ```

2. Envíe la solicitud a la instancia de Bing Custom Search e imprima los resultados de búsqueda devueltos.  

    ```python
    r = requests.get(url, headers={'Ocp-Apim-Subscription-Key': subscriptionKey})
    print(r.text)
    ```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Creación de una página web de Custom Search](./tutorials/custom-search-web-page.md)
