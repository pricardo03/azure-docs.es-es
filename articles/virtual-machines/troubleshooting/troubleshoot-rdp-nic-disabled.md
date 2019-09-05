---
title: No se puede establecer conexión de forma remota con Azure Virtual Machines porque el adaptador de red está deshabilitado | Microsoft Docs
description: Obtenga información sobre cómo solucionar un problema en el que se produce un error con RDP porque la NIC está deshabilitada en VM de Azure | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: cshepard
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/12/2018
ms.author: genli
ms.openlocfilehash: bb4583c88f2e867c619a2163d91bff1d48b6a7a3
ms.sourcegitcommit: 2aefdf92db8950ff02c94d8b0535bf4096021b11
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/03/2019
ms.locfileid: "70230710"
---
#  <a name="cannot-remote-desktop-to-a-vm-because-the-network-interface-is-disabled"></a>No se puede establecer el escritorio remoto en una VM porque la interfaz de red está deshabilitada

En este artículo se muestra cómo resolver un problema en el que no se puede establecer la conexión del escritorio remoto en Azure Windows Virtual Machines (VM) si la interfaz de red está deshabilitada.

> [!NOTE]
> Azure tiene dos modelos de implementación diferentes para crear recursos y trabajar con ellos: [Resource Manager y el clásico](../../azure-resource-manager/resource-manager-deployment-model.md). En este artículo se explica el uso del modelo de implementación de Resource Manager, que es el que se recomienda usar para las nuevas implementaciones, en lugar del modelo de implementación clásica.

## <a name="symptoms"></a>Síntomas

No puede establecer ninguna conexión RDP ni ningún otro tipo de conexión con ningún otro puerto en una VM en Azure porque la interfaz de red de la VM está deshabilitada.

## <a name="solution"></a>Solución

Antes de seguir estos pasos, tome una instantánea del disco del sistema operativo de la máquina virtual afectada como copia de seguridad. Para obtener más información, consulte [Instantánea de un disco](../windows/snapshot-copy-managed-disk.md).

Para habilitar la interfaz para la VM, use el control de serie o [restablezca la interfaz de red](##reset-network-interface) para la VM.

### <a name="use-serial-control"></a>Uso del control serie

1. Conéctese a una [consola serie y abra la instancia de CMD](./serial-console-windows.md#use-cmd-or-powershell-in-serial-console
). Si la consola serie no está habilitada en la VM, consulte el artículo sobre cómo [restablecer la interfaz de red](#reset-network-interface).
2. Compruebe el estado de la interfaz de red:

        netsh interface show interface

    Anótese el nombre de la interfaz de red deshabilitada.

3. Habilite la interfaz de red:

        netsh interface set interface name="interface Name" admin=enabled

    Por ejemplo, si la interfaz de red se llama "Ethernet 2", ejecute el siguiente comando:

        netsh interface set interface name="Ethernet 2" admin=enabled

4.  Vuelva a comprobar el estado de la interfaz de red para asegurarse de que está habilitada.

        netsh interface show interface

    No tiene que reiniciar la VM en este momento. La máquina virtual volverá a ser accesible.

5.  Conéctese a la máquina virtual y compruebe si el problema se resuelve.

## <a name="reset-network-interface"></a>Restablecimiento de la interfaz de red

Para restablecer la interfaz de red, cambie la dirección IP a otra dirección IP que esté disponible en la subred. Para ello, use Azure Portal o Azure PowerShell. Para obtener más información, consulte el artículo sobre cómo [restablecer la interfaz de red](reset-network-interface.md).
