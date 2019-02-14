---
title: 'Inicio rápido: Bing Spell Check API'
titlesuffix: Azure Cognitive Services
description: Se muestra cómo empezar a usar Bing Spell Check API.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: quickstart
ms.date: 06/21/2016
ms.author: scottwhi
ms.openlocfilehash: 70aec949dd8f1b27a2a04c28a892b4caa2122844
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55867824"
---
# <a name="quickstart-your-first-spell-check-request"></a>Inicio rápido: La primera solicitud de corrector ortográfico

Obtenga una [clave de acceso de Cognitive Services](https://azure.microsoft.com/try/cognitive-services/) en **Buscar**.  Consulte también [Precios de Cognitive Services - Bing Search API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

Para buscar en una cadena de texto existente errores ortográficos y gramaticales, enviaría una solicitud GET al punto de conexión siguiente:  
  
```
https://api.cognitive.microsoft.com/bing/v5.0/spellcheck
```

> [!NOTE]
> Punto de conexión de la versión preliminar de V7:
> 
> ```
> https://api.cognitive.microsoft.com/bing/v7.0/spellcheck
> ```  
  
La solicitud debe usar el protocolo HTTPS.

Se recomienda que todas las solicitudes procedan de un servidor. Al distribuir la clave como parte de una aplicación cliente, existe una mayor probabilidad de que un tercero malintencionado acceda a ella. Además, la realización de llamadas desde un servidor proporciona un único punto de actualización para futuras versiones de la API.

La solicitud debe especificar el parámetro de consulta [text](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#text), que contiene la cadena de texto para probar. Aunque es opcional, la solicitud debe especificar también el parámetro de consulta [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#mkt), que identifica el mercado de donde desea que procedan los resultados. Para ver una lista de parámetros de consulta opcionales, como `mode`, consulte [Query Parameters](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#query-parameters) (Parámetros de consulta). Todos los valores de parámetro de consulta deben estar codificados con una dirección URL.  
  
La solicitud debe especificar el encabezado [Ocp-Apim-Subscription-Key](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#subscriptionkey). Aunque es opcional, se recomienda especificar los encabezados siguientes:  
  
-   [User-Agent](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#useragent)  
-   [X-MSEdge-ClientID](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#clientid)  
-   [X-Search-ClientIP](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#clientip)  
-   [X-Search-Location](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#location)  

Para ver una lista de todos los encabezados de solicitud y respuesta, consulte [Encabezados](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#headers).

## <a name="the-request"></a>La solicitud

A continuación se muestra una solicitud que incluye todos los encabezados y parámetros de consulta sugeridos. Si es la primera vez que llama a cualquiera de las API de Bing, no incluya el encabezado de identificador de cliente. Solo debe incluir el identificador de cliente si se ha llamado previamente a una API de Bing y Bing ha devuelto un identificador de cliente para esa combinación de usuario y dispositivo. 
  
```  
GET https://api.cognitive.microsoft.com/bing/v5.0/spellcheck?text=when+its+your+turn+turn,+john,+come+runing&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

> [!NOTE]
> Solicitud de versión preliminar de V7:

> ```  
> GET https://api.cognitive.microsoft.com/bing/v7.0/spellcheck?text=when+its+your+turn+turn,+john,+come+runing&mkt=en-us HTTP/1.1
> Ocp-Apim-Subscription-Key: 123456789ABCDE  
> X-MSEdge-ClientIP: 999.999.999.999  
> X-Search-Location: lat:47.60357;long:-122.3295;re:100  
> X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
> Host: api.cognitive.microsoft.com  
> ```  

A continuación se muestra la respuesta a la solicitud anterior. En el ejemplo también se muestran los encabezados de respuesta específicos de Bing.

```
BingAPIs-TraceId: 76DD2C2549B94F9FB55B4BD6FEB6AC
X-MSEdge-ClientID: 1C3352B306E669780D58D607B96869
BingAPIs-Market: en-US

{  
    "_type" : "SpellCheck",  
    "flaggedTokens" : [{  
        "offset" : 5,  
        "token" : "its",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "it's",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 25,  
        "token" : "john",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "John",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 19,  
        "token" : "turn",  
        "type" : "RepeatedToken",  
        "suggestions" : [{  
            "suggestion" : "",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 35,  
        "token" : "runing",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "running",  
            "score" : 1  
        }]  
    }]  
}  
```  


## <a name="next-steps"></a>Pasos siguientes

Pruebe la API. Vaya a la [consola de pruebas de Spell Check API](https://dev.cognitive.microsoft.com/docs/services/56e73033cf5ff80c2008c679/operations/57855119bca1df1c647bc358). 

Para más información sobre cómo consumir los objetos de respuesta, consulte [Spell check text strings](./proof-text.md) (Cadenas de texto de Spell Check).

