---
title: Modificación del modelo de licencia para una VM con SQL Server en Azure | Microsoft Docs
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
ms.openlocfilehash: 8526716b299d26d8d70c9c5e5cdace34e188d019
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2019
ms.locfileid: "56111074"
---
# <a name="how-to-change-the-licensing-model-for-a-sql-server-virtual-machine-in-azure"></a>Modificación del modelo de licencia para una máquina virtual de SQL Server en Azure
En este artículo se describe cómo cambiar el modelo de licencia para una máquina virtual con SQL Server en Azure mediante el nuevo proveedor de recursos de VM con SQL, **Microsoft.SqlVirtualMachine**. Existen dos modelos de licencia para una máquina virtual (VM) que hospeda SQL Server: pago por uso y traiga su propia licencia (BYOL). Y ahora, con PowerShell o la CLI de Azure, puede modificar qué modelo de licencia usa la VM con SQL Server. 

El modelo **Pago por uso** (PAYG) significa que el costo por segundo de ejecutar la máquina virtual de Azure incluye el costo de la licencia de SQL Server.

El modelo **Traiga su propia licencia** (BYOL), también conocido como [Ventaja híbrida de Azure](https://azure.microsoft.com/pricing/hybrid-benefit/), le permite usar su propia licencia de SQL Server con una máquina virtual que ejecuta SQL Server. Para más información sobre los precios, consulte [Orientación de precios de VM con SQL Server](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance).

El cambio entre los dos modelos de licencia no genera **ningún tiempo de inactividad**, no reinicia la VM, no agrega **ningún costo adicional** (de hecho, al activar AHB se *reduce* el costo) y es **efectivo inmediatamente**. 

  >[!NOTE]
  > - Actualmente, la capacidad de convertir el modelo de licencia solo está disponible cuando se inicia con una imagen de VM con SQL Server de pago por uso. Si inicia con una imagen de traiga su propia licencia desde el portal, no podrá convertir esa imagen al modo pago por uso. 
  > - Los clientes CSP pueden utilizar la Ventaja híbrida de Azure si implementan una máquina virtual de pago por uso y, a continuación, la convierten al modo traiga su propia licencia. 


## <a name="prerequisites"></a>Requisitos previos
El uso del proveedor de recursos de VM con SQL requiere la extensión IaaS de SQL. Por lo tanto, para seguir usando el proveedor de recursos de VM con SQL, necesita lo siguiente:
- Una [suscripción de Azure](https://azure.microsoft.com/free/).
- Una [VM con SQL Server](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision) con la [extensión IaaS de SQL](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension) instalada. 


## <a name="register-existing-sql-server-vm-with-sql-resource-provider"></a>Registro de una VM con SQL Server existente con el proveedor de recursos de SQL
La capacidad para cambiar entre modelos de licencia es una característica que ofrece el nuevo proveedor de recursos de máquina virtual de SQL (Microsoft.SqlVirtualMachine). Las VM con SQL Server implementadas después de diciembre de 2018 se registran automáticamente con el nuevo proveedor de recursos. Sin embargo, las VM existentes que se implementaron antes de esta fecha deben registrarse manualmente con el proveedor de recursos para poder cambiar su modelo de licencia. 

  > [!NOTE] 
  > Si quita el recurso de máquina virtual de SQL, volverá a la configuración de licencia codificada de forma rígida de la imagen. 

### <a name="register-sql-resource-provider-with-your-subscription"></a>Registro del proveedor de recursos de SQL en la suscripción 

Para registrar la VM con SQL Server con el proveedor de recursos de SQL, debe registrar a dicho proveedor en su suscripción. Puede hacerlo con PowerShell o mediante Azure Portal. 

#### <a name="using-powershell"></a>Con PowerShell
El siguiente fragmento de código registrará al proveedor de recursos de SQL en su suscripción de Azure. 

```powershell
# Register the new SQL resource provider for your subscription
Register-AzResourceProvider -ProviderNamespace Microsoft.SqlVirtualMachine
```

#### <a name="using-azure-portal"></a>Uso de Azure Portal
Los siguientes pasos le ayudarán a registrar al proveedor de recursos de SQL en su suscripción de Azure mediante Azure Portal. 

1. Abra Azure Portal y vaya a **Todos los servicios**. 
1. Vaya a **Suscripciones** y seleccione la suscripción que le interese.  
1. En la hoja **Suscripciones**, vaya a **Proveedores de recursos**. 
1. Escriba `sql` en el filtro para mostrar los proveedores de recursos relacionados con SQL. 
1. Seleccione *Registrar*, *Volver a registrar* o *Anular registro* para el proveedor **Microsoft.SqlVirtualMachine**, según la acción que quiera realizar. 

  ![Modificación del proveedor](media/virtual-machines-windows-sql-ahb/select-resource-provider-sql.png)

### <a name="register-sql-server-vm-with-sql-resource-provider"></a>Registro de la VM con SQL Server con el proveedor de recursos SQL
Una vez que haya registrado al proveedor de recursos de SQL en su suscripción, puede usar PowerShell para registrar la VM con SQL Server con dicho proveedor. 


```powershell
# Register your existing SQL Server VM with the new resource provider
# example: $vm=Get-AzureRmVm -ResourceGroupName AHBTest -Name AHBTest
$vm=Get-AzureRmVm -ResourceGroupName <ResourceGroupName> -Name <VMName>
New-AzureRmResource -ResourceName $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location -ResourceType Microsoft.SqlVirtualMachine/sqlVirtualMachines -Properties @{virtualMachineResourceId=$vm.Id}
```


## <a name="use-powershell"></a>Uso de PowerShell 
Puede usar PowerShell para cambiar el modelo de licencia.  Antes de cambiar el modelo de licencia, asegúrese de que la VM con SQL Server ya se haya registrado en el nuevo proveedor de recursos SQL. 

El siguiente fragmento de código cambia el modelo de licencia de pago por uso a BYOL (o mediante Ventaja híbrida de Azure): 
```PowerShell
#example: $SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName AHBTest -ResourceName AHBTest
$SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm.Properties.sqlServerLicenseType="AHUB"
<# the following code snippet is only necessary if using Azure Powershell version > 4
$SqlVm.Kind= "LicenseChange"
$SqlVm.Plan= [Microsoft.Azure.Management.ResourceManager.Models.Plan]::new()
$SqlVm.Sku= [Microsoft.Azure.Management.ResourceManager.Models.Sku]::new() #>
$SqlVm | Set-AzResource -Force 
``` 

El siguiente fragmento de código cambia el modelo BYOL a pago por uso:
```PowerShell
#example: $SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName AHBTest -ResourceName AHBTest
$SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm.Properties.sqlServerLicenseType="PAYG"
<# the following code snippet is only necessary if using Azure Powershell version > 4
$SqlVm.Kind= "LicenseChange"
$SqlVm.Plan= [Microsoft.Azure.Management.ResourceManager.Models.Plan]::new()
$SqlVm.Sku= [Microsoft.Azure.Management.ResourceManager.Models.Sku]::new() #>
$SqlVm | Set-AzResource -Force 
```

  >[!NOTE]
  > Para cambiar entre licencias, debe estar usando el nuevo proveedor de recursos de máquina virtual de SQL. Si intenta ejecutar estos comandos antes de registrar la VM con SQL Server con el nuevo proveedor, se puede producir este error: `Get-AzResource : The Resource 'Microsoft.SqlVirtualMachine/SqlVirtualMachines/AHBTest' under resource group 'AHBTest' was not found. The property 'sqlServerLicenseType' cannot be found on this object. Verify that the property exists and can be set. ` Si ve este error, registre la VM con SQL Server con el nuevo proveedor de recursos. 

 

## <a name="use-azure-cli"></a>Uso de CLI de Azure
Puede usar la CLI de Azure para cambiar el modelo de licencia.  Antes de cambiar el modelo de licencia, asegúrese de que la VM con SQL Server ya se haya registrado en el nuevo proveedor de recursos SQL. 

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
  >Para cambiar entre licencias, debe estar usando el nuevo proveedor de recursos de máquina virtual de SQL. Si intenta ejecutar estos comandos antes de registrar la VM con SQL Server con el nuevo proveedor, se puede producir este error: `The Resource 'Microsoft.SqlVirtualMachine/SqlVirtualMachines/AHBTest' under resource group 'AHBTest' was not found. ` Si ve este error, [registre la VM con SQL Server con el nuevo proveedor de recursos](#register-existing-sql-server-vm-with-sql-resource-provider). 

## <a name="view-current-licensing"></a>Visualización del modelo de licencia actual 

El siguiente fragmento de código le permite ver el modelo de licencia actual de la VM con SQL Server. 

```PowerShell
# View current licensing model for your SQL Server VM
#example: $SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm.Properties.sqlServerLicenseType
```

## <a name="known-errors"></a>Errores conocidos

### <a name="sql-iaas-extension-is-not-installed-on-virtual-machine"></a>La extensión IaaS de SQL no está instalada en la máquina virtual
La extensión IaaS de SQL es un requisito previo necesario para registrar la VM con SQL Server con el proveedor de recursos de VM con SQL. Si intenta registrar la VM con SQL Server antes de instalar la extensión IaaS de SQL, se producirá el error siguiente:

`Sql IaaS Extension is not installed on Virtual Machine: '{0}'. Please make sure it is installed and in running state and try again later.`

Para resolver este problema, instale la extensión IaaS de SQL antes de intentar registrar la VM con SQL Server. 

  > [!NOTE]
  > La instalación de la extensión IaaS de SQL reiniciará el servicio de SQL Server y solo debe realizarse durante una ventana de mantenimiento. Para más información, consulte [Instalación de la extensión IaaS de SQL](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension#installation). 

### <a name="cannot-validate-argument-on-parameter-sku"></a>Cannot validate argument on parameter 'Sku' (No se puede validar el argumento del parámetro "Sku")
Este error puede aparecer al intentar cambiar el modelo de licencia de la VM con SQL Server al usar una versión de Azure PowerShell posterior a la versión 4.0:

`Set-AzResource : Cannot validate argument on parameter 'Sku'. The argument is null or empty. Provide an argument that is not null or empty, and then try the command again.`

Para resolver este error, elimine estas líneas del fragmento de código de PowerShell mencionado anteriormente al cambiar el modelo de licencia: 
```PowerShell
# the following code snippet is necessary if using Azure Powershell version > 4
$SqlVm.Kind= "LicenseChange"
$SqlVm.Plan= [Microsoft.Azure.Management.ResourceManager.Models.Plan]::new()
$SqlVm.Sku= [Microsoft.Azure.Management.ResourceManager.Models.Sku]::new()
```

Use el código siguiente para comprobar la versión de Azure PowerShell:

```PowerShell
Get-Module -ListAvailable -Name Azure -Refresh
```

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte los siguientes artículos. 

* [Introducción a SQL Server en máquinas virtuales Windows](virtual-machines-windows-sql-server-iaas-overview.md)
* [Preguntas más frecuentes para SQL Server en máquinas virtuales Windows](virtual-machines-windows-sql-server-iaas-faq.md)
* [Orientación de precios de SQL Server para máquinas virtuales Windows](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server on a Windows VM release notes](virtual-machines-windows-sql-server-iaas-release-notes.md) (Notas de la versión de SQL Server en máquinas virtuales Windows)


