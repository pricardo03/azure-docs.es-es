---
title: 'Tutorial: Adición de una base de datos única de Azure SQL Database a un grupo de conmutación por error | Microsoft Docs'
description: Adición de una base de datos única de Azure SQL Database a un grupo de conmutación por error mediante Azure Portal, PowerShell o la CLI de Azure
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.reviewer: sstein, carlrab
ms.date: 06/19/2019
ms.openlocfilehash: 6cf688750ac73763c7f0da4eea152cf6bf0c8285
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68935019"
---
# <a name="tutorial-add-an-azure-sql-database-single-database-to-a-failover-group"></a>Tutorial: Adición de una base de datos única de Azure SQL Database a un grupo de conmutación por error

Configure un grupo de conmutación por error para una base de datos única de Azure SQL Database y pruebe la conmutación por error mediante Azure Portal, PowerShell o la CLI de Azure.  En este tutorial, aprenderá a:

> [!div class="checklist"]
> - Crear una base de datos única de Azure SQL Database.
> - Crear un [grupo de conmutación por error](sql-database-auto-failover-group.md) para una base de datos única entre dos servidores lógicos de SQL.
> - Probar la conmutación por error.

## <a name="prerequisites"></a>Requisitos previos

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)
Para completar este tutorial, asegúrese de disponer de los siguientes elementos: 

- Una suscripción de Azure. [Cree una cuenta gratuita](https://azure.microsoft.com/free/) si aún no tiene una.


# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)
Para completar el tutorial, asegúrese de que cuenta con los elementos siguientes:

- Una suscripción de Azure. [Cree una cuenta gratuita](https://azure.microsoft.com/free/) si aún no tiene una.
- [Azure PowerShell](/powershell/azureps-cmdlets-docs)


# <a name="azure-clitabazure-cli"></a>[CLI de Azure](#tab/azure-cli)
Para completar el tutorial, asegúrese de que cuenta con los elementos siguientes:

- Una suscripción de Azure. [Cree una cuenta gratuita](https://azure.microsoft.com/free/) si aún no tiene una.
- La versión más reciente de la [CLI de Azure](/cli/azure/install-azure-cli?view=azure-cli-latest). 

---

## <a name="1---create-a-single-database"></a>1\. Creación de una base de datos única 

[!INCLUDE [sql-database-create-single-database](includes/sql-database-create-single-database.md)]

## <a name="2---create-the-failover-group"></a>2\. Creación de un grupo de conmutación por error 
En este paso, va a crear un [grupo de conmutación por error](sql-database-auto-failover-group.md) entre un servidor de Azure SQL Server existente y uno nuevo en otra región. A continuación, agregue la base de datos de ejemplo al grupo de conmutación por error. 

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)
Cree el grupo de conmutación por error y agregue la base de datos única mediante Azure Portal. 


1. Seleccione **Todos los servicios** en la esquina superior izquierda de [Azure Portal](https://portal.azure.com). 
1. Escriba `sql servers` en el cuadro de búsqueda. 
1. (Opcional) Seleccione el icono de estrella junto a Servidores SQL Server para marcar como favorito **Servidor de SQL Server** y agréguelo al panel de navegación izquierdo. 
    
    ![Localización de servidores SQL Server](media/sql-database-single-database-create-failover-group-tutorial/all-services-sql-servers.png)

1. Seleccione **Servidores SQL Server** y elija el servidor que ha creado en la sección 1, como `mysqlserver`.
1. Seleccione **Grupos de conmutación por error** en el panel **Configuración** y, después, seleccione **Agregar grupo** para crear un nuevo grupo de conmutación por error. 

    ![Adición de un grupo de conmutación por error](media/sql-database-single-database-create-failover-group-tutorial/sqldb-add-new-failover-group.png)

1. En la página **Grupo de conmutación por error**, escriba o seleccione los valores siguientes y, después, seleccione **Crear**:
    - **Nombre del grupo de conmutación por error** Escriba un nombre del grupo de conmutación por error único, como `failovergrouptutorial`. 
    - **Servidor secundario**: Seleccione la opción para *configurar los valores obligatorios* y, a continuación, elija **Crear un nuevo servidor**. Como alternativa, puede elegir un servidor ya existente como servidor secundario. Después de escribir los valores siguientes, elija **Seleccionar**. 
        - **Nombre del servidor**: Escriba un nombre único para el servidor secundario, como `mysqlsecondary`. 
        - **Inicio de sesión del administrador del servidor**: Escriba `azureuser`
        - **Contraseña**: Escriba una contraseña compleja que cumpla los requisitos de contraseña.
        - **Ubicación**: Elija una ubicación en la lista desplegable, como Este de EE. UU. 2. Esta ubicación no puede ser la misma que la del servidor principal.

    > [!NOTE]
    > La configuración del firewall y de inicio de sesión del servidor debe coincidir con la del servidor principal. 
    
      ![Creación de un servidor secundario para el grupo de conmutación por error](media/sql-database-single-database-create-failover-group-tutorial/create-secondary-failover-server.png)

   - **Bases de datos en el grupo** Cuando se selecciona un servidor secundario, esta opción se desbloquea. Selecciónelo para **seleccionar las bases de datos que quiera agregar.** y después elija la base de datos que ha creado en la sección 1. Al agregar la base de datos al grupo de conmutación por error, se iniciará automáticamente el proceso de replicación geográfica. 
        
    ![Adición de la base de datos SQL a un grupo de conmutación por error](media/sql-database-single-database-create-failover-group-tutorial/add-sqldb-to-failover-group.png)
        

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)
Cree el grupo de conmutación por error y agregue la base de datos única mediante PowerShell. 

   > [!NOTE]
   > La configuración del firewall y de inicio de sesión del servidor debe coincidir con la del servidor principal. 

   ```powershell-interactive
   # $subscriptionId = '<SubscriptionID>'
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"
   # $location = "West US 2"
   # $adminLogin = "azureuser"
   # $password = "PWD27!"+(New-Guid).Guid
   # $serverName = "mysqlserver-$(Get-Random)"
   # $databaseName = "mySampleDatabase"
   $drLocation = "East US 2"
   $drServerName = "mysqlsecondary-$(Get-Random)"
   $failoverGroupName = "failovergrouptutorial-$(Get-Random)"

   # Show randomized variables
   Write-host "DR Server name is" $drServerName 
   Write-host "Failover group name is" $failoverGroupName
   
   # Create a secondary server in the failover region
   Write-host "Creating a secondary logical server in the failover region..."
   $drServer = New-AzSqlServer -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName `
      -Location $drLocation `
      -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential `
         -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
   $drServer
   
   
   # Create a failover group between the servers
   $failovergroup = Write-host "Creating a failover group between the primary and secondary server..."
   New-AzSqlDatabaseFailoverGroup `
      –ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -PartnerServerName $drServerName  `
      –FailoverGroupName $failoverGroupName `
      –FailoverPolicy Automatic `
      -GracePeriodWithDataLossHours 2
   $failovergroup
   
   # Add the database to the failover group
   Write-host "Adding the database to the failover group..." 
   Get-AzSqlDatabase `
      -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -DatabaseName $databaseName | `
   Add-AzSqlDatabaseToFailoverGroup `
      -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -FailoverGroupName $failoverGroupName
   Write-host "Successfully added the database to the failover group..." 
   ```

# <a name="azure-clitabazure-cli"></a>[CLI de Azure](#tab/azure-cli)
Cree el grupo de conmutación por error y agregue la base de datos única mediante la CLI de AZ. 

   > [!NOTE]
   > La configuración del firewall y de inicio de sesión del servidor debe coincidir con la del servidor principal. 

   ```azurecli-interactive
   #!/bin/bash
   # Set variables
   # subscriptionID=<SubscriptionID>
   # resourceGroupName=myResourceGroup-$RANDOM
   # location=SouthCentralUS
   # adminLogin=azureuser
   # password="PWD27!"+`openssl rand -base64 18`
   # serverName=mysqlserver-$RANDOM
   # databaseName=mySampleDatabase
   drLocation=NorthEurope
   drServerName=mysqlsecondary-$RANDOM
   failoverGroupName=failovergrouptutorial-$RANDOM

   # Create a secondary server in the failover region
   echo "Creating a secondary logical server in the DR region..."
   az sql server create \
      --name $drServerName \
      --resource-group $resourceGroupName \
      --location $drLocation  \
      --admin-user $adminLogin\
      --admin-password $password
   
   # Create a failover group between the servers and add the database
   echo "Creating a failover group between the two servers..."
   az sql failover-group create \
      --name $failoverGroupName  \
      --partner-server $drServerName \
      --resource-group $resourceGroupName \
      --server $serverName \
      --add-db $databaseName
      --failover-policy Automatic
   ```

---

## <a name="3---test-failover"></a>3\. Prueba de la conmutación por error 
En este paso, se producirá un error en el grupo de conmutación por error en el servidor secundario y, a continuación, se realizará la conmutación por recuperación mediante Azure Portal. 

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)
Pruebe la conmutación por error mediante Azure Portal. 

1. Vaya a **Servidores de SQL Server** en [Azure Portal](https://portal.azure.com). 
1. Seleccione **Grupos de conmutación por error** en el panel **Configuración** y, a continuación, elija el grupo de conmutación por error que ha creado en la sección 2. 
  
   ![Seleccione el grupo de conmutación por error en el portal.](media/sql-database-single-database-create-failover-group-tutorial/select-failover-group.png)

1. Revise cuál es el servidor principal y cuál el secundario. 
1. Seleccione **Conmutación por error** en el panel de tareas para conmutar por error el grupo correspondiente que contiene la base de datos de ejemplo 
1. Seleccione **Sí** en la advertencia que le notifica que las sesiones de TDS se desconectarán. 

   ![Conmutación por error del grupo de conmutación por error que contiene la base de datos SQL](media/sql-database-single-database-create-failover-group-tutorial/failover-sql-db.png)

1. Revise qué servidor es ahora el principal y cuál el secundario. Si la conmutación por error se realiza correctamente, los dos servidores deben tener los roles intercambiados. 
1. Vuelva a seleccionar **Conmutación por error** para devolver los servidores a sus roles originales. 

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)
Pruebe la conmutación por error mediante PowerShell. 


Compruebe el rol de la réplica secundaria: 

   ```powershell-interactive
   # Set variables
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"
   # $serverName = "mysqlserver-$(Get-Random)"
   # $failoverGroupName = "failovergrouptutorial-$(Get-Random)"
   
   # Check role of secondary replica
   Write-host "Confirming the secondary replica is secondary...." 
   (Get-AzSqlDatabaseFailoverGroup `
      -FailoverGroupName $failoverGroupName `
      -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName).ReplicationRole
   ```


Conmute por error el servidor secundario: 

   ```powershell-interactive
   # Set variables
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"
   # $serverName = "mysqlserver-$(Get-Random)"
   # $failoverGroupName = "failovergrouptutorial-$(Get-Random)"
   
   # Failover to secondary server
   Write-host "Failing over failover group to the secondary..." 
   Switch-AzSqlDatabaseFailoverGroup `
      -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName `
      -FailoverGroupName $failoverGroupName
   Write-host "Failed failover group to sucessfully to" $drServerName 
   ```

Revierta el grupo de conmutación por error al servidor principal:

   ```powershell-interactive
   # Set variables
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"
   # $serverName = "mysqlserver-$(Get-Random)"
   # $failoverGroupName = "failovergrouptutorial-$(Get-Random)"
   
   # Revert failover to primary server
   Write-host "Failing over failover group to the primary...." 
   Switch-AzSqlDatabaseFailoverGroup `
      -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -FailoverGroupName $failoverGroupName
   Write-host "Failed failover group to successfully to back to" $serverName
   ```

# <a name="azure-clitabazure-cli"></a>[CLI de Azure](#tab/azure-cli)
Pruebe la conmutación por error mediante la CLI de Azure. 


Compruebe cuál es el servidor secundario:

   
   ```azurecli-interactive
   # Set variables
   # resourceGroupName=myResourceGroup-$RANDOM
   # serverName=mysqlserver-$RANDOM
   
   # Verify which server is secondary
   echo "Verifying which server is in the secondary role..."
   az sql failover-group list \
      --server $serverName \
      --resource-group $resourceGroupName
   ```

Conmute por error el servidor secundario: 

   ```azurecli-interactive
   # Set variables
   # resourceGroupName=myResourceGroup-$RANDOM
   # drServerName=mysqlsecondary-$RANDOM
   # failoverGroupName=failovergrouptutorial-$RANDOM

   
   echo "Failing over group to the secondary server..."
   az sql failover-group set-primary \
      --name $failoverGroupName \
      --resource-group $resourceGroupName \
      --server $drServerName
   echo "Successfully failed failover group over to" $drServerName
   ```

Revierta el grupo de conmutación por error al servidor principal:

   ```azurecli-interactive
   # Set variables
   # resourceGroupName=myResourceGroup-$RANDOM
   # serverName=mysqlserver-$RANDOM
   # failoverGroupName=failovergrouptutorial-$RANDOM
   
   echo "Failing over group back to the primary server..."
   az sql failover-group set-primary \
      --name $failoverGroupName \
      --resource-group $resourceGroupName \
      --server $serverName
   echo "Successfully failed failover group back to" $serverName
   ```

---

## <a name="clean-up-resources"></a>Limpieza de recursos 
Limpie los recursos mediante la eliminación del grupo de recursos. 

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)
Elimine el grupo de recursos mediante Azure Portal. 


1. Vaya a su grupo de recursos en [Azure Portal](https://portal.azure.com).
1. Seleccione **Eliminar grupo de recursos** para eliminar todos los recursos del grupo, así como el propio grupo de recursos. 
1. En la nueva ventana, escriba el nombre del grupo de recursos, `myResourceGroup`, y luego seleccione **Eliminar** para eliminar el grupo de recursos.  

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)
Elimine el grupo de recursos mediante Azure PowerShell. 


   ```powershell-interactive
   # Set variables
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"

   # Remove the resource group
   Write-host "Removing resource group..."
   Remove-AzResourceGroup -ResourceGroupName $resourceGroupName
   Write-host "Resource group removed =" $resourceGroupName
   ```

# <a name="azure-clitabazure-cli"></a>[CLI de Azure](#tab/azure-cli)
Elimine el grupo de recursos mediante la CLI de Azure. 


   ```azurecli-interactive
   # Set variables
   # resourceGroupName=myResourceGroup-$RANDOM
   
   # Clean up resources by removing the resource group
   echo "Cleaning up resources by removing the resource group..."
   az group delete \
     --name $resourceGroupName
   echo "Successfully removed resource group" $resourceGroupName
   ```

---


## <a name="full-scripts"></a>Scripts completos

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

[!code-powershell-interactive[main](../../powershell_scripts/sql-database/failover-groups/add-single-db-to-failover-group-az-ps.ps1 "Add single database to a failover group")]

# <a name="azure-clitabazure-cli"></a>[CLI de Azure](#tab/azure-cli)

[!code-azurecli-interactive[main](../../cli_scripts/sql-database/failover-groups/add-single-db-to-failover-group-az-cli.sh "Create SQL Database")]

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)
No hay scripts disponibles para Azure Portal.
 
---

Puede encontrar otros scripts de Azure SQL Database aquí: [Azure PowerShell](sql-database-powershell-samples.md) y [CLI de Azure](sql-database-cli-samples.md). 

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha agregado una base de datos única de Azure SQL Database a un grupo de conmutación por error y ha probado la conmutación por error. Ha aprendido a:

> [!div class="checklist"]
> - Crear una base de datos única de Azure SQL Database. 
> - Crear un [grupo de conmutación por error](sql-database-auto-failover-group.md) para una base de datos única entre dos servidores lógicos de SQL.
> - Probar la conmutación por error.

Avance al siguiente tutorial sobre cómo agregar un grupo elástico a un grupo de conmutación por error. 

> [!div class="nextstepaction"]
> [Tutorial: Adición de un grupo elástico de Azure SQL Database a un grupo de conmutación por error](sql-database-elastic-pool-failover-group-tutorial.md)
