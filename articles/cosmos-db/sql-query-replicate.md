---
title: REPLICATE en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información acerca de la función del sistema de SQL REPLICATE en Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 5e0b7f29c503daa8a95dcc46238e60728c0cec50
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71349576"
---
# <a name="replicate-azure-cosmos-db"></a>REPLICATE (Azure Cosmos DB)
 Repite un valor de cadena un número especificado de veces.
  
## <a name="syntax"></a>Sintaxis
  
```sql
REPLICATE(<str_expr>, <num_expr>)
```  
  
## <a name="arguments"></a>Argumentos
  
*str_expr*  
   Es una expresión de cadena.
  
*num_expr*  
   Es una expresión numérica. Si el valor *num_expr* es negativo o no finito, el resultado es indefinido.
  
## <a name="return-types"></a>Tipos de valor devuelto
  
  Devuelve una expresión de cadena.
  
## <a name="remarks"></a>Comentarios
  La longitud máxima del resultado es de 10 000 caracteres, es decir, (length(*str_expr*)  *  *num_expr*) <= 10 000.

## <a name="examples"></a>Ejemplos
  
  En el ejemplo siguiente se muestra cómo usar `REPLICATE` en una consulta.
  
```sql
SELECT REPLICATE("a", 3) AS replicate
```  
  
 El conjunto de resultados es el siguiente:
  
```json
[{"replicate": "aaa"}]
```  

## <a name="next-steps"></a>Pasos siguientes

- [Funciones de cadena Azure Cosmos DB](sql-query-string-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
