---
title: LOWER en el lenguaje de consulta de Azure Cosmos DB
description: Más información sobre la función del sistema SQL LOWER en Azure Cosmos DB para devolver una expresión de cadena después de convertir los datos de caracteres en mayúsculas a minúsculas.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 35efbb8d4d97ab52abb20487d15a80985946c499
ms.sourcegitcommit: c32050b936e0ac9db136b05d4d696e92fefdf068
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2020
ms.locfileid: "75732610"
---
# <a name="lower-azure-cosmos-db"></a>LOWER (Azure Cosmos DB)
 Devuelve una expresión de cadena después de convertir los datos de caracteres en mayúsculas a minúsculas.  

La función del sistema LOWER no emplea el índice. Si planea realizar con frecuencia comparaciones que no distingan mayúsculas de minúsculas, la función del sistema LOWER puede consumir una cantidad significativa de RU. Si este es el caso, en lugar de usar la función del sistema LOWER para normalizar los datos cada vez que realice comparaciones, puede normalizar el uso de mayúsculas y minúsculas durante la inserción. Al hacerlo, una consulta como SELECT * FROM c WHERE LOWER(c.name) = 'bob' simply se convertirá en SELECT * FROM c WHERE c.name = 'bob'.

## <a name="syntax"></a>Sintaxis
  
```sql
LOWER(<str_expr>)  
```  
  
## <a name="arguments"></a>Argumentos
  
*str_expr*  
   Es una expresión de cadena.  
  
## <a name="return-types"></a>Tipos de valores devueltos
  
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
