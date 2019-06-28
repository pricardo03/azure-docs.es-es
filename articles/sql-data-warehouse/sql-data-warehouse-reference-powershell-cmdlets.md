---
title: Cmdlets de PowerShell para Azure SQL Data Warehouse
description: Busque los principales cmdlets de PowerShell para Azure SQL Data Warehouse, incluidos aquellos para pausar y reanudar una base de datos.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 095e66c6c5f75a27b1f0231dfe8cabfd4d741d18
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65205173"
---
# <a name="powershell-cmdlets-and-rest-apis-for-sql-data-warehouse"></a>Cmdlets de PowerShell y API de REST para SQL Data Warehouse
Muchas tareas de administración de SQL Data Warehouse se pueden administrar mediante los cmdlets de Azure PowerShell o las API de REST.  A continuación se muestran algunos ejemplos de cómo usar comandos de PowerShell para automatizar tareas comunes en SQL Data Warehouse.  Para ver algunos buenos ejemplos de REST, consulte el artículo [Administración de la potencia de proceso en SQL Data Warehouse de Azure (REST)][Manage scalability with REST].

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="get-started-with-azure-powershell-cmdlets"></a>Introducción a los cmdlets de Azure PowerShell
1. Abra Windows PowerShell.
2. En el símbolo del sistema de PowerShell, ejecute estos comandos para iniciar sesión en Azure Resource Manager y seleccione su suscripción.
   
    ```powershell
    Connect-AzAccount
    Get-AzSubscription
    Select-AzSubscription -SubscriptionName "MySubscription"
    ```

## <a name="pause-sql-data-warehouse-example"></a>Ejemplo de pausa de SQL Data Warehouse
Pausa una base de datos denominada "Database02" que está hospedada en un servidor cuyo nombre es "Server01".  El servidor está en un grupo de recursos de Azure denominado "ResourceGroup1."

```Powershell
Suspend-AzSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
```
Este ejemplo, que es una variación, canaliza el objeto recuperado a [Suspend-AzSqlDatabase][Suspend-AzSqlDatabase].  El resultado es que se pausa la base de datos. El comando final muestra los resultados.

```Powershell
$database = Get-AzSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzSqlDatabase
$resultDatabase
```

## <a name="start-sql-data-warehouse-example"></a>Ejemplo de inicio de SQL Data Warehouse
Reanuda el funcionamiento de una base de datos denominada "Database02" que está hospedada en un servidor cuyo nombre es "Server01". El servidor está en un grupo de recursos denominado "ResourceGroup1."

```Powershell
Resume-AzSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" -DatabaseName "Database02"
```

Este ejemplo, que es una variación, recupera una base de datos denominada "Database02" desde un servidor cuyo nombre es "Server01" que está incluido en un grupo de recursos denominado "ResourceGroup1". Canaliza el objeto recuperado a [Resume-AzSqlDatabase][Resume-AzSqlDatabase].

```Powershell
$database = Get-AzSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzSqlDatabase
```

> [!NOTE]
> Tenga en cuenta que si el servidor es foo.database.windows.net, debe usar "foo" como -ServerName en los cmdlets de Powershell.
> 
> 

## <a name="other-supported-powershell-cmdlets"></a>Otros cmdlets de PowerShell admitidos
Estos cmdlets de PowerShell se admiten en Azure SQL Data Warehouse.

* [Get-AzSqlDatabase][Get-AzSqlDatabase]
* [Get-AzSqlDeletedDatabaseBackup][Get-AzSqlDeletedDatabaseBackup]
* [Get-AzSqlDatabaseRestorePoint][Get-AzSqlDatabaseRestorePoint]
* [New-AzSqlDatabase][New-AzSqlDatabase]
* [Remove-AzSqlDatabase][Remove-AzSqlDatabase]
* [Restore-AzSqlDatabase][Restore-AzSqlDatabase]
* [Resume-AzSqlDatabase][Resume-AzSqlDatabase]
* [Select-AzSubscription][Select-AzSubscription]
* [Set-AzSqlDatabase][Set-AzSqlDatabase]
* [Suspend-AzSqlDatabase][Suspend-AzSqlDatabase]

## <a name="next-steps"></a>Pasos siguientes
Para obtener más ejemplos de PowerShell, consulte:

* [Creación de SQL Data Warehouse con PowerShell][Create a SQL Data Warehouse using PowerShell]
* [Restauración de base de datos][Database restore]

Para conocer otras tareas que se pueden automatizar con PowerShell, consulte [Cmdlets de Azure SQL Database][Azure SQL Database Cmdlets]. Tenga en cuenta que no todos los cmdlets de Azure SQL Database se admiten para Azure SQL Data Warehouse.  Para ver una lista de todas las tareas que se pueden automatizar con PowerShell, consulte [Operaciones para Azure SQL Database][Operations for Azure SQL Database].

<!--Image references-->

<!--Article references-->
[How to install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Create a SQL Data Warehouse using PowerShell]: ./create-data-warehouse-powershell.md
[Database restore]: ./sql-data-warehouse-restore-database-powershell.md
[Manage scalability with REST]: ./sql-data-warehouse-manage-compute-rest-api.md

<!--MSDN references-->
[Azure SQL Database Cmdlets]: https://docs.microsoft.com/powershell/module/az.sql
[Operations for Azure SQL Database]: https://msdn.microsoft.com/library/azure/dn505719.aspx
[Get-AzSqlDatabase]: https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabase
[Get-AzSqlDeletedDatabaseBackup]: https://docs.microsoft.com/powershell/module/az.sql/get-azsqldeleteddatabasebackup
[Get-AzSqlDatabaseRestorePoint]: https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabaserestorepoint
[New-AzSqlDatabase]: https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabase
[Remove-AzSqlDatabase]: https://docs.microsoft.com/powershell/module/az.sql/remove-azsqldatabase
[Restore-AzSqlDatabase]: https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase
[Resume-AzSqlDatabase]: https://docs.microsoft.com/powershell/module/az.sql/resume-azsqldatabase
<!-- It appears that Select-AzSubscription isn't documented, so this points to Select-AzureSubscription -->
[Select-AzSubscription]: https://msdn.microsoft.com/library/dn722499.aspx
[Set-AzSqlDatabase]: https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase
[Suspend-AzSqlDatabase]: https://docs.microsoft.com/powershell/module/az.sql/suspend-azsqldatabase

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
