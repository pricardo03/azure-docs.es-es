---
title: 'Referencia de funciones geoespaciales de OData: Azure Search'
description: Funciones geoespaciales de OData, geo.distance y geo.intersects, en consultas de Azure Search.
ms.date: 09/13/2019
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
ms.openlocfilehash: 03220786c65ab510a632252b20d593cd96a90494
ms.sourcegitcommit: e97a0b4ffcb529691942fc75e7de919bc02b06ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/15/2019
ms.locfileid: "71003457"
---
# <a name="odata-geo-spatial-functions-in-azure-search---geodistance-and-geointersects"></a>Funciones geoespaciales de OData en Azure Search: `geo.distance` y `geo.intersects`

Azure Search admite consultas geoespaciales en las [expresiones de filtro de OData](query-odata-filter-orderby-syntax.md) a través de las funciones `geo.distance` y `geo.intersects`. La función `geo.distance` devuelve la distancia en kilómetros entre dos puntos, siendo uno de ellos un campo o una variable de rango, y el otro una constante que se pasa como parte del filtro. La función `geo.intersects` devuelve `true` si un punto determinado se encuentra dentro de un polígono determinado, donde el punto es un campo o una variable de rango, y el polígono se especifica como una constante que se pasa como parte del filtro.

La función `geo.distance` también se puede usar en el [parámetro **$orderby**](search-query-odata-orderby.md) para ordenar los resultados de la búsqueda por distancia con respecto a un punto dado. La sintaxis de `geo.distance` en **$orderby** es la misma que la de **$filter**. Cuando se usa `geo.distance` en **$orderby**, el campo al que se aplica debe ser de tipo `Edm.GeographyPoint` y también debe ser **ordenable**.

> [!NOTE]
> Al usar `geo.distance` en el parámetro **$orderby**, el campo que se pasa a la función debe contener solo un punto geográfico. En otras palabras, debe ser de tipo `Edm.GeographyPoint` y no `Collection(Edm.GeographyPoint)`. No es posible ordenar por campos de colección en Azure Search.

## <a name="syntax"></a>Sintaxis

La EBNF siguiente ([notación de Backus-Naur extendida](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) define la gramática de las funciones `geo.distance` y `geo.intersects`, así como los valores geoespaciales sobre los que operan:

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
geo_distance_call ::=
    'geo.distance(' variable ',' geo_point ')'
    | 'geo.distance(' geo_point ',' variable ')'

geo_point ::= "geography'POINT(" lon_lat ")'"

lon_lat ::= float_literal ' ' float_literal

geo_intersects_call ::=
    'geo.intersects(' variable ',' geo_polygon ')'

/* You need at least four points to form a polygon, where the first and
last points are the same. */
geo_polygon ::=
    "geography'POLYGON((" lon_lat ',' lon_lat ',' lon_lat ',' lon_lat_list "))'"

lon_lat_list ::= lon_lat(',' lon_lat)*
```

También está disponible un diagrama de sintaxis interactivo:

> [!div class="nextstepaction"]
> [Diagrama de sintaxis de OData para Azure Search](https://azuresearch.github.io/odata-syntax-diagram/#geo_distance_call)

> [!NOTE]
> Vea [Referencia de sintaxis de expresiones OData para Azure Search](search-query-odata-syntax-reference.md) para obtener la EBNF completa.

### <a name="geodistance"></a>geo.distance

La función `geo.distance` toma dos parámetros de tipo `Edm.GeographyPoint` y devuelve un valor `Edm.Double` que es la distancia entre ellos en kilómetros. Esto difiere de otros servicios que admiten operaciones geoespaciales de OData, que normalmente devuelven las distancias en metros.

Uno de los parámetros de `geo.distance` debe ser una constante de punto de geografía, y el otro una ruta de acceso de campo (o una variable de rango en el caso de un filtro que recorra en iteración un campo de tipo `Collection(Edm.GeographyPoint)`). El orden de estos parámetros no importa.

La constante de punto de geografía tiene el formato `geography'POINT(<longitude> <latitude>)'`, donde la longitud y la latitud son constantes numéricas.

> [!NOTE]
> Al usar `geo.distance` en un filtro, debe comparar la distancia devuelta por la función con una constante mediante `lt`, `le`, `gt` o `ge`. Los operadores `eq` y `ne` no se admiten cuando se comparan las distancias. Por ejemplo, este es un uso correcto de `geo.distance`: `$filter=geo.distance(location, geography'POINT(-122.131577 47.678581)') le 5`.

### <a name="geointersects"></a>geo.intersects

La función `geo.intersects` toma una variable de tipo `Edm.GeographyPoint` y una constante `Edm.GeographyPolygon`, y devuelve un valor `Edm.Boolean` -- `true` si el punto está dentro de los límites del polígono, o bien `false` en caso contrario.

El polígono es una superficie bidimensional almacenada como una secuencia de puntos que definen un anillo delimitador (vea los [ejemplos](#examples) siguientes). El polígono debe estar cerrado, lo que significa que el primer y último conjunto de puntos deben ser los mismos. [Los puntos de un polígono deben estar ordenados en sentido contrario a las agujas del reloj](https://docs.microsoft.com/rest/api/searchservice/supported-data-types#Anchor_1).

### <a name="geo-spatial-queries-and-polygons-spanning-the-180th-meridian"></a>Consultas geoespaciales y polígonos que abarcan el meridiano 180

Para muchas bibliotecas de consultas geoespaciales, formular una consulta que incluya el meridiano 180 (cerca del cambio de fecha) está fuera de los límites o requiere una solución alternativa, como dividir el polígono en dos partes, una a cada lado del meridiano.

En Azure Search, las consultas geoespaciales que incluyen la longitud de 180 grados funcionarán según lo esperado si la forma de la consulta es rectangular y las coordenadas se alinean en un diseño de cuadrícula a lo largo de la longitud y la latitud (por ejemplo, `geo.intersects(location, geography'POLYGON((179 65, 179 66, -179 66, -179 65, 179 65))'`). En caso contrario, para formas no rectangulares o no alineadas, considere el enfoque del polígono dividido.  

### <a name="geo-spatial-functions-and-null"></a>Funciones geoespaciales y `null`

Como sucede con todos los demás campos que no son de colección en Azure Search, los campos de tipo `Edm.GeographyPoint` pueden contener valores `null`. Cuando Azure Search evalúa `geo.intersects` para un campo que es `null`, el resultado siempre será `false`. En este caso, el comportamiento de `geo.distance` depende del contexto:

- En los filtros, `geo.distance` de un campo `null` da como resultado `null`. Esto significa que el documento no coincidirá porque `null` comparado con cualquier valor distinto de NULL se evalúa como `false`.
- Al ordenar los resultados mediante **$orderby**, `geo.distance` de un campo `null` da como resultado la máxima distancia posible. Los documentos con este tipo de campo se ordenarán por debajo del resto cuando se usa la dirección de ordenación `asc` (el valor predeterminado), y por encima del resto cuando la dirección sea `desc`.

## <a name="examples"></a>Ejemplos

### <a name="filter-examples"></a>Ejemplos de filtros

Buscar todos los hoteles a una distancia no superior a diez kilómetros de un punto de referencia determinado (donde la ubicación es un campo de tipo `Edm.GeographyPoint`):

    geo.distance(location, geography'POINT(-122.131577 47.678581)') le 10

Buscar todos los hoteles dentro de una ventanilla dada descrita como un polígono (donde la ubicación es un campo de tipo `Edm.GeographyPoint`). Tenga en cuenta que el polígono está cerrado (el primer y el último conjunto de puntos deben ser los mismos) y [los puntos deben aparecer ordenados en sentido contrario a las agujas del reloj](https://docs.microsoft.com/rest/api/searchservice/supported-data-types#Anchor_1).

    geo.intersects(location, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))')

### <a name="order-by-examples"></a>Ejemplos de OrderBy

Clasificar hoteles en orden descendente por `rating` y después en orden ascendente por distancia de las coordenadas dadas:

    rating desc,geo.distance(location, geography'POINT(-122.131577 47.678581)') asc

Clasificar hoteles en orden descendente por `search.score` y `rating`, y después en orden ascendente por distancia de las coordenadas dadas de manera que, entre dos hoteles con valoraciones idénticas, el más cercano aparezca primero en la lista:

    search.score() desc,rating desc,geo.distance(location, geography'POINT(-122.131577 47.678581)') asc

## <a name="next-steps"></a>Pasos siguientes  

- [Filtros de Azure Search](search-filters.md)
- [Información general sobre el lenguaje de expresiones OData para Azure Search](query-odata-filter-orderby-syntax.md)
- [Referencia de sintaxis de expresiones OData para Azure Search](search-query-odata-syntax-reference.md)
- [Search Documents &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) (Búsqueda en documentos [API REST de Azure Search Service])
