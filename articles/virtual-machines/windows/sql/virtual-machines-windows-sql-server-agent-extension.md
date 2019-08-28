---
title: Automatización de las tareas de administración en Azure Virtual Machines con la extensión del Agente de IaaS de SQL Server | Microsoft Docs
description: En este artículo se describe cómo administrar la extensión Agente de IaaS de SQL Server, que automatiza tareas de administración específicas de SQL Server. Entre ellas se incluyen la copia de seguridad automatizada, la aplicación de revisiones automatizada y la integración de Azure Key Vault.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: jroth
editor: ''
tags: azure-resource-manager
ms.assetid: effe4e2f-35b5-490a-b5ef-b06746083da4
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/24/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 59b5138950e0fb94ea0051fa9cfe9aa75cd7d770
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2019
ms.locfileid: "69877804"
---
# <a name="automate-management-tasks-on-azure-virtual-machines-by-using-the-sql-server-iaas-agent-extension"></a>Automatización de las tareas de administración en Azure Virtual Machines con la extensión del Agente de IaaS de SQL Server
> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-sql-server-agent-extension.md)
> * [Clásico](../sqlclassic/virtual-machines-windows-classic-sql-server-agent-extension.md)

La extensión del agente de IaaS SQL Server (SqlIaasExtension) se ejecuta en Azure Virtual Machines para automatizar las tareas de administración. En este artículo se proporciona información general de los servicios que admite la extensión. En este artículo también se proporcionan instrucciones para la instalación, el estado y la eliminación de la extensión.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Para ver la versión clásica de este artículo, consulte [Extensión del Agente de IaaS de SQL Server para VM con SQL Server (clásico)](../sqlclassic/virtual-machines-windows-classic-sql-server-agent-extension.md).

Hay tres modos de administración para la extensión IaaS de SQL Server: 

- El modo **Full** ofrece todas las funcionalidades, pero requiere reiniciar los permisos de administrador del sistema y SQL Server. Esta es la opción que se instala de manera predeterminada. Úselo para administrar una VM con SQL Server con una sola instancia. 

- El modo **Lightweight** no requiere reiniciar SQL Server, pero solo permite cambiar la edición y el tipo de licencia de SQL Server. Use esta opción para VM con SQL Server con varias instancias o para participar en una instancia de clúster de conmutación por error (FCI). 

- El modo **NoAgent** está dedicado a SQL Server 2008 y SQL Server 2008 R2 instalados en Windows Server 2008. Para obtener información sobre cómo usar este modo para la imagen de Windows Server 2008, consulte [Registro de Windows Server 2008](virtual-machines-windows-sql-register-with-resource-provider.md#register-sql-server-2008-or-2008-r2-on-windows-server-2008-vms). 

## <a name="supported-services"></a>Servicios admitidos
La extensión del Agente de IaaS SQL Server es compatible con las siguientes tareas de administración:

| Característica de administración | DESCRIPCIÓN |
| --- | --- |
| **Copia de seguridad automatizada de SQL Server** |Automatiza la programación de copias de seguridad de todas las bases de datos para la instancia predeterminada o una instancia con nombre [instalada de forma correcta](virtual-machines-windows-sql-server-iaas-faq.md#administration) de SQL Server en la máquina virtual. Para más información, consulte [Automated Backup para SQL Server en Azure Virtual Machines (Resource Manager)](virtual-machines-windows-sql-automated-backup.md). |
| **Aplicación de revisiones automatizada de SQL Server** |Configura una ventana de mantenimiento durante la cual tienen lugar actualizaciones importantes de Windows en la máquina virtual, de tal forma que puede evitar las actualizaciones durante las horas punta de la carga de trabajo. Para más información, consulte [Automated Patching para SQL Server en Azure Virtual Machines (Resource Manager)](virtual-machines-windows-sql-automated-patching.md). |
| **Integración de Azure Key Vault** |Permite instalar y configurar automáticamente Azure Key Vault en la máquina virtual SQL Server. Para más información, consulte [Configurar la integración de Azure Key Vault para SQL Server en Azure Virtual Machines (Resource Manager)](virtual-machines-windows-ps-sql-keyvault.md). |

Una vez que la extensión del Agente de IaaS de SQL Server está instalada y en ejecución, hace que las características de administración estén disponibles:

* En el panel de SQL Server de la máquina virtual en Azure Portal y a través de Azure PowerShell para las imágenes de SQL Server en Azure Marketplace.
* A través de Azure PowerShell para instalaciones manuales de la extensión. 

## <a name="prerequisites"></a>Requisitos previos
A continuación se indican los requisitos para usar la extensión del Agente de IaaS SQL Server en la VM:

**Sistema operativo**:

* Windows Server 2008 R2
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016
* Windows Server 2019 

**Versión de SQL Server**:

* SQL Server 2008 
* SQL Server 2008 R2
* SQL Server 2012
* SQL Server 2014
* SQL Server 2016
* SQL Server 2017

**Azure PowerShell**:

* [Descargar y configurar los comandos de Azure PowerShell más recientes](/powershell/azure/overview)

[!INCLUDE [updated-for-az.md](../../../../includes/updated-for-az.md)]


## <a name="change-management-modes"></a>Modos de la administración de cambios

Puede ver el modo actual del Agente de IaaS de SQL Server mediante PowerShell: 

  ```powershell-interactive
     #Get the SqlVirtualMachine
     $sqlvm = Get-AzResource -Name $vm.Name  -ResourceGroupName $vm.ResourceGroupName  -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines
     $sqlvm.Properties.sqlManagement
  ```

Las VM con SQL Server que tengan instalada la extensión IaaS *Lightweight*, puede actualizar el modo a _Full_ mediante Azure Portal. Las VM con SQL Server en el modo _NoAgent_ pueden actualizar a _Full_ tras actualizar el sistema operativo a Windows 2008 R2 y versiones posteriores. No se puede cambiar a una versión anterior. Para hacerlo, deberá desinstalar por completo la extensión IaaS de SQL e instalarla de nuevo. 

Para actualizar el modo del agente a Full: 


# <a name="azure-portaltabazure-portal"></a>[Azure Portal](#tab/azure-portal)

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Vaya al recurso [Máquinas virtuales SQL](virtual-machines-windows-sql-manage-portal.md#access-the-sql-virtual-machines-resource). 
1. Seleccione la máquina virtual con SQL Server y seleccione **Introducción**. 
1. En el caso de las VM con SQL Server con los modos de IaaS NoAgent o Lightweight, seleccione el mensaje **Only license type and edition updates are available with the SQL IaaS extension** (Solo las actualizaciones de la edición y el tipo de licencia están disponibles con la extensión IaaS de SQL).

   ![Selecciones para cambiar el modo desde el portal](media/virtual-machines-windows-sql-server-agent-extension/change-sql-iaas-mode-portal.png)

1. Seleccione la casilla **Acepto reiniciar el servicio SQL Server en la máquina virtual** y, a continuación, seleccione **Confirmar** para actualizar el modo de IaaS a Full. 

    ![Casilla para aceptar el reinicio del servicio SQL Server en la máquina virtual](media/virtual-machines-windows-sql-server-agent-extension/enable-full-mode-iaas.png)

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


##  <a name="installation"></a>Instalación
La extensión IaaS de SQL Server se instala al registrar la VM con SQL Server con el [proveedor de recursos de VM con SQL](virtual-machines-windows-sql-register-with-resource-provider.md). Si es necesario, puede instalar el agente de IaaS de SQL Server manualmente mediante el modo Full o Lightweight. 

La extensión del Agente de IaaS de SQL Server con el modo Full se instala automáticamente al aprovisionar una de las imágenes de Azure Marketplace de máquina virtual de SQL Server mediante Azure Portal. 

### <a name="install-in-full-mode"></a>Instalación en modo Full
El modo Full de la extensión IaaS de SQL Server ofrece una capacidad de administración completa para una única instancia en la VM con SQL Server. Si hay una instancia predeterminada, la extensión funcionará con la instancia predeterminada y no admitirá la administración de otras instancias. Si no hay ninguna instancia predeterminada, sino solo una instancia con nombre, administrará la instancia con nombre. Si no hay ninguna instancia predeterminada y hay varias instancias con nombre, no se podrá instalar la extensión. 

Instale el Agente de IaaS de SQL Server con el modo Full mediante PowerShell:

  ```powershell-interactive
     #Get the existing compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
     #Register the SQL Server VM with 'Full' SQL Server IaaS agent
     New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
        -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
        -Properties @{virtualMachineResourceId=$vm.Id;sqlServerLicenseType='AHUB';sqlManagement='Full'}  
  
  ```

| Parámetro | Valores aceptables                        |
| :------------------| :-------------------------------|
| **sqlServerLicenseType** | `AHUB` o `PAYG`     |
| &nbsp;             | &nbsp;                          |


> [!NOTE]
> Si la extensión aún no está instalada, al instalar la extensión Full, se reiniciará el servicio SQL Server. Para que no se reinicie el servicio SQL Server, instale el modo Lightweight con capacidad de administración limitada en su lugar.
> 
> Si actualiza la extensión IaaS de SQL Server, no se reinicia el servicio SQL Server. 

### <a name="install-on-a-vm-with-a-single-named-sql-server-instance"></a>Instalación en una máquina virtual con una única instancia de SQL Server con nombre
La extensión IaaS de SQL Server funcionará con una instancia con nombre en un servidor de SQL Server si la instancia predeterminada se ha desinstalado y se vuelve a instalar la extensión IaaS.

Para usar una instancia con nombre de SQL Server:
   1. Implemente una VM SQL Server desde Azure Marketplace. 
   1. Desinstale la extensión IaaS desde [Azure Portal](https://portal.azure.com).
   1. Desinstale SQL Server por completo de la máquina virtual SQL Server.
   1. Instale SQL Server con una instancia con nombre en la máquina virtual SQL Server. 
   1. Desinstale la extensión IaaS desde Azure Portal.  


### <a name="install-in-lightweight-mode"></a>Instalación en modo Lightweight
El modo Lightweight no reiniciará el servicio SQL Server, pero ofrece una funcionalidad limitada. 

Instale el Agente de IaaS de SQL Server con el modo Lightweight mediante PowerShell:


  ```powershell-interactive
     /#Get the existing  Compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
     #Register the SQL Server VM with the 'Lightweight' SQL IaaS agent
     New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
        -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
        -Properties @{virtualMachineResourceId=$vm.Id;sqlServerLicenseType='AHUB';sqlManagement='LightWeight'}  
  
  ```

| Parámetro | Valores aceptables                        |
| :------------------| :-------------------------------|
| **sqlServerLicenseType** | `AHUB` o `PAYG`     |
| &nbsp;             | &nbsp;                          |


## <a name="get-the-status-of-the-sql-server-iaas-extension"></a>Obtención del estado de la extensión IaaS de SQL Server
Una manera de comprobar que la extensión está instalada consiste en ver el estado del agente en Azure Portal. Seleccione **Todas las configuraciones** en la ventana de la máquina virtual y seleccione **Extensiones**. Debería aparecer la extensión **SqlIaasExtension**.

![Estado de la extensión del Agente de IaaS de SQL Server en Azure Portal](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-portal.png)

También puede usar el cmdlet de Azure PowerShell **Get-AzVMSqlServerExtension**:

   ```powershell-interactive
   Get-AzVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
   ```

El comando anterior confirma que el agente está instalado y proporciona información de estado general. Puede obtener información de estado específica sobre Automated Backup y Automated Patching con los siguientes comandos:

   ```powershell-interactive
    $sqlext = Get-AzVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings
   ```

## <a name="removal"></a>Eliminación
En Azure Portal, puede desinstalar la extensión si selecciona los puntos suspensivos de la ventana **Extensiones** de las propiedades de la máquina virtual. A continuación, seleccione **Eliminar**.

![Desinstalación de la extensión del Agente de IaaS de SQL Server en Azure Portal](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-uninstall.png)

También puede usar el cmdlet de PowerShell **Remove-AzVMSqlServerExtension**:

   ```powershell-interactive
    Remove-AzVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SqlIaasExtension"
   ```

## <a name="next-steps"></a>Pasos siguientes
Empiece utilizando uno de los servicios admitidos por la extensión. Para obtener más información, consulte los artículos a los que se hace referencia en la sección [Servicios admitidos](#supported-services) de este artículo.

Para obtener más información sobre cómo ejecutar SQL Server en Azure Virtual Machines, consulte [¿Qué es SQL Server en Azure Virtual Machines?](virtual-machines-windows-sql-server-iaas-overview.md).

