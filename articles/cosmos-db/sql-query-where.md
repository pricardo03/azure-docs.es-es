---
title: Cláusula WHERE en Azure Cosmos DB
description: Obtenga información sobre la cláusula WHERE de SQL para Azure Cosmos DB.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: tisande
ms.openlocfilehash: 31653b598f0f3a79bf7f9c09231b1d111f167a16
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2020
ms.locfileid: "76982236"
---
# <a name="where-clause-in-azure-cosmos-db"></a>Cláusula WHERE en Azure Cosmos DB

La cláusula WHERE opcional (`WHERE <filter_condition>`) especifica las condiciones que deben cumplir los elementos JSON de origen para que la consulta los incluya en los resultados. Un elemento JSON debe evaluar que las condiciones especificadas sean `true` para que se le tenga en cuenta para el resultado. La capa de índice usa la cláusula WHERE para determinar el subconjunto menor de documentos de origen que puede formar parte del resultado.
  
## <a name="syntax"></a>Sintaxis
  
```sql  
WHERE <filter_condition>  
<filter_condition> ::= <scalar_expression>  
  
```  
  
## <a name="arguments"></a>Argumentos

- `<filter_condition>`  
  
   Especifica la condición que debe cumplirse para que se devuelvan los documentos.  
  
- `<scalar_expression>`  
  
   Expresión que representa el valor que hay que calcular. Para más información, consulte [Expresiones escalares](sql-query-scalar-expressions.md).  
  

## <a name="remarks"></a>Observaciones
  
  Para que el documento se devuelva, debe establecerse en true una expresión especificada como condición de filtro. Solo un valor booleano true cumplirá la condición, los demás valores (undefined, null, false, número, matriz u objeto) no cumplirán la condición. 

## <a name="examples"></a>Ejemplos

En la consulta siguiente se solicitan elementos que contienen una propiedad `id` cuyo valor es `AndersenFamily`. Excluye todo elemento que no tiene una propiedad `id` o cuyo valor no coincide con `AndersenFamily`.

```sql
    SELECT f.address
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

Los resultados son:

```json
    [{
      "address": {
        "state": "WA",
        "county": "King",
        "city": "Seattle"
      }
    }]
```

### <a name="scalar-expressions-in-the-where-clause"></a>Expresiones escalares en la cláusula WHERE

En el ejemplo anterior se mostraba una sencilla consulta de igualdad. La API de SQL también admite varias [expresiones escalares](sql-query-scalar-expressions.md). Las que más se suelen usar son binarias y unarias. Las referencias de propiedad del objeto JSON de origen también son expresiones válidas.

Puede usar los operadores binarios admitidos siguientes:  

|**Nombre de operador**  | **Valores** |
|---------|---------|
|Aritméticos | +,-,*,/,% |
|Bit a bit    | \|, &, ^, <<, >>, >>> (desplazamiento a la derecha con relleno de ceros) |
|Lógicos    | AND, OR, NOT      |
|De comparación | =, !=, &lt;, &gt;, &lt;=, &gt;=, <> |
|String     |  \|\| (concatenar) |

Las consultas siguientes usan operadores binarios:

```sql
    SELECT *
    FROM Families.children[0] c
    WHERE c.grade % 2 = 1     -- matching grades == 5, 1

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade ^ 4 = 1    -- matching grades == 5

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade >= 5    -- matching grades == 5
```

También puede usar los operadores unarios +, -, ~ y NOT en las consultas, tal como se muestra en los ejemplos siguientes:

```sql
    SELECT *
    FROM Families.children[0] c
    WHERE NOT(c.grade = 5)  -- matching grades == 1

    SELECT *
    FROM Families.children[0] c
    WHERE (-c.grade = -5)  -- matching grades == 5
```

Igualmente, puede usar referencias de propiedad en las consultas. Por ejemplo, `SELECT * FROM Families f WHERE f.isRegistered` devuelve el elemento JSON que contiene la propiedad `isRegistered` con un valor igual a `true`. Cualquier otro valor, como `false`, `null`, `Undefined`, `<number>`, `<string>`, `<object>` o `<array>`, excluye el elemento del resultado.

## <a name="next-steps"></a>Pasos siguientes

- [Introducción](sql-query-getting-started.md)
- [Palabra clave IN](sql-query-keywords.md#in)
- [Cláusula FROM](sql-query-from.md)