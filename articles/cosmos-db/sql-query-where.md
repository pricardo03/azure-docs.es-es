---
title: Cláusula WHERE en Azure Cosmos DB
description: Obtenga información sobre la cláusula WHERE de SQL para Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: mjbrown
ms.openlocfilehash: 6a942e48ffea7785fe971cc2f8fa66e8569ed672
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/24/2019
ms.locfileid: "67343228"
---
# <a name="where-clause"></a>Cláusula WHERE

La cláusula WHERE opcional (`WHERE <filter_condition>`) especifica las condiciones que deben cumplir los elementos JSON de origen para que la consulta los incluya en los resultados. Un elemento JSON debe evaluar que las condiciones especificadas sean `true` para que se le tenga en cuenta para el resultado. La capa de índice usa la cláusula WHERE para determinar el subconjunto más pequeño de documentos de origen que puede formar parte del resultado.
  
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
  

## <a name="remarks"></a>Comentarios
  
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
|Cadena     |  \|\| (concatenar) |

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

También puede usar referencias de propiedad en las consultas. Por ejemplo, `SELECT * FROM Families f WHERE f.isRegistered` devuelve el elemento JSON que contiene la propiedad `isRegistered` con un valor igual a `true`. Cualquier otro valor, como `false`, `null`, `Undefined`, `<number>`, `<string>`, `<object>` o `<array>`, excluye el elemento del resultado. 

## <a name="next-steps"></a>Pasos siguientes

- [Introducción](sql-query-getting-started.md)
- [Ejemplos de .NET de Azure Cosmos DB](https://github.com/Azure/azure-cosmosdb-dotnet)
- [Cláusula FROM](sql-query-from.md)