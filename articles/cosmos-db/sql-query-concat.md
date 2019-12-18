---
title: CONCAT en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información sobre cómo la función del sistema SQL CONCAT en Azure Cosmos DB devuelve una cadena que es el resultado de concatenar dos o más valores de cadena
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: adb8564d4addaaa3f4a383b32941549aed3d53bd
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2019
ms.locfileid: "74871557"
---
# <a name="concat-azure-cosmos-db"></a>CONCAT (Azure Cosmos DB)
 Devuelve una cadena que es el resultado de concatenar dos o más valores de cadena.  
  
## <a name="syntax"></a>Sintaxis
  
```sql
CONCAT(<str_expr1>, <str_expr2> [, <str_exprN>])  
```  
  
## <a name="arguments"></a>Argumentos
  
*str_expr*  
   Es una expresión de cadena que se va a concatenar a los otros valores. La función `CONCAT` requiere al menos dos argumentos *str_expr*.  
  
## <a name="return-types"></a>Tipos de valor devuelto
  
  Devuelve una expresión de cadena.  
  
## <a name="examples"></a>Ejemplos
  
  El siguiente ejemplo devuelve la cadena concatenada de los valores especificados.  
  
```sql
SELECT CONCAT("abc", "def") AS concat  
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[{"concat": "abcdef"}]  
```  
  

## <a name="next-steps"></a>Pasos siguientes

- [Funciones de cadena Azure Cosmos DB](sql-query-string-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
