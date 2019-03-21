---
title: Copia de seguridad y restaurar archivos de Azure con Azure Backup y PowerShell
description: Copia de seguridad y restaurar archivos de Azure con Azure Backup y PowerShell.
author: pvrk
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 03/05/2018
ms.author: pullabhk
ms.openlocfilehash: 489de7b2fe87d1bb218e3fce39f269d866d66dd7
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "57845178"
---
# <a name="back-up-and-restore-azure-files-with-powershell"></a>Copia de seguridad y restaurar archivos de Azure con PowerShell

En este artículo se describe cómo usar Azure PowerShell para realizar copias de seguridad y recuperar un recurso compartido de archivos de Azure Files con un [Azure Backup](backup-overview.md) almacén de Recovery Services. 

Este tutorial explica cómo realizar lo siguiente:

> [!div class="checklist"]
> * Configuración de PowerShell y registrar el proveedor de Azure Recovery Services.
> * Cree un almacén de Recovery Services.
> * Configurar copia de seguridad para un recurso compartido de archivos de Azure.
> * Ejecutar un trabajo de copia de seguridad.
> * Restaurar una copia de seguridad de recurso compartido de archivos o un archivo individual desde un recurso compartido.
> * Supervisar copias de seguridad y restaurar trabajos.


## <a name="before-you-start"></a>Antes de comenzar

- [Obtenga más información](backup-azure-recovery-services-vault-overview.md) sobre almacenes de Recovery Services.
- Obtenga información sobre las capacidades de vista previa para [la copia de seguridad de los recursos compartidos de archivos de Azure](backup-azure-files.md).
- Revise la jerarquía de objetos de PowerShell para servicios de recuperación.


## <a name="recovery-services-object-hierarchy"></a>Jerarquía de objetos de Recovery Services

En el diagrama siguiente, se resume la jerarquía de objetos.

![Jerarquía de objetos de Recovery Services](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

Revise el **Az.RecoveryServices** [referencia de cmdlet](/powershell/module/az.recoveryservices) referencia en la biblioteca de Azure.


## <a name="set-up-and-install"></a>Configurar e instalar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Configuración de PowerShell como sigue:

1. [Descargue la versión más reciente de Azure PowerShell](/powershell/azure/install-az-ps). La versión 1.0.0 es la versión mínima requerida.

2. Buscar los cmdlets de PowerShell de Azure Backup con este comando:

    ```powershell
    Get-Command *azrecoveryservices*
    ```
3. Revise los alias y cmdlets de copia de seguridad de Azure, Azure Site Recovery y el almacén de Recovery Services aparecen. Este es un ejemplo de lo que podría ver. No es una lista completa de los cmdlets.

    ![Lista de cmdlets de Recovery Services](./media/backup-azure-afs-automation/list-of-recoveryservices-ps-az.png)

3. Inicie sesión en su cuenta de Azure con **Connect AzAccount**.
4. En la página web que aparece, se le pedirá que escriba sus credenciales de cuenta.

    - Como alternativa, puede incluir sus credenciales de cuenta como un parámetro en el **Connect AzAccount** cmdlet con **-Credential**.
    - Si es un asociado CSP que trabaja en nombre de un inquilino, especifique al cliente como un inquilino, mediante su nombre de dominio principal tenantID o inquilino. Un ejemplo es **Connect-AzAccount -Tenant** fabrikam.com.

4. Asocie la suscripción que desea usar con la cuenta, ya que una cuenta puede tener varias suscripciones.

    ```powershell
    Select-AzSubscription -SubscriptionName $SubscriptionName
    ```

5. Si utiliza Azure Backup por primera vez, use el cmdlet **Register-AzResourceProvider** para registrar el proveedor de Azure Recovery Services con su suscripción.

    ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

6. Compruebe que los proveedores se registraron correctamente:

    ```powershell
    Get-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
7. En la salida del comando, compruebe que **RegistrationState** cambia a **registrado**. Si no, se ejecuta el **Register AzResourceProvider** cmdlet nuevo.



## <a name="create-a-recovery-services-vault"></a>Creación de un almacén de Recovery Services

Siga estos pasos para crear un almacén de Recovery Services.

- El almacén de Recovery Services es un recurso de Resource Manager, por lo que deberá colocarlo dentro de un grupo de recursos. Puede usar un grupo de recursos existente o crear uno con el cmdlet **New-AzResourceGroup**. Al crear un grupo de recursos, especifique el nombre y la ubicación. 

1. Un almacén se coloca en un grupo de recursos. Si no tiene un recurso existente de grupo, cree uno nuevo con el [New AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup?view=azps-1.4.0). En este ejemplo, crearemos un nuevo grupo de recursos en la región Oeste de Estados Unidos.

   ```powershell
   New-AzResourceGroup -Name "test-rg" -Location "West US"
   ```
2. Use la [New AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/New-AzRecoveryServicesVault?view=azps-1.4.0) cmdlet para crear el almacén. Especifique para el almacén la misma ubicación del grupo de recursos.

    ```powershell
    New-AzRecoveryServicesVault -Name "testvault" -ResourceGroupName "test-rg" -Location "West US"
    ```
3. Especifique el tipo de redundancia que se usará para el almacenamiento del almacén.

   - Puede usar [almacenamiento con redundancia local](../storage/common/storage-redundancy-lrs.md) o [almacenamiento con redundancia geográfica](../storage/common/storage-redundancy-grs.md).
   - El ejemplo siguiente se establece la **- BackupStorageRedundancy** opción el[conjunto AzRecoveryServicesBackupProperties](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupproperties?view=azps-1.4.0) cmd para **testvault** establecido en  **Con redundancia geográfica**.

     ```powershell
     $vault1 = Get-AzRecoveryServicesVault -Name "testvault"
     Set-AzRecoveryServicesBackupProperties  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
     ```

### <a name="view-the-vaults-in-a-subscription"></a>Visualización de los almacenes de una suscripción

Para ver todos los almacenes de la suscripción, use [Get-AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesvault?view=azps-1.4.0).

```powershell
Get-AzRecoveryServicesVault
```

El resultado es similar al siguiente. Tenga en cuenta que el grupo de recursos asociado y la ubicación se proporcionan.

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

Store el objeto de almacén en una variable y establezca el contexto de almacén.

- Muchos de los cmdlets de Azure Backup requieren el objeto de almacén de Recovery Services como una entrada, por lo que es conveniente almacenar el objeto de almacén en una variable.
- El contexto de almacén es el tipo de los datos protegidos en el almacén. Establecerlo con [conjunto AzRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesvaultcontext?view=azps-1.4.0). Después de establece el contexto, se aplica a todos los cmdlets posteriores.


En el ejemplo siguiente se establece el contexto del almacén de **testvault**.

```powershell
Get-AzRecoveryServicesVault -Name "testvault" | Set-AzRecoveryServicesVaultContext
```

### <a name="fetch-the-vault-id"></a>Obtener el identificador del almacén

Tenemos previsto dejar de usar el contexto de almacén configuración de acuerdo con las directrices de Azure PowerShell. En su lugar, puede almacenar o recuperar el identificador del almacén y pase a los comandos pertinentes, como sigue:

```powershell
$vaultID = Get-AzRecoveryServicesVault -ResourceGroupName "Contoso-docs-rg" -Name "testvault" | select -ExpandProperty ID
```

## <a name="configure-a-backup-policy"></a>Configuración de una directiva de copia de seguridad

Una directiva de copia de seguridad especifica la programación para copias de seguridad y cuánto tiempo los puntos de recuperación de copia de seguridad se deben mantener:

- Una directiva de copia de seguridad está asociada con al menos una directiva de retención. Una directiva de retención define el tiempo que se conserva un punto de recuperación antes de que se elimine.
- La retención de directiva de copia de seguridad predeterminada mediante la vista [Get AzRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupretentionpolicyobject?view=azps-1.4.0).
- La programación de directiva de copia de seguridad predeterminada mediante la vista [Get AzRecoveryServicesBackupSchedulePolicyObject](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupschedulepolicyobject?view=azps-1.4.0).
-  Usa el [New AzRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupprotectionpolicy?view=azps-1.4.0) para crear una nueva directiva de copia de seguridad. Había entrada de los objetos de directiva de retención y programación.

En el ejemplo siguiente se almacenan la directiva de programación y la directiva de retención en variables. A continuación, usa esos variable como parámetros para una nueva directiva (**NewAFSPolicy**). **NewAFSPolicy** realiza una copia de seguridad diaria y la conserva durante 30 días.

```powershell
$schPol = Get-AzRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureFiles"
$retPol = Get-AzRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureFiles"
New-AzRecoveryServicesBackupProtectionPolicy -Name "NewAFSPolicy" -WorkloadType "AzureFiles" -RetentionPolicy $retPol -SchedulePolicy $schPol
```

El resultado es similar al siguiente.

```powershell
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
NewAFSPolicy           AzureFiles            AzureStorage              10/24/2017 1:30:00 AM
```



## <a name="enable-backup"></a>Habilitación de la copia de seguridad

Después de definir la directiva de copia de seguridad, puede habilitar la protección para el recurso compartido de archivos de Azure mediante la directiva.

### <a name="retrieve-a-backup-policy"></a>Recuperar una directiva de copia de seguridad

Capturar el objeto de directiva relevante con [Get AzRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupprotectionpolicy?view=azps-1.4.0). Usar este cmdlet para obtener una directiva específica, o para ver las directivas asociadas con un tipo de carga de trabajo.

#### <a name="retrieve-a-policy-for-a-workload-type"></a>Recuperar una directiva para un tipo de carga de trabajo

El ejemplo siguiente recupera las directivas para el tipo de carga de trabajo **AzureFiles**.

```powershell
Get-AzRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureFiles"
```

El resultado es similar al siguiente.

```powershell
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
dailyafs             AzureFiles         AzureStorage         1/10/2018 12:30:00 AM
```
> [!NOTE]
> La zona horaria del campo **BackupTime** de PowerShell es Hora universal coordinada (UTC). Cuando el tiempo de copia de seguridad se muestra en Azure Portal, la hora se ajusta a la zona horaria local.

### <a name="retrieve-a-specific-policy"></a>Recuperar una directiva específica

La directiva siguiente recupera la directiva de copia de seguridad llamada **dailyafs**.

```powershell
$afsPol =  Get-AzRecoveryServicesBackupProtectionPolicy -Name "dailyafs"
```

### <a name="enable-backup-and-apply-policy"></a>Habilitar copia de seguridad y aplicar la directiva

Habilitar la protección con [Enable AzRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/enable-azrecoveryservicesbackupprotection?view=azps-1.4.0). Después de la directiva se asocia con el almacén, se activan las copias de seguridad según la programación de la directiva.

El ejemplo siguiente habilita la protección para el recurso compartido de archivos de Azure **testAzureFileShare** en la cuenta de almacenamiento **testStorageAcct**, con la directiva **dailyafs**.

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

Use [AzRecoveryServicesBackupItem de copia de seguridad](https://docs.microsoft.com/powershell/module/az.recoveryservices/backup-azrecoveryservicesbackupitem?view=azps-1.4.0) para ejecutar una copia de seguridad y a petición para un recurso compartido de archivos protegidos de Azure.

1. Recuperar la cuenta de almacenamiento y el archivo compartan desde el contenedor en el almacén que contiene los datos de copia de seguridad con [Get AzRecoveryServicesBackupContainer](/powershell/module/az.recoveryservices.backup/get-Azrecoveryservicesbackupcontainer).
2. Para iniciar un trabajo de copia de seguridad, obtener información acerca de la máquina virtual con [Get AzRecoveryServicesBackupItem](/powershell/module/Az.RecoveryServices.Backup/Get-AzRecoveryServicesBackupItem).
3. Ejecutar una copia de seguridad y a petición con[AzRecoveryServicesBackupItem de copia de seguridad](/powershell/module/az.recoveryservices.backup/backup-Azrecoveryservicesbackupitem).

Ejecute la copia de seguridad y a petición como sigue:
    
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

### <a name="modify-the-protection-policy"></a>Modificación de la directiva de protección

Para cambiar la directiva utilizada para la copia de seguridad del recurso compartido de archivos de Azure, use [Enable AzRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/enable-azrecoveryservicesbackupprotection?view=azps-1.4.0). Especifique el elemento de copia de seguridad pertinente y la nueva directiva de copia de seguridad.

En el siguiente ejemplo se cambia la directiva de protección de **testAzureFS** de **dailyafs** a **monthlyafs**.

```powershell
$monthlyafsPol =  Get-AzRecoveryServicesBackupProtectionPolicy -Name "monthlyafs"
$afsContainer = Get-AzRecoveryServicesBackupContainer -FriendlyName "testStorageAcct" -ContainerType AzureStorage
$afsBkpItem = Get-AzRecoveryServicesBackupItem -Container $afsContainer -WorkloadType AzureFiles -Name "testAzureFS"
Enable-AzRecoveryServicesBackupProtection -Item $afsBkpItem -Policy $monthlyafsPol
```

## <a name="restore-azure-file-shares-and-files"></a>Restaurar archivos y recursos compartidos de archivos de Azure

Puede restaurar un recurso compartido de archivo completo o archivos específicos en el recurso compartido. Puede restaurar en la ubicación original o en una ubicación alternativa. 

### <a name="fetch-recovery-points"></a>Captura de puntos de recuperación

Use [Get AzRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackuprecoverypoint?view=azps-1.4.0) para enumerar todos los puntos de recuperación para el elemento de copia de seguridad.

En el siguiente script:

- La variable **$rp** es una matriz de puntos de recuperación para el elemento seleccionado de copia de seguridad de los últimos siete días.
- La matriz se ordena en orden inverso de tiempo con el punto de recuperación más reciente en el índice **0**.
- Use la indexación de matrices de PowerShell estándar para seleccionar el punto de recuperación.
- En el ejemplo, **$rp[0]** selecciona el último punto de recuperación.

```powershell
$startDate = (Get-Date).AddDays(-7)
$endDate = Get-Date
$rp = Get-AzRecoveryServicesBackupRecoveryPoint -Item $afsBkpItem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()

$rp[0] | fl
```

El resultado es similar al siguiente.

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
Después de selecciona el punto de recuperación pertinentes, restaure el archivo o recurso compartido de archivos en la ubicación original o en una ubicación alternativa.

### <a name="restore-an-azure-file-share-to-an-alternate-location"></a>Restaurar un recurso compartido de archivos de Azure en una ubicación alternativa

Use la [restauración AzRecoveryServicesBackupItem](https://docs.microsoft.com/en-us/powershell/module/az.recoveryservices/restore-azrecoveryservicesbackupitem?view=azps-1.4.0) para restaurar hasta el punto de recuperación seleccionado. Especifique estos parámetros para identificar la ubicación alternativa: 

- **TargetStorageAccountName**: La cuenta de almacenamiento en la que se restaura el contenido de la copia de seguridad. La cuenta de almacenamiento de destino debe estar en la misma ubicación que el almacén.
- **TargetFileShareName**: Los recursos compartidos de archivos de la cuenta de almacenamiento de destino en la que se restaura el contenido de la copia de seguridad.
- **TargetFolder**: La carpeta del recurso compartido de archivos en la que se restauran los datos. Si el contenido de la copia de seguridad debe restaurarse en una carpeta raíz, asigne los valores de la carpeta de destino como una cadena vacía.
- **ResolveConflict**: Instrucción en caso de conflicto con los datos restaurados. Acepta **Overwrite** o **Skip**.

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

### <a name="restore-an-azure-file-to-an-alternate-location"></a>Restaurar un archivo de Azure en una ubicación alternativa

Use la [restauración AzRecoveryServicesBackupItem](https://docs.microsoft.com/en-us/powershell/module/az.recoveryservices/restore-azrecoveryservicesbackupitem?view=azps-1.4.0) para restaurar hasta el punto de recuperación seleccionado. Especifique estos parámetros para identificar la ubicación alternativa y para identificar de forma única el archivo que desea restaurar.

* **TargetStorageAccountName**: La cuenta de almacenamiento en la que se restaura el contenido de la copia de seguridad. La cuenta de almacenamiento de destino debe estar en la misma ubicación que el almacén.
* **TargetFileShareName**: Los recursos compartidos de archivos de la cuenta de almacenamiento de destino en la que se restaura el contenido de la copia de seguridad.
* **TargetFolder**: La carpeta del recurso compartido de archivos en la que se restauran los datos. Si el contenido de la copia de seguridad debe restaurarse en una carpeta raíz, asigne los valores de la carpeta de destino como una cadena vacía.
* **SourceFilePath**: La ruta de acceso absoluta del archivo que se va a restaurar en el recurso compartido de archivos, como una cadena. Esta ruta de acceso es la misma ruta utilizada en el cmdlet de PowerShell **Get-AzStorageFile**.
* **SourceFileType**: Indica si se ha seleccionado un directorio o un archivo. Admite **Directory** o **File**.
* **ResolveConflict**: Instrucción en caso de conflicto con los datos restaurados. Acepta **Overwrite** o **Skip**.

Los parámetros adicionales (SourceFilePath y SourceFileType) sólo están relacionadas con el archivo individual que desee restaurar.

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -TargetStorageAccountName "TargetStorageAcct" -TargetFileShareName "DestAFS" -TargetFolder "testAzureFS_restored" -SourceFileType File -SourceFilePath "TestDir/TestDoc.docx" -ResolveConflict Overwrite
```

Este comando devuelve un trabajo con un Id. de seguimiento, como se muestra en la sección anterior.

### <a name="restore-azure-file-shares-and-files-to-the-original-location"></a>Restaurar archivos y recursos compartidos de archivos de Azure en la ubicación original

Cuando se restaura en una ubicación original, no es necesario especificar los parámetros relacionados con el destino y el destino. Solo debe proporcionarse **ResolveConflict**.

#### <a name="overwrite-an-azure-file-share"></a>Sobrescribir un recurso compartido de archivos de Azure

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -ResolveConflict Overwrite
```

#### <a name="overwrite-an-azure-file"></a>Sobrescribir un archivo de Azure

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -SourceFileType File -SourceFilePath "TestDir/TestDoc.docx" -ResolveConflict Overwrite
```

## <a name="track-backup-and-restore-jobs"></a>Seguimiento de trabajos de copia de seguridad y restauración

Las operaciones de copia de seguridad y restauración de petición devuelvan un trabajo junto con un identificador, como se muestra cuando se [realizó una copia de seguridad y a petición](#trigger-an-on-demand-backup). Use la [Get AzRecoveryServicesBackupJobDetails](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupjob?view=azps-1.4.0) cmdlet para realizar un seguimiento del progreso del trabajo y los detalles.

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
[Obtenga información sobre](backup-azure-files.md) copia de seguridad de Azure Files en Azure portal.
