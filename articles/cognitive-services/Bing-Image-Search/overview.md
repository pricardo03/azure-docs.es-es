---
title: ¿Qué es Bing Image Search? | Microsoft Docs
description: Obtenga información acerca de cómo usar Bing Images Search API para buscar imágenes en la Web.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 1446AD8B-A685-4F5F-B4AA-74C8E9A40BE9
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: overview
ms.date: 10/11/2017
ms.author: scottwhi
ms.openlocfilehash: 457707065059b5cb83c9d2b81f5d073cf1c89b84
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "36336526"
---
# <a name="what-is-bing-image-search"></a>¿Qué es Bing Image Search?

Bing Image Search API proporciona una experiencia similar a [Imágenes de Bing](https://www.bing.com/images), permitiendo enviar una consulta de búsqueda de usuario a Bing y obtener una lista de imágenes apropiadas.

Si va a crear una página de resultados de búsqueda solo de imágenes para buscar las imágenes que están relacionadas con la consulta de búsqueda del usuario, llame a esta API en lugar de llamar a la más general [Web Search API](../bing-web-search/search-the-web.md). Si desea imágenes y otros tipos de contenido, como páginas web, noticias y vídeos, llame a Web Search API.

## <a name="suggesting--using-search-terms"></a>Sugerencias y uso de términos de búsqueda

Si proporciona un cuadro de búsqueda donde el usuario escribe su término de búsqueda, use [Bing Autosuggest API](../bing-autosuggest/get-suggested-search-terms.md) para mejorar la experiencia. La API devuelve cadenas consulta sugeridas basadas en términos de búsqueda parciales a medida que el usuario escribe.

Una vez que el usuario escribe el término de búsqueda, codifique el término en formato de URL antes de establecer el parámetro de consulta [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#query). Por ejemplo, si el usuario escribe *sailing dinghies*, establezca `q` en `sailing+dinghies` o `sailing%20dinghies`.

## <a name="getting-images"></a>Obtención de imágenes

Para obtener imágenes relacionadas con el término de búsqueda del usuario desde la Web, envíe la solicitud GET siguiente:

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
X-MSEdge-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

Todas las solicitudes deben realizarse desde un servidor. No puede realizar llamadas desde un cliente.

Si es la primera vez que llama a cualquiera de las API de Bing, no incluya el encabezado de identificador de cliente. Solo debe incluir el identificador de cliente si se ha llamado previamente a una API de Bing y Bing ha devuelto un identificador de cliente para esa combinación de usuario y dispositivo.

Para obtener imágenes de un dominio específico, utilice el operador de consulta [site:](http://msdn.microsoft.com/library/ff795613.aspx).

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies+site:contososailing.com&mkt=en-us HTTP/1.1
```

La respuesta contiene un elemento [Images](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) que contiene una lista de imágenes que para Bing eran importantes para la consulta. Cada objeto [Image](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image) de la lista incluye la dirección URL de la imagen, su tamaño, sus dimensiones y su formato de codificación. El objeto de imagen también incluye la dirección URL de una vista en miniatura de la imagen y las dimensiones de la miniatura.

```json
{
    "name": "Rich Passage Sailing Dinghy",
    "webSearchUrl": "https:\/\/www.bing.com\/cr?IG=73118C8B4E3...",
    "thumbnailUrl": "https:\/\/tse1.mm.bing.net\/th?id=OIP.GNarK7m...",
    "datePublished": "2011-10-29T11:26:00",
    "contentUrl": "http:\/\/www.bing.com\/cr?IG=73118C8B4E3D4C3...",
    "hostPageUrl": "http:\/\/www.bing.com\/cr?IG=73118C8B4E3D4C3687...",
    "contentSize": "79239 B",
    "encodingFormat": "jpeg",
    "hostPageDisplayUrl": "en.contoso.org\/wiki\/File:Rich_Passage...",
    "width": 526,
    "height": 688,
    "thumbnail": {
        "width": 229,
        "height": 300
    },
    "imageInsightsToken": "ccid_GNarK7ma*mid_CCF85447ADA6...",
    "insightsSourcesSummary": {
        "shoppingSourcesCount": 0,
        "recipeSourcesCount": 0
    },
    "imageId": "CCF85447ADA6FFF9E96E7DF0B796F7A86E34593",
    "accentColor": "376094"
},
```

Podría mostrar un collage de todas las imágenes en miniatura o podría mostrar un subconjunto de las miniaturas. Si muestra un subconjunto, proporcione al usuario la opción para ver las imágenes restantes. Debe mostrar las imágenes en el orden proporcionado en la respuesta.

También puede expandir la miniatura cuando el usuario desplace el cursor sobre la misma. No olvide establecer el atributo de la imagen si se expande. Por ejemplo, extrayendo el host de [hostPageDisplayUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image-hostpagedisplayurl) y mostrándolo debajo de la imagen. Para obtener información acerca de cómo cambiar el tamaño de la miniatura, consulte [Cambio de tamaño y recorte de miniaturas](./resize-and-crop-thumbnails.md).

<!-- Removing image until we can replace it with a sanatized version.
![Expanded view of thumbnail image](../bing-web-search/media/cognitive-services-bing-web-api/bing-web-image-thumbnail-expansion.PNG)
-->

Si el usuario hace clic en la miniatura, puede usar [contentUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image-contenturl) para mostrar al usuario la imagen a tamaño completo. No olvide establecer el atributo de la imagen.

Si `shoppingSourcesCount` o `recipeSourcesCount` son mayores que cero, agregue distintivos, como un carro de la compra, a la vista en miniatura para indicar que existen posibilidades de compra o recetas para el elemento de la imagen.

<!-- this is a sanitized version but we're removing all images for now until everything is sanitized.
![Shopping sources badge](./media/cognitive-services-bing-images-api/bing-images-shopping-source.PNG)
-->

Para obtener información detallada sobre la imagen, como las páginas web que incluyen la imagen o las personas que se reconocen en la imagen, use [imageInsightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image-imageinsightstoken). Para más información, consulte [Detalles de la imagen](./image-insights.md).

## <a name="filtering-images"></a>Filtrado de imágenes

 De forma predeterminada, Bing Image Search API devuelve todas las imágenes que están relacionadas con la consulta. Pero si desea solo aquellas imágenes con un fondo transparente o imágenes de un tamaño específico, utilice los siguientes parámetros de consulta para filtrar las imágenes que devuelve Bing.  

- [aspect](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#aspect) (aspecto): filtra las imágenes según la relación de aspecto (por ejemplo, imágenes de pantalla estándar o extendida)
- [color](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#color): filtra las imágenes por color dominante o blanco y negro
- [freshness](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#freshness) (actualización): filtra las imágenes por edad (por ejemplo, imágenes detectadas por Bing en la última semana)
- [height](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#height), [width](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#width) (alto, ancho): filtra las imágenes por ancho y alto
- [imageContent](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagecontent): filtra las imágenes por contenido (por ejemplo, las imágenes que muestran solo la cara de una persona)
- [imageType](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagetype): filtra las imágenes por tipo (por ejemplo, imágenes prediseñadas, GIF animados o fondos transparentes)
- [license](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#license) (licencia): filtra las imágenes por el tipo de licencia asociado al sitio
- [size](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#size) (tamaño): filtra las imágenes por tamaño, como las pequeñas imágenes de hasta 200 x 200 píxeles

Para obtener imágenes de un dominio específico, utilice el operador de consulta [site:](http://msdn.microsoft.com/library/ff795613.aspx).

> [!NOTE]
> En función de la consulta, si usa el operador `site:`, existe la posibilidad de que la respuesta incluya contenido para adultos sin tener en cuenta la configuración [safeSearch](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#safesearch) (búsqueda segura). Debería usar `site:` solo si es consciente del contenido del sitio y el escenario admite la posibilidad de contenido para adultos.

En el ejemplo siguiente se muestra cómo obtener imágenes pequeñas de ContosoSailing.com que Bing ha detectado en la última semana.  

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies+site:contososailing.com&size=small&freshness=week&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

## <a name="pivoting-the-query"></a>Dinamizar la consulta

Si Bing puede segmentar la consulta de búsqueda original, el objeto [Images](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) contiene el campo `pivotSuggestions`. Por ejemplo, si la consulta original era *Microsoft Surface*, Bing podría segmentar la consulta en *Microsoft* y *Surface*.  

El ejemplo siguiente muestra las sugerencias dinámicas para *Microsoft Surface*.  

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

Podría mostrar al usuario los términos de consulta opcionales en caso de que sean de interés para el usuario.

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

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Pasos siguientes

Para empezar a trabajar rápidamente con la primera solicitud, consulte la [guía de inicio rápido de C#](quickstarts/csharp.md).

Familiarícese con la referencia de [Bing Image Search API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference). La referencia contiene la lista de los puntos de conexión, encabezados y parámetros de consulta que se utilizan para solicitar los resultados de la búsqueda. También incluye definiciones de los objetos de respuesta.

Para mejorar la experiencia de usuario del cuadro de búsqueda, consulte [Bing Autosuggest API](../bing-autosuggest/get-suggested-search-terms.md). A medida que el usuario escribe el término de consulta, puede llamar a esta API para obtener términos de consulta relacionados que fueron utilizados por otros usuarios.

No olvide leer los [Requisitos de visualización y uso de Bing](./useanddisplayrequirements.md) para evitar infringir alguna de las reglas sobre el uso de los resultados de búsqueda.

Cuando se llama a Bing Image Search API, Bing devuelve una lista de resultados. La lista es un subconjunto del número total de resultados que están relacionados con la consulta. El campo `totalEstimatedMatches` de la respuesta contiene una estimación del número de imágenes que están disponibles para ver. Para más información acerca de cómo paginar a través de las imágenes restantes, consulte [Paginación de imágenes](./paging-images.md).