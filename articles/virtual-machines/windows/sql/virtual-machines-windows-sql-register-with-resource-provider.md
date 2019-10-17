---
title: Registro de una máquina virtual con SQL Server en Azure con el proveedor de recursos de máquina virtual con SQL | Microsoft Docs
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
ms.openlocfilehash: b0a7221107f05ff2239bd77cc18e7ffedc18efc1
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2019
ms.locfileid: "72023602"
---
# <a name="register-a-sql-server-virtual-machine-in-azure-with-the-sql-vm-resource-provider"></a>Registro de una máquina virtual con SQL Server en Azure con el proveedor de recursos de máquina virtual con SQL

En este artículo se describe cómo registrar una máquina virtual con SQL Server en Azure con el proveedor de recursos de máquina virtual con SQL. 

La implementación de una imagen de Azure Marketplace de una máquina virtual con SQL Server mediante Azure Portal registra la máquina virtual con SQL Server con el proveedor de recursos. Si elige instalar automáticamente SQL Server en una máquina virtual de Azure, en lugar de elegir una imagen de Azure Marketplace, o si aprovisiona una máquina virtual de Azure desde un disco duro virtual personalizado con SQL Server, debe registrar la máquina virtual con SQL Server con el proveedor de recursos para:

- **Simplificar la administración de licencias**: según los términos de los productos de Microsoft, los clientes deben indicar a Microsoft que usan [Ventaja híbrida de Azure](https://azure.microsoft.com/pricing/hybrid-benefit/). El registro en el proveedor de recursos de máquina virtual de SQL simplifica la administración de licencias de SQL Server y permite identificar rápidamente máquinas virtuales con SQL Server mediante Ventaja híbrida de Azure en el [portal](virtual-machines-windows-sql-manage-portal.md) o la CLI de Azure: 

   ```azurecli-interactive
   $vms = az sql vm list | ConvertFrom-Json
   $vms | Where-Object {$_.sqlServerLicenseType -eq "AHUB"}
   ```

- **Ventajas de las características**: al registrar de una máquina virtual con SQL Server con el proveedor de recursos, se desbloquean la [aplicación automática de revisiones](virtual-machines-windows-sql-automated-patching.md), la [copia de seguridad automática](virtual-machines-windows-sql-automated-backup-v2.md) y las funcionalidades de supervisión y administración. También se desbloquean las [licencias](virtual-machines-windows-sql-ahb.md) y la flexibilidad de [ediciones](virtual-machines-windows-sql-change-edition.md). Anteriormente, estas características solo estaban disponibles para las imágenes de máquina virtual con SQL Server de Azure Marketplace.

- **Administración gratuita**:  El registro en el proveedor de recursos de máquina virtual de SQL y todos los modos de administración son completamente gratis. No hay costos adicionales asociados al proveedor de recursos o al cambio de modo de administración. 

Para utilizar el proveedor de recursos de VM de SQL, también debe registrarlo con su suscripción. Para ello, puede usar Azure Portal, PowerShell o la CLI de Azure. 

  > [!NOTE]
  > No hay requisitos de licencias adicionales asociados al registro en el proveedor de recursos. El registro en el proveedor de recursos de máquina virtual de SQL ofrece un método simplificado para satisfacer el requisito de notificar a Microsoft que se ha habilitado Ventaja híbrida de Azure, en el lugar de administrar los formularios de registro de licencias para cada recurso. 

Para más información sobre las ventajas de usar el proveedor de recursos de máquina virtual de SQL, vea el siguiente vídeo de [channel9](https://channel9.msdn.com/Shows/Data-Exposed/Benefit-from-SQL-VM-Resource-Provider-when-self-installing-SQL-Server-on-Azure?WT.mc_id=dataexposed-c9-niner): 

<iframe src="https://channel9.msdn.com/Shows/Data-Exposed/Benefit-from-SQL-VM-Resource-Provider-when-self-installing-SQL-Server-on-Azure/player" width="960" height="540" allowFullScreen frameBorder="0" title="Ventajas del proveedor de recursos de máquina virtual de SQL al instalar automáticamente SQL Server en Azure: vídeo de Microsoft Channel 9"></iframe>


## <a name="prerequisites"></a>Requisitos previos

Para registrar una máquina virtual con SQL Server con el proveedor de recursos, necesita lo siguiente: 

- Una [suscripción de Azure](https://azure.microsoft.com/free/).
- Una [VM con SQL Server](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision). 
- La versión más reciente de la [CLI de Azure](/cli/azure/install-azure-cli) o [PowerShell](/powershell/azure/new-azureps-module-az). 


## <a name="register-with-sql-vm-resource-provider"></a>Registro en un proveedor de recursos de VM de SQL
Si la [extensión Agente de IaaS de SQL Server](virtual-machines-windows-sql-server-agent-extension.md) no está instalada en la máquina virtual, para registrarse con el proveedor de recursos de máquina virtual con SQL especifique el modo de administración de SQL ligero. 

Cuando se especifica dicho modo de administración durante el proceso de registro, el proveedor de recursos de VM de SQL instalará automáticamente la extensión SQL IaaS en [modo ligero](#change-management-modes) y comprobará los metadatos de la instancia de SQL Server; esta operación no reiniciará el servicio SQL Server. Deberá especificar el tipo de licencia de SQL Server que desea al registrarse en un proveedor de recursos de VM de SQL como "PAYG" o "AHUB".

El registro en el proveedor de recursos de VM de SQL en modo ligero garantizará el cumplimiento y habilitará la concesión de licencias flexible, así como las actualizaciones de la edición de SQL Server en vigor. Tanto las instancias del clúster de conmutación por error como las implementaciones de instancias múltiples solo se pueden registrar en el proveedor de recursos de VM de SQL en el modo ligero. Puede [actualizar](#change-management-modes) al modo de administración completa en cualquier momento, pero si lo hace, se reiniciará el servicio de SQL Server. 


# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
Use el siguiente fragmento de código para realizar el registro con el proveedor de recursos de máquina virtual con SQL si la extensión IaaS de SQL Server ya está instalada en la máquina virtual. Debe especificar el tipo de licencia de SQL Server que desee al realizar el registro con el proveedor de recursos de máquina virtual con SQL: pago por uso (`PAYG`) o Ventaja híbrida de Azure (`AHUB`). 

Registre la máquina virtual con SQL Server con el siguiente fragmento de código de PowerShell:

  ```powershell-interactive
     # Get the existing compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
     # Register SQL VM with 'Lightweight' SQL IaaS agent
     New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
        -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
        -Properties @{virtualMachineResourceId=$vm.Id;SqlServerLicenseType='PAYG';sqlManagement='LightWeight'}  
  
  ```

# <a name="az-clitabbash"></a>[CLI de AZ](#tab/bash)

Para ediciones de pago (Enterprise o Estándar):

  ```azurecli-interactive
  # Register Enterprise or Standard self-installed VM in Lightweight mode

  az sql vm create --name <vm_name> --resource-group <resource_group_name> --location <vm_location> --license-type PAYG 

  ```

Para ediciones gratuitas (Developer, Web o Express):

  ```azurecli-interactive
  # Register Developer, Web, or Express self-installed VM in Lightweight mode

  az sql vm create --name <vm_name> --resource-group <resource_group_name> --location <vm_location> --license-type PAYG 
  ```
---

Si la extensión IaaS de SQL ya se instaló manualmente en la máquina virtual, puede registrarse en el proveedor de recursos de máquina virtual con SQL en modo completo con solo crear un recurso de metadatos del tipo Microsoft.SqlVirtualMachine/SqlVirtualMachines. A continuación encontrará el fragmento de código para registrarse en el proveedor de recursos de VM de SQL si la extensión SQL IaaS ya está instalada en la máquina virtual. Debe proporcionar el tipo de SQL Server licencia deseado como "Pago por uso" o "Ventaja para uso híbrido de Azure". Para registrarse en el modo de administración completa, use el siguiente comando de PowerShell:

  ```powershell-interactive
  # Get the existing  Compute VM
   $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
        
   # Register with SQL VM resource provider
   New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
      -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
      -Properties @{virtualMachineResourceId=$vm.Id;SqlServerLicenseType='PAYG'}
  ```


## <a name="register-sql-server-2008-or-2008-r2-on-windows-server-2008-vms"></a>Registro de SQL Server 2008 o 2008 R2 en máquinas virtuales Windows Server 2008

SQL Server 2008 y 2008 R2 instalados en Windows Server 2008 se pueden registrar con el proveedor de recursos de máquina virtual con SQL en modo [sin agente](#change-management-modes). Esta opción garantiza el cumplimiento de normas y permite que la VM con SQL Server se supervise en Azure Portal con una funcionalidad limitada.

En la tabla siguiente se muestran los valores aceptables de los parámetros que se proporcionan durante el registro:

| Parámetro | Valores aceptables                                 |
| :------------------| :--------------------------------------- |
| **sqlLicenseType** | `AHUB` o `PAYG`                    |
| **sqlImageOffer**  | `SQL2008-WS2008` o `SQL2008R2-WS2008`|
| &nbsp;             | &nbsp;                                   |


Para registrar su instancia de SQL Server 2008 o 2008 R2 en una instancia de Windows Server 2008, use el siguiente fragmento de código de Powershell o la CLI de Azure:  

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
  ```powershell-interactive
     # Get the existing compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
    New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
      -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
      -Properties @{virtualMachineResourceId=$vm.Id;SqlServerLicenseType='PAYG'; `
       sqlManagement='NoAgent';sqlImageSku='Standard';sqlImageOffer='SQL2008R2-WS2008'}
  ```

# <a name="az-clitabbash"></a>[CLI de AZ](#tab/bash)

  ```azurecli-interactive
   az sql vm create -n sqlvm -g myresourcegroup -l eastus |
   --license-type PAYG --sql-mgmt-type NoAgent 
   --image-sku Enterprise --image-offer SQL2008-WS2008R2
 ```

---

## <a name="verify-registration-status"></a>Comprobación del estado de registro
Puede comprobar si su máquina virtual con SQL Server ya se ha registrado con el proveedor de recursos de máquina virtual con SQL mediante Azure Portal, la CLI de Azure o PowerShell. 

### <a name="azure-portal"></a>Portal de Azure 

1. Inicie sesión en el [Azure Portal](https://portal.azure.com). 
1. Vaya a [Máquinas virtuales SQL Server](virtual-machines-windows-sql-manage-portal.md).
1. Seleccione la máquina con SQL Server en la lista. Si la máquina virtual con SQL Server no aparece en la lista, es probable que no se haya registrado con el proveedor de recursos de máquina virtual con SQL. 
1. Vea el valor de **Estado**. Si el valor de **Estado** es **Correcto**, la máquina virtual con SQL Server se ha registrado con el proveedor de recursos de máquina virtual con SQL correctamente. 

![Comprobación del estado con el registro en un proveedor de recursos de SQL](media/virtual-machines-windows-sql-register-with-rp/verify-registration-status.png)

### <a name="command-line"></a>Línea de comandos

Compruebe el estado actual del registro de una máquina virtual con SQL Server con la CLI de Azure o PowerShell. `ProvisioningState` mostrará `Succeeded` si el registro fue correcto. 

# <a name="az-clitabbash"></a>[CLI de AZ](#tab/bash)


  ```azurecli-interactive
  az sql vm show -n <vm_name> -g <resource_group>
 ```


# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

  ```powershell-interactive
  Get-AzResource -ResourceName <vm_name> -ResourceGroupName <resource_group> `
  -ResourceType Microsoft.SqlVirtualMachine/sqlVirtualMachines
  ```

---

Un error indica que la VM con SQL Server no se ha registrado en el proveedor de recursos. 

## <a name="change-management-modes"></a>Modos de la administración de cambios

Hay tres modos de administración de la manejabilidad para la extensión IaaS de SQL Server: 

- El modo **Full** ofrece todas las funcionalidades, pero requiere reiniciar los permisos de administrador del sistema y SQL Server. Esta es la opción que se instala de manera predeterminada. Úselo para administrar una VM con SQL Server con una sola instancia. El modo completo instala dos servicios de Windows que tienen un impacto mínimo en la memoria y la CPU (se pueden supervisar mediante el administrador de tareas). No hay ningún costo asociado al uso del modo de administración completa. 

- El modo **Lightweight** no requiere reiniciar SQL Server, pero solo permite cambiar la edición y el tipo de licencia de SQL Server. Use esta opción para VM con SQL Server con varias instancias o para participar en una instancia de clúster de conmutación por error (FCI). Si se usa el modo ligero, no se produce ningún impacto en la memoria ni en la CPU. No hay ningún costo asociado al uso del modo de administración ligera. 

- El modo **NoAgent** está dedicado a SQL Server 2008 y SQL Server 2008 R2 instalados en Windows Server 2008. Si se usa el modo NoAgent, ni la memoria ni la CPU resultan afectadas. No hay ningún costo asociado al uso del modo de administración NoAgent. 

Puede ver el modo actual del Agente de IaaS de SQL Server mediante PowerShell: 

  ```powershell-interactive
     #Get the SqlVirtualMachine
     $sqlvm = Get-AzResource -Name $vm.Name  -ResourceGroupName $vm.ResourceGroupName  -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines
     $sqlvm.Properties.sqlManagement
  ```

Las VM con SQL Server que tengan instalada la extensión IaaS *Lightweight*, puede actualizar el modo a _Full_ mediante Azure Portal. Las VM con SQL Server en el modo _NoAgent_ pueden actualizar a _Full_ tras actualizar el sistema operativo a Windows 2008 R2 y versiones posteriores. No se puede cambiar a una versión anterior. Para hacerlo, deberá desinstalar por completo la extensión IaaS de SQL e instalarla de nuevo. 

Para actualizar el modo del agente a Full: 


### <a name="azure-portal"></a>Portal de Azure

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Vaya al recurso [Máquinas virtuales SQL](virtual-machines-windows-sql-manage-portal.md#access-the-sql-virtual-machines-resource). 
1. Seleccione la máquina virtual con SQL Server y seleccione **Introducción**. 
1. En el caso de las VM con SQL Server con los modos de IaaS NoAgent o Lightweight, seleccione el mensaje **Only license type and edition updates are available with the SQL IaaS extension** (Solo las actualizaciones de la edición y el tipo de licencia están disponibles con la extensión IaaS de SQL).

   ![Selecciones para cambiar el modo desde el portal](media/virtual-machines-windows-sql-server-agent-extension/change-sql-iaas-mode-portal.png)

1. Seleccione la casilla **Acepto reiniciar el servicio SQL Server en la máquina virtual** y, a continuación, seleccione **Confirmar** para actualizar el modo de IaaS a Full. 

    ![Casilla para aceptar el reinicio del servicio SQL Server en la máquina virtual](media/virtual-machines-windows-sql-server-agent-extension/enable-full-mode-iaas.png)

### <a name="command-line"></a>Línea de comandos

# <a name="az-clitabbash"></a>[CLI de AZ](#tab/bash)

Ejecute el siguiente fragmento de código de la CLI de Azure:

  ```azurecli-interactive
  # Update to full mode

  az sql vm update --name <vm_name> --resource-group <resource_group_name> --sql-mgmt-type full  
  ```

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

Ejecute el siguiente fragmento de código de PowerShell:

  ```powershell-interactive
  # Update to full mode

  $SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
  $SqlVm.Properties.sqlManagement="Full"
  $SqlVm | Set-AzResource -Force
  ```
---

## <a name="register-the-sql-vm-resource-provider-with-a-subscription"></a>Registro de un proveedor de recursos de máquina virtual con SQL en una suscripción 

Para registrar la máquina virtual con SQL Server con el proveedor de recursos de máquina virtual con SQL, debe registrar dicho proveedor en su suscripción. Para ello, puede usar Azure Portal, PowerShell o la CLI de Azure.

### <a name="azure-portal"></a>Portal de Azure

1. Abra Azure Portal y vaya a **Todos los servicios**. 
1. Vaya a **Suscripciones** y seleccione la suscripción que le interese.  
1. En la página **Suscripciones**, vaya a **Proveedores de recursos**. 
1. Escriba **sql** en el filtro para mostrar los proveedores de recursos relacionados con SQL. 
1. Seleccione **Registrar**, **Volver a registrar** o **Anular registro** para el proveedor **Microsoft.SqlVirtualMachine**, según la acción que quiera realizar. 

![Modificación del proveedor](media/virtual-machines-windows-sql-ahb/select-resource-provider-sql.png)


### <a name="command-line"></a>Línea de comandos

Registre el proveedor de recursos de máquina virtual con SQL en su suscripción de Azure mediante la CLI de Azure o PowerShell. 

# <a name="az-clitabbash"></a>[CLI de AZ](#tab/bash)
El siguiente fragmento de código registrará el proveedor de recursos de VM de SQL en su suscripción de Azure. 

```azurecli-interactive
# Register the new SQL VM resource provider to your subscription 
az provider register --namespace Microsoft.SqlVirtualMachine 
```

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

```powershell-interactive
# Register the new SQL VM resource provider to your subscription
Register-AzResourceProvider -ProviderNamespace Microsoft.SqlVirtualMachine
```
---

## <a name="remarks"></a>Comentarios

- El proveedor de recursos de máquina virtual con SQL solo admite máquinas virtuales con SQL Server implementadas mediante Azure Resource Manager. No se admiten las máquinas virtuales con SQL Server implementadas con el modelo clásico. 
- El proveedor de recursos de máquina virtual con SQL solo admite máquinas virtuales con SQL Server implementadas en la nube pública. No se admiten implementaciones en la nube privada ni en la nube de administración pública. 
 

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes 

**¿Debo registrar mi máquina virtual con SQL Server aprovisionada a partir de una imagen de SQL Server en Azure Marketplace?**

No. Microsoft registra automáticamente las máquinas virtuales aprovisionadas a partir de imágenes de SQL Server en Azure Marketplace. El registro con el proveedor de recursos de máquina virtual con SQL solo es necesario si la máquina virtual *no* se aprovisionó a partir de las imágenes de SQL Server en Azure Marketplace y SQL Server se instaló automáticamente.

**¿El proveedor de recursos de VM de SQL está disponible para todos los clientes?** 

Sí. Los clientes deben registrar sus máquinas virtuales con SQL Server con el proveedor de recursos de máquina virtual con SQL si no usaron una imagen de SQL Server de Azure Marketplace y, en su lugar, se instaló automáticamente SQL Server o si usan su propio VHD personalizado. Las máquinas virtuales propiedad de todos los tipos de suscripciones (directa, Contrato Enterprise y proveedor de soluciones en la nube) se pueden registrar con el proveedor de recursos de máquina virtual con SQL.

**¿Debo registrarme con el proveedor de recursos de máquina virtual con SQL si mi máquina virtual con SQL Server ya tiene instalada la extensión IaaS de SQL Server?**

Si la máquina virtual con SQL Server se instaló automáticamente y no se aprovisionó a partir de las imágenes de SQL Server de Azure Marketplace, debe registrarla con el proveedor de recursos de máquina virtual con SQL aunque haya instalado la extensión IaaS de SQL Server. Al registrarse con el proveedor de recursos de máquina virtual con SQL, se crea un nuevo recurso de tipo Microsoft.SqlVirtualMachines. La instalación de la extensión IaaS de SQL Server no crea ese recurso.

**¿Cuál es el modo de administración predeterminado al registrarse con el proveedor de recursos de máquina virtual con SQL?**

El modo de administración predeterminado al registrarse con el proveedor de recursos de máquina virtual con SQL es *Completo*. Si no se establece la propiedad de administración de SQL Server al registrarse con el proveedor de recursos de máquina virtual con SQL, se establecerá el modo de administración completa. Tener la extensión IaaS de SQL Server instalada en la máquina virtual es el requisito previo para el registro con el proveedor de recursos de máquina virtual con SQL en el modo de administración completa.

**¿Cuáles son los requisitos previos para registrarse con el proveedor de recursos de máquina virtual con SQL?**

No hay ningún requisito previo para el registro con el proveedor de recursos de VM de SQL en modo ligero o sin agente. El requisito previo para el registro con el proveedor de recursos de máquina virtual con SQL en modo completo es tener la extensión IaaS de SQL Server instalada en la máquina virtual.

**¿Puedo realizar el registro con el proveedor de recursos de máquina virtual con SQL si no tengo la extensión IaaS de SQL Server instalada en la máquina virtual?**

Sí, puede registrarse con el proveedor de recursos de máquina virtual con SQL en el modo de administración ligera si no tiene la extensión IaaS de SQL Server instalada en la máquina virtual. En el modo ligero, el proveedor de recursos de máquina virtual con SQL usará una aplicación de consola para comprobar la versión y la edición de la instancia de SQL Server. 

El modo de administración SQL predeterminado al registrarse con el proveedor de recursos de máquina virtual con SQL es _Completo_. Si no se establece la propiedad administración SQL al registrarse con el proveedor de recursos de VM de SQL, se establecerá el modo Administración completa. Tener la extensión IaaS de SQL instalada en la VM es el requisito previo para el registro con el proveedor de recursos de VM de SQL en el modo Administración completa.

**Durante el registro con el proveedor de recursos de máquina virtual con SQL, ¿se instalará un agente en mi máquina virtual?**

No. Al registrarse con el proveedor de recursos de máquina virtual con SQL solo se creará un nuevo recurso de metadatos. No se instalará ningún agente en la máquina virtual.

La extensión IaaS de SQL Server solo es necesaria para habilitar el modo de administración completa. Si se actualiza el modo de administración de ligera a completa, se instalará la extensión IaaS de SQL Server y se reiniciará SQL Server.

**Durante el registro con el proveedor de recursos de máquina virtual con SQL Server, ¿se reiniciará SQL Server en mi máquina virtual?**

No. Al registrarse con el proveedor de recursos de máquina virtual con SQL solo se creará un nuevo recurso de metadatos. No se reiniciará SQL Server en la máquina virtual. 

Solo es necesario reiniciar SQL Server para instalar la extensión IaaS de SQL Server. Esta extensión solo es necesaria para habilitar el modo de administración completa. Si se actualiza el modo de administración de ligera a completa, se instalará la extensión IaaS de SQL Server y se reiniciará SQL Server.

**¿Cuál es la diferencia entre los modos de administración ligera y sin agente al registrarse con el proveedor de recursos de máquina virtual con SQL?** 

El modo de administración sin agente solo está disponible para SQL Server 2008 y SQL Server 2008 R2 en Windows Server 2008. Es el único modo de administración disponible para estas versiones. En todas las demás versiones de SQL Server, los dos modos de administración disponibles son ligera y completa. 

El modo sin agente requiere que el cliente establezca las propiedades de la versión y edición de SQL Server. El modo ligero consulta a la máquina virtual para encontrar la versión y la edición de la instancia de SQL Server.

**¿Se puede registrar con el proveedor de recursos de máquina virtual con SQL en modo ligero o sin agente con la CLI de Azure?**

No. La propiedad de modo de administración solo está disponible cuando se registra con el proveedor de recursos de máquina virtual con SQL con Azure PowerShell. La CLI de Azure no admite el establecimiento de la propiedad de administración de SQL Server. Siempre se registra con el proveedor de recursos de máquina virtual con SQL en el modo predeterminado (administración completa).

**¿Puedo registrarme con el proveedor de recursos de máquina virtual con SQL sin especificar el tipo de licencia de SQL Server?**

No. El tipo de licencia de SQL Server no es una propiedad opcional cuando se registra con el proveedor de recursos de máquina virtual con SQL. Tiene que establecer el tipo de licencia de SQL Server como pago por uso o Ventaja híbrida de Azure al registrarse con el proveedor de recursos de máquina virtual con SQL en todos los modos de administración (sin agente, ligera y completa) tanto con la CLI de Azure como con PowerShell.

**¿Se puede actualizar la extensión IaaS de SQL Server del modo sin agente al modo completo?**

No. La actualización del modo de administración a completo o ligero no está disponible para el modo sin agente. Se trata de una limitación técnica de Windows Server 2008.

**¿Se puede actualizar la extensión IaaS de SQL Server del modo ligero al modo completo?**

Sí. La actualización del modo de administración de ligero a completo se admite desde PowerShell o Azure Portal. Es necesario reiniciar SQL Server.

**¿Se puede cambiar la extensión de IaaS de SQL Server del modo completo a los modos inferiores de administración sin agente o ligera?**

No. No se admite cambiar a una versión inferior del modo de administración de la extensión IaaS de SQL Server. El modo de administración no se puede cambiar a una versión inferior, del modo completo al modo ligero o sin agente, ni del modo ligero al modo sin agente. 

Para cambiar el modo de administración completa, quite la extensión IaaS de SQL Server. A continuación, quite el recurso Microsoft.SqlVirtualMachine y vuelva a registrar la máquina virtual con SQL Server utilizando el proveedor de recursos de máquinas virtuales con SQL.

**¿Puedo registrarme en un proveedor de recursos de máquina virtual con SQL desde Azure Portal?**

No. El registro en un proveedor de recursos de máquina virtual con SQL no está disponible en Azure Portal. El registro con el proveedor de recursos de máquina virtual con SQL en el modo de administración completa se admite con la CLI de Azure o PowerShell. El registro con el proveedor de recursos de máquina virtual con SQL en el modo de administración ligero o sin agente solo es compatible con API de Azure PowerShell.

**¿Puedo registrar una máquina virtual con el proveedor de recursos de máquina virtual con SQL antes de instalar SQL Server?**

No. Una máquina virtual debe tener al menos una instancia de SQL Server para registrarse correctamente con el proveedor de recursos de máquina virtual con SQL. Si no hay ninguna instancia de SQL Server en la máquina virtual, el nuevo recurso Microsoft.SqlVirtualMachine tendrá un estado de error.

**¿Puedo registrar una máquina virtual en un proveedor de recursos de máquina virtual con SQL si hay varias instancias de SQL Server?**

Sí. El proveedor de recursos de máquina virtual con SQL solo registrará una instancia de SQL Server. El proveedor de recursos de máquina virtual con SQL registrará la instancia de SQL Server predeterminada en caso de varias instancias. Si no hay ninguna instancia predeterminada, solo se admite el registro en modo ligero. Para actualizar del modo ligero al modo de administración completa, debe existir la instancia de SQL Server predeterminada o la máquina virtual debe tener solo una instancia de SQL Server con nombre.

**¿Se puede registrar una instancia de clúster de conmutación por error de SQL Server con el proveedor de recursos de máquina virtual con SQL?**

Sí. Las instancias de clúster de conmutación por error de SQL Server en una máquina virtual de Azure se pueden registrar con el proveedor de recursos de máquina virtual con SQL en modo ligero. Sin embargo, las instancias de clúster de conmutación por error de SQL Server no se pueden actualizar al modo de administración completa.

**¿Puedo registrar mi máquina virtual con el proveedor de recursos de máquina virtual con SQL si el grupo de disponibilidad AlwaysOn está configurado?**

Sí. No hay ninguna restricción para registrar una instancia de SQL Server instalada en una máquina virtual de Azure con el proveedor de recursos de máquina virtual con SQL si participa en una configuración de grupo de disponibilidad AlwaysOn.

**¿Cuál es el costo del registro en el proveedor de recursos de máquina virtual de SQL o de la actualización al modo de administración completa?**
Ninguno. No hay ningún precio asociado al registro en el proveedor de recursos de máquina virtual de SQL ni al uso de ninguno de los tres modos de manejabilidad. La administración de una VM con SQL Server con el proveedor de recursos es totalmente gratuita. 

**¿Como afecta el uso de los distintos modos de administración al rendimiento?**
El uso de los modos de manejabilidad *NoAgent* y *ligero* no tiene ningún impacto en el rendimiento. Si se usa el modo de manejabilidad *completa* desde dos servicios instalados en el sistema operativo, el impacto es mínimo. Dichos servicios se pueden supervisar desde el administrador de tareas. 

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte los siguientes artículos. 

* [Introducción a SQL Server en máquinas virtuales Windows](virtual-machines-windows-sql-server-iaas-overview.md)
* [Preguntas más frecuentes de SQL Server en máquinas virtuales Windows](virtual-machines-windows-sql-server-iaas-faq.md)
* [Orientación de precios de SQL Server para máquinas virtuales de Azure](virtual-machines-windows-sql-server-pricing-guidance.md)
* [Notas de la versión de SQL Server en Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-release-notes.md)
