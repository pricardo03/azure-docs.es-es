---
title: 'Referencia de select de OData: Azure Search'
description: Referencia del lenguaje OData para la sintaxis de select en consultas de Azure Search.
ms.date: 06/13/2019
services: search
ms.service: search
ms.topic: conceptual
author: Brjohnstmsft
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
ms.openlocfilehash: 64e9ad75d88f595ab5def6fe8b63fee9407ae0fe
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2019
ms.locfileid: "69647870"
---
# <a name="odata-select-syntax-in-azure-search"></a>Sintaxis de $select de OData en Azure Search

 Puede usar el [parámetro **$select** de OData](query-odata-filter-orderby-syntax.md) para elegir qué campos quiere incluir en los resultados de la búsqueda de Azure Search. En este artículo se describe con detalle la sintaxis de **$select**. Para obtener información general sobre cómo usar **$select** al presentar los resultados de la búsqueda, vea [Uso de los resultados de la búsqueda en Azure Search](search-pagination-page-layout.md).

## <a name="syntax"></a>Sintaxis

El parámetro **$select** determina qué campos de cada documento se devuelven en el conjunto de resultados de la consulta. La siguiente EBNF ([notación de Backus-Naur extendida](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) define la gramática del parámetro **$select**:

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
select_expression ::= '*' | field_path(',' field_path)*

field_path ::= identifier('/'identifier)*
```

También está disponible un diagrama de sintaxis interactivo:

> [!div class="nextstepaction"]
> [Diagrama de sintaxis de OData para Azure Search](https://azuresearch.github.io/odata-syntax-diagram/#select_expression)

> [!NOTE]
> Vea [Referencia de sintaxis de expresiones OData para Azure Search](search-query-odata-syntax-reference.md) para obtener la EBNF completa.

El parámetro **$select** tiene dos formatos:

1. Una sola estrella (`*`), que indica que se deben devolver todos los campos recuperables, o bien
1. Una lista separada por comas de las rutas de acceso de campo, para identificar qué campos se deben devolver.

Al usar el segundo formato, en la lista solo se pueden especificar campos recuperables.

Si se indica un campo complejo sin especificar sus subcampos de forma explícita, todos los campos secundarios recuperables se incluirán en el conjunto de resultados de la consulta. Por ejemplo, imagine que el índice tiene un campo `Address` con los subcampos `Street`, `City` y `Country`, todos ellos recuperables. Si se especifica `Address` en **$select**, en los resultados de la consulta se incluirán los tres subcampos.

## <a name="examples"></a>Ejemplos

En los resultados se incluyen los campos de nivel superior `HotelId`, `HotelName` y `Rating`, así como el subcampo `City` de `Address`:

    $select=HotelId, HotelName, Rating, Address/City

Un resultado de ejemplo podría tener este aspecto:

```json
{
  "HotelId": "1",
  "HotelName": "Secret Point Motel",
  "Rating": 4,
  "Address": {
    "City": "New York"
  }
}
```

En los resultados se incluye el campo de nivel superior `HotelName`, así como todos los subcampos de `Address` y los subcampos `Type` y `BaseRate` de todos los objetos de la colección `Rooms`:

    $select=HotelName, Address, Rooms/Type, Rooms/BaseRate

Un resultado de ejemplo podría tener este aspecto:

```json
{
  "HotelName": "Secret Point Motel",
  "Rating": 4,
  "Address": {
    "StreetAddress": "677 5th Ave",
    "City": "New York",
    "StateProvince": "NY",
    "Country": "USA",
    "PostalCode": "10022"
  },
  "Rooms": [
    {
      "Type": "Budget Room",
      "BaseRate": 9.69
    },
    {
      "Type": "Budget Room",
      "BaseRate": 8.09
    }
  ]
}
```

## <a name="next-steps"></a>Pasos siguientes  

- [Uso de los resultados de la búsqueda en Azure Search](search-pagination-page-layout.md)
- [Información general sobre el lenguaje de expresiones OData para Azure Search](query-odata-filter-orderby-syntax.md)
- [Referencia de sintaxis de expresiones OData para Azure Search](search-query-odata-syntax-reference.md)
- [Search Documents &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) (Búsqueda en documentos [API REST de Azure Search Service])
