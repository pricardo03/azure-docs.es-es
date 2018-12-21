---
title: 'Guía de inicio rápido: Realizar una búsqueda de noticias con Python: API de REST de Bing News Search'
titlesuffix: Azure Cognitive Services
description: Use esta guía de inicio rápido para enviar una solicitud a la API de REST de Bing News Search mediante Python y reciba una respuesta de JSON.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: quickstart
ms.date: 9/21/2017
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 8ce8353df9a6f8354c56d9c9115645c0b7f2136a
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "53251664"
---
# <a name="quickstart-perform-a-news-search-using-python-and-the-bing-news-search-rest-api"></a>Guía de inicio rápido: Realizar una búsqueda de noticias mediante Python y la API de REST de Bing News Search

Este tutorial muestra un ejemplo sencillo de llamada a Bing News Search API y procesamiento posterior del objeto JSON resultante. Para obtener más información, consulte la [documentación de Bing News Search](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference).  

Para ejecutar este ejemplo como Jupyter Notebook en [MyBinder](https://mybinder.org), puede hacer clic en el distintivo launch Binder: 

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingNewsSearchAPI.ipynb)

## <a name="prerequisites"></a>Requisitos previos

Debe tener una [cuenta de Cognitive Services API](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con **Bing Search APIs**. La [cuenta de evaluación gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) es suficiente para esta guía de inicio rápido. Necesitará la clave de acceso proporcionada al activar la evaluación gratuita.  Consulte también [Precios de Cognitive Services - Bing Search API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="running-the-walkthrough"></a>Ejecución del tutorial
En primer lugar, establezca `subscription_key` como la clave de API para el servicio de la API de Bing.


```python
subscription_key = None
assert subscription_key
```

A continuación, compruebe que el punto de conexión `search_url` sea correcto. En este documento, se utiliza un único punto de conexión Bing Search APIs. Si se producen errores de autorización, vuelva a comprobar este valor frente al punto de conexión de búsqueda de Bing en el panel de Azure.


```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/news/search"
```

Establezca `search_term` para buscar nuevos artículos acerca de Microsoft.


```python
search_term = "Microsoft"
```

El siguiente bloque usa la biblioteca `requests` de Python para llamar a Bing Search APIs y devolver los resultados como objeto JSON. Observe que se pasa la clave de API a través del diccionario `headers`, y el término de búsqueda a través del diccionario `params`. Para ver la lista completa de opciones que puede usarse para filtrar los resultados de la búsqueda, consulte la documentación de la [API de REST](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference).


```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "textDecorations": True, "textFormat": "HTML"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

El objeto `search_results` contiene los nuevos artículos pertinentes, junto con metadatos enriquecidos. Por ejemplo, la siguiente línea de código extrae las descripciones de los artículos.


```python
descriptions = [article["description"] for article in search_results["value"]]
```

A continuación, estas descripciones se pueden representar como una tabla con la palabra clave de búsqueda resaltada en **negrita**.


```python
from IPython.display import HTML
rows = "\n".join(["<tr><td>{0}</td></tr>".format(desc) for desc in descriptions])
HTML("<table>"+rows+"</table>")
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Paginación de noticias](paging-news.md)
> [Uso de marcadores de decoración para resaltar texto](hit-highlighting.md)

## <a name="see-also"></a>Otras referencias 

 [Búsqueda de noticias en la web](search-the-web.md)  
 [Pruébelo](https://azure.microsoft.com/services/cognitive-services/bing-news-search-api/)
