---
title: 'Desconexión de un disco de datos de una máquina virtual Windows: Azure | Microsoft Docs'
description: Desconecte un disco de datos de una máquina virtual de Azure creada mediante el modelo de implementación de Resource Manager.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-service-management
ms.assetid: 13180343-ac49-4a3a-85d8-0ead95e2028c
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/17/2018
ms.author: cynthn
ms.subservice: disks
ms.openlocfilehash: ca9a4478249e935afb6a52520c77d9df159fe9e7
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2019
ms.locfileid: "67718730"
---
# <a name="how-to-detach-a-data-disk-from-a-windows-virtual-machine"></a>Desacoplamiento de un disco de datos de una máquina virtual de Windows

Cuando ya no necesite un disco de datos que se encuentra conectado a una máquina virtual, puede desconectarlo fácilmente. Esto quita el disco de la máquina virtual, pero no lo quita del almacenamiento.

> [!WARNING]
> Si se desconecta un disco, no se elimina automáticamente. Si se ha suscrito a Almacenamiento premium, seguirá acumulando cargos de almacenamiento por el disco. Para más información, vea [Precios y facturación al utilizar Premium Storage](disks-types.md#billing).

Si desea volver a usar los datos existentes en el disco, puede acoplarlo de nuevo a la misma máquina virtual (o a otra).

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="detach-a-data-disk-using-powershell"></a>Desconexión de un disco de datos con PowerShell

También puede quitar un disco de datos *en caliente* con PowerShell, pero asegúrese de que no haya nada que use activamente el disco antes de desconectarlo de la máquina virtual.

En este ejemplo, se quita el disco denominado **myDisk** de la máquina virtual **myVM** del grupo de recursos **myResourceGroup**. Quite primero el disco con el cmdlet [Remove-AzVMDataDisk](https://docs.microsoft.com/powershell/module/az.compute/remove-azvmdatadisk). Luego, actualice el estado de la máquina virtual con el cmdlet [Update-AzVM](https://docs.microsoft.com/powershell/module/az.compute/update-azvm) para completar el proceso de eliminación del disco de datos.

```azurepowershell-interactive
$VirtualMachine = Get-AzVM -ResourceGroupName "myResourceGroup" -Name "myVM"
Remove-AzVMDataDisk -VM $VirtualMachine -Name "myDisk"
Update-AzVM -ResourceGroupName "myResourceGroup" -VM $VirtualMachine
```

El disco permanece en el almacenamiento pero ya no está acoplado a una máquina virtual.

## <a name="detach-a-data-disk-using-the-portal"></a>Desconexión de un disco de datos mediante el portal

1. En el menú de la izquierda, seleccione **Máquinas virtuales**.
2. Seleccione la máquina virtual que tiene el disco de datos que quiere desconectar y haga clic en **Detener** para desasignar la máquina virtual.
3. En el panel de la máquina virtual, seleccione **Discos**.
4. En la parte superior del panel **Discos**, seleccione **Editar**.
5. En el panel **Discos**, en el extremo derecho del disco de datos que quiere desasociar, haga clic en el ![imagen del botón Desasociar](./media/detach-disk/detach.png) botón de desasociación.
5. Una vez que haya quitado el disco, haga clic en **Guardar**, en la parte superior del panel.
6. En el panel de la máquina virtual, haga clic en **Información general** y, luego, en el botón **Iniciar** de la parte superior del panel para reiniciar la máquina virtual.

El disco permanece en el almacenamiento pero ya no está acoplado a una máquina virtual.

## <a name="next-steps"></a>Pasos siguientes

Si desea reutilizar el disco de datos, basta con que lo [conecte a otra máquina virtual](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)