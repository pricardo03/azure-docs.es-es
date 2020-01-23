---
title: Copia de seguridad y restauración de archivos de Azure Files con PowerShell
description: En este artículo se aprende a realizar copias de seguridad y a restaurar archivos de Azure Files mediante el servicio Azure Backup y PowerShell.
ms.topic: conceptual
ms.date: 08/20/2019
ms.openlocfilehash: f9665bbc3562faab760562e1e6729d8be0796acd
ms.sourcegitcommit: 7221918fbe5385ceccf39dff9dd5a3817a0bd807
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/21/2020
ms.locfileid: "76294055"
---
# <a name="back-up-and-restore-azure-files-with-powershell"></a>Copia de seguridad y restauración de archivos de Azure Files con PowerShell

En este artículo se describe cómo usar Azure PowerShell para realizar una copia de seguridad de un recurso compartido de archivos de Azure Files y recuperarlo mediante el almacén de Recovery Services de [Azure Backup](backup-overview.md).

En este artículo se explica lo siguiente:

> [!div class="checklist"]
>
> * Configurar PowerShell y registrar el proveedor de Azure Recovery Services.
> * Cree un almacén de Recovery Services.
> * Configurar la copia de seguridad de un recurso compartido de archivos de Azure.
> * Ejecutar un trabajo de copia de seguridad.
> * Restaurar un recurso compartido de archivos del que se ha realizado una copia de seguridad, o bien un archivo individual de un recurso compartido.
> * Supervisar los trabajos de copia de seguridad y restauración.

## <a name="before-you-start"></a>Antes de comenzar

* [Más información](backup-azure-recovery-services-vault-overview.md) sobre los almacenes de Recovery Services.
* Descubra las funcionalidades de vista previa para realizar una [copia de seguridad de recursos compartidos de archivos de Azure](backup-afs.md).
* Revise la jerarquía de objetos de PowerShell para Recovery Services.

## <a name="recovery-services-object-hierarchy"></a>Jerarquía de objetos de Recovery Services

En el diagrama siguiente, se resume la jerarquía de objetos.

![Jerarquía de objetos de Recovery Services](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

Revise la [referencia de cmdlet](/powershell/module/az.recoveryservices) de **Az.RecoveryServices** en la biblioteca de Azure.

## <a name="set-up-and-install"></a>Configuración e instalación

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Configure PowerShell como sigue:

1. [Descargue la versión más reciente de Azure PowerShell](/powershell/azure/install-az-ps). La versión 1.0.0 es la versión mínima requerida.

2. Busque los cmdlets de PowerShell de Azure Backup con este comando:

    ```powershell
    Get-Command *azrecoveryservices*
    ```

3. Revise los alias y cmdlets de Azure Backup, Azure Site Recovery y el almacén de Recovery Services. Este es un ejemplo de lo que puede ver. No es la lista completa de los cmdlets.

    ![Lista de cmdlets de Recovery Services](./media/backup-azure-afs-automation/list-of-recoveryservices-ps-az.png)

4. Inicie sesión en su cuenta de Azure con el cmdlet **Connect-AzAccount**.
5. En la página web que aparece, se le pedirá que escriba las credenciales de su cuenta.

    * Como alternativa, puede incluir las credenciales de su cuenta como un parámetro en el cmdlet **Connect-AzAccount** mediante el parámetro **-Credential**.
    * Si usted es un asociado CSP que trabaja en nombre de un inquilino, especifique el cliente como inquilino mediante el uso del TenantID o del nombre de dominio principal de inquilino. Un ejemplo es **Connect-AzAccount -Tenant** fabrikam.com.

6. Asocie la suscripción que quiere usar con la cuenta, dado que una cuenta puede tener varias suscripciones.

    ```powershell
    Select-AzSubscription -SubscriptionName $SubscriptionName
    ```

7. Si utiliza Azure Backup por primera vez, use el cmdlet **Register-AzResourceProvider** para registrar el proveedor de Azure Recovery Services con su suscripción.

    ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

8. Compruebe que los proveedores se han registrado correctamente:

    ```powershell
    Get-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

9. En la salida del comando, compruebe que **RegistrationState** cambia a **Registered**. En caso contrario, vuelva a ejecutar el cmdlet **Register-AzResourceProvider**.

## <a name="create-a-recovery-services-vault"></a>Creación de un almacén de Recovery Services

El almacén de Recovery Services es un recurso de Resource Manager, por lo que deberá colocarlo dentro de un grupo de recursos. Puede usar un grupo de recursos existente o crear uno con el cmdlet **New-AzResourceGroup**. Al crear un grupo de recursos, especifique el nombre y la ubicación.

Siga estos pasos para crear un almacén de Recovery Services.

1. Un almacén se coloca en un grupo de recursos. Si no tiene un grupo de recursos, cree uno con [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup?view=azps-1.4.0). En el ejemplo, se crea un grupo de recursos en la región Oeste de EE. UU.

   ```powershell
   New-AzResourceGroup -Name "test-rg" -Location "West US"
   ```

2. Use el cmdlet [New-AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/New-AzRecoveryServicesVault?view=azps-1.4.0) para crear el almacén. Especifique para el almacén la misma ubicación del grupo de recursos.

    ```powershell
    New-AzRecoveryServicesVault -Name "testvault" -ResourceGroupName "test-rg" -Location "West US"
    ```

3. Especifique el tipo de redundancia para el almacenamiento.

   * Puede usar [almacenamiento con redundancia local](../storage/common/storage-redundancy-lrs.md) o [almacenamiento con redundancia geográfica](../storage/common/storage-redundancy-grs.md).
   * En el siguiente ejemplo se establece la opción **-BackupStorageRedundancy** para el cmdlet [Set-AzRecoveryServicesBackupProperties](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupproperty) para **testvault** establecida en **GeoRedundant**.

     ```powershell
     $vault1 = Get-AzRecoveryServicesVault -Name "testvault"
     Set-AzRecoveryServicesBackupProperties  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
     ```

### <a name="view-the-vaults-in-a-subscription"></a>Visualización de los almacenes de una suscripción

Para ver todos los almacenes de la suscripción, use [Get-AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesvault?view=azps-1.4.0).

```powershell
Get-AzRecoveryServicesVault
```

La salida será similar a la siguiente. Observe que se proporcionan el grupo de recursos y la ubicación asociados.

```powershell
Name              : Contoso-vault
ID                : /subscriptions/1234
Type              : Microsoft.RecoveryServices/vaults
Location          : WestUS
ResourceGroupName : Contoso-docs-rg
SubscriptionId    : 1234-567f-8910-abc
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
```

### <a name="set-the-vault-context"></a>Establecer el contexto de almacén

Almacene el objeto de almacén en una variable y establezca el contexto de almacén.

* Muchos de los cmdlets de Azure Backup requieren el objeto de almacén de Recovery Services como una entrada, por lo que es conveniente almacenar el objeto de almacén en una variable.
* El contexto de almacén es el tipo de los datos protegidos en el almacén. Establézcalo con [Set-AzRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesvaultcontext?view=azps-1.4.0). Una vez que se haya establecido el contexto, se aplica a todos los cmdlets posteriores.

En el ejemplo siguiente se establece el contexto del almacén de **testvault**.

```powershell
Get-AzRecoveryServicesVault -Name "testvault" | Set-AzRecoveryServicesVaultContext
```

### <a name="fetch-the-vault-id"></a>Recuperación del identificador del almacén

Tenemos previsto dejar de usar la configuración del contexto de almacén según las directrices de Azure PowerShell. En su lugar, puede almacenar o recuperar el identificador del almacén y pasarlo a los comandos pertinentes. Por tanto, si no ha establecido el contexto de almacén o quiere especificar el comando que se va a ejecutar para un determinado almacén, pase el identificador de almacén como "-vaultID" a todos los comandos pertinentes de la manera siguiente:

```powershell
$vaultID = Get-AzRecoveryServicesVault -ResourceGroupName "Contoso-docs-rg" -Name "testvault" | select -ExpandProperty ID
New-AzRecoveryServicesBackupProtectionPolicy -Name "NewAFSPolicy" -WorkloadType "AzureFiles" -RetentionPolicy $retPol -SchedulePolicy $schPol -VaultID $vaultID
```

## <a name="configure-a-backup-policy"></a>Configuración de una directiva de copia de seguridad

Una directiva de copia de seguridad especifica la programación para las copias de seguridad y cuánto tiempo se deben mantener los puntos de recuperación de copia de seguridad:

* Una directiva de copia de seguridad está asociada con al menos una directiva de retención. Una directiva de retención define el tiempo que se conserva un punto de recuperación antes de que se elimine.
* Vea la retención de la directiva de copia de seguridad predeterminada con [Get-AzRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupretentionpolicyobject?view=azps-1.4.0).
* Vea la programación de la directiva de copia de seguridad predeterminada con [Get-AzRecoveryServicesBackupSchedulePolicyObject](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupschedulepolicyobject?view=azps-1.4.0).
* El cmdlet [New AzRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupprotectionpolicy?view=azps-1.4.0) se usa para crear una directiva de copia de seguridad. Introduzca los objetos de directiva de retención y programación.

De forma predeterminada, se define una hora de inicio en el objeto de directiva de programación. Use el siguiente ejemplo para cambiar la hora de inicio a la hora de inicio deseada. La hora de inicio deseada debe estar también en formato UTC. En el ejemplo siguiente se supone que la hora de inicio deseada es la 1:00 UTC para las copias de seguridad diarias.

```powershell
$schPol = Get-AzRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureFiles"
$UtcTime = Get-Date -Date "2019-03-20 01:30:00Z"
$UtcTime = $UtcTime.ToUniversalTime()
$schpol.ScheduleRunTimes[0] = $UtcTime
```

> [!IMPORTANT]
> Solo tiene que proporcionar la hora de inicio en múltiplos de 30 minutos. En el ejemplo anterior, solo puede ser "01:00:00" o "02:30:00". La hora de inicio no puede ser "01:15:00".

En el ejemplo siguiente se almacenan la directiva de programación y la directiva de retención en variables. Después usa estas variables como parámetros para una nueva directiva (**NewAFSPolicy**). **NewAFSPolicy** realiza una copia de seguridad diaria y la conserva durante 30 días.

```powershell
$schPol = Get-AzRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureFiles"
$retPol = Get-AzRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureFiles"
New-AzRecoveryServicesBackupProtectionPolicy -Name "NewAFSPolicy" -WorkloadType "AzureFiles" -RetentionPolicy $retPol -SchedulePolicy $schPol
```

La salida será similar a la siguiente.

```powershell
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
NewAFSPolicy           AzureFiles            AzureStorage              10/24/2019 1:30:00 AM
```

## <a name="enable-backup"></a>Habilitación de la copia de seguridad

Una vez que haya definido la directiva de copia de seguridad, puede habilitar la protección para el recurso compartido de archivos de Azure con esta directiva.

### <a name="retrieve-a-backup-policy"></a>Recuperación de una directiva de copia de seguridad

Recupere el objeto de directiva pertinente con [Get-AzRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupprotectionpolicy?view=azps-1.4.0). Use este cmdlet para obtener una directiva específica o para ver las directivas asociadas a un tipo de carga de trabajo.

#### <a name="retrieve-a-policy-for-a-workload-type"></a>Recuperación de una directiva para un tipo de carga de trabajo

En el ejemplo siguiente se recuperan las directivas para el tipo de carga de trabajo **AzureFiles**.

```powershell
Get-AzRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureFiles"
```

La salida será similar a la siguiente.

```powershell
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
dailyafs             AzureFiles         AzureStorage         1/10/2018 12:30:00 AM
```

> [!NOTE]
> La zona horaria del campo **BackupTime** de PowerShell es Hora universal coordinada (UTC). Cuando el tiempo de copia de seguridad se muestra en Azure Portal, la hora se ajusta a la zona horaria local.

### <a name="retrieve-a-specific-policy"></a>Recuperación de una directiva específica

La directiva siguiente recupera la directiva de copia de seguridad llamada **dailyafs**.

```powershell
$afsPol =  Get-AzRecoveryServicesBackupProtectionPolicy -Name "dailyafs"
```

### <a name="enable-backup-and-apply-policy"></a>Habilitación de la copia de seguridad y aplicación de la directiva

Habilite la protección con [Enable-AzRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/enable-azrecoveryservicesbackupprotection?view=azps-1.4.0). Una vez que la directiva se ha asociado al almacén, las copias de seguridad se desencadenan según la programación de la directiva.

En el ejemplo siguiente se habilita la protección para el recurso compartido de archivos de Azure **testAzureFileShare**, en la cuenta de almacenamiento **testStorageAcct**, con la directiva **dailyafs**.

```powershell
Enable-AzRecoveryServicesBackupProtection -StorageAccountName "testStorageAcct" -Name "testAzureFS" -Policy $afsPol
```

El comando espera hasta que el trabajo de protección configurado termina y proporciona una salida similar, tal como se muestra.

```cmd
WorkloadName       Operation            Status                 StartTime                                                                                                         EndTime                   JobID
------------             ---------            ------               ---------                                  -------                   -----
testAzureFS       ConfigureBackup      Completed            11/12/2018 2:15:26 PM     11/12/2018 2:16:11 PM     ec7d4f1d-40bd-46a4-9edb-3193c41f6bf6
```

## <a name="trigger-an-on-demand-backup"></a>Desencadenamiento de una copia de seguridad a petición

Use [Backup-AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/backup-azrecoveryservicesbackupitem?view=azps-1.4.0) para ejecutar una copia de seguridad a petición para un recurso compartido de archivos protegido de Azure.

1. Recupere la cuenta de almacenamiento y el recurso compartido de archivos del contenedor del almacén que contenga los datos de copia de seguridad con [Get-AzRecoveryServicesBackupContainer](/powershell/module/az.recoveryservices/get-Azrecoveryservicesbackupcontainer).
2. Para iniciar un trabajo de copia de seguridad, obtenga información sobre la máquina virtual con [Get-AzRecoveryServicesBackupItem](/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupItem).
3. Ejecute una copia de seguridad a petición con [Backup-AzRecoveryServicesBackupItem](/powershell/module/az.recoveryservices/backup-Azrecoveryservicesbackupitem).

Ejecute la copia de seguridad a petición de la manera siguiente:

```powershell
$afsContainer = Get-AzRecoveryServicesBackupContainer -FriendlyName "testStorageAcct" -ContainerType AzureStorage
$afsBkpItem = Get-AzRecoveryServicesBackupItem -Container $afsContainer -WorkloadType "AzureFiles" -Name "testAzureFS"
$job =  Backup-AzRecoveryServicesBackupItem -Item $afsBkpItem
```

El comando devuelve un trabajo con un identificador del cual se puede realizar un seguimiento, como se indica en el ejemplo siguiente.

```powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
testAzureFS       Backup               Completed            11/12/2018 2:42:07 PM     11/12/2018 2:42:11 PM     8bdfe3ab-9bf7-4be6-83d6-37ff1ca13ab6
```

Las instantáneas de recursos compartidos de archivos de Azure se usan al realizar copias de seguridad, por lo que el trabajo normalmente se completa en el momento en que el comando devuelve esta salida.

### <a name="using-on-demand-backups-to-extend-retention"></a>Uso de copias de seguridad a petición para ampliar la retención

Las copias de seguridad a petición se pueden usar para conservar las instantáneas durante 10 años. Los programadores pueden usarse para ejecutar scripts de PowerShell a petición con la retención seleccionada. De este modo, se toman instantáneas a intervalos regulares cada semana, mes o año. Al tomar instantáneas periódicas, haga referencia a las [limitaciones de las copias de seguridad a petición](https://docs.microsoft.com/azure/backup/backup-azure-files-faq#how-many-on-demand-backups-can-i-take-per-file-share) mediante Azure Backup.

Si busca scripts de ejemplo, puede ver el script de ejemplo en GitHub (<https://github.com/Azure-Samples/Use-PowerShell-for-long-term-retention-of-Azure-Files-Backup>) con el runbook de Azure Automation que le permite programar las copias de seguridad de forma periódica y conservarlas hasta 10 años.

### <a name="modify-the-protection-policy"></a>Modificación de la directiva de protección

Para cambiar la directiva empleada para la realizar la copia de seguridad del recurso compartido de archivos de Azure, use [Enable-AzRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/enable-azrecoveryservicesbackupprotection?view=azps-1.4.0). Especifique el elemento de copia de seguridad pertinente y la nueva directiva de copia de seguridad.

En el siguiente ejemplo se cambia la directiva de protección de **testAzureFS** de **dailyafs** a **monthlyafs**.

```powershell
$monthlyafsPol =  Get-AzRecoveryServicesBackupProtectionPolicy -Name "monthlyafs"
$afsContainer = Get-AzRecoveryServicesBackupContainer -FriendlyName "testStorageAcct" -ContainerType AzureStorage
$afsBkpItem = Get-AzRecoveryServicesBackupItem -Container $afsContainer -WorkloadType AzureFiles -Name "testAzureFS"
Enable-AzRecoveryServicesBackupProtection -Item $afsBkpItem -Policy $monthlyafsPol
```

## <a name="restore-azure-file-shares-and-files"></a>Restauración de archivos y recursos compartidos de archivos de Azure

Puede restaurar un recurso compartido de archivos al completo o archivos específicos de dicho recurso compartido. La restauración puede llevarse a cabo en la ubicación original o en una ubicación alternativa.

### <a name="fetch-recovery-points"></a>Captura de puntos de recuperación

Use [Get-AzRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackuprecoverypoint?view=azps-1.4.0) para enumerar todos los puntos de recuperación del elemento del que se ha realizado la copia de seguridad.

En el script siguiente:

* La variable **$rp**es una matriz de puntos de recuperación para el elemento de copia de seguridad seleccionado de los últimos siete días.
* La matriz se ordena en orden inverso de tiempo con el punto de recuperación más reciente en el índice **0**.
* Use la indexación de matrices de PowerShell estándar para seleccionar el punto de recuperación.
* En el ejemplo, **$rp[0]** selecciona el último punto de recuperación.

```powershell
$startDate = (Get-Date).AddDays(-7)
$endDate = Get-Date
$rp = Get-AzRecoveryServicesBackupRecoveryPoint -Item $afsBkpItem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()

$rp[0] | fl
```

La salida será similar a la siguiente.

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

Una vez seleccionado el punto de recuperación adecuado, restaure el recurso compartido de archivos o el archivo en la ubicación original o en una ubicación alternativa.

### <a name="restore-an-azure-file-share-to-an-alternate-location"></a>Restauración de un recurso compartido de archivos de Azure en una ubicación alternativa

Use [Restore-AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/restore-azrecoveryservicesbackupitem?view=azps-1.4.0) para restaurar en el punto de recuperación seleccionado. Especifique estos parámetros para identificar la ubicación alternativa:

* **TargetStorageAccountName**: La cuenta de almacenamiento en la que se restaura el contenido de la copia de seguridad. La cuenta de almacenamiento de destino debe estar en la misma ubicación que el almacén.
* **TargetFileShareName**: Los recursos compartidos de archivos de la cuenta de almacenamiento de destino en la que se restaura el contenido de la copia de seguridad.
* **TargetFolder**: La carpeta del recurso compartido de archivos en la que se restauran los datos. Si el contenido de la copia de seguridad debe restaurarse en una carpeta raíz, asigne los valores de la carpeta de destino como una cadena vacía.
* **ResolveConflict**: Instrucción en caso de conflicto con los datos restaurados. Acepta **Overwrite** o **Skip**.

Ejecute el cmdlet con los parámetros de la manera siguiente:

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -TargetStorageAccountName "TargetStorageAcct" -TargetFileShareName "DestAFS" -TargetFolder "testAzureFS_restored" -ResolveConflict Overwrite
```

El comando devuelve un trabajo con un identificador del cual se puede realizar un seguimiento, como se indica en el ejemplo siguiente.

```powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
testAzureFS        Restore              InProgress           12/10/2018 9:56:38 AM                               9fd34525-6c46-496e-980a-3740ccb2ad75
```

### <a name="restore-an-azure-file-to-an-alternate-location"></a>Restauración de un archivo de Azure en una ubicación alternativa

Use [Restore-AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/restore-azrecoveryservicesbackupitem?view=azps-1.4.0) para restaurar en el punto de recuperación seleccionado. Especifique estos parámetros para identificar la ubicación alternativa y para identificar de forma única el archivo que quiere restaurar.

* **TargetStorageAccountName**: La cuenta de almacenamiento en la que se restaura el contenido de la copia de seguridad. La cuenta de almacenamiento de destino debe estar en la misma ubicación que el almacén.
* **TargetFileShareName**: Los recursos compartidos de archivos de la cuenta de almacenamiento de destino en la que se restaura el contenido de la copia de seguridad.
* **TargetFolder**: La carpeta del recurso compartido de archivos en la que se restauran los datos. Si el contenido de la copia de seguridad debe restaurarse en una carpeta raíz, asigne los valores de la carpeta de destino como una cadena vacía.
* **SourceFilePath**: La ruta de acceso absoluta del archivo que se va a restaurar en el recurso compartido de archivos, como una cadena. Esta ruta de acceso es la misma ruta utilizada en el cmdlet de PowerShell **Get-AzStorageFile**.
* **SourceFileType**: Indica si se ha seleccionado un directorio o un archivo. Admite **Directory** o **File**.
* **ResolveConflict**: Instrucción en caso de conflicto con los datos restaurados. Acepta **Overwrite** o **Skip**.

Los parámetros adicionales (SourceFilePath y SourceFileType) solo están relacionadas con el archivo individual que quiere restaurar.

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -TargetStorageAccountName "TargetStorageAcct" -TargetFileShareName "DestAFS" -TargetFolder "testAzureFS_restored" -SourceFileType File -SourceFilePath "TestDir/TestDoc.docx" -ResolveConflict Overwrite
```

Este comando devuelve un trabajo con un identificador del cual se puede realizar un seguimiento, como se indica en la sección anterior.

### <a name="restore-azure-file-shares-and-files-to-the-original-location"></a>Restauración de archivos y recursos compartidos de archivos de Azure en la ubicación original

Al restaurar en una ubicación original, no es necesario especificar parámetros relacionados con el destino. Solo debe proporcionarse **ResolveConflict**.

#### <a name="overwrite-an-azure-file-share"></a>Sobrescribir un recurso compartido de archivos de Azure

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -ResolveConflict Overwrite
```

#### <a name="overwrite-an-azure-file"></a>Sobrescribir un archivo de Azure

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -SourceFileType File -SourceFilePath "TestDir/TestDoc.docx" -ResolveConflict Overwrite
```

## <a name="track-backup-and-restore-jobs"></a>Seguimiento de trabajos de copia de seguridad y restauración

Las operaciones de copia de seguridad y restauración a petición devuelven un trabajo con un identificador, como se ha indicado al [ejecutar una copia de seguridad a petición](#trigger-an-on-demand-backup). Use el cmdlet [Get-AzRecoveryServicesBackupJobDetails](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupjob?view=azps-1.4.0) para hacer un seguimiento del progreso del trabajo y de los detalles.

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
1073871825 Microsoft Azure Backup encountered an internal error. Wait for a few minutes and then try the operation again. If the issue persists, please contact Microsoft support.
```

## <a name="next-steps"></a>Pasos siguientes

[Obtenga información](backup-afs.md) sobre cómo realizar copias de seguridad de archivos de Azure Files en Azure Portal.
