---
title: LOG10 en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información sobre la función del sistema SQL LOG10 en Azure Cosmos DB para devolver el logaritmo en base 10 de la expresión numérica especificada.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 5950a2a40a78716a76e49fcdaeb3129948b6a0e1
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2019
ms.locfileid: "74871064"
---
# <a name="log10-azure-cosmos-db"></a>LOG10 (Azure Cosmos DB)
 Devuelve el logaritmo de base 10 de la expresión numérica especificada.  
  
## <a name="syntax"></a>Sintaxis
  
```sql
LOG10 (<numeric_expr>)  
```  
  
## <a name="arguments"></a>Argumentos
  
*numeric_expression*  
   Es una expresión numérica.  
  
## <a name="return-types"></a>Tipos de valor devuelto
  
  Devuelve una expresión numérica.  
  
## <a name="remarks"></a>Comentarios
  
  Las funciones LOG10 y POWER tienen una relación inversa. Por ejemplo, 10 ^ LOG10 (n) = n.  
  
## <a name="examples"></a>Ejemplos
  
  En el ejemplo siguiente se declara una variable y se devuelve el valor de LOG10 de la variable especificada (100).  
  
```sql
SELECT LOG10(100) AS log10 
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[{log10: 2}]  
```  

## <a name="next-steps"></a>Pasos siguientes

- [Funciones matemáticas (Azure Cosmos DB)](sql-query-mathematical-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
