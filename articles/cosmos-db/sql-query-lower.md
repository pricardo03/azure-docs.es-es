---
title: LOWER en el lenguaje de consulta de Azure Cosmos DB
description: Más información sobre la función del sistema SQL LOWER en Azure Cosmos DB para devolver una expresión de cadena después de convertir los datos de caracteres en mayúsculas a minúsculas.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 8f7cd864d90b8ab17a180a00a7a6d4bb683da9be
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2019
ms.locfileid: "74873308"
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
