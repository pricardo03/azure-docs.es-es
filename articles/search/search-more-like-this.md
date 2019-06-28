---
title: 'moreLikeThis en Azure Search (versión preliminar): Azure Search'
description: Documentación preliminar de la característica moreLikeThis (versión preliminar), expuesta en la API de REST de Azure Search.
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: brjohnst
ms.custom: seodec2018
ms.openlocfilehash: 5723f1ab7258a9e0d672b5c0fd9fd0b9c4dc8721
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65522925"
---
# <a name="morelikethis-in-azure-search"></a>moreLikeThis en Azure Search

> [!Note]
> moreLikeThis se encuentra en versión preliminar y no está pensado para su uso en producción. En la [API REST, versión 2019-05-06-Preview](search-api-preview.md) se proporciona esta característica. Por el momento, no hay compatibilidad con .NET SDK.

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
> [Exploración de las API REST de Azure Search mediante Postman](search-fiddler.md)