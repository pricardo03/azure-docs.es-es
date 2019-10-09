---
title: Funciones matemáticas en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información acerca de las funciones matemáticas del sistema de SQL en Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: fe4a84db3f2cbcfc2d9841caf520404afec2a297
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71349678"
---
# <a name="mathematical-functions-azure-cosmos-db"></a>Funciones matemáticas (Azure Cosmos DB)  

Las funciones matemáticas realizan un cálculo, basado en valores de entrada proporcionados como argumentos, y devuelven un valor numérico.

Puede ejecutar consultas similares a las del ejemplo siguiente:

```sql
    SELECT VALUE ABS(-4)
```

El resultado es el siguiente:

```json
    [4]
```

## <a name="functions"></a>Functions

Las siguientes funciones matemáticas integradas y compatibles realizan un cálculo, basado normalmente en valores de entrada proporcionados como argumentos, y devuelven un valor numérico.
  
||||  
|-|-|-|  
|[ABS](sql-query-abs.md)|[ACOS](sql-query-acos.md)|[ASIN](sql-query-asin.md)|  
|[ATAN](sql-query-atan.md)|[ATN2](sql-query-atn2.md)|[CEILING](sql-query-ceiling.md)|  
|[COS](sql-query-cos.md)|[COT](sql-query-cot.md)|[DEGREES](sql-query-degrees.md)|  
|[EXP](sql-query-exp.md)|[FLOOR](sql-query-floor.md)|[LOG](sql-query-log.md)|  
|[LOG10](sql-query-log10.md)|[PI](sql-query-pi.md)|[POWER](sql-query-power.md)|  
|[RADIANS](sql-query-radians.md)|[RAND](sql-query-rand.md)|[ROUND](sql-query-round.md)|
|[SIGN](sql-query-sign.md)|[SIN](sql-query-sin.md)|[SQRT](sql-query-sqrt.md)|
|[SQUARE](sql-query-square.md)|[TAN](sql-query-tan.md)|[TRUNC](sql-query-trunc.md)||  
  
Todas las funciones matemáticas, excepto RAND, son funciones deterministas. Esto significa que devuelven los mismos resultados cada vez que se llaman con un conjunto específico de valores de entrada.

## <a name="next-steps"></a>Pasos siguientes

- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
- [Funciones definidas por el usuario](sql-query-udfs.md)
- [Aggregates](sql-query-aggregates.md) (Agregados)
