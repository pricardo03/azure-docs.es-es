---
title: ¿Qué son las versiones de Bing Search API?
titleSuffix: Azure Cognitive Services
description: Use este artículo para conocer las versiones de Bing Search API y cómo habilitar las búsquedas cognitivas en Internet en sus aplicaciones y servicios.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: article
ms.date: 01/31/2019
ms.author: aahi
ms.openlocfilehash: b2277f540b076307fe74c57068ff288860f82796
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55513089"
---
# <a name="what-are-the-bing-search-apis"></a>¿Qué son las versiones de Bing Search API?

Las versiones de Bing Search API permiten crear aplicaciones y servicios conectados a la web que buscan páginas web, imágenes, noticias, ubicaciones y mucho más sin anuncios. Mediante el envío de solicitudes de búsqueda con las API REST o los SDK de Bing Search, puede obtener información y contenido apropiados para búsquedas web. Use este artículo para conocer las diversas API de búsqueda de Bing y cómo puede integrar las búsquedas cognitivas en sus aplicaciones y servicios. Los precios y límites de velocidad pueden variar según la API.

## <a name="the-bing-web-search-api"></a>Bing Web Search API

[Bing Web Search API](../Bing-Web-Search/index.yml) devuelve páginas web, imágenes, vídeo, noticias y mucho más. Las consultas de búsqueda que se envían a esta API se pueden filtrar para incluir o excluir determinados tipos de contenido.

Considere el uso de Bing Web Search API en aplicaciones que necesitan buscar todos los tipos de contenido web apropiado. Si la aplicación busca un tipo específico de contenido en línea, considere una de las siguientes API de búsqueda: 

## <a name="content-specific-bing-search-apis"></a>API de búsqueda de Bing específicas de contenido

Las siguientes API de búsqueda de Bing devuelven contenido específico de la web, como imágenes, noticias, negocios locales y vídeos.

| API de Bing | DESCRIPCIÓN |
| -- | -- | 
| [Entity Search](../Bing-Entities-Search/index.yml) | Bing Entity Search API devuelve resultados de búsqueda que contienen entidades, que pueden ser personas, lugares o cosas. En función de la consulta, la API devolverá una o varias entidades que satisfacen la consulta de búsqueda, que pueden incluir personajes destacados, negocios locales, puntos de referencia, destinos, etc. |
| [Image Search](../Bing-Image-Search/index.yml) | Bing Image Search API le permite buscar y encontrar imágenes estáticas y animadas de alta calidad, de forma parecida a [Bing.com/images](https://www.Bing.com/images). Puede refinar las búsquedas para incluir o excluir imágenes por atributo, como tamaño, color, licencia y novedad. También puede buscar imágenes populares, cargar imágenes para obtener conclusiones sobre ellas y mostrar vistas previas en miniatura. |
| [News Search](../Bing-News-Search/index.yml) | Bing News Search API le permite buscar noticias, de forma parecida a [Bing.com/news](https://www.Bing.com/news). La API devuelve artículos de noticias de varios orígenes o dominios concretos. Puede buscar entre las categorías para obtener artículos populares, artículos destacados y titulares.
| [Video Search](../Bing-Video-Search/index.yml) | Bing Video Search API le permite buscar vídeos en la web. Obtenga vídeos populares, contenido relacionado y vistas previas en miniatura. |
| [Visual Search](../Bing-visual-search/index.yml) | Cargue una imagen o use una dirección URL para obtener información detallada sobre ella, como productos visualmente parecidos, imágenes y búsquedas relacionadas. |
 [Local Business Search](../bing-local-business-search/index.yml) | Bing Local Business Search API permite a las aplicaciones buscar información acerca de la ubicación y forma de contacto de empresas locales en función de las consultas de búsquedas. |

## <a name="the-bing-custom-search-api"></a>Bing Custom Search API

La creación de una instancia de búsqueda personalizada con [Bing Custom Search API](../Bing-Custom-Search/index.yml) le permite crear una experiencia de búsqueda centrada únicamente en el contenido y los temas que le interesan. Por ejemplo, después de especificar los dominios, sitios web y páginas web específicas que se buscarán en Bing, los resultados se ajustan a ese contenido en particular. Puede incorporar las API Bing Custom Autosuggest, Image, y Video Search para personalizar aún más la experiencia de búsqueda.  

## <a name="additional-bing-search-apis"></a>Otras versiones de Bing Search API

Las siguientes versiones de Bing Search API le permiten mejorar la experiencia de búsqueda ya que se pueden combinar con otras API de búsqueda de Bing.

| API | DESCRIPCIÓN |
| -- | -- | 
| [Bing Autosuggest](../Bing-Autosuggest/index.yml) | Mejore la experiencia de búsqueda de la aplicación con Bing Autosuggest API gracias a que las búsquedas sugeridas se devuelven en tiempo real.  |
| [Bing Statistics](bing-web-stats.md) | Bing Statistics proporciona análisis de las versiones de Bing Search API que usa la aplicación. Algunos de los análisis disponibles incluyen el volumen de llamadas, las cadenas de consulta principales y la distribución geográfica. |

## <a name="next-steps"></a>Pasos siguientes

* [Detalles de precios](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/) de Bing Search API
* En los [requisitos de uso y visualización de Bing](./use-display-requirements.md) se especifican usos aceptables del contenido y la información adquirida mediante las API de búsqueda de Bing.
