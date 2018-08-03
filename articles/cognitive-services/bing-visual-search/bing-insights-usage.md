---
title: Ejemplos de la información de Bing | Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Muestra ejemplos de la información de imágenes que se muestra en Bing.com.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 04/17/2018
ms.author: scottwhi
ms.openlocfilehash: 927a02b400c7a7b250b315ad5668feb198fd590a
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2018
ms.locfileid: "39005696"
---
# <a name="examples-of-bing-insights-usage"></a>Ejemplos del uso de la información de Bing

Esta sección contiene ejemplos de cómo Bing puede mostrar información en Bing.com.

## <a name="pagesincluding-insight-example"></a>Ejemplo de páginas que incluyen la información

A continuación se muestra cómo Bing puede mostrar páginas web que incluyan la imagen. En el ejemplo se muestra un vínculo a la primera página web y permite al usuario expandir y contraer la lista de otras páginas web que incluyen la imagen.

![Páginas expandidas con información](./media/pages-including.PNG)


## <a name="shoppingsources-insight-example"></a>Ejemplo orígenes de compra de la información

En la ilustración siguiente se indica cómo Bing podría mostrar orígenes de compras de productos que se muestran en la imagen.

![Orígenes de compra](./media/shopping-sources.PNG)


## <a name="visualsearch-insight-example"></a>Ejemplo de búsqueda visual de la información

Lo siguiente muestra cómo Bing podría mostrar imágenes visualmente similares (consulte las **imágenes relacionadas** en el ejemplo).

![Imágenes visualmente similares](./media/similar-images.PNG)

## <a name="recipes-insight-example"></a>Ejemplo de recetas de la información

Lo siguiente muestra cómo Bing podría mostrar recetas de los alimentos que se muestran en la imagen. El ejemplo permite al usuario saber qué recetas están disponibles.

![Recetas y páginas con información](./media/recipes-pages-including.PNG)

 Y proporciona el vínculo a las recetas cuando el usuario expande la lista.

![Páginas con recetas expandidas con información](./media/expanded-recipes-pages-including.PNG)


## <a name="relatedsearches-insight-example"></a>Ejemplo de búsquedas relacionadas con la información

Lo siguiente muestra cómo Bing puede mostrar búsquedas relacionadas de imágenes realizadas por otros. Si el usuario hace clic en la imagen, se le lleva a la página de resultados de búsqueda de imágenes de Bing.com/images para esa consulta relacionada.

![Búsquedas de imágenes relacionadas](./media/bordered-related-searches.PNG)


## <a name="entity-insight-example"></a>Ejemplo de entidades de información

Lo siguiente muestra cómo Bing puede mostrar información sobre la entidad (persona, lugar o cosa) mostrada en la imagen. Si el usuario hace clic en el vínculo de la entidad, el usuario accede a la página de resultados de búsqueda de Bing.com para la entidad.

![Entidad mostrada en la imagen](./media/entity.PNG)


## <a name="displaying-other-insights-that-the-user-might-explore"></a>Visualización de otra información que el usuario podría explorar

Lo siguiente explica cómo Bing puede mostrar otra información sobre la imagen que el usuario puede explorar.

![Exploración de otra información sobre la imagen](./media/apple-pie-more-tags.PNG)


## <a name="bounding-boxes-and-hot-spots"></a>Rectángulos delimitadores y zonas activas

Las etiquetas no predeterminadas incluyen el rectángulo delimitador que identifica el área de interés en la imagen a la que se aplica la etiqueta. Si el rectángulo delimitador no identifica toda la imagen, utilice este rectángulo para crear una zona activa en la imagen. El usuario puede hacer clic en la zona activa para obtener información relacionada con el contenido que se encuentra bajo dicha zona activa (o rectángulo). Por ejemplo, si la imagen es una imagen de moda, los resultados pueden contener etiquetas (y rectángulos delimitadores) para los accesorios mostrados en la imagen, como un bolso, joyas, bufandas, etc. El siguiente ejemplo muestra un rectángulo de zona activa para las gafas de sol que se muestran en la imagen.

![Rectángulo delimitador y zona activa](./media/click-to-search.PNG)



## <a name="next-steps"></a>Pasos siguientes

Para revisar el código JSON detrás de estos ejemplos, consulte la [información predeterminada](default-insights-tag.md) y la [respuesta JSON](overview.md#the-response).

Para empezar a trabajar rápidamente con la primera solicitud, consulte las guías de inicio rápido sobre [C#](quickstarts/csharp.md) | [Java](quickstarts/java.md) | [node.js](quickstarts/nodejs.md) | [Python](quickstarts/python.md)





