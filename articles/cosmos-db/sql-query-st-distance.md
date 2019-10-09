---
title: ST_DISTANCE en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información acerca de la función del sistema de SQL ST_DISTANCE en Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 1c55bac14b3379f29d57bbad36026749089ec0fd
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71349397"
---
# <a name="st_distance-azure-cosmos-db"></a>ST_DISTANCE (Azure Cosmos DB)
 Devuelve la distancia entre dos expresiones Point, Polygon o LineString de GeoJSON.  
  
## <a name="syntax"></a>Sintaxis
  
```sql
ST_DISTANCE (<spatial_expr>, <spatial_expr>)  
```  
  
## <a name="arguments"></a>Argumentos
  
*spatial_expr*  
   Es cualquier expresión válida de objeto de tipo Point, Polygon o LineString de GeoJSON.  
  
## <a name="return-types"></a>Tipos de valor devuelto
  
  Devuelve una expresión numérica con la distancia, que se expresa en metros para el sistema de referencia predeterminado.  
  
## <a name="examples"></a>Ejemplos
  
  En el siguiente ejemplo se muestra cómo se devuelven todos los documentos de la familia en un radio de 30 km a partir de la ubicación especificada mediante la función integrada `ST_DISTANCE`. .  
  
```sql
SELECT f.id   
FROM Families f   
WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000  
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[{  
  "id": "WakefieldFamily"  
}]  
```  

## <a name="next-steps"></a>Pasos siguientes

- [Funciones espaciales (Azure Cosmos DB)](sql-query-spatial-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
