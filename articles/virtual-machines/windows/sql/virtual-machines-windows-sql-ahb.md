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
ms.date: 02/13/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: eded5b2b8715d6a09f7c1a50012b262cec17bfb1
ms.sourcegitcommit: 09bb15a76ceaad58517c8fa3b53e1d8fec5f3db7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2019
ms.locfileid: "58762759"
---
# <a name="how-to-change-the-licensing-model-for-a-sql-server-virtual-machine-in-azure"></a>Modificación del modelo de licencia para una máquina virtual de SQL Server en Azure
En este artículo se describe cómo cambiar el modelo de licencia para una máquina virtual con SQL Server en Azure mediante el nuevo proveedor de recursos de VM con SQL, **Microsoft.SqlVirtualMachine**. Hay dos modelos para una máquina virtual (VM) que hospeda SQL Server - pago por uso, de licencia y traiga su propia licencia (BYOL). Y ahora, con PowerShell o la CLI de Azure, puede modificar qué modelo de licencia usa la VM con SQL Server. 

El **pago por uso** modelo (PAYG) significa que el costo por segundo de la ejecución de la máquina virtual de Azure incluye el costo de la licencia de SQL Server.

El **bring-your-propia licencia** modelo (BYOL) es también conocida como la [ventaja híbrida de Azure (AHB)](https://azure.microsoft.com/pricing/hybrid-benefit/), y le permite usar su propia licencia de SQL Server con una máquina virtual que ejecuta SQL Server. Para más información sobre los precios, consulte [Orientación de precios de VM con SQL Server](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance).

El cambio entre los dos modelos de licencia no genera **ningún tiempo de inactividad**, no reinicia la VM, no agrega **ningún costo adicional** (de hecho, al activar AHB se *reduce* el costo) y es **efectivo inmediatamente**. 


## <a name="remarks"></a>Comentarios

 - Actualmente, la capacidad de convertir el modelo de licencia solo está disponible cuando se inicia con una imagen de VM con SQL Server de pago por uso. Si inicia con una imagen de traiga su propia licencia desde el portal, no podrá convertir esa imagen al modo pago por uso.
 - Los clientes CSP pueden utilizar la Ventaja híbrida de Azure si implementan una máquina virtual de pago por uso y, a continuación, la convierten al modo traiga su propia licencia. 
 - Actualmente esta capacidad está habilitada solo para las instalaciones de la nube pública.
 - Al registrar una imagen de máquina virtual de SQL Server personalizada con el proveedor de recursos, especifique el tipo de licencia como = 'AHUB'. Salir de la licencia, escriba en blanco, o especificando 'PAYG' provocará que se producirá un error en el registro. 

## <a name="prerequisites"></a>Requisitos previos
El uso del proveedor de recursos de VM con SQL requiere la extensión IaaS de SQL. Por lo tanto, para seguir usando el proveedor de recursos de VM con SQL, necesita lo siguiente:
- Una [suscripción de Azure](https://azure.microsoft.com/free/).
- [Programa Software assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default). 
- Un *pago por uso* [VM de SQL Server](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision) con el [extensión SQL IaaS](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension) instalado. 


## <a name="register-sql-resource-provider-to-your-subscription"></a>Registrar proveedor de recursos SQL a su suscripción 

La capacidad para cambiar entre modelos de licencia es una característica que ofrece el nuevo proveedor de recursos de máquina virtual de SQL (Microsoft.SqlVirtualMachine). Las VM con SQL Server implementadas después de diciembre de 2018 se registran automáticamente con el nuevo proveedor de recursos. Sin embargo, las VM existentes que se implementaron antes de esta fecha deben registrarse manualmente con el proveedor de recursos para poder cambiar su modelo de licencia. 

  > [!NOTE] 
  > Si quita el recurso de VM con SQL Server, volverá a la configuración de licencia codificada de forma rígida de la imagen. 

Para registrar la VM con SQL Server con el proveedor de recursos de SQL, debe registrar a dicho proveedor en su suscripción. Puede hacerlo con el portal de Azure, CLI de Azure o PowerShell. 

### <a name="with-the-azure-portal"></a>Con Azure Portal
Los pasos siguientes registrarán el proveedor de recursos SQL a su suscripción de Azure mediante Azure portal. 

1. Abra Azure Portal y vaya a **Todos los servicios**. 
1. Vaya a **Suscripciones** y seleccione la suscripción que le interese.  
1. En la hoja **Suscripciones**, vaya a **Proveedores de recursos**. 
1. Escriba `sql` en el filtro para mostrar los proveedores de recursos relacionados con SQL. 
1. Seleccione *Registrar*, *Volver a registrar* o *Anular registro* para el proveedor **Microsoft.SqlVirtualMachine**, según la acción que quiera realizar. 

   ![Modificación del proveedor](media/virtual-machines-windows-sql-ahb/select-resource-provider-sql.png)

### <a name="with-azure-cli"></a>Con la CLI de Azure
El siguiente fragmento de código registrará el proveedor de recursos SQL a su suscripción de Azure. 

```azurecli
# Register the new SQL resource provider to your subscription 
az provider register --namespace Microsoft.SqlVirtualMachine 
```

### <a name="with-powershell"></a>Con PowerShell
El siguiente fragmento de código registrará el proveedor de recursos SQL a su suscripción de Azure. 

```powershell
# Register the new SQL resource provider to your subscription
Register-AzResourceProvider -ProviderNamespace Microsoft.SqlVirtualMachine
```


## <a name="register-sql-server-vm-with-sql-resource-provider"></a>Registro de la VM con SQL Server con el proveedor de recursos SQL
Una vez registrado el proveedor de recursos SQL a su suscripción, a continuación, puede registrar la máquina virtual de SQL Server con el proveedor de recursos. Puede hacerlo mediante la CLI de Azure y PowerShell. 

### <a name="with-azure-cli"></a>Con la CLI de Azure

Registrar la máquina virtual de SQL Server mediante la CLI de Azure con el siguiente fragmento de código: 

```azurecli
# Register your existing SQL Server VM with the new resource provider
az sql vm create -n <VMName> -g <ResourceGroupName> -l <VMLocation>
```

### <a name="with-powershell"></a>Con PowerShell

Registrar la máquina virtual de SQL Server mediante PowerShell con el siguiente fragmento de código: 

```powershell
# Register your existing SQL Server VM with the new resource provider
# example: $vm=Get-AzureRmVm -ResourceGroupName AHBTest -Name AHBTest
$vm=Get-AzureRmVm -ResourceGroupName <ResourceGroupName> -Name <VMName>
New-AzureRmResource -ResourceName $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location -ResourceType Microsoft.SqlVirtualMachine/sqlVirtualMachines -Properties @{virtualMachineResourceId=$vm.Id}
```

## <a name="change-licensing-model"></a>Cambiar el modelo de licencias

Una vez que la máquina virtual de SQL Server se ha registrado con el proveedor de recursos, puede cambiar el modelo de licencias mediante Azure portal, CLI de Azure o PowerShell. 

### <a name="with-the-azure-portal"></a>Con Azure Portal

Puede modificar el modelo de licencia directamente desde el portal. 

1. Vaya a la máquina virtual de SQL Server dentro de la [portal Azure](https://portal.azure.com). 
1. Seleccione **configuración de SQL Server** en el **configuración** panel. 
1. Seleccione **editar** en el **licencia de SQL Server** panel para modificar la licencia. 

![Ventaja híbrida de AZURE en el Portal](media/virtual-machines-windows-sql-ahb/ahb-in-portal.png)

  >[!NOTE]
  > Esta opción no está disponible para las imágenes de bring-your-propia licencia. 

### <a name="with-azure-cli"></a>Con la CLI de Azure

Puede usar la CLI de Azure para cambiar el modelo de licencia.  

El fragmento de código siguiente cambia el modelo de pago por uso de licencias BYOL (o con ventaja híbrida de Azure):

```azurecli
# Switch your SQL Server VM license from pay-as-you-go to bring-your-own
# example: az sql vm update -n AHBTest -g AHBTest --license-type AHUB

az sql vm update -n <VMName> -g <ResourceGroupName> --license-type AHUB
```

El fragmento de código siguiente cambia el modelo BYOL a pago por uso: 

```azurecli
# Switch your SQL Server VM license from bring-your-own to pay-as-you-go
# example: az sql vm update -n AHBTest -g AHBTest --license-type PAYG

az sql vm update -n <VMName> -g <ResourceGroupName> --license-type PAYG
```

### <a name="with-powershell"></a>Con PowerShell 

Puede usar PowerShell para cambiar el modelo de licencia. 

El fragmento de código siguiente cambia el modelo de pago por uso de licencias BYOL (o con ventaja híbrida de Azure): 

```PowerShell
# Switch your SQL Server VM license from pay-as-you-go to bring-your-own
#example: $SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName AHBTest -ResourceName AHBTest

$SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm.Properties.sqlServerLicenseType="AHUB"
<# the following code snippet is only necessary if using Azure Powershell version > 4
$SqlVm.Kind= "LicenseChange"
$SqlVm.Plan= [Microsoft.Azure.Management.ResourceManager.Models.Plan]::new()
$SqlVm.Sku= [Microsoft.Azure.Management.ResourceManager.Models.Sku]::new() #>
$SqlVm | Set-AzResource -Force 
```

El fragmento de código siguiente cambia el modelo BYOL a pago por uso:

```PowerShell
# Switch your SQL Server VM license from bring-your-own to pay-as-you-go
#example: $SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName AHBTest -ResourceName AHBTest

$SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm.Properties.sqlServerLicenseType="PAYG"
<# the following code snippet is only necessary if using Azure Powershell version > 4
$SqlVm.Kind= "LicenseChange"
$SqlVm.Plan= [Microsoft.Azure.Management.ResourceManager.Models.Plan]::new()
$SqlVm.Sku= [Microsoft.Azure.Management.ResourceManager.Models.Sku]::new() #>
$SqlVm | Set-AzResource -Force 
``` 


## <a name="view-current-licensing"></a>Visualización del modelo de licencia actual 

El siguiente fragmento de código le permite ver el modelo de licencia actual de la VM con SQL Server. 

```powershell
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
```powershell
# the following code snippet is necessary if using Azure Powershell version > 4
$SqlVm.Kind= "LicenseChange"
$SqlVm.Plan= [Microsoft.Azure.Management.ResourceManager.Models.Plan]::new()
$SqlVm.Sku= [Microsoft.Azure.Management.ResourceManager.Models.Sku]::new()
```

Use el código siguiente para comprobar la versión de Azure PowerShell:

```powershell
Get-Module -ListAvailable -Name Azure -Refresh
```

### <a name="the-resource-microsoftsqlvirtualmachinesqlvirtualmachinesresource-group-under-resource-group-resource-group-was-not-found-the-property-sqlserverlicensetype-cannot-be-found-on-this-object-verify-that-the-property-exists-and-can-be-set"></a>No se encontró el recurso 'Microsoft.SqlVirtualMachine/SqlVirtualMachines/ < resource-group >' en '< resource-group >' del grupo de recursos. La propiedad 'sqlServerLicenseType' no se encuentra en este objeto. Compruebe que la propiedad existe y se puede establecer.
Este error se produce cuando se intenta cambiar el modelo de licencias en una VM con SQL Server que no se ha registrado con el proveedor de recursos SQL. Deberá registrar el proveedor de recursos para su [suscripción](#register-sql-resource-provider-to-your-subscription)y, a continuación, registre la máquina virtual de SQL Server con el lenguaje SQL [proveedor de recursos](#register-sql-server-vm-with-sql-resource-provider). 

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte los siguientes artículos. 

* [Introducción a SQL Server en máquinas virtuales Windows](virtual-machines-windows-sql-server-iaas-overview.md)
* [Preguntas más frecuentes para SQL Server en máquinas virtuales Windows](virtual-machines-windows-sql-server-iaas-faq.md)
* [Orientación de precios de SQL Server para máquinas virtuales Windows](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server on a Windows VM release notes](virtual-machines-windows-sql-server-iaas-release-notes.md) (Notas de la versión de SQL Server en máquinas virtuales Windows)


