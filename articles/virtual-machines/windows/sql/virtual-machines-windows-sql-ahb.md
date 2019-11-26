---
title: Cambio del modelo de licencia de una VM con SQL Server en Azure
description: Aprenda a cambiar de licencia en una máquina virtual con SQL Server en Azure de la modalidad de "pago por uso" a "traiga su propia licencia" con la Ventaja híbrida de Azure.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: jroth
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/05/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: aac20034fb4a528e48d5b383f39205a952878539
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2019
ms.locfileid: "72900690"
---
# <a name="change-the-license-model-for-a-sql-server-virtual-machine-in-azure"></a>Cambio del modelo de licencia en una máquina virtual con SQL Server en Azure
En este artículo se describe cómo cambiar el modelo de licencia de una máquina virtual con SQL Server en Azure mediante el nuevo proveedor de recursos de máquina virtual con SQL, **Microsoft.SqlVirtualMachine**.

Existen dos modelos de licencia para una máquina virtual que hospeda SQL Server: pago por uso y Ventaja híbrida de Azure. Puede modificar el modelo de licencia de la VM con SQL Server mediante Azure Portal, la CLI de Azure o PowerShell. 

El modelo de pago por uso significa que el costo por segundo de ejecutar la máquina virtual de Azure incluye el costo de la licencia de SQL Server.
La [Ventaja híbrida de Azure](https://azure.microsoft.com/pricing/hybrid-benefit/) le permite usar su propia licencia de SQL Server con una máquina virtual que ejecuta SQL Server. 

La Ventaja híbrida de Azure permite el uso de licencias de SQL Server con Software Assurance ("Licencia calificada") en Azure Virtual Machines. Con la Ventaja híbrida de Azure, los clientes no pagan por el uso de una licencia de SQL Server en una máquina virtual. Sin embargo, deben pagar por el costo del proceso en la nube subyacente (es decir, la tasa base), el almacenamiento y las copias de seguridad. También deben pagar por la E/S asociada a su uso de los servicios (según corresponda).

Según los términos de los productos de Microsoft: "Los clientes deben indicar que usan Azure SQL Database (Instancia administrada, Grupo elástico y Base de datos única), Azure Data Factory, SQL Server Integration Services o Máquinas virtuales de SQL Server en la Ventaja híbrida de Azure para SQL Server al configurar cargas de trabajo en Azure".

Existen tres opciones para indicar el uso de la Ventaja híbrida de Azure para SQL Server en una máquina virtual de Azure y que es compatible:

- Aprovisionar una máquina virtual mediante una imagen de SQL Server de "traiga su propia licencia" desde Azure Marketplace. Esta opción solo está disponible para los clientes con un Contrato Enterprise.
- Aprovisionar una máquina virtual con una imagen de SQL Server de "pago por uso" desde Azure Marketplace y activar la Ventaja híbrida de Azure
- Instalar automáticamente SQL Server en una máquina virtual de Azure, [registrar manualmente la VM con SQL Server](virtual-machines-windows-sql-register-with-resource-provider.md) y activar la Ventaja híbrida de Azure.

El tipo de licencia de SQL Server se establece cuando se aprovisiona la máquina virtual. Puede cambiarlo en cualquier momento con posterioridad. El cambio entre los modelos de licencia no genera ningún tiempo de inactividad, no reinicia la máquina virtual, no agrega ningún costo adicional y es efectivo inmediatamente. De hecho, activar la Ventaja híbrida de Azure *reduce* el costo.

## <a name="prerequisites"></a>Requisitos previos

El uso del proveedor de recursos de máquina virtual de SQL requiere la extensión IaaS de SQL Server. Como tal, necesita lo siguiente:
- Una [suscripción de Azure](https://azure.microsoft.com/free/).
- [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default). 
- Una [máquina virtual con SQL Server](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision) registrada con el [proveedor de recursos de máquina virtual con SQL](virtual-machines-windows-sql-register-with-resource-provider.md).


## <a name="change-the-license-for-vms-already-registered-with-the-resource-provider"></a>Cambio de la licencia para máquinas virtuales ya registradas con el proveedor de recursos 

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

[!INCLUDE [windows-virtual-machines-sql-use-new-management-blade](../../../../includes/windows-virtual-machines-sql-new-resource.md)]

El modelo de licencia se puede modificar directamente desde el portal: 

1. Abra [Azure Portal](https://portal.azure.com) y el [recurso de las máquinas virtuales de SQL](virtual-machines-windows-sql-manage-portal.md#access-the-sql-virtual-machines-resource) para su VM con SQL Server. 
1. Seleccione **Configurar** en **Configuración**. 
1. Seleccione la opción **Ventaja híbrida de Azure** y active la casilla para confirmar que tiene una licencia de SQL Server con Software Assurance. 
1. Seleccione **Aplicar** en la parte inferior de la página **Configurar**. 

![Ventaja híbrida de Azure en el portal](media/virtual-machines-windows-sql-ahb/ahb-in-portal.png)


# <a name="azure-clitabazure-cli"></a>[CLI de Azure](#tab/azure-cli)

Puede usar la CLI de Azure para cambiar el modelo de licencia.  

El siguiente fragmento de código cambia el modelo de licencia de pago por uso a traiga su propia licencia (o mediante la Ventaja híbrida de Azure):

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

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)
Puede usar PowerShell para cambiar el modelo de licencia.

El siguiente fragmento de código cambia el modelo de licencia de pago por uso a traiga su propia licencia (o mediante la Ventaja híbrida de Azure):

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

El fragmento de código siguiente cambia el modelo de traiga su propia licencia a pago por uso:

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

## <a name="change-the-license-for-vms-not-registered-with-the-resource-provider"></a>Cambio de la licencia para máquinas virtuales no registradas con el proveedor de recursos

Si aprovisionó una VM con SQL Server a partir de imágenes de Azure Marketplace de "pago por uso", el tipo de licencia de SQL Server será de pago por uso. Si aprovisionó una VM con SQL Server con una imagen de "traiga su propia licencia" desde Azure Marketplace, el tipo de licencia será AHUB. Todas las máquinas virtuales con SQL Server aprovisionadas a partir de las imágenes predeterminadas (pago por uso) o de imágenes de Azure Marketplace de "traiga su propia licencia" se registrarán automáticamente con el proveedor de recursos de máquina virtual con SQL, para que puedan cambiar el [tipo de licencia](#change-the-license-for-vms-already-registered-with-the-resource-provider).

Solo puede instalar automáticamente SQL Server en una máquina virtual de Azure mediante la Ventaja híbrida de Azure. [Estas máquinas virtuales se deben registrar con el proveedor de recursos de máquina virtual de SQL](virtual-machines-windows-sql-register-with-resource-provider.md) mediante el establecimiento de la licencia de SQL Server como Ventaja híbrida de Azure, para indicar el uso de este modo de acuerdo con los términos de los productos de Microsoft.

Solo puede cambiar el tipo de licencia de una máquina virtual con SQL Server que sea de pago por uso o Ventaja híbrida de Azure si dicha máquina virtual está registrada con el proveedor de recursos de máquina virtual con SQL.

## <a name="remarks"></a>Comentarios

- Para que los clientes del Proveedor de soluciones en la nube de Azure (CSP) puedan usar la Ventaja híbrida de Azure, deben implementar primero una máquina virtual de "pago por uso" y, después, convertirla al modo "traiga su propia licencia" si tienen una instancia activa de Software Assurance.
- Si coloca el recurso de VM con SQL Server, volverá al valor de la licencia codificada de forma rígida de la imagen. 
- La posibilidad de cambiar el modelo de licencia es una característica del proveedor de recursos de máquina virtual de SQL. Al implementarse una imagen de Azure Marketplace mediante Azure Portal se registra de forma automática una VM con SQL Server con el proveedor de recursos. Sin embargo, los clientes que instalen automáticamente SQL Server deberán [registrar su VM con SQL Server](virtual-machines-windows-sql-register-with-resource-provider.md) de forma manual. 
- Para agregar una VM con SQL Server a un conjunto de disponibilidad, es necesario volver a crear la máquina virtual. Por tanto, todas las máquinas virtuales que se agreguen a un conjunto de disponibilidad volverán a tener el tipo de licencia predeterminado de pago por uso. También será necesario habilitar de nuevo la Ventaja híbrida de Azure. 


## <a name="limitations"></a>Limitaciones

- El cambio del modelo de licencia solo está disponible para los clientes que tengan Software Assurance.
- El cambio del modelo de licencia solo se admite con las versiones Standard Edition y Enterprise Edition de SQL Server. No se admiten los cambios de licencia para Express, Web ni Developer. 
- El cambio del modelo de licencia solo se admite en máquinas virtuales implementadas mediante el modelo de Azure Resource Manager. Las máquinas virtuales implementadas mediante el modelo clásico no se admiten. Puede migrar la máquina virtual del modelo clásico al modelo de Resource Manager y registrarla con el proveedor de recursos de máquina virtual de SQL. Una vez que la máquina virtual está registrada con el proveedor de recursos de máquina virtual de SQL, los cambios en el modelo de licencias estarán disponibles en la máquina virtual.
- El cambio del modelo de licencia solo está habilitado para las instalaciones de la nube pública.
- El cambio del modelo de licencia solo se admite en las máquinas virtuales que tengan una sola NIC (interfaz de red). En las que tienen varias, hay que quitar una de ellas (desde Azure Portal) antes de intentar el procedimiento. De lo contrario, recibirá un error parecido a este: 
   
  `The virtual machine '\<vmname\>' has more than one NIC associated.` 
   
  Aunque quizás pueda volver a agregar la NIC a la máquina virtual después de cambiar el modo de licencia, las operaciones realizadas mediante la página de configuración de SQL Server en Azure Portal, como la aplicación automática de revisiones y la copia de seguridad, dejarán de tener soporte técnico.

## <a name="known-errors"></a>Errores conocidos

### <a name="the-resource-microsoftsqlvirtualmachinesqlvirtualmachinesresource-group-under-resource-group-resource-group-was-not-found"></a>No se encontró el recurso "Microsoft.SqlVirtualMachine/SqlVirtualMachines/\<resource-group>" en el grupo de recursos "\<resource-group>".
Este error se produce al intentar cambiar el modelo de licencia en una máquina virtual con SQL Server que no se ha registrado con el proveedor de recursos de máquina virtual con SQL.

`The Resource 'Microsoft.SqlVirtualMachine/SqlVirtualMachines/\<resource-group>' under resource group '\<resource-group>' was not found. The property 'sqlServerLicenseType' cannot be found on this object. Verify that the property exists and can be set.`

Tendrá que registrar la suscripción con el proveedor de recursos y, después, [registrar la máquina virtual de SQL Server con el proveedor de recursos](virtual-machines-windows-sql-register-with-resource-provider.md). 

### <a name="cannot-validate-argument-on-parameter-sku"></a>Cannot validate argument on parameter 'Sku' (No se puede validar el argumento del parámetro "Sku")
Este error puede aparecer al intentar cambiar el modelo de licencia de la VM con SQL Server con versiones de Azure PowerShell posteriores a la 4.0:

`Set-AzResource: Cannot validate argument on parameter 'Sku'. The argument is null or empty. Provide an argument that is not null or empty, and then try the command again.`

Para resolver este error, quite las marcas de comentarios del fragmento de código de PowerShell mencionado anteriormente al cambiar de modelo de licencia:

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
* [Preguntas más frecuentes de SQL Server en máquinas virtuales Windows](virtual-machines-windows-sql-server-iaas-faq.md)
* [Orientación de precios de SQL Server para máquinas virtuales de Azure](virtual-machines-windows-sql-server-pricing-guidance.md)
* [Notas de la versión de SQL Server en Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-release-notes.md)


