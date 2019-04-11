---
title: Artículo sobre problemas conocidos y limitaciones de migración con las migraciones en línea a Azure SQL Database | Microsoft Docs
description: Información acerca de problemas conocidos y limitaciones de migración con las migraciones en línea a Azure SQL Database.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 04/09/2019
ms.openlocfilehash: a822e540db87c36358f1a0e34d75e05ed866868d
ms.sourcegitcommit: 6e32f493eb32f93f71d425497752e84763070fad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2019
ms.locfileid: "59471246"
---
# <a name="known-issuesmigration-limitations-with-online-migrations-to-azure-sql-db"></a>Problemas conocidos y limitaciones de migración con las migraciones en línea a Azure SQL DB

A continuación se describen problemas conocidos y limitaciones relacionadas con las migraciones en línea de SQL Server a Azure SQL Database.

> [!IMPORTANT]
> Con las migraciones en línea de SQL Server a Azure SQL Database, no se admite la migración de los tipos de datos SQL_variant.

### <a name="migration-of-temporal-tables-not-supported"></a>Migración de tablas temporales no compatibles

**Síntoma**

Si la base de datos de origen consta de una o de varias tablas temporales, se produce un error en la migración de la base de datos durante la operación de "carga completa de datos" y verá el mensaje siguiente:

{ "resourceId":"/subscriptions/<subscription id>/resourceGroups/migrateready/providers/Microsoft.DataMigration/services/<DMS Service name>", "errorType":"Database migration error", "errorEvents":"["Capture functionalities could not be set. RetCode: SQL_ERROR SqlState: 42000 NativeError: 13570 NativeError: 11.0 Message: [Microsoft][SQL Server Native Client 11.0][SQL Server]The use of replication is not supported with system-versioned temporal table '[Application. Cities]' Line: 1 Column: -1 "]" }
 
 ![Ejemplo de errores de tablas temporales](media/known-issues-azure-sql-online/dms-temporal-tables-errors.png)

**Solución alternativa**

1. Busque las tablas temporales en el esquema de origen mediante la siguiente consulta.
     ``` 
     select name,temporal_type,temporal_type_desc,* from sys.tables where temporal_type <>0
     ```
2. Excluya estas tablas de la hoja **Configurar los valores de la migración**, en la que especifica las tablas para la migración.

3. Vuelva a ejecutar la actividad de migración.

**Recursos**

Para más información, consulte el artículo [Tablas temporales](https://docs.microsoft.com/sql/relational-databases/tables/temporal-tables?view=sql-server-2017).
 
### <a name="migration-of-tables-includes-one-or-more-columns-with-the-hierarchyid-data-type"></a>La migración de las tablas incluye una o varias columnas con el tipo de datos hierarchyid

**Síntoma**

Es posible que vea una excepción SQL que indique que "ntext no es compatible con hierarchyid" durante la operación de "carga completa de datos":
     
![ejemplo de errores de hierarchyid](media/known-issues-azure-sql-online/dms-hierarchyid-errors.png)

**Solución alternativa**

1. Busque las tablas de usuario que incluyan columnas con el tipo de datos hierarchyid mediante la siguiente consulta.

      ``` 
      select object_name(object_id) 'Table name' from sys.columns where system_type_id =240 and object_id in (select object_id from sys.objects where type='U')
      ``` 

2. Excluya estas tablas de la hoja **Configurar los valores de la migración**, en la que especifica las tablas para la migración.

3. Vuelva a ejecutar la actividad de migración.

### <a name="migration-failures-with-various-integrity-violations-with-active-triggers-in-the-schema-during-full-data-load-or-incremental-data-sync"></a>Errores de migración con varias infracciones de integridad con desencadenadores activos en el esquema durante la "carga completa de datos" o la "sincronización de datos incrementales"

**Solución alternativa**

1. Busque los desencadenadores que están actualmente activos en la base de datos de origen mediante la siguiente consulta:

     ```
     select * from sys.triggers where is_disabled =0
     ```

2. Deshabilite los desencadenadores en la base de datos de origen mediante los pasos proporcionados en el artículo [DISABLE TRIGGER (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/disable-trigger-transact-sql?view=sql-server-2017).

3. Vuelva a ejecutar la actividad de migración.

### <a name="support-for-lob-data-types"></a>Compatibilidad con tipos de datos de LOB

**Síntoma**

Si la longitud de columna de objetos grandes (LOB) es mayor que 32 KB, es posible que se trunquen los datos en el destino. Puede comprobar la longitud de columna de LOB mediante la siguiente consulta: 

``` 
SELECT max(DATALENGTH(ColumnName)) as LEN from TableName
```

**Solución alternativa**

Si tiene una columna LOB que es mayor que 32 KB, póngase en contacto con el equipo de ingeniería en [formular las migraciones de base de datos de Azure](mailto:AskAzureDatabaseMigrations@service.microsoft.com).

### <a name="issues-with-timestamp-columns"></a>Problemas con las columnas de marca de tiempo

**Síntoma**

DMS no migra el valor de marca de tiempo de origen; en su lugar, DMS genera un nuevo valor de marca de tiempo en la tabla de destino.

**Solución alternativa**

Si necesita DMS para migrar el valor de marca de tiempo exacta almacenado en la tabla de origen, póngase en contacto con el equipo de ingeniería en [formular las migraciones de base de datos de Azure](mailto:AskAzureDatabaseMigrations@service.microsoft.com).

### <a name="data-migration-errors-dont-provide-additional-details-on-the-database-detailed-status-blade"></a>Errores de migración de datos no proporcionan detalles adicionales en la hoja de estado detallado de la base de datos.

**Síntoma**

Si se encuentra con errores de migración en la vista de estado de detalles de las bases de datos, seleccionando la **errores de migración de datos** vínculo en la cinta de opciones superior no puede proporcionar detalles adicionales específicos de los errores de migración.

![ejemplo no detallado de errores de migración de datos](media/known-issues-azure-sql-online/dms-data-migration-errors-no-details.png)

**Solución alternativa**

Para obtener detalles concretos del error, siga estos pasos.

1. Cierre la hoja de estado detallado de la base de datos para mostrar la pantalla de la actividad de migración.

     ![pantalla de la actividad de migración](media/known-issues-azure-sql-online/dms-migration-activity-screen.png)

2. Seleccione **Ver detalles del error** para ver mensajes de error específicos que le ayudarán a solucionar los errores de migración.
