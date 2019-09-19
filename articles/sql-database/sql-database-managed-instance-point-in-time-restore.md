---
title: 'Instancia administrada de SQL Database: restauración a un momento dado | Microsoft Docs'
description: Cómo restaurar una base de datos en una instancia administrada de SQL a un momento dado anterior.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlrab, mathoma
ms.date: 08/25/2019
ms.openlocfilehash: 67f13d16dcf6bbe4fa13fe3a6e78d3e4d61e1999
ms.sourcegitcommit: 65131f6188a02efe1704d92f0fd473b21c760d08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70861999"
---
# <a name="restore-a-sql-managed-instance-database-to-a-previous-point-in-time"></a>Restauración de una base de datos de instancia administrada de SQL a un momento dado anterior

La restauración a un momento dado le permite crear una base de datos como una copia de otra base de datos en algún momento del pasado. En este artículo se describe cómo realizar una restauración a un momento dado de una base de datos en una instancia administrada.

La restauración a un momento dado se puede usar en escenarios de recuperación, como incidentes causados por errores, datos cargados incorrectamente, eliminación de datos cruciales y otros problemas, así como simplemente para fines de prueba o auditoría. En función de la configuración de la base de datos, los archivos de copia de seguridad se conservan durante un período comprendido entre 7 y 35 días.

La restauración a un momento dado se puede usar para:

- Restaurar una base de datos a partir de una base de datos existente.
- Restaurar una base de datos a partir de una base de datos eliminada.

Además, con una instancia administrada, la restauración a un momento dado se puede usar para: 

- Restaurar una base de datos a la misma instancia administrada.
- Restaurar una base de datos a otra instancia administrada.


> [!NOTE]
> No es posible la restauración a un momento dado de una instancia administrada completa. Lo que sí es posible, y es lo que se explica en este artículo, es la restauración a un momento dado de una base de datos hospedada en una instancia administrada.


## <a name="limitations"></a>Limitaciones

Al restaurar en otra instancia administrada, ambas instancias deben estar en la misma suscripción y región. Actualmente no se admite la restauración entre regiones y entre suscripciones.

> [!WARNING]
> Tenga cuidado con el tamaño de almacenamiento de la instancia administrada: según el tamaño de los datos de restauración, puede quedarse sin almacenamiento para la instancia. Si no hay suficiente espacio para los datos restaurados, utilice un enfoque alternativo.

En la tabla siguiente se muestran escenarios de recuperación a un momento dado para la instancia administrada:

|           |Restauración de una base de datos existente| Restauración de una base de datos existente|Restauración de una base de datos suprimida| Restauración de una base de datos suprimida|
|:----------|:----------|:----------|:----------|:----------|
|Destino| Misma instancia administrada|Otra instancia administrada |Misma instancia administrada|Otra instancia administrada |
|Portal de Azure| Sí|No |No|Sin|
|CLI de Azure|Sí |Sí |No|Sin|
|PowerShell| Sí|Sí |Sí|Sí|


## <a name="restore-existing-database"></a>Restauración de una base de datos existente

Restaure una base de datos existente en la misma instancia mediante Azure Portal, PowerShell o la CLI de Azure. Restaure una base de datos a otra instancia mediante PowerShell o la CLI de Azure especificando las propiedades del grupo de recursos y de la instancia administrada de destino. Si no se especifican estos parámetros, la base de datos se restaurará de forma predeterminada en la instancia existente. La restauración a otra instancia no se admite actualmente a través de la Azure Portal. 

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com). 
1. Vaya a la instancia administrada y seleccione la base de datos que quiera restaurar. 
1. Seleccione **Restaurar** en la página de la base de datos. 

    ![Restauración de una base de datos existente](media/sql-database-managed-instance-point-in-time-restore/restore-database-to-mi.png)

1. En la página **Restaurar**, seleccione el punto relativo a fecha y hora en el pasado al que quiere restaurar la base de datos.
1. Seleccione **Confirmar** para restaurar la base de datos. Esto inicia el proceso de restauración, que crea una nueva base de datos y se rellena con datos de la base de datos original en el momento deseado. Para más información sobre el tiempo de recuperación, consulte el [tiempo de recuperación](sql-database-recovery-using-backups.md#recovery-time). 

1. Busque la instancia administrada.
1. Seleccione la base de datos que quiere restaurar.
1. En la pantalla de la base de datos, haga clic en la acción de restaurar.
1. En la pantalla de restauración, seleccione la fecha y hora del punto en el pasado al que va a restaurar la base de datos.
1. Después de confirmar, se iniciará el proceso de restauración y, según el tamaño de la base de datos, se creará una nueva base de datos y se rellenará con los datos de la base de datos original en el momento deseado. Para conocer la duración del proceso de recuperación, consulte la información sobre recuperación en el artículo de copias de seguridad.


# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Si aún no tiene instalado Azure PowerShell, vea el [módulo para la instalación de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps).

Para restaurar la base de datos mediante PowerShell, actualice los parámetros con sus valores y ejecute el siguiente comando:

```powershell-interactive
$subscriptionId = "<Subscription ID>"
$resourceGroupName = "<Resource group name>"
$managedInstanceName = "<Managed instance name>"
$databaseName = "<Source-database>"
$pointInTime = "2018-06-27T08:51:39.3882806Z"
$targetDatabase = "<Name of new database to be created>"
 
Get-AzSubscription -SubscriptionId $subscriptionId
Select-AzSubscription -SubscriptionId $subscriptionId
 
Restore-AzSqlInstanceDatabase -FromPointInTimeBackup `
                              -ResourceGroupName $resourceGroupName `
                               -InstanceName $managedInstanceName `
                               -Name $databaseName `
                               -PointInTime $pointInTime `
                               -TargetInstanceDatabaseName $targetDatabase `
```

Para restaurar la base de datos en otra instancia administrada, establezca el nombre del grupo de recursos de destino y el nombre de la instancia administrada de destino.  

```powershell-interactive
$targetResourceGroupName = "<Resource group of target managed instance>"
$targetInstanceName = "<Target managed instance name>"

Restore-AzSqlInstanceDatabase -FromPointInTimeBackup `
                              -ResourceGroupName $resourceGroupName `
                              -InstanceName $managedInstanceName `
                              -Name $databaseName `
                              -PointInTime $pointInTime `
                              -TargetInstanceDatabaseName $targetDatabase `
                              -TargetResourceGroupName $targetResourceGroupName `
                              -TargetInstanceName $targetInstanceName 
```

Para obtener más información, consulte [Restore-AzSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqlinstancedatabase).


# <a name="azure-clitabazure-cli"></a>[CLI de Azure](#tab/azure-cli)

Si no ha instalado aún la CLI de Azure, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli?view=azure-cli-latest).

Para restaurar la base de datos mediante la CLI de Azure, actualice los parámetros con sus valores y ejecute el siguiente comando:


```azurecli-interactive
az sql midb restore -g mygroupname --mi myinstancename |
-n mymanageddbname --dest-name targetmidbname --time "2018-05-20T05:34:22"
```


Para restaurar la base de datos en otra instancia administrada, establezca el nombre del grupo de recursos de destino y el nombre de la instancia administrada de destino.  

```azurecli-interactive
az sql midb restore -g mygroupname --mi myinstancename -n mymanageddbname |
       --dest-name targetmidbname --time "2018-05-20T05:34:22" |
       --dest-resource-group mytargetinstancegroupname |
       --dest-mi mytargetinstancename
```

Para obtener una explicación detallada de los parámetros disponibles, consulte la información sobre la [CLI de instancia administrada](https://docs.microsoft.com/cli/azure/sql/midb?view=azure-cli-latest#az-sql-midb-restore). 

---

## <a name="restore-a-deleted-database"></a>Restauración de una base de datos eliminada 
 
La restauración de una base de datos eliminada solo se puede realizar con PowerShell. La base de datos se puede restaurar en la misma instancia o en otra. 

Para restaurar la base de datos eliminada mediante PowerShell, actualice los parámetros con sus valores y ejecute el siguiente comando:

```powershell-interactive
$subscriptionId = "<Subscription ID>"
Get-AzSubscription -SubscriptionId $subscriptionId
Select-AzSubscription -SubscriptionId $subscriptionId

$resourceGroupName = "<Resource group name>"
$managedInstanceName = "<Managed instance name>"
$deletedDatabaseName = "<Source database name>"

$deleted_db = Get-AzSqlDeletedInstanceDatabaseBackup -ResourceGroupName $resourceGroupName `
            -InstanceName $managedInstanceName -DatabaseName $deletedDatabaseName 

$pointInTime = "2018-06-27T08:51:39.3882806Z"
$properties = New-Object System.Object
$properties | Add-Member -type NoteProperty -name CreateMode -Value "PointInTimeRestore"
$properties | Add-Member -type NoteProperty -name RestorePointInTime -Value $pointInTime
$properties | Add-Member -type NoteProperty -name RestorableDroppedDatabaseId -Value $deleted_db.Id
```

Para restaurar la base de datos eliminada en otra instancia, cambie el nombre del grupo de recursos y el nombre de la instancia administrada.

El parámetro de ubicación debe coincidir con la ubicación del grupo de recursos y de la instancia administrada.

```powershell-interactive
$resourceGroupName = "<Second resource group name>"
$managedInstanceName = "<Second managed instance name>"

$location = "West Europe"

$restoredDBName = "WorldWideImportersPITR"
$resource_id = "subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Sql/managedInstances/$managedInstanceName/databases/$restoredDBName"

New-AzResource -Location $location -Properties $properties `
        -ResourceId $resource_id -ApiVersion "2017-03-01-preview" -Force
```

## <a name="overwrite-existing-database"></a>Sobrescritura de base de datos existente 
 
Para sobrescribir una base de datos existente, también debe:

1. QUITAR la base de datos existente que desea sobrescribir.
1. Cambiar el nombre de la base de datos restaurada en un momento dado al nombre de la base de datos que se quitó. 


### <a name="drop-original-database"></a>QUITAR la base de datos original. 
 
La eliminación de la base de datos se puede realizar con Azure Portal, PowerShell o la CLI de Azure. 

También puede quitar la base de datos conectándose directamente a la instancia administrada, iniciando SQL Server Management Studio (SSMS) y ejecutando el siguiente comando de Transact-SQL (T-SQL).

```sql
DROP DATABASE WorldWideImporters;
```

Use uno de los métodos siguientes para conectarse a su base de datos de instancia administrada: 

- [Máquina virtual SQL](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-vm)
- [De punto a sitio](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-p2s)
- [Punto de conexión público](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-public-endpoint-configure)

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

En Azure Portal, seleccione la base de datos de la instancia administrada y seleccione **Eliminar**.

   ![Restauración de una base de datos existente](media/sql-database-managed-instance-point-in-time-restore/delete-database-from-mi.png)

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Use el siguiente comando de PowerShell para quitar una base de datos existente de una instancia administrada: 

```powershell
$resourceGroupName = "<Resource group name>"
$managedInstanceName = "<Managed instance name>"
$databaseName = "<Source database>"

Remove-AzSqlInstanceDatabase -Name $databaseName -InstanceName $managedInstanceName -ResourceGroupName $resourceGroupName
```

# <a name="azure-clitabazure-cli"></a>[CLI de Azure](#tab/azure-cli)

Use el siguiente comando de la CLI de Azure para quitar una base de datos existente de una instancia administrada: 

```azurecli-interactive
az sql midb delete -g mygroupname --mi myinstancename -n mymanageddbname
```

---


### <a name="alter-new-database-name-to-original"></a>CAMBIE el nuevo nombre de la base de datos al original

Conéctese directamente a la instancia administrada, inicie SQL Server Management Studio y, a continuación, ejecute la siguiente consulta de Transact-SQL (T-SQL) para cambiar el nombre de la base de datos restaurada al de la base de datos eliminada que desea sobrescribir. 


```sql
ALTER WorldWideImportersPITR MODIFY NAME = WorldWideImporters;
```


Use uno de los métodos siguientes para conectarse a su base de datos de instancia administrada: 

- [Máquina virtual SQL](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-vm)
- [De punto a sitio](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-p2s)
- [Punto de conexión público](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-public-endpoint-configure)

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre la [retención a largo plazo](sql-database-long-term-retention.md) y las [copias de seguridad automatizadas](sql-database-automated-backups.md). 
