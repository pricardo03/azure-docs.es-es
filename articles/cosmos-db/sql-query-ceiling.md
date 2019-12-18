---
title: CEILING en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información sobre cómo la función del sistema SQL CEILING en Azure Cosmos DB devuelve el valor principal del arco tangente de y/x, expresado en radianes.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 18462b152b698e7c6bc4c6c1e59511b4bacaa611
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2019
ms.locfileid: "74873444"
---
# <a name="ceiling-azure-cosmos-db"></a>CEILING (Azure Cosmos DB)
 Devuelve el valor entero más pequeño mayor o igual que la expresión numérica especificada.  
  
## <a name="syntax"></a>Sintaxis
  
```sql
CEILING (<numeric_expr>)  
```  
  
## <a name="arguments"></a>Argumentos
  
*numeric_expr*  
   Es una expresión numérica.  
  
## <a name="return-types"></a>Tipos de valor devuelto
  
  Devuelve una expresión numérica.  
  
## <a name="examples"></a>Ejemplos
  
  En el ejemplo siguiente se muestran valores numéricos positivos, negativos y cero con la función `CEILING`.  
  
```sql
SELECT CEILING(123.45) AS c1, CEILING(-123.45) AS c2, CEILING(0.0) AS c3  
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[{c1: 124, c2: -123, c3: 0}]  
```  

## <a name="next-steps"></a>Pasos siguientes

- [Funciones matemáticas (Azure Cosmos DB)](sql-query-mathematical-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
