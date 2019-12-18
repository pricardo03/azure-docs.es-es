---
title: ATAN en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información sobre cómo la función del sistema Arcotangente (ATAN) de SQL en Azure Cosmos DB devuelve el ángulo, en radianes, cuya tangente es la expresión numérica especificada.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 8fd398e343a3e617ceb9f49ca8f31ee776a7f073
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2019
ms.locfileid: "74873461"
---
# <a name="atan-azure-cosmos-db"></a>ATAN (Azure Cosmos DB)
 Devuelve el ángulo, en radianes, cuya tangente es la expresión numérica especificada. También se denomina arcotangente.  
  
## <a name="syntax"></a>Sintaxis
  
```sql
ATAN(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Argumentos
  
*numeric_expr*  
   Es una expresión numérica.  
  
## <a name="return-types"></a>Tipos de valor devuelto
  
  Devuelve una expresión numérica.  
  
## <a name="examples"></a>Ejemplos
  
  El siguiente ejemplo devuelve la `ATAN` del valor especificado.  
  
```sql
SELECT ATAN(-45.01) AS atan  
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[{"atan": -1.5485826962062663}]  
```  
  

## <a name="next-steps"></a>Pasos siguientes

- [Funciones matemáticas (Azure Cosmos DB)](sql-query-mathematical-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
