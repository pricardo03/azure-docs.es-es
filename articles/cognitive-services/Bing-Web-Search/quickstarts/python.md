---
title: 'Llamada y respuesta: inicio rápido de Python para Azure Cognitive Services, Bing Web Search API | Microsoft Docs'
description: Obtenga información y ejemplos de código que le ayuden a empezar a usar rápidamente Bing Web Search API en Microsoft Cognitive Services en Azure.
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 9/18/2017
ms.author: v-jerkin
ms.openlocfilehash: 8d4df9db60c7a74a5b9e53d4622528c0054b4f19
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380715"
---
# <a name="call-and-response-your-first-bing-web-search-query-in-python"></a>Llamada y respuesta: la primera consulta de Bing Web Search en Python

Bing Web Search API proporciona una experiencia similar a Bing.com/Search al devolver resultados de búsqueda que Bing determina que están relacionados con la consulta del usuario. Los resultados pueden incluir páginas web, imágenes, vídeos, noticias y entidades, junto con las consultas de búsqueda relacionadas, correcciones ortográficas, zonas horarias, conversión de unidades, traducciones y cálculos. Los tipos de resultados que obtendrá se basan en su importancia y en el nivel al que esté suscrito en Bing Search APIs.

Consulte la [referencia de la API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference) para obtener detalles técnicos acerca de las API.

Para ejecutar este ejemplo como Jupyter Notebook en [MyBinder](https://mybinder.org), puede hacer clic en el distintivo launch Binder: 

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingWebSearchAPI.ipynb)


## <a name="prerequisites"></a>requisitos previos
Debe tener una [cuenta de Cognitive Services API](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con **Bing Search APIs**. La [cuenta de evaluación gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) es suficiente para esta guía de inicio rápido. Necesita la clave de acceso que se le proporciona al activar la versión de evaluación gratuita, o puede usar una clave de suscripción de pago desde su panel de Azure.

## <a name="running-the-walkthrough"></a>Ejecución del tutorial

Establezca `subscription_key` como la clave de API para el servicio de la API de Bing.


```python
subscription_key = None
assert subscription_key
```

A continuación, compruebe que el punto de conexión `search_url` sea correcto. En este documento, se utiliza un único punto de conexión Bing Search APIs. Si se producen errores de autorización, vuelva a comprobar este valor frente al punto de conexión de búsqueda de Bing en el panel de Azure.


```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/search"
```

Establezca `search_term` para consultar por Microsoft Cognitive Services en Bing.


```python
search_term = "Microsoft Cognitive Services"
```

El siguiente bloque usa la biblioteca `requests` de Python para llamar a Bing Search APIs y devolver los resultados como objeto JSON. Observe que se pasa la clave de API a través del diccionario `headers`, y el término de búsqueda a través del diccionario `params`. Para ver la lista completa de opciones que puede usarse para filtrar los resultados de la búsqueda, consulte la documentación de la [API de REST](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference).


```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "textDecorations":True, "textFormat":"HTML"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

El objeto `search_results` contiene los resultados de la búsqueda, junto con metadatos enriquecidos como consultas y páginas relacionadas. Las siguientes líneas de código dan formato a las páginas principales devueltas por la consulta.


```python
from IPython.display import HTML

rows = "\n".join(["""<tr>
                       <td><a href=\"{0}\">{1}</a></td>
                       <td>{2}</td>
                     </tr>""".format(v["url"],v["name"],v["snippet"]) \
                  for v in search_results["webPages"]["value"]])
HTML("<table>{0}</table>".format(rows))
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial de aplicación de una sola página de Bing Web Search](../tutorial-bing-web-search-single-page-app.md)

## <a name="see-also"></a>Otras referencias 

[Introducción a Bing Web Search](../overview.md)  
[Pruébelo](https://azure.microsoft.com/services/cognitive-services/bing-web-search-api/)  
[Obtener una clave de acceso para evaluación gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api)
[Referencia de Bing Web Search API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference)
