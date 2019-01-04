---
title: Administración de la retención de copias de seguridad a largo plazo de Azure SQL Database | Microsoft Docs
description: Aprenda a almacenar copias de seguridad automatizadas en el almacenamiento de SQL Azure y, después, a restaurarlas
services: sql-database
ms.service: sql-database
ms.subservice: backup-restore
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
manager: craigg
ms.date: 10/24/2018
ms.openlocfilehash: ed3203cde8bad8c506bacf3607faecfbcde2cd75
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/19/2018
ms.locfileid: "53606867"
---
# <a name="manage-azure-sql-database-long-term-backup-retention"></a>Administración de la retención de copias de seguridad a largo plazo de Azure SQL Database

En Azure SQL Database, puede configurar una base de datos sola o agrupada con una directiva de [retención de copias de seguridad a largo plazo](sql-database-long-term-retention.md) (LTR) para retener automáticamente copias de seguridad en Azure Blob Storage hasta durante 10 años. Posteriormente, puede recuperar una base de datos mediante esas copias de seguridad con Azure Portal o PowerShell.

> [!IMPORTANT]
> [Instancia administrada de Azure SQL Database](sql-database-managed-instance.md) no admite actualmente la retención de copias de seguridad a largo plazo.

## <a name="use-the-azure-portal-to-configure-long-term-retention-policies-and-restore-backups"></a>Uso de Azure Portal para configurar directivas de retención a largo plazo y restaurar las copias de seguridad
En las secciones siguientes se explica cómo usar Azure Portal para configurar la retención a largo plazo, ver las copias de seguridad incluidas en ella y restaurarlas.

### <a name="configure-long-term-retention-policies"></a>Configuración de directivas de retención a largo plazo

Puede configurar SQL Database para [ conservar las copias de seguridad automatizadas](sql-database-long-term-retention.md) durante un período superior al período de retención del nivel de servicio. 

1. En Azure Portal, seleccione el servidor SQL Server y después, haga clic en **Administrar copias de seguridad**. En la pestaña **Configurar directivas**, seleccione la casilla de la base de datos en la que desea establecer o modificar las directivas de retención de copias de seguridad a largo plazo.

   ![vínculo para administrar copias de seguridad](./media/sql-database-long-term-retention/ltr-configure-ltr.png)

2. En el panel **Configurar directivas**, seleccione si desea conservar las copias de seguridad semanales, mensuales o anuales y especifique el período de retención de cada una. 

   ![configurar directivas](./media/sql-database-long-term-retention/ltr-configure-policies.png)

3. Cuando haya terminado, haga clic en **Aplicar**.

### <a name="view-backups-and-restore-from-a-backup-using-azure-portal"></a>Visualización y restauración de copias de seguridad mediante Azure Portal

Vea las copias de seguridad que se han conservado para una base de datos concreta con una directiva LTR y realice la restauración a partir de esas copias de seguridad. 

1. En Azure Portal, seleccione el servidor SQL Server y después, haga clic en **Administrar copias de seguridad**. En la pestaña **Copias de seguridad disponibles**, seleccione la base de datos de la que desea ver las copias de seguridad disponibles.

   ![seleccionar base de datos](./media/sql-database-long-term-retention/ltr-available-backups-select-database.png)

3. En el panel **Copias de seguridad disponibles**, revise las copias de seguridad disponibles. 

   ![ver copias de seguridad](./media/sql-database-long-term-retention/ltr-available-backups.png)

4. Seleccione la copia de seguridad desde la que desea restaurar y, a continuación, especifique el nuevo nombre de la base de datos.

   ![Restauración](./media/sql-database-long-term-retention/ltr-restore.png)

5. Haga clic en **Aceptar** para restaurar la base de datos de la copia de seguridad del almacenamiento de Azure SQL en la nueva base de datos.

6. En la barra de herramientas, haga clic en el icono de notificación para ver el estado del trabajo de restauración.

   ![progreso del trabajo de restauración](./media/sql-database-get-started-backup-recovery/restore-job-progress-long-term.png)

5. Cuando se complete el trabajo de restauración, abra la página **Bases de datos SQL** para ver la base de datos recién restaurada.

> [!NOTE]
> Desde aquí, puede conectarse a la base de datos restaurada mediante SQL Server Management Studio para realizar las tareas necesarias, como [extraer un bit de datos de la base de datos restaurada para copiarlo en la base de datos existente o para eliminar la base de datos existente y cambiar el nombre de la base de datos restaurada por el nombre de la base de datos existente](sql-database-recovery-using-backups.md#point-in-time-restore).
>

## <a name="use-powershell-to-configure-long-term-retention-policies-and-restore-backups"></a>Uso de PowerShell para configurar directivas de retención a largo plazo y restaurar las copias de seguridad

En las siguientes secciones se explica cómo usar PowerShell para configurar la retención de copias de seguridad a largo plazo, ver las copias de seguridad en el almacén de Azure SQL y realizar una restauración a partir de una copia de seguridad del almacén de Azure SQL.

> [!IMPORTANT]
> La API de LTR V2 se admite en las siguientes versiones de PowerShell:
- [AzureRM.Sql-4.5.0](https://www.powershellgallery.com/packages/AzureRM.Sql/4.5.0) o más recientes
- [AzureRM-6.1.0](https://www.powershellgallery.com/packages/AzureRM/6.1.0) o más recientes
> 

### <a name="rbac-roles-to-manage-long-term-retention"></a>Roles de RBAC para administrar la retención a largo plazo

Para administrar las copias de seguridad LTR, debe ser 
- propietario de la suscripción o
- colaborador de SQL Server en el ámbito de **suscripción**, o
- colaborador de SQL Database en el ámbito de la **suscripción**.

Si se requiere un control más pormenorizado, puede crear roles de RBAC personalizados y asignarlos en el ámbito de la **suscripción**. 

En **Get-AzureRmSqlDatabaseLongTermRetentionBackup** y **Restore-AzureRmSqlDatabase**, el rol debe tener los siguientes permisos:

Microsoft.Sql/locations/longTermRetentionBackups/read Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionBackups/read Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionDatabases/longTermRetentionBackups/read
 
En **Remove-AzureRmSqlDatabaseLongTermRetentionBackup**, el rol debe tener los siguientes permisos:

Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionDatabases/longTermRetentionBackups/delete


### <a name="create-an-ltr-policy"></a>Creación de una directiva LTR

```powershell
# Get the SQL server 
# $subId = “{subscription-id}”
# $serverName = “{server-name}”
# $resourceGroup = “{resource-group-name}” 
# $dbName = ”{database-name}”

Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId $subId

# get the server
$server = Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $resourceGroup

# create LTR policy with WeeklyRetention = 12 weeks. MonthlyRetention and YearlyRetention = 0 by default.
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName -ResourceGroupName $resourceGroup -WeeklyRetention P12W 

# create LTR policy with WeeklyRetention = 12 weeks, YearlyRetetion = 5 years and WeekOfYear = 16 (week of April 15). MonthlyRetention = 0 by default.
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName -ResourceGroupName $resourceGroup -WeeklyRetention P12W -YearlyRetention P5Y -WeekOfYear 16
```

### <a name="view-ltr-policies"></a>Visualización de directivas de LTR
Este ejemplo muestra cómo enumerar las directivas de LTR de un servidor

```powershell
# Get all LTR policies within a server
$ltrPolicies = Get-AzureRmSqlDatabase -ResourceGroupName Default-SQL-WestCentralUS -ServerName trgrie-ltr-server | Get-AzureRmSqlDatabaseLongTermRetentionPolicy -Current 

# Get the LTR policy of a specific database 
$ltrPolicies = Get-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName  -ResourceGroupName $resourceGroup -Current
```
### <a name="clear-an-ltr-policy"></a>Borrado de una directiva LTR
Este ejemplo muestra cómo borrar una directiva LTR desde una base de datos

```powershell
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName -ResourceGroupName $resourceGroup -RemovePolicy
```

### <a name="view-ltr-backups"></a>Visualización de copias de seguridad de LTR

Este ejemplo muestra cómo enumerar las copias de seguridad de LTR de un servidor. 

```powershell
# Get the list of all LTR backups in a specific Azure region 
# The backups are grouped by the logical database id.
# Within each group they are ordered by the timestamp, the earliest
# backup first.  
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -Location $server.Location 

# Get the list of LTR backups from the Azure region under 
# the named server. 
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -Location $server.Location -ServerName $serverName

# Get the LTR backups for a specific database from the Azure region under the named server 
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -Location $server.Location -ServerName $serverName -DatabaseName $dbName

# List LTR backups only from live databases (you have option to choose All/Live/Deleted)
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -Location $server.Location -DatabaseState Live

# Only list the latest LTR backup for each database 
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -Location $server.Location -ServerName $serverName -OnlyLatestPerDatabase
```

### <a name="delete-ltr-backups"></a>Eliminación de copias de seguridad de LTR

En este ejemplo se muestra cómo eliminar una copia de seguridad de LTR desde la lista de copias de seguridad.

```powershell
# remove the earliest backup 
$ltrBackup = $ltrBackups[0]
Remove-AzureRmSqlDatabaseLongTermRetentionBackup -ResourceId $ltrBackup.ResourceId
```
> [!IMPORTANT]
> La eliminación de la copia de seguridad de LTR no es reversible. Puede configurar notificaciones sobre cada eliminación en Azure Monitor filtrando por la operación "Elimina una copia de seguridad de retención a largo plazo". El registro de actividad contiene información sobre quién y cuándo realizó la solicitud. Consulte [Creación de alertas del registro de actividad](../azure-monitor/platform/alerts-activity-log.md) para obtener instrucciones detalladas.
>

### <a name="restore-from-ltr-backups"></a>Restauración desde copias de seguridad de LTR
Este ejemplo muestra cómo restaurar desde una copia de seguridad de LTR. Observe que aunque esta interfaz no cambió, el parámetro ResourceId requiere ahora el identificador de recurso de la copia de seguridad de LTR. 

```powershell
# Restore LTR backup as an S3 database
Restore-AzureRmSqlDatabase -FromLongTermRetentionBackup -ResourceId $ltrBackup.ResourceId -ServerName $serverName -ResourceGroupName $resourceGroup -TargetDatabaseName $dbName -ServiceObjectiveName S3
```

> [!NOTE]
> Desde aquí, puede conectarse a la base de datos restaurada mediante SQL Server Management Studio para realizar las tareas necesarias, como extraer un bit de datos de la base de datos restaurada para copiarlo en la base de datos existente o para eliminar la base de datos existente y cambiar el nombre de la base de datos restaurada por el nombre de la base de datos existente. Consulte la [restauración a un momento dado](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="next-steps"></a>Pasos siguientes

- Para aprender sobre las copias de seguridad automáticas generadas por el servicio, consulte [copias de seguridad automáticas](sql-database-automated-backups.md)
- Para más información sobre la retención de copia de seguridad a largo plazo, consulte sobre la [retención de copia de seguridad a largo plazo](sql-database-long-term-retention.md).
