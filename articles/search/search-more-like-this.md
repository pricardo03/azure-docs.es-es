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
ms.openlocfilehash: d18069335bb20f78a5bcda85eb6fcb2a5abe75f7
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65024670"
---
# <a name="morelikethis-in-azure-search-preview"></a>moreLikeThis en Azure Search (versión preliminar)

`moreLikeThis=[key]` es un parámetro de consulta en el [API de búsqueda de documentos](https://docs.microsoft.com/rest/api/searchservice/search-documents) que busca documentos similares al documento especificado por la clave del documento. Cuando se realiza una solicitud de búsqueda con `moreLikeThis`, se genera una consulta con los términos de búsqueda extraídos del documento especificado que describen mejor ese documento. La consulta generada se usa luego para realizar la solicitud de búsqueda. De forma predeterminada, se considera el contenido de todos los campos de búsqueda, menos cualquier campos restringidos que ha especificado mediante el `searchFields` parámetro. El parámetro `moreLikeThis` no se puede usar con el parámetro de búsqueda, `search=[string]`.

De forma predeterminada, se considera el contenido de todos los campos de búsqueda nivel superior. Si desea especificar los campos concretos en su lugar, puede usar el `searchFields` parámetro. 

> [!NOTE]
> `moreLikeThis` vista previa no funciona en campos secundarios que se puede buscar en un [tipo complejo](search-howto-complex-data-types.md).

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

## <a name="feature-availability"></a>Disponibilidad de características

El `moreLikeThis` parámetro está disponible en vista previa de las API de REST únicamente (`api-version=2019-05-06-Preview`).

## <a name="next-steps"></a>Pasos siguientes

Puede utilizar cualquier herramienta de prueba de web para experimentar con esta característica.  Se recomienda usar Postman para este ejercicio.

> [!div class="nextstepaction"]
> [Explorar la API de REST de Azure Search con Postman](search-fiddler.md)