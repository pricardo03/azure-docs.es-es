---
title: Comandos comunes de PowerShell para Azure Virtual Machines | Microsoft Docs
description: Comandos comunes de PowerShell para comenzar a crear y administrar máquinas virtuales Windows en Azure.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ba3839a2-f3d5-4e19-a5de-95bfb1c0e61e
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 06/01/2018
ms.author: cynthn
ms.openlocfilehash: cb9f03ab87079ba33135840e3e7599d2e8cc95e9
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2018
ms.locfileid: "42146922"
---
# <a name="common-powershell-commands-for-creating-and-managing-azure-virtual-machines"></a>Comandos comunes de PowerShell para crear y administrar Azure Virtual Machines

En este artículo se tratan algunos de los comandos de Azure PowerShell que puede usar para crear y administrar máquinas virtuales de su suscripción de Azure.  Para más ayuda con opciones y modificadores concretos de la línea de comandos, puede utilizar el **comando** *Get-Help*.

Consulte [Cómo instalar y configurar Azure PowerShell](/powershell/azure/overview) para obtener más información sobre cómo instalar la versión más reciente de Azure PowerShell, seleccionar la suscripción que quiere usar e iniciar sesión en su cuenta.

Estas variables podrían serle útiles si ejecuta más de uno de los comandos de este artículo:

- $location: la ubicación de la máquina virtual. Puede usar [Get-AzureRmLocation](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermlocation) para buscar una [región geográfica](https://azure.microsoft.com/regions/) que se adapte a sus necesidades.
- Valor predeterminado personalizado: Nombre del grupo de recursos que contiene las máquinas virtuales.
- $myVM: el nombre de la máquina virtual.

## <a name="create-a-vm---simplified"></a>Creación de una máquina virtual: método simplificado

| Task | Get-Help |
| ---- | ------- |
| Creación de una máquina virtual simple | [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) -Name $myVM <BR></BR><BR></BR> New-AzureRmVM tiene un conjunto de parámetros *simplificados* donde todo lo que se requiere es un nombre. El valor de -Name se usará como nombre de todos los recursos necesarios para crear una máquina virtual. Se pueden especificar más, pero esto es todo lo necesario.|
| Crear una imagen personalizada a partir de una máquina virtual | New-AzureRmVm -ResourceGroupName $myResourceGroup -Name $myVM ImageName "myImage" -Location $location  <BR></BR><BR></BR>Debe haber creado su propia [imagen administrada](capture-image-resource.md). Puede usar una sola imagen para crear varias máquinas virtuales idénticas. |



## <a name="create-a-vm-configuration"></a>Creación de una configuración de máquina virtual

| Task | Get-Help |
| ---- | ------- |
| Creación de una configuración de máquina virtual |$vm = [New-AzureRmVMConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvmconfig) -VMName $myVM -VMSize "Standard_D1_v1"<BR></BR><BR></BR>La configuración de máquina virtual se utiliza para definir o actualizar la configuración de la máquina virtual. La configuración se inicializa con el nombre de la VM y su [tamaño](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). |
| Adición de valores de configuración |$vm = [Set-AzureRmVMOperatingSystem](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) -VM $vm -Windows -ComputerName $myVM -Credential $cred -ProvisionVMAgent -EnableAutoUpdate<BR></BR><BR></BR>La configuración del sistema operativo que incluye [credenciales](https://technet.microsoft.com/library/hh849815.aspx) se agrega al objeto de configuración que creó anteriormente mediante New-AzureRmVMConfig. |
| Adición de una interfaz de red |$vm = [Add-AzureRmVMNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.compute/Add-AzureRmVMNetworkInterface) -VM $vm -Id $nic.Id<BR></BR><BR></BR>Una VM debe tener un [interfaz de red](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) para comunicarse en una red virtual. También puede usar [Get AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) para recuperar un objeto de interfaz de red existente. |
| Especificación de una imagen de plataforma |$vm = [Set-AzureRmVMSourceImage](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmsourceimage) -VM $vm -PublisherName "nombre_publicador" -Offer "oferta_publicador" -Skus "sku_producto" -Version "más_reciente"<BR></BR><BR></BR>La [información de la imagen](cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) se agrega al objeto de configuración que creó anteriormente mediante New-AzureRmVMConfig. El objeto que se devuelve con este comando solo se utiliza cuando se establece el disco del sistema operativo que utilizará una imagen de plataforma. |
| Crear una VM |[New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) -ResourceGroupName $myResourceGroup -Location $location -VM $vm<BR></BR><BR></BR>Todos los recursos se crean en un [grupo de recursos](../../azure-resource-manager/powershell-azure-resource-manager.md). Antes de utilizar este comando, ejecute New-AzureRmVMConfig, Set-AzureRmVMOperatingSystem, Set-AzureRmVMSourceImage, Add-AzureRmVMNetworkInterface y Set-AzureRmVMOSDisk. |
| Actualización de una máquina virtual |[Update-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvm) -ResourceGroupName $myResourceGroup -VM $vm<BR></BR><BR></BR>Obtenga la configuración de máquina virtual actual mediante Get-AzureRmVM, cambie los valores de configuración del objeto de máquina virtual y, luego, ejecute este comando. |

## <a name="get-information-about-vms"></a>Obtención de información sobre VM

| Task | Get-Help |
| ---- | ------- |
| Enumeración de las máquinas virtuales de una suscripción |[Get-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvm) |
| Enumeración de las máquinas virtuales de un grupo de recursos |Get-AzureRmVM -ResourceGroupName $myResourceGroup<BR></BR><BR></BR>Para obtener una lista de grupos de recursos de la suscripción, utilice [Get-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermresourcegroup). |
| Obtención información acerca de una máquina virtual |Get-AzureRmVM -ResourceGroupName $myResourceGroup -Name $myVM |

## <a name="manage-vms"></a>Administración de máquinas virtuales
| Task | Get-Help |
| --- | --- |
| Inicio de una máquina virtual |[Start-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/start-azurermvm) -ResourceGroupName $myResourceGroup -Name $myVM |
| Detención de una máquina virtual |[Stop-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/stop-azurermvm) -ResourceGroupName $myResourceGroup -Name $myVM |
| Reinicio de una máquina virtual en ejecución |[Restart-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/restart-azurermvm) -ResourceGroupName $myResourceGroup -Name $myVM |
| Eliminación de una máquina virtual |[Remove-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/remove-azurermvm) -ResourceGroupName $myResourceGroup -Name $myVM |


## <a name="next-steps"></a>Pasos siguientes
* Consulte los pasos básicos para crear una máquina virtual en [Creación de una máquina virtual de Windows con Resource Manager y PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

