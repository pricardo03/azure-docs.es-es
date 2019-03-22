---
title: ¿Qué son las versiones de Bing Search API?
titleSuffix: Azure Cognitive Services
description: Use este artículo para conocer las versiones de Bing Search API y cómo habilitar las búsquedas cognitivas en Internet en sus aplicaciones y servicios.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: article
ms.date: 03/12/2019
ms.author: aahi
ms.openlocfilehash: 5a883fcb3533374afbbf946281b6a4a1e9a2912e
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57903120"
---
# <a name="what-are-the-bing-search-apis"></a>¿Qué son las versiones de Bing Search API?

Bing Search APIs le permiten crear aplicaciones conectadas a la web y servicios que buscar las páginas Web, imágenes, noticias, ubicaciones y mucho más gracias sin anuncios. Mediante el envío de solicitudes de búsqueda con las API REST o los SDK de Bing Search, puede obtener información y contenido apropiados para búsquedas web. Use este artículo para obtener información sobre la búsqueda de Bing diferentes API y cómo puede integrar cognitivas búsquedas en sus aplicaciones y servicios. Los precios y límites de velocidad pueden variar según la API.

## <a name="the-bing-web-search-api"></a>Bing Web Search API

[Bing Web Search API](../Bing-Web-Search/index.yml) devuelve páginas web, imágenes, vídeo, noticias y mucho más. Puede filtrar las consultas de búsqueda que se envían a esta API para incluir o excluir ciertos tipos de contenido.

Considere el uso de Bing Web Search API en aplicaciones que necesitan buscar todos los tipos de contenido web apropiado. Si la aplicación busca un tipo específico de contenido en línea, considere una de las siguientes API de búsqueda:

## <a name="content-specific-bing-search-apis"></a>API de búsqueda de Bing específicas de contenido

La siguiente API de búsqueda de Bing devuelve contenido específico desde la web, como imágenes, noticias, vídeos y negocios locales.

| API de Bing | DESCRIPCIÓN |
| -- | -- |
| [Entity Search](../Bing-Entities-Search/index.yml) | Bing Entity Search API devuelve resultados de búsqueda que contienen entidades, que pueden ser personas, lugares o cosas. Dependiendo de la consulta, la API devolverá una o más entidades que satisfacen la consulta de búsqueda. La consulta de búsqueda puede incluir personas importantes, negocios locales, puntos de referencia, destinos y mucho más. |
| [Image Search](../Bing-Image-Search/index.yml) | Bing Image Search API permite buscar y búsqueda estáticos y animados imágenes de alta calidad similar a [Bing.com/images](https://www.Bing.com/images). Puede refinar las búsquedas para incluir o excluir imágenes por atributo, como tamaño, color, licencia y novedad. También puede buscar imágenes populares, cargar imágenes para obtener conclusiones sobre ellas y mostrar vistas previas en miniatura. |
| [News Search](../Bing-News-Search/index.yml) | Bing News Search API permite buscar noticias similar a [Bing.com/news](https://www.Bing.com/news). La API devuelve artículos de noticias de varios orígenes o dominios concretos. Puede buscar entre las categorías para obtener artículos populares, artículos destacados y titulares. |
| [Video Search](../Bing-Video-Search/index.yml) | Bing Video Search API permite buscar vídeos en la web. Obtenga vídeos populares, contenido relacionado y vistas previas en miniatura. |
| [Visual Search](../Bing-visual-search/index.yml) | Cargue una imagen o use una dirección URL para obtener información detallada sobre ella, como productos visualmente parecidos, imágenes y búsquedas relacionadas. |
 [Local Business Search](../bing-local-business-search/index.yml) | La API de búsqueda de Bing Local empresarial permite encontrar información de contacto y la ubicación sobre negocios locales basadas en consultas de búsqueda a sus aplicaciones. |

## <a name="the-bing-custom-search-api"></a>Bing Custom Search API

Creación de una instancia de custom search con la [Bing Custom Search](../Bing-Custom-Search/index.yml) API le permite crear una experiencia de búsqueda que se centra únicamente en contenido y los temas que le interesan. Por ejemplo, después de especificar los dominios, sitios Web y las páginas Web específicas que se buscará en Bing, Bing Custom Search se adaptar los resultados en ese contenido específico. Puede incorporar las API Bing Custom Autosuggest, Image, y Video Search para personalizar aún más la experiencia de búsqueda.

## <a name="additional-bing-search-apis"></a>Otras versiones de Bing Search API

Las siguientes API de búsqueda de Bing le permiten mejorar su experiencia de búsqueda al combinarlas con otra API de búsqueda de Bing.

| API | DESCRIPCIÓN |
| -- | -- |
| [Bing Autosuggest](../Bing-Autosuggest/index.yml) | Mejorar la experiencia de búsqueda de la aplicación con Bing Autosuggest API devolviendo sugerencias de búsqueda en tiempo real.  |
| [Bing Statistics](bing-web-stats.md) | Bing Statistics proporciona análisis de las versiones de Bing Search API que usa la aplicación. Algunos de los análisis disponibles incluyen el volumen de llamadas, las cadenas de consulta principales y la distribución geográfica. |

## <a name="next-steps"></a>Pasos siguientes

* [Detalles de precios](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/) de Bing Search API
* En los [requisitos de uso y visualización de Bing](./use-display-requirements.md) se especifican usos aceptables del contenido y la información adquirida mediante las API de búsqueda de Bing.
