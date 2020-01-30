---
title: API REST versión 2019-05-06-Preview
titleSuffix: Azure Cognitive Search
description: La API REST del servicio Azure Cognitive Search, versión 2019-05-06-Preview, incluye características experimentales como almacén de conocimiento y almacenamiento en caché de indizadores para enriquecimiento incremental.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/04/2020
ms.openlocfilehash: 940ada83aeabf4bf8746ad5f90592e0917f7b403
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76844452"
---
# <a name="azure-cognitive-search-service-rest-api-version-2019-05-06-preview"></a>API REST del servicio Azure Cognitive Search, versión 2019-05-06-Preview

En este artículo se describe la versión `api-version=2019-05-06-Preview` de la API REST del servicio Search y se proporcionan características experimentales que normalmente no están disponibles.

> [!NOTE]
> Las características en versión preliminar están disponibles para prueba y experimentación con el fin de recabar comentarios y están sujetas a cambios. Le recomendamos encarecidamente que no use una versión preliminar de API en aplicaciones de producción.


## <a name="new-in-2019-05-06-preview"></a>Novedades de 2019-05-06, versión preliminar

+ [Enriquecimiento incremental (versión preliminar)](cognitive-search-incremental-indexing-conceptual.md) agrega almacenamiento en caché a una canalización de enriquecimiento, lo que le permite reusar los resultados existentes si una modificación de destino, como una actualización de conjunto de aptitudes u otro objeto, no cambia el contenido. El almacenamiento en caché solo se aplica a los documentos enriquecidos generados por un conjunto de aptitudes.

+ El [indizador de Cosmos DB](search-howto-index-cosmosdb.md) admite las versiones preliminares de MongoDB API, Gremlin API y Cassandra API.

+ El [indizador de Azure Data Lake Storage Gen2 (versión preliminar)](search-howto-index-azure-data-lake-storage.md) puede indexar el contenido y los metadatos de Data Lake Storage Gen2.

+ El [almacén de conocimiento](knowledge-store-concept-intro.md)es un nuevo destino de una canalización de enriquecimiento basada en inteligencia artificial. La estructura de datos física existe en Azure Blob Storage y Azure Table Storage, y se crea y rellena al ejecutar un indexador que tiene un conjunto de aptitudes cognitivas conectado. La definición de un almacén de conocimiento se especifica en la definición de un conjunto de aptitudes. En la definición del almacén de conocimiento, controla las estructuras físicas de los datos a través de los elementos de *proyección* que determinan cómo se forman los datos, si estos se almacenan en Table Storage o Blob Storage, y si hay varias vistas.

## <a name="earlier-preview-features"></a>Características de las versiones preliminares anteriores

Las características anunciadas en versiones preliminares anteriores aún están en versión preliminar pública. Si está llamando a una API con una versión anterior de la API en versión preliminar, puede continuar usando esa versión o cambiar a `2019-05-06-Preview` sin que se produzcan cambios en el comportamiento esperado.

+ El [parámetro de consulta moreLikeThis](search-more-like-this.md) busca documentos que sean pertinentes para un documento específico. Esta característica ha aparecido en versiones anteriores. 

+ La [indexación de blobs en CSV](search-howto-index-csv-blobs.md) crea un documento por línea, en lugar de un documento por blob de texto.

## <a name="how-to-call-a-preview-api"></a>Cómo llamar a una API en versión preliminar

Las vistas previas anteriores siguen funcionando, pero quedarán obsoletas con el paso del tiempo. Si el código llama a `api-version=2016-09-01-Preview` o `api-version=2017-11-11-Preview`, dichas llamadas siguen siendo válidas. Sin embargo, solo la versión preliminar más reciente se actualiza con mejoras. 

En la sintaxis de ejemplo siguiente se ilustra una llamada a la versión de API de versión preliminar.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?search=*&api-version=2019-05-06-Preview

El servicio Azure Cognitive Search está disponible en varias versiones. Para obtener más información, consulte [Versiones de API](search-api-versions.md).

## <a name="next-steps"></a>Pasos siguientes

Revise la documentación de referencia de la API de REST del servicio Search. Si tiene algún problema, puede solicitar ayuda en [StackOverflow](https://stackoverflow.com/) o [ponerse en contacto con el servicio de soporte técnico](https://azure.microsoft.com/support/community/?product=search).

> [!div class="nextstepaction"]
> [Referencia de la API de REST del servicio Search](https://docs.microsoft.com/rest/api/searchservice/)