---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/05/2019
ms.author: alkohli
ms.openlocfilehash: aebb82690a7a49aba071ed64349d37d516208cca
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186776"
---
Para restablecer el dispositivo, debe borrar de forma segura todos los datos del disco de datos y del disco de arranque del dispositivo. 

Utilice el cmdlet `Reset-HcsAppliance` para borrar tanto los discos de datos como el disco de arranque o solo los discos de datos. Los identificadores `ClearData` y `BootDisk` le permiten borrar los discos de datos y el disco de arranque, respectivamente.

Si utiliza el restablecimiento del dispositivo en la interfaz de usuario web local, solo los discos de datos se borran de forma segura, pero el disco de arranque se mantiene intacto. El disco de arranque contiene la configuración del dispositivo.

1. [Conéctese a la interfaz de PowerShell](#connect-to-the-powershell-interface).
2. En el símbolo del sistema, escriba:

    `Reset-HcsAppliance -ClearData -BootDisk`

    En el ejemplo siguiente se muestra cómo utilizar este cmdlet:
    ```
    [10.128.24.33]: PS>Reset-HcsAppliance -ClearData -BootDisk
    
    Confirm
    Are you sure you want to perform this action?
    Performing the operation "Reset-HcsAppliance" on target "ShouldProcess appliance".
    [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [?] Help (default is "Y"): N
    ```
