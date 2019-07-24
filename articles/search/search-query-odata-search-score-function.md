---
title: Referencia de la función search.score de OData en Azure Search
description: Función search.score de OData en consultas de Azure Search.
ms.date: 06/13/2019
services: search
ms.service: search
ms.topic: conceptual
author: brjohnstmsft
ms.author: brjohnst
ms.manager: cgronlun
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: dc444216c4677b9970b867e92aa5ae259a197220
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67081849"
---
# <a name="odata-searchscore-function-in-azure-search"></a>Función `search.score` de OData en Azure Search

Cuando se envía una consulta a Azure Search sin el [parámetro **$orderby**](search-query-odata-orderby.md), los resultados que se devuelven se ordenarán en orden descendente por puntuación de relevancia. Incluso cuando se usa **$orderby**, de forma predeterminada se usará la puntuación de relevancia para resolver los empates. Pero en ocasiones resulta útil usar la puntuación de relevancia como un criterio de ordenación inicial y cualquier otro criterio como el factor de desempate. La función `search.score` permite hacer esto.

## <a name="syntax"></a>Sintaxis

La sintaxis de `search.score` en **$orderby** es `search.score()`. La función `search.score` no toma ningún parámetro. Se puede usar con el especificador de criterio de ordenación `asc` o `desc`, al igual que cualquier otra cláusula del parámetro **$orderby**. Puede aparecer en cualquier lugar de la lista de criterios de ordenación.

## <a name="example"></a>Ejemplo

Clasificar hoteles en orden descendente por `search.score` y `rating`, y después en orden ascendente por distancia de las coordenadas dadas de manera que, entre dos hoteles con valoraciones idénticas, el más cercano aparezca primero en la lista:

    search.score() desc,rating desc,geo.distance(location, geography'POINT(-122.131577 47.678581)') asc

## <a name="next-steps"></a>Pasos siguientes  

- [Información general sobre el lenguaje de expresiones OData para Azure Search](query-odata-filter-orderby-syntax.md)
- [Referencia de sintaxis de expresiones OData para Azure Search](search-query-odata-syntax-reference.md)
- [Search Documents &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) (Búsqueda en documentos [API REST de Azure Search Service])
