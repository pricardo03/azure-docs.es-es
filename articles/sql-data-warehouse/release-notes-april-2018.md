---
title: Notas de la versión de Azure SQL Data Warehouse de abril de 2018 | Microsoft Docs
description: Notas de la versión de Azure SQL Data Warehouse.
services: sql-data-warehouse
author: twounder
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 05/28/2018
ms.author: twounder
ms.reviewer: twounder
ms.openlocfilehash: 49ffc3ddfd586964ae8a9688aeb48fffdd327b45
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2018
ms.locfileid: "34738961"
---
# <a name="whats-new-in-azure-sql-data-warehouse-april-2018"></a>¿Cuáles son las novedades de Azure SQL Data Warehouse (abril de 2018)?
Continuamente se aplican mejoras a Azure SQL Data Warehouse. En este artículo se describen las nuevas características y los cambios que se han incluido en abril de 2018.

## <a name="features"></a>Características

### <a name="ability-to-truncate-a-partition-before-a-switch"></a>Posibilidad de truncar una partición antes de una modificación
Los clientes utilizan a menudo la modificación de particiones como un patrón para cargar datos de una tabla a otra cambiando los metadatos de la tabla a través de la sintaxis `ALTER TABLE SourceTable SWITCH PARTITION X TO TargetTable PARTITION X`. SQL Data Warehouse no admite la modificación de particiones cuando la partición de destino contiene datos. Si la partición de destino ya contiene datos, el cliente necesitaría truncar la partición de destino y luego realizar la modificación.

SQL Data Warehouse ahora admite esta operación en una sola instrucción T-SQL.

```sql
ALTER TABLE SourceTable 
    SWITCH PARTITION X TO TargetTable PARTITION X
    WITH (TRUNCATE_TARGET_PARTITION = ON)
```
Para más información, consulte el artículo [ALTER TABLE](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql).

### <a name="improved-query-compilation-performance"></a>Rendimiento mejorado de la compilación de consultas
SQL Data Warehouse introdujo un conjunto de cambios para mejorar el paso de compilación de consultas para las consultas distribuidas. Estos cambios mejoran los tiempos de compilación de consultas hasta **10 veces**, reduciendo los tiempos de ejecución de consultas en general. Estos cambios son más evidentes en almacenamientos de datos con un gran número de objetos (tablas, funciones, vistas y procedimientos).

## <a name="behavior-changes"></a>Cambios de comportamiento

### <a name="dbcc-commands-do-not-consume-concurrency-slots"></a>Los comandos DBCC no consumen espacios de simultaneidad
SQL Data Warehouse admite un subconjunto de [comandos DBCC](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-transact-sql) T-SQL como [DBCC DROPCLEANBUFFERS](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-dropcleanbuffers-transact-sql). Anteriormente, estos comandos consumían un [espacio de simultaneidad](https://docs.microsoft.com/azure/sql-data-warehouse/resource-classes-for-workload-management#concurrency-slots) reduciendo el número de cargas y consultas de usuario que podían ejecutarse. Los comandos `DBCC` ahora se ejecutan en una cola local que no consumen un espacio de recursos, lo que mejora el rendimiento general de la ejecución de consultas.

### <a name="updated-error-message-for-excessive-literals"></a>Mensaje de error actualizado para literales excesivos
Anteriormente, SQL Data Warehouse incluía un recuento *aproximado* cuando una consulta contenía demasiados literales.
```
Msg 100086
Cannot have more than 20,000 literals in the query. The query contains [n] literals.
```

El mensaje de error se actualizó para indicar solamente que se ha alcanzado el límite de literales.
```
Msg 100086
The number of literals in the query is beyond the limit. Please rewrite your query.
```

Para más información, consulte la sección [Consultas](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-service-capacity-limits#queries) del artículo [Límites de capacidad](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-service-capacity-limits) para más detalles sobre los límites máximos.

### <a name="removed-the-syspdwdatabasemappings-view"></a>Se quitó la vista SYS.PDW_DATABASE_MAPPINGS
Esta vista `sys.pdw_database_mappings` no se utiliza en SQL Data Warehouse. Anteriormente, una instrucción SELECT de esta vista no devolvía ningún resultado. Se ha quitado la vista. 