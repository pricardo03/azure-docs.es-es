---
title: 'Cambio de tamaño y recorte de miniaturas de Bing: Bing Image Search API'
description: Obtenga información sobre cómo cambiar el tamaño de las miniaturas que se incluyen en la respuesta de Bing Image Search API, así como recortarlas.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.assetid: F4FFAE91-A003-4F7C-8E60-83A142485E28
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: conceptual
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: de82cc5554af91294dda3826dfb394cc94dbf3d0
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2018
ms.locfileid: "46296234"
---
# <a name="resizing-and-cropping-thumbnail-images"></a>Cambio de tamaño y recorte de imágenes miniatura

Cuando se procesa una consulta de búsqueda, Bing genera información sobre las miniaturas de todas las imágenes de la [respuesta](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-get-images#bing-image-search-response-format). Esta información puede utilizarse para mostrar todas las miniaturas devueltas o un subconjunto de ellas. Si solo se va a mostrar un subconjunto, proporcione al usuario una opción para que pueda consultar las imágenes restantes.


<!-- Removing image until we can replace it with a sanatized version.
![Expanded view of thumbnail image](../bing-web-search/media/cognitive-services-bing-web-api/bing-web-image-thumbnail-expansion.PNG)
-->

Si el usuario hace clic en la miniatura, puede usar [contentUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image-contenturl) para mostrar al usuario la imagen a tamaño completo. No olvide establecer el atributo de la imagen.

Si `shoppingSourcesCount` o `recipeSourcesCount` son mayores que cero, agregue distintivos, como un carro de la compra, a la vista en miniatura para indicar que existen posibilidades de compra o recetas para el elemento de la imagen.

<!-- this is a sanitized version but we're removing all images for now until everything is sanitized.
![Shopping sources badge](./media/cognitive-services-bing-images-api/bing-images-shopping-source.PNG)
-->

Para obtener información detallada sobre la imagen, como las páginas web que incluyen la imagen o las personas que se reconocen en la imagen, use [imageInsightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image-imageinsightstoken). Para más información, consulte [Detalles de la imagen](image-insights.md).

## <a name="resizing-and-cropping-thumbnails"></a>Cambio de tamaño y recorte de miniaturas

También puede cambiar el tamaño de las miniaturas y expandirlas; por ejemplo, cuando el cursor de un usuario se sitúe encima de una de ellas.
> [!NOTE]
> No olvide establecer el atributo de la imagen si se expande. Por ejemplo, extrayendo el host de [hostPageDisplayUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image-hostpagedisplayurl) y mostrándolo debajo de la imagen.

[!INCLUDE [cognitive-services-bing-resize-crop-thumbnails](../../../includes/cognitive-services-bing-resize-crop-thumbnails.md)]
