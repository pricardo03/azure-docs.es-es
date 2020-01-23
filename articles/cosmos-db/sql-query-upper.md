---
title: UPPER en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información sobre la función del sistema SQL UPPER en Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 9809a138a5e28eb069d545d39cfda815c915bd78
ms.sourcegitcommit: c32050b936e0ac9db136b05d4d696e92fefdf068
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2020
ms.locfileid: "75728895"
---
# <a name="upper-azure-cosmos-db"></a>UPPER (Azure Cosmos DB)
 Devuelve una expresión de cadena después de convertir datos de caracteres en minúsculas a mayúsculas.  

La función del sistema UPPER no emplea el índice. Si planea realizar con frecuencia comparaciones que no distingan mayúsculas de minúsculas, la función del sistema UPPER puede consumir una cantidad significativa de RU. Si este es el caso, en lugar de usar la función del sistema UPPER para normalizar los datos cada vez que realice comparaciones, puede normalizar el uso de mayúsculas y minúsculas durante la inserción. Al hacerlo, una consulta como SELECT * FROM c WHERE UPPER(c.name) = 'BOB' simplemente se convertirá en SELECT * FROM c WHERE c.name = 'BOB'.

## <a name="syntax"></a>Sintaxis
  
```sql
UPPER(<str_expr>)  
```  
  
## <a name="arguments"></a>Argumentos
  
*str_expr*  
   Es una expresión de cadena.  
  
## <a name="return-types"></a>Tipos de valores devueltos
  
  Devuelve una expresión de cadena.  
  
## <a name="examples"></a>Ejemplos
  
  En el ejemplo siguiente se muestra cómo usar `UPPER` en una consulta  
  
```sql
SELECT UPPER("Abc") AS upper  
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[{"upper": "ABC"}]  
```

## <a name="next-steps"></a>Pasos siguientes

- [Funciones de cadena Azure Cosmos DB](sql-query-string-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
