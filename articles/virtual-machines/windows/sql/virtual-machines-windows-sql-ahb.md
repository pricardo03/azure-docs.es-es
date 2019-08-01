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
ms.openlocfilehash: 667a696e96234aca33981946a5b063ab5bfb080b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67075920"
---
# <a name="how-to-change-the-licensing-model-for-a-sql-server-virtual-machine-in-azure"></a>Modificación del modelo de licencia para una máquina virtual de SQL Server en Azure
En este artículo se describe cómo cambiar el modelo de licencia para una máquina virtual con SQL Server en Azure mediante el nuevo proveedor de recursos de VM con SQL, **Microsoft.SqlVirtualMachine**. Existen dos modelos de licencia para una máquina virtual (VM) que hospeda SQL Server: pago por uso y traiga su propia licencia (BYOL). Y ahora, si se usan Azure Portal, la CLI de Azure o PowerShell, se puede modificar el modelo de licencia que usa la VM con SQL Server. 

El modelo de **pago por uso** (PAYG) significa que el costo por segundo de ejecutar la máquina virtual de Azure incluye el costo de la licencia de SQL Server.

El modelo de **traiga su propia licencia** (BYOL), también conocido como [Ventaja híbrida de Azure](https://azure.microsoft.com/pricing/hybrid-benefit/), permite usar la licencia propia de SQL Server con una máquina virtual en que se ejecuta SQL Server. Para más información sobre los precios, consulte [Orientación de precios de VM con SQL Server](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance).

El cambio entre los dos modelos de licencia no genera **ningún tiempo de inactividad**, no reinicia la VM, no agrega **ningún costo adicional** (de hecho, al activar AHB se *reduce* el costo) y es **efectivo inmediatamente**. 

## <a name="remarks"></a>Comentarios


 - Los clientes de Azure Cloud Solution Partner (CSP) puedan utilizar Ventaja híbrida de Azure si implementan una máquina virtual de pago por uso y, después, la convierten al modo traiga su propia licencia. 
 - Al registrar una imagen de VM con SQL Server personalizada en el proveedor de recursos, especifique el tipo de licencia como = "AHUB". Si deja el tipo de licencia en blanco, o especifica "PAYG", no se realizará el registro. 
 - Si coloca el recurso de VM con SQL Server, volverá al valor de la licencia codificada de forma rígida de la imagen. 
 - Para agregar una máquina virtual con SQL Server a un conjunto de disponibilidad se necesita volver a crear la máquina virtual. Por tanto, todas las máquinas virtuales que se agregue a un conjunto de disponibilidad volverán a tener el tipo de licencia predeterminado, de pago por uso, y será preciso volver a habilitar Ventaja híbrida de Azure. 
 - La capacidad de cambiar el modelo de licencia es una característica del proveedor de recursos de la máquina virtual SQL. Al implementarse una imagen de Marketplace mediante Azure Portal se registra de forma automática una VM con SQL Server en el proveedor de recursos. Sin embargo, los clientes que instalen automáticamente SQL Server deberán [registrar su VM con SQL Server](#register-sql-server-vm-with-the-sql-vm-resource-provider) de forma manual. 
 

 
## <a name="limitations"></a>Limitaciones

 - Actualmente, la capacidad de convertir el modelo de licencia solo está disponible cuando se inicia con una imagen de VM con SQL Server de pago por uso. Si inicia con una imagen de traiga su propia licencia desde el portal, no podrá convertir esa imagen al modo pago por uso.
 - El cambio del modelo de licencia solo se admite para las máquinas virtuales implementadas mediante el modelo de Resource Manager. Las máquinas virtuales implementadas mediante el modelo clásico no se admiten. 
 - El cambio del modelo de licencia solo está habilitado para las instalaciones de la nube pública.
 - El cambio del modelo de licencia solo se admite en las máquinas virtuales que tengan una sola NIC (interfaz de red). En las que tienen varias, hay que quitar una de ellas (desde Azure Portal) antes de intentar el procedimiento. De lo contrario se producirá un error similar al siguiente: `The virtual machine '\<vmname\>' has more than one NIC associated.` Aunque quizás pueda volver a agregar la NIC a la máquina virtual después de cambiar el modo de licencia, las operaciones realizadas a través de la hoja de configuración de SQL, como la aplicación automática de revisiones y la copia de seguridad, dejarán de tener soporte técnico.

## <a name="prerequisites"></a>Requisitos previos

El uso del proveedor de recursos de VM con SQL requiere la extensión IaaS de SQL. Por lo tanto, para seguir usando el proveedor de recursos de VM con SQL, necesita lo siguiente:
- Una [suscripción de Azure](https://azure.microsoft.com/free/).
- [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default). 
- Una [VM con SQL Server](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision) con el modelo de *pago por uso* con la [extensión IaaS de SQL](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension)instalada. 

## <a name="with-the-azure-portal"></a>Con Azure Portal

[!INCLUDE [windows-virtual-machines-sql-use-new-management-blade](../../../../includes/windows-virtual-machines-sql-new-resource.md)]

El modelo de licencia se puede modificar directamente desde el portal. 

1. Abra[Azure Portal](https://portal.azure.com) y ejecute el [recurso de las máquinas virtuales de SQL](virtual-machines-windows-sql-manage-portal.md#access-sql-virtual-machine-resource) para su VM con SQL Server. 
1. Seleccione **Configurar** en **Configuración**. 
1. Seleccione la opción **Ventaja híbrida de Azure** y confirme que tiene una licencia de SQL Server con Software Assurance. 
1. Seleccione **Aplicar** en la parte inferior de la página **Configurar**. 

![Ventaja híbrida de Azure en el portal](media/virtual-machines-windows-sql-ahb/ahb-in-portal.png)

  >[!NOTE]
  > Esta opción no está disponible para las imágenes con el modelo traiga su propia licencia. 

## <a name="with-azure-cli"></a>Con la CLI de Azure

Puede usar la CLI de Azure para cambiar el modelo de licencia.  

El siguiente fragmento de código cambia el modelo de licencia de pago por uso a BYOL (o mediante Ventaja híbrida de Azure):

```azurecli-interactive
# Switch your SQL Server VM license from pay-as-you-go to bring-your-own
# example: az sql vm update -n AHBTest -g AHBTest --license-type AHUB

az sql vm update -n <VMName> -g <ResourceGroupName> --license-type AHUB
```

El fragmento de código siguiente cambia el modelo de traiga su propia licencia a pago por uso: 

```azurecli-interactive
# Switch your SQL Server VM license from bring-your-own to pay-as-you-go
# example: az sql vm update -n AHBTest -g AHBTest --license-type PAYG

az sql vm update -n <VMName> -g <ResourceGroupName> --license-type PAYG
```

## <a name="with-powershell"></a>Con PowerShell
Puede usar PowerShell para cambiar el modelo de licencia.

El siguiente fragmento de código cambia el modelo de licencia de pago por uso a BYOL (o mediante Ventaja híbrida de Azure):

```powershell-interactive
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

El siguiente fragmento de código cambia el modelo de traiga su propia licencia a pago por uso:

```powershell-interactive
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


## <a name="register-sql-server-vm-with-the-sql-vm-resource-provider"></a>Registro de una VM con SQL Server en el proveedor de recursos de una máquina virtual SQL
En determinadas situaciones, es posible que deba registrar manualmente su VM con SQL Server en el proveedor de recursos de VM de SQL. Para ello, también es posible que deba registrar manualmente el proveedor de recursos en su suscripción. 


### <a name="register-sql-vm-resource-provider-with-subscription"></a>Registro de un proveedor de recursos de VM con SQL en una suscripción 

Para registrar la VM con SQL Server con el proveedor de recursos de SQL, debe registrar a dicho proveedor en su suscripción. Puede hacerlo con Azure Portal o mediante la CLI de Azure. 

#### <a name="with-the-azure-portal"></a>Con Azure Portal
Los siguientes pasos le ayudarán a registrar al proveedor de recursos de SQL en su suscripción de Azure mediante Azure Portal. 

1. Abra Azure Portal y vaya a **Todos los servicios**. 
1. Vaya a **Suscripciones** y seleccione la suscripción que le interese.  
1. En la hoja **Suscripciones**, vaya a **Proveedores de recursos**. 
1. Escriba `sql` en el filtro para mostrar los proveedores de recursos relacionados con SQL. 
1. Seleccione *Registrar*, *Volver a registrar* o *Anular registro* para el proveedor **Microsoft.SqlVirtualMachine**, según la acción que quiera realizar. 

   ![Modificación del proveedor](media/virtual-machines-windows-sql-ahb/select-resource-provider-sql.png)

#### <a name="with-azure-cli"></a>Con la CLI de Azure
El siguiente fragmento de código registrará el proveedor de recursos de VM de SQL en su suscripción de Azure. 

```azurecli-interactive
# Register the new SQL resource provider to your subscription 
az provider register --namespace Microsoft.SqlVirtualMachine 
```

#### <a name="with-powershell"></a>Con PowerShell

El siguiente fragmento de código registrará el proveedor de recursos de SQL en su suscripción de Azure.

```powershell-interactive
# Register the new SQL resource provider to your subscription
Register-AzResourceProvider -ProviderNamespace Microsoft.SqlVirtualMachine
```

### <a name="register-sql-server-vm-with-sql-resource-provider"></a>Registro de la VM con SQL Server con el proveedor de recursos SQL
Una vez que el proveedor de recursos de VM de SQL se haya registrado en su suscripción, puede registrar una VM con SQL Server con dicho proveedor mediante la CLI de Azure. 

#### <a name="with-azure-cli"></a>Con la CLI de Azure

Registre la VM con SQL Server mediante la CLI de Azure con el siguiente fragmento de código: 

```azurecli-interactive
# Register your existing SQL Server VM with the new resource provider
az sql vm create -n <VMName> -g <ResourceGroupName> -l <VMLocation> --license-type <AHUB or PAYG>
```

#### <a name="with-powershell"></a>Con PowerShell
Registre una VM con SQL Server mediante PowerShell con el siguiente fragmento de código:

```powershell-interactive
# Register your existing SQL Server VM with the new resource provider
# example: $vm=Get-AzVm -ResourceGroupName AHBTest -Name AHBTest
$vm=Get-AzVm -ResourceGroupName <ResourceGroupName> -Name <VMName>
New-AzResource -ResourceName $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location -ResourceType Microsoft.SqlVirtualMachine/sqlVirtualMachines -Properties @{virtualMachineResourceId=$vm.Id}
```



## <a name="known-errors"></a>Errores conocidos

### <a name="sql-iaas-extension-is-not-installed-on-virtual-machine"></a>La extensión IaaS de SQL no está instalada en la máquina virtual
La extensión IaaS de SQL es un requisito previo necesario para registrar la VM con SQL Server con el proveedor de recursos de VM con SQL. Si intenta registrar la VM con SQL Server antes de instalar la extensión IaaS de SQL, se producirá el error siguiente:

`Sql IaaS Extension is not installed on Virtual Machine: '{0}'. Please make sure it is installed and in running state and try again later.`

Para resolver este problema, instale la extensión IaaS de SQL antes de intentar registrar la VM con SQL Server. 

  > [!NOTE]
  > La instalación de la extensión IaaS de SQL reiniciará el servicio de SQL Server y solo debe realizarse durante una ventana de mantenimiento. Para más información, consulte [Instalación de la extensión IaaS de SQL](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension#installation). 


### <a name="the-resource-microsoftsqlvirtualmachinesqlvirtualmachinesresource-group-under-resource-group-resource-group-was-not-found-the-property-sqlserverlicensetype-cannot-be-found-on-this-object-verify-that-the-property-exists-and-can-be-set"></a>No se encontró el recurso "Microsoft.SqlVirtualMachine/SqlVirtualMachines/\<resource-group>" en el grupo de recursos "\<resource-group>". La propiedad "sqlServerLicenseType" no se puede encontrar en este objeto. Compruebe que la propiedad existe y se puede establecer.
Este error se produce al intentar cambiar el modelo de licencias de una VM con SQL Server que no se ha registrado en el proveedor de recursos de SQL. Tendrá que registrar el proveedor de recursos en su [suscripción](#register-sql-vm-resource-provider-with-subscription) y, después, registre la VM con SQL Server con el [proveedor de recursos](#register-sql-server-vm-with-sql-resource-provider) de SQL. 

### <a name="cannot-validate-argument-on-parameter-sku"></a>Cannot validate argument on parameter 'Sku' (No se puede validar el argumento del parámetro "Sku")
Este error puede aparecer al intentar cambiar el modelo de licencia de la VM con SQL Server al usar una versión de Azure PowerShell posterior a la versión 4.0: Set-AzResource: No se puede validar el argumento en el parámetro "Sku". El argumento es nulo o está vacío. Especifique un argumento que no sea NULL o esté vacío y, después, vuelva a intentar el comando.
Para resolver este error, elimine estas líneas del fragmento de código de PowerShell mencionado anteriormente al cambiar el modelo de licencia:

```powershell
# the following code snippet is necessary if using Azure Powershell version > 4
$SqlVm.Kind= "LicenseChange"
$SqlVm.Plan= [Microsoft.Azure.Management.ResourceManager.Models.Plan]::new()
$SqlVm.Sku= [Microsoft.Azure.Management.ResourceManager.Models.Sku]::new()
```

Use el código siguiente para comprobar la versión de Azure PowerShell:

```powershell-interactive
Get-Module -ListAvailable -Name Azure -Refresh
```

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte los siguientes artículos. 

* [Introducción a SQL Server en máquinas virtuales Windows](virtual-machines-windows-sql-server-iaas-overview.md)
* [Preguntas más frecuentes para SQL Server en máquinas virtuales Windows](virtual-machines-windows-sql-server-iaas-faq.md)
* [Orientación de precios de SQL Server para máquinas virtuales Windows](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server on a Windows VM release notes](virtual-machines-windows-sql-server-iaas-release-notes.md) (Notas de la versión de SQL Server en máquinas virtuales Windows)


