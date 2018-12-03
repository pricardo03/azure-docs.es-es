---
title: 'Inicio rápido: SDK de Bing Visual Search, Python'
titleSuffix: Azure Cognitive Services
description: Configuración de la aplicación de consola del SDK de Video Search para Python.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: quickstart
ms.date: 06/11/2018
ms.author: v-gedod
ms.openlocfilehash: 9f2a6d9b75ccf704862d169b96ea1a1f2edb9815
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "52445693"
---
# <a name="quickstart-bing-visual-search-sdk-python"></a>Inicio rápido: SDK de Bing Visual Search en Python

El SDK de Bing Visual Search contiene la funcionalidad de la API REST para solicitudes web y análisis de resultados.
El [código fuente de los ejemplos del SDK de Bing Visual Search para Python](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/visual_search_samples.py) está disponible en Git Hub.

Los escenarios de código se documentan en los siguientes encabezados:
* [Cliente de Visual Search](#client)
* [Aplicación completa de consola](#complete-console)
* [Post de archivo binario de imagen con cropArea](#binary-crop)
* [Parámetro KnowledgeRequest](#knowledge-req)
* [Etiquetas, acciones y actionType](#tags-actions)

## <a name="application-dependencies"></a>Dependencias de aplicaciones
* Para esta guía de inicio rápido, sera preciso iniciar una suscripción en el nivel de precios S9 como se muestra en [Precios de Cognitive Services: Bing Search API](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/search-api/). 

Para iniciar una suscripción en Azure Portal:
1. Escriba "BingSearchV7" en el cuadro de texto de la parte superior de Azure Portal que dice `Search resources, services, and docs`.  
2. En Marketplace en la lista desplegable, seleccione `Bing Search v7`.
3. Escriba `Name` para el nuevo recurso.
4. Seleccione la suscripción `Pay-As-You-Go`.
5. Seleccione el plan de tarifa `S9`.
6. Haga clic en `Enable` para iniciar la suscripción.
 
* Si aún no lo tiene, instale Python. El SDK es compatible con Python 2.7, 3.3, 3.4, 3.5 y 3.6.
* La recomendación general para el desarrollo de Python es usar un [entorno virtual](https://docs.python.org/3/tutorial/venv.html). Instale e inicialice el entorno virtual con el [módulo venv](https://pypi.python.org/pypi/virtualenv). Instale virtualenv para Python 2.7.
```
python -m venv mytestenv
```
Instale las dependencias del SDK de Bing Visual Search:
```
cd mytestenv
python -m pip install azure-cognitiveservices-search-visualsearch
```

<a name="client"></a> 
## <a name="visual-search-client"></a>Cliente de Visual Search
Para crear una instancia del cliente `VisualSearchAPI`, importe las siguientes bibliotecas:
```
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
Reemplace el valor de la cadena subscriptionKey por su clave de suscripción válida.
```
subscription_key = 'YOUR-VISUAL-SEARCH-ACCESS-KEY'
```
Después, cree una instancia del cliente:
```
var client = new WebSearchAPI(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));
```
Use el cliente para buscar imágenes y analizar resultados:
```
PATH = 'C:\\Users\\USER\\azure-cognitive-samples\\mytestenv\\TestImages\\'
image_path = os.path.join(PATH, "image.jpg")

with open(image_path, "rb") as image_fd:

    # You need to pass the serialized form of the model
    knowledge_request = json.dumps(VisualSearchRequest().serialize())

    print("\r\nSearch visual search request with binary of dog image")
    result = client.images.visual_search(image=image_fd, knowledge_request=knowledge_request)
        
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

<a name="complete-console"></a> 
## <a name="complete-console-application"></a>Aplicación completa de consola

La siguiente aplicación de consola ejecuta la consulta definida previamente y analiza los resultados:
```
import http.client, urllib.parse
import json
import os.path

# Replace the subscriptionKey string value with your valid subscription key.
subscription_key = 'YOUR-VISUAL-SEARCH-ACCESS-KEY'

PATH = 'C:\\Users\\v-gedod\\azure-cognitive-samples\\mytestenv\\TestImages\\'

from azure.cognitiveservices.search.visualsearch import VisualSearchAPI
from azure.cognitiveservices.search.visualsearch.models import (
    VisualSearchRequest,
    CropArea,
    ImageInfo,
    Filters,
    KnowledgeRequest,)

from msrest.authentication import CognitiveServicesCredentials

client = VisualSearchAPI(CognitiveServicesCredentials(subscription_key))

image_path = os.path.join(PATH, "image.jpg")

with open(image_path, "rb") as image_fd:

    # You need to pass the serialized form of the model
    knowledge_request = json.dumps(VisualSearchRequest().serialize())

    print("\r\nSearch visual search request with binary of dog image")
    result = client.images.visual_search(image=image_fd, knowledge_request=knowledge_request)
        
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


# Uncomment these methods to include code under the following headings of this documentation.
#search_image_binary_with_crop_area(client, subscription_key, PATH)
#search_url_with_filters(client, subscription_key)
#search_insights_token_with_crop_area(client, subscription_key)

```

Los ejemplos de búsqueda de Bing muestran diversas características del SDK.  Agregue las siguientes funciones a la clase `VisualSrchSDK` previamente definida.

<a name="binary-crop"></a>
## <a name="image-binary-post-with-croparea"></a>POST de archivo binario de imagen con cropArea

El siguiente código envía un archivo binario de imagen en el cuerpo de la solicitud POST, junto con un objeto cropArea.  Después, imprime el objeto imageInsightsToken, el número de etiquetas, el número de acciones y el primer objeto actionType.

```
def search_image_binary_with_crop_area(client, sub_key, file_path):

    #client = VisualSearchAPI(CognitiveServicesCredentials(sub_key))

    image_path = os.path.join(file_path, "image.jpg")
    with open(image_path, "rb") as image_fd:

        crop_area = CropArea(top=0.1,bottom=0.5,left=0.1,right=0.9)
        knowledge_request = VisualSearchRequest(image_info=ImageInfo(crop_area=crop_area))

        # You need to pass the serialized form of the model
        knowledge_request = json.dumps(knowledge_request.serialize())

        print("\r\nSearch visual search request with binary of dog image")
        result = client.images.visual_search(image=image_fd, knowledge_request=knowledge_request)

        if not result:
            print("No visual search result data.")
            return

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
<a name="knowledge-req"></a>
## <a name="knowledgerequest-parameter"></a>Parámetro KnowledgeRequest

El código siguiente envía una dirección URL de imagen en el parámetro `knowledgeRequest`, junto con un filtro \"site:pinterest.com\". Después, imprime el objeto `imageInsightsToken`, el número de etiquetas, el número de acciones y el primer objeto actionType.
```
def search_url_with_filters(client_in, sub_key):

    client = client_in

    image_url = "https://images.unsplash.com/photo-1512546148165-e50d714a565a?w=600&q=80"
    filters = Filters(site="pinterest.com")

    knowledge_request = VisualSearchRequest(
        image_info=ImageInfo(url=image_url),
        knowledge_request=KnowledgeRequest(filters=filters)
    )

    # You need to pass the serialized form of the model
    knowledge_request = json.dumps(knowledge_request.serialize())

    print("\r\nSearch visual search request with url of dog image")
    result = client.images.visual_search(knowledge_request=knowledge_request)

    if not result:
        print("No visual search result data.")
        return

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
<a name="tags-actions"></a>
## <a name="tags-actions-and-actiontype"></a>Etiquetas, acciones y actionType

El código siguiente envía un token de información de la imagen en el parámetro knowledgeRequest, junto con un objeto cropArea. Después, imprime el objeto imageInsightsToken, el número de etiquetas, el número de acciones y el primer objeto actionType.

```
    client = client_in

    image_insights_token = "bcid_CA6BDBEA28D57D52E0B9D4B254F1DF0D*ccid_6J+8V1zi*thid_R.CA6BDBEA28D57D52E0B9D4B254F1DF0D"
    crop_area = CropArea(top=0.1,bottom=0.5,left=0.1,right=0.9)

    knowledge_request = VisualSearchRequest(
        image_info=ImageInfo(
            image_insights_token=image_insights_token,
            crop_area=crop_area
        ),
    )

    # You need to pass the serialized form of the model
    knowledge_request = json.dumps(knowledge_request.serialize())

    print("\r\nSearch visual search request with URL of dog image")
    result = client.images.visual_search(knowledge_request=knowledge_request)

    if not result:
        print("No visual search result data.")
        return

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

[Ejemplos del SDK de Cognitive Services para .NET](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)