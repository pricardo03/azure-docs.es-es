---
title: Términos de búsqueda de Bing Autosuggest
titleSuffix: Microsoft Cognitive Services
description: Empareje Bing Web Search API con Bing Autosuggest API para proporcionar a los usuarios una experiencia de búsqueda mejorada.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 8/13/2018
ms.author: erhopf
ms.openlocfilehash: df8a57b3136abfacce971f4d01ccb2296dfa784c
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42889353"
---
# <a name="autosuggest-bing-search-terms"></a>Términos de búsqueda de Bing Autosuggest

Si proporciona un cuadro de búsqueda donde el usuario escribe su término de búsqueda, use [Bing Autosuggest API](../bing-autosuggest/get-suggested-search-terms.md) para mejorar la experiencia. La API devuelve cadenas consulta sugeridas basadas en términos de búsqueda parciales a medida que el usuario escribe.

Después de que el usuario escriba un término de búsqueda, se debe codificar la dirección URL antes de que se establezca el parámetro de consulta [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#query). Por ejemplo, si el usuario escribe *sailing dinghies*, establezca `q` en `sailing+dinghies` o `sailing%20dinghies`.

Si el término de consulta contiene un error ortográfico, la respuesta de búsqueda incluye un objeto [QueryContext](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#querycontext). El objeto muestra la ortografía original y la ortografía corregida que Bing utilizó para la búsqueda.

```json
"queryContext": {
    "originalQuery": "sialing dingy for sale",
    "alteredQuery": "sailing dinghy for sale",
    "alterationOverrideQuery": "+sialing +dingy for sale"
}
```

Puede usar esta información para indicar al usuario que ha modificado su cadena de consulta cuando muestre los resultados de búsqueda.

![Ejemplo de experiencia del usuario del contexto de consulta](./media/cognitive-services-bing-web-api/bing-query-context.PNG)  

## <a name="next-steps"></a>Pasos siguientes  

* Ejemplo de revisión [Respuestas de Bing Web Search API](search-responses.md).  

## <a name="see-also"></a>Otras referencias  

* [Referencia de Bing Web Search API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference)
