---
title: Cláusula GROUP BY en Azure Cosmos DB
description: Obtenga información sobre la cláusula GROUP BY de SQL para Azure Cosmos DB.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/11/2019
ms.author: tisande
ms.openlocfilehash: d92e24836a0eb5757de9bbdb516be290456deb7f
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72333421"
---
# <a name="group-by-clause-in-azure-cosmos-db"></a>Cláusula GROUP BY en Azure Cosmos DB

La cláusula GROUP BY divide los resultados de la consulta de acuerdo con los valores de una o más propiedades especificadas.

> [!NOTE]
> Actualmente, Azure Cosmos DB admite GROUP BY en el [SDK de .NET 3.3](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/3.3.0) o posterior.
> La compatibilidad con SDK de otros lenguajes y Azure Portal no está disponible actualmente, pero está prevista.

## <a name="syntax"></a>Sintaxis

```sql  
<group_by_clause> ::= GROUP BY <scalar_expression_list>

<scalar_expression_list> ::=
          <scalar_expression>
        | <scalar_expression_list>, <scalar_expression>
```  

## <a name="arguments"></a>Argumentos

- `<scalar_expression_list>`

   Especifica las expresiones que se usarán para dividir los resultados de la consulta.

- `<scalar_expression>`
  
   Se permite cualquier expresión escalar, excepto para las subconsultas escalares y los agregados escalares. Cada expresión escalar debe contener al menos una referencia de propiedad. No hay ningún límite en cuanto al número de expresiones individuales o la cardinalidad de cada expresión.

## <a name="remarks"></a>Comentarios
  
  Cuando una consulta utiliza una cláusula GROUP BY, la cláusula SELECT solo puede contener el subconjunto de propiedades y las funciones del sistema incluidas en la cláusula GROUP BY. Una excepción son las [funciones del sistema de agregado](sql-query-aggregates.md), que pueden aparecer en la cláusula SELECT sin incluirse en la cláusula GROUP BY. También puede incluir siempre valores literales en la cláusula SELECT.

  La cláusula GROUP BY debe estar después de las cláusulas SELECT, FROM y WHERE y antes de la cláusula OFFSET LIMIT. Actualmente no se puede usar GROUP BY con una cláusula ORDER BY, si bien está previsto que sea posible hacerlo en el futuro.

  La cláusula GROUP BY no permite nada de lo siguiente:
  
- Propiedades de alias o funciones del sistema de alias (el alias sigue estando permitido en la cláusula SELECT)
- Subconsultas
- Funciones del sistema de agregado (solo se permiten en la cláusula SELECT)

## <a name="examples"></a>Ejemplos

En estos ejemplos, se usa el conjunto de datos de nutrición disponible mediante el [Sitio de prueba de consultas de Azure Cosmos DB](https://www.documentdb.com/sql/demo).

Por ejemplo, esta es una consulta que devuelve el recuento total de elementos de cada grupo foodGroup:

```sql
SELECT TOP 4 COUNT(1) AS foodGroupCount, f.foodGroup
FROM Food f
GROUP BY f.foodGroup
```

Estos son algunos de los resultados (la palabra clave TOP se usa para limitar los resultados):

```json
[{
  "foodGroup": "Fast Foods",
  "foodGroupCount": 371
},
{
  "foodGroup": "Finfish and Shellfish Products",
  "foodGroupCount": 267
},
{
  "foodGroup": "Meals, Entrees, and Side Dishes",
  "foodGroupCount": 113
},
{
  "foodGroup": "Sausages and Luncheon Meats",
  "foodGroupCount": 244
}]
```

Esta consulta tiene dos expresiones que se usan para dividir los resultados:

```sql
SELECT TOP 4 COUNT(1) AS foodGroupCount, f.foodGroup, f.version
FROM Food f
GROUP BY f.foodGroup, f.version
```

Estos son algunos de los resultados:

```json
[{
  "version": 1,
  "foodGroup": "Nut and Seed Products",
  "foodGroupCount": 133
},
{
  "version": 1,
  "foodGroup": "Finfish and Shellfish Products",
  "foodGroupCount": 267
},
{
  "version": 1,
  "foodGroup": "Fast Foods",
  "foodGroupCount": 371
},
{
  "version": 1,
  "foodGroup": "Sausages and Luncheon Meats",
  "foodGroupCount": 244
}]
```

Esta consulta tiene una función del sistema en la cláusula GROUP BY:

```sql
SELECT TOP 4 COUNT(1) AS foodGroupCount, UPPER(f.foodGroup) AS upperFoodGroup
FROM Food f
GROUP BY UPPER(f.foodGroup)
```

Estos son algunos de los resultados:

```json
[{
  "foodGroupCount": 371,
  "upperFoodGroup": "FAST FOODS"
},
{
  "foodGroupCount": 267,
  "upperFoodGroup": "FINFISH AND SHELLFISH PRODUCTS"
},
{
  "foodGroupCount": 389,
  "upperFoodGroup": "LEGUMES AND LEGUME PRODUCTS"
},
{
  "foodGroupCount": 113,
  "upperFoodGroup": "MEALS, ENTREES, AND SIDE DISHES"
}]
```

Esta consulta usa palabras clave y funciones del sistema en la expresión de propiedad del elemento:

```sql
SELECT COUNT(1) AS foodGroupCount, ARRAY_CONTAINS(f.tags, {name: 'orange'}) AS containsOrangeTag,  f.version BETWEEN 0 AND 2 AS correctVersion
FROM Food f
GROUP BY ARRAY_CONTAINS(f.tags, {name: 'orange'}), f.version BETWEEN 0 AND 2
```

Los resultados son:

```json
[{
  "correctVersion": true,
  "containsOrangeTag": false,
  "foodGroupCount": 8608
},
{
  "correctVersion": true,
  "containsOrangeTag": true,
  "foodGroupCount": 10
}]
```

## <a name="next-steps"></a>Pasos siguientes

- [Introducción](sql-query-getting-started.md)
- [Cláusula SELECT](sql-query-select.md)
- [Funciones de agregado](sql-query-aggregates.md)
