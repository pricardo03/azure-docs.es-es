---
title: 'Sintaxis de expresiones de OData para filtros y cláusulas OrderBy: Azure Search'
description: Sintaxis de OData para filtros y expresiones OrderBy para consultas de Azure Search.
ms.date: 03/27/2019
services: search
ms.service: search
ms.topic: conceptual
author: Brjohnstmsft
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
ms.openlocfilehash: ab98c3be75fb59603be66ee84e0d288de56cdc91
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2019
ms.locfileid: "58648510"
---
# <a name="odata-expression-syntax-for-filters-and-order-by-clauses-in-azure-search"></a>Sintaxis de expresiones de OData para filtros y cláusulas OrderBy en Azure Search

Azure Search admite un subconjunto de la sintaxis de expresiones de OData para las expresiones **$filter** y **$orderby**. Las expresiones de filtro se evalúan durante el análisis de la consulta, restringiendo la búsqueda a campos específicos o agregando criterios de coincidencia que se usan durante los exámenes de índice. Las expresiones OrderBy se aplican como un paso posterior al procesamiento a través de un conjunto de resultados. Tanto los filtros como las expresiones OrderBy se incluyen en una solicitud de consulta, conforme a una sintaxis de OData independiente de la sintaxis de consulta [simple](query-simple-syntax.md) o [completa](query-lucene-syntax.md) utilizada en un parámetro **search**. En este artículo se proporciona la documentación de referencia para las expresiones de OData usadas en filtros y en expresiones de ordenación.

## <a name="filter-syntax"></a>Sintaxis de filtro

Una expresión **$filter** se puede ejecutar de forma independiente como una consulta expresada por completo o refinar una consulta que tenga parámetros adicionales. En los ejemplos siguientes se ilustran unos pocos escenarios clave. En el primer ejemplo, el filtro es el fundamento de la consulta.


```POST
POST /indexes/hotels/docs/search?api-version=2017-11-11
    {
      "filter": "(baseRate ge 60 and baseRate lt 300) or hotelName eq 'Fancy Stay'"
    }
```

Otro caso de uso común son las facetas combinadas de filtros, donde el filtro reduce el área de superficie de consulta en función de una selección de navegación de faceta controlada por el usuario:

```POST
POST /indexes/hotels/docs/search?api-version=2017-11-11
    {
      "search": "test",
      "facets": [ "tags", "baseRate,values:80|150|220" ],
      "filter": "rating eq 3 and category eq 'Motel'"
    }
```

### <a name="filter-operators"></a>Operadores de filtro  

- Operadores lógicos (and, or, not).  

- Expresiones de comparación (`eq, ne, gt, lt, ge, le`). La comparación de cadenas distingue mayúsculas de minúsculas.  

- Constantes de los tipos de [Entity Data Model](https://docs.microsoft.com/dotnet/framework/data/adonet/entity-data-model) (EDM) admitidos. Consulte [Supported data types &#40;Azure Search&#41;](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) (Tipos de datos admitidos [Azure Search]) para una lista de tipos admitidos. No se admiten las constantes de tipos de colección.  

- Referencias a nombres de campo. Solo los campos `filterable` se pueden usar en expresiones de filtro.  

- `any` sin parámetros. Esto prueba si un campo de tipo `Collection(Edm.String)` contiene todos los elementos.  

- `any` y `all` con compatibilidad con expresiones lambda limitada. 
    
  -   `any/all` se admiten en los campos de tipo `Collection(Edm.String)`. 
    
  -   `any` solo puede usarse con las expresiones de igualdad simple o con una función `search.in`. Las expresiones simples constan de una comparación entre un campo único y un valor literal, por ejemplo, `Title eq 'Magna Carta'`.
    
  -   `all` solo puede usarse con las expresiones de desigualdad simples o con `not search.in`.   

- Funciones geoespaciales `geo.distance` y `geo.intersects`. La función `geo.distance` devuelve la distancia en kilómetros entre dos puntos, siendo uno de ellos un campo y el otro una constante que se pasa como parte del filtro. La función `geo.intersects` devuelve true si un punto determinado se encuentra dentro de un polígono determinado, donde el punto es un campo y el polígono se especifica como una constante que se pasa como parte del filtro.  

  El polígono es una superficie bidimensional almacenada como una secuencia de puntos que define un anillo delimitador (vea el ejemplo siguiente). El polígono debe estar cerrado, lo que significa que el primer y último conjunto de puntos deben ser los mismos. [Los puntos de un polígono deben estar ordenados en sentido contrario a las agujas del reloj](https://docs.microsoft.com/rest/api/searchservice/supported-data-types#Anchor_1).

  `geo.distance` devuelve la distancia en kilómetros en Azure Search. Esto difiere de otros servicios que admiten operaciones geoespaciales de OData, que normalmente devuelven las distancias en metros.  

  > [!NOTE]  
  >  Al usar geo.distance en un filtro, debe comparar la distancia devuelta por la función con una constante mediante `lt`, `le`, `gt` o `ge`. Los operadores `eq` y `ne` no se admiten cuando se comparan las distancias. Por ejemplo, este es un uso correcto de geo.distance: `$filter=geo.distance(location, geography'POINT(-122.131577 47.678581)') le 5`.  

- La función `search.in` prueba si un campo de cadena dado es igual a uno de una lista de valores determinada. También se puede usar en any o all para comparar un valor único de un campo de la colección de cadenas con una lista de valores determinada. La igualdad entre el campo y cada valor de la lista se determina distinguiendo entre mayúsculas y minúsculas, del mismo modo que para el operador `eq`. Por lo tanto, una expresión como `search.in(myfield, 'a, b, c')` es equivalente a `myfield eq 'a' or myfield eq 'b' or myfield eq 'c'`, salvo que `search.in` ofrecerá un rendimiento mucho mejor. 

   El primer parámetro para la función `search.in` es la referencia de campo de cadena (o una variable de rango a través de un campo de la colección de cadenas en el caso donde `search.in` se utiliza dentro de una expresión `any` o `all`). 
  
   El segundo parámetro es una cadena que contiene la lista de valores, separados por espacios y/o comas. 
  
   El tercer parámetro es una cadena donde cada carácter de la cadena o subconjunto de esta cadena se trata como un separador al analizar la lista de valores en el segundo parámetro. Si tiene que usar separadores que no sean espacios y comas porque sus valores incluyen dichos caracteres, puede especificar un tercer parámetro opcional para `search.in`. 

  > [!NOTE]   
  > Algunos escenarios requieren la comparación de un campo con un gran número de valores constantes. Por ejemplo, la implementación del recorte de seguridad con los filtros puede requerir la comparación del campo de identificador de documento con una lista de identificadores a la que se ha concedido acceso de lectura al usuario solicitante. En escenarios como este, es muy recomendable utilizar la función `search.in` en lugar de una disyunción más complicada de expresiones de igualdad. Por ejemplo, use `search.in(Id, '123, 456, ...')` en lugar de `Id eq 123 or Id eq 456 or ....`. 
  >
  > Si usa `search.in`, puede esperar un tiempo de respuesta de fracciones de segundo cuando el segundo parámetro contiene una lista de cientos o miles de valores. Tenga en cuenta que no hay ningún límite explícito en el número de elementos que puede pasar a `search.in`, aunque sigue estando limitado por el tamaño máximo de la solicitud. Sin embargo, la latencia aumentará a medida que crece el número de valores.

- La función `search.ismatch` evalúa la consulta de búsqueda como parte de una expresión de filtro. En el conjunto de resultados, se devolverán los documentos donde se encontraran coincidencias con la consulta de búsqueda. Están disponibles las siguientes sobrecargas de esta función:
  - `search.ismatch(search)`
  - `search.ismatch(search, searchFields)`
  - `search.ismatch(search, searchFields, queryType, searchMode)`

  donde: 
  
  - `search`: la consulta de búsqueda (en sintaxis de consulta [simple](query-simple-syntax.md) o [completa](query-lucene-syntax.md)). 
  - `queryType`: "simple" o "full", siendo "simple" la opción predeterminada. Especifica qué lenguaje de consulta se usó en el parámetro `search`.
  - `searchFields`: lista separada por comas de los campos de búsqueda para buscar, siendo el valor predeterminado todos los campos de búsqueda del índice.    
  - `searchMode`: "any" o "all", siendo "any" la opción predeterminada. Indica si alguno o todos los términos de búsqueda deben coincidir con el fin de que el documento cuente como una coincidencia.

  Todos los parámetros anteriores son equivalentes a los [parámetros de solicitud de búsqueda](https://docs.microsoft.com/rest/api/searchservice/search-documents) correspondientes.

- La función `search.ismatchscoring`, así como la función `search.ismatch`, devuelve true para los documentos donde se encontraron coincidencias con la consulta de búsqueda que se pasó como parámetro. La diferencia entre ellas es que la puntuación de relevancia de los documentos en los que se encontraron coincidencias con la consulta `search.ismatchscoring` contribuirá a la puntuación total de los documentos, mientras que en el caso de `search.ismatch`, la puntuación de los documentos no cambiará. Las siguientes sobrecargas de esta función están disponibles con parámetros idénticos a los de `search.ismatch`:
  - `search.ismatchscoring(search)`
  - `search.ismatchscoring(search, searchFields)`
  - `search.ismatchscoring(search, searchFields, queryType, searchMode)`

  Las funciones `search.ismatch` y `search.ismatchscoring` son totalmente ortogonales entre sí y el resto del álgebra del filtro. Esto significa que ambas funciones se pueden usar en la misma expresión de filtro. 

### <a name="geospatial-queries-and-polygons-spanning-the-180th-meridian"></a>Consultas geoespaciales y polígonos que abarcan el meridiano 180  
 Para muchas bibliotecas de consultas geoespaciales, formular una consulta que incluya el meridiano 180 (cerca del cambio de fecha) está fuera de los límites o requiere una solución alternativa, como dividir el polígono en dos partes, una a cada lado del meridiano.  

 En Azure Search, las consultas geoespaciales que incluyen la longitud de 180 grados funcionará según lo esperado si la forma de la consulta es rectangular y las coordenadas se alinean en un diseño de cuadrícula a lo largo de la longitud y la latitud (por ejemplo, `geo.intersects(location, geography'POLYGON((179 65,179 66,-179 66,-179 65,179 65))'`). En caso contrario, para formas no rectangulares o no alineadas, considere el enfoque del polígono dividido.  

<a name="bkmk_limits"></a>

## <a name="filter-size-limitations"></a>Limitaciones del tamaño del filtro 

 Hay límites de tamaño y complejidad para las expresiones de filtro que puede enviar a Azure Search. A grandes rasgos, los límites se basan en el número de cláusulas en su expresión de filtro. Como regla general, si tiene cientos de cláusulas, está en riesgo de alcanzar el límite. Es recomendable diseñar la aplicación de manera que no genere filtros de tamaño ilimitado.  


## <a name="filter-examples"></a>Ejemplos de filtros  

 Buscar todos los hoteles con una tarifa base inferior a 200 dólares que están clasificados como 4 o superior:  

```
$filter=baseRate lt 200.0 and rating ge 4
```

 Buscar todos los hoteles que no sean "Roach Motel" que se hayan reformado desde 2010:  

```
$filter=hotelName ne 'Roach Motel' and lastRenovationDate ge 2010-01-01T00:00:00Z
```

 Buscar todos los hoteles con una tarifa base inferior a 200 dólares que se hayan renovado desde 2010, con un literal datetime que incluya información de zona horaria para hora estándar del Pacífico:  

```
$filter=baseRate lt 200 and lastRenovationDate ge 2010-01-01T00:00:00-08:00
```

 Buscar todos los hoteles que tengan estacionamiento y no permitan fumar:  

```
$filter=parkingIncluded and not smokingAllowed
```

 \- O BIEN -  

```
$filter=parkingIncluded eq true and smokingAllowed eq false
```

 Buscar todos los hoteles que sean de lujo o que incluyan estacionamiento y tengan una valoración de cinco:  

```
$filter=(category eq 'Luxury' or parkingIncluded eq true) and rating eq 5
```

 Buscar todos los hoteles con la etiqueta "wifi" (donde cada hotel tiene etiquetas almacenadas en un campo Collection (EDM.String)):  

```
$filter=tags/any(t: t eq 'wifi')
```

 Buscar todos los hoteles sin la etiqueta "motel":  

```
$filter=tags/all(t: t ne 'motel')
```

 Buscar todos los hoteles con cualquier etiqueta:  

```
$filter=tags/any()
```

Buscar todos los hoteles que no tienen etiquetas:  

```
$filter=not tags/any()
```


 Buscar todos los hoteles a una distancia no superior a diez kilómetros de un punto de referencia determinado (donde la ubicación es un campo de tipo Edm.GeographyPoint):  

```
$filter=geo.distance(location, geography'POINT(-122.131577 47.678581)') le 10
```

 Buscar todos los hoteles dentro de una ventanilla dada descrita como un polígono (donde la ubicación es un campo de tipo Edm.GeographyPoint). Tenga en cuenta que el polígono está cerrado (el primer y el último conjunto de puntos deben ser los mismos) y [los puntos deben aparecer ordenados en sentido contrario a las agujas del reloj](https://docs.microsoft.com/rest/api/searchservice/supported-data-types#Anchor_1).

```
$filter=geo.intersects(location, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))')
```

 Buscar todos los hoteles que no tengan ningún valor en el campo "description" o que ese valor esté explícitamente establecido en null:  

```
$filter=description eq null
```

Buscar todos los hoteles con nombre igual a 'Motel Roach' o 'Hotel presupuesto'). Frases contienen espacios, que es un delimitador de forma predeterminada. Puede specicfy un delimitador alternativo de comillas simples como tercer parámetro de cadena:  

```
$filter=search.in(name, 'Roach motel,Budget hotel', ',')
```

Buscar todos los hoteles cuyo nombre sea "Roach motel" o "Budget hotel" separados por "|"):  

```
$filter=search.in(name, 'Roach motel|Budget hotel', '|')
```

Buscar todos los hoteles con la etiqueta "wifi" o "pool":  

```
$filter=tags/any(t: search.in(t, 'wifi, pool'))
```

Buscar a una coincidencia en frases dentro de una colección, como 'toalla calentado bastidores' o 'secarse el pelo incluidos' en las etiquetas. 

```
$filter=tags/any(t: search.in(t, 'heated towel racks,hairdryer included', ','))
```

Buscar todos los hoteles sin la etiqueta "motel" ni "cabin":  

```
$filter=tags/all(t: not search.in(t, 'motel, cabin'))
```

Buscar documentos con la palabra "waterfront". Esta consulta de filtro es idéntica a una [solicitud de búsqueda](https://docs.microsoft.com/rest/api/searchservice/search-documents) con `search=waterfront`.

```
$filter=search.ismatchscoring('waterfront')
```

Buscar documentos con la palabra "hostel" y una valoración superior o igual a cuatro, o documentos con la palabra "motel" y una valoración igual a cinco. Tenga en cuenta que esta solicitud no se puede expresar sin la función `search.ismatchscoring`.

```
$filter=search.ismatchscoring('hostel') and rating ge 4 or search.ismatchscoring('motel') and rating eq 5
```

Buscar documentos sin la palabra "luxury".

```
$filter=not search.ismatch('luxury') 
```

Buscar documentos con la frase "ocean view" o con una valoración igual a cinco. La consulta `search.ismatchscoring` se ejecutará solo en los campos `hotelName` y `description`.
Tenga en cuenta que los documentos que cumplan la segunda cláusula de la disyunción también se devolverán (hoteles con una valoración igual a cinco). Para dejar claro que esos documentos no cumplían ninguna de las partes puntuadas de la expresión, se devolverán con una puntuación igual a cero.

```
$filter=search.ismatchscoring('"ocean view"', 'description,hotelName') or rating eq 5
```

Buscar documentos donde los términos "hotel" y "airport" no estén separados más de cinco palabras entre sí en la descripción del hotel y donde no se permita fumar. Esta consulta utiliza el [lenguaje de consulta completo de Lucene](query-lucene-syntax.md).

```
$filter=search.ismatch('"hotel airport"~5', 'description', 'full', 'any') and not smokingAllowed 
```

## <a name="order-by-syntax"></a>Sintaxis OrderBy

El parámetro **$orderby** acepta una lista separada por comas de hasta 32 expresiones con la forma `sort-criteria [asc|desc]`. Los criterios de ordenación pueden ser el nombre de un campo `sortable` o una llamada a las funciones `geo.distance` o `search.score`. Puede usar `asc` o `desc` para especificar explícitamente el criterio de ordenación. El orden predeterminado es ascendente.

Si varios documentos tienen los mismos criterios de ordenación y la función `search.score` no se utiliza (por ejemplo, si se ordena por el campo numérico `rating` y tres documentos tienen una valoración de cuatro), los empates se resolverán por la puntuación del documento en orden descendente. Cuando las puntuaciones de documento sean las mismas (por ejemplo, cuando no haya ninguna consulta de búsqueda de texto completo especificada en la solicitud), entonces el orden relativo de los documentos empatados es indeterminado.
 
Puede especificar varios criterios de ordenación. El orden de las expresiones determina el criterio de ordenación final. Por ejemplo, para clasificar en orden descendente por puntuación, seguido de la valoración, la sintaxis sería `$orderby=search.score() desc,rating desc`.

La sintaxis de `geo.distance` en **$orderby** es la misma que la de **$filter**. Cuando se usa `geo.distance` en **$orderby**, el campo al que se aplica debe ser de tipo `Edm.GeographyPoint` y también debe ser `sortable`.  

La sintaxis de `search.score` en **$orderby** es `search.score()`. La función `search.score` no toma ningún parámetro.  
 

## <a name="order-by-examples"></a>Ejemplos de OrderBy

Clasificar hoteles en orden ascendente por la tarifa base:

```
$orderby=baseRate asc
```

Clasificar hoteles en orden descendente por valoración y después en orden ascendente por tarifa base (recuerde que el orden ascendente es la opción predeterminada):

```
$orderby=rating desc,baseRate
```

Clasificar hoteles en orden descendente por valoración y después en orden ascendente por distancia desde las coordenadas dadas:

```
$orderby=rating desc,geo.distance(location, geography'POINT(-122.131577 47.678581)') asc
```

Clasificar hoteles en orden descendente por search.score y valoración y después en orden ascendente por distancia desde las coordenadas dadas de manera que, entre dos hoteles con valoraciones idénticas, el más cercano aparezca primero en la lista:

```
$orderby=search.score() desc,rating desc,geo.distance(location, geography'POINT(-122.131577 47.678581)') asc
```
<a name="bkmk_unsupported"></a>

## <a name="unsupported-odata-syntax"></a>Sintaxis de OData no admitida

-   Expresiones aritméticas  

-   Funciones (excepto las funciones geoespaciales de distancia e intersecciones)  

-   `any/all` con expresiones lambda arbitrarias  

## <a name="see-also"></a>Vea también  

+ [Navegación por facetas en Azure Search](search-faceted-navigation.md) 
+ [Filtros de Azure Search](search-filters.md) 
+ [Search Documents &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) (Búsqueda en documentos [API REST de Azure Search Service]) 
+ [Sintaxis de consulta de Lucene](query-lucene-syntax.md)
+ [Simple query syntax in Azure Search](query-simple-syntax.md) (Sintaxis de consulta simple en Azure Search)   
