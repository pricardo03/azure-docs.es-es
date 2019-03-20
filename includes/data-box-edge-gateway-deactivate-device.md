---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/05/2019
ms.author: alkohli
ms.openlocfilehash: aebb82690a7a49aba071ed64349d37d516208cca
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57556767"
---
Para restablecer el dispositivo, deberá borrar de forma segura todos los datos en el disco de datos y el disco de arranque del dispositivo. 

Use el `Reset-HcsAppliance` cmdlet borrar de los discos de datos y el disco de arranque o solo los discos de datos. El `ClearData` y `BootDisk` conmutadores le permiten borrar los discos de datos y el disco de arranque, respectivamente.

Si usa el dispositivo de restablecimiento de la interfaz de usuario web local, solo los discos de datos se borran de forma segura, pero el disco de arranque se mantiene intacto. El disco de arranque contiene la configuración del dispositivo.

1. [Conectarse a la interfaz de PowerShell](#connect-to-the-powershell-interface).
2. En el símbolo del sistema, escriba:

    `Reset-HcsAppliance -ClearData -BootDisk`

    El ejemplo siguiente muestra cómo usar este cmdlet:
    ```
    [10.128.24.33]: PS>Reset-HcsAppliance -ClearData -BootDisk
    
    Confirm
    Are you sure you want to perform this action?
    Performing the operation "Reset-HcsAppliance" on target "ShouldProcess appliance".
    [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [?] Help (default is "Y"): N
    ```
