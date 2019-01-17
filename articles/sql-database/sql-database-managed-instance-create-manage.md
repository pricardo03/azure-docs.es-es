---
title: Referencia sobre la API de administración de la Instancia administrada de Azure SQL Database | Microsoft Docs
description: Información acerca de la creación y administración de Instancias administradas de Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 01/03/2019
ms.openlocfilehash: 30efbcd5ba36ea912a7238eef9e300085cb1d3d1
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2019
ms.locfileid: "54040207"
---
# <a name="managed-api-reference-for-azure-sql-database-managed-instances"></a>Referencia sobre la API de administración de la Instancia administrada de Azure SQL Database

Puede crear y administrar Instancias administradas de Azure SQL Database mediante Azure Portal, PowerShell, la CLI de Azure, la API REST y Transact-SQL. En este artículo, puede encontrar información general sobre las funciones y la API que puede usar para crear y configurar Instancia administrada.

## <a name="azure-portal-create-a-managed-instance"></a>Azure Portal: Creación de una instancia administrada

Para consultar un artículo de inicio rápido que muestra cómo crear una Instancia administrada de Azure SQL Database, consulte [Inicio rápido: Creación de una Instancia administrada de Azure SQL Database](sql-database-managed-instance-get-started.md).

## <a name="powershell-create-and-manage-a-managed-instance"></a>PowerShell: Creación y administración de una Instancia administrada

Para crear y administrar servidores, bases de datos y firewalls SQL de Azure con Azure PowerShell, use los siguientes cmdlets de PowerShell. Si necesita instalar o actualizar PowerShell, consulte [Install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps) (Instalación y configuración de Azure PowerShell).

> [!TIP]
> Para los scripts de ejemplo de PowerShell, consulte [Quick-start script: Create Azure SQL Managed Instance using PowerShell library](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/27/quick-start-script-create-azure-sql-managed-instance-using-powershell/) (Script de inicio rápido: Creación de una Instancia administrada de Azure SQL Database mediante una biblioteca de PowerShell).

| Cmdlet | DESCRIPCIÓN |
| --- | --- |
|[New-AzureRmSqlInstance](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqlinstance)|Crea una Instancia administrada de Azure SQL Database |
|[Get-AzureRmSqlInstance](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlinstance)|Devuelve información sobre Instancia administrada de Azure SQL Database|
|[Set-AzureRmSqlInstance](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqlinstance)|Establece las propiedades para una Instancia administrada de Azure SQL Database|
|[Remove-AzureRmSqlInstance](https://docs.microsoft.com/powershell/module/azurerm.sql/remove-azurermsqlinstance)|Elimina una Instancia administrada de Azure SQL Database|
|[New-AzureRmSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqlinstancedatabase)|Crea una base de datos de Instancia administrada de Azure SQL Database|
|[Get-AzureRmSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlinstancedatabase)|Devuelve información sobre base de datos de Instancia administrada de Azure SQL Database|
|[Remove-AzureRmSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/azurerm.sql/remove-azurermsqlinstancedatabase)|Elimina una base de datos de Instancia administrada de Azure SQL Database|
|[Restore-AzureRmSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/azurerm.sql/restore-azurermsqlinstancedatabase)|Restaura una base de datos de Instancia administrada de Azure SQL Database|

## <a name="azure-cli-manage-logical-servers-and-databases"></a>CLI de Azure: Administración de servidores lógicos y bases de datos

Para crear y administrar servidores, bases de datos y firewalls SQL Azure con la [CLI de Azure](/cli/azure), use los siguientes comandos de [Instancia administrada de la CLI de Azure SQL Database](/cli/azure/sql/mi). Use [Cloud Shell](/azure/cloud-shell/overview) para ejecutar la CLI en el explorador o [instálela](/cli/azure/install-azure-cli) en Windows, Linux o macOS.

> [!TIP]
> Para consultar un artículo de inicio rápido acerca de la CLI de Azure, consulte [Working with SQL Managed Instance using Azure CLI](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44) (Uso de Instancia administrada de Azure SQL Database mediante la CLI de Azure).

| Cmdlet | DESCRIPCIÓN |
| --- | --- |
|[az sql mi create](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-create) |Crea una instancia administrada|
|[az sql mi list](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-list)|Enumera las instancias administradas disponibles|
|[az sql mi show](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-show)|Obtiene los detalles de una instancia administrada|
|[az sql mi update](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-update)|Actualiza una instancia administrada|
|[az sql mi delete](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-delete)|Elimina una instancia administrada|
|[az sql midb create](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-create) |Crea una base de datos administrada|
|[az sql midb list](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-list)|Enumera las bases de datos administradas disponibles|
|[az sql midb restore](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-restore)|Restaura una base de datos administrada|
|[az sql midb update](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-update)|Actualiza una base de datos administrada|
|[az sql midb delete](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-delete)|Elimina una base de datos administrada|

## <a name="transact-sql-manage-logical-servers-and-databases"></a>Transact-SQL: Administración de servidores lógicos y bases de datos

Para crear y administrar una base de datos de Instancia administrada de Azure SQL Database una vez creada la instancia, use los siguientes comandos de T-SQL. Puede emitir estos comandos mediante Azure Portal, [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio) y [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is). [Visual Studio Code](https://code.visualstudio.com/docs), o cualquier otro programa que se pueda conectar a un servidor de Azure SQL Database y pasar comandos de Transact-SQL.

> [!TIP]
> Para consultar artículos de inicio rápido que describan la configuración y conexión a una instancia administrada mediante SQL Server Management Studio en Microsoft Windows, consulte [Inicio rápido: Configuración de una máquina virtual de Azure para la conexión a Instancia administrada de Azure SQL Database](sql-database-managed-instance-configure-vm.md) y [Inicio rápido: Configuración de una conexión de punto a sitio a una Instancia administrada de Azure SQL Database desde el entorno local](sql-database-managed-instance-configure-p2s.md).
> [!IMPORTANT]
> No se puede crear ni eliminar una instancia administrada mediante Transact-SQL.

| Get-Help | DESCRIPCIÓN |
| --- | --- |
|[CREATE DATABASE](https://docs.microsoft.com/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-mi-current)|Crea una nueva base de datos de instancias administradas. Debe estar conectado a la base de datos maestra para crear una base de datos.|
| [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-mi-current) |Modifica una base de datos de Instancia administrada de Azure SQL Database.|

## <a name="rest-api-manage-logical-servers-and-databases"></a>API REST: Administración de servidores lógicos y bases de datos

Para crear y administrar una Instancia administrada de Azure SQL Database, use estas solicitudes de la API REST.

| Get-Help | DESCRIPCIÓN |
| --- | --- |
|[Managed Instances - Create Or Update](https://docs.microsoft.com/rest/api/sql/managedinstances/createorupdate)|Crea o actualiza una instancia administrada.|
|[Managed Instances - Delete](https://docs.microsoft.com/rest/api/sql/managedinstances/delete)|Elimina una instancia administrada.|
|[Managed Instances - Get](https://docs.microsoft.com/rest/api/sql/managedinstances/get)|Obtiene una instancia administrada.|
|[Managed Instances - List](https://docs.microsoft.com/rest/api/sql/managedinstances/list)|Devuelve una lista de instancias administradas en una suscripción.|
|[Managed Instances - List By Resource Group](https://docs.microsoft.com/rest/api/sql/managedinstances/listbyresourcegroup)|Devuelve una lista de instancias administradas en un grupo de recursos.|
|[Managed Instances - Update](https://docs.microsoft.com/rest/api/sql/managedinstances/update)|Actualiza una instancia administrada.|

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información sobre cómo migrar una base de datos de SQL Server a Azure, vea [Migración a Azure SQL Database](sql-database-cloud-migrate.md).
- Para obtener información sobre las características admitidas, consulte el [artículo que trata sobre dicho tema](sql-database-features.md).
