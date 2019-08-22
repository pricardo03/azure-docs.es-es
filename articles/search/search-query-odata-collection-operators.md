---
title: 'Referencia de operadores de colección de OData: Azure Search'
description: Operadores de colección de OData, any y all, y expresiones lambda en consultas de Azure Search.
ms.date: 06/13/2019
services: search
ms.service: search
ms.topic: conceptual
author: brjohnstmsft
ms.author: brjohnst
manager: nitinme
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
ms.openlocfilehash: e057d0b57162d10aab13d8b1f77e0eaddca2ec2a
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2019
ms.locfileid: "69647632"
---
# <a name="odata-collection-operators-in-azure-search---any-and-all"></a>Operadores de colección de OData en Azure Search: `any` y `all`

Al escribir una [expresión de filtro de OData](query-odata-filter-orderby-syntax.md) para usarla con Azure Search, a menudo es útil filtrar por campos de colección. Esto se puede conseguir con los operadores `any` y `all`.

## <a name="syntax"></a>Sintaxis

En la siguiente EBNF ([forma de Backus-Naur extendida](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) se define la gramática de una expresión de OData en la que se usa `any` o `all`.

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
collection_filter_expression ::=
    field_path'/all(' lambda_expression ')'
    | field_path'/any(' lambda_expression ')'
    | field_path'/any()'

lambda_expression ::= identifier ':' boolean_expression
```

También está disponible un diagrama de sintaxis interactivo:

> [!div class="nextstepaction"]
> [Diagrama de sintaxis de OData para Azure Search](https://azuresearch.github.io/odata-syntax-diagram/#collection_filter_expression)

> [!NOTE]
> Vea [Referencia de sintaxis de expresiones OData para Azure Search](search-query-odata-syntax-reference.md) para obtener la EBNF completa.

Hay tres formas de expresión que filtran las colecciones.

- Las dos primeras recorren en iteración un campo de colección y aplican un predicado con forma de expresión lambda a cada elemento de la colección.
  - Una expresión que use `all` devuelve `true` si el predicado es true para todos los elementos de la colección.
  - Una expresión que use `any` devuelve `true` si el predicado es true al menos para un elemento de la colección.
- En la tercera forma de filtro de colección se usa `any` sin una expresión lambda para comprobar si un campo de la colección está vacío. Si la colección tiene algún elemento, devuelve `true`. Si la colección está vacía, devuelve `false`.

Una **expresión lambda** en un filtro de colección es como el cuerpo de un bucle en un lenguaje de programación. Define una variable, denominada **variable de rango**, que contiene el elemento actual de la colección durante la iteración. También define otra expresión booleana que son los criterios de filtro que se van a aplicar a la variable de rango para cada elemento de la colección.

## <a name="examples"></a>Ejemplos

Comparar documentos cuyo campo `tags` contenga exactamente la cadena "wifi":

    tags/any(t: t eq 'wifi')

Comparar documentos donde todos los elementos del campo `ratings` estén comprendidos entre 3 y 5, inclusive:

    ratings/all(r: r ge 3 and r le 5)

Comparar documentos donde cualquiera de las geocoordenadas del campo `locations` esté dentro del polígono indicado:

    locations/any(loc: geo.intersects(loc, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'))

Comparar documentos donde el campo `rooms` está vacío:

    not rooms/any()

Comparar documentos donde, para todas las habitaciones, el campo `rooms/amenities` contiene "tv" y `rooms/baseRate` es inferior a 100:

    rooms/all(room: room/amenities/any(a: a eq 'tv') and room/baseRate lt 100.0)

## <a name="limitations"></a>Limitaciones

No todas las características de las expresiones de filtro están disponibles dentro del cuerpo de una expresión lambda. Las limitaciones varían según el tipo de datos del campo de colección que se quiere filtrar. En la tabla siguiente se resumen las limitaciones.

[!INCLUDE [Limitations on OData lambda expressions in Azure Search](../../includes/search-query-odata-lambda-limitations.md)]

Para obtener más detalles sobre estas limitaciones, así como ejemplos, vea [Solución de problemas de los filtros de colección en Azure Search](search-query-troubleshoot-collection-filters.md). Para obtener información más detallada sobre por qué existen estas limitaciones, vea [Descripción de los filtros de colección en Azure Search](search-query-understand-collection-filters.md).

## <a name="next-steps"></a>Pasos siguientes  

- [Filtros de Azure Search](search-filters.md)
- [Información general sobre el lenguaje de expresiones OData para Azure Search](query-odata-filter-orderby-syntax.md)
- [Referencia de sintaxis de expresiones OData para Azure Search](search-query-odata-syntax-reference.md)
- [Search Documents &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) (Búsqueda en documentos [API REST de Azure Search Service])
