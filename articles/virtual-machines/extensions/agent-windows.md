---
title: Información general del agente de máquina virtual de Azure | Microsoft Docs
description: Información general del agente de máquina virtual de Azure
services: virtual-machines-windows
documentationcenter: virtual-machines
author: zroiy
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 0a1f212e-053e-4a39-9910-8d622959f594
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/30/2018
ms.author: roiyz
ms.openlocfilehash: 9185b786a3eea402b8013eb71a5d7268787900c5
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2018
ms.locfileid: "39414505"
---
# <a name="azure-virtual-machine-agent-overview"></a>Información general del agente de máquina virtual de Azure
El agente de máquina virtual de Microsoft Azure (agente VM) es un proceso ligero y seguro que administra la interacción de máquinas virtuales (VM) con el controlador de tejido de Azure. El agente de VM tiene un rol principal que consiste en habilitar y ejecutar extensiones de máquina virtual de Azure. Las extensiones de máquina virtual habilitan la configuración posterior a la implementación de máquinas virtuales, como la instalación y la configuración de software. Las extensiones de máquina virtual también habilitan características de recuperación, como el restablecimiento de la contraseña administrativa de una máquina virtual. Sin el agente de máquina virtual de Azure, no se pueden ejecutar extensiones de máquina virtual.

En este artículo se describe la instalación, la detección y la eliminación del agente de máquina virtual de Azure.

## <a name="install-the-vm-agent"></a>Instalar el agente de VM

### <a name="azure-marketplace-image"></a>Imagen de Azure Marketplace

El agente de máquina virtual de Azure se instala de forma predeterminada en cualquier máquina virtual de Windows implementada a partir de una imagen de Azure Marketplace. Al implementar una imagen de Azure Marketplace desde el portal, PowerShell, la interfaz de la línea de comandos o una plantilla de Azure Resource Manager, también se instalará el agente de máquina virtual de Azure.

El paquete de Windows Guest Agent se divide en dos partes:

- Agente de aprovisionamiento (PA)
- Windows Guest Agent (WinGA)

Para arrancar una máquina virtual debe tener el agente de aprovisionamiento instalado en la máquina virtual, pero no es necesario instalar WinGA. En tiempo de implementación de la máquina virtual, puede seleccionar no instalar WinGA. En el ejemplo siguiente se muestra cómo seleccionar la opción *provisionVmAgent* con una plantilla de Azure Resource Manager:

```json
"resources": [{
"name": "[parameters('virtualMachineName')]",
"type": "Microsoft.Compute/virtualMachines",
"apiVersion": "2016-04-30-preview",
"location": "[parameters('location')]",
"dependsOn": ["[concat('Microsoft.Network/networkInterfaces/', parameters('networkInterfaceName'))]"],
"properties": {
    "osProfile": {
    "computerName": "[parameters('virtualMachineName')]",
    "adminUsername": "[parameters('adminUsername')]",
    "adminPassword": "[parameters('adminPassword')]",
    "windowsConfiguration": {
        "provisionVmAgent": "false"
}
```

Si no tiene instalados los agentes, no puede usar algunos servicios de Azure, como Azure Backup o Azure Security. Estos servicios requieren una extensión para instalarse. Si ha implementado una máquina virtual sin WinGA, puede instalar más tarde la versión más reciente del agente.

### <a name="manual-installation"></a>Instalación manual
El agente de máquina virtual de Windows puede instalarse manualmente con un paquete de Windows Installer. Es posible que sea necesaria la instalación manual cuando se crea una imagen de máquina virtual personalizada que se implementa en Azure. Para instalar manualmente el agente de máquina virtual de Windows, [descargue el instalador del agente de máquina virtual](http://go.microsoft.com/fwlink/?LinkID=394789).

Para instalar el agente de máquina virtual, haga doble clic en el archivo de Windows Installer. Para una instalación automatizada o desatendida del agente de máquina virtual, ejecute el comando siguiente:

```cmd
msiexec.exe /i WindowsAzureVmAgent.2.7.1198.778.rd_art_stable.160617-1120.fre /quiet
```

## <a name="detect-the-vm-agent"></a>Detección del agente de VM

### <a name="powershell"></a>PowerShell

El módulo de PowerShell de Azure Resource Manager puede usarse para recuperar información sobre máquinas virtuales de Azure. Para ver información sobre una máquina virtual, como el estado de aprovisionamiento para el agente de máquina virtual de Azure, use [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm):

```powershell` Get-AzureRmVM
```

The following condensed example output shows the *ProvisionVMAgent* property nested inside *OSProfile*. This property can be used to determine if the VM agent has been deployed to the VM:

```PowerShell
OSProfile                  :
  ComputerName             : myVM
  AdminUsername            : myUserName
  WindowsConfiguration     :
    ProvisionVMAgent       : True
    EnableAutomaticUpdates : True
```

El siguiente script se puede usar para devolver una lista concisa de nombres de máquinas virtuales y el estado del agente de máquina virtual:

```PowerShell
$vms = Get-AzureRmVM

foreach ($vm in $vms) {
    $agent = $vm | Select -ExpandProperty OSProfile | Select -ExpandProperty Windowsconfiguration | Select ProvisionVMAgent
    Write-Host $vm.Name $agent.ProvisionVMAgent
}
```

### <a name="manual-detection"></a>Detección manual
Cuando inicia sesión en una máquina virtual de Microsoft Azure, el Administrador de tareas se puede usar para examinar los procesos en ejecución. Para comprobar el agente de máquina virtual de Azure, abra el Administrador de tareas, haga clic en la pestaña *Detalles* y busque el nombre de proceso **WindowsAzureGuestAgent.exe**. La presencia de este proceso indica que el agente de VM está instalado.


## <a name="upgrade-the-vm-agent"></a>Actualización del agente de VM
El agente de VM de Azure para Windows se actualiza automáticamente. A medida que se implementan nuevas máquinas virtuales en Azure, reciben el agente de máquina virtual más reciente en tiempo de aprovisionamiento de máquina virtual. Las imágenes de máquina virtual personalizadas se deben actualizar manualmente para que incluyan el nuevo agente de máquina virtual en tiempo de creación de la imagen.


## <a name="next-steps"></a>Pasos siguientes
Para más información sobre las extensiones de máquina virtual, consulte [Características y extensiones de las máquinas virtuales de Azure](overview.md).