---
title: Creación de una VM a partir de una imagen administrada en Azure | Microsoft Docs
description: Cree una máquina virtual con Windows a partir de una imagen administrada generalizada con Azure PowerShell o Azure Portal en el modelo de implementación de Resource Manager.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/17/2018
ms.author: cynthn
ms.openlocfilehash: 9157765afaa610d207a47e19b73f80ae3898fd68
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/09/2019
ms.locfileid: "55977565"
---
# <a name="create-a-vm-from-a-managed-image"></a>Creación de una máquina virtual a partir de una imagen administrada

Puede crear varias máquinas virtuales (VM) a partir de una imagen de VM administrada con Azure mediante PowerShell o Azure Portal. Una imagen de VM administrada contiene la información necesaria para crear una VM, incluidos los discos del SO y de datos. Los discos duros virtuales (VHD) que componen la imagen, incluidos los discos del sistema operativo y los discos de datos, se almacenan como discos administrados. 

Antes de crear una máquina virtual, deberá [crear una imagen de máquina virtual administrada](capture-image-resource.md) para usarla como imagen de origen. 


## <a name="use-the-portal"></a>Uso del portal

1. Abra [Azure Portal](https://portal.azure.com).
2. En el menú izquierdo, seleccione **Todos los recursos**. Puede ordenar los recursos por **Tipo** para localizar fácilmente sus imágenes.
3. Seleccione la imagen que desea usar en la lista. Se abre la página **Información general** de la imagen.
4. Seleccione **Crear VM** en el menú.
5. Escriba la información de la máquina virtual. El nombre de usuario y la contraseña que especifique aquí, se usarán para iniciar sesión en la máquina virtual. Cuando haya terminado, seleccione **Aceptar**. Puede crear la máquina virtual en un grupo de recursos existente, o bien hacer clic en **Crear nuevo** para crear un grupo de recursos y así almacenar la máquina virtual.
6. Seleccione un tamaño para la máquina virtual. Para ver más tamaños, seleccione **Ver todo** o cambie el filtro **Supported disk type** (Tipo de disco admitido). 
7. En **Configuración**, realice cambios según sea necesario y haga clic en **Aceptar**. 
8. En la página de resumen, verá el nombre de la imagen que se ha establecido como **Imagen privada**. Haga clic en **Aceptar** para iniciar la implementación de la máquina virtual.


## <a name="use-powershell"></a>Uso de PowerShell

Puede usar PowerShell para crear una VM a partir de una imagen mediante el parámetro simplificado establecido para el cmdlet [New-AzVm](https://docs.microsoft.com/powershell/module/az.compute/new-azvm). La imagen debe estar en el mismo grupo de recursos donde quiere crear la máquina virtual.

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

El parámetro simplificado que se estableció para [New-AzVm](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) solo requiere que proporcione un nombre, un grupo de recursos y un nombre de imagen para crear una VM a partir de una imagen. El cmdlet New-AzVm usará el valor del parámetro **-Name** como nombre de todos los recursos que cree automáticamente. En este ejemplo, se proporcionan nombres más detallados para cada uno de los recursos, pero se permite que el cmdlet los cree automáticamente. También puede crear recursos de antemano, como la red virtual, y pasar el nombre del recurso al cmdlet. New-AzVm usará los recursos existentes si los encuentra por su nombre.

En el ejemplo siguiente, se crea una máquina virtual denominada *myVMfromImage* en el grupo de recursos *myResourceGroup*, a partir de la imagen llamada *myImage*. 


```azurepowershell-interactive
New-AzVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVMfromImage" `
    -ImageName "myImage" `
    -Location "East US" `
    -VirtualNetworkName "myImageVnet" `
    -SubnetName "myImageSubnet" `
    -SecurityGroupName "myImageNSG" `
    -PublicIpAddressName "myImagePIP" `
    -OpenPorts 3389
```



## <a name="next-steps"></a>Pasos siguientes
[Crear y administrar máquinas virtuales Windows con el módulo de Azure PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

