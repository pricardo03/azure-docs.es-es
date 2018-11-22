---
title: ¿Qué es Bing Custom Search?
titlesuffix: Azure Cognitive Services
description: Proporciona una descripción general de Bing Custom Search.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: overview
ms.date: 09/29/2017
ms.author: aahi
ms.openlocfilehash: 2be1d76cc37de2ce2965cc08c116b37286bd8b52
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2018
ms.locfileid: "52163252"
---
# <a name="what-is-bing-custom-search"></a>¿Qué es Bing Custom Search?

Bing Custom Search le permite crear experiencias de búsqueda a medida de los temas que le interesan. Por ejemplo, si tiene un sitio web que proporciona una experiencia de búsqueda, puede especificar los dominios, sitios web y páginas web en los que busca Bing. Los usuarios ven los resultados de búsqueda a la medida del contenido que les interesa en lugar de recorrer las páginas de los resultados de búsqueda que pueden contener contenido carente de interés.

Para crear la vista personalizada de la web, use el [portal](https://customsearch.ai) de Bing Custom Search. El portal le permite crear una instancia de búsqueda personalizada que especifique los dominios, los sitios web y las páginas web en los que quiere que busque Bing, y aquellos sitios web en los que no quiere que busque. Además de especificar las direcciones URL del contenido que conoce, también puede usar el portal para buscar contenido pertinente que quizás quiera agregar.

El portal también le permite anclar una página web a la parte superior del resultado de búsqueda si el usuario escribe un término de búsqueda específico. 

Después de definir la instancia, puede integrar la búsqueda personalizada en su aplicación móvil, aplicación de escritorio o sitio web mediante una llamada a Custom Search API. Si tiene una aplicación o sitio basado en web, puede permitir que la interfaz de usuario hospedada represente la interfaz de búsqueda automáticamente.

La siguiente imagen muestra la simplicidad de la integración de la búsqueda personalizada.

![imagen alt](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/cognitive-services/Bing-Custom-Search/media/BCS-Overview.png "Funcionamiento de Bing Custom Search")

## <a name="adding-custom-search-box-suggestions"></a>Agregar sugerencias personalizadas del cuadro de búsqueda

Puede enriquecer su experiencia de búsqueda personalizada gracias a las sugerencias personalizadas del cuadro de búsqueda. Esta característica le permite proporcionar sugerencias de búsqueda personalizadas que sean relevantes para su experiencia de búsqueda. Cuando el usuario escribe algo en el cuadro de búsqueda, la lista desplegable que se muestra contiene las cadenas de consulta sugeridas en función de la cadena de consulta parcial del usuario. Puede especificar si desea devolver solo sugerencias personalizadas o también incluir las sugerencias de Bing. [Más información](define-custom-suggestions.md).

## <a name="adding-custom-image-search-experience"></a>Agregar la experiencia personalizada de búsqueda de imágenes

Puede enriquecer su experiencia de búsqueda personalizada con imágenes. De forma similar a los resultados web, la búsqueda personalizada permite buscar imágenes en la lista de sitios web de su instancia. [Más información](get-images-from-instance.md).

## <a name="adding-custom-video-search-experience"></a>Agregar la experiencia personalizada de búsqueda de vídeos

Puede enriquecer su experiencia de búsqueda personalizada con vídeos. De forma similar a los resultados web, la búsqueda personalizada permite buscar vídeos en la lista de sitios web de su instancia. [Más información](get-videos-from-instance.md).

## <a name="sharing-your-custom-search-instance-with-others"></a>Compartir la instancia de búsqueda personalizada con otros usuarios

Puede permitir fácilmente la edición y prueba colaborativas de la instancia al compartirla con miembros del equipo. [Más información](share-your-custom-search.md).

## <a name="next-steps"></a>Pasos siguientes

Para iniciar rápidamente, consulte [Create your first Bing Custom Search instance](quick-start.md) (Creación de la primera instancia de Bing Custom Search).

Para obtener más información sobre cómo personalizar su instancia de búsqueda, consulte [Define a custom search instance](define-your-custom-view.md) (Definir una instancia de búsqueda personalizada).

Familiarícese con el contenido de referencia de cada uno de los puntos de conexión de búsqueda personalizada. La referencia contiene los puntos de conexión, los encabezados y los parámetros de consulta que se utilizan para solicitar los resultados de la búsqueda. También incluye definiciones de los objetos de respuesta.

- [Custom Search API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference)
- [Custom Image API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-images-api-v7-reference)
- [Custom Video API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-videos-api-v7-reference)
- [Custom Autosuggest API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-autosuggest-api-v7-reference)


No olvide leer los [Requisitos de visualización y uso de Bing](./use-and-display-requirements.md) para evitar infringir alguna de las reglas sobre el uso de los resultados de búsqueda.
