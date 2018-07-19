---
title: API REST del servicio Azure Search versión 2017-11-11-Preview | Microsoft Docs
description: La API REST del servicio Azure Search, versión 2017-11-11-Preview incluye características experimentales como búsquedas de sinónimos y moreLikeThis.
services: search
author: HeidiSteen
manager: cgronlun
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: search
ms.date: 06/28/2018
ms.author: HeidiSteen
ms.openlocfilehash: b5cb60bf16a4c904c9a6060113eba8b4d3a671ef
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2018
ms.locfileid: "37112609"
---
# <a name="azure-search-service-rest-api-version-2017-11-11-preview"></a>API REST del servicio Azure Search: versión 2017-11-11-Preview
En este artículo se describe la versión `api-version=2017-11-11-Preview` de la API de REST del servicio Azure Search y se proporcionan características experimentales que normalmente no están disponibles.

> [!NOTE]
> Las características en versión preliminar están disponibles para prueba y experimentación con el fin de recabar comentarios y están sujetas a cambios. Le recomendamos encarecidamente que no use una versión preliminar de API en aplicaciones de producción.


## <a name="new-in-2017-11-11-preview"></a>Novedades de 2017-11-11-Preview

[**Autocompletar**](search-autocomplete-tutorial.md) une la [API de sugerencias](https://docs.microsoft.com/rest/api/searchservice/suggestions) existente para agregar experiencias de escritura anticipada complementaria a la barra de búsqueda. Autocompletar devuelve términos de consulta de candidatos que un usuario puede elegir como cadena de consulta para búsquedas posteriores. Sugerencias devuelve documentos reales en respuesta a entradas parciales: los resultados de búsqueda son inmediatos y cambian de manera dinámica a medida que crece la longitud y la especificidad de la entrada del término de búsqueda.

[**Cognitive Search**](cognitive-search-concept-intro.md) es una nueva funcionalidad de enriquecimiento de Azure Search que busca información latente en orígenes no textuales y texto sin diferenciar, y la transforma en contenido que permite realizar búsquedas de texto completo en Azure Search. Los recursos siguientes se han introducido o modificado en la versión preliminar de la API de REST. El resto de las API de REST son las mismas sin importar si se llama a la versión generalmente disponible o a la versión preliminar.

+ [Skillset operations(api-version=2017-11-11-Preview)](https://docs.microsoft.com/rest/api/searchservice/skillset-operations) [Operaciones con conjunto de aptitudes (api-version=2017-11-11-Preview)]

+ [Create Indexer (api-version=2017-11-11-Preview)](https://docs.microsoft.com/rest/api/searchservice/create-indexer) (Creación de indexador [api-version=2017-11-11-Preview])

+ [Aptitudes predefinidas](cognitive-search-predefined-skills.md)

El resto de las API de REST son iguales, independientemente de la configuración que haya establecido para la versión de API. Por ejemplo, `GET https://[service name].search.windows.net/indexes/hotels?api-version=2017-11-11-Preview` y `GET https://[service name].search.windows.net/indexes/hotels?api-version=2017-11-11` (sin `Preview`) son funcionalmente equivalentes.

## <a name="other-preview-features"></a>Otras características de la versión preliminar

Las características anunciadas en versiones preliminares anteriores aún están en versión preliminar pública. Si está llamando a una API con una versión anterior de la API en versión preliminar, puede continuar usando esa versión o cambiar a `2017-11-11-Preview` sin que se produzcan cambios en el comportamiento esperado.

+ Los [archivos CSV en la indexación de Azure Blob](search-howto-index-csv-blobs.md), incorporados en `api-version=2015-02-28-Preview`, siguen siendo una característica en versión preliminar. Esta característica forma parte de la indexación de Azure Blob y se invoca mediante una configuración de valores. Cada línea de un archivo a CSV se indexa como documento independiente.

+ Las [matrices de JSON en la indexación de Azure Blob](search-howto-index-json-blobs.md), incorporadas en `api-version=2015-02-28-Preview`, siguen siendo una característica en versión preliminar. Esta característica forma parte de la indexación de Azure Blob y se invoca mediante una configuración de valores, donde cada elemento de la matriz se indexa como un documento independiente.

+ El [parámetro de consulta moreLikeThis](search-more-like-this.md) busca documentos que sean pertinentes para un documento específico. Esta característica ha aparecido en versiones anteriores. 


## <a name="how-to-call-a-preview-api"></a>Cómo llamar a una API en versión preliminar

Las vistas previas anteriores siguen funcionando, pero quedarán obsoletas con el paso del tiempo. Si el código llama a `api-version=2016-09-01-Preview` o `api-version=2015-02-28-Preview`, dichas llamadas siguen siendo válidas. Sin embargo, solo la versión preliminar más reciente se actualiza con mejoras. 

En la sintaxis de ejemplo siguiente se ilustra una llamada a la versión de API de versión preliminar.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?search=*&api-version=2017-11-11-Preview

El servicio Azure Search está disponible en varias versiones. Para obtener más información, consulte [Versiones de API](search-api-versions.md).
