---
title: 'Inicio rápido: Bing Autosuggest API'
titlesuffix: Azure Cognitive Services
description: Se muestra cómo empezar a usar Bing Autosuggest API.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-autosuggest
ms.topic: quickstart
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: c748f58028d747b2d963d6ac28ea2661e41ec68c
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55184519"
---
# <a name="quickstart-making-your-first-autosuggest-query"></a>Inicio rápido: Realización de la primera consulta con Autosuggest

Para poder realizar la primera llamada, debe obtener una clave de suscripción de Cognitive Services. Para obtener una clave, consulte [Pruebe Cognitive Services](https://azure.microsoft.com/try/cognitive-services/?api=autosuggest-api).

Para obtener los resultados de búsqueda web, envíe una solicitud GET al siguiente punto de conexión:

```http
https://api.cognitive.microsoft.com/bing/v5.0/Suggestions
```

> [!NOTE]
> Punto de conexión de la versión preliminar de V7:
>
> ```http
> https://api.cognitive.microsoft.com/bing/v7.0/Suggestions
> ```

La solicitud debe usar el protocolo HTTPS.

Se recomienda que todas las solicitudes procedan de un servidor. Al distribuir la clave como parte de una aplicación cliente, existe una mayor probabilidad de que un tercero malintencionado acceda a ella. Además, la realización de llamadas desde un servidor proporciona un único punto de actualización para futuras versiones de la API.

La solicitud debe especificar el parámetro de consulta [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#query), que contiene el término de búsqueda parcial del usuario. Aunque es opcional, la solicitud debe especificar también el parámetro de consulta [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#mkt), que identifica el mercado de donde desea que procedan los resultados. Para ver una lista de parámetros de consulta opcionales, consulte [Query Parameters](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#query-parameters) (Parámetros de consulta). Todos los valores de parámetro de consulta deben estar codificados con una dirección URL.

La solicitud debe especificar el encabezado [Ocp-Apim-Subscription-Key](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#subscriptionkey). Aunque es opcional, se recomienda especificar los encabezados siguientes:

- [User-Agent](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#useragent)
- [X-MSEdge-ClientID](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#clientid)
- [X-Search-ClientIP](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#clientip)
- [X-Search-Location](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#location)

Los encabezados de IP y ubicación del cliente son importantes para devolver contenido relativo a la ubicación.

Para ver una lista de todos los encabezados de solicitud y respuesta, consulte [Encabezados](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#headers).

## <a name="the-request"></a>La solicitud

La solicitud debe incluir todos los encabezados y parámetros de consulta sugeridos. A esta API se le llamaría cada vez que el usuario escribe un carácter nuevo en el cuadro de búsqueda. La integridad de la cadena de consulta afecta a la relevancia de los términos de la consulta sugeridos que devuelve la API. Cuanto más completa sea la cadena de consulta, más apropiado es la lista de términos de consulta sugeridos. Por ejemplo, las sugerencias que puede devolver la API para *s* es probable que tengan menos relevancia que las consultas que devuelve para *veleros*. 

En el ejemplo siguiente se muestra una solicitud que devuelve las cadenas de consulta sugeridas para *navegar*.

```http
GET https://api.cognitive.microsoft.com/bing/v5.0/suggestions?q=sail&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

> [!NOTE]
> Solicitud de versión preliminar de V7:

> ```http
> GET https://api.cognitive.microsoft.com/bing/v7.0/suggestions?q=sail&mkt=en-us HTTP/1.1
> Ocp-Apim-Subscription-Key: 123456789ABCDE
> X-MSEdge-ClientIP: 999.999.999.999
> X-Search-Location: lat:47.60357;long:-122.3295;re:100
> X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
> Host: api.cognitive.microsoft.com
> ```

Si es la primera vez que llama a cualquiera de las API de Bing, no incluya el encabezado de identificador de cliente. Solo debe incluir el encabezado del identificador de cliente si se ha llamado previamente a una API de Bing y Bing ha devuelto un identificador de cliente para esa combinación de usuario y dispositivo.

A continuación se muestra la respuesta a la solicitud anterior. La respuesta incluye un grupo de sugerencias web que contiene una lista de sugerencias de las consultas de búsqueda. Cada sugerencia incluye un campo `displayText`, `query` y `url`.

El campo `displayText` contiene la consulta sugerida que usaría para rellenar la lista desplegable del cuadro de búsqueda. Debe mostrar todas las sugerencias que incluye la respuesta, y en el orden especificado.  

Si el usuario selecciona una consulta en la lista desplegable, puede usar la cadena de consulta del campo `query` para llamar a [Bing Search API](../bing-web-search/search-the-web.md) y mostrar los resultados por sí mismo. O bien, puede usar la dirección URL del campo `url` para enviar al usuario a la página de resultados de la búsqueda de Bing.

```  
BingAPIs-TraceId: 76DD2C2549B94F9FB55B4BD6FEB6AC
X-MSEdge-ClientID: 1C3352B306E669780D58D607B96869
BingAPIs-Market: en-US

{
    "_type" : "Suggestions",
    "queryContext" : {
        "originalQuery" : "sail"
    },
    "suggestionGroups" : [{
        "name" : "Web",
        "searchSuggestions" : [{
            "url" : "https:\/\/www.bing.com\/search?q=sailing+lessons+seattle&FORM=USBAPI",
            "displayText" : "sailing lessons seattle",
            "query" : "sailing lessons seattle",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailor+moon+news&FORM=USBAPI",
            "displayText" : "sailor moon news",
            "query" : "sailor moon news",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailor+jack%27s+lincoln+city&FORM=USBAPI",
            "displayText" : "sailor jack's lincoln city",
            "query" : "sailor jack's lincoln city",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailing+anarchy&FORM=USBAPI",
            "displayText" : "sailing anarchy",
            "query" : "sailing anarchy",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailboats+for+sale&FORM=USBAPI",
            "displayText" : "sailboats for sale",
            "query" : "sailboats for sale",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailstn.mylabsplus.com&FORM=USBAPI",
            "displayText" : "sailstn.mylabsplus.com",
            "query" : "sailstn.mylabsplus.com",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailusfood&FORM=USBAPI",
            "displayText" : "sailusfood",
            "query" : "sailusfood",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailboats+for+sale+seattle&FORM=USBAPI",
            "displayText" : "sailboats for sale seattle",
            "query" : "sailboats for sale seattle",
            "searchKind" : "WebSearch"
        }]
    }]
}
```

## <a name="next-steps"></a>Pasos siguientes

Pruebe la API. Vaya a [Consola de pruebas de Autosuggest API](https://dev.cognitive.microsoft.com/docs/services/56c7694ecf5ff801a090fbd1/operations/56c769a2cf5ff801a090fbd2).

Para más información acerca del consumo de los objetos de respuesta, consulte [Getting Suggested Search Terms](./get-suggested-search-terms.md) (Obtención de términos de búsqueda sugerida).
