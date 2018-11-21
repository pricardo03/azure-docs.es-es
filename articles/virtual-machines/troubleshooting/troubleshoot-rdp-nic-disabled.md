---
title: No se puede establecer el escritorio remoto en Azure Virtual Machines porque la NIC está deshabilitada | Microsoft Docs
description: Obtenga información sobre cómo solucionar un problema en el que se produce un error con RDP porque la NIC está deshabilitada en VM de Azure | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: cshepard
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/12/2018
ms.author: genli
ms.openlocfilehash: 6b14530bd6b4c1b6617cb1d5c88d710a32e5372c
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "51634839"
---
#  <a name="cannot-remote-desktop-to-a-vm-because-the-network-interface-is-disabled"></a>No se puede establecer el escritorio remoto en una VM porque la interfaz de red está deshabilitada

En este artículo se muestra cómo resolver un problema en el que no se puede establecer el escritorio remoto en Azure Windows Virtual Machines (VM) porque la interfaz de red está deshabilitada.

> [!NOTE] 
> Azure tiene dos modelos de implementación diferentes para crear y trabajar con recursos: [el Administrador de recursos y el clásico](../../azure-resource-manager/resource-manager-deployment-model.md). En este artículo se explica el uso del modelo de implementación de Resource Manager, que es el que se recomienda usar para las nuevas implementaciones, en lugar del modelo de implementación clásica. 

## <a name="symptoms"></a>Síntomas 

No puede establecer ninguna conexión RDP ni ningún otro tipo de conexión con ningún otro puerto en una VM en Azure porque la interfaz de red de la VM está deshabilitada.

## <a name="solution"></a>Solución 

Antes de seguir estos pasos, tome una instantánea del disco del sistema operativo de la máquina virtual afectada como copia de seguridad. Para obtener más información, consulte [Instantánea de un disco](../windows/snapshot-copy-managed-disk.md).

Para habilitar la interfaz para la VM, use el control de serie o [restablezca la interfaz de red](##reset-network-interface) para la VM.

### <a name="use-serial-control"></a>Uso del control serie

1. Conéctese a una [consola serie y abra la instancia de CMD](./serial-console-windows.md#open-cmd-or-powershell-in-serial-console
). Si la consola serie no está habilitada en la VM, consulte el artículo sobre cómo [restablecer la interfaz de red](#reset-network-interface).
2. Compruebe el estado de la interfaz de red:

        netsh interface show interface

    Anótese el nombre de la interfaz de red deshabilitada. 

3. Habilite la interfaz de red:

        netsh interface set interface name="interface Name" admin=enabled

    Por ejemplo, si la interfaz de red se denomina "Ethernet 2", ejecute el siguiente comando:

        netsh interface set interface name=""Ethernet 2" admin=enabled
    

4.  Vuelva a comprobar el estado de la interfaz de red para asegurarse de que está habilitada.

        netsh interface show interface

    No tiene que reiniciar la VM en este momento. La VM volverá a ser accesible.
        
5.  Conéctese a la VM y compruebe si el problema se resuelve.

## <a name="reset-network-interface"></a>Restablecimiento de la interfaz de red

Para restablecer la interfaz de red, cambie la dirección IP a otra que esté disponible en la subred mediante Azure Portal o Azure PowerShell. Para más información, consulte el artículo sobre cómo [restablecer la interfaz de red](reset-network-interface.md). 