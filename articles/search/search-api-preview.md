---
title: Características en versión preliminar de la API REST
titleSuffix: Azure Cognitive Search
description: La API REST del servicio Azure Cognitive Search, versión 2019-05-06-Preview, incluye características experimentales, como almacén de conocimiento y almacenamiento en caché de indexadores para enriquecimiento incremental.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/03/2020
ms.openlocfilehash: fd21a4b821e1911e94d542a0922e5269786c365d
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2020
ms.locfileid: "76991072"
---
# <a name="preview-features-in-azure-cognitive-search"></a>Características en versión preliminar de Azure Cognitive Search

En este artículo se enumeran las características que se encuentran actualmente en versión preliminar. Se han eliminado de la lista las que pasan de versión preliminar a disponibilidad general. Puede marcar [Actualizaciones del servicio](https://azure.microsoft.com/updates/?product=search) o [Novedades](whats-new.md) para ver los anuncios relativos a la disponibilidad general.

Aunque algunas características en versión preliminar pueden estar disponibles en el portal y en el SDK de .NET, la API REST siempre tiene características en versión preliminar. 

+ En el caso de las operaciones de búsqueda, la versión preliminar actual de la API es [`2019-05-06-Preview`](https://docs.microsoft.com/rest/api/searchservice/index-2019-05-06-preview).
+ En el caso de las operaciones de administración, la versión preliminar actual es [`2019-10-01-Preview`](https://docs.microsoft.com/rest/api/searchmanagement/index-2019-10-01-preview).

> [!IMPORTANT]
> La funcionalidad de versión preliminar se ofrece sin un Acuerdo de Nivel de Servicio y no es aconsejable usarla para cargas de trabajo de producción. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="ai-enrichment-features"></a>Características de enriquecimiento con IA

Explore las últimas mejoras del enriquecimiento con IA mediante la [API de búsqueda en versión preliminar](https://docs.microsoft.com/rest/api/searchservice/index-2019-05-06-preview).

+ [La aptitud de búsqueda de entidades personalizadas (versión preliminar)](cognitive-search-skill-custom-entity-lookup.md ) es una aptitud cognitiva que busca texto de una lista de palabras y frases personalizada definida por el usuario. Con esta lista, se etiquetan todos los documentos con entidades coincidentes. La aptitud también admite un grado de coincidencia aproximada que se puede aplicar para buscar coincidencias similares pero no exactas. 

+ La [aptitud de detección de PII (versión preliminar)](cognitive-search-skill-pii-detection.md) es una aptitud cognitiva usada durante la indexación que extrae información de identificación personal de un texto de entrada y le ofrece la opción de enmascararla a partir de ese texto de varias maneras.

+ [Enriquecimiento incremental (versión preliminar)](cognitive-search-incremental-indexing-conceptual.md) agrega almacenamiento en caché a una canalización de enriquecimiento, lo que le permite reutilizar los resultados existentes si una modificación de destino, como una actualización de conjunto de aptitudes u otro objeto, no cambia el contenido. El almacenamiento en caché solo se aplica a los documentos enriquecidos generados por un conjunto de aptitudes.

+ El [almacén de conocimiento (versión preliminar)](knowledge-store-concept-intro.md) es un nuevo destino de una canalización de enriquecimiento basada en inteligencia artificial. La estructura de datos física existe en Azure Blob Storage y Azure Table Storage, y se crea y rellena al ejecutar un indexador que tiene un conjunto de aptitudes cognitivas conectado. La definición de un almacén de conocimiento se especifica en la definición de un conjunto de aptitudes. En la definición del almacén de conocimiento, controla las estructuras físicas de los datos a través de los elementos de *proyección* que determinan cómo se forman los datos, si estos se almacenan en Table Storage o Blob Storage, y si hay varias vistas.

## <a name="indexing-and-query-features"></a>Características de indexación y consulta

Las características en versión preliminar del indexador están disponibles en la API de búsqueda en versión preliminar. 

+ El [indizador de Cosmos DB](search-howto-index-cosmosdb.md) admite las versiones preliminares de MongoDB API, Gremlin API y Cassandra API.

+ El [indizador de Azure Data Lake Storage Gen2 (versión preliminar)](search-howto-index-azure-data-lake-storage.md) puede indexar el contenido y los metadatos de Data Lake Storage Gen2.

+ El [parámetro de consulta moreLikeThis (versión preliminar)](search-more-like-this.md) busca documentos que sean pertinentes para un documento específico. Esta característica ha aparecido en versiones anteriores. 

## <a name="management-features"></a>Características de administración

+ [Compatibilidad con puntos de conexión privados](service-create-private-endpoint.md) a través de [`api-version=2019-10-01-Preview`](https://docs.microsoft.com/rest/api/searchmanagement/index-2019-10-01-preview) de la API REST de administración. Puede crear un servicio que tenga restricciones sobre cómo se accede a su punto de conexión.

## <a name="earlier-preview-features"></a>Características de las versiones preliminares anteriores

Las características anunciadas en versiones preliminares anteriores siguen estando en versión preliminar pública si no han pasado a disponibilidad general. Si está llamando a una API con una versión anterior de la API en versión preliminar, puede continuar usando esa versión o cambiar a `2019-05-06-Preview` sin que se produzcan cambios en el comportamiento esperado.

## <a name="how-to-call-a-preview-api"></a>Cómo llamar a una API en versión preliminar

Las vistas previas anteriores siguen funcionando, pero quedarán obsoletas con el paso del tiempo. Si el código llama a `api-version=2016-09-01-Preview` o `api-version=2017-11-11-Preview`, dichas llamadas siguen siendo válidas. Sin embargo, solo la versión preliminar más reciente se actualiza con mejoras. 

En la sintaxis de ejemplo siguiente se ilustra una llamada a la versión de API de versión preliminar.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?search=*&api-version=2019-05-06-Preview

El servicio Azure Cognitive Search está disponible en varias versiones. Para obtener más información, consulte [Versiones de API](search-api-versions.md).

## <a name="next-steps"></a>Pasos siguientes

Revise la documentación de referencia de la API de REST del servicio Search. Si tiene algún problema, puede solicitar ayuda en [StackOverflow](https://stackoverflow.com/) o [ponerse en contacto con el servicio de soporte técnico](https://azure.microsoft.com/support/community/?product=search).

> [!div class="nextstepaction"]
> [Referencia de la API de REST del servicio Search](https://docs.microsoft.com/rest/api/searchservice/)