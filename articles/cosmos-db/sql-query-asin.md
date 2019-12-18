---
title: ASIN en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información sobre cómo la función del sistema SQL Arcsine (ASIN) en Azure Cosmos DB devuelve el ángulo, en radianes, cuyo seno es la expresión numérica especificada
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 3e790eb5ae5eb780637b199a1a65dec2dd02d1bc
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2019
ms.locfileid: "74871745"
---
# <a name="asin-azure-cosmos-db"></a>ASIN (Azure Cosmos DB)
 Devuelve el ángulo, en radianes, cuyo seno es la expresión numérica especificada. También se denomina arcoseno.  
  
## <a name="syntax"></a>Sintaxis
  
```sql
ASIN(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Argumentos
  
*numeric_expr*  
   Es una expresión numérica.  
  
## <a name="return-types"></a>Tipos de valor devuelto
  
  Devuelve una expresión numérica.  
  
## <a name="examples"></a>Ejemplos
  
  El siguiente ejemplo devuelve el `ASIN` de -1.  
  
```sql
SELECT ASIN(-1) AS asin  
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[{"asin": -1.5707963267948966}]  
```  

## <a name="next-steps"></a>Pasos siguientes

- [Funciones matemáticas (Azure Cosmos DB)](sql-query-mathematical-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
