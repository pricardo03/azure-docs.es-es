---
title: Versión preliminar de la API de REST para Azure Search 2019-05-06, versión preliminar – Azure Search
description: La API de REST del servicio Azure Search, 2019-05-06, versión preliminar, incluye características experimentales como almacén de conocimiento y claves de cifrado administradas por el cliente.
services: search
author: HeidiSteen
manager: cgronlun
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: search
ms.date: 05/02/2019
ms.author: HeidiSteen
ms.custom: seodec2018
ms.openlocfilehash: 5374ff896613dd8f8563a2054be8a92103e63fbb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65523914"
---
# <a name="azure-search-service-rest-api-version-2019-05-06-preview"></a>API de REST del servicio Azure Search, 2019-05-06, versión preliminar
En este artículo se describe la versión `api-version=2019-05-06-Preview` de la API de REST del servicio Azure Search y se proporcionan características experimentales que normalmente no están disponibles.

> [!NOTE]
> Las características en versión preliminar están disponibles para prueba y experimentación con el fin de recabar comentarios y están sujetas a cambios. Le recomendamos encarecidamente que no use una versión preliminar de API en aplicaciones de producción.


## <a name="new-in-2019-05-06-preview"></a>Novedades de 2019-05-06, versión preliminar

El [**almacén de conocimiento**](knowledge-store-concept-intro.md) es un nuevo destino de una canalización de enriquecimiento basada en inteligencia artificial. Además de un índice, ahora puede conservar las estructuras de datos rellenadas que se crean durante la indexación en Azure Storage. Igualmente, controla las estructuras físicas de los datos a través de los elementos de un conjunto de aptitudes, incluido cómo se forman los datos, si estos se almacenan en Table Storage o Blob Storage, y si hay varias vistas.

Las [**claves de cifrado administradas por el cliente**](search-security-manage-encryption-keys.md) para el cifrado en reposo del lado del servicio son también una nueva característica en vista previa. Además del cifrado en reposo integrado administrado por Microsoft, puede aplicar una capa adicional de cifrado, donde será el titular exclusivo de las claves de cifrado.

## <a name="other-preview-features"></a>Otras características de la versión preliminar

Las características anunciadas en versiones preliminares anteriores aún están en versión preliminar pública. Si está llamando a una API con una versión anterior de la API en versión preliminar, puede continuar usando esa versión o cambiar a `2019-05-06-Preview` sin que se produzcan cambios en el comportamiento esperado.

+ El [parámetro de consulta moreLikeThis](search-more-like-this.md) busca documentos que sean pertinentes para un documento específico. Esta característica ha aparecido en versiones anteriores. 
* La [indexación de blobs en CSV](search-howto-index-csv-blobs.md) crea un documento por línea, en lugar de un documento por blob de texto.
* La [compatibilidad de la API MongoDB con los indexadores de Cosmos DB](search-howto-index-cosmosdb.md) está en versión preliminar.


## <a name="how-to-call-a-preview-api"></a>Cómo llamar a una API en versión preliminar

Las vistas previas anteriores siguen funcionando, pero quedarán obsoletas con el paso del tiempo. Si el código llama a `api-version=2016-09-01-Preview` o `api-version=2017-11-11-Preview`, dichas llamadas siguen siendo válidas. Sin embargo, solo la versión preliminar más reciente se actualiza con mejoras. 

En la sintaxis de ejemplo siguiente se ilustra una llamada a la versión de API de versión preliminar.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?search=*&api-version=2019-05-06-Preview

El servicio Azure Search está disponible en varias versiones. Para obtener más información, consulte [Versiones de API](search-api-versions.md).

## <a name="next-steps"></a>Pasos siguientes

Revise la documentación de referencia de la API de REST del servicio Azure Search. Si tiene algún problema, puede solicitar ayuda en [StackOverflow](https://stackoverflow.com/) o [ponerse en contacto con el servicio de soporte técnico](https://azure.microsoft.com/support/community/?product=search).

> [!div class="nextstepaction"]
> [Referencia de la API de REST del servicio Search](https://docs.microsoft.com/rest/api/searchservice/)