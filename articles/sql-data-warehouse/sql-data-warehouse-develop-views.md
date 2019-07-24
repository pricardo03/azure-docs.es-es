---
title: Uso de vistas T-SQL en Azure SQL Data Warehouse | Microsoft Docs
description: Sugerencias para usar las vistas T-SQL en Azure SQL Data Warehouse para desarrollar soluciones.
services: sql-data-warehouse
author: XiaoyuL-Preview
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: e8d516cfd764f947bd2fe7fc25f6394c313c0d9a
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67595499"
---
# <a name="views-in-azure-sql-data-warehouse"></a>Vistas en Azure SQL Data Warehouse
Sugerencias para usar las vistas T-SQL en Azure SQL Data Warehouse para desarrollar soluciones. 

## <a name="why-use-views"></a>¿Por qué usar vistas?
Las vista se pueden usar de formas diferentes para mejorar la calidad de la solución.  Este artículo resalta algunos ejemplos de cómo enriquecer su solución con vistas, así como las limitaciones que se deben tener en cuenta.


> [!IMPORTANT]
> Vea la nueva sintaxis de la vista materializada en la instrucción [CREATE MATERIALIZED VIEW AS SELECT](/sql/t-sql/statements/create-materialized-view-as-select-transact-sql?view=azure-sqldw-latest).  Para más información, consulte las [notas de la versión](/azure/sql-data-warehouse/release-notes-10-0-10106-0).
>


> [!NOTE]
> En este artículo no se explica la sintaxis de CREATE VIEW. Para más información, consulte la documentación de [CREATE VIEW](/sql/t-sql/statements/create-view-transact-sql).
> 

## <a name="architectural-abstraction"></a>Abstracción de arquitectura

Se trata de un patrón de aplicación común para volver a crear tablas con la característica CREATE TABLE AS SELECT (CTAS) seguida de un patrón de cambio de nombre de objetos mientras se cargan los datos.

En el ejemplo siguiente se agregan registros de fecha nuevos a una dimensión de fecha. Observe cómo una nueva tabla, DimDate_New, se crea por primera vez y luego cambia de nombre para reemplazar la versión original de la tabla.

```sql
CREATE TABLE dbo.DimDate_New
WITH (DISTRIBUTION = ROUND_ROBIN
, CLUSTERED INDEX (DateKey ASC)
)
AS
SELECT *
FROM   dbo.DimDate  AS prod
UNION ALL
SELECT *
FROM   dbo.DimDate_stg AS stg
;

RENAME OBJECT DimDate TO DimDate_Old;
RENAME OBJECT DimDate_New TO DimDate;

```

Sin embargo, este enfoque puede provocar que las tablas aparezcan y desaparezcan de la vista del usuario, así como mensajes de error del tipo "la tabla no existe". Las vistas pueden utilizarse para proporcionar una capa de presentación coherente mientras se cambia el nombre de los objetos subyacentes. Al proporcionar acceso a los datos a través de vistas, los usuarios no necesitan visibilidad para las tablas subyacentes. Esta capa proporciona una experiencia de usuario coherente asegurándose de que los diseñadores del almacenamiento de datos puedan hacer evolucionar el modelo de datos. Hacer evolucionar las tablas subyacentes significa que los diseñadores pueden usar CTAS para maximizar el rendimiento durante el proceso de carga de datos.   

## <a name="performance-optimization"></a>Optimización del rendimiento
Las vistas se pueden usar también para aplicar combinaciones de rendimiento optimizado entre tablas. Por ejemplo, una vista puede incorporar una clave de distribución redundante como parte de los criterios de combinación para minimizar el movimiento de datos. Otra ventaja de una vista podría ser forzar una consulta específica o una sugerencia de combinación. La utilización de vistas de esta manera garantiza que las combinaciones siempre se realizarán de manera óptima, evitando la necesidad de los usuarios de recordar la construcción correcta de sus combinaciones.

## <a name="limitations"></a>Limitaciones
Las vistas en SQL Data Warehouse se almacenan solo como metadatos. Por lo tanto, no están disponibles las siguientes opciones:

* No hay ninguna opción de enlace de esquema.
* Las tablas base no se puede actualizar a través de la vista.
* No se pueden crear vistas en tablas temporales.
* No hay compatibilidad con las sugerencias EXPAND y NOEXPAND
* No hay ninguna vista indexada en SQL Data Warehouse.

## <a name="next-steps"></a>Pasos siguientes
Para obtener más sugerencias sobre desarrollo, consulte la [información general sobre desarrollo de SQL Data Warehouse](sql-data-warehouse-overview-develop.md).


