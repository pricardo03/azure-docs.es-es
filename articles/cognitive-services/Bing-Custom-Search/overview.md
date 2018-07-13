---
title: ¿Qué es Bing Custom Search? | Microsoft Docs
description: Proporciona una descripción detallada de Bing Custom Search.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/29/2017
ms.author: v-brapel
ms.openlocfilehash: 7cd61fc63d0d7734b842ed222c67c6753da9a418
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381011"
---
# <a name="what-is-bing-custom-search"></a>¿Qué es Bing Custom Search?

Bing Custom Search le permite crear experiencias de búsqueda a medida de los temas que le interesan. Por ejemplo, si tiene un sitio web que proporciona una experiencia de búsqueda, puede especificar los dominios, sitios web y páginas web en los que busca Bing. Los usuarios ven los resultados de búsqueda a la medida del contenido que les interesa en lugar de recorrer las páginas de los resultados de búsqueda que pueden contener contenido carente de interés.

Para crear la vista personalizada de la web, use el [portal](https://customsearch.ai) de Bing Custom Search. El portal le permite crear una instancia de búsqueda personalizada que especifique los dominios, los sitios web y las páginas web en los que quiere que busque Bing, y aquellos sitios web en los que no quiere que busque. Además de especificar las direcciones URL del contenido que conoce, también puede usar el portal para buscar contenido pertinente que quizás quiera agregar.

El portal también le permite anclar una página web a la parte superior del resultado de búsqueda si el usuario escribe un término de búsqueda específico. 

Después de definir la instancia, puede integrar la búsqueda personalizada en su aplicación móvil, aplicación de escritorio o sitio web mediante una llamada a Custom Search API. Si tiene una aplicación o sitio basado en web, puede permitir que la interfaz de usuario hospedada represente la interfaz de búsqueda automáticamente.

La siguiente imagen muestra la simplicidad de la integración de la búsqueda personalizada.

![imagen alt](./media/bcs-overview.png "Funcionamiento de Bing Custom Search")

## <a name="customize-search-suggestions"></a>Personalización de las sugerencias de búsqueda

Si está suscrito a Custom Search en el nivel adecuado (consulte las [páginas de precios](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/)), puede personalizar las sugerencias de búsqueda realizadas en su experiencia de Custom Search. Custom Autosuggest API devuelve una lista de consultas sugeridas basándose en una cadena de consulta parcial proporcionada por el usuario. Con Custom Autosuggest, proporcione sugerencias de búsqueda personalizadas pertinentes para su experiencia de búsqueda. Especifique si desea devolver solo sugerencias personalizadas o incluir sugerencias de Bing. Si se incluyen las sugerencias de Bing, las sugerencias personalizadas aparecen antes de las sugerencias que ofrece Bing. Las sugerencias de Bing están restringidas al contexto de la instancia de Custom Search.

## <a name="next-steps"></a>Pasos siguientes

Para iniciar rápidamente, consulte [Create your first Bing Custom Search instance](quick-start.md) (Creación de la primera instancia de Bing Custom Search).

Para obtener más información sobre las opciones disponibles para personalizar su instancia de búsqueda, vea [Define a custom search instance](define-your-custom-view.md) (Definición de una instancia de búsqueda personalizada).

Familiarícese con la referencia de [Custom Search API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference). La referencia contiene la lista de los puntos de conexión, encabezados y parámetros de consulta que se utilizan para solicitar los resultados de la búsqueda. También incluye definiciones de los objetos de respuesta.

Para obtener información sobre cómo personalizar sugerencias, consulte [Define custom search suggestions](define-custom-suggestions.md) (Definición de sugerencias de búsqueda personalizadas).

No olvide leer los [Requisitos de visualización y uso de Bing](./use-and-display-requirements.md) para evitar infringir alguna de las reglas sobre el uso de los resultados de búsqueda.