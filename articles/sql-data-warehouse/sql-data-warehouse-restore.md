---
title: Restauración de una instancia de Azure SQL Data Warehouse | Microsoft Docs
description: Guía para restaurar una instancia de Azure SQL Data Warehouse.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 08/29/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: ebbcbcc3d0934800980b7d8e00895b11ff2747b7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60310441"
---
# <a name="restoring-azure-sql-data-warehouse"></a>Restauración de una instancia de Azure SQL Data Warehouse 
En este artículo, aprenderá cómo hacer lo siguiente en Azure Portal y PowerShell:

- Crear un punto de restauración
- Realizar una restauración a partir de un punto de restauración automático o un punto de restauración definido por el usuario
- Realizar una restauración a partir de una base de datos eliminada
- Realizar una restauración a partir de una copia de seguridad de replicación geográfica
- Crear una copia del almacenamiento de datos desde un punto de restauración definido por el usuario

## <a name="before-you-begin"></a>Antes de empezar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

**Compruebe la capacidad DTU**. Cada instancia de SQL Data Warehouse está hospedada en un servidor SQL Server (p. ej., myserver.database.windows.net) que tiene una cuota de DTU predeterminada.  Para poder restaurar una instancia de SQL Data Warehouse, compruebe que su servidor SQL Server tiene suficiente cuota de DTU restante para la base de datos en proceso de restauración. Para obtener más información sobre cómo calcular la unidad DTU necesaria o solicitar más DTU, consulte cómo [solicitar un cambio en la cuota de DTU][Request a DTU quota change].

## <a name="restore-through-powershell"></a>Restauración mediante PowerShell

## <a name="install-powershell"></a>Instale PowerShell.
Para poder usar Azure PowerShell con SQL Data Warehouse, deberá instalar Azure PowerShell.  Puede comprobar la versión ejecutando **Get-Module - ListAvailable-nombre de Az**. Para más información sobre cómo instalar la versión más reciente, consulte [Cómo instalar y configurar Azure PowerShell][How to install and configure Azure PowerShell].

## <a name="restore-an-active-or-paused-database-using-powershell"></a>Restauración de una base de datos activa o en pausa mediante PowerShell
Para restaurar una base de datos desde un punto de restauración, use el [restauración AzSqlDatabase] [ Restore-AzSqlDatabase] cmdlet de PowerShell.

1. Abra Windows PowerShell.

2. Conéctese a su cuenta de Azure y enumere todas las suscripciones asociadas a su cuenta.

3. Seleccione la suscripción que contiene la base de datos que se va a restaurar.

4. Enumere los puntos de restauración de la base de datos.

5. Elija el punto de restauración deseado mediante RestorePointCreationDate.

   > [!NOTE]
   > Al realizar la restauración, puede especificar otro ServiceObjectiveName (DWU) u otro servidor que resida en otra región.

6. Restaure la base de datos al punto de restauración deseado.

7. Compruebe que la base de datos restaurada está en línea.

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Connect-AzAccount
Get-AzSubscription
Select-AzSubscription -SubscriptionName $SubscriptionName

# List the last 10 database restore points
((Get-AzSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName ($DatabaseName)).RestorePointCreationDate)[-10 .. -1]

# Or list all restore points
Get-AzSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Get the specific database to restore
$Database = Get-AzSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Pick desired restore point using RestorePointCreationDate
$PointInTime="<RestorePointCreationDate>"  

# Restore database from a restore point
$RestoredDatabase = Restore-AzSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Verify the status of restored database
$RestoredDatabase.status

```

> [!NOTE]
> Una vez finalizada la restauración, puede configurar la base de datos recuperada siguiendo la guía [Configuración de la base de datos después de realizar la recuperación][Configure your database after recovery].
>

## <a name="copy-your-data-warehouse-with-user-defined-restore-points-using-powershell"></a>Creación de una copia del almacenamiento de datos con puntos de restauración definidos por el usuario mediante PowerShell
Para restaurar una base de datos desde un punto de restauración definido por el usuario, use el [restauración AzSqlDatabase] [ Restore-AzSqlDatabase] cmdlet de PowerShell.

1. Abra Windows PowerShell.
2. Conéctese a su cuenta de Azure y enumere todas las suscripciones asociadas a su cuenta.
3. Seleccione la suscripción que contiene la base de datos que se va a restaurar.
4. Cree un punto de restauración para realizar una copia inmediata de la base de datos.
5. Cambie el nombre de la base de datos a un nombre temporal.
6. Recupere el punto de restauración más reciente mediante el RestorePointLabel especificado.
7. Obtenga el identificador del recurso de la base de datos para iniciar la restauración
8. Restaure la base de datos al punto de restauración deseado.
9. Compruebe que la base de datos restaurada está en línea.

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$TempDatabaseName = "<YourTemporaryDatabaseName>"
$Label = "<YourRestorePointLabel"

Connect-AzAccount
Get-AzSubscription
Select-AzSubscription -SubscriptionName $SubscriptionName

# Create a restore point of the original database
New-AzSqlDatabaseRestorePoint -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName -RestorePointLabel $Label

# Rename the database to a temporary name
Set-AzSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName -NewName $TempDatabaseName

# Get the most recent restore point with the specified label
$LabelledRestorePoint = Get-AzSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $TempDatabaseName | where {$_.RestorePointLabel -eq $Label} | sort {$_.RestorePointCreationDate} | select -Last 1

# Get the resource id of the database
$ResourceId = (Get-AzSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $TempDatabaseName).ResourceId

# Restore the database to its original name from the labelled restore point from the temporary database
$RestoredDatabase = Restore-AzSqlDatabase -FromPointInTimeBackup -ResourceGroupName $ResourceGroupName -ServerName $ServerName -ResourceId $ResourceId -PointInTime $LabelledRestorePoint.RestorePointCreationDate -TargetDatabaseName $DatabaseName

# Verify the status of restored database
$RestoredDatabase.status

# The original temporary database can be deleted at this point

```

## <a name="restore-a-deleted-database-using-powershell"></a>Uso de PowerShell para restaurar una base de datos eliminada
Para restaurar una base de datos eliminada, use el [restauración AzSqlDatabase] [ Restore-AzSqlDatabase] cmdlet.

1. Abra Windows PowerShell.
2. Conéctese a su cuenta de Azure y enumere todas las suscripciones asociadas a su cuenta.
3. Seleccione la suscripción que contiene la base de datos eliminada que se va a restaurar.
4. Obtenga la base de datos eliminada específica.
5. Restaure la base de datos eliminada.
6. Compruebe que la base de datos restaurada está en línea.

```Powershell
$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Connect-AzAccount
Get-AzSubscription
Select-AzSubscription -SubscriptionName $SubscriptionName

# Get the deleted database to restore
$DeletedDatabase = Get-AzSqlDeletedDatabaseBackup -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Restore deleted database
$RestoredDatabase = Restore-AzSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $DeletedDatabase.ResourceID

# Verify the status of restored database
$RestoredDatabase.status
```

> [!NOTE]
> Una vez finalizada la restauración, puede configurar la base de datos recuperada siguiendo la guía [Configuración de la base de datos después de realizar la recuperación][Configure your database after recovery].
>

## <a name="restore-from-an-azure-geographical-region-using-powershell"></a>Uso de PowerShell para restaurar desde una región geográfica de Azure
Para recuperar una base de datos, use el [restauración AzSqlDatabase] [ Restore-AzSqlDatabase] cmdlet.

> [!NOTE]
> Puede realizar una restauración geográfica en Gen2. Para ello, especifique Gen2 ServiceObjectiveName (p. ej., DW1000**c**) como parámetro opcional.
>

1. Abra Windows PowerShell.
2. Conéctese a su cuenta de Azure y enumere todas las suscripciones asociadas a su cuenta.
3. Seleccione la suscripción que contiene la base de datos que se va a restaurar.
4. Obtenga la base de datos que desea recuperar.
5. Cree la solicitud de recuperación para la base de datos.
6. Compruebe el estado de la base de datos restaurada geográficamente.

```Powershell
Connect-AzAccount
Get-AzSubscription
Select-AzSubscription -SubscriptionName "<Subscription_name>"

# Get the database you want to recover
$GeoBackup = Get-AzSqlDatabaseGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>"

# Recover database
$GeoRestoredDatabase = Restore-AzSqlDatabase –FromGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourTargetServer>" -TargetDatabaseName "<NewDatabaseName>" –ResourceId $GeoBackup.ResourceID -ServiceObjectiveName "<YourTargetServiceLevel>"

# Verify that the geo-restored database is online
$GeoRestoredDatabase.status
```

> [!NOTE]
> Para configurar la base de datos una vez finalizada la restauración, consulte [Configuración de la base de datos después de realizar la recuperación][Configure your database after recovery].
>

La base de datos recuperada estará habilitada para TDE si la base de datos de origen está habilitada para TDE.

## <a name="restore-through-the-azure-portal"></a>Restauración mediante Azure Portal

## <a name="create-a-user-defined-restore-point-using-the-azure-portal"></a>Uso de Azure Portal para crear un punto de restauración definido por el usuario
1. Inicie sesión en [Azure Portal][Azure portal].

2. Vaya a la instancia de SQL Data Warehouse para el que desea crear un punto de restauración.

3. En la parte superior de la hoja de información general, seleccione **+ Nuevo punto de restauración**.

    ![Nuevo punto de restauración](./media/sql-data-warehouse-restore-database-portal/creating_restore_point_0.png)

4. Especifique un nombre para el punto de restauración.

    ![Nombre de punto de restauración](./media/sql-data-warehouse-restore-database-portal/creating_restore_point_1.png)

## <a name="restore-an-active-or-paused-database-using-the-azure-portal"></a>Uso de Azure Portal para restaurar una base de datos activa o en pausa
1. Inicie sesión en [Azure Portal][Azure portal].
2. Vaya a la instancia de SQL Data Warehouse desde la que desea realizar la restauración.
3. En la parte superior de la hoja de información general, seleccione **Restaurar**.

    ![ Introducción a la restauración](./media/sql-data-warehouse-restore-database-portal/restoring_0.png)

4. Seleccione **Puntos de restauración automáticos** o **Puntos de restauración definidos por el usuario**.

    ![Puntos de restauración automáticos](./media/sql-data-warehouse-restore-database-portal/restoring_1.png)

5. En el caso de los puntos de restauración definidos por el usuario, **seleccione un punto de restauración** o **cree un punto de restauración definido por el usuario**.

    ![Puntos de restauración definidos por el usuario](./media/sql-data-warehouse-restore-database-portal/restoring_2_udrp.png)

## <a name="restore-a-deleted-database-using-the-azure-portal"></a>Uso de Azure Portal para restaurar una base de datos eliminada
1. Inicie sesión en [Azure Portal][Azure portal].
2. Vaya al servidor de SQL Server en el que estaba alojada la base de datos eliminada.
3. Seleccione el icono Bases de datos eliminadas en la tabla de contenido.

    ![Bases de datos eliminadas](./media/sql-data-warehouse-restore-database-portal/restoring_deleted_0.png)

4. Seleccione la base de datos eliminada que desea restaurar.

    ![Seleccionar bases de datos eliminadas](./media/sql-data-warehouse-restore-database-portal/restoring_deleted_1.png)

5. Especifique un nombre de base de datos nuevo.

    ![Especificar nombre de base de datos](./media/sql-data-warehouse-restore-database-portal/restoring_deleted_2.png)

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Request a DTU quota change]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[How to install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Overview]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery

<!--MSDN references-->
[Restore-AzSqlDatabase]: https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
