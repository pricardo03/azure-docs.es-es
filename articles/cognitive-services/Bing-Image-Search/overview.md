---
title: ¿Qué es Bing Image Search API?
titleSuffix: Azure Cognitive Services
description: Bing Image Search API le permite usar funcionalidades cognitivas de búsqueda de imágenes de Bing en la aplicación. Envíe consultas de búsqueda de usuarios con la API para obtener y mostrar imágenes relacionadas y de alta calidad similares a Bing Images.
services: cognitive-services
author: aahill
manager: cgronlun
ms.assetid: 1446AD8B-A685-4F5F-B4AA-74C8E9A40BE9
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: overview
ms.date: 10/11/2017
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 4ae9c0c1fb6cd341e812f9e8b35eaff789637561
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/06/2019
ms.locfileid: "55765653"
---
# <a name="what-is-the-bing-image-search-api"></a>¿Qué es Bing Image Search API?

Bing Image Search API le permite usar funcionalidades cognitivas de búsqueda de imágenes de Bing en la aplicación. Envíe consultas de búsqueda de usuarios con la API para obtener y mostrar imágenes relacionadas y de alta calidad similares a [Bing Images](https://www.bing.com/images).

Tenga en cuenta que Bing Image Search API proporciona resultados de búsqueda basados solo en imágenes. Use [Bing Web Search API](../bing-web-search/search-the-web.md), [Video Search API](https://docs.microsoft.com/azure/cognitive-services/Bing-Video-Search) y [News Search API](https://review.docs.microsoft.com/azure/cognitive-services/bing-news-search) para otros tipos de contenido web.

## <a name="bing-image-search-features"></a>Características de Bing Image Search

Aunque Bing Image Search encuentra y devuelve principalmente imágenes relevantes en una consulta de búsqueda, el servicio también proporciona varias características adicionales para la recuperación inteligente y centrada de imágenes en la Web.


| Característica                                                                                                                                                                                 | DESCRIPCIÓN                                                                                                                                                            |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Sugerencia de términos de búsqueda en tiempo real](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-sending-queries) | Mejore su experiencia con las aplicaciones mediante [Bing Autosuggest API](../bing-autosuggest/get-suggested-search-terms.md) para mostrar términos de búsqueda sugeridos a medida que se escriben. |
| [Filtrado y restricción de resultados de imagen](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-get-images)                       | Filtre las imágenes que devuelve Bing mediante la edición de parámetros de consulta.                                                                                                       |
| [Recorte, cambio de tamaño y vista de miniaturas](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/resize-and-crop-thumbnails)                                                | Edite y muestre vistas previas en miniatura de las imágenes que devuelve Bing Image Search.                                                                                      |
| [Dinamización y expansión de las consultas de búsqueda de usuarios](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-sending-queries)               | Expanda las funcionalidades de búsqueda con la inclusión y visualización de términos de búsqueda sugeridos por Bing en las consultas.                                                                    |
| [Obtención de imágenes populares](https://review.docs.microsoft.com/azure/cognitive-services/bing-image-search/trending-images)                                                                     | Personalice una búsqueda de imágenes de tendencias de todo el mundo.                                                                                                          |

## <a name="workflow"></a>Flujo de trabajo

Bing Image Search API es un servicio web RESTful, lo que significa que es fácil llamar desde cualquier lenguaje de programación que pueda realizar solicitudes HTTP y analizar código JSON. Puede usar el servicio con la [API REST](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/quickstarts/csharp?) o el [SDK](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/image-search-sdk-quickstart).

1. Cree una [cuenta de API de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con acceso a Bing Search APIs. Si no tiene una suscripción de Azure, puede [crear una cuenta](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) gratuita.
2. Envíe una solicitud a la API con una [consulta de búsqueda](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-sending-queries) válida.
3. Analice el mensaje JSON devuelto para procesar la respuesta de API.

## <a name="next-steps"></a>Pasos siguientes

Primero, pruebe la [demostración interactiva](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/) de Bing Image Search API.
Esta demostración muestra cómo puede personalizar rápidamente una consulta de búsqueda y explorar la Web en busca de imágenes.

Cuando esté listo para llamar a la API, cree una [cuenta de API de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account). Si no tiene una suscripción de Azure, puede [crear una cuenta](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) gratuita.

Para empezar rápidamente con la primera solicitud de API, puede conocer cómo:

* [Enviar consultas de búsqueda a Bing](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/quickstarts/csharp) mediante la API REST o
* [Solicitar y filtrar](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/image-search-sdk-quickstart) las imágenes que devuelve Bing mediante el SDK.

## <a name="see-also"></a>Otras referencias

* La sección de referencia de [Bing Image Search API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference) contiene definiciones e información sobre los puntos de conexión, los encabezados, las respuestas de API y los parámetros de consulta que puede usar para solicitar resultados de búsqueda basados en imágenes.

* En los [requisitos de uso y visualización de Bing](./useanddisplayrequirements.md) se especifican usos aceptables del contenido y la información adquirida mediante las API de búsqueda de Bing.

* En el tema [Obtención de imágenes desde la web con Bing Image Search API](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-get-images) se describe cómo buscar y obtener imágenes de la Web.

* En el tema [Envío y trabajo con consultas de búsqueda](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-sending-queries) se describe cómo realizar, personalizar y dinamizar consultas de búsqueda.
