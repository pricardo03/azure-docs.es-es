---
title: 'Ejemplos de información de Bing: Bing Visual Search'
titleSuffix: Azure Cognitive Services
description: Muestra ejemplos de la información de imágenes que se muestra en Bing.com.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: scottwhi
ms.openlocfilehash: 94f8d4ea98f42ad919597ef53dc63281825f0e6b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60610320"
---
# <a name="examples-of-bing-insights-usage"></a>Ejemplos del uso de la información de Bing

Este artículo contiene ejemplos de cómo Bing puede mostrar y usar información de imágenes en Bing.com.

## <a name="pagesincluding-insight-example"></a>Ejemplo de páginas que incluyen la información

A continuación muestra un vínculo a la primera página de Web y permite al usuario expandir y contraer la lista de otras páginas Web que incluyen la imagen:

![Páginas expandidas con información](./media/pages-including.PNG)

## <a name="shoppingsources-insight-example"></a>Ejemplo orígenes de compra de la información

A continuación muestra cómo Bing podría mostrar orígenes de la compra de productos que se muestra en la imagen:

![Orígenes de compra](./media/shopping-sources.PNG)

## <a name="visualsearch-insight-example"></a>Ejemplo de búsqueda visual de la información

La siguiente muestra cómo Bing podría mostrar imágenes similares visualmente (consulte **imágenes relacionadas con** en el ejemplo):

![Imágenes visualmente similares](./media/similar-images.PNG)

## <a name="recipes-insight-example"></a>Ejemplo de recetas de la información

Lo siguiente muestra cómo Bing podría mostrar recetas de los alimentos que se muestran en la imagen. El ejemplo permite al usuario saber hay recetas disponibles:

![Recetas y páginas con información](./media/recipes-pages-including.PNG)

 Y proporciona el vínculo a las recetas cuando el usuario expande la lista:

![Páginas con recetas expandidas con información](./media/expanded-recipes-pages-including.PNG)

## <a name="relatedsearches-insight-example"></a>Ejemplo de búsquedas relacionadas con la información

Lo siguiente muestra cómo Bing puede mostrar búsquedas relacionadas de imágenes realizadas por otros. Si el usuario hace clic en la imagen, se le lleva a la página de resultados de búsqueda de imágenes de Bing.com/images para esa consulta relacionada.

![Búsquedas de imágenes relacionadas](./media/bordered-related-searches.PNG)

## <a name="entity-insight-example"></a>Ejemplo de entidades de información

Lo siguiente muestra cómo Bing puede mostrar información sobre la entidad (persona, lugar o cosa) mostrada en la imagen. Si el usuario hace clic en el vínculo de la entidad, el usuario se realiza a la página de resultados de búsqueda de Bing.com para la entidad:

![Entidad mostrada en la imagen](./media/entity.PNG)

## <a name="displaying-other-insights-that-the-user-might-explore"></a>Visualización de otra información que el usuario podría explorar

Lo siguiente explica cómo Bing puede mostrar otra información sobre la imagen que el usuario puede explorar.

![Exploración de otra información sobre la imagen](./media/apple-pie-more-tags.PNG)

## <a name="bounding-boxes-and-hot-spots"></a>Rectángulos delimitadores y zonas activas

Las etiquetas no predeterminadas incluyen el rectángulo delimitador que identifica el área de interés en la imagen a la que se aplica la etiqueta. Si el rectángulo delimitador no identifica toda la imagen, utilice este rectángulo para crear una zona activa en la imagen. El usuario puede hacer clic en la zona activa para obtener información relacionada con el contenido que se encuentra bajo dicha zona activa (o rectángulo). Por ejemplo, si la imagen es un modo de alta, los resultados pueden contener etiquetas (y cuadros de límite) para accesorios que se muestra en la imagen, por ejemplo, un bolso, joyas, scarfs y así sucesivamente. El ejemplo siguiente muestra un rectángulo puntos problemáticos para el que se muestra en la imagen de gafas de sol:

![Rectángulo delimitador y zona activa](./media/click-to-search.PNG)

## <a name="next-steps"></a>Pasos siguientes

Para empezar a trabajar con la primera solicitud, consulte las guías de inicio rápido: [C#](quickstarts/csharp.md) | [Java](quickstarts/java.md) | [node.js](quickstarts/nodejs.md) | [Python](quickstarts/python.md).





