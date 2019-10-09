---
title: ST_INTERSECTS en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información sobre la función del sistema de SQL ST_INTERSECTS en Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 549c6b69e9112a491060478e859338c14e977612
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71349389"
---
# <a name="st_intersects-azure-cosmos-db"></a>ST_INTERSECTS (Azure Cosmos DB)
 Devuelve una expresión condicional que indica si el objeto de GeoJSON (Point, Polygon o LineString) especificado en el primer argumento forma intersección con el objeto de GeoJSON (Point, Polygon o LineString) del segundo.  
  
## <a name="syntax"></a>Sintaxis
  
```sql
ST_INTERSECTS (<spatial_expr>, <spatial_expr>)  
```  
  
## <a name="arguments"></a>Argumentos
  
*spatial_expr*  
   Es cualquier expresión de objeto de tipo Point, Polygon o LineString de GeoJSON.  
  
## <a name="return-types"></a>Tipos de valor devuelto
  
  Devuelve un valor booleano.  
  
## <a name="examples"></a>Ejemplos
  
  En el siguiente ejemplo se muestra cómo buscar todas las áreas de intersección con el polígono indicado.  
  
```sql
SELECT a.id
FROM Areas a
WHERE ST_INTERSECTS(a.location, {  
    'type':'Polygon',
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[{ "id": "IntersectingPolygon" }]  
```  

## <a name="next-steps"></a>Pasos siguientes

- [Funciones espaciales (Azure Cosmos DB)](sql-query-spatial-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
