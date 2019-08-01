---
title: Registro de una máquina virtual con SQL Server en Azure con el proveedor de recursos de VM de SQL | Microsoft Docs
description: Registre una VM con SQL Server con el proveedor de recursos de VM de SQL para mejorar la manejabilidad.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/24/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 95a167cbc8fde4488e3f46ffd850f0619cf1b651
ms.sourcegitcommit: 770b060438122f090ab90d81e3ff2f023455213b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2019
ms.locfileid: "68305866"
---
# <a name="register-sql-server-virtual-machine-in-azure-with-the-sql-vm-resource-provider"></a>Registro de una máquina virtual con SQL Server en Azure con el proveedor de recursos de VM de SQL

En este artículo se describe cómo registrar una máquina virtual de Azure SQL Server (VM) con el proveedor de recursos de VM de SQL. 

La implementación de una imagen de Marketplace de una VM con SQL Server a través de Azure Portal registra la VM con SQL Server con el proveedor de recursos. Sin embargo, si elige instalar automáticamente SQL Server en una máquina virtual de Azure, en lugar de elegir una imagen de Azure Marketplace, debe registrar la VM con SQL Server con el proveedor de recursos hoy mismo para:

-  **Cumplimiento**: en función de los términos de los productos de Microsoft, los clientes que usan [Ventaja híbrida de Azure](https://azure.microsoft.com/pricing/hybrid-benefit/) deben indicar a Microsoft cuando usar Ventaja híbrida de Azure (para ello, se deben registrar con el proveedor de recursos de VM de SQL). 

-  **Ventajas de las características**: el registro de una VM con SQL Server con el proveedor de recursos desbloquea la [aplicación automática de revisiones](virtual-machines-windows-sql-automated-patching.md), la [copia de seguridad automática](virtual-machines-windows-sql-automated-backup-v2.md), las funcionalidades de supervisión y administración, así como las [licencias](virtual-machines-windows-sql-ahb.md) y la flexibilidad de [ediciones](virtual-machines-windows-sql-change-edition.md). Anteriormente, solo estaban disponibles para las imágenes de VM con SQL Server de Azure Marketplace.

La instalación automática de SQL Server en una máquina virtual de Azure o el aprovisionamiento de una máquina virtual de Azure desde un disco duro virtual personalizado con SQL Server se pueden realizar a través de la Ventaja híbrida de Azure para SQL Server con la condición de que los clientes indiquen que lo usan a Microsoft mediante el registro con un proveedor de recursos de VM de SQL. 

Para utilizar el proveedor de recursos de VM de SQL, también debe registrarlo con su suscripción. Esto se puede lograr con Azure Portal, la CLI de Azure y PowerShell. 

## <a name="prerequisites"></a>Requisitos previos

Para registrar una VM con SQL Server en el proveedor de recursos, necesita lo siguiente: 

- Una [suscripción de Azure](https://azure.microsoft.com/free/).
- Una [VM con SQL Server](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision). 
- [CLI de Azure](/cli/azure/install-azure-cli) y [PowerShell](/powershell/azure/new-azureps-module-az). 

## <a name="register-with-sql-vm-resource-provider"></a>Registro en un proveedor de recursos de VM de SQL
Si la [extensión SQL IaaS](virtual-machines-windows-sql-server-agent-extension.md) ya está instalada en la máquina virtual, el registro en el proveedor de recursos de VM de SQL solo crea un recurso de metadatos del tipo Microsoft.SqlVirtualMachine/SqlVirtualMachines. A continuación encontrará el fragmento de código para registrarse en el proveedor de recursos de VM de SQL si la extensión SQL IaaS ya está instalada en la máquina virtual. Deberá especificar el tipo de licencia de SQL Server que desea al registrarse en un proveedor de recursos de VM de SQL como "PAYG" o "AHUB". 

Registre una VM con SQL Server mediante PowerShell con el siguiente fragmento de código:

  ```powershell-interactive
     # Get the existing  Compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
     # Register with SQL VM resource provider
     New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
        -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
        -Properties @{virtualMachineResourceId=$vm.Id;SqlServerLicenseType='AHUB'}  
  
  ```

Si la extensión SQL IaaS no está instalada en la máquina virtual, para registrarse en el proveedor de recursos de VM de SQL especifique el modo de administración de SQL ligero. En dicho modo de administración, el proveedor de recursos de VM de SQL instalará automáticamente la extensión SQL IaaS en [modo ligero](virtual-machines-windows-sql-server-agent-extension.md#install-in-lightweight-mode) y comprobará los metadatos de la instancia de SQL Server; esta operación no reiniciará el servicio SQL Server. Deberá especificar el tipo de licencia de SQL Server que desea al registrarse en un proveedor de recursos de VM de SQL como "PAYG" o "AHUB". 

Registre una VM con SQL Server en modo de administración de SQL ligero mediante PowerShell con el siguiente fragmento de código:

  ```powershell-interactive
     # Get the existing  Compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
     # Register SQL VM with 'Lightweight' SQL IaaS agent
     New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
        -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
        -Properties @{virtualMachineResourceId=$vm.Id;SqlServerLicenseType='AHUB';sqlManagement='LightWeight'}  
  
  ```

El registro en el proveedor de recursos de VM de SQL en [modo ligero](virtual-machines-windows-sql-server-agent-extension.md#install-in-lightweight-mode) garantizará el cumplimiento y habilitará la concesión de licencias flexible, así como las actualizaciones de la edición de SQL Server en vigor. Tanto las instancias del clúster de conmutación por error como las implementaciones de instancias múltiples solo se pueden registrar en el proveedor de recursos de VM de SQL en el modo ligero. Puede seguir las instrucciones que se encuentran en Azure Portal para realizar la actualización al [modo completo](virtual-machines-windows-sql-server-agent-extension.md#full-mode-installation) y habilitar el conjunto de características de manejabilidad completo con un reinicio de SQL Server en cualquier momento. 

## <a name="register-sql-server-2008r2-on-windows-server-2008-vms"></a>Registrar SQL Server 2008 R2 en máquinas virtuales con Windows Server 2008

SQL Server 2008 y 2008 R2 instalados en Windows Server 2008 se pueden registrar en el proveedor de recursos de la máquina virtual de SQL en modo [NoAgent](virtual-machines-windows-sql-server-agent-extension.md). Esta opción garantiza el cumplimiento de normas y permite que la VM con SQL Server se supervise en Azure Portal con una funcionalidad limitada.

En la tabla siguiente se muestran los valores aceptables de los parámetros que se proporcionan durante el registro:

| Parámetro | Valores aceptables                                 |
| :------------------| :--------------------------------------- |
| **sqlLicenseType** | `'AHUB'` o `'PAYG'`                    |
| **sqlImageOffer**  | `'SQL2008-WS2008'` o `'SQL2008R2-WS2008`|
| &nbsp;             | &nbsp;                                   |


Para registrar SQL Server 2008 o 2008 R2 en una instancia de Windows Server 2008, use el siguiente fragmento de código de Powershell:  

  ```powershell-interactive
     # Get the existing  Compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
    New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
      -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
      -Properties @{virtualMachineResourceId=$vm.Id;SqlServerLicenseType='AHUB'; `
       sqlManagement='NoAgent';sqlImageSku='Standard';sqlImageOffer='SQL2008R2-WS2008'}
  ```

## <a name="verify-registration-status"></a>Comprobación del estado de registro
Puede comprobar si su servidor SQL Server ya se ha registrado en el proveedor de recursos de VM de SQL mediante Azure Portal, la CLI de Azure o PowerShell. 

### <a name="azure-portal"></a>Portal de Azure 
Para comprobar el estado del registro mediante Azure Portal, siga estos pasos.

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com). 
1. Vaya a sus [máquinas virtuales con SQL](virtual-machines-windows-sql-manage-portal.md).
1. Seleccione la máquina con SQL Server en la lista. Si la máquina virtual con SQL Server no aparece en la lista, es probable que no se haya registrado en el proveedor de recursos de VM de SQL. 
1. Vea el valor de `Status`. Si `Status = Succeeded`, la máquina virtual con SQL Server se ha registrado en el proveedor de recursos de VM de SQL correctamente. 

    ![Comprobación del estado con el registro en un proveedor de recursos de SQL](media/virtual-machines-windows-sql-register-with-rp/verify-registration-status.png)

### <a name="az-cli"></a>CLI de Azure

Compruebe el estado actual del registro de una máquina virtual con SQL Server con el siguiente comando de la CLI de Azure. `ProvisioningState` mostrará `Succeeded` si el registro fue correcto. 

  ```azurecli-interactive
  az sql vm show -n <vm_name> -g <resource_group>
  ```


### <a name="powershell"></a>PowerShell

Compruebe el estado actual del registro de una máquina virtual con SQL Server con el siguiente cmdlet de PowerShell. `ProvisioningState` mostrará `Succeeded` si el registro fue correcto. 

  ```powershell-interactive
  Get-AzResource -ResourceName <vm_name> -ResourceGroupName <resource_group> -ResourceType Microsoft.SqlVirtualMachine/sqlVirtualMachines
  ```
Un error indica que la VM con SQL Server no se ha registrado en el proveedor de recursos. 

---

## <a name="register-sql-vm-resource-provider-with-subscription"></a>Registro del proveedor de recursos de VM con SQL en la suscripción 

Para registrar la VM con SQL Server en el proveedor de recursos de VM con SQL, debe registrar dicho proveedor en su suscripción. Puede hacerlo con Azure Portal o mediante la CLI de Azure.

### <a name="azure-portal"></a>Portal de Azure

Los siguientes pasos le ayudarán a registrar al proveedor de recursos de VM de SQL en su suscripción de Azure mediante Azure Portal. 

1. Abra Azure Portal y vaya a **Todos los servicios**. 
1. Vaya a **Suscripciones** y seleccione la suscripción que le interese.  
1. En la página **Suscripciones**, vaya a **Proveedores de recursos**. 
1. Escriba `sql` en el filtro para mostrar los proveedores de recursos relacionados con SQL. 
1. Seleccione *Registrar*, *Volver a registrar* o *Anular registro* para el proveedor **Microsoft.SqlVirtualMachine**, según la acción que quiera realizar. 

   ![Modificación del proveedor](media/virtual-machines-windows-sql-ahb/select-resource-provider-sql.png)

### <a name="az-cli"></a>CLI de Azure
El siguiente fragmento de código registrará el proveedor de recursos de VM de SQL en su suscripción de Azure. 

```azurecli-interactive
# Register the new SQL VM resource provider to your subscription 
az provider register --namespace Microsoft.SqlVirtualMachine 
```

### <a name="powershell"></a>PowerShell

El siguiente fragmento de código de PowerShell registrará el proveedor de recursos de VM de SQL en su suscripción de Azure.

```powershell-interactive
# Register the new SQL VM resource provider to your subscription
Register-AzResourceProvider -ProviderNamespace Microsoft.SqlVirtualMachine
```
---

## <a name="remarks"></a>Comentarios

 - El proveedor de recursos de VM de SQL solo admite VM con SQL Server implementado mediante ''Resource Manager''. Las VM con SQL Server implementadas mediante el ''modelo clásico'' no se admiten. 
 - El proveedor de recursos de VM de SQL solo admite VM con SQL Server implementadas en la nube pública. No se admiten implementaciones en la nube privada ni en la nube de administración pública. 
 
## <a name="frequently-asked-questions"></a>Preguntas más frecuentes 

**¿Debo registrar mi VM de SQL Server aprovisionada a partir de una imagen de SQL en Azure Marketplace?**

No. Microsoft registra automáticamente las VM aprovisionadas a partir de imágenes SQL en Azure Marketplace. El registro con el proveedor de recursos de VM de SQL solo es necesario si la VM NO se aprovisionó desde las imágenes SQL en Azure Marketplace y SQL Server se instaló automáticamente.

**¿El proveedor de recursos de VM de SQL está disponible para todos los clientes?** 

Sí. Los clientes deben registrar sus VM con SQL Server con el proveedor de recursos de VM de SQL si no usaron una imagen de SQL Server de Azure Marketplace y no se instaló automáticamente SQL Server o usan su propio VHD personalizado. Las VM propiedad de cualquier tipo de suscripción (Direct, EA y CSP) se pueden registrar con el proveedor de recursos de VM de SQL.

**¿Debo registrarme con el proveedor de recursos de VM de SQL si mi VM con SQL Server ya tiene instalada la extensión IaaS de SQL?**

Si la VM con SQL Server se instaló automáticamente, no se aprovisionó a partir de las imágenes SQL de Azure Marketplace, debe registrarse con el proveedor de recursos de VM de SQL aunque haya instalado la extensión IaaS de SQL. Al registrarse con el proveedor de recursos de VM de SQL, se crea un nuevo recurso de tipo Microsoft.SqlVirtualMachines. La instalación de la extensión IaaS de SQL no crea ese recurso.

**¿Cuál es el modo de administración SQL predeterminado al registrarse con el proveedor de recursos de VM de SQL?**

El modo de administración SQL predeterminado al registrarse con el proveedor de recursos de VM de SQL es _Completo_. Si no se establece la propiedad administración SQL al registrarse con el proveedor de recursos de VM de SQL, se establecerá el modo Administración completa. Tener la extensión IaaS de SQL instalada en la VM es el requisito previo para el registro con el proveedor de recursos de VM de SQL en el modo Administración completa.

**¿Cuáles son los requisitos previos para registrarse con el proveedor de recursos de VM de SQL?**

No hay ningún requisito previo para el registro con el proveedor de recursos de VM de SQL en modo ligero o sin agente. El requisito previo para el registro con el proveedor de recursos de VM de SQL en modo completo es tener la extensión IaaS de SQL instalada en la VM.

**¿Puedo registrarme con el proveedor de recurso de VM de SQL si no tengo la extensión IaaS de SQL instalada en la VM?**

Sí, puede registrarse con el proveedor de recursos de VM de SQL en el modo de administración ligera si no tiene la extensión IaaS de SQL instalada en la VM. En el modo ligero, el proveedor de recursos de VM de SQL usará una aplicación de consola para comprobar la versión y la edición de la instancia de SQL. La aplicación de consola se apagará después de comprobar que hay al menos una instancia de SQL en ejecución en la VM. El registro con el proveedor de recursos de VM de SQL en modo ligero no reiniciará SQL Server y no creará ningún agente en la VM.

**Durante el registro con el proveedor de recursos de VM de SQL, ¿se instalará un agente en mi VM?**

No. Al registrarse con el proveedor de recursos de VM de SQL solo se creará un nuevo recurso de metadatos y no se instalará ningún agente en la VM. La extensión IaaS de SQL solo es necesaria para habilitar la administración completa, por lo que si se actualiza el modo de administración de ligera a completa, se instalará la extensión IaaS de SQL y se reiniciará SQL Server.

**Durante el registro con el proveedor de recursos de VM de SQL, ¿se reiniciará SQL Server en mi VM?**

No. Al registrarse con el proveedor de recursos de VM de SQL solo se creará un nuevo recurso de metadatos y no se reiniciará SQL Server en la VM. Solo es necesario reiniciar SQL Server para instalar la extensión IaaS de SQL; y se necesita la extensión IaaS de SQL para habilitar la administración completa. Si se actualiza el modo de administración de ligera a completa, se instalará la extensión IaaS de SQL y se reiniciará SQL Server.

**¿Cuál es la diferencia entre los modos de administración ligera y sin agente al registrarse con el proveedor de recursos de VM de SQL?** 

El modo de administración sin agente solo está disponible para SQL Server 2008/R2 en Windows Server 2008 y es el único modo de administración disponible para estas versiones. En todas las demás versiones de SQL Server, los dos modos de administración disponibles son ligera y completa. El modo sin agente requiere que el cliente defina las propiedades de versión y edición de SQL Server. El modo ligero consulta la VM para detectar la versión y la edición de la instancia de SQL.

**¿Se puede registrar con el proveedor de recursos de VM de SQL en modo ligero o sin agente con la CLI de Azure?**

No. La propiedad modo de administración de SQL solo está disponible cuando se registra con el proveedor de recursos de VM de SQL con Azure PowerShell. La CLI de Azure no admite la configuración de la propiedad de administración de SQL y siempre se registra con el proveedor de recursos de VM de SQL en el modo predeterminado: administración completa.

**¿Puedo registrarme con el proveedor de recursos de VM de SQL sin especificar el tipo de licencia de SQL?**

No. El tipo de licencia de SQL no es una propiedad opcional al registrarse con el proveedor de recursos de MV de SQL. Tiene que establecer el tipo de licencia de SQL como PAYG o AHUB al registrarse con el proveedor de recursos de VM de SQL en todos los modos de administración (sin agente, ligera y completa) tanto con la CLI de AZ como con PowerShell.

**¿Se puede actualizar la extensión IaaS de SQL del modo sin agente al modo completo?**

No. La actualización del modo de administración de SQL a completo o ligero no está disponible para el modo sin agente. Se trata de una limitación técnica de Windows Server 2008.

**¿Se puede actualizar la extensión IaaS de SQL del modo ligero al modo completo?**

Sí. La actualización del modo de administración de SQL de ligero a completo se admite desde PowerShell o Azure Portal (es necesario reiniciar SQL Server).

**¿Se puede cambiar la extensión de IaaS de SQL del modo completo a los modos inferiores de administración sin agente o ligera?**

No. No se admite cambiar a una versión inferior del modo de administración de la extensión IaaS de SQL. El modo de administración de SQL no se puede cambiar a una versión anterior, del modo completo al modo ligero o sin agente, ni del modo ligero al modo sin agente. Para cambiar el modo de administración completa, quite la extensión IaaS de SQL, anule el recurso Microsoft.SqlVirtualMachine y vuelva a registrar la VM de SQL Server con el proveedor de recursos de VM de SQL.

**¿Puedo registrarme con un proveedor de recursos de VM de SQL de Azure Portal?**

No. El registro con el proveedor de recursos de VM de SQL no está disponible en Azure Portal. El registro con el proveedor de recursos de VM de SQL en el modo de administración completa es compatible con la CLI de Azure o PowerShell, mientras que el registro con el proveedor de recursos de VM de SQL en los modos de administración ligera o sin agente solo es compatible con las API de Azure PowerShell.

**¿Puedo registrar una VM con el proveedor de recursos de VM de SQL antes de instalar SQL Server?**

No. La VM debe tener al menos una instancia de SQL para registrarse correctamente con el proveedor de recursos de VM de SQL. Si no hay ninguna instancia de SQL en la VM, el nuevo recurso Microsoft.SqlVirtualMachine tendrá el estado de error.

**¿Puedo registrar una VM con un recurso de VM de SQL si hay varias instancias de SQL?**

Sí. El proveedor de recursos de VM de SQL solo registrará una instancia de SQL. El proveedor de recursos de VM de SQL registrará la instancia de SQL predeterminada en caso de varias instancias. Si no hay ninguna instancia predeterminada, solo se admite el registro en modo ligero. Para actualizar del modo ligero al modo de administración completa, debe existir la instancia de SQL predeterminada o la VM debe tener solo una instancia de SQL con nombre.

**¿Se puede registrar una instancia de clúster de conmutación por error de SQL Server con el proveedor de recursos de VM de SQL?**

Sí. Las instancias de FCI de SQL en una VM de Azure se pueden registrar con el proveedor de recursos de VM de SQL en modo ligero. Sin embargo, las instancias de FCI de SQL no se pueden actualizar al modo de administración completa.

**¿Puedo registrar mi VM con el proveedor de recursos de VM de SQL si el grupo de disponibilidad AlwaysOn está configurado?**

Sí. No hay ninguna restricción para registrar una instancia de SQL Server en una VM de Azure con el proveedor de recursos de VM de SQL si participa en una configuración de grupo de disponibilidad AlwaysOn.

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte los siguientes artículos. 

* [Introducción a SQL Server en máquinas virtuales Windows](virtual-machines-windows-sql-server-iaas-overview.md)
* [Preguntas más frecuentes para SQL Server en máquinas virtuales Windows](virtual-machines-windows-sql-server-iaas-faq.md)
* [Orientación de precios de SQL Server para máquinas virtuales Windows](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server on a Windows VM release notes](virtual-machines-windows-sql-server-iaas-release-notes.md) (Notas de la versión de SQL Server en máquinas virtuales Windows)
