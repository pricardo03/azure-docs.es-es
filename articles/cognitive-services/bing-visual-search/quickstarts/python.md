---
title: 'Inicio rápido: Obtención de conclusiones de imágenes mediante la API REST Bing Visual Search y Python'
titleSuffix: Azure Cognitive Services
description: Aprenda a cargar una imagen en Bing Visual Search API y obtener conclusiones sobre ella.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: quickstart
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: 3930de4d8d1f50c0ba6908ea642fc152c29b7371
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2018
ms.locfileid: "53744809"
---
# <a name="quickstart-your-first-bing-visual-search-query-in-python"></a>Inicio rápido: Su primera consulta de Bing Visual Search en Python

Use este inicio rápido para realizar la primera llamada a Bing Visual Search API y ver los resultados de búsqueda. Esta sencilla aplicación de JavaScript carga una imagen en la API y muestra la información que se devuelve sobre ella. Si bien esta aplicación está escrita en Java, la API es un servicio web RESTful compatible con la mayoría de los lenguajes de programación.

Al cargar una imagen local, los datos del formulario POST deben incluir el encabezado Content-Disposition. Asimismo, el parámetro `name` se debe establecer en "imagen" y el parámetro `filename` se puede establecer en cualquier cadena. El contenido del formulario es el binario de la imagen. Recuerde que el tamaño de imagen máximo que puede cargar es de 1 MB.

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

## <a name="prerequisites"></a>Requisitos previos

* [Python 3.x](https://www.python.org/)


[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="initialize-the-application"></a>Inicialización de la aplicación

1. Cree un archivo de Python en el IDE o editor que prefiera y agregue las siguientes instrucciones de importación.

    ```python
    import requests, json
    ```

2. Cree variables para la clave de suscripción, el punto de conexión y la ruta de acceso a la imagen que está cargando.

    ```python

    BASE_URI = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch'
    SUBSCRIPTION_KEY = 'your-subscription-key'
    imagePath = 'your-image-path'
    ```

3. Cree un objeto de diccionario para almacenar la información de encabezado de las solicitudes. Enlace la clave de suscripción a la cadena `Ocp-Apim-Subscription-Key`, como se muestra a continuación.

    ```python
    HEADERS = {'Ocp-Apim-Subscription-Key': SUBSCRIPTION_KEY}
    ```

4. Cree otro diccionario para almacenar la imagen, que se abrirá y cargará cuando envíe la solicitud. 

    ```python
    file = {'image' : ('myfile', open(imagePath, 'rb'))}
    ```

## <a name="parse-the-json-response"></a>Procese la respuesta JSON.

1. Cree un método denominado `print_json()` para realizar en la respuesta de API e imprima el JSON.

    ```python
    def print_json(obj):
        """Print the object as json"""
        print(json.dumps(obj, sort_keys=True, indent=2, separators=(',', ': ')))
    ```

## <a name="send-the-request"></a>Envío de la solicitud

1. Use `requests.post()` para enviar una solicitud a Bing Visual Search API. Incluya la cadena para la información de archivo, encabezado y punto de conexión. Imprima `response.json()` con `print_json()`

    ```python
    try:
        response = requests.post(BASE_URI, headers=HEADERS, files=file)
        response.raise_for_status()
        print_json(response.json())
    
    except Exception as ex:
        raise ex
    ```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Creación de una página web de Custom Search](../tutorial-bing-visual-search-single-page-app.md)