---
title: Restauración de una instancia de Azure SQL Data Warehouse existente | Microsoft Docs
description: Guía de procedimientos para restaurar una instancia de Azure SQL Data Warehouse existente.
services: sql-data-warehouse
author: anumjs
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 08/29/2018
ms.author: anjangsh
ms.reviewer: igorstan
ms.openlocfilehash: 7470dd96109823057a174d2ecf097583dcb51898
ms.sourcegitcommit: 5ded08785546f4a687c2f76b2b871bbe802e7dae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/19/2019
ms.locfileid: "69575274"
---
# <a name="restore-an-existing-azure-sql-data-warehouse"></a>Restauración de una instancia de Azure SQL Data Warehouse existente

En este artículo, aprenderá a restaurar una instancia de Azure SQL Data Warehouse existente mediante Azure Portal y PowerShell:

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

**Compruebe la capacidad DTU**. Cada instancia de SQL Data Warehouse está hospedada en un servidor SQL Server (por ejemplo, myserver.database.windows.net) que tiene una cuota de DTU predeterminada. Compruebe que el servidor de SQL Server tiene suficiente cuota de DTU restante para la base de datos que se va a restaurar. Para más información sobre cómo calcular la unidad DTU necesaria o solicitar más DTU, consulte cómo [solicitar un cambio en la cuota de DTU][Request a DTU quota change].

## <a name="before-you-begin"></a>Antes de empezar

1. Asegúrese de [instalar Azure PowerShell][Install Azure PowerShell].
2. Tenga un punto de restauración existente desde el que desee realizar la restauración. Si desea crear una restauración, consulte [el tutorial para crear un de restauración definido por el usuario][the tutorial to create a new user-defined restore point].

## <a name="restore-an-existing-data-warehouse-through-powershell"></a>Restauración de una base de datos de almacenamiento de datos existente mediante PowerShell

Para restaurar una base de datos de almacenamiento de datos existente a partir de un punto de restauración, use el cmdlet [Restore-AzSqlDatabase][Restore-AzSqlDatabase] de PowerShell.

1. Abra PowerShell.

2. Conéctese a su cuenta de Azure y enumere todas las suscripciones asociadas a su cuenta.

3. Seleccione la suscripción que contiene la base de datos que se va a restaurar.

4. Enumere los puntos de restauración de la base de datos de almacenamiento de datos.

5. Elija el punto de restauración deseado mediante RestorePointCreationDate.

6. Restaure la base de datos de almacenamiento de datos al punto de restauración deseado mediante el comando [Restore-AzSqlDatabase][Restore-AzSqlDatabase] de PowerShell.
        1. Para restaurar la instancia de SQL Data Warehouse en un servidor lógico diferente, asegúrese de especificar el nombre del otro servidor lógico.  Este servidor lógico también puede estar en un grupo de recursos y una región diferentes.
        2. Para realizar la restauración en otra suscripción, use el botón Mover para mover el servidor lógico a otra suscripción.

7. Compruebe que la base de datos de almacenamiento de datos restaurada está en línea.

8. Una vez finalizada la restauración, puede configurar la base de datos de almacenamiento de datos recuperada siguiendo la explicación que se proporciona en el apartado [Configuración de la base de datos después de realizar la recuperación][Configure your database after recovery].

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
#$TargetResourceGroupName="<YourTargetResourceGroupName>" # uncomment to restore to a different logical server.
#$TargetServerName="<YourtargetServerNameWithoutURLSuffixSeeNote>"  
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Connect-AzAccount
Get-AzSubscription
Select-AzSubscription -SubscriptionName $SubscriptionName

# Or list all restore points
Get-AzSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Get the specific database to restore
$Database = Get-AzSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Pick desired restore point using RestorePointCreationDate "xx/xx/xxxx xx:xx:xx xx"
$PointInTime="<RestorePointCreationDate>"  

# Restore database from a restore point
$RestoredDatabase = Restore-AzSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Use the following command to restore to a different logical server
#$RestoredDatabase = Restore-AzSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $Database.ResourceTargetGroupName -ServerName $TargetServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Verify the status of restored database
$RestoredDatabase.status

```

## <a name="restore-an-existing-data-warehouse-through-the-azure-portal"></a>Restauración de una base de datos de almacenamiento de datos existente mediante Azure Portal

1. Inicie sesión en el [Azure Portal][Azure portal].
2. Vaya a la instancia de SQL Data Warehouse desde la que desea realizar la restauración.
3. En la parte superior de la hoja de información general, seleccione **Restaurar**.

    ![ Introducción a la restauración](./media/sql-data-warehouse-restore-active-paused-dw/restoring-01.png)

4. Seleccione **Puntos de restauración automáticos** o **Puntos de restauración definidos por el usuario**. Si la base de datos de almacenamiento de datos no tiene ningún punto de restauración automática, espere unas horas o cree un punto de restauración definido por el usuario antes de llevar a cabo la restauración. En el caso de los puntos de restauración definidos por el usuario, seleccione uno existente o cree uno nuevo. En **Servidor**, puede elegir un servidor lógico de otro grupo de recursos y región, o bien crear uno nuevo. Después de proporcionar todos los parámetros, haga clic en **Revisar y restaurar**.

    ![Puntos de restauración automáticos](./media/sql-data-warehouse-restore-active-paused-dw/restoring-11.png)

## <a name="next-steps"></a>Pasos siguientes
- [Restauración de un almacén de datos eliminado][Restore a deleted data warehouse]
- [Restauración desde un almacenamiento de datos de copia de seguridad geográfica][Restore from a geo-backup data warehouse]
 
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
[the tutorial to create a new user-defined restore point]:../sql-data-warehouse/sql-data-warehouse-restore-points.md
[Install Azure PowerShell]: https://docs.microsoft.com/powershell/azure/overview
[Restore an existing data warehouse]:./sql-data-warehouse-restore-active-paused-dw.md
[Restore a deleted data warehouse]:./sql-data-warehouse-restore-deleted-dw.md
[Restore from a geo-backup data warehouse]:./sql-data-warehouse-restore-from-geo-backup.md

<!--MSDN references-->
[Restore-AzSqlDatabase]: https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
