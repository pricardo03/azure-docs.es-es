---
title: Envío de consultas a Bing Image Search API | Microsoft Docs
description: Obtenga información acerca del envío y la personalización de las consultas de búsqueda que se envían a Bing Image Search API.
services: cognitive-services
author: aahill
manager: cgronlun
ms.assetid: C2862E98-8BCC-423B-9C4A-AC79A287BE38
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: conceptual
ms.date: 8/8/2018
ms.author: aahi
ms.openlocfilehash: d74f59ffcf095e639686a3ada3b09dac988fc544
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2018
ms.locfileid: "40082586"
---
# <a name="sending-queries-to-the-bing-image-search-api"></a>Envío de consultas a Bing Image Search API

Bing Image Search API proporciona una experiencia similar a Bing.com/Images al permitir enviar una consulta de búsqueda de usuario a Bing y obtener una lista de imágenes apropiadas.

## <a name="using-and-suggesting-search-terms"></a>Uso y sugerencias de términos de búsqueda

Después de especificar un término de búsqueda, codifique el término en formato de URL antes de establecer el parámetro de consulta [**q**](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#query). Por ejemplo, si un usuario escribe *sailing dinghies*, establezca `q` en `sailing+dinghies` o `sailing%20dinghies`.

Si la aplicación tiene un cuadro de búsqueda donde se escriben los términos de búsqueda, puede usar [Bing Autosuggest API](../../bing-autosuggest/get-suggested-search-terms.md) para mostrar los términos de búsqueda sugeridos en tiempo real y mejorar la experiencia. La API devuelve sugerencias de cadenas de consulta basadas en términos de búsqueda parciales y Azure Cognitive Services.

## <a name="pivoting-the-query"></a>Dinamizar la consulta

Si Bing puede segmentar la consulta de búsqueda original, el objeto [Images](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) devuelto contendrá el elemento `pivotSuggestions`, que se puede mostrar como términos de búsqueda opcionales al usuario. Por ejemplo, si la consulta original era *Microsoft Surface*, Bing podría segmentar la consulta en *Microsoft* y *Surface* y proporcionar las sugerencias dinámicas de cada una. Estas se pueden mostrar como términos de consulta opcionales al usuario.

El ejemplo siguiente muestra las sugerencias dinámicas para *Microsoft Surface*:  

```json
{
    "_type": "Images",
    "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=microsoft%20surface&FORM=OIIARP",
    "totalEstimatedMatches": 1000,
    "value": [...],
    "queryExpansions": [...],
    "pivotSuggestions": [{
        "pivot": "microsoft",
        "suggestions": [{
            "text": "Contoso Surface",
            "displayText": "Contoso",
            "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=OtterBox+Surface&FORM=IRQBPS",
            "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=Contoso...",
                    "searchLink": "https:\/\/api.cognitive.microsoft.com\/api...",
            "thumbnail": {
                "thumbnailUrl": "https:\/\/tse3.mm.bing.net\/th?q=Contoso+Surface..."
            }
        },
        {
            "text": "Adatum Surface",
            "displayText": "Adatum",
            "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Adatum+Surface&FORM=IRQBPS",
            "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=...",
            "thumbnail": {
                "thumbnailUrl": "https:\/\/tse3.mm.bing.net\/th?q=Adatum+Surface&pid=Ap..."
            }
        },
        ...
        ]
    },
    {
        "pivot": "surface",
        "suggestions": [{
            "text": "Microsoft Surface4",
            "displayText": "Surface4",
            "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Surface...",
            "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?...",
            "thumbnail": {
                "thumbnailUrl": "https:\/\/tse4.mm.bing.net\/th?q=Microsoft..."
            }
        },
        {
            "text": "Microsoft Tablet",
            "displayText": "Tablet",
            "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Tablet&FORM=IRQBPS",
            "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?...",
            "thumbnail": {
                "thumbnailUrl": "https:\/\/tse3.mm.bing.net\/th?q=Microsoft+Tablet..."
            }
        },
        ...
    ],
    "nextOffsetAddCount": 0
}
```

El campo `pivotSuggestions` contiene la lista de segmentos (áreas dinámicas) en los que se ha dividido la consulta original. Para cada área dinámica, la respuesta contiene una lista de objetos [Query](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#query_obj) que contienen las consultas sugeridas. El campo `text` contiene la consulta sugerida y el campo `displayText` contiene el término que reemplaza al área dinámica en la consulta original. Por ejemplo, Release Date of Surface (fecha de lanzamiento de Surface).

Puede usar los campos `text` y `thumbnail` para mostrar al usuario las cadenas de consulta dinámica en caso de que la cadena de consulta dinámica sea realmente lo que están buscando. Ofrezca al usuario la posibilidad de hacer clic en la miniatura y el texto mediante la dirección URL `webSearchUrl` o la dirección URL `searchLink`. Use `webSearchUrl` para enviar al usuario a los resultados de búsqueda de Bing o `searchLink` si proporciona su propia página de resultados.

<!-- Need a sanitized version of the image
The following shows an example of the pivot queries.

![Pivot suggestions](./media/cognitive-services-bing-images-api/bing-image-pivotsuggestion.GIF)
-->

## <a name="expanding-the-query"></a>Expansión de la consulta

Si Bing puede expandir la consulta para restringir la búsqueda original, el objeto [Images](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) contiene el campo `queryExpansions`. Por ejemplo, si la consulta era *Microsoft Surface*, las consultas expandidas podrían ser: Microsoft Surface **Pro 3**, Microsoft Surface **RT**, Microsoft Surface **Phone** y Microsoft Surface **Hub**.

El ejemplo siguiente muestra las consultas expandidas para *Microsoft Surface*.

```json
{
    "_type": "Images",
    "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=microsoft%20surface...",
    "totalEstimatedMatches": 1000,
    "value": [...],
    "queryExpansions":  [{
        "text": "Microsoft Surface Pro 3",
        "displayText": "Pro 3",
        "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Surface+Pro+3...",
        "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=Microsoft...",
        "thumbnail": {
            "thumbnailUrl": "https:\/\/tse4.mm.bing.net\/th?q=Microsoft+Surface+Pro+3..."
        }
    },
    {
        "text": "Microsoft Surface RT",
        "displayText": "RT",
        "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Surface+RT...",
        "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=...",
        "thumbnail": {
            "thumbnailUrl": "https:\/\/tse4.mm.bing.net\/th?q=Microsoft+Surface+RT..."
        }
    },
    {
        "text": "Microsoft Surface Phone",
        "displayText": "Phone",
        "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Surface+Phone",
        "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=...",
        "thumbnail": {
            "thumbnailUrl": "https:\/\/tse4.mm.bing.net\/th?q=Microsoft+Surface+Phone..."
        }
    }],
    "pivotSuggestions": [...],
    "nextOffsetAddCount": 0
}
```

El campo `queryExpansions` contiene una lista de objetos [Query](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#query_obj). El campo `text` contiene la consulta expandida y el campo `displayText` contiene el término de expansión. Puede usar los campos `text` y `thumbnail` para mostrar al usuario las cadenas de consulta expandidas en caso de que la cadena de consulta expandida sea realmente lo que están buscando. Ofrezca al usuario la posibilidad de hacer clic en la miniatura y el texto mediante la dirección URL `webSearchUrl` o la dirección URL `searchLink`. Use `webSearchUrl` para enviar al usuario a los resultados de búsqueda de Bing o `searchLink` si proporciona su propia página de resultados.

<!-- Removing until we can replace with a sanitized image.
The following shows an example Bing implementation that uses expanded queries. If the user clicks the Microsoft Surface Pro 3 link, they're taken to the Bing search results page, which shows them images of the Pro 3.

![Query expansion suggestions](./media/cognitive-services-bing-images-api/bing-image-queryexpansion.GIF)
-->


## <a name="throttling-requests"></a>Solicitudes de limitación

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Pasos siguientes

Si no ha probado Bing Image Search API antes, consulte una [guía de inicio rápido](../quickstarts/csharp.md). Si está buscando algo más complejo, consulte el tutorial para crear una [aplicación web de página única](../tutorial-bing-image-search-single-page-app.md).
