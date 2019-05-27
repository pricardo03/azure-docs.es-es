---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/05/2019
ms.author: alkohli
ms.openlocfilehash: b657ee32e76dd90671f7e91337ced01b925889a1
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66161137"
---
Si experimenta los problemas del dispositivo, puede crear un paquete de soporte técnico de los registros del sistema. Microsoft Support usa este paquete para solucionar los problemas. Siga estos pasos para crear un paquete de soporte:

1. [Conectarse a la interfaz de PowerShell del dispositivo](#connect-to-the-powershell-interface).
2. Use el `Get-HcsNodeSupportPackage` comando para crear un paquete de soporte técnico. El uso del cmdlet es como sigue:

    ```powershell
    Get-HcsNodeSupportPackage [-Path] <string> [-Zip] [-ZipFileName <string>] [-Include {None | RegistryKeys | EtwLogs
            | PeriodicEtwLogs | LogFiles | DumpLog | Platform | FullDumps | MiniDumps | ClusterManagementLog | ClusterLog |
            UpdateLogs | CbsLogs | StorageCmdlets | ClusterCmdlets | ConfigurationCmdlets | KernelDump | RollbackLogs |
            Symbols | NetworkCmdlets | NetworkCmds | Fltmc | ClusterStorageLogs | UTElement | UTFlag | SmbWmiProvider |
            TimeCmds | LocalUILogs | ClusterHealthLogs | BcdeditCommand | BitLockerCommand | DirStats | ComputeRolesLogs |
            ComputeCmdlets | DeviceGuard | Manifests | MeasuredBootLogs | Stats | PeriodicStatLogs | MigrationLogs |
            RollbackSupportPackage | ArchivedLogs | Default}] [-MinimumTimestamp <datetime>] [-MaximumTimestamp <datetime>]
            [-IncludeArchived] [-IncludePeriodicStats] [-Credential <pscredential>]  [<CommonParameters>]
    ```

    El cmdlet recopila registros de su dispositivo y copia esos registros en una red especificada o un recurso compartido local.

    Los parámetros utilizados son como sigue:

    - `-Path` -Especifique la red o la ruta de acceso local para copiar el paquete de soporte. (obligatorio)
    - `-Credential` -Especifique las credenciales para tener acceso a la ruta de acceso protegido.
    - `-Zip` -Especifique para generar un archivo zip.
    - `-Include` -Especificar para incluir los componentes que se incluirán en el paquete de soporte técnico. Si no se especifica, `Default` se da por hecho.
    - `-IncludeArchived` -Especificar para incluir los registros archivados en el paquete de soporte técnico.
    - `-IncludePeriodicStats` -Especifique esta opción para incluir registros stat periódicos en el paquete de soporte técnico.

    
