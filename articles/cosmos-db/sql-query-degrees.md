---
title: DEGREES en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información acerca de la función del sistema de SQL DEGREES en Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: d8c2109c2c11f137b1966741a95d1d0c02895016
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71351217"
---
# <a name="degrees-azure-cosmos-db"></a>DEGREES (Azure Cosmos DB)
 Devuelve el ángulo correspondiente en grados de un ángulo especificado en radianes.  
  
## <a name="syntax"></a>Sintaxis
  
```sql
DEGREES (<numeric_expr>)  
```  
  
## <a name="arguments"></a>Argumentos
  
*numeric_expr*  
   Es una expresión numérica.  
  
## <a name="return-types"></a>Tipos de valor devuelto
  
  Devuelve una expresión numérica.  
  
## <a name="examples"></a>Ejemplos
  
  En el ejemplo siguiente se devuelven los grados de un ángulo de pi/2 radianes.  
  
```sql
SELECT DEGREES(PI()/2) AS degrees  
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[{"degrees": 90}]  
```  

## <a name="next-steps"></a>Pasos siguientes

- [Funciones matemáticas (Azure Cosmos DB)](sql-query-mathematical-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
