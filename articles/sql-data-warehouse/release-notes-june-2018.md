---
title: Notas de la versión de Azure SQL Data Warehouse de junio de 2018 | Microsoft Docs
description: Notas de la versión de Azure SQL Data Warehouse.
services: sql-data-warehouse
author: twounder
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 07/23/2018
ms.author: twounder
ms.reviewer: twounder
ms.openlocfilehash: 1db503ea40a9f07720aa9c130cd3bcc22f87f2af
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2018
ms.locfileid: "39324826"
---
# <a name="whats-new-in-azure-sql-data-warehouse-june-2018"></a>¿Cuáles son las novedades de Azure SQL Data Warehouse? Junio de 2018
Continuamente se aplican mejoras a Azure SQL Data Warehouse. En este artículo se describen las nuevas características y los cambios que se han incluido en junio de 2018. 

## <a name="user-defined-restore-points"></a>Puntos de restauración definidos por el usuario
SQL Data Warehouse toma automáticamente instantáneas del almacenamiento de datos cada ocho horas, lo que garantiza un objetivo de punto de recuperación (RPO) de ocho horas. Aunque esta toma automática de instantáneas facilita la carga de administración que supone la ejecución del almacenamiento de datos, es necesario tomar instantáneas en los momentos críticos según sus necesidades empresariales. Por ejemplo, puede tomar una instantánea justo antes de una carga significativa de datos o de la implementación de nuevos scripts en el almacenamiento de datos para habilitar un punto de restauración justo antes de la operación. 

SQL Data Warehouse ahora admite [puntos de restauración definidos por el usuario](https://azure.microsoft.com/blog/quick-recovery-time-with-sql-data-warehouse-using-user-defined-restore-points/) mediante el cmdlet [New AzureRmSqlDatabaseRestorePoint](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqldatabaserestorepoin).

```PowerShell
New-AzureRmSqlDatabaseRestorePoint
    -ResourceGroupName $ResourceGroupName
    -ServerName $ServerName
    -DatabaseName $DatabaseName
    -RestorePointLabel $RestorePointName
```

## <a name="column-level-security"></a>Seguridad a nivel de columnas
La administración del acceso a los datos y la seguridad en el almacenamiento de datos son fundamentales para generar confianza con los clientes y los asociados. SQL Data Warehouse [ahora es compatible con la seguridad de nivel de columna (CLS)](https://azure.microsoft.com/blog/column-level-security-is-now-supported-in-azure-sql-data-warehouse/) que le permite ajustar los permisos para ver datos confidenciales, limitando el acceso de usuario a columnas específicas de las tablas sin tener que volver a diseñar el almacenamiento de datos.

CLS permite controlar el acceso a columnas de la tabla en el contexto de ejecución del usuario o su pertenencia a grupos mediante la instrucción T-SQL estándar [GRANT](https://docs.microsoft.com/azure/sql-data-warehouse/column-level-security). La lógica de restricción de acceso está en el nivel de la base de datos, en lugar de encontrarse lejos de los datos en otra aplicación, lo que simplifica la implementación de seguridad global.


```sql
CREATE USER Nurse WITHOUT LOGIN;   
GRANT SELECT ON Membership(MemberID, FirstName, LastName, Phone, Email) TO Nurse;   
EXECUTE AS USER = 'Nurse';
SELECT * FROM Membership;

Msg 230, Level 14, State 1, Line 12 
The SELECT permission was denied on the column 'SSN' of the object 'Membership', database 'CLS_TestDW', schema 'dbo'.
```

## <a name="objectschemaname"></a>OBJECT_SCHEMA_NAME
La función [OBJECT_SCHEMA_NAME()]() devuelve el nombre de esquema de base de datos para los objetos de ámbito de esquema. Esta función se ha convertido en habitual en las herramientas ETL al hacer la validación del esquema de objetos. 

```sql
SELECT
    OBJECT_SCHEMA_NAME([object_id]) [table_schema]
    , [name]                        [table_name]
FROM
    [sys].[tables];
```

**Resultados de ejemplo**
```
table_schema    | table_name
-----------------------------
dbo               customer
dbo               lineitem
dbo               nation
dbo               orders
```

## <a name="support-for-the-systimezoneinfo-view"></a>Compatibilidad con la vista sys.time_zone_info
La vista [sys.time_zone_info](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-time-zone-info-transact-sql) devuelve información acerca de las zonas horarias compatibles dentro de Azure SQL Data Warehouse.

```sql
SELECT * FROM [sys].[time_zone_info];
```

**Resultados de ejemplo**
```
name                            | current_utc_offset | is_currently_dst
-------------------------------------------------------------------------
Pacific Standard Time (Mexico)    -07:00               1
US Mountain Standard Time         -07:00               0
Mountain Standard Time (Mexico)   -06:00               1
Central Standard Time             -05:00               1
```

## <a name="auto-stats-operations-appear-in-sysdmpdwexecrequests-behavior-change"></a>Las operaciones de Auto Stats aparecen en sys.dm_pdw_exec_requests (cambio de comportamiento)

Con la introducción de la opción de [creación automática de estadísticas](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-statistics#automatic-creation-of-statistics), Azure SQL Data Warehouse generará las estadísticas para optimizar la ejecución de consultas. La versión de junio de 2018 agrega la capacidad para supervisar cuándo se generan automáticamente estadísticas incorporando un registro en la vista [sys.dm_pdw_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql) cada vez que se ejecuta alguna operación [CREATE STATISTICS](https://docs.microsoft.com/sql/t-sql/statements/create-statistics-transact-sql).

```sql
SELECT
    [start_time]
    , [end_time]
    , [command]
FROM
    [sys].[dm_pdw_exec_requests]
WHERE
    [command] LIKE 'CREATE STATISTICS _WA_Sys%';
```
**Resultados de ejemplo**
```
start_time                | end_time                | command
------------------------------------------------------------------------------------------------------------------------------
2018-06-06 19:06:26.173    2018-06-06 19:06:26.173    CREATE STATISTICS _WA_Sys_00000001_63D998CC ON dbo.LineItem(l_orderkey);
```

## <a name="next-steps"></a>Pasos siguientes
Ahora que ya conoce un poco SQL Data Warehouse, aprenda a [crear una instancia de SQL Data Warehouse][create a SQL Data Warehouse] rápidamente. Si no está familiarizado con Azure, el [Glosario de Azure][Azure glossary] le puede resultar útil para consultar la nueva terminología que se encuentre. O bien, examine algunos de estos otros recursos de SQL Data Warehouse.  

* [Casos de éxito de clientes]
* [Blogs]
* [Solicitud de función]
* [Vídeos]
* [Blogs de Customer Advisory Team]
* [Foro Stack Overflow]
* [Twitter]


[Blogs]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Blogs de Customer Advisory Team]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Casos de éxito de clientes]: https://azure.microsoft.com/case-studies/?service=sql-data-warehouse
[Solicitud de función]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Foro Stack Overflow]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Vídeos]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[create a SQL Data Warehouse]: ./create-data-warehouse-portal.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md