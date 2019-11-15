---
title: Característica de la consulta moreLikeThis (versión preliminar)
titleSuffix: Azure Cognitive Search
description: Se describe la característica moreLikeThis (versión preliminar), que está disponible en las versiones preliminares de la API REST de Azure Cognitive Search.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: fdde89f9ff88b15c464af805b81708b268e5ddf5
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2019
ms.locfileid: "73721739"
---
# <a name="morelikethis-preview-in-azure-cognitive-search"></a>moreLikeThis (versión preliminar) en Azure Cognitive Search

> [!IMPORTANT] 
> Esta característica actualmente está en su versión preliminar pública. La funcionalidad de versión preliminar se ofrece sin un Acuerdo de Nivel de Servicio y no es aconsejable usarla para cargas de trabajo de producción. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). En la [API REST, versión 2019-05-06-Preview](search-api-preview.md) se proporciona esta característica. Actualmente no hay compatibilidad con el portal ni con el SDK de .NET.

`moreLikeThis=[key]` es un parámetro de consulta de [Search Documents API](https://docs.microsoft.com/rest/api/searchservice/search-documents) que busca documentos similares al documento especificado por la clave del documento. Cuando se realiza una solicitud de búsqueda con `moreLikeThis`, se genera una consulta con los términos de búsqueda extraídos del documento especificado que describen mejor ese documento. La consulta generada se usa luego para realizar la solicitud de búsqueda. De forma predeterminada, se tiene en cuenta el contenido de todos los campos que permiten búsquedas, menos cualquier campo restringido especificado mediante el parámetro `searchFields`. El parámetro `moreLikeThis` no se puede usar con el parámetro de búsqueda, `search=[string]`.

De forma predeterminada, se tiene en cuenta el contenido de todos los campos que permiten búsquedas de nivel superior. Si quiere especificar campos concretos en su lugar, puede usar el parámetro `searchFields`. 

No se puede usar moreLikeThis en campos secundarios que permiten búsquedas de un [tipo complejo](search-howto-complex-data-types.md).

## <a name="examples"></a>Ejemplos 

A continuación se muestra un ejemplo de una consulta moreLikeThis. La consulta busca los documentos cuyos campos de descripción son lo más parecido al campo del documento de origen según se especifica con el parámetro `moreLikeThis`.

```
Get /indexes/hotels/docs?moreLikeThis=1002&searchFields=description&api-version=2019-05-06-Preview
```

```
POST /indexes/hotels/docs/search?api-version=2019-05-06-Preview
    {
      "moreLikeThis": "1002",
      "searchFields": "description"
    }
```


## <a name="next-steps"></a>Pasos siguientes

Puede usar cualquier herramienta de pruebas web para experimentar con esta característica.  Se recomienda usar Postman para este ejercicio.

> [!div class="nextstepaction"]
> [Exploración de la API REST de Azure Cognitive Search mediante Postman](search-get-started-postman.md)