---
title: Aprovisionamiento del entorno de ejecución de integración de SSIS para Azure con PowerShell | Microsoft Docs
description: Aprenda a aprovisionar entorno de ejecución de integración de SSIS para Azure en Azure Data Factory con PowerShell para que pueda implementar y ejecutar paquetes de SSIS en Azure.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: tutorial
ms.date: 10/28/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: cba8af369beac935834da8d2073510e0f997648b
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58123256"
---
# <a name="provision-the-azure-ssis-integration-runtime-in-azure-data-factory-with-powershell"></a>Aprovisionamiento del entorno de ejecución de integración de SSIS para Azure en Azure Data Factory con PowerShell
En este tutorial, se describen los pasos necesarios para aprovisionar un entorno de ejecución de integración (IR) de SSIS para Azure en Azure Data Factory. Posteriormente, podrá usar SQL Server Data Tools (SSDT) o SQL Server Management Studio (SSMS) para implementar y ejecutar paquetes de SQL Server Integration Services (SSIS) en este entorno de ejecución de Azure. En este tutorial, realizará los siguientes pasos:

> [!NOTE]
> En este artículo, se usa Azure PowerShell para aprovisionar un entorno de ejecución de integración de SSIS para Azure. Si desea utilizar la interfaz de usuario de Data Factory para aprovisionar un entorno de ejecución de integración de SSIS para Azure, consulte [Tutorial: Creación de una instancia de Azure-SSIS Integration Runtime](tutorial-create-azure-ssis-runtime-portal.md). 

> [!div class="checklist"]
> * Creación de una factoría de datos.
> * Creación de un entorno de ejecución de integración de SSIS para Azure
> * Inicio del entorno de ejecución de integración de SSIS para Azure
> * Implementación de paquetes de SSIS
> * Revisión del script completo

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- **Suscripción de Azure**. Si no tiene una suscripción a Azure, cree una cuenta [gratuita](https://azure.microsoft.com/free/) antes de empezar. Para obtener información conceptual acerca del entorno de ejecución de integración de SSIS para Azure, consulte esta [introducción del entorno de ejecución de integración de SSIS para Azure](concepts-integration-runtime.md#azure-ssis-integration-runtime). 
- **Servidor de Azure SQL Database**. Si aún no tiene un servidor de bases de datos, cree uno en Azure Portal antes de empezar. Este servidor va a hospedar la base de datos del catálogo de SSIS (SSISDB). Le recomendamos que cree el servidor de bases de datos en la misma región de Azure que el entorno de ejecución de integración. Esta configuración permite que el entorno de ejecución de integración escriba registros de ejecución en SSISDB sin traspasar regiones de Azure. 
    - En función del servidor de bases de datos seleccionado, la SSISDB puede crearse en su nombre como base de datos única, como parte de un grupo elástico o en una instancia administrada y estar accesible desde la red pública o mediante la unión a una red virtual. Para obtener directrices sobre cómo elegir el tipo de servidor de base de datos para hospedar la SSISDB, consulte [Comparación entre bases de datos únicas y grupos elásticos de Azure SQL Database y la instancia administrada](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). Si usa Azure SQL Database con puntos de conexión de servicio de red virtual o una instancia administrada para hospedar SSISDB, o si necesita tener acceso a datos locales, deberá unir la instancia de Azure-SSIS IR a una red virtual. Para ello, consulte [Creación de una instancia de Azure-SSIS IR en una red virtual](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). 
    - Confirme que el valor "**Permitir el acceso a servicios de Azure**" está en la posición **ON** (Activado) en el servidor de bases de datos. Este valor no es aplicable si se usa Azure SQL Database con puntos de conexión de servicio de red virtual o una instancia administrada para hospedar SSISDB. Para más información, consulte [Protección de Azure SQL Database](../sql-database/sql-database-security-tutorial.md#create-firewall-rules). Para habilitar este valor mediante PowerShell, consulte [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule). 
    - Agregue la dirección IP del equipo cliente o un intervalo de direcciones IP que incluya la dirección IP del equipo cliente a la lista de direcciones IP de cliente en la configuración del firewall del servidor de bases de datos. Para más información, consulte [Reglas de firewall de nivel de base de datos y de nivel de servidor de Azure SQL Database](../sql-database/sql-database-firewall-configure.md). 
    - Puede conectarse al servidor de bases de datos mediante la autenticación de SQL con sus credenciales de administrador del servidor, o por medio de la autenticación de Azure Active Directory (AAD) con la identidad administrada de Azure Data Factory.  En este último caso, debe agregar la entidad administrada para su ADF en un grupo de AAD con permisos de acceso al servidor de bases de datos. Para ello, consulte [Crear una instancia de Integration Runtime de Azure SSIS en Azure Data Factory](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). 
    - Confirme que el servidor de Azure SQL Database no tiene un catálogo de SSIS (base de datos SSIDB). El aprovisionamiento de entornos de ejecución de SSIS en Azure no admite el uso de un catálogo de SSIS existente. 
- **Azure PowerShell**. Siga las instrucciones de [Instalación y configuración de Azure PowerShell](/powershell/azure/install-Az-ps). Usa PowerShell para ejecutar un script para aprovisionar una instancia de Integration Runtime de SSIS de Azure que ejecuta paquetes de SSIS en la nube. 

> [!NOTE]
> - Para ver una lista de regiones de Azure en las que Data Factory y Azure-SSIS Integration Runtime están actualmente disponibles, consulte [Disponibilidad de ADF + SSIS IR por región](https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all). 

## <a name="launch-windows-powershell-ise"></a>Inicio de Windows PowerShell ISE
Inicie **Windows PowerShell ISE** con privilegios administrativos. 

## <a name="create-variables"></a>Creación de variables
Copie y pegue el siguiente script: Especifique valores para las variables. Para una lista de los **planes de tarifa** admitidos para Azure SQL Database, consulte [Límites de recursos de SQL Database](../sql-database/sql-database-resource-limits.md).

```powershell
# Azure Data Factory information 
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$"
$SubscriptionName = "[Azure subscription name]"
$ResourceGroupName = "[Azure resource group name]"
# Data factory name. Must be globally unique
$DataFactoryName = "[Data factory name]"
$DataFactoryLocation = "EastUS"

# Azure-SSIS integration runtime information - This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "[Specify a name for your Azure-SSIS IR]"
$AzureSSISDescription = "[Specify a description for your Azure-SSIS IR]"
$AzureSSISLocation = "EastUS"
# For supported node sizes, see https://azure.microsoft.com/pricing/details/data-factory/ssis/
$AzureSSISNodeSize = "Standard_D8_v3"
# 1-10 nodes are currently supported
$AzureSSISNodeNumber = 2 
# Azure-SSIS IR edition/license info: Standard or Enterprise 
$AzureSSISEdition = "Standard" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "LicenseIncluded" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, 1-4 parallel executions per node are supported, but for other nodes, 1-8 are currently supported
$AzureSSISMaxParallelExecutionsPerNode = 8
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored

# SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name].database.windows.net" # WARNING: Please ensure that there is no existing SSISDB, so we can prepare and manage one on your behalf    
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication]"
# For the basic pricing tier, specify "Basic", not "B" - For standard/premium/elastic pool tiers, specify "S0", "S1", "S2", "S3", etc.
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>)]"
```

## <a name="validate-the-connection-to-database"></a>Validación de la conexión a la base de datos
Agregue el siguiente script para validar su servidor de Azure SQL Database, `<servername>.database.windows.net`. 

```powershell
$SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID=" + $SSISDBServerAdminUserName + ";Password=" + $SSISDBServerAdminPassword    
$sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
Try
{
    $sqlConnection.Open();
}
Catch [System.Data.SqlClient.SqlException]
{
    Write-Warning "Cannot connect to your Azure SQL Database server, exception: $_";
    Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
    $yn = Read-Host
    if(!($yn -ieq "Y"))
    {
        Return;
    } 
}
```

Para crear una base de datos SQL de Azure como parte del script, consulte el ejemplo siguiente: 

Establezca los valores de las variables que aún no se hayan definido. Por ejemplo:  SSISDBServerName, FirewallIPAddress. 

```powershell
New-AzSqlServer -ResourceGroupName $ResourceGroupName `
    -ServerName $SSISDBServerName `
    -Location $DataFactoryLocation `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $SSISDBServerAdminUserName, $(ConvertTo-SecureString -String $SSISDBServerAdminPassword -AsPlainText -Force))    

New-AzSqlServerFirewallRule -ResourceGroupName $ResourceGroupName `
    -ServerName $SSISDBServerName `
    -FirewallRuleName "ClientIPAddress_$today" -StartIpAddress $FirewallIPAddress -EndIpAddress $FirewallIPAddress

New-AzSqlServerFirewallRule -ResourceGroupName $ResourceGroupName -ServerName $SSISDBServerName -AllowAllAzureIPs
```

## <a name="log-in-and-select-subscription"></a>Inicio de sesión y selección de la suscripción
Agregue el código siguiente al script para iniciar sesión y seleccione la suscripción de Azure: 

```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName $SubscriptionName
```

## <a name="create-a-resource-group"></a>Crear un grupo de recursos
Cree un [grupo de recursos de Azure](../azure-resource-manager/resource-group-overview.md) con el comando [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). Un grupo de recursos es un contenedor lógico en el que se implementan y se administran recursos de Azure como un grupo. En el ejemplo siguiente, se crea un grupo de recursos denominado `myResourceGroup` en la ubicación `westeurope`.

Si ya existe el grupo de recursos, no copie este código en el script. 

```powershell
New-AzResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
```

## <a name="create-a-data-factory"></a>Crear una factoría de datos
Ejecute el comando siguiente para crear una factoría de datos:

```powershell
Set-AzDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName
```

## <a name="create-an-integration-runtime"></a>Creación de una instancia de Integration Runtime
Ejecute el siguiente comando para crear una instancia de Integration Runtime de SSIS de Azure que ejecute paquetes de SSIS en Azure: 

```powershell
$secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
$serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)
  
Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Type Managed `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -CatalogServerEndpoint $SSISDBServerEndpoint `
                                           -CatalogAdminCredential $serverCreds `
                                           -CatalogPricingTier $SSISDBPricingTier

if(![string]::IsNullOrEmpty($SetupScriptContainerSasUri))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -SetupScriptContainerSasUri $SetupScriptContainerSasUri
}
```

## <a name="start-integration-runtime"></a>Inicio de la instancia de Integration Runtime
Ejecute el siguiente comando para iniciar la instancia de Integration Runtime de SSIS de Azure: 

```powershell
write-host("##### Starting your Azure-SSIS integration runtime. This command takes 20 to 30 minutes to complete. #####")
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")                                  
```
Este comando tarda entre **20 y 30 minutos** en completarse. 

## <a name="deploy-ssis-packages"></a>Implementación de paquetes de SSIS
Luego, use SQL Server Data Tools (SSDT) o SQL Server Management Studio (SSMS) para implementar los paquetes de SSIS en Azure. Conéctese a la instancia de SQL Server de Azure que hospeda el catálogo de SSIS (SSISDB). El nombre del servidor de Azure SQL Database tiene el formato `<servername>.database.windows.net`. 

Consulte los siguientes artículos de la documentación de SSIS: 

- [Deploy, run, and monitor an SSIS package on Azure](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial) (Implementación, ejecución y supervisión de un paquete SSIS en Azure)   
- [Connect to SSIS catalog on Azure](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database) (Conexión al catálogo de SSIS en Azure)
- [Schedule package execution on Azure](/sql/integration-services/lift-shift/ssis-azure-schedule-packages) (Programación de la ejecución de un paquete en Azure)
- [Connect to on-premises data sources with Windows authentication](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) (Conexión a orígenes de datos locales con autenticación de Windows) 

## <a name="full-script"></a>Script completo
El script de PowerShell de esta sección configura una instancia de Integration Runtime de SSIS de Azure en la nube que ejecuta paquetes de SSIS. Después de ejecutar correctamente este script, puede implementar y ejecutar paquetes de SSIS en la nube de Microsoft Azure con SSISDB hospedado en Azure SQL Database.

1. Inicie Windows PowerShell Integrated Scripting Environment (ISE).
2. En el entorno ISE, ejecute el siguiente comando desde el símbolo del sistema.    
    ```powershell
    Set-ExecutionPolicy Unrestricted -Scope CurrentUser
    ```
3. Copie el script de PowerShell de esta sección y péguelo en el entorno ISE.
4. Proporcione los valores adecuados para todos los parámetros al principio del script.
5. Ejecute el script. El comando `Start-AzDataFactoryV2IntegrationRuntime`, situado cerca del final del script, se ejecuta durante **20 o 30 minutos**.

> [!NOTE]
> - El script se conecta al servidor de Azure SQL Database para preparar la base de datos de catálogo de SSIS (SSISDB).
> 
> - Al aprovisionar una instancia del entorno de ejecución para la integración de SSIS en Azure, también se instala el paquete de características de Azure para SSIS y el acceso redistribuible. Estos componentes proporcionan conectividad a los archivos de Excel y Access, y a diversos orígenes de datos de Azure, además de los admitidos por los componentes integrados. También puede instalar componentes adicionales. Para más información, consulte [Custom setup for the Azure-SSIS integration runtime](how-to-configure-azure-ssis-ir-custom-setup.md) (Instalación personalizada de la instancia de Integration Runtime para la integración de SSIS en Azure).

Para una lista de los **planes de tarifa** admitidos para Azure SQL Database, consulte [Límites de recursos de SQL Database](../sql-database/sql-database-resource-limits.md). 

Para ver una lista de regiones de Azure en las que Data Factory y Azure-SSIS Integration Runtime están actualmente disponibles, consulte [Disponibilidad de ADF + SSIS IR por región](https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all). 

```powershell
# Azure Data Factory information 
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$"
$SubscriptionName = "[Azure subscription name]"
$ResourceGroupName = "[Azure resource group name]"
# Data factory name. Must be globally unique
$DataFactoryName = "[Data factory name]"
$DataFactoryLocation = "EastUS"

# Azure-SSIS integration runtime information - This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "[Specify a name for your Azure-SSIS IR]"
$AzureSSISDescription = "[Specify a description for your Azure-SSIS IR]"
$AzureSSISLocation = "EastUS"
# For supported node sizes, see https://azure.microsoft.com/pricing/details/data-factory/ssis/
$AzureSSISNodeSize = "Standard_D8_v3"
# 1-10 nodes are currently supported
$AzureSSISNodeNumber = 2 
# Azure-SSIS IR edition/license info: Standard or Enterprise 
$AzureSSISEdition = "Standard" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "LicenseIncluded" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, 1-4 parallel executions per node are supported, but for other nodes, 1-8 are currently supported
$AzureSSISMaxParallelExecutionsPerNode = 8
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored

# SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name].database.windows.net" # WARNING: Please ensure that there is no existing SSISDB, so we can prepare and manage one on your behalf    
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication]"
# For the basic pricing tier, specify "Basic", not "B" - For standard/premium/elastic pool tiers, specify "S0", "S1", "S2", "S3", etc.
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>)]"

$SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID=" + $SSISDBServerAdminUserName + ";Password=" + $SSISDBServerAdminPassword    
$sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
Try
{
    $sqlConnection.Open();
}
Catch [System.Data.SqlClient.SqlException]
{
    Write-Warning "Cannot connect to your Azure SQL Database server, exception: $_";
    Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
    $yn = Read-Host
    if(!($yn -ieq "Y"))
    {
        Return;
    } 
}

Connect-AzAccount
Select-AzSubscription -SubscriptionName $SubscriptionName

Set-AzDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName
    
$secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
$serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)
    
Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Type Managed `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -CatalogServerEndpoint $SSISDBServerEndpoint `
                                           -CatalogAdminCredential $serverCreds `
                                           -CatalogPricingTier $SSISDBPricingTier

if(![string]::IsNullOrEmpty($SetupScriptContainerSasUri))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -SetupScriptContainerSasUri $SetupScriptContainerSasUri
}

write-host("##### Starting your Azure-SSIS integration runtime. This command takes 20 to 30 minutes to complete. #####")
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")
```

## <a name="join-azure-ssis-ir-to-a-virtual-network"></a>Unión de la instancia de Integration Runtime para la integración de SSIS en Azure a una red virtual
Si usa Azure SQL Database con puntos de conexión de red virtual o una instancia administrada que se une a una red virtual para hospedar SSISDB, también debe unir su instancia de Azure-SSIS Integration Runtime a la misma red virtual. Azure Data Factory le permite unir su instancia de Integration Runtime para la integración de SSIS en Azure a una red virtual. Para más información, consulte [Unión de una instancia de Integration Runtime de SSIS de Azure a una red virtual](join-azure-ssis-integration-runtime-virtual-network.md).

Para obtener un script completo para crear una instancia de Integration Runtime de SSIS de Azure que se une a una red virtual, consulte [Creación de una instancia de Integration Runtime de SSIS de Azure](create-azure-ssis-integration-runtime.md).

## <a name="monitor-and-manage-azure-ssis-ir"></a>Supervisión y administración de una instancia de Integration Runtime de SSIS de Azure
Consulte los artículos siguientes para más información sobre la supervisión y administración de una instancia de Integration Runtime de SSIS de Azure. 

- [Supervisión de una instancia de Integration Runtime de SSIS de Azure](monitor-integration-runtime.md#azure-ssis-integration-runtime)
- [Administración de una instancia de Integration Runtime de SSIS de Azure](manage-azure-ssis-integration-runtime.md)

## <a name="next-steps"></a>Pasos siguientes
En este tutorial aprendió lo siguiente: 

> [!div class="checklist"]
> * Creación de una factoría de datos.
> * Creación de un entorno de ejecución de integración de SSIS para Azure
> * Inicio del entorno de ejecución de integración de SSIS para Azure
> * Implementación de paquetes de SSIS
> * Revisión del script completo

Para obtener información acerca de la personalización del entorno de ejecución de integración de SSIS de Azure, vaya al siguiente artículo:

> [!div class="nextstepaction"]
>[Personalización de Azure-SSIS Integration Runtime](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup)
