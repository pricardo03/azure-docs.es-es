---
title: StringToNull en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información acerca de la función del sistema de SQL StringToNull en Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: c7ea3ca82cadb1351d8581f3d652ce060d67969b
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71349246"
---
# <a name="stringtonull-azure-cosmos-db"></a>StringToNull (Azure Cosmos DB)
 Devuelve la expresión traducida a null. Si no se puede traducir la expresión, devuelve undefined.  
  
## <a name="syntax"></a>Sintaxis
  
```sql
StringToNull(<str_expr>)  
```  
  
## <a name="arguments"></a>Argumentos
  
*str_expr*  
   Expresión de cadena que se va a analizar como expresión null.
  
## <a name="return-types"></a>Tipos de valor devuelto
  
  Devuelve una expresión null o undefined.  
  
## <a name="examples"></a>Ejemplos
  
  En el ejemplo siguiente se muestra cómo se comporta `StringToNull` en tipos diferentes. 

Los siguientes son ejemplos con entradas válidas.

 Se admiten espacios en blanco únicamente antes o después de "null".

```sql
SELECT 
    StringToNull("null") AS n1, 
    StringToNull("  null ") AS n2,
    IS_NULL(StringToNull("null   ")) AS n3
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[{"n1": null, "n2": null, "n3": true}]
```  

Los siguientes son ejemplos con una entrada no válida.

Null distingue mayúsculas de minúsculas y debe escribirse con caracteres todos en minúsculas; es decir, "null".

```sql
SELECT    
    StringToNull("NULL"),
    StringToNull("Null")
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[{}]
```  

La expresión pasada se analizará como una expresión null; estas entradas no se evalúan para el tipo null y, por tanto, se devuelven undefined.

```sql
SELECT    
    StringToNull("true"), 
    StringToNull(false), 
    StringToNull(undefined),
    StringToNull(NaN) 
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[{}]
```  

## <a name="next-steps"></a>Pasos siguientes

- [Funciones de cadena Azure Cosmos DB](sql-query-string-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
