---
title: Funciones de agregado en Azure Cosmos DB
description: Obtenga información sobre la sintaxis de las funciones de agregado de SQL para Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/31/2019
ms.author: mjbrown
ms.openlocfilehash: a6937e9e811ea8e44eda6f2bcb5d2c7d78db4934
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/24/2019
ms.locfileid: "67343150"
---
# <a name="aggregate-functions-in-azure-cosmos-db"></a>Funciones de agregado en Azure Cosmos DB

Las funciones de agregado realizan un cálculo en un conjunto de valores en la cláusula SELECT y devuelven un valor único. Por ejemplo, la consulta siguiente devuelve el número de elementos dentro del contenedor `Families`:

## <a name="examples"></a>Ejemplos

```sql
    SELECT COUNT(1)
    FROM Families f
```

Los resultados son:

```json
    [{
        "$1": 2
    }]
```

También puede devolver solo el valor escalar del agregado mediante la palabra clave VALUE. Por ejemplo, la siguiente consulta devuelve el número de valores como un único número:

```sql
    SELECT VALUE COUNT(1)
    FROM Families f
```

Los resultados son:

```json
    [ 2 ]
```

También puede combinar las agregaciones con filtros. Por ejemplo, la consulta siguiente devuelve el número de elementos cuyo estado de dirección es `WA`.

```sql
    SELECT VALUE COUNT(1)
    FROM Families f
    WHERE f.address.state = "WA"
```

Los resultados son:

```json
    [ 1 ]
```

## <a name="types-of-aggregate-functions"></a>Tipos de funciones de agregado

API de SQL admite las siguientes funciones de agregado. SUM y AVG operan en valores numéricos, y COUNT, MIN y MAX trabajan con números, cadenas, valores booleanos y null.

| Función | DESCRIPCIÓN |
|-------|-------------|
| COUNT | Devuelve el número de elementos de la expresión. |
| SUM   | Devuelve la suma de todos los valores de la expresión. |
| MÍN   | Devuelve el valor mínimo de la expresión. |
| MÁX   | Devuelve el valor máximo de la expresión. |
| MEDIA   | Devuelve la media de los valores de la expresión. |

También puede aplicar agregaciones a los resultados de la iteración de una matriz.

> [!NOTE]
> En el Explorador de datos de Azure Portal, las consultas de agregación pueden agregar resultados parciales a lo largo de solo una página de consulta. El SDK genera un único valor acumulado en todas las páginas. Para realizar consultas de agregación mediante código, necesita el SDK de .NET 1.12.0, el SDK de .NET Core 1.1.0 o el SDK de Java 1.9.5 o superior.

## <a name="next-steps"></a>Pasos siguientes

- [Introducción a Azure Cosmos DB](introduction.md)
- [System functions](sql-query-system-functions.md) (Funciones del sistema)
- [User defined functions](sql-query-udfs.md) (Funciones definidas por el usuario)