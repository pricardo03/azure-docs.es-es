---
title: 'Creación y administración de grupos elásticos: Azure SQL Database | Microsoft Docs'
description: Cree y administre grupos elásticos de Azure SQL.
services: sql-database
ms.service: sql-database
subservice: elastic-pool
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: carlrab
manager: craigg
ms.date: 09/20/2018
ms.openlocfilehash: 6418694097c472afd6a2c706e55a9026ab03dcff
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2018
ms.locfileid: "47162726"
---
# <a name="create-and-manage-elastic-pools-in-azure-sql-database"></a>Creación y administración de grupos elásticos en Azure SQL Database

Con un grupo elástico, se determina la cantidad de recursos que este requiere para controlar la carga de trabajo de sus bases de datos y la cantidad de recursos para cada base de datos agrupada. 

## <a name="azure-portal-manage-elastic-pools-and-pooled-databases"></a>Azure Portal: administración de grupos elásticos y bases de datos agrupadas

Todas las configuraciones de grupo pueden encontrarse en un único lugar: la hoja **Configurar grupo**. Para llegar aquí, busque un grupo elástico en el portal y haga clic en **Configurar grupo** desde la parte superior de la hoja o desde el menú de recursos de la izquierda.

Desde aquí puede realizar cualquier combinación de los siguientes cambios y guardarlos todos en un lote:
1. Cambiar el nivel de servicio del grupo
2. Escalar o reducir verticalmente el rendimiento (DTU o núcleos virtuales) y el almacenamiento
3. Agregar o quitar bases de datos en el grupo
4. Establecer un límite de rendimiento mínimo (garantizado) y máximo para las bases de datos de los grupos
5. Revisar el resumen de costos para ver los cambios en su factura como resultado de las nuevas selecciones

![Hoja de configuración del grupo elástico](./media/sql-database-elastic-pool-manage-portal/configure-pool.png)

## <a name="powershell-manage-elastic-pools-and-pooled-databases"></a>PowerShell: administración de grupos elásticos y bases de datos agrupadas 

Para crear y administrar grupos elásticos y bases de datos agrupadas de SQL Database con Azure PowerShell, use los siguientes cmdlets de PowerShell. Si necesita instalar o actualizar PowerShell, consulte [Install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps) (Instalación y configuración de Azure PowerShell). Para crear y administrar los servidores lógicos para un grupo elástico, consulte el artículo sobre cómo [crear y administrar servidores lógicos](sql-database-logical-servers.md). Para crear y administrar reglas de firewall, consulte la sección [Administración de reglas de firewall mediante Azure PowerShell](sql-database-firewall-configure.md#manage-firewall-rules-using-azure-powershell).

> [!TIP]
> Para obtener scripts de ejemplo de PowerShell, vea [Creación de grupos elásticos y traslado de bases de datos de un grupo a otro y fuera de un grupo mediante PowerShell](scripts/sql-database-move-database-between-pools-powershell.md) y [Uso de PowerShell para supervisar y escalar un grupo elástico de SQL en Azure SQL Database](scripts/sql-database-monitor-and-scale-pool-powershell.md).
>

| Cmdlet | DESCRIPCIÓN |
| --- | --- |
|[New-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool)|Crea un grupo de bases de datos elásticas en un servidor SQL Server lógico.|
|[Get-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/get-azurermsqlelasticpool)|Obtiene los grupos elásticos y los valores de sus propiedades de un servidor SQL Server lógico.|
|[Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool)|Modifica las propiedades de un grupo de bases de datos elásticas en un servidor SQL Server lógico. Por ejemplo, use la propiedad **StorageMB** para modificar el almacenamiento máximo de un grupo elástico.|
|[Remove-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/remove-azurermsqlelasticpool)|Elimina un grupo de bases de datos elásticas en un servidor SQL Server lógico.|
|[Get-AzureRmSqlElasticPoolActivity](/powershell/module/azurerm.sql/get-azurermsqlelasticpoolactivity)|Obtiene el estado de las operaciones en un grupo elástico en un servidor SQL Server lógico.|
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Crea una nueva base de datos en un grupo existente o como una sola base de datos. |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Obtiene una o más bases de datos.|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Establece las propiedades de una base de datos o mueve una base de datos existente a un grupo elástico, fuera de él o entre grupos elásticos.|
|[Remove-AzureRmSqlDatabase](/powershell/module/azurerm.sql/remove-azurermsqldatabase)|Quita una base de datos.|


> [!TIP]
> La creación de varias bases de datos en un grupo elástico puede tardar tiempo cuando se realiza mediante el portal o los cmdlets de PowerShell que crean una base de datos única cada vez. Para automatizar la creación en un grupo elástico, vea [CreateOrUpdateElasticPoolAndPopulate](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).
>

## <a name="azure-cli-manage-elastic-pools-and-pooled-databases"></a>CLI de Azure: administración de grupos elásticos y bases de datos agrupadas

Para crear y administrar grupos elásticos de SQL Database con la [CLI de Azure](/cli/azure), use los siguientes comandos de la [CLI de Azure para SQL Database](/cli/azure/sql/db). Use [Cloud Shell](/azure/cloud-shell/overview) para ejecutar la CLI en el explorador o [instálela](/cli/azure/install-azure-cli) en Windows, Linux o macOS.

> [!TIP]
> Para obtener scripts de ejemplo de la CLI de Azure, vea [Uso de la CLI para mover una instancia de Azure SQL Database a un grupo elástico de SQL](scripts/sql-database-move-database-between-pools-cli.md) y [Uso de la CLI de Azure para escalar un grupo elástico de SQL en Azure SQL Database](scripts/sql-database-scale-pool-cli.md).
>

| Cmdlet | DESCRIPCIÓN |
| --- | --- |
|[az sql elastic-pool create](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-create)|Crea un grupo elástico.|
|[az sql elastic-pool list](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-list)|Devuelve una lista de grupos elásticos de un servidor.|
|[az sql elastic-pool list-dbs](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-list-dbs)|Devuelve una lista de bases de datos de un grupo elástico.|
|[az sql elastic-pool list-editions](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-list-editions)|Además incluye los parámetros disponibles de DTU de grupo, los límites de almacenamiento y la configuración por base de datos. Para reducir el nivel de detalle, los límites de almacenamiento y la configuración por base de datos adicionales están ocultos de forma predeterminada.|
|[az sql elastic-pool update](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-update)|Actualiza un grupo elástico.|
|[az sql elastic-pool delete](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-delete)|Elimina el grupo elástico.|

## <a name="transact-sql-manage-pooled-databases"></a>Transact-SQL: administración de bases de datos agrupadas

Para crear y mover bases de datos en grupos elásticos existentes o para devolver información sobre un grupo elástico de SQL Database con Transact-SQL, use los siguientes comandos de T-SQL. Puede emitir estos comandos mediante Azure Portal, [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs) o cualquier otro programa que pueda conectarse a un servidor de Azure SQL Database y pasar comandos de Transact-SQL. Para crear y administrar reglas de firewall mediante T-SQL, consulte [Administración de reglas de firewall mediante Transact-SQL](sql-database-firewall-configure.md#manage-firewall-rules-using-transact-sql).

> [!IMPORTANT]
> No es posible crear, actualizar ni eliminar un grupo elástico de Azure SQL Database mediante Transact-SQL. Puede agregar o quitar bases de datos de un grupo elástico y puede usar DMV para devolver información sobre grupos elásticos existentes.
>

| Get-Help | DESCRIPCIÓN |
| --- | --- |
|[CREATE DATABASE (Azure SQL Database)](/sql/t-sql/statements/create-database-azure-sql-database)|Crea una nueva base de datos en un grupo existente o como una sola base de datos. Debe estar conectado a la base de datos maestra para crear una base de datos.|
| [ALTER DATABASE (Azure SQL Database)](/sql/t-sql/statements/alter-database-azure-sql-database) |Mueve una base de datos a un grupo elástico, fuera de él o entre grupos elásticos.|
|[DROP DATABASE (Transact-SQL)](/sql/t-sql/statements/drop-database-transact-sql)|Permite eliminar una base de datos.|
|[sys.elastic_pool_resource_stats (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database)|Devuelve estadísticas de uso de recursos de todos los grupos de bases de datos elásticas de un servidor lógico. Para cada grupo de bases de datos elásticas hay una fila por cada ventana de informe de 15 segundos (cuatro filas por minuto). Esto incluye uso de CPU, E/S, registro, almacenamiento y empleo simultáneo de solicitudes o sesiones por parte de todas las bases de datos del grupo.|
|[sys.database_service_objectives (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Devuelve la edición (nivel de servicio), el objetivo de servicio (plan de tarifa) y el nombre del grupo elástico, si existe, para una base de datos SQL de Azure o una instancia de Azure SQL Data Warehouse. Si inició sesión en la base de datos maestra en un servidor de Azure SQL Database, devuelve información sobre todas las bases de datos. Para Azure SQL Data Warehouse, debe estar conectado a la base de datos maestra.|

## <a name="rest-api-manage-elastic-pools-and-pooled-databases"></a>API de REST: administración de grupos elásticos y bases de datos agrupadas

Para crear y administrar grupos elásticos y bases de datos agrupadas de SQL Database, use estas solicitudes de la API de REST.

| Get-Help | DESCRIPCIÓN |
| --- | --- |
|[Grupos elásticos: creación o actualización](/rest/api/sql/elasticpools/createorupdate)|Crea un nuevo grupo elástico o actualiza uno ya existente.|
|[Grupos elásticos: eliminación](/rest/api/sql/elasticpools/delete)|Elimina el grupo elástico.|
|[Grupos elásticos: obtención](/rest/api/sql/elasticpools/get)|Obtiene un grupo elástico.|
|[Grupos elásticos: enumeración por servidor](/rest/api/sql/elasticpools/listbyserver)|Devuelve una lista de grupos elásticos de un servidor.|
|[Grupos elásticos: actualización](/rest/api/sql/elasticpools/update)|Actualiza un grupo elástico ya existente.|
|[Actividades de los grupos elásticos](/rest/api/sql/elasticpoolactivities)|Devuelve las actividades de los grupos elásticos.|
|[Actividades de las bases de datos de grupos elásticos](/rest/api/sql/elasticpooldatabaseactivities)|Devuelve la actividad de las bases de datos de un grupo elástico.|
|[Databases - Create Or Update](/rest/api/sql/databases/createorupdate)|Crea una nueva base de datos o actualiza una ya existente.|
|[Databases - Get](/rest/api/sql/databases/get)|Obtiene una base de datos.|
|[Databases - List By Elastic Pool](/rest/api/sql/databases/listbyelasticpool)|Devuelve una lista de bases de datos de un grupo elástico.|
|[Databases - List By Server](/rest/api/sql/databases/listbyserver)|Devuelve una lista de bases de datos de un servidor.|
|[Databases - Update](/rest/api/sql/databases/update)|Actualiza una base de datos existente.|

## <a name="next-steps"></a>Pasos siguientes

* Para ver un vídeo, vea el [Curso de vídeo de la Academia virtual de Microsoft sobre las funcionalidades de las bases de datos elásticas en Azure SQL Database](https://mva.microsoft.com/training-courses/elastic-database-capabilities-with-azure-sql-db-16554).
* Para más información sobre los patrones de diseño de las aplicaciones SaaS que usan grupos elásticos, consulte [Design Patterns for Multi-tenant SaaS Applications with Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md)(Diseño de patrones para aplicaciones SaaS multiempresa con Base de datos SQL de Azure).
* Para obtener un tutorial sobre SaaS con grupos elásticos, vea [Introducción a la aplicación de SaaS Wingtip](sql-database-wtp-overview.md).
