---
title: ¿Qué es Bing Autosuggest?
titlesuffix: Azure Cognitive Services
description: Aprenda a usar Bing Autosuggest API.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-autosuggest
ms.topic: overview
ms.date: 09/12/2017
ms.author: scottwhi
ms.openlocfilehash: e1e1ec2a9f5329f5d7331b7ce3ced4924d001a49
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55174183"
---
# <a name="what-is-bing-autosuggest"></a>¿Qué es Bing Autosuggest?

Si envía consultas a cualquier Bing Search API, puede usar Bing Autosuggest API para mejorar su experiencia con el cuadro de búsqueda. Bing Autosuggest API devuelve una lista de consultas sugeridas basadas en la cadena de consulta parcial que el usuario escribe en el cuadro de búsqueda. Visualice las sugerencias en la lista desplegable del cuadro de búsqueda. Los términos sugeridos se basan en las consultas sugeridas que otros usuarios han buscado y la intención del usuario.

Por lo general, se llama a esta API cada vez que el usuario escribe un carácter nuevo en el cuadro de búsqueda. La integridad de la cadena de consulta afecta a la relevancia de los términos de la consulta sugeridos que devuelve la API. Cuanto más completa sea la cadena de consulta, más apropiada será la lista de términos de consulta sugeridos. Por ejemplo, las sugerencias que puede devolver la API para *s* es probable que tengan menos relevancia que las consultas que devuelve para *veleros*.

## <a name="getting-suggested-search-terms"></a>Obtención de términos de búsqueda sugeridos

En el ejemplo siguiente se muestra una solicitud que devuelve las cadenas de consulta sugeridas para *navegar*. Recuerde codificar como dirección URL el término de consulta parcial del usuario al establecer el parámetro de consulta [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v7-reference#query). Por ejemplo, si el usuario escribe *sailing les*, establezca `q` en `sailing+les` o `sailing%20les`.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/suggestions?q=sail&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
X-MSEdge-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

La respuesta siguiente contiene una lista de objetos [SearchAction](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v7-reference#searchaction) que contienen los términos de consulta sugeridos.

```json
{
    "url" : "https:\/\/www.bing.com\/search?q=sailing+lessons+seattle&FORM=USBAPI",
    "displayText" : "sailing lessons seattle",
    "query" : "sailing lessons seattle",
    "searchKind" : "WebSearch"
}, ...
```

Cada sugerencia incluye un campo `displayText`, `query` y `url`. El campo `displayText` contiene la consulta sugerida que usa para rellenar la lista desplegable del cuadro de búsqueda. Debe mostrar todas las sugerencias que incluye la respuesta, y en el orden especificado.

A continuación se muestra un ejemplo de cuadro de búsqueda desplegable con términos de consulta sugeridos.

![Lista del cuadro de búsqueda desplegable de AutoSuggest](./media/cognitive-services-bing-autosuggest-api/bing-autosuggest-drop-down-list.PNG)

Si el usuario selecciona una consulta sugerida en la lista desplegable, puede usar la cadena de consulta del campo `query` para llamar a [Bing Web Search API](../bing-web-search/search-the-web.md) y ver los resultados por sí mismo. O bien, puede usar la dirección URL del campo `url` para enviar al usuario a la página de resultados de la búsqueda de Bing.

## <a name="throttling-requests"></a>Solicitudes de limitación

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Pasos siguientes

Para comenzar rápidamente con su primera solicitud, consulte [Making Your First Query](quickstarts/csharp.md) (Realizar su primera consulta).

Familiarícese con la referencia de [Bing Autosuggest API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v7-reference). La referencia contiene la lista de puntos de conexión, encabezados y parámetros de consulta que se usan para solicitar términos de consulta sugeridos y las definiciones de los objetos de la respuesta.

Aprenda a realizar búsquedas en la web mediante [Bing Web Search API](../bing-web-search/search-the-web.md).

No olvide leer los [Requisitos de visualización y uso de Bing](./useanddisplayrequirements.md) para evitar infringir alguna de las reglas sobre el uso de los resultados de búsqueda.
