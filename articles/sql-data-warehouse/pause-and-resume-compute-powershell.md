---
title: 'Inicio rápido: Pausar y reanudar un proceso en Azure SQL Data Warehouse (PowerShell) | Microsoft Docs'
description: Use PowerShell para pausar un proceso en Azure SQL Data Warehouse con la finalidad de ahorrar costos. Reanude el proceso cuando esté listo para usar el almacenamiento de datos.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: quickstart
ms.subservice: manage
ms.date: 03/20/2019
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: fe9cd6c951f9eba73cee1bea66df88f3143859b9
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "58846838"
---
# <a name="quickstart-pause-and-resume-compute-in-azure-sql-data-warehouse-with-powershell"></a>Inicio rápido: Pausar y reanudar un proceso en Azure SQL Data Warehouse con PowerShell

Use PowerShell para pausar un proceso en Azure SQL Data Warehouse con la finalidad de ahorrar costos. [Reanude el proceso](sql-data-warehouse-manage-compute-overview.md) cuando esté listo para usar el almacenamiento de datos.

Si no tiene una suscripción a Azure, cree una cuenta [gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="before-you-begin"></a>Antes de empezar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

En este inicio rápido se da por supuesto que ya tiene una instancia de SQL Data Warehouse que puede pausar y reanudar. Si tiene que crear una, puede usar [Crear y conectar: Portal](create-data-warehouse-portal.md) para crear un almacenamiento de datos denominado **mySampleDataWarehouse**.

## <a name="log-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en la suscripción de Azure con el comando [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount) y siga las instrucciones de la pantalla.

```powershell
Connect-AzAccount
```

Para ver qué suscripción está usando, ejecute [Get-AzSubscription](/powershell/module/az.accounts/get-azsubscription).

```powershell
Get-AzSubscription
```

Si necesita usar una suscripción diferente de la predeterminada, ejecute [Set-AzContext](/powershell/module/az.accounts/set-azcontext).

```powershell
Set-AzContext -SubscriptionName "MySubscription"
```

## <a name="look-up-data-warehouse-information"></a>Buscar información del almacenamiento de datos

Busque el nombre de la base de datos, el nombre del servidor y el grupo de recursos para el almacenamiento de datos que tiene previsto pausar y reanudar.

Siga estos pasos para buscar información de ubicación para el almacenamiento de datos.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).
2. En la página izquierda de Azure Portal, haga clic en **Bases de datos SQL**.
3. Seleccione **mySampleDataWarehouse** en la página **Bases de datos SQL**. Se abre el almacenamiento de datos.

    ![Nombre del servidor y grupo de recursos](media/pause-and-resume-compute-powershell/locate-data-warehouse-information.png)

4. Anote el nombre del almacenamiento de datos, que es el nombre de la base de datos. Además, anote el nombre del servidor y el grupo de recursos.
6. Si su servidor es foo.database.windows.net, use solo la primera parte como nombre de servidor en los cmdlets de PowerShell. En la imagen anterior, el nombre completo del servidor es newserver-20171113.database.windows.net. Elimine el sufijo y use **newserver-20171113** como nombre del servidor en el cmdlet de PowerShell.

## <a name="pause-compute"></a>Pausa del proceso

Para ahorrar costos, puede pausar y reanudar recursos de proceso a petición. Por ejemplo, si no va a usar la base de datos durante la noche y los fines de semana, puede pausarla durante esas horas y reanudarla durante el día. No se le cobrará por recursos de proceso mientras la base de datos se encuentre en pausa. Sin embargo, se le seguirá cobrando por el almacenamiento.

Para pausar una base de datos, use el cmdlet [Suspend-AzSqlDatabase](/powershell/module/az.sql/suspend-azsqldatabase). En el ejemplo siguiente, se pausa un almacenamiento de datos denominado **mySampleDataWarehouse** hospedado en un servidor denominado **newserver 20171113**. El servidor está en un grupo de recursos de Azure denominado **myResourceGroup**.


```Powershell
Suspend-AzSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "newserver-20171113" –DatabaseName "mySampleDataWarehouse"
```

En el ejemplo siguiente, que es una variación, se recupera la base de datos en el objeto $database. A continuación, canaliza el objeto a [Suspend-AzSqlDatabase](/powershell/module/az.sql/suspend-azsqldatabase). Los resultados se almacenan en el objeto resultDatabase. El comando final muestra los resultados.

```Powershell
$database = Get-AzSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "newserver-20171113" –DatabaseName "mySampleDataWarehouse"
$resultDatabase = $database | Suspend-AzSqlDatabase
$resultDatabase
```


## <a name="resume-compute"></a>Reanudación del proceso

Para iniciar una base de datos, use el cmdlet [Resume-AzSqlDatabase](/powershell/module/az.sql/resume-azsqldatabase). En el siguiente ejemplo se inicia una base de datos denominada mySampleDataWarehouse que está hospedada en un servidor denominado newserver-20171113. El servidor está en un grupo de recursos de Azure denominado myResourceGroup.

```Powershell
Resume-AzSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "newserver-20171113" -DatabaseName "mySampleDataWarehouse"
```

En el ejemplo siguiente, que es una variación, se recupera la base de datos en el objeto $database. A continuación, canaliza el objeto a [Resume-AzSqlDatabase](/powershell/module/az.sql/resume-azsqldatabase) y almacena los resultados en $resultDatabase. El comando final muestra los resultados.

```Powershell
$database = Get-AzSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzSqlDatabase
$resultDatabase
```

## <a name="check-status-of-your-data-warehouse-operation"></a>Compruebe el estado de la operación de almacenamiento de datos

Para comprobar el estado del almacenamiento de datos, use el cmdlet [Get-AzSqlDatabaseActivity](https://docs.microsoft.com/powershell/module/az.sql/Get-AzSqlDatabaseActivity#description).

```
Get-AzSqlDatabaseActivity -ResourceGroupName "ResourceGroup01" -ServerName "Server01" -DatabaseName "Database02"
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Se le está cobrando por unidades de almacenamiento de datos y por los datos almacenados en el almacenamiento de datos. Estos recursos de proceso y de almacenamiento se facturan por separado.

- Si quiere conservar los datos de almacenamiento, pause el proceso.
- Si desea quitar cobros futuros, puede eliminar el almacenamiento de datos.

Siga estos pasos para limpiar los recursos según estime oportuno.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y haga clic en el almacenamiento de datos.

    ![Limpieza de recursos](media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

2. Para pausar el proceso, haga clic en el botón **Pausar**. Cuando el almacenamiento de datos se haya puesto en pausa, verá un botón **Iniciar**.  Para reanudar el proceso, haga clic en **Iniciar**.

3. Para quitar el almacenamiento de datos para que no se le cobre por proceso o almacenamiento, haga clic en **Eliminar**.

4. Para quitar el servidor SQL que creó, haga clic en **mynewserver-20171113.database.windows.net** y, luego, haga clic en **Eliminar**.  Debe tener cuidado con este procedimiento, ya que la eliminación del servidor elimina también todas las bases de datos asignadas al servidor.

5. Para quitar el grupo de recursos, haga clic en **myResourceGroup** y luego haga clic en **Eliminar grupo de recursos**.


## <a name="next-steps"></a>Pasos siguientes

Ya ha pausado y reanudado el proceso para el almacenamiento de datos. Para más información sobre Azure SQL Data Warehouse, siga el tutorial para cargar los datos.

> [!div class="nextstepaction"]
> [Carga de datos en una instancia de SQL Data Warehouse](load-data-from-azure-blob-storage-using-polybase.md)
