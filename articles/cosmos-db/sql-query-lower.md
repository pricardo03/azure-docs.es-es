---
title: LOWER en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información acerca de la función del sistema de SQL LOWER en Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 3a9c122ef65772458b832d3b1651e7e63e42985e
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71349706"
---
# <a name="lower-azure-cosmos-db"></a>LOWER (Azure Cosmos DB)
 Devuelve una expresión de cadena después de convertir los datos de caracteres en mayúsculas a minúsculas.  
  
## <a name="syntax"></a>Sintaxis
  
```sql
LOWER(<str_expr>)  
```  
  
## <a name="arguments"></a>Argumentos
  
*str_expr*  
   Es una expresión de cadena.  
  
## <a name="return-types"></a>Tipos de valor devuelto
  
  Devuelve una expresión de cadena.  
  
## <a name="examples"></a>Ejemplos
  
  En el ejemplo siguiente se muestra cómo usar `LOWER` en una consulta.  
  
```sql
SELECT LOWER("Abc") AS lower
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[{"lower": "abc"}]  
  
```  

## <a name="next-steps"></a>Pasos siguientes

- [Funciones de cadena Azure Cosmos DB](sql-query-string-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
