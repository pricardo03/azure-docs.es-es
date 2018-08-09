---
title: Administración del espacio de archivo de Azure SQL Database | Microsoft Docs
description: En esta página se describe cómo administrar el espacio de archivo con Azure SQL Database, y se proporcionan ejemplos de código para determinar si se debe reducir una base de datos y cómo hacerlo.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: how-to
ms.topic: conceptual
ms.date: 08/01/2018
ms.author: carlrab
ms.openlocfilehash: 1ecc0ce08ef42f5f5935bca29e8269be2ea142f0
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2018
ms.locfileid: "39416008"
---
# <a name="manage-file-space-in-azure-sql-database"></a>Administración del espacio de archivo en Azure SQL Database

En este artículo se describen los diferentes tipos de espacio de almacenamiento en Azure SQL Database y los pasos que se pueden realizar cuando el cliente debe administrar el espacio de archivo asignado para bases de datos y grupos elásticos.

## <a name="overview"></a>Información general

En Azure SQL Database, las métricas de tamaño de almacenamiento mostradas en Azure Portal y las siguientes API miden el número de páginas de datos usadas en bases de datos y grupos elásticos:
- API de métricas basadas en Azure Resource Manager, como [get-metrics](https://docs.microsoft.com/powershell/module/azurerm.insights/get-azurermmetric) de PowerShell
- T-SQL: [sys.dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database)
- T-SQL:  [sys.resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)
- T-SQL: [sys.elastic_pool_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database)

Existen patrones de carga de trabajo en los que la asignación de espacio en los archivos de datos subyacentes de las bases de datos se vuelve más grande que el número de páginas de datos usadas en los archivos de datos. Este escenario puede darse cuando el espacio usado aumenta y posteriormente se eliminan los datos. Cuando se eliminan los datos, el espacio de archivo asignado no se reclama automáticamente. En estos escenarios, el espacio asignado para una base de datos o un grupo puede superar los límites máximos admitidos establecidos (o admitidos); como resultado, se puede impedir que crezcan los datos o que cambie el nivel de rendimiento, aunque el espacio de la base de datos usado realmente sea inferior al límite de espacio máximo. Para solucionar este problema, puede que deba reducir la base de datos para reducir el espacio asignado, pero sin usar de la base de datos.

El servicio SQL Database no reduce automáticamente los archivos de base de datos para reclamar el espacio asignado sin usar debido a las posibles repercusiones en el rendimiento de la base de datos. Sin embargo, puede reducir el archivo de una base de datos en un momento determinado si sigue los pasos descritos en [Reclamación del espacio asignado sin usar](#reclaim-unused-allocated-space). 

> [!NOTE]
> A diferencia de los archivos de datos, el servicio SQL Database reduce automáticamente los archivos de registro, ya que esa operación no afecta al rendimiento de la base de datos.

## <a name="understanding-the-types-of-storage-space-for-a-database"></a>Tipos de espacio de almacenamiento para una base de datos

Para administrar el espacio de archivo, debe comprender los siguientes términos relacionados con el almacenamiento de base de datos, tanto para una única base de datos como para un grupo elástico.

|Espacio de almacenamiento|Definición|Comentarios|
|---|---|---|
|**Espacio de datos usado**|La cantidad de espacio usado para almacenar los datos de la base de datos en páginas de 8 KB.|Por lo general, este espacio usado aumenta (disminuye) en las inserciones (eliminaciones). En algunos casos, el espacio usado no cambia en las inserciones o eliminaciones, según la cantidad y el patrón de datos implicados en la operación y las posibles fragmentaciones. Por ejemplo, al eliminar una fila de cada página de datos no disminuye necesariamente el espacio usado.|
|**Espacio asignado**|La cantidad de espacio de archivo de formato disponible para almacenar datos de la base de datos.|El espacio asignado crece automáticamente, pero nunca disminuye después de las eliminaciones. Este comportamiento garantiza que las futuras inserciones son más rápidas puesto que no es necesario volver a formatear el espacio.|
|**Espacio asignado, pero no usado**|La cantidad de espacio de archivo de datos sin usar asignado para la base de datos.|Esta cantidad es la diferencia entre la cantidad de espacio asignado y el espacio usado, y representa la cantidad máxima de espacio que se puede reclamar reduciendo los archivos de base de datos.|
|**Tamaño máximo**|La cantidad máxima de espacio de datos que puede usar la base de datos.|El espacio de datos asignado no puede crecer por encima del tamaño máximo de datos.|
||||

En el siguiente diagrama se ilustra la relación entre los tipos de espacio de almacenamiento.

![tipos de espacio de almacenamiento y relaciones](./media/sql-database-file-space-management/storage-types.png)

## <a name="query-a-database-for-storage-space-information"></a>Consulta de la información de espacio de almacenamiento en una base de datos

Para determinar si tiene espacio asignado pero sin usar en una base de datos que quiera reclamar, use las siguientes consultas:

### <a name="database-data-space-used"></a>Espacio de datos de la base de datos usado
Modifique la siguiente consulta para devolver la cantidad de espacio de datos de la base de datos usado en MB.

```sql
-- Connect to master
-- Database data space used in MB
SELECT TOP 1 storage_in_megabytes AS DatabaseDataSpaceUsedInMB
FROM sys.resource_stats
WHERE database_name = 'db1'
ORDER BY end_time DESC
```

### <a name="database-data-allocated-and-allocated-space-unused"></a>Espacio asignado a datos de la base de datos y espacio asignado sin usar
Modifique la siguiente consulta para devolver la cantidad de espacio asignado a los datos de la base de datos y el espacio asignado sin usar.

```sql
-- Connect to database
-- Database data space allocated in MB and database data space allocated unused in MB
SELECT SUM(size/128.0) AS DatabaseDataSpaceAllocatedInMB, 
SUM(size/128.0 - CAST(FILEPROPERTY(name, 'SpaceUsed') AS int)/128.0) AS DatabaseDataSpaceAllocatedUnusedInMB 
FROM sys.database_files
GROUP BY type_desc
HAVING type_desc = 'ROWS'
```
 
### <a name="database-max-size"></a>Tamaño máximo de la base de datos
Modifique la siguiente consulta para devolver el tamaño máximo de la base de datos en bytes.

```sql
-- Connect to database
-- Database data max size in bytes
SELECT DATABASEPROPERTYEX('db1', 'MaxSizeInBytes') AS DatabaseDataMaxSizeInBytes
```

## <a name="query-an-elastic-pool-for-storage-space-information"></a>Consulta de la información de espacio de almacenamiento en un grupo elástico

Para determinar si tiene espacio asignado pero sin usar en un grupo elástico, y para cada base de datos agrupada que quiera reclamar, use las siguientes consultas:

### <a name="elastic-pool-data-space-used"></a>Espacio usado de datos de grupo elástico
Modifique la siguiente consulta para devolver la cantidad de espacio usado de datos del grupo elástico en MB.

```sql
-- Connect to master
-- Elastic pool data space used in MB  
SELECT TOP 1 avg_storage_percent * elastic_pool_storage_limit_mb AS ElasticPoolDataSpaceUsedInMB
FROM sys.elastic_pool_resource_stats
WHERE elastic_pool_name = 'ep1'
ORDER BY end_time DESC
```

### <a name="elastic-pool-data-allocated-and-allocated-space-unused"></a>Espacio asignado a datos del grupo elástico y espacio asignado sin usar

Modifique el siguiente script de PowerShell para devolver una tabla que muestre el espacio total asignado y el espacio asignado sin usar para cada base de datos de un grupo elástico. La tabla ordena las bases de datos desde las que tienen más hasta las que tienen menos cantidad de espacio asignado sin usar.  

Los resultados de la consulta para determinar el espacio asignado para cada base de datos del grupo se pueden agregar juntos para determinar el espacio de grupo elástico asignado. El espacio de grupo elástico asignado no puede exceder el tamaño máximo del grupo elástico.  

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

### <a name="elastic-pool-max-size"></a>Tamaño máximo del grupo elástico

Use la siguiente consulta T-SQL para devolver el tamaño máximo de la base de datos elástica en MB.

```sql
-- Connect to master
-- Elastic pools max size in MB
SELECT TOP 1 elastic_pool_storage_limit_mb AS ElasticPoolMaxSizeInMB
FROM sys.elastic_pool_resource_stats
WHERE elastic_pool_name = 'ep1'
ORDER BY end_time DESC
```

## <a name="reclaim-unused-allocated-space"></a>Reclamación del espacio asignado sin usar

Cuando haya determinado que tiene espacio asignado sin usar que quiere reclamar, use el siguiente comando para reducir el espacio de base de datos asignado. 

> [!IMPORTANT]
> En el caso de las bases de datos de un grupo elástico, se deben reducir primero las bases de datos con el máximo espacio asignado sin usar a fin de reclamar espacio de archivo de manera más rápida.  

Use el siguiente comando para reducir todos los archivos de datos de la base de datos especificada:

```sql
-- Shrink database data space allocated.
DBCC SHRINKDATABASE (N'<database_name>')
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