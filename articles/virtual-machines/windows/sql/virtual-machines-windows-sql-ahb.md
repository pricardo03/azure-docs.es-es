---
title: Modificación del modelo de licencia para una máquina virtual de SQL en Azure | Microsoft Docs
description: Aprenda a cambiar el sistema de licencias de una máquina virtual de SQL en Azure.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.assetid: aa5bf144-37a3-4781-892d-e0e300913d03
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 11/14/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: cd784163047f4fe15fde719ce56aba64eed60dd2
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2018
ms.locfileid: "53336992"
---
# <a name="how-to-change-the-licensing-model-for-a-sql-server-virtual-machine-in-azure"></a>Modificación del modelo de licencia para una máquina virtual de SQL Server en Azure
En este artículo se describe cómo cambiar el modelo de licencia para una máquina virtual de SQL Server en Azure con el nuevo proveedor de recursos SQL, **Microsoft.SqlVirtualMachine**. Existen dos modelos de licencia para una máquina virtual (VM) que hospeda SQL Server: pago por uso y traiga su propia licencia (BYOL). Y ahora, con PowerShell o la CLI de Azure, puede modificar qué modelo de licencia usa la máquina virtual de SQL. 

El modelo **Pago por uso** significa que el costo por segundo de ejecutar la máquina virtual de Azure incluye el costo de la licencia de SQL Server.

El modelo **Traiga su propia licencia**, también conocido como [Ventaja híbrida de Azure](https://azure.microsoft.com/pricing/hybrid-benefit/), le permite usar su propia licencia de SQL Server con una máquina virtual que ejecuta SQL Server. Para más información sobre los precios, consulte [Orientación de precios de SQL Server para máquinas virtuales de Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance).

El cambio entre los dos modelos de licencia no genera **ningún tiempo de inactividad**, no reinicia la máquina virtual, no agrega **ningún costo adicional** (de hecho, al activar AHB se reduce el costo) y es **efectivo inmediatamente**. 


## <a name="register-existing-sql-vm-with-new-resource-provider"></a>Registro de máquinas virtuales de SQL existentes con el nuevo proveedor de recursos
La capacidad para cambiar entre modelos de licencia es una característica que ofrece el nuevo proveedor de recursos de máquina virtual de SQL (Microsoft.SqlVirtualMachine). En este momento, para poder cambiar de modelo de licencia, primero debe registrar el nuevo proveedor en su suscripción y, luego, registrar la máquina virtual existente en el nuevo proveedor de recursos de máquina virtual de SQL. Para utilizar el proveedor de recursos de máquina virtual de SQL, también debe instalar la extensión IaaS para SQL. Hacerlo, le permitirá registrar una máquina virtual que se implementó con un disco duro virtual. Para más información, consulte [Extensión de IaaS para SQL](virtual-machines-windows-sql-server-agent-extension.md). 

  >[!IMPORTANT]
  > Si quita el recurso de máquina virtual de SQL, volverá a la configuración de licencia codificada de forma rígida de la imagen. 

### <a name="powershell"></a>PowerShell


El siguiente fragmento de código le conectará a Azure y comprobará qué identificador de suscripción está usando. 
```PowerShell
# Connect to Azure
Connect-AzureRmAccount
Account: <account_name>

# Verify your subscription ID
Get-AzureRmContext

# Set the correct Azure Subscription ID
Set-AzureRmContext -SubscriptionId <Subscription_ID>
```

El fragmento de código siguiente registra primero el nuevo proveedor de recursos SQL en su suscripción y, luego, registra la máquina virtual de SQL existente en el nuevo proveedor de recursos. 

```powershell
# Register the new SQL resource provider for your subscription
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.SqlVirtualMachine

# Register your existing SQL VM with the new resource provider
# example: $vm=Get-AzureRmVm -ResourceGroupName AHBTest -Name AHBTest
$vm=Get-AzureRmVm -ResourceGroupName <ResourceGroupName> -Name <VMName>
New-AzureRmResource -ResourceName $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location -ResourceType Microsoft.SqlVirtualMachine/sqlVirtualMachines -Properties @{virtualMachineResourceId=$vm.Id}
```

### <a name="portal"></a>Portal
También puede registrar el nuevo proveedor de recursos de máquina virtual de SQL mediante el portal. Para ello, siga estos pasos:
1. Abra Azure Portal y vaya a **Todos los servicios**. 
1. Vaya a **Suscripciones** y seleccione la suscripción que le interese.  
1. En la hoja **Suscripciones**, vaya a **Proveedor de recursos**. 
1. Escriba `sql` en el filtro para mostrar los proveedores de recursos relacionados con SQL. 
1. Seleccione *Registrar*, *Volver a registrar* o *Anular registro* para el proveedor **Microsoft.SqlVirtualMachine**, según la acción que quiera realizar. 

  ![Modificación del proveedor](media/virtual-machines-windows-sql-ahb/select-resource-provider-sql.png)


## <a name="use-powershell"></a>Uso de PowerShell 
Puede usar PowerShell para cambiar el modelo de licencia.  Antes de cambiar el modelo de licencia, asegúrese de que la máquina virtual de SQL ya se haya registrado en el nuevo proveedor de recursos SQL. 

El siguiente fragmento de código cambia el modelo de licencia de pago por uso a BYOL (o mediante Ventaja híbrida de Azure): 
```PowerShell
#example: $SqlVm = Get-AzureRmResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName AHBTest -ResourceName AHBTest
$SqlVm = Get-AzureRmResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm.Properties.sqlServerLicenseType="AHUB"
$SqlVm | Set-AzureRmResource -Force 
``` 

El siguiente fragmento de código cambia el modelo BYOL a pago por uso:
```PowerShell
#example: $SqlVm = Get-AzureRmResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName AHBTest -ResourceName AHBTest
$SqlVm = Get-AzureRmResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm.Properties.sqlServerLicenseType="PAYG"
$SqlVm | Set-AzureRmResource -Force 
```

  >[!NOTE]
  > Para cambiar entre licencias, debe estar usando el nuevo proveedor de recursos de máquina virtual de SQL. Si intenta ejecutar estos comandos antes de registrar la máquina virtual de SQL con el nuevo proveedor se puede producir este error: `Get-AzureRmResource : The Resource 'Microsoft.SqlVirtualMachine/SqlVirtualMachines/AHBTest' under resource group 'AHBTest' was not found. The property 'sqlServerLicenseType' cannot be found on this object. Verify that the property exists and can be set. ` Si ve este error, [registre la máquina virtual de SQL con el nuevo proveedor de recursos](#register-existing-SQL-vm-with-new-resource-provider). 
 

## <a name="use-azure-cli"></a>Uso de CLI de Azure
Puede usar la CLI de Azure para cambiar el modelo de licencia.  Antes de cambiar el modelo de licencia, asegúrese de que la máquina virtual de SQL ya se haya registrado en el nuevo proveedor de recursos SQL. 

El siguiente fragmento de código cambia el modelo de licencia de pago por uso a BYOL (o mediante Ventaja híbrida de Azure):
```azurecli
az resource update -g <resource_group_name> -n <sql_virtual_machine_name> --resource-type "Microsoft.SqlVirtualMachine/SqlVirtualMachines" --set properties.sqlServerLicenseType=AHUB
# example: az resource update -g AHBTest -n AHBTest --resource-type "Microsoft.SqlVirtualMachine/SqlVirtualMachines" --set properties.sqlServerLicenseType=AHUB
```

El siguiente fragmento de código cambia el modelo BYOL a pago por uso: 
```azurecli
az resource update -g <resource_group_name> -n <sql_virtual_machine_name> --resource-type "Microsoft.SqlVirtualMachine/SqlVirtualMachines" --set properties.sqlServerLicenseType=PAYG
# example: az resource update -g AHBTest -n AHBTest --resource-type "Microsoft.SqlVirtualMachine/SqlVirtualMachines" --set properties.sqlServerLicenseType=PAYG
```

  >[!NOTE]
  >Para cambiar entre licencias, debe estar usando el nuevo proveedor de recursos de máquina virtual de SQL. Si intenta ejecutar estos comandos antes de registrar la máquina virtual de SQL con el nuevo proveedor se puede producir este error: `The Resource 'Microsoft.SqlVirtualMachine/SqlVirtualMachines/AHBTest' under resource group 'AHBTest' was not found. ` Si ve este error, [registre la máquina virtual de SQL con el nuevo proveedor de recursos](#register-existing-SQL-vm-with-new-resource-provider). 

## <a name="view-current-licensing"></a>Visualización del modelo de licencia actual 

El siguiente fragmento de código le permite ver el modelo de licencia actual de la máquina virtual de SQL. 

```PowerShell
# View current licensing model for your SQL VM
#example: $SqlVm = Get-AzureRmResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm = Get-AzureRmResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm.Properties.sqlServerLicenseType
```

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte los siguientes artículos. 

* [Introducción a SQL Server en máquinas virtuales Windows](virtual-machines-windows-sql-server-iaas-overview.md)
* [Preguntas más frecuentes para SQL Server en máquinas virtuales Windows](virtual-machines-windows-sql-server-iaas-faq.md)
* [Orientación de precios de SQL Server para máquinas virtuales Windows](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server on a Windows VM release notes](virtual-machines-windows-sql-server-iaas-release-notes.md) (Notas de la versión de SQL Server en máquinas virtuales Windows)


