---
title: Copia de seguridad de Azure Stack | Microsoft Docs
description: Realice una copia de seguridad a petición en Azure Stack con la copia de seguridad vigente.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 9565DDFB-2CDB-40CD-8964-697DA2FFF70A
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2018
ms.author: jeffgilb
ms.reviewer: hectorl
ms.openlocfilehash: 578bb864f56b788db77d1201533e73d3b9616669
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/10/2018
ms.locfileid: "41948133"
---
# <a name="back-up-azure-stack"></a>Copia de seguridad de Azure Stack

*Se aplica a: sistemas integrados de Azure Stack y Kit de desarrollo de Azure Stack*

Realice una copia de seguridad a petición en Azure Stack con la copia de seguridad vigente. Para obtener instrucciones acerca de cómo configurar el entorno de PowerShell, consulte [Instalación de PowerShell para Azure Stack](azure-stack-powershell-install.md). Para iniciar sesión en Azure Stack, consulte [Uso del portal de administración de Azure Stack](azure-stack-manage-portals.md).

## <a name="start-azure-stack-backup"></a>Inicio de la copia de seguridad de Azure Stack

### <a name="start-a-new-backup-without-job-progress-tracking"></a>Inicio de una nueva copia de seguridad sin seguimiento del progreso de trabajo
Use Start-AzSBackup para iniciar una nueva copia de seguridad inmediatamente sin seguir el progreso de trabajo.

```powershell
   Start-AzsBackup -Force
```

### <a name="start-azure-stack-backup-with-job-progress-tracking"></a>Inicio de una copia de seguridad de Azure Stack con seguimiento del progreso de trabajo
Use Start-AzSBackup para iniciar una nueva copia de seguridad con la variable -AsJob para realizar un seguimiento del progreso del trabajo de copia de seguridad.

```powershell
    $backupjob = Start-AzsBackup -Force -AsJob 
    "Start time: " + $backupjob.PSBeginTime;While($backupjob.State -eq "Running"){("Job is currently: " `
    + $backupjob.State+" ;Duration: " + (New-TimeSpan -Start ($backupjob.PSBeginTime) `
    -End (Get-Date)).Minutes);Start-Sleep -Seconds 30};$backupjob.Output

    if($backupjob.State -eq "Completed"){Get-AzsBackup | where {$_.BackupId -eq $backupjob.Output.BackupId}}
```

## <a name="confirm-backup-has-completed"></a>Confirmación de que la copia de seguridad se ha completado

### <a name="confirm-backup-has-completed-using-powershell"></a>Confirmación de que la copia de seguridad se ha completado con PowerShell
Use los siguientes comandos de PowerShell para asegurarse de que la copia de seguridad se ha completado correctamente:

```powershell
   Get-AzsBackup
```

El resultado debe tener un aspecto similar a la siguiente salida:

```powershell
    BackupDataVersion : 1.0.1
    BackupId          : <backup ID>
    RoleStatus        : {NRP, SRP, CRP, KeyVaultInternalControlPlane...}
    Status            : Succeeded
    CreatedDateTime   : 7/6/2018 6:46:24 AM
    TimeTakenToCreate : PT20M32.364138S
    DeploymentID      : <deployment ID>
    StampVersion      : 1.1807.0.41
    OemVersion        : 
    Id                : /subscriptions/<subscription ID>/resourceGroups/System.local/providers/Microsoft.Backup.Admin/backupLocations/local/backups/<backup ID>
    Name              : local/<local name>
    Type              : Microsoft.Backup.Admin/backupLocations/backups
    Location          : local
    Tags              : {}
```

### <a name="confirm-backup-has-completed-in-the-administration-portal"></a>Confirmación de que la copia de seguridad se ha completado en el portal de administración
Use el portal de administración de Azure Stack para comprobar que esa copia de seguridad se ha completado correctamente; para ello, siga estos pasos:

1. Abra [el portal de administración de Azure Stack](azure-stack-manage-portals.md).
2. Seleccione **Más servicios** > **Infrastructure backup** (Copia de seguridad de infraestructura). Elija **Configuración** en la hoja **Copia de seguridad de infraestructura**.
3. Busque el **Nombre** y la **Fecha de finalización** de la copia de seguridad en la lista **Copias de seguridad disponibles**.
4. Compruebe que el **Estado** es **Correcto**.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre el flujo de trabajo para recuperarse de un evento de pérdida de datos. Consulte [Recover from catastrophic data loss](azure-stack-backup-recover-data.md) (Recuperación después de una pérdida de datos grave).
