---
title: Administración del espacio de archivo de Azure SQL Database | Microsoft Docs
description: En esta página se describe cómo administrar el espacio de archivo con Azure SQL Database, y se proporcionan ejemplos de código para determinar si se debe reducir una base de datos y cómo hacerlo.
services: sql-database
author: oslake
manager: craigg
ms.service: sql-database
ms.custom: how-to
ms.topic: conceptual
ms.date: 08/08/2018
ms.author: moslake
ms.openlocfilehash: 5dce07996191af3df3a4bdf16b211c29d59a994f
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2018
ms.locfileid: "40003865"
---
# <a name="manage-file-space-in-azure-sql-database"></a>Administración del espacio de archivo en Azure SQL Database
En este artículo se describen los diferentes tipos de espacio de almacenamiento en Azure SQL Database y los pasos que se pueden realizar cuando el espacio de archivo asignado para bases de datos y grupos elásticos necesita administrarse explícitamente.

## <a name="overview"></a>Información general

En Azure SQL Database, la mayoría de las métricas de espacio de almacenamiento mostradas en Azure Portal y las siguientes API miden el número de páginas de datos usadas en bases de datos y grupos elásticos:
- API de métricas basadas en Azure Resource Manager, como [get-metrics](https://docs.microsoft.com/powershell/module/azurerm.insights/get-azurermmetric) de PowerShell
- T-SQL: [sys.dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database)
- T-SQL:  [sys.resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)
- T-SQL: [sys.elastic_pool_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database)

Hay patrones de carga de trabajo donde la asignación de archivos de datos subyacentes para las bases de datos puede llegar a ser mayor que la cantidad de páginas de datos que se usan.  Esto puede darse cuando el espacio usado aumenta y posteriormente se eliminan los datos.  Esto se debe a que el espacio de archivo asignado no se reclama automáticamente cuando se eliminan los datos.  En tales escenarios, el espacio asignado para una base de datos o un grupo puede superar los límites admitidos e impedir el crecimiento de los datos o evitar cambios de nivel de rendimiento, y requieren la reducción de los archivos de datos para mitigar esta circunstancia.

El servicio SQL DB no reduce automáticamente los archivos de datos para reclamar el espacio asignado sin usar debido a las posibles repercusiones en el rendimiento de la base de datos.  Sin embargo, los clientes pueden reducir los archivos de datos a través de un autoservicio en el momento que estimen oportuno siguiendo los pasos descritos en [Reclamación del espacio asignado sin usar](#reclaim-unused-allocated-space). 

> [!NOTE]
> A diferencia de los archivos de datos, el servicio SQL Database reduce automáticamente los archivos de registro, ya que esa operación no afecta al rendimiento de la base de datos. 

## <a name="understanding-types-of-storage-space-for-a-database"></a>Descripción de los tipos de espacio de almacenamiento para una base de datos

Comprender las cantidades de espacio de almacenamiento siguientes es importante para administrar el espacio de archivo de una base de datos.

|Cantidad de base de datos|Definición|Comentarios|
|---|---|---|
|**Espacio de datos usado**|La cantidad de espacio usado para almacenar los datos de la base de datos en páginas de 8 KB.|Por lo general, el espacio usado aumenta (disminuciones) en las inserciones (eliminaciones). En algunos casos, el espacio usado no cambia en las inserciones o eliminaciones, según la cantidad y el patrón de datos implicados en la operación y las posibles fragmentaciones. Por ejemplo, al eliminar una fila de cada página de datos no disminuye necesariamente el espacio usado.|
|**Espacio de datos asignado**|La cantidad de espacio de archivo de formato disponible para almacenar datos de la base de datos.|La cantidad de espacio asignado crece automáticamente, pero nunca disminuye después de las eliminaciones. Este comportamiento garantiza que las futuras inserciones son más rápidas puesto que no es necesario volver a formatear el espacio.|
|**Espacio de datos asignado, pero no usado**|La diferencia entre la cantidad de espacio de datos asignado y el espacio de datos usado.|Esta cantidad representa la cantidad máxima de espacio libre que se puede reclamar mediante la reducción de archivos de datos de base de datos.|
|**Tamaño máximo de datos**|La cantidad máxima de espacio que se puede usar para almacenar datos de base de datos.|La cantidad de espacio de datos asignado no puede crecer por encima del tamaño máximo de datos.|
||||

En el siguiente diagrama se ilustra la relación entre los diferentes tipos de espacio de almacenamiento para una base de datos.

![tipos de espacio de almacenamiento y relaciones](./media/sql-database-file-space-management/storage-types.png) 

## <a name="query-a-database-for-storage-space-information"></a>Consulta de la información de espacio de almacenamiento en una base de datos

Las consultas siguientes pueden utilizarse para determinar las cantidades de espacio de almacenamiento para una base de datos.  

### <a name="database-data-space-used"></a>Espacio de datos de la base de datos usado
Modifique la siguiente consulta para devolver la cantidad de espacio de datos de base de datos usado.  Las unidades de resultado de la consulta están en MB.

```sql
-- Connect to master
-- Database data space used in MB
SELECT TOP 1 storage_in_megabytes AS DatabaseDataSpaceUsedInMB
FROM sys.resource_stats
WHERE database_name = 'db1'
ORDER BY end_time DESC
```

### <a name="database-data-space-allocated-and-unused-allocated-space"></a>Espacio de datos de base de datos asignado y espacio asignado sin usar
Use la siguiente consulta para devolver la cantidad de espacio de datos de base de datos asignado y la cantidad de espacio asignado sin usar.  Las unidades de resultado de la consulta están en MB.

```sql
-- Connect to database
-- Database data space allocated in MB and database data space allocated unused in MB
SELECT SUM(size/128.0) AS DatabaseDataSpaceAllocatedInMB, 
SUM(size/128.0 - CAST(FILEPROPERTY(name, 'SpaceUsed') AS int)/128.0) AS DatabaseDataSpaceAllocatedUnusedInMB 
FROM sys.database_files
GROUP BY type_desc
HAVING type_desc = 'ROWS'
```
 
### <a name="database-data-max-size"></a>Tamaño máximo de datos de base de datos
Modifique la siguiente consulta para devolver el tamaño máximo de datos de la base de datos.  Las unidades del resultado de la consulta están en bytes.

```sql
-- Connect to database
-- Database data max size in bytes
SELECT DATABASEPROPERTYEX('db1', 'MaxSizeInBytes') AS DatabaseDataMaxSizeInBytes
```

## <a name="understanding-types-of-storage-space-for-an-elastic-pool"></a>Descripción de los tipos de espacio de almacenamiento para un grupo elástico

Comprender las cantidades de espacio de almacenamiento siguientes es importante para administrar el espacio de archivo de un grupo elástico.

|Cantidad de grupo elástico|Definición|Comentarios|
|---|---|---|
|**Espacio de datos usado**|La suma del espacio de datos utilizado por todas las bases de datos en el grupo elástico.||
|**Espacio de datos asignado**|La suma del espacio de datos asignado por todas las bases de datos en el grupo elástico.||
|**Espacio de datos asignado, pero no usado**|La diferencia entre la cantidad de espacio de datos asignado y el espacio de datos usado por todas las base de datos del grupo elástico.|Esta cantidad representa la cantidad máxima de espacio asignado para el grupo elástico que se puede reclamar mediante la reducción de archivos de datos de base de datos.|
|**Tamaño máximo de datos**|La cantidad máxima de espacio de datos que puede usar el grupo elástico para todas sus bases de datos.|El espacio asignado para el grupo elástico no puede exceder el tamaño máximo del grupo elástico.  Si esto ocurre, el espacio asignado que no se usa se puede reclamar mediante la reducción de archivos de datos de base de datos.|
||||

## <a name="query-an-elastic-pool-for-storage-space-information"></a>Consulta de la información de espacio de almacenamiento en un grupo elástico

Las consultas siguientes pueden utilizarse para determinar las cantidades de espacio de almacenamiento para un grupo elástico.  

### <a name="elastic-pool-data-space-used"></a>Espacio usado de datos de grupo elástico
Modifique la siguiente consulta para devolver la cantidad de espacio usado de datos del grupo elástico.  Las unidades de resultado de la consulta están en MB.

```sql
-- Connect to master
-- Elastic pool data space used in MB  
SELECT TOP 1 avg_storage_percent * elastic_pool_storage_limit_mb AS ElasticPoolDataSpaceUsedInMB
FROM sys.elastic_pool_resource_stats
WHERE elastic_pool_name = 'ep1'
ORDER BY end_time DESC
```

### <a name="elastic-pool-data-space-allocated-and-unused-allocated-space"></a>Espacio de datos de grupo elástico asignado y espacio asignado sin usar

Modifique el siguiente script de PowerShell para devolver una tabla que muestre el espacio asignado y el espacio asignado sin usar para cada base de datos de un grupo elástico. La tabla ordena las bases de datos desde las que tienen más hasta las que tienen menos cantidad de espacio asignado sin usar.  Las unidades de resultado de la consulta están en MB.  

Los resultados de la consulta para determinar el espacio asignado para cada base de datos del grupo se pueden agregar juntos para determinar el espacio total asignado para el grupo elástico. El espacio de grupo elástico asignado no puede exceder el tamaño máximo del grupo elástico.  

```powershell
# Resource group name
$resourceGroupName = "rg1" 
# Server name
$serverName = "ls2" 
# Elastic pool name
$poolName = "ep1"
# User name for server
$userName = "name"
# Password for server
$password = "password"

# Get list of databases in elastic pool
$databasesInPool = Get-AzureRmSqlElasticPoolDatabase `
    -ResourceGroupName $resourceGroupName `
    -ServerName $serverName `
    -ElasticPoolName $poolName
$databaseStorageMetrics = @()

# For each database in the elastic pool,
# get its space allocated in MB and space allocated unused in MB.
# Requires SQL Server PowerShell module – see here to install.  
foreach ($database in $databasesInPool)
{
    $sqlCommand = "SELECT DB_NAME() as DatabaseName, `
    SUM(size/128.0) AS DatabaseDataSpaceAllocatedInMB, `
    SUM(size/128.0 - CAST(FILEPROPERTY(name, 'SpaceUsed') AS int)/128.0) AS DatabaseDataSpaceAllocatedUnusedInMB `
    FROM sys.database_files `
    GROUP BY type_desc `
    HAVING type_desc = 'ROWS'"
    $serverFqdn = "tcp:" + $serverName + ".database.windows.net,1433"
    $databaseStorageMetrics = $databaseStorageMetrics + 
        (Invoke-Sqlcmd -ServerInstance $serverFqdn `
        -Database $database.DatabaseName `
        -Username $userName `
        -Password $password `
        -Query $sqlCommand)
}
# Display databases in descending order of space allocated unused
Write-Output "`n" "ElasticPoolName: $poolName"
Write-Output $databaseStorageMetrics | Sort `
    -Property DatabaseDataSpaceAllocatedUnusedInMB `
    -Descending | Format-Table
```

La captura de pantalla siguiente es un ejemplo de la salida del script:

![ejemplo de espacio asignado al grupo elástico y espacio asignado sin usar](./media/sql-database-file-space-management/elastic-pool-allocated-unused.png)

### <a name="elastic-pool-data-max-size"></a>Tamaño máximo de datos del grupo elástico

Modifique la siguiente consulta T-SQL para devolver el tamaño máximo de datos del grupo elástico.  Las unidades de resultado de la consulta están en MB.

```sql
-- Connect to master
-- Elastic pools max size in MB
SELECT TOP 1 elastic_pool_storage_limit_mb AS ElasticPoolMaxSizeInMB
FROM sys.elastic_pool_resource_stats
WHERE elastic_pool_name = 'ep1'
ORDER BY end_time DESC
```

## <a name="reclaim-unused-allocated-space"></a>Reclamación del espacio asignado sin usar

Una vez que se han identificado las bases de datos para reclamar el espacio asignado sin usar, modifique el siguiente comando para reducir los archivos de datos para cada base de datos.

```sql
-- Shrink database data space allocated.
DBCC SHRINKDATABASE (N'db1')
```

Para más información sobre este comando, consulte [SHRINKDATABASE](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql). 

> [!IMPORTANT] 
> Considere la posibilidad de recompilar los índices de base de datos después de reducir los archivos de datos de la base de datos; los índices se pueden fragmentar y perder su eficacia para la optimización del rendimiento. Si esto sucede, se deben recompilar. Para más información sobre la fragmentación y la recompilación de índices, consulte [Reorganizar y volver a generar índices](https://docs.microsoft.com/sql/relational-databases/indexes/reorganize-and-rebuild-indexes).

## <a name="next-steps"></a>Pasos siguientes

- Para información sobre los tamaños máximos de base de datos, consulte:
  - [Límites del modelo de compra basado en núcleos virtuales de Azure SQL Database para una base de datos única](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-single-databases)
  - [Límites de recursos para bases de datos únicas que utilizan el modelo de compra basado en DTU](https://docs.microsoft.com/azure/sql-database/sql-database-dtu-resource-limits-single-databases)
  - [Límites del modelo de compra basado en núcleos virtuales de Azure SQL Database para grupos elásticos](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-elastic-pools)
  - [Límites de recursos para grupos elásticos que utilizan el modelo de compra basado en DTU](https://docs.microsoft.com/azure/sql-database/sql-database-dtu-resource-limits-elastic-pools)
- Para más información sobre el comando `SHRINKDATABASE`, consulte [SHRINKDATABASE](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql). 
- Para más información sobre la fragmentación y la recompilación de índices, consulte [Reorganizar y volver a generar índices](https://docs.microsoft.com/sql/relational-databases/indexes/reorganize-and-rebuild-indexes).
