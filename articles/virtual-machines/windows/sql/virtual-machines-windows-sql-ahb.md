---
title: Modificación del modelo de licencia para una VM con SQL Server en Azure
description: Obtenga información sobre cómo modificar la licencia de una máquina virtual con SQL en Azure desde la modalidad de "pago por uso" a "traiga su propia licencia" con la Ventaja híbrida de Azure.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: jroth
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
ms.openlocfilehash: 78ad784a45d2b0063932791daedc9b1ec1aafd72
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2019
ms.locfileid: "67786762"
---
# <a name="how-to-change-licensing-model-for-a-sql-server-virtual-machine-in-azure"></a>Modificación del modelo de licencia para una máquina virtual con SQL Server en Azure
En este artículo se describe cómo cambiar el modelo de licencia para una máquina virtual con SQL Server en Azure mediante el nuevo proveedor de recursos de VM con SQL, **Microsoft.SqlVirtualMachine**.

Existen dos modelos de licencia para una máquina virtual (VM) que hospeda SQL Server: pago por uso y la Ventaja híbrida de Azure (AHB). Y ahora, si se usan Azure Portal, la CLI de Azure o PowerShell, se puede modificar el modelo de licencia de la VM con SQL Server. 

El modelo de **pago por uso** (PAYG) significa que el costo por segundo de ejecutar la máquina virtual de Azure incluye el costo de la licencia de SQL Server.
La [Ventaja híbrida de Azure (AHB)](https://azure.microsoft.com/pricing/hybrid-benefit/) le permite usar su propia licencia de SQL Server con una VM que ejecuta SQL Server. 

La Ventaja híbrida de Microsoft Azure para SQL Server permite usar licencias de SQL Server con Software Assurance ("Licencia calificada") en Azure Virtual Machines. Con la Ventaja híbrida de Azure para SQL Server, no se les cobrará a los clientes por el uso de la licencia de SQL Server en la máquina virtual, pero de todos modos tendrán que pagar el costo del proceso en la nube subyacente (es decir, la tasa base), el almacenamiento y las copias de seguridad, así como la E/S asociada con el uso que hacen de los servicios (según corresponda).

Según los términos de los productos de Microsoft: "Los clientes deben indicar que usan Azure SQL Database (Instancia administrada, Grupo elástico y Base de datos única), Azure Data Factory, SQL Server Integration Services o Máquinas virtuales de SQL Server en la Ventaja híbrida de Azure para SQL Server al configurar cargas de trabajo en Azure".

Existen tres opciones para indicar el uso de la Ventaja híbrida de Azure para SQL Server en una máquina virtual de Azure y ser compatible:

1. Aprovisionamiento de una máquina virtual con una imagen de SQL Server de "traiga su propia licencia" de Azure Marketplace, disponible solo para clientes con contrato Enterprise.
1. Aprovisionamiento de una máquina virtual con una imagen de SQL Server de "pago por uso" de Azure Marketplace y activación de AHB.
1. Instalación automática de SQL Server en una máquina virtual de Azure, [registro manual de su máquina virtual con SQL Server](virtual-machines-windows-sql-register-with-resource-provider.md) y activación de AHB.

El tipo de licencia de SQL Server se establece cuando se aprovisiona la máquina virtual y se puede modificar en cualquier momento posterior. El cambio entre los modelos de licencia no genera **ningún tiempo de inactividad**, no reinicia la VM, no agrega **ningún costo adicional** (de hecho, al activar AHB se *reduce* el costo) y es **efectivo inmediatamente**. 

## <a name="prerequisites"></a>Requisitos previos

El uso del proveedor de recursos de VM con SQL requiere la extensión IaaS de SQL. Por lo tanto, para seguir usando el proveedor de recursos de VM con SQL, necesita lo siguiente:
- Una [suscripción de Azure](https://azure.microsoft.com/free/).
- [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default). 
- Una [máquina virtual con SQL Server](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision) registrada con el [proveedor de recursos de máquina virtual con SQL](virtual-machines-windows-sql-register-with-resource-provider.md) instalado. 


## <a name="change-license-for-vms-already-registered-with-resource-provider"></a>Modificación de la licencia para máquinas virtuales ya registradas con el proveedor de recursos 

# <a name="azure-portaltabazure-portal"></a>[Azure Portal](#tab/azure-portal)

[!INCLUDE [windows-virtual-machines-sql-use-new-management-blade](../../../../includes/windows-virtual-machines-sql-new-resource.md)]

El modelo de licencia se puede modificar directamente desde el portal. 

1. Abra[Azure Portal](https://portal.azure.com) y ejecute el [recurso de las máquinas virtuales de SQL](virtual-machines-windows-sql-manage-portal.md#access-sql-virtual-machine-resource) para su VM con SQL Server. 
1. Seleccione **Configurar** en **Configuración**. 
1. Seleccione la opción **Ventaja híbrida de Azure** y active la casilla para confirmar que tiene una licencia de SQL Server con Software Assurance. 
1. Seleccione **Aplicar** en la parte inferior de la página **Configurar**. 

![Ventaja híbrida de Azure en el portal](media/virtual-machines-windows-sql-ahb/ahb-in-portal.png)


# <a name="az-clitabbash"></a>[CLI de AZ](#tab/bash)

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

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
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
---

## <a name="change-license-for-vms-not-registered-with-resource-provider"></a>Modificación de la licencia para máquinas virtuales no registradas con el proveedor de recursos

Si aprovisionó una VM con SQL Server desde imágenes de Azure Marketplace de "pago por uso", el tipo de licencia de SQL será de "pago por uso". Si aprovisionó una VM con SQL Server desde una imagen de "traiga su propia licencia" de Azure Marketplace, el tipo de licencia será AHUB. Todas las máquinas virtuales con SQL Server aprovisionadas a partir de las imágenes predeterminadas (pago por uso) o de imágenes de Azure Marketplace de "traiga su propia licencia" se registrarán automáticamente con el proveedor de recursos de máquinas virtuales de SQL, para que se pueda cambiar el [tipo de licencia](#change-license-for-vms-already-registered-with-resource-provider).

Solo puede realizar una instalación automática de SQL Server en una máquina virtual de Azure a través de la Ventaja híbrida de Azure y debe [registrar estas máquinas virtuales con el proveedor de recursos de una máquina virtual con SQL](virtual-machines-windows-sql-register-with-resource-provider.md) al establecer la licencia de SQL Server como AHB para indicar el uso de AHB según los términos de los productos de Microsoft.

Solo puede cambiar el tipo de licencia de una VM con SQL Server como "pago por uso" o AHB si la máquina virtual con SQL está registrada con el proveedor de recursos de una máquina virtual con SQL. Además, todas las máquinas virtuales de SQL se deben registrar con un proveedor de recursos de una máquina virtual con SQL para el cumplimiento de la licencia.

## <a name="remarks"></a>Comentarios

 - Los clientes de Azure Cloud Solution Partner (CSP) puedan utilizar Ventaja híbrida de Azure si implementan una máquina virtual de "pago por uso" y, después, la convierten al modo "traiga su propia licencia" si tienen una instancia activa de Software Assurance.
 - Si coloca el recurso de VM con SQL Server, volverá al valor de la licencia codificada de forma rígida de la imagen. 
  - La capacidad de cambiar el modelo de licencia es una característica del proveedor de recursos de la máquina virtual SQL. Al implementarse una imagen de Marketplace mediante Azure Portal se registra de forma automática una VM con SQL Server en el proveedor de recursos. Sin embargo, los clientes que instalen automáticamente SQL Server deberán [registrar la máquina virtual con SQL Server](virtual-machines-windows-sql-register-with-resource-provider.md) manualmente. 
- Para agregar una máquina virtual con SQL Server a un conjunto de disponibilidad se necesita volver a crear la máquina virtual. Por tanto, todas las máquinas virtuales que se agregue a un conjunto de disponibilidad volverán a tener el tipo de licencia predeterminado, de pago por uso, y será preciso volver a habilitar Ventaja híbrida de Azure. 


## <a name="limitations"></a>Limitaciones

 - El cambio del modelo de licencia solo está disponible para los clientes que cuentan con Software Assurance.
 - El cambio del modelo de licencia solo se admite en las ediciones Standard Edition y Enterprise Edition de SQL Server. No se admiten los cambios de licencia para Express, Web ni Developer. 
 - El cambio del modelo de licencia solo se admite para las máquinas virtuales implementadas mediante el modelo de Resource Manager. Las máquinas virtuales implementadas mediante el modelo clásico no se admiten. 
 - El cambio del modelo de licencia solo está habilitado para las instalaciones de la nube pública.
 - El cambio del modelo de licencia solo se admite en las máquinas virtuales que tengan una sola NIC (interfaz de red). En las que tienen varias, hay que quitar una de ellas (desde Azure Portal) antes de intentar el procedimiento. De lo contrario se producirá un error similar al siguiente: `The virtual machine '\<vmname\>' has more than one NIC associated.` Aunque quizás pueda volver a agregar la NIC a la máquina virtual después de cambiar el modo de licencia, las operaciones realizadas a través de la página de configuración de SQL en Azure Portal, como la aplicación automática de revisiones y la copia de seguridad, dejarán de tener soporte técnico.


## <a name="known-errors"></a>Errores conocidos

### <a name="the-resource-microsoftsqlvirtualmachinesqlvirtualmachinesresource-group-under-resource-group-resource-group-was-not-found-the-property-sqlserverlicensetype-cannot-be-found-on-this-object-verify-that-the-property-exists-and-can-be-set"></a>No se encontró el recurso "Microsoft.SqlVirtualMachine/SqlVirtualMachines/\<resource-group>" en el grupo de recursos "\<resource-group>". La propiedad "sqlServerLicenseType" no se puede encontrar en este objeto. Compruebe que la propiedad existe y se puede establecer.
Este error se produce al intentar cambiar el modelo de licencias de una VM con SQL Server que no se ha registrado en el proveedor de recursos de una máquina virtual con SQL. Tendrá que registrar el proveedor de recursos en su [suscripción](virtual-machines-windows-sql-register-with-resource-provider.md#register-sql-vm-resource-provider-with-subscription) y, después, registre la VM con SQL Server con el [proveedor de recursos](virtual-machines-windows-sql-register-with-resource-provider.md) de SQL. 

### <a name="cannot-validate-argument-on-parameter-sku"></a>Cannot validate argument on parameter 'Sku' (No se puede validar el argumento del parámetro "Sku")
Este error puede aparecer al intentar cambiar el modelo de licencia de la VM con SQL Server al usar una versión de Azure PowerShell posterior a la versión 4.0: `Set-AzResource: Cannot validate argument on parameter 'Sku'. The argument is null or empty. Provide an argument that is not null or empty, and then try the command again.`

Para resolver este error, elimine estas líneas del fragmento de código de PowerShell mencionado anteriormente al cambiar el modelo de licencia:

  ```powershell-interactive
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


