---
title: LOWER en el lenguaje de consulta de Azure Cosmos DB
description: Más información sobre la función del sistema SQL LOWER en Azure Cosmos DB para devolver una expresión de cadena después de convertir los datos de caracteres en mayúsculas a minúsculas.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 80dba57d4fe05630eb5ae4f8fc96bd0aa214c6d4
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2020
ms.locfileid: "78302277"
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

## <a name="remarks"></a>Observaciones

Esta función del sistema no usará el índice.

## <a name="next-steps"></a>Pasos siguientes

- [Funciones de cadena Azure Cosmos DB](sql-query-string-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
