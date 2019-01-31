---
title: Habilitación de la copia de seguridad para Azure Stack con PowerShell | Microsoft Docs
description: Habilite el servicio Infrastructure Backup con Windows PowerShell para que Azure Stack se pueda restaurar si se produce un error.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2018
ms.author: jeffgilb
ms.reviewer: hectorl
ms.lastreviewed: 08/16/2018
ms.openlocfilehash: 10d7303c4323305e177cf006b9a259a817dc695e
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55247483"
---
# <a name="enable-backup-for-azure-stack-with-powershell"></a>Habilitar la copia de seguridad de Azure Stack con PowerShell

*Se aplica a: Sistemas integrados de Azure Stack y Kit de desarrollo de Azure Stack*

Habilite el servicio Copia de seguridad de infraestructura con Windows PowerShell para realizar copias de seguridad periódicas de:
 - Servicio de identidad interno y certificados raíz.
 - Planes de usuario, ofertas, suscripciones
 - Secretos de Key Vault
 - Directivas y roles de RBAC de usuario

Puede tener acceso a los cmdlets de PowerShell para habilitar la copia de seguridad, iniciar la copia de seguridad y obtener información de la copia de seguridad a través del punto de conexión de administración del operador.

## <a name="prepare-powershell-environment"></a>Preparación del entorno de PowerShell

Para obtener instrucciones acerca de cómo configurar el entorno de PowerShell, consulte [Instalación de PowerShell para Azure Stack](azure-stack-powershell-install.md). Para iniciar sesión en Azure Stack, consulte [Configuración del entorno de operador e inicio de sesión en Azure Stack](azure-stack-powershell-configure-admin.md).

## <a name="provide-the-backup-share-credentials-and-encryption-key-to-enable-backup"></a>Provisión del recurso compartido de copia de seguridad, las credenciales y la clave de cifrado para habilitar la copia de seguridad

En la misma sesión de PowerShell, modifique el siguiente script de PowerShell agregando las variables del entorno. Ejecute el script actualizado para proporcionar la clave de cifrado, las credenciales y la clave de cifrado al servicio Infrastructure Backup.

| Variable        | DESCRIPCIÓN   |
|---              |---                                        |
| $username       | Escriba el **nombre de usuario** con el dominio y el nombre de usuario para la ubicación de la unidad compartida con acceso suficiente para leer y escribir archivos. Por ejemplo, `Contoso\backupshareuser`. |
| $password       | Escriba la **Contraseña** del usuario. |
| $sharepath      | Escriba la ruta de acceso a la **ubicación de almacenamiento de la copia de seguridad**. Debe utilizar una cadena de convención de nomenclatura universal (UNC) para la ruta de acceso de un recurso compartido de archivos hospedado en un dispositivo independiente. Una cadena UNC especifica la ubicación de recursos como archivos compartidos o dispositivos. Para garantizar la disponibilidad de los datos de copia de seguridad, el dispositivo debe estar en una ubicación independiente. |
| $frequencyInHours | La frecuencia en horas determina con qué frecuencia se crean las copias de seguridad. El valor predeterminado es 12. Scheduler admite un máximo de 12 y un mínimo de 4.|
| $retentionPeriodInDays | El período de retención en días determina cuántos días de copias de seguridad se conservan en la ubicación externa. El valor predeterminado es 7. Scheduler admite un máximo de 14 y un mínimo de 2. Las copias de seguridad anteriores al período de retención se eliminan automáticamente de la ubicación externa.|
|     |     |

   ```powershell
    # Example username:
    $username = "domain\backupadmin"
    # Example share path:
    $sharepath = "\\serverIP\AzSBackupStore\contoso.com\seattle"
   
    $password = Read-Host -Prompt ("Password for: " + $username) -AsSecureString
    
    # The encryption key is generated using the New-AzsEncryptionKeyBase64 cmdlet provided in Azure Stack PowerShell.
    # Make sure to store your encryption key in a secure location after it is generated.
    $Encryptionkey = New-AzsEncryptionKeyBase64
    $key = ConvertTo-SecureString -String ($Encryptionkey) -AsPlainText -Force

    Set-AzsBackupShare -BackupShare $sharepath -Username $username -Password $password -EncryptionKey $key
   ```
   
##  <a name="confirm-backup-settings"></a>Confirmación de la configuración de copia de seguridad

En la misma sesión de PowerShell, ejecute los comandos siguientes:

   ```powershell
    Get-AzsBackupLocation | Select-Object -Property Path, UserName
   ```

El resultado debe tener una apariencia similar a la del ejemplo siguiente:

   ```powershell
    Path                        : \\serverIP\AzsBackupStore\contoso.com\seattle
    UserName                    : domain\backupadmin
   ```

## <a name="update-backup-settings"></a>Actualización de la configuración de copia de seguridad
En la misma sesión de PowerShell, puede actualizar los valores predeterminados para el período de retención y la frecuencia de las copias de seguridad. 

   ```powershell
    #Set the backup frequency and retention period values.
    $frequencyInHours = 10
    $retentionPeriodInDays = 5

    Set-AzsBackupShare -BackupFrequencyInHours $frequencyInHours -BackupRetentionPeriodInDays $retentionPeriodInDays
    Get-AzsBackupLocation | Select-Object -Property Path, UserName, AvailableCapacity, BackupFrequencyInHours, BackupRetentionPeriodInDays
   ```

El resultado debe tener una apariencia similar a la del ejemplo siguiente:

   ```powershell
    Path                        : \\serverIP\AzsBackupStore\contoso.com\seattle
    UserName                    : domain\backupadmin
    AvailableCapacity           : 60 GB
    BackupFrequencyInHours      : 10
    BackupRetentionPeriodInDays : 5
   ```

## <a name="next-steps"></a>Pasos siguientes

 - Aprenda a ejecutar una copia de seguridad: consulte [Copia de seguridad de Azure Stack](azure-stack-backup-back-up-azure-stack.md ).  
 - Aprenda a comprobar que la copia de seguridad se ejecutó: consulte [Confirm backup completed in administration portal](azure-stack-backup-back-up-azure-stack.md ) (Confirmación de copia de seguridad completada en el portal de administración).
