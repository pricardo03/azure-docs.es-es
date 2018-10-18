---
title: Validación de una copia de seguridad de Azure Stack mediante el ASDK | Microsoft Docs
description: Cómo validar una copia de seguridad de sistemas integrados de Azure Stack mediante el ASDK.
services: azure-stack
author: jeffgilb
manager: femila
cloud: azure-stack
ms.service: azure-stack
ms.topic: article
ms.date: 09/05/2018
ms.author: jeffgilb
ms.reviewer: hectorl
ms.openlocfilehash: 6fa3ba36dca45d5b99c6b5f2ba24367bcd077024
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/06/2018
ms.locfileid: "44030084"
---
# <a name="use-the-asdk-to-validate-an-azure-stack-backup"></a>Uso del ASDK para validar una copia de seguridad de Azure Stack
Después de implementar Azure Stack y aprovisionar recursos de usuario, como ofertas, planes, cuotas y suscripciones, debe [habilitar la infraestructura de copia de seguridad de Azure Stack](..\azure-stack-backup-enable-backup-console.md). La programación y ejecución de copias de seguridad de infraestructura regulares asegurará que los datos de administración de infraestructura no se pierdan si se produce un error de servicio o de hardware muy grave.

> [!TIP]
> Se recomienda [ejecutar una copia de seguridad bajo petición](..\azure-stack-backup-back-up-azure-stack.md) antes de comenzar este procedimiento para asegurarse de que dispone de una copia de los datos de infraestructura más recientes. Asegúrese de que captura el identificador de copia de seguridad una vez finalizada correctamente la copia de seguridad. Este identificador será necesario durante la recuperación en la nube. 

Las copias de seguridad de infraestructura de Azure Stack contienen datos importantes sobre la nube que se pueden restaurar durante la reimplementación de Azure Stack. El ASDK se puede usar para validar estas copias de seguridad sin afectar a la nube de producción. 

La validación de las copias de seguridad en ASDK se admite para los escenarios siguientes:

|Escenario|Propósito|
|-----|-----|
|Validación de las copias de seguridad de infraestructura desde una solución integrada.|Validación de duración breve de que los datos de la copia de seguridad son válidos.|
|Obtención de información sobre el flujo de trabajo de recuperación completo.|Uso del ASDK para validar la experiencia completa de copia de seguridad y restauración.|
|     |     |

El escenario siguiente **no** se admite al validar las copias de seguridad en el ASDK:

|Escenario|Propósito|
|-----|-----|
|Compilación del ASDK para crear la copia de seguridad y la restauración.|Restauración de los datos de copia de seguridad desde una versión anterior del ASDK en una versión más reciente.|
|     |     |


## <a name="cloud-recovery-deployment"></a>Implementación de la recuperación en la nube
Las copias de seguridad de infraestructura de la implementación de sistemas integrados se pueden validar mediante la realización de una implementación de recuperación en la nube del ASDK. En este tipo de implementación, los datos de servicio específicos se restauran a partir de la copia de seguridad después de instalar el ASDK en el equipo host.



### <a name="cloud-recovery-prerequisites"></a>Requisitos previos de recuperación en la nube
Antes de iniciar una implementación de recuperación en la nube del ASDK, asegúrese de que tiene la información siguiente:

|Requisito previo|DESCRIPCIÓN|
|-----|-----|
|Ruta de acceso del recurso compartido de copia de seguridad.|La ruta de acceso del recurso compartido de archivo UNC de la copia de seguridad de Azure Stack más reciente que se va a usar para recuperar información de la infraestructura de Azure Stack. Este recurso compartido local se creará durante el proceso de implementación de recuperación en la nube.|
|Clave de cifrado de copia de seguridad.|La clave de cifrado que se usó para programar la ejecución de la copia de seguridad de infraestructura mediante el portal de administración de Azure Stack.|
|Id. de la copia de seguridad que se va a restaurar.|El identificador de copia de seguridad, en formato alfanumérico "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx", que identifica la copia de seguridad que se va a restaurar durante la recuperación en la nube.|
|Dirección IP del servidor horario.|Se necesita una dirección IP de servidor horario válida, como 132.163.97.2, para la implementación de Azure Stack.|
|Contraseña del certificado externo.|La contraseña para el certificado externo que se usa en Azure Stack. La copia de seguridad de la entidad de certificación contiene los certificados externos que se deben restaurar con esta contraseña.|
|     |     | 

## <a name="prepare-the-host-computer"></a>Preparación del equipo host 
Como en una implementación normal del ASDK, se debe preparar el entorno del sistema de host del ASDK para la instalación. Cuando el equipo host del kit de desarrollo esté preparado, se iniciará desde el disco duro de la máquina virtual CloudBuilder.vhdx para comenzar la implementación del Kit de desarrollo de Azure Stack.

En el equipo host del ASDK, descargue un archivo cloudbuilder.vhdx nuevo correspondiente a la misma versión de Azure Stack de la que se realizó la copia de seguridad y siga las instrucciones para [preparar el equipo host del ASDK](asdk-prepare-host.md).

Una vez reiniciado el servidor host desde el archivo cloudbuilder.vhdx, se debe crear un recurso compartido de archivos y copiar los datos de copia de seguridad en él. El recurso compartido de archivos debe ser accesible para la cuenta que ejecuta el programa de instalación, que es la de Administrador en estos comandos de PowerShell de ejemplo: 

```powershell
$shares = New-Item -Path "c:\" -Name "Shares" -ItemType "directory"
$azsbackupshare = New-Item -Path $shares.FullName -Name "AzSBackups" -ItemType "directory"
New-SmbShare -Path $azsbackupshare.FullName -FullAccess ($env:computername + "\Administrator")  -Name "AzSBackups"
```

Después, copie los archivos de copia de seguridad de Azure Stack más recientes en el recurso compartido recién creado. La estructura de carpetas del recurso compartido debe ser: `\\<ComputerName>\AzSBackups\MASBackup\<BackupID>\`.

## <a name="deploy-the-asdk-in-cloud-recovery-mode"></a>Implementación del ASDK en modo de recuperación en la nube
Para iniciar la recuperación en la nube se usa el script **InstallAzureStackPOC.ps1**. 

> [!IMPORTANT]
> La instalación de ASDK admite exactamente una tarjeta de interfaz de red (NIC) para la conexión con redes. Si tiene varias NIC, asegúrese de que solo una está habilitada (y todas las demás deshabilitadas) antes de ejecutar el script de implementación.

Modifique los comandos de PowerShell siguientes para el entorno y ejecútelos para implementar el ASDK en modo de recuperación en la nube:

```powershell
cd C:\CloudDeployment\Setup     
$adminPass = Get-Credential Administrator
$key = ConvertTo-SecureString "<Your backup encryption key>" -AsPlainText -Force ` 
$certPass = Read-Host -AsSecureString  

.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -BackupStorePath ("\\" + $env:COMPUTERNAME + "\AzSBackups") `
-BackupEncryptionKeyBase64 $key -BackupStoreCredential $adminPass -BackupId "<Backup ID to restore>" `
-TimeServer "<Valid time server IP>" -ExternalCertPassword $certPass
```

## <a name="restore-infrastructure-data-from-backup"></a>Restauración de los datos de la infraestructura de copia de seguridad
Después de una implementación de recuperación en la nube correcta, tendrá que completar la restauración con el cmdlet **Restore-AzureStack**. 

Después de iniciar sesión como operador de Azure Stack, [instale PowerShell de Azure Stack](asdk-post-deploy.md#install-azure-stack-powershell) y, después, sustituya el identificador de copia de seguridad por el parámetro `Name` y ejecute el comando siguiente:

```powershell
Restore-AzsBackup -Name "<BackupID>"
```
Espere 60 minutos después de llamar a este cmdlet para iniciar la comprobación de los datos de copia de seguridad en el ASDK recuperado en la nube.

## <a name="next-steps"></a>Pasos siguientes
[Registrar Azure Stack](asdk-register.md)

