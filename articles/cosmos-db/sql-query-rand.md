---
title: RAND en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información acerca de la función del sistema de SQL RAND en Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/16/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 4a0672000e630c9e06df84d9c2da5cb8b988c05a
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71349602"
---
# <a name="rand-azure-cosmos-db"></a>RAND (Azure Cosmos DB)
 Devuelve un valor numérico generado de forma aleatoria a partir de [0,1).
 
## <a name="syntax"></a>Sintaxis
  
```sql
RAND ()  
```  

## <a name="return-types"></a>Tipos de valor devuelto

  Devuelve una expresión numérica.

## <a name="remarks"></a>Comentarios

  `RAND` es una función no determinista. Las llamadas repetitivas de `RAND` no devuelven los mismos resultados.

## <a name="examples"></a>Ejemplos
  
  En el ejemplo siguiente, se devuelve un valor numérico generado de forma aleatoria.
  
```sql
SELECT RAND() AS rand 
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[{"rand": 0.87860053195618093}]  
``` 

## <a name="next-steps"></a>Pasos siguientes

- [Funciones matemáticas (Azure Cosmos DB)](sql-query-mathematical-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
