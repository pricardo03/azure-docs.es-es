---
title: DEGREES en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información sobre la función del sistema SQL DEGREES en Azure Cosmos DB que devuelve el ángulo correspondiente en grados para un ángulo especificado en radianes
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 5f432f5ef57c8eccc0cf629f00d2231364785b9a
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2019
ms.locfileid: "74871336"
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
