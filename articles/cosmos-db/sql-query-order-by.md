---
title: Cláusula ORDER BY en Azure Cosmos DB
description: Obtenga información sobre la cláusula ORDER BY de SQL para Azure Cosmos DB. Use SQL como lenguaje de consulta de JSON para Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: mjbrown
ms.openlocfilehash: 5cae2bdd7d1f2f26e626c81ea95d2cee3cc8ae13
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75444785"
---
# <a name="order-by-clause-in-azure-cosmos-db"></a>Cláusula ORDER BY en Azure Cosmos DB

La cláusula opcional ORDER BY especifica el criterio de ordenación para los resultados que devuelve la consulta.

## <a name="syntax"></a>Sintaxis
  
```sql  
ORDER BY <sort_specification>  
<sort_specification> ::= <sort_expression> [, <sort_expression>]  
<sort_expression> ::= {<scalar_expression> [ASC | DESC]} [ ,...n ]  
```  

## <a name="arguments"></a>Argumentos
  
- `<sort_specification>`  
  
   Especifica una propiedad o una expresión para ordenar el conjunto de resultados de la consulta. Se puede especificar una columna de ordenación como alias de nombre o de propiedad.  
  
   Se pueden especificar varias propiedades. Los nombres de propiedad deben ser únicos. La secuencia de las propiedades de ordenación en la cláusula ORDER BY define la organización del conjunto de resultados ordenado. Es decir, el conjunto de resultados se ordena en función de la primera propiedad, esa lista ordenada se ordena en función de la segunda propiedad y así sucesivamente.  
  
   Los nombres de propiedad indicados en la cláusula ORDER BY deben corresponderse con una propiedad de la lista seleccionada o con una que se defina en la colección que se especifique en la cláusula FROM sin ambigüedades.  
  
- `<sort_expression>`  
  
   Especifica una o más propiedades o expresiones para ordenar el conjunto de resultados de la consulta.  
  
- `<scalar_expression>`  
  
   Consulte la sección [Expresiones escalares](sql-query-scalar-expressions.md) para más información.  
  
- `ASC | DESC`  
  
   Indica que los valores de la columna especificada se deben ordenar en sentido ascendente o descendente. ASC ordena del valor mínimo al valor máximo. DESC ordena del valor máximo al valor mínimo. ASC es el criterio de ordenación predeterminado. Los valores NULL se tratan como los valores más bajos posibles.  
  
## <a name="remarks"></a>Observaciones  
  
   La cláusula ORDER BY requiere que la directiva de indexación incluya un índice para los campos que se ordenan. El tiempo de ejecución de consulta de Azure Cosmos DB admite la ordenación con un nombre de propiedad y no con las propiedades calculadas. Azure Cosmos DB admite varias propiedades de ORDER BY. Para ejecutar una consulta con varias propiedades ORDER BY, debe definir un [índice compuesto](index-policy.md#composite-indexes) en los campos que se ordenan.
   
> [!Note] 
> Al usar el SDK de .NET 3.4.0 o una versión posterior, si es posible que las propiedades que se van a usar para ordenar no estén definidas en algunos documentos, debe crear explícitamente un índice en esas propiedades. La directiva de indexación predeterminada no permite la recuperación de los documentos en los que la propiedad de ordenación no está definida.

## <a name="examples"></a>Ejemplos

Por ejemplo, aquí hay una consulta que recupera las familias por nombre de la ciudad de residencia en orden ascendente:

```sql
    SELECT f.id, f.address.city
    FROM Families f
    ORDER BY f.address.city
```

Los resultados son:

```json
    [
      {
        "id": "WakefieldFamily",
        "city": "NY"
      },
      {
        "id": "AndersenFamily",
        "city": "Seattle"
      }
    ]
```

La consulta siguiente recupera los `id` de familia en el orden de la fecha de creación del elemento. El elemento `creationDate` es un número que representa la *hora de época* o el tiempo transcurrido desde el 1 de enero de 1970, en segundos.

```sql
    SELECT f.id, f.creationDate
    FROM Families f
    ORDER BY f.creationDate DESC
```

Los resultados son:

```json
    [
      {
        "id": "WakefieldFamily",
        "creationDate": 1431620462
      },
      {
        "id": "AndersenFamily",
        "creationDate": 1431620472
      }
    ]
```

Además, puede ordenar por varias propiedades. Una consulta que ordena por varias propiedades requiere un [índice compuesto](index-policy.md#composite-indexes). Considere la siguiente consulta:

```sql
    SELECT f.id, f.creationDate
    FROM Families f
    ORDER BY f.address.city ASC, f.creationDate DESC
```

Esta consulta recupera los `id` de familia en orden ascendente del nombre de la ciudad. Si varios elementos tienen el mismo nombre de ciudad, la consulta se ordena por `creationDate` en orden descendente.

## <a name="next-steps"></a>Pasos siguientes

- [Introducción](sql-query-getting-started.md)
- [Cláusula SELECT](sql-query-select.md)
- [Cláusula OFFSET LIMIT](sql-query-offset-limit.md)
