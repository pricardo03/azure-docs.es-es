---
title: Automatización de las tareas de administración en Azure Virtual Machines con la extensión del Agente de IaaS de SQL Server | Microsoft Docs
description: En este artículo se describe cómo administrar la extensión Agente de SQL Server, que automatiza tareas de administración específicas de SQL Server. Entre ellas se incluyen la copia de seguridad automatizada, la aplicación de revisiones automatizada y la integración de Azure Key Vault.
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
ms.openlocfilehash: 41023103dc30d16f599e847f9d324bc7bb4be11c
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67798050"
---
# <a name="automate-management-tasks-on-azure-virtual-machines-with-the-sql-server-iaas-agent-extension"></a>Automatización de las tareas de administración en Azure Virtual Machines con la extensión del Agente de IaaS de SQL Server
> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-sql-server-agent-extension.md)
> * [Clásico](../sqlclassic/virtual-machines-windows-classic-sql-server-agent-extension.md)

La extensión del agente de IaaS SQL Server (SqlIaasExtension) se ejecuta en Azure Virtual Machines para automatizar las tareas de administración. En este artículo se proporciona una introducción y los servicios admitidos por la extensión, así como instrucciones sobre la instalación, el estado y la eliminación.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Para ver la versión clásica de este artículo, consulte [Extensión del Agente SQL Server para VM con SQL Server (clásico)](../sqlclassic/virtual-machines-windows-classic-sql-server-agent-extension.md).

Hay tres modos de administración de SQL para la extensión IaaS de SQL: **Full**, **Lightweight** y **NoAgent**. 

- El modo **Full** ofrece todas las funcionalidades, pero requiere reiniciar los permisos de SA y SQL Server. Esta es la opción que está instalada de forma predeterminada y debe usarse para administrar una VM con SQL Server con una sola instancia. 

- El modo **Lightweight** no requiere reiniciar SQL Server, pero solo permite cambiar la edición y el tipo de licencia de SQL Server. Esta opción debe usarse para VM con SQL Server con varias instancias o para participar en una instancia de clúster de conmutación por error (FCI). 

- El modo **NoAgent** está dedicado a SQL Server 2008 y SQL Server 2008 R2 instalados en Windows Server 2008. Para obtener información sobre cómo usar el modo `NoAgent` para la imagen de Windows Server 2008, consulte [Registro de Windows Server 2008](virtual-machines-windows-sql-register-with-resource-provider.md#register-sql-server-2008r2-on-windows-server-2008-vms). 

## <a name="supported-services"></a>Servicios admitidos
La extensión del Agente de IaaS SQL Server es compatible con las siguientes tareas de administración:

| Característica de administración | DESCRIPCIÓN |
| --- | --- |
| **Automated Backup de SQL** |Automatiza la programación de copias de seguridad de todas las bases de datos para la instancia predeterminada o una instancia con nombre [instalada de forma correcta](virtual-machines-windows-sql-server-iaas-faq.md#administration) de SQL Server en la máquina virtual. Para más información, consulte [Copia de seguridad automatizada para SQL Server en Azure Virtual Machines (Resource Manager)](virtual-machines-windows-sql-automated-backup.md). |
| **Aplicación de revisiones automatizada de SQL** |Configura una ventana de mantenimiento durante la cual tienen lugar actualizaciones importantes de Windows en la máquina virtual, de tal forma que puede evitar las actualizaciones durante las horas punta de la carga de trabajo. Para más información, consulte [Aplicación de revisión automatizada para SQL Server en Azure Virtual Machines (Resource Manager)](virtual-machines-windows-sql-automated-patching.md). |
| **Integración de Azure Key Vault** |Permite instalar y configurar automáticamente Azure Key Vault en la máquina virtual SQL Server. Para más información, consulte [Configuración de la integración de Azure Key Vault para SQL Server en máquinas virtuales de Azure (Resource Manager)](virtual-machines-windows-ps-sql-keyvault.md). |

Una vez instalada y en ejecución, la extensión Agente de IaaS de SQL Server permite que estas características de administración estén disponibles en el panel de SQL Server de la máquina virtual de Azure Portal y a través de Azure PowerShell, tanto para imágenes de Marketplace de SQL Server como para instalaciones manuales de la extensión. 

## <a name="prerequisites"></a>Requisitos previos
Requisitos para usar la extensión del Agente de IaaS SQL Server en la máquina virtual:

**Sistema operativo**:

* Windows Server 2008 R2
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016
* Windows Server 2019 

**Versiones de SQL Server**:

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

Puede ver el modo actual del agente de IaaS de SQL mediante PowerShell: 

  ```powershell-interactive
     //Get the SqlVirtualMachine
     $sqlvm = Get-AzResource -Name $vm.Name  -ResourceGroupName $vm.ResourceGroupName  -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines
     $sqlvm.Properties.sqlManagement
  ```

En el caso de las VM con SQL Server que tengan instalada la extensión IaaS *NoAgent* o *Lightweight*, puede actualizar el modo a *Full* mediante Azure Portal. No se puede cambiar a una versión anterior. Para hacerlo, deberá desinstalar por completo la extensión IaaS de SQL e instalarla de nuevo. 

Para actualizar el modo del agente a *Full*, haga lo siguiente: 

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com).
1. Vaya al recurso [Máquinas virtuales SQL](virtual-machines-windows-sql-manage-portal.md#access-sql-virtual-machine-resource). 
1. Seleccione la máquina virtual con SQL Server y seleccione **Introducción**. 
1. En el caso de las máquinas virtuales de SQL con los modos de IaaS *NoAgent* o *Lightweight*, seleccione el mensaje **Only license type and edition updates are available with the SQL IaaS extension** (Solo las actualizaciones de la edición y el tipo de licencia están disponibles con la extensión IaaS de SQL).

    ![Cambio de modo de inicio desde el portal](media/virtual-machines-windows-sql-server-agent-extension/change-sql-iaas-mode-portal.png)

1. Acepte **reiniciar el servicio SQL Server**; para ello, seleccione la casilla y después seleccione **Confirmar** para actualizar el modo de IaaS a "Full". 

    ![Habilitación de la administración completa para la extensión IaaS](media/virtual-machines-windows-sql-server-agent-extension/enable-full-mode-iaas.png)

##  <a name="installation"></a>Instalación
La extensión IaaS de SQL se instala al registrar la VM con SQL Server con el [proveedor de recursos de VM con SQL](virtual-machines-windows-sql-register-with-resource-provider.md#register-with-sql-vm-resource-provider). En cambio, si es necesario, el agente de IaaS de SQL también se puede instalar manualmente mediante la instalación en modo *Full* o *Lightweight*. 

La extensión del Agente de IaaS de SQL Server con el modo *Full* se instala automáticamente al aprovisionar una de las imágenes de la galería de máquina virtual de SQL Server mediante Azure Portal. 

### <a name="full-mode-installation"></a>Instalación en modo Full
La extensión IaaS de SQL con el modo *Full* ofrece una capacidad de administración completa para una única instancia en la VM con SQL Server. Si hay una instancia predeterminada, la extensión funcionará con la instancia predeterminada y no admitirá la administración de otras instancias. Si no hay ninguna instancia predeterminada, sino solo una instancia con nombre, administrará la instancia con nombre. Si no hay ninguna instancia predeterminada y hay varias instancias con nombre, no se podrá instalar la extensión. 

Al instalar el modo *Full* de IaaS de SQL, se reiniciará el servicio SQL Server. Para que no se reinicie el servicio SQL Server, instale el modo *Lightweight* con capacidad de administración limitada en su lugar. 

Instale el agente de IaaS de SQL con el modo *Full* mediante PowerShell:

  ```powershell-interactive
     // Get the existing  Compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
     // Register SQL VM with 'Full' SQL IaaS agent
     New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
        -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
        -Properties @{virtualMachineResourceId=$vm.Id;sqlServerLicenseType='AHUB';sqlManagement='Full'}  
  
  ```

| Parámetro | Valores aceptables                        |
| :------------------| :-------------------------------|
| **sqlServerLicenseType** | `'AHUB'` o `'PAYG'`     |
| &nbsp;             | &nbsp;                          |


> [!WARNING]
> - Si la extensión aún no está instalada, al instalar la extensión **Full**, se reiniciará el servicio SQL Server. Use el modo **Lightweight** para no tener que reiniciar el servicio SQL Server. 
> - Si actualiza la extensión IaaS de SQL, no se reinicia el servicio SQL Server. 

#### <a name="install-on-a-vm-with-a-single-named-sql-server-instance"></a>Instalación en una máquina virtual con una única instancia de SQL Server con nombre
La extensión IaaS de SQL funcionará con una instancia con nombre en un servidor de SQL Server si la instancia predeterminada se ha desinstalado y se vuelve a instalar la extensión IaaS.

Para usar una instancia con nombre de SQL Server, realice lo siguiente:
   1. Implemente una máquina virtual SQL Server desde Marketplace. 
   1. Desinstale la extensión IaaS desde [Azure Portal](https://portal.azure.com).
   1. Desinstale SQL Server por completo de la máquina virtual SQL Server.
   1. Instale SQL Server con una instancia con nombre en la máquina virtual SQL Server. 
   1. Instale la extensión IaaS desde Azure Portal.  


### <a name="install-in-lightweight-mode"></a>Instalación en modo Lightweight
El modo Lightweight no reiniciará el servicio SQL Server, pero ofrece una funcionalidad limitada. 

Instale el agente de IaaS de SQL con el modo *Lightweight* mediante PowerShell:


  ```powershell-interactive
     // Get the existing  Compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
     // Register SQL VM with 'Lightweight' SQL IaaS agent
     New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
        -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
        -Properties @{virtualMachineResourceId=$vm.Id;sqlServerLicenseType='AHUB';sqlManagement='LightWeight'}  
  
  ```

| Parámetro | Valores aceptables                        |
| :------------------| :-------------------------------|
| **sqlServerLicenseType** | `'AHUB'` o `'PAYG'`     |
| &nbsp;             | &nbsp;                          |


## <a name="get-status-of-sql-iaas-extension"></a>Obtención del estado de la extensión IaaS de SQL
Una manera de comprobar que la extensión está instalada consiste en ver el estado del agente en Azure Portal. Seleccione **Todas las configuraciones** en la ventana de la máquina virtual y haga clic en **Extensiones**. Debería aparecer la extensión **SqlIaasExtension**.

![Extensión del Agente de IaaS de SQL Server en Azure Portal](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-portal.png)

También puede usar el cmdlet de Azure PowerShell **Get-AzVMSqlServerExtension**.

   ```powershell-interactive
   Get-AzVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
   ```

El comando anterior confirma que el agente está instalado y proporciona información de estado general. También puede obtener información de estado específica sobre Automated Backup y Automated Backup con los siguientes comandos.

   ```powershell-interactive
    $sqlext = Get-AzVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings
   ```

## <a name="removal"></a>Eliminación
En Azure Portal, puede desinstalar la extensión si hace clic en los puntos suspensivos de la ventana **Extensiones** de las propiedades de la máquina virtual. Después, haga clic en **Eliminar**.

![Desinstalación de la extensión Agente de IaaS de SQL Server en Azure Portal](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-uninstall.png)

También puede usar el cmdlet de PowerShell **Remove-AzVMSqlServerExtension**.

   ```powershell-interactive
    Remove-AzVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SqlIaasExtension"
   ```

## <a name="next-steps"></a>Pasos siguientes
Empiece utilizando uno de los servicios admitidos por la extensión. Para obtener más información, consulte los artículos a los que se hace referencia en la sección [Servicios admitidos](#supported-services) de este artículo.

Para obtener más información sobre cómo ejecutar SQL Server en Azure Virtual Machines, consulte [Información general sobre SQL Server en Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).

