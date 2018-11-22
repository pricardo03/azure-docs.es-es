---
title: 'Inicio rápido: Realizar una búsqueda con Python - Bing Web Search API'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido, aprenderá a realizar la primera llamada a Bing Web Search API con Python y recibir una respuesta JSON.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: quickstart
ms.date: 8/16/2018
ms.author: aahi
ms.openlocfilehash: 2f034bd60747282ac9724250ab260e06122def6f
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2018
ms.locfileid: "52161001"
---
# <a name="quickstart-use-python-to-call-the-bing-web-search-api"></a>Inicio rápido: Uso de Python para llamar a Bing Web Search API  

Use esta guía de inicio rápido para realizar la primera llamada a Bing Web Search API y recibir una respuesta JSON en menos de 10 minutos.  

[!INCLUDE [bing-web-search-quickstart-signup](../../../../includes/bing-web-search-quickstart-signup.md)]

En este ejemplo se ejecuta como una instancia de Jupyter Notebook en [MyBinder](https://mybinder.org). Haga clic en el distintivo launch Binder:

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingWebSearchAPI.ipynb)

## <a name="define-variables"></a>Definición de variables

Reemplace el valor `subscription_key` por una clave de suscripción válida de su cuenta de Azure.

```python
subscription_key = "YOUR_ACCESS_KEY"
assert subscription_key
```

Declare el punto de conexión de Bing Web Search API. Si se producen errores de autorización, vuelva a comprobar este valor con el punto de conexión de búsqueda de Bing en el panel de Azure.

```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/search"
```

No dude en personalizar la consulta de búsqueda reemplazando el valor de `search_term`.

```python
search_term = "Azure Cognitive Services"
```

## <a name="make-a-request"></a>Realización de una solicitud

Este bloque usa la biblioteca `requests` para llamar a Bing Web Search API y devolver los resultados como objeto JSON. La clave de API se pasa en el diccionario `headers` y el término de búsqueda y los parámetros de consulta se pasan en el diccionario `params`. Consulte la documentación de [Bing Web Search API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference) para obtener una lista completa de opciones y parámetros.

```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "textDecorations":True, "textFormat":"HTML"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

## <a name="format-and-display-the-response"></a>Formato y presentación de la respuesta

El objeto `search_results` incluye los resultados de la búsqueda, junto con metadatos como consultas y páginas relacionadas. Este código utiliza la biblioteca `IPython.display` para dar formato y mostrar la respuesta en el explorador.

```python
from IPython.display import HTML

rows = "\n".join(["""<tr>
                       <td><a href=\"{0}\">{1}</a></td>
                       <td>{2}</td>
                     </tr>""".format(v["url"],v["name"],v["snippet"]) \
                  for v in search_results["webPages"]["value"]])
HTML("<table>{0}</table>".format(rows))
```

## <a name="sample-code-on-github"></a>Código de ejemplo en GitHub

Si desea ejecutar este código localmente, el [ejemplo completo está disponible en GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/nodejs/Search/BingWebSearchv7.js).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial de aplicación de una sola página de Bing Web Search](../tutorial-bing-web-search-single-page-app.md)

[!INCLUDE [bing-web-search-quickstart-see-also](../../../../includes/bing-web-search-quickstart-see-also.md)]
