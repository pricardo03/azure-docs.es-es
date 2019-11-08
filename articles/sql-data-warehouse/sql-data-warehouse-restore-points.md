---
title: Puntos de restauración definidos por el usuario
description: Creación de un punto de restauración para Azure SQL Data Warehouse.
services: sql-data-warehouse
author: anumjs
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 07/03/2019
ms.author: anjangsh
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 61ceb2f1271d085321215aff1c6d138feb95d743
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73692500"
---
# <a name="user-defined-restore-points"></a>Puntos de restauración definidos por el usuario

En este artículo, aprenderá a crear un punto de restauración definido por el usuario para Azure SQL Data Warehouse mediante PowerShell y Azure Portal.

## <a name="create-user-defined-restore-points-through-powershell"></a>Creación de puntos de restauración definidos por el usuario mediante PowerShell

Para crear un punto de restauración definido por el usuario, utilice el cmdlet de PowerShell [New-AzSqlDatabaseRestorePoint][New-AzSqlDatabaseRestorePoint].

1. Antes de empezar, asegúrese de [instalar Azure PowerShell][Install Azure PowerShell].
2. Abra PowerShell.
3. Conéctese a su cuenta de Azure y enumere todas las suscripciones asociadas a su cuenta.
4. Seleccione la suscripción que contiene la base de datos que se va a restaurar.
5. Cree un punto de restauración para realizar una copia inmediata de la base de datos de almacenamiento de datos.

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$Label = "<YourRestorePointLabel>"

Connect-AzAccount
Get-AzSubscription
Select-AzSubscription -SubscriptionName $SubscriptionName

# Create a restore point of the original database
New-AzSqlDatabaseRestorePoint -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName -RestorePointLabel $Label

```

6. Consulte la lista de todos los puntos de restauración existentes.

```Powershell
# List all restore points
Get-AzSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName
```

## <a name="create-user-defined-restore-points-through-the-azure-portal"></a>Creación de puntos de restauración definidos por el usuario mediante Azure Portal

Los puntos de restauración definidos por el usuario también se pueden crear mediante Azure Portal.

1. Inicie sesión en su cuenta de [Azure Portal][Azure portal].

2. Vaya a la instancia de SQL Data Warehouse para la que desea crear un punto de restauración.

3. Seleccione **Información general** en el panel izquierdo y seleccione **Nuevo punto de restauración**. Si el botón Nuevo punto de restauración no está habilitado, asegúrese de que la base de datos de almacenamiento de datos no está en pausa.

    ![Nuevo punto de restauración](./media/sql-data-warehouse-restore-points/creating-restore-point-01.png)

4. Especifique el nombre del punto de restauración definido por el usuario y haga clic en **Aplicar**. Los puntos de restauración definidos por el usuario tienen un período de retención predeterminado de siete días.

    ![Nombre de punto de restauración](./media/sql-data-warehouse-restore-points/creating-restore-point-11.png)

## <a name="next-steps"></a>Pasos siguientes

- [Restauración de un almacenamiento de datos existente][Restore an existing data warehouse]
- [Restauración de un almacén de datos eliminado][Restore a deleted data warehouse]
- [Restauración desde un almacenamiento de datos de copia de seguridad geográfica][Restore from a geo-backup data warehouse]

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Request a DTU quota change]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[Install Azure PowerShell]: https://docs.microsoft.com/powershell/azure/overview
[Overview]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[PowerShelldoc]:./sql-data-warehouse-restore-points.md#create-user-defined-restore-points-through-powershell
[Restore an existing data warehouse]:./sql-data-warehouse-restore-active-paused-dw.md
[Restore a deleted data warehouse]:./sql-data-warehouse-restore-deleted-dw.md
[Restore from a geo-backup data warehouse]:./sql-data-warehouse-restore-from-geo-backup.md
<!--MSDN references-->
[New-AzSqlDatabaseRestorePoint]: https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabaserestorepoint?view=azps-2.4.0

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
