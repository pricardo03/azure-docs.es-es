---
title: Implementación y administración de copias de seguridad de recursos compartidos de archivos de Azure mediante PowerShell
description: Use PowerShell para implementar y administrar copias de seguridad de Azure para recursos compartidos de archivos de Azure.
services: backup
author: pvrk
manager: shivamg
keywords: PowersShell; Copia de seguridad de archivos de Azure; Restauración de archivos de Azure;
ms.service: backup
ms.topic: conceptual
ms.date: 11/12/2018
ms.author: pullabhk
ms.assetid: 80da8ece-2cce-40dd-8dce-79960b6ae073
ms.openlocfilehash: 90623981f67bbed15ade743192525676e58a0a83
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53318431"
---
# <a name="use-powershell-to-back-up-and-restore-azure-file-shares"></a>Uso de PowerShell para la copia de seguridad y restauración de recursos compartidos de archivos de Azure

En este artículo se muestra cómo usar cmdlets de Azure PowerShell para realizar copias de seguridad de un recurso compartido de archivos de Azure y recuperarlas desde un almacén de Recovery Services. Un almacén de Recovery Services es un recurso de Azure Resource Manager y se usa para proteger datos y recursos en los servicios Azure Backup y Azure Site Recovery.

## <a name="concepts"></a>Conceptos

Si no está familiarizado con el servicio Azure Backup, puede obtener información general al respecto en el artículo [¿Qué es Azure Backup?](backup-introduction-to-azure-backup.md) Antes de empezar, asegúrese de que conoce las funcionalidades de la versión preliminar de la copia de seguridad de recursos compartidos de Azure que están documentadas [aquí](backup-azure-files.md).

Para usar PowerShell de forma eficaz, es preciso conocer la jerarquía de los objetos y desde dónde empezar.

![Jerarquía de objetos de Recovery Services](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

Para ver la referencia del cmdlet de PowerShell AzureRm.RecoveryServices.Backup, vea [Azure Backup - Recovery Services Cmdlets](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup) (Azure Backup: cmdlets de Recovery Services) en la biblioteca de Azure.

## <a name="setup-and-registration"></a>Instalación y registro

> [!NOTE]
> Como se indica [aquí](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-6.13.0), la compatibilidad con las nuevas características del módulo de AzureRM termina en noviembre de 2018. Por ello, vamos a proporcionar compatibilidad con la copia de seguridad de recursos compartidos de Azure con el nuevo módulo "Az" PS. También estamos planeando embarcarnos en el lanzamiento en modo de disponibilidad general del módulo Az.

Para empezar:

1. [Descargue la versión más reciente de "Az" PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azurermps-6.13.0) (la versión mínima necesaria es: 0.7.0)

2. Para buscar los cmdlets de PowerShell de Azure Backup disponibles, escriba el siguiente comando:

    ```powershell
    Get-Command *azrecoveryservices*
    ```
    Aparecen los alias y cmdlets de Azure Backup, Azure Site Recovery y el almacén de Recovery Services. La imagen siguiente es un ejemplo de lo que verá. No es la lista completa de los cmdlets.

    ![Lista de Recovery Services](./media/backup-azure-afs-automation/list-of-recoveryservices-ps-az.png)

3. Inicie sesión en su cuenta de Azure mediante el cmdlet **Connect-AzAccount**. El cmdlet abrirá una página web que le solicitará las credenciales de la cuenta:

    * Como alternativa, puede incluir sus credenciales de cuenta como un parámetro en el cmdlet **Connect-AzAccount** mediante el parámetro **-Credential**.
    * Si usted es un asociado CSP que trabaja en nombre de un inquilino, especifique el cliente como inquilino usando su TenantID o su nombre de dominio principal de inquilino. Por ejemplo:  **Connect-AzAccount -Tenant "fabrikam.com"**

4. Ya que una cuenta puede tener varias suscripciones, le recomendamos que asocie la suscripción que quiera usar a esa cuenta:

    ```powershell
    Select-AzureRmSubscription -SubscriptionName $SubscriptionName
    ```

5. Si utiliza Azure Backup por primera vez, debe utilizar el cmdlet **Register-AzResourceProvider** para registrar el proveedor de Azure Recovery Services con su suscripción.

    ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

6. Puede comprobar que los proveedores se registraron correctamente mediante los siguientes comandos:
    ```powershell
    Get-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
    En la salida del comando, **RegistrationState** se debe cambiar a **Registrado**. En caso contrario, vuelva a ejecutar el cmdlet **Register-AzResourceProvider**.

Las siguientes tareas se pueden automatizar con PowerShell:

* Creación de un almacén de Recovery Services
* Configuración de la copia de seguridad de los recursos compartidos de archivos de Azure
* Desencadenamiento de un trabajo de copia de seguridad
* Supervisión de un trabajo de copia de seguridad
* Restauración de un recurso compartido de archivos de Azure
* Restauración de un archivo individual de Azure a partir de un recurso compartido de archivos de Azure

## <a name="create-a-recovery-services-vault"></a>Creación de un almacén de Recovery Services

Los siguientes pasos le guiarán por el proceso de creación de un almacén de Recovery Services.

1. El almacén de Recovery Services es un recurso de Resource Manager, por lo que deberá colocarlo dentro de un grupo de recursos. Puede usar un grupo de recursos existente o crear uno con el cmdlet **New-AzResourceGroup**. Al crear un grupo de recursos, especifique el nombre y la ubicación.  

    ```powershell
    New-AzResourceGroup -Name "test-rg" -Location "West US"
    ```
2. Use el cmdlet **New-AzRecoveryServicesVault** para crear el almacén de Recovery Services. Asegúrese de especificar para el almacén la misma ubicación del grupo de recursos.

    ```powershell
    New-AzRecoveryServicesVault -Name "testvault" -ResourceGroupName "test-rg" -Location "West US"
    ```
3. Especifique el tipo de redundancia de almacenamiento que se usará: [almacenamiento con redundancia local (LRS)](../storage/common/storage-redundancy-lrs.md) o [almacenamiento con redundancia geográfica (GRS)](../storage/common/storage-redundancy-grs.md). En el ejemplo siguiente se muestra que la opción -BackupStorageRedundancy para testvault está establecida en GeoRedundant.

    ```powershell
    $vault1 = Get-AzRecoveryServicesVault -Name "testvault"
    Set-AzRecoveryServicesBackupProperties  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

## <a name="view-the-vaults-in-a-subscription"></a>Visualización de los almacenes de una suscripción

Para ver todos los almacenes de la suscripción, use **Get-AzRecoveryServicesVault**:

```powershell
Get-AzRecoveryServicesVault
```

El resultado es similar al ejemplo siguiente, tenga en cuenta que los valores de ResourceGroupName y Location asociados se proporcionan.

```powershell
Name              : Contoso-vault
ID                : /subscriptions/1234
Type              : Microsoft.RecoveryServices/vaults
Location          : WestUS
ResourceGroupName : Contoso-docs-rg
SubscriptionId    : 1234-567f-8910-abc
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
```

Muchos de los cmdlets de Azure Backup requieren el objeto de almacén de Recovery Services como entrada.

Use el cmdlet **Set-AzRecoveryServicesVaultContext** para establecer el contexto de almacén. Una vez que se haya establecido el contexto de almacén, se aplica a todos los cmdlets posteriores. En el ejemplo siguiente se establece el contexto del almacén *testvault*.

```powershell
Get-AzRecoveryServicesVault -Name "testvault" | Set-AzRecoveryServicesVaultContext
```

> [!NOTE]
> Tenemos previsto dejar de utilizar la configuración del contexto de almacén según las directrices de Azure PowerShell. En su lugar, se recomienda a los usuarios pasar el identificador del almacén como se menciona más abajo

Como alternativa, puede almacenar o recuperar el identificador del almacén en el que desea realizar una operación de PowerShell y pasarlo al comando apropiado.

```powershell
$vaultID = Get-AzRecoveryServicesVault -ResourceGroupName "Contoso-docs-rg" -Name "testvault" | select -ExpandProperty ID
```

## <a name="configure-backup-for-an-azure-file-share"></a>Configuración de la copia de seguridad de un recurso compartido de archivos de Azure

### <a name="create-protection-policy"></a>Creación de una directiva de protección

Una directiva de protección de copia de seguridad está asociada con al menos una directiva de retención. La directiva de retención define el tiempo que se conserva el punto de recuperación antes de que se elimine. Use **Get-AzRecoveryServicesBackupRetentionPolicyObject** para ver la directiva de retención predeterminada.  Del mismo modo, puede usar **Get-AzRecoveryServicesBackupSchedulePolicyObject** para obtener la directiva de programación predeterminada. El cmdlet **New-AzRecoveryServicesBackupProtectionPolicy** crea un objeto de PowerShell que contiene información de la directiva de copia de seguridad. Los objetos de directiva de retención y programación se usan como entradas para el cmdlet **New-AzRecoveryServicesBackupProtectionPolicy**. En el ejemplo siguiente se almacenan la directiva de programación y la directiva de retención en variables. En el ejemplo se usan esas variables para definir los parámetros al crear la directiva de protección *NewPolicy*.

```powershell
$schPol = Get-AzRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureFiles"
$retPol = Get-AzRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureFiles"
New-AzRecoveryServicesBackupProtectionPolicy -Name "NewAFSPolicy" -WorkloadType "AzureFiles" -RetentionPolicy $retPol -SchedulePolicy $schPol
```

La salida es similar a la del ejemplo siguiente:

```powershell
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
NewAFSPolicy           AzureFiles            AzureStorage              10/24/2017 1:30:00 AM
```

### <a name="enable-protection"></a>Habilitar protección

Una vez que haya definido la directiva de protección, puede habilitar la protección para el recurso compartido de archivos de Azure con esta directiva.

En primer lugar, recupere el objeto de directiva con el cmdlet **Get-AzRecoveryServicesBackupProtectionPolicy**. Puede usar este cmdlet para obtener una directiva específica o para ver las directivas asociadas a un tipo de carga de trabajo.

En el ejemplo siguiente se obtienen las directivas para el tipo de carga de trabajo AzureFiles.

```powershell
Get-AzRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureFiles"
```

La salida es similar a la del ejemplo siguiente:

```powershell
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
dailyafs             AzureFiles         AzureStorage         1/10/2018 12:30:00 AM
```

> [!NOTE]
> La zona horaria del campo BackupTime en PowerShell es UTC. Sin embargo, cuando el tiempo de copia de seguridad se muestra en Azure Portal, la hora se ajusta a la zona horaria local.
>
>

La directiva siguiente recupera la directiva de copia de seguridad llamada "dailyafs"

```powershell
$afsPol =  Get-AzRecoveryServicesBackupProtectionPolicy -Name "dailyafs"
```

Use **Enable AzRecoveryServicesBackupProtection** para habilitar la protección del elemento con la directiva especificada. Después de que la directiva se haya asociado con el almacén, el flujo de trabajo de copia de seguridad se desencadena a la hora definida en la programación de la directiva.

El ejemplo siguiente habilita la protección para el recurso compartido de archivos de Azure "testAzureFileShare", en la cuenta de almacenamiento "testStorageAcct", con la directiva "dailyafs".

```powershell
Enable-AzRecoveryServicesBackupProtection -StorageAccountName "testStorageAcct" -Name "testAzureFS" -Policy $afsPol
```

El comando espera hasta que el trabajo de protección configurado termina y proporciona una salida parecida a la siguiente.

```cmd
WorkloadName       Operation            Status                 StartTime                                                                                                         EndTime                   JobID
------------             ---------            ------               ---------                                  -------                   -----
testAzureFS       ConfigureBackup      Completed            11/12/2018 2:15:26 PM     11/12/2018 2:16:11 PM     ec7d4f1d-40bd-46a4-9edb-3193c41f6bf6
```

### <a name="trigger-an-on-demand-backup"></a>Desencadenamiento de una copia de seguridad a petición

Use **Backup-AzRecoveryServicesBackupItem** para desencadenar un trabajo de copia de seguridad para un recurso compartido de archivos protegido de Azure. Recupere la cuenta de almacenamiento y el recurso compartido de archivos que contiene mediante los comandos siguientes y desencadene una copia de seguridad a petición.

```powershell
$afsContainer = Get-AzRecoveryServicesBackupContainer -FriendlyName "testStorageAcct" -ContainerType AzureStorage
$afsBkpItem = Get-AzRecoveryServicesBackupItem -Container $afsContainer -WorkloadType "AzureFiles" -Name "testAzureFS"
$job =  Backup-AzRecoveryServicesBackupItem -Item $afsBkpItem
```

El comando devuelve un trabajo del cual se puede realizar un seguimiento con un identificador como se indica en el ejemplo siguiente.

```powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
testAzureFS       Backup               Completed            11/12/2018 2:42:07 PM     11/12/2018 2:42:11 PM     8bdfe3ab-9bf7-4be6-83d6-37ff1ca13ab6
```

Estamos aprovechando las instantáneas de recursos compartidos de archivos de Azure al realizar copias de seguridad y, por tanto, el trabajo normalmente se completa en el momento en que el comando devuelve esta salida

### <a name="modify-protection-policy"></a>Modificación de una directiva de protección

Si desea cambiar la directiva con la que se protege el recurso compartido de archivos de Azure, use el cmdlet **Enable-AzRecoveryServicesBackupProtection** con el elemento de copia de seguridad adecuado y la nueva directiva de protección.

En el siguiente ejemplo se cambia la directiva de protección de "testAzureFS" de "dailyafs" a "monthlyafs"

```powershell
$monthlyafsPol =  Get-AzRecoveryServicesBackupProtectionPolicy -Name "monthlyafs"
$afsContainer = Get-AzRecoveryServicesBackupContainer -FriendlyName "testStorageAcct" -ContainerType AzureStorage
$afsBkpItem = Get-AzRecoveryServicesBackupItem -Container $afsContainer -WorkloadType AzureFiles -Name "testAzureFS"
Enable-AzRecoveryServicesBackupProtection -Item $afsBkpItem -Policy $monthlyafsPol
```

## <a name="restore-azure-file-sharesazure-files"></a>Restauración de recursos compartidos de archivos de Azure o de archivos de Azure

Puede restaurar un recurso compartido de archivos completo a su ubicación original o a otra ubicación alternativa. De forma similar, también se pueden restaurar archivos individuales a partir del recurso compartido de archivos.

### <a name="fetching-recovery-points"></a>Recuperación de puntos de recuperación

Use el cmdlet **Get-AzRecoveryServicesBackupRecoveryPoint** para enumerar todos los puntos de recuperación del elemento de copia de seguridad. En el siguiente script, la variable **$rp**es una matriz de puntos de recuperación para el elemento de copia de seguridad seleccionado de los últimos siete días. La matriz se ordena en orden inverso de tiempo con el punto de recuperación más reciente en el índice 0. Use la indexación de matrices de PowerShell estándar para seleccionar el punto de recuperación. En el ejemplo, $rp[0] selecciona el último punto de recuperación.

```powershell
$startDate = (Get-Date).AddDays(-7)
$endDate = Get-Date
$rp = Get-AzRecoveryServicesBackupRecoveryPoint -Item $afsBkpItem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()

$rp[0] | fl
```

La salida es similar a la del ejemplo siguiente:

```powershell
FileShareSnapshotUri : https://testStorageAcct.file.core.windows.net/testAzureFS?sharesnapshot=2018-11-20T00:31:04.00000
                       00Z
RecoveryPointType    : FileSystemConsistent
RecoveryPointTime    : 11/20/2018 12:31:05 AM
RecoveryPointId      : 86593702401459
ItemName             : testAzureFS
Id                   : /Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/Micros                      oft.RecoveryServices/vaults/testVault/backupFabrics/Azure/protectionContainers/StorageContainer;storage;teststorageRG;testStorageAcct/protectedItems/AzureFileShare;testAzureFS/recoveryPoints/86593702401462
WorkloadType         : AzureFiles
ContainerName        : storage;teststorageRG;testStorageAcct
ContainerType        : AzureStorage
BackupManagementType : AzureStorage
```

Una vez seleccionado el punto de recuperación adecuado, continúe para restaurar el recurso compartido de archivos o el archivo en una ubicación alternativa (o en la ubicación original) como se indica a continuación.

### <a name="restore-azure-file-shares-to-an-alternate-location"></a>Restauración de recursos compartidos de archivos de Azure en una ubicación alternativa

#### <a name="restoring-an-azure-file-share"></a>Restauración de un recurso compartido de archivos de Azure

Identifique la ubicación alternativa. Para ello, proporcione la siguiente información:

* ***TargetStorageAccountName***: Cuenta de almacenamiento en la que se restaura el contenido de la copia de seguridad. La cuenta de almacenamiento de destino debe estar en la misma ubicación que el almacén.
* ***TargetFileShareName***: Recursos compartidos de archivos de la cuenta de almacenamiento en la que se restaura el contenido de la copia de seguridad
* ***TargetFolder***: La carpeta del recurso compartido de archivos en la que se restauran los datos. Si el contenido de la copia de seguridad debe restaurarse en la carpeta raíz, asigne los valores de la carpeta de destino como una cadena vacía
* ***ResolveConflict***: Instrucción en caso de conflicto con los datos restaurados. Acepta "Overwrite" u "skip"

Proporcione estos parámetros al comando restore para restaurar una copia de seguridad de un recurso compartido de archivos en una ubicación alternativa.

````powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -TargetStorageAccountName "TargetStorageAcct" -TargetFileShareName "DestAFS" -TargetFolder "testAzureFS_restored" -ResolveConflict Overwrite
````

El comando devuelve un trabajo con un identificador del cual se puede realizar un seguimiento como se indica en el ejemplo siguiente.

````powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
testAzureFS        Restore              InProgress           12/10/2018 9:56:38 AM                               9fd34525-6c46-496e-980a-3740ccb2ad75
````

#### <a name="restoring-an-azure-file"></a>Restauración de un archivo de Azure

En caso de que desee restaurar un archivo individual en lugar de todo un recurso compartido de archivos, el archivo individual se debe identificar de manera exclusiva proporcionando los parámetros siguientes.

* ***TargetStorageAccountName***: Cuenta de almacenamiento en la que se restaura el contenido de la copia de seguridad. La cuenta de almacenamiento de destino debe estar en la misma ubicación que el almacén.
* ***TargetFileShareName***: Recursos compartidos de archivos de la cuenta de almacenamiento en la que se restaura el contenido de la copia de seguridad
* ***TargetFolder***: La carpeta del recurso compartido de archivos en la que se restauran los datos. Si el contenido de la copia de seguridad debe restaurarse en la carpeta raíz, asigne los valores de la carpeta de destino como una cadena vacía
* ***SourceFilePath***: La ruta de acceso absoluta del archivo que se va a restaurar en el recurso compartido de archivos, como una cadena. Se trata de la misma ruta de acceso que se usa en el cmdlet ```Get-AzStorageFile``` de PS.
* ***SourceFileType***: Indica si se ha seleccionado un directorio o un archivo. Admite "Directory" o "File"
* ***ResolveConflict***: Instrucción en caso de conflicto con los datos restaurados. Acepta "Overwrite" u "skip"

Como puede ver, los parámetros adicionales solo están relacionados con el archivo individual que se va a restaurar.

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -TargetStorageAccountName "TargetStorageAcct" -TargetFileShareName "DestAFS" -TargetFolder "testAzureFS_restored" -SourceFileType File -SourceFilePath "TestDir/TestDoc.docx" -ResolveConflict Overwrite
```

Este también devolverá un trabajo del que se puede realizar un seguimiento mediante un identificador como se indicó anteriormente.

### <a name="restore-azure-file-shares-to-original-location"></a>Restauración de recursos compartidos de archivos de Azure en la ubicación original

En caso de restauración en la ubicación original, no es necesario especificar todos los parámetros de destino correspondientes. Solo se debe proporcionar ```ResolveConflict```

#### <a name="overwrite-an-azure-file-share"></a>Sobrescribir un recurso compartido de archivos de Azure

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -ResolveConflict Overwrite
```

#### <a name="overwrite-an-azure-file"></a>Sobrescribir un archivo de Azure

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -SourceFileType File -SourceFilePath "TestDir/TestDoc.docx" -ResolveConflict Overwrite
```

## <a name="track-backup-and-restore-jobs"></a>Seguimiento de trabajos de copia de seguridad y restauración

Las operaciones de copia de seguridad y restauración a petición devuelven un trabajo con un identificador como se ha indicado [anteriormente](#trigger-an-on-demand-backup). Use el cmdlet ```Get-AzRecoveryServicesBackupJobDetails``` para hacer un seguimiento del progreso del trabajo y obtener más detalles.

```powershell
$job = Get-AzRecoveryServicesBackupJob -JobId 00000000-6c46-496e-980a-3740ccb2ad75 -VaultId $vaultID

 $job | fl


IsCancellable        : False
IsRetriable          : False
ErrorDetails         : {Microsoft.Azure.Commands.RecoveryServices.Backup.Cmdlets.Models.AzureFileShareJobErrorInfo}
ActivityId           : 00000000-5b71-4d73-9465-8a4a91f13a36
JobId                : 00000000-6c46-496e-980a-3740ccb2ad75
Operation            : Restore
Status               : Failed
WorkloadName         : testAFS
StartTime            : 12/10/2018 9:56:38 AM
EndTime              : 12/10/2018 11:03:03 AM
Duration             : 01:06:24.4660027
BackupManagementType : AzureStorage

$job.ErrorDetails

 ErrorCode ErrorMessage                                          Recommendations
 --------- ------------                                          ---------------
1073871825 Microsoft Azure Backup encountered an internal error. Wait for a few minutes and then try the operation again. If the issue persists, please contact Microsoft support
```
