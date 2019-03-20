---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/05/2019
ms.author: alkohli
ms.openlocfilehash: 880b630ae48eda086f6454f0d7108d27d3403b77
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57556587"
---
Si se arrancan en un entorno sin DHCP, siga estos pasos para implementar la máquina virtual para la puerta de enlace de datos de cuadro.

1. [Conectarse a la interfaz de Windows PowerShell del dispositivo](#connect-to-the-powershell-interface).
2. Use el `Get-HcsIpAddress` cmdlet para enumerar las interfaces de red habilitadas en el dispositivo virtual. Si el dispositivo tiene una única interfaz de red habilitada, el nombre predeterminado asignado a esta interfaz es `Ethernet`.

    El ejemplo siguiente muestra el uso de este cmdlet:

    ```
    [10.100.10.10]: PS>Get-HcsIpAddress

    OperationalStatus : Up
    Name              : Ethernet
    UseDhcp           : True
    IpAddress         : 10.100.10.10
    Gateway           : 10.100.10.1
    ```

3. Utilice el cmdlet `Set-HcsIpAddress` para configurar la red. Consulte el ejemplo siguiente:

    ```
    Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1
    ```

