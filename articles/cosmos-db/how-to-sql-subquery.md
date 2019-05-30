---
title: Subconsultas SQL para Azure Cosmos DB
description: Obtenga información acerca de las subconsultas SQL y sus casos de uso comunes de Azure Cosmos DB
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: tisande
ms.openlocfilehash: 68465f4ca195930ce08bb579e68d0227e9c18dd6
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66242844"
---
# <a name="sql-subquery-examples-for-azure-cosmos-db"></a>Ejemplos de subconsulta SQL para Azure Cosmos DB

Una subconsulta es una consulta anidada dentro de otra consulta. Una subconsulta también se denomina consulta interna o selección interna. La instrucción que contiene una subconsulta normalmente se llama a una consulta externa.

En este artículo se describe las subconsultas SQL y sus casos de uso comunes de Azure Cosmos DB.

## <a name="types-of-subqueries"></a>Tipos de subconsultas

Hay dos tipos principales de subconsultas:

* **Correlated**: Una subconsulta que hace referencia a valores de la consulta externa. La subconsulta se evalúa una vez para cada fila que procesa la consulta externa.
* **No correlacionados**: Una subconsulta que es independiente de la consulta externa. Se puede ejecutar en su propio sin tener que depender del exterior consulta.

> [!NOTE]
> Azure Cosmos DB admite solo las subconsultas correlacionadas.

Las subconsultas se pueden clasificar aún más en función del número de filas y columnas que devuelven. Hay tres tipos:
* **Tabla**: Devuelve varias filas y varias columnas.
* **Multivalor**: Devuelve varias filas y una sola columna.
* **Scalar**: Devuelve una sola fila y una sola columna.

Las consultas SQL de Azure Cosmos DB siempre devuelven una sola columna (un valor simple o un documento complejo). Por lo tanto, solo las subconsultas escalares y de varios valores son aplicables en Azure Cosmos DB. Puede usar una subconsulta de valor múltiple solo en la cláusula FROM como una expresión relacional. Puede usar una subconsulta escalar, como una expresión escalar en la instrucción SELECT o una cláusula WHERE o como una expresión relacional en la cláusula FROM.


## <a name="multi-value-subqueries"></a>Subconsultas de varios valores

Las subconsultas de varios valores devuelven un conjunto de documentos y siempre se usan dentro de la cláusula FROM. Se usan para:

* La optimización de expresiones de combinación. 
* La evaluación de expresiones costosas una vez y hacer referencia a varias veces.

### <a name="optimize-join-expressions"></a>Optimizar las expresiones de combinación

Las subconsultas de varios valores pueden optimizar las expresiones de combinación mediante la inserción de predicados después de cada expresión select varios en lugar de después de todas las combinaciones cruzadas en la cláusula WHERE.

Considere la siguiente consulta:

```sql
SELECT Count(1) AS Count
FROM c
JOIN t IN c.tags
JOIN n IN c.nutrients
JOIN s IN c.servings
WHERE t.name = 'infant formula' AND (n.nutritionValue > 0 
AND n.nutritionValue < 10) AND s.amount > 1
```

Para esta consulta, el índice coincidirá con cualquier documento que tiene una etiqueta con el nombre "preparados para fórmula." Es un elemento con un valor entre 0 y 10 de nutrientes y un elemento de servicio con una cantidad mayor que 1. La expresión de combinación aquí llevará a cabo el producto cruzado de todos los elementos de matrices de porciones, nutrientes y etiquetas para cada documento coincidente antes de aplicar cualquier filtro. 

La cláusula WHERE, a continuación, aplicará el filtro de predicado en cada < c, t, n, s > múltiple. Por ejemplo, si un documento coincidente tiene 10 elementos en cada una de las tres matrices, se expandirá y 1 x 10 x 10 x 10 (es decir, 1000) tuplas. Usar subconsultas aquí puede ayudar a filtrar los elementos de matriz combinada antes de unirse a la siguiente expresión.

Esta consulta es equivalente al anterior, pero utiliza subconsultas:

```sql
SELECT Count(1) AS Count
FROM c
JOIN (SELECT VALUE t FROM t IN c.tags WHERE t.name = 'infant formula')
JOIN (SELECT VALUE n FROM n IN c.nutrients WHERE n.nutritionValue > 0 AND n.nutritionValue < 10)
JOIN (SELECT VALUE s FROM s IN c.servings WHERE s.amount > 1)
```

Se supone que solo un elemento de la matriz de etiquetas coincide con el filtro, y hay cinco elementos para las matrices de nutrientes y porciones. Las expresiones de combinación, a continuación, se expandirán a 1 x 1 x 5 x 5 = 25 elementos, en lugar de 1000 elementos en la primera consulta.

### <a name="evaluate-once-and-reference-many-times"></a>Evaluar una vez y referencia varias veces

Subconsultas pueden ayudar a optimizar las consultas con expresiones costosas como funciones definidas por el usuario (UDF), cadenas complejas o expresiones aritméticas. Puede usar una subconsulta junto con una expresión de combinación para evaluar la expresión una vez pero haga referencia a ella varias veces.

La siguiente consulta ejecuta la UDF `GetMaxNutritionValue` dos veces:

```sql
SELECT c.id, udf.GetMaxNutritionValue(c.nutrients) AS MaxNutritionValue
FROM c
WHERE udf.GetMaxNutritionValue(c.nutrients) > 100
```

Esta es una consulta equivalente que se ejecuta la UDF de una sola vez:

```sql
SELECT TOP 1000 c.id, MaxNutritionValue
FROM c
JOIN (SELECT VALUE udf.GetMaxNutritionValue(c.nutrients)) MaxNutritionValue
WHERE MaxNutritionValue > 100
``` 

> [!NOTE] 
> Tenga en cuenta el comportamiento del producto cruzado de las expresiones de combinación. Si se puede evaluar la expresión de UDF sin definir, debe asegurarse de que la expresión de combinación siempre produce una sola fila devolviendo un objeto de la subconsulta en lugar de con el valor directamente.
>

Este es un ejemplo similar que devuelve un objeto en lugar de un valor:

```sql
SELECT TOP 1000 c.id, m.MaxNutritionValue
FROM c
JOIN (SELECT udf.GetMaxNutritionValue(c.nutrients) AS MaxNutritionValue) m
WHERE m.MaxNutritionValue > 100
```

El enfoque no se limita a las UDF. Se aplica a cualquier expresión puede ser costosa. Por ejemplo, puede tomar el mismo enfoque con la función matemática `avg`:

```sql
SELECT TOP 1000 c.id, AvgNutritionValue
FROM c
JOIN (SELECT VALUE avg(n.nutritionValue) FROM n IN c.nutrients) AvgNutritionValue
WHERE AvgNutritionValue > 80
```

### <a name="mimic-join-with-external-reference-data"></a>Imitar la combinación con datos de referencia externa

A menudo, es posible que deba hacer referencia a datos estáticos que no suele cambian, por ejemplo, unidades de medida o códigos de país. Es mejor no duplicarla dichos datos para cada documento. Evitar esta duplicación guardará en el almacenamiento y mejorar el rendimiento de escritura al mantener el tamaño del documento más pequeños. Puede usar una subconsulta para imitar la semántica de combinación interna con una colección de datos de referencia.

Por ejemplo, considere este conjunto de datos de referencia:

| **Unidad** | **Nombre**            | **Multiplicador** | **Unidad base** |
| -------- | ------------------- | -------------- | ------------- |
| ng       | Nanogram            | 1.00E-09       | Gram          |
| µg       | Graduada en microgramos           | 1.00E-06       | Gram          |
| mg       | Mg           | 1.00E-03       | Gram          |
| g        | Gram                | 1.00E+00       | Gram          |
| kg       | Kilogramo            | 1.00E + 03       | Gram          |
| Mg       | Megagram            | 1.00E + 06       | Gram          |
| Gg       | Gigagram            | 1.00E+09       | Gram          |
| nJ       | Nanojoule           | 1.00E-09       | Joule         |
| µJ       | Microjoule          | 1.00E-06       | Joule         |
| mJ       | Millijoule          | 1.00E-03       | Joule         |
| J        | Joule               | 1.00E+00       | Joule         |
| kJ       | Kilojoule           | 1.00E + 03       | Joule         |
| MJ       | Megajoule           | 1.00E + 06       | Joule         |
| GJ       | Gigajoule           | 1.00E+09       | Joule         |
| cal      | Calorie             | 1.00E+00       | calorie       |
| kcal     | Calorie             | 1.00E + 03       | calorie       |
| IU       | Unidades internacionales |                |               |


La siguiente consulta imita la unión con estos datos para que agregue el nombre de la unidad a la salida:

```sql
SELECT TOP 10 n.id, n.description, n.nutritionValue, n.units, r.name
FROM food
JOIN n IN food.nutrients
JOIN r IN (
    SELECT VALUE [
        {unit: 'ng', name: 'nanogram', multiplier: 0.000000001, baseUnit: 'gram'},
        {unit: 'µg', name: 'microgram', multiplier: 0.000001, baseUnit: 'gram'},
        {unit: 'mg', name: 'milligram', multiplier: 0.001, baseUnit: 'gram'},
        {unit: 'g', name: 'gram', multiplier: 1, baseUnit: 'gram'},
        {unit: 'kg', name: 'kilogram', multiplier: 1000, baseUnit: 'gram'},
        {unit: 'Mg', name: 'megagram', multiplier: 1000000, baseUnit: 'gram'},
        {unit: 'Gg', name: 'gigagram', multiplier: 1000000000, baseUnit: 'gram'},
        {unit: 'nJ', name: 'nanojoule', multiplier: 0.000000001, baseUnit: 'joule'},
        {unit: 'µJ', name: 'microjoule', multiplier: 0.000001, baseUnit: 'joule'},
        {unit: 'mJ', name: 'millijoule', multiplier: 0.001, baseUnit: 'joule'},
        {unit: 'J', name: 'joule', multiplier: 1, baseUnit: 'joule'},
        {unit: 'kJ', name: 'kilojoule', multiplier: 1000, baseUnit: 'joule'},
        {unit: 'MJ', name: 'megajoule', multiplier: 1000000, baseUnit: 'joule'},
        {unit: 'GJ', name: 'gigajoule', multiplier: 1000000000, baseUnit: 'joule'},
        {unit: 'cal', name: 'calorie', multiplier: 1, baseUnit: 'calorie'},
        {unit: 'kcal', name: 'Calorie', multiplier: 1000, baseUnit: 'calorie'},
        {unit: 'IU', name: 'International units'}
    ]
)
WHERE n.units = r.unit
```

## <a name="scalar-subqueries"></a>Subconsultas escalares

Una expresión de subconsulta escalar es una subconsulta que se evalúa como un valor único. El valor de la expresión de subconsulta escalar es el valor de la proyección (cláusula SELECT) de la subconsulta.  Puede usar una expresión de subconsulta escalar en muchos lugares donde una expresión escalar es válida. Por ejemplo, puede usar una subconsulta escalar en cualquier expresión en tanto la instrucción SELECT y cláusulas WHERE.

Uso de una subconsulta escalar siempre no ayuda a optimizar, sin embargo. Por ejemplo, proporciona ninguna ventaja en el consumo de recursos de unidad (RU) o la latencia pasando una subconsulta escalar como argumento a un sistema o funciones definidas por el usuario.

Subconsultas escalares se pueden clasificar como:
* Expresión simple de subconsultas escalares
* Subconsultas escalares agregadas

### <a name="simple-expression-scalar-subqueries"></a>Expresión simple de subconsultas escalares

Una subconsulta escalar simple-expression es una subconsulta correlacionada con una cláusula SELECT que no contiene las expresiones de agregado. Estas subconsultas no proporcionan ninguna ventaja de optimización porque el compilador convierte en una expresión simple mayor. No hay ningún contexto correlacionado entre las consultas internas y externas.

Estos son algunos ejemplos:

**Ejemplo 1**

```sql
SELECT 1 AS a, 2 AS b
```

Puede volver a escribir esta consulta, utilizando una subconsulta escalar sencilla expresión, para:

```sql
SELECT (SELECT VALUE 1) AS a, (SELECT VALUE 2) AS b
```

Ambas consultas producen esta salida:

```json
[
  { "a": 1, "b": 2 }
]
```

**Ejemplo 2**

```sql
SELECT TOP 5 Concat('id_', f.id) AS id
FROM food f
```

Puede volver a escribir esta consulta, utilizando una subconsulta escalar sencilla expresión, para:

```sql
SELECT TOP 5 (SELECT VALUE Concat('id_', f.id)) AS id
FROM food f
```

Salida de la consulta:

```json
[
  { "id": "id_03226" },
  { "id": "id_03227" },
  { "id": "id_03228" },
  { "id": "id_03229" },
  { "id": "id_03230" }
]
```

**Ejemplo 3**

```sql
SELECT TOP 5 f.id, Contains(f.description, 'fruit') = true ? f.description : undefined
FROM food f
```

Puede volver a escribir esta consulta, utilizando una subconsulta escalar sencilla expresión, para:

```sql
SELECT TOP 10 f.id, (SELECT f.description WHERE Contains(f.description, 'fruit')).description
FROM food f
```

Salida de la consulta:

```json
[
  { "id": "03230" },
  { "id": "03238", "description":"Babyfood, dessert, tropical fruit, junior" },
  { "id": "03229" },
  { "id": "03226", "description":"Babyfood, dessert, fruit pudding, orange, strained" },
  { "id": "03227" }
]
```

### <a name="aggregate-scalar-subqueries"></a>Subconsultas escalares agregadas

Una subconsulta escalar agregada es una subconsulta que tiene una función de agregado en su proyección o el filtro que se evalúa como un valor único.

**Ejemplo 1:**

Esta es una subconsulta con una expresión de función de agregado único en su proyección:

```sql
SELECT TOP 5 
    f.id, 
    (SELECT VALUE Count(1) FROM n IN f.nutrients WHERE n.units = 'mg'
) AS count_mg
FROM food f
```

Salida de la consulta:

```json
[
  { "id": "03230", "count_mg": 13 },
  { "id": "03238", "count_mg": 14 },
  { "id": "03229", "count_mg": 13 },
  { "id": "03226", "count_mg": 15 },
  { "id": "03227", "count_mg": 19 }
]
```

**Ejemplo 2**

Esta es una subconsulta con varias expresiones de función de agregado:

```sql
SELECT TOP 5 f.id, (
    SELECT Count(1) AS count, Sum(n.nutritionValue) AS sum 
    FROM n IN f.nutrients 
    WHERE n.units = 'mg'
) AS unit_mg
FROM food f
```

Salida de la consulta:

```json
[
  { "id": "03230","unit_mg": { "count": 13,"sum": 147.072 } },
  { "id": "03238","unit_mg": { "count": 14,"sum": 107.385 } },
  { "id": "03229","unit_mg": { "count": 13,"sum": 141.579 } },
  { "id": "03226","unit_mg": { "count": 15,"sum": 183.91399999999996 } },
  { "id": "03227","unit_mg": { "count": 19,"sum": 94.788999999999987 } }
]
```

**Ejemplo 3**

Esta es una consulta con una subconsulta en la proyección y el filtro agregada:

```sql
SELECT TOP 5 
    f.id, 
    (SELECT VALUE Count(1) FROM n IN f.nutrients WHERE n.units = 'mg') AS count_mg
FROM food f
WHERE (SELECT VALUE Count(1) FROM n IN f.nutrients WHERE n.units = 'mg') > 20
```

Salida de la consulta:

```json
[
  { "id": "03235", "count_mg": 27 },
  { "id": "03246", "count_mg": 21 },
  { "id": "03267", "count_mg": 21 },
  { "id": "03269", "count_mg": 21 },
  { "id": "03274", "count_mg": 21 }
]
```

Una manera más óptima de escribir esta consulta es combinar en la subconsulta y hacer referencia a la subconsulta alias en ambos SELECT y cláusulas WHERE. Esta consulta es más eficaz porque la necesitará ejecutar la subconsulta solo dentro de la instrucción de combinación y no en la proyección y el filtro.

```sql
SELECT TOP 5 f.id, count_mg
FROM food f
JOIN (SELECT VALUE Count(1) FROM n IN f.nutrients WHERE n.units = 'mg') AS count_mg
WHERE count_mg > 20
```

#### <a name="exists-expression"></a>Expresión EXISTS

Azure Cosmos DB admite expresiones EXISTS. Se trata de una subconsulta escalar agregada integrada en Azure Cosmos DB SQL API. EXISTS es una expresión booleana que toma una expresión de subconsulta y devuelve true si la subconsulta devuelve todas las filas. En caso contrario, devuelve false.

Dado que la API de SQL de Azure Cosmos DB no distingue entre expresiones booleanas y otras expresiones escalares, puede utilizar EXISTS en, seleccione ambos y cláusulas WHERE. Esto es diferente a Transact-SQL, donde está restringida al filtro de una expresión booleana (por ejemplo, EXISTS, BETWEEN e IN).

Si la subconsulta EXISTS devuelve un valor único que tiene un indefinido, existe se evaluará como false. Por ejemplo, considere la siguiente consulta se evalúa como false:
```sql
SELECT EXISTS (SELECT VALUE undefined)
```   


Si se omite la palabra clave VALUE en la subconsulta anterior, la consulta se evaluará como true:
```sql
SELECT EXISTS (SELECT undefined) 
```

La subconsulta enmarcará la lista de valores en la lista seleccionada en un objeto. Si la lista seleccionada no tiene valores, la subconsulta devolverá el valor único '{}'. Este valor se define, EXISTS se evalúa como true.

#### <a name="example-rewriting-arraycontains-and-join-as-exists"></a>Ejemplo: Reescritura ARRAY_CONTAINS y combinación EXISTS

Es un caso de uso común de ARRAY_CONTAINS filtrar un documento por la existencia de un elemento en una matriz. En este caso, estamos comprobando para ver si la matriz de etiquetas contiene un elemento denominado "anaranjado".

```sql
SELECT TOP 5 f.id, f.tags
FROM food f
WHERE ARRAY_CONTAINS(f.tags, {name: 'orange'})
```

Puede volver a escribir la misma consulta para utilizar EXISTS:

```sql
SELECT TOP 5 f.id, f.tags
FROM food f
WHERE EXISTS(SELECT VALUE t FROM t IN f.tags WHERE t.name = 'orange')
```

Además, ARRAY_CONTAINS sólo puede comprobar si un valor es igual a cualquier elemento dentro de una matriz. Si necesita filtros más complejos en las propiedades de la matriz, utilice la combinación.

Considere la siguiente consulta que filtra según las unidades y `nutritionValue` propiedades de la matriz: 

```sql
SELECT VALUE c.description
FROM c
JOIN n IN c.nutrients
WHERE n.units= "mg" AND n.nutritionValue > 0
```

Para cada uno de los documentos de la colección, un producto cruzado se realiza con sus elementos de matriz. Esta operación de combinación permite filtrar las propiedades dentro de la matriz. Sin embargo, el consumo de RU de la consulta será alta. Por ejemplo, si 1.000 documentos tenían 100 elementos en cada matriz, se expandirá a 1.000 x 100 (es decir, 100 000) tuplas.

Utilizar EXISTS puede ayudar a evitar costosa este producto cruzado:

```sql
SELECT VALUE c.description
FROM c
WHERE EXISTS(
    SELECT VALUE n
    FROM n IN c.nutrients
    WHERE n.units = "mg" AND n.nutritionValue > 0
)
```

En este caso, puede filtrar elementos de la matriz dentro de la subconsulta EXISTS. Si un elemento de matriz coincide con el filtro, a continuación, proyecte y EXISTS se evalúa como true.

También puede alias EXISTS y hacer referencia a él en la proyección:

```sql
SELECT TOP 1 c.description, EXISTS(
    SELECT VALUE n
    FROM n IN c.nutrients
    WHERE n.units = "mg" AND n.nutritionValue > 0) as a
FROM c
```

Salida de la consulta:

```json
[
    {
        "description": "Babyfood, dessert, fruit pudding, orange, strained",
        "a": true
    }
]
```

#### <a name="array-expression"></a>Expresión de matriz

Puede usar la expresión de matriz para proyectar los resultados de una consulta como una matriz. Puede utilizar esta expresión solo dentro de la cláusula SELECT de la consulta.

```sql
SELECT TOP 1   f.id, ARRAY(SELECT VALUE t.name FROM t in f.tags) AS tagNames
FROM  food f
```

Salida de la consulta:

```json
[
    {
        "id": "03238",
        "tagNames": [
            "babyfood",
            "dessert",
            "tropical fruit",
            "junior"
        ]
    }
]
```

Al igual que con las demás subconsultas, son posibles los filtros con la expresión de matriz.

```sql
SELECT TOP 1 c.id, ARRAY(SELECT VALUE t FROM t in c.tags WHERE t.name != 'infant formula') AS tagNames
FROM c
```

Salida de la consulta:

```json
[
    {
        "id": "03226",
        "tagNames": [
            {
                "name": "babyfood"
            },
            {
                "name": "dessert"
            },
            {
                "name": "fruit pudding"
            },
            {
                "name": "orange"
            },
            {
                "name": "strained"
            }
        ]
    }
]
```

Las expresiones de la matriz también pueden proceder después de la cláusula FROM en subconsultas.

```sql
SELECT TOP 1 c.id, ARRAY(SELECT VALUE t.name FROM t in c.tags) as tagNames
FROM c
JOIN n IN (SELECT VALUE ARRAY(SELECT t FROM t in c.tags WHERE t.name != 'infant formula'))
```

Salida de la consulta:

```json
[
    {
        "id": "03238",
        "tagNames": [
            "babyfood",
            "dessert",
            "tropical fruit",
            "junior"
        ]
    }
]
```

## <a name="next-steps"></a>Pasos siguientes

- [Ejemplos de consultas SQL](how-to-sql-query.md)
- [Ejemplos de .NET de Azure Cosmos DB](https://github.com/Azure/azure-cosmosdb-dotnet)
- [Datos del documento de modelo](modeling-data.md)
