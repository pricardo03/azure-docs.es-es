---
title: Implementación y administración de copias de seguridad para las máquinas virtuales implementadas según el modelo de Resource Manager mediante PowerShell
description: Use PowerShell para implementar y administrar copias de seguridad de Azure para máquinas virtuales implementadas según el modelo de Resource Manager.
services: backup
author: markgalioto
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 9/10/2018
ms.author: markgal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 19dd385effbdea0d9cd4209ec79f7582c0943e0c
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2018
ms.locfileid: "44720045"
---
# <a name="use-powershell-to-back-up-and-restore-virtual-machines"></a>Uso de PowerShell para crear copias de seguridad de máquinas virtuales y restaurarlas

En este artículo se muestra cómo usar cmdlets de Azure PowerShell para realizar copias de seguridad de una máquina virtual (VM) de Azure y recuperarlas desde un almacén de Recovery Services. Un almacén de Recovery Services es un recurso de Azure Resource Manager y se usa para proteger datos y recursos en los servicios Azure Backup y Azure Site Recovery. 

> [!NOTE]
> Azure cuenta con dos modelos de implementación para crear recursos y trabajar con ellos: [Resource Manager y el modelo clásico](../azure-resource-manager/resource-manager-deployment-model.md). La información de este artículo es para su uso con las máquinas virtuales creadas con el modelo de Resource Manager.
>
>

Este artículo le guiará en el uso de PowerShell para proteger una máquina virtual y restaurar datos a partir de un punto de recuperación.

## <a name="concepts"></a>Conceptos
Si no está familiarizado con el servicio Azure Backup, puede obtener información general al respecto en el artículo [¿Qué es Azure Backup?](backup-introduction-to-azure-backup.md) Antes de comenzar, asegúrese de abordar los requisitos previos necesarios para trabajar con Azure Backup y las limitaciones de la solución actual de copia de seguridad de VM.

Para usar PowerShell de forma eficaz, es preciso conocer la jerarquía de los objetos y desde dónde empezar.

![Jerarquía de objetos de Recovery Services](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

Para ver la referencia del cmdlet de PowerShell AzureRm.RecoveryServices.Backup, vea [Azure Backup - Recovery Services Cmdlets](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup) (Azure Backup: cmdlets de Recovery Services) en la biblioteca de Azure.

## <a name="setup-and-registration"></a>Instalación y registro

Para empezar:

1. [Descargue la versión más reciente de PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) (la versión mínima necesaria es 1.4.0).

2. Para buscar los cmdlets de PowerShell de Azure Backup disponibles, escriba el siguiente comando:
   
    ```powershell
    Get-Command *azurermrecoveryservices*
    ```    
    Aparecen los alias y cmdlets de Azure Backup, Azure Site Recovery y el almacén de Recovery Services. La imagen siguiente es un ejemplo de lo que verá. No es la lista completa de los cmdlets.

    ![Lista de Recovery Services](./media/backup-azure-vms-automation/list-of-recoveryservices-ps.png)

3. Inicie sesión en su cuenta de Azure mediante el cmdlet **Connect-AzureRmAccount**. El cmdlet abrirá una página web que le solicitará las credenciales de la cuenta:

    * Como alternativa, puede incluir sus credenciales de cuenta como un parámetro en el cmdlet **Connect-AzureRmAccount** mediante el parámetro **-Credential**.
    * Si usted es un asociado CSP que trabaja en nombre de un inquilino, especifique el cliente como inquilino usando su TenantID o su nombre de dominio principal de inquilino. Por ejemplo: **Connect-AzureRmAccount -Tenant "fabrikam.com"**.

4. Ya que una cuenta puede tener varias suscripciones, le recomendamos que asocie la suscripción que quiera usar a esa cuenta:

    ```powershell
    Select-AzureRmSubscription -SubscriptionName $SubscriptionName
    ```

5. Si es la primera vez que usa Azure Backup, debe usar el cmdlet **[Register-AzureRmResourceProvider](http://docs.microsoft.com/powershell/module/azurerm.resources/register-azurermresourceprovider)** para registrar el proveedor de Azure Recovery Services en su suscripción.

    ```powershell
    Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

6. Puede comprobar que los proveedores se registraron correctamente mediante los siguientes comandos:
    ```powershell
    Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ``` 
    En la salida del comando, **RegistrationState** se debe cambiar a **Registrado**. En caso contrario, vuelva a ejecutar el cmdlet **[Register-AzureRmResourceProvider](http://docs.microsoft.com/powershell/module/azurerm.resources/register-azurermresourceprovider)**.

Las siguientes tareas se pueden automatizar con PowerShell:

* [Creación de un almacén de Recovery Services](backup-azure-vms-automation.md#create-a-recovery-services-vault)
* [Copia de seguridad de máquinas virtuales de Azure](backup-azure-vms-automation.md#back-up-azure-vms)
* [Desencadenamiento de un trabajo de copia de seguridad](backup-azure-vms-automation.md#trigger-a-backup-job)
* [Supervisión de un trabajo de copia de seguridad](backup-azure-vms-automation.md#monitoring-a-backup-job)
* [Restauración de máquinas virtuales de Azure](backup-azure-vms-automation.md#restore-an-azure-vm)

## <a name="create-a-recovery-services-vault"></a>Creación de un almacén de Recovery Services

Los siguientes pasos le guiarán por el proceso de creación de un almacén de Recovery Services. Un almacén de Recovery Services no es lo mismo que un almacén de copia de seguridad.

1. El almacén de Recovery Services es un recurso de Resource Manager, por lo que deberá colocarlo dentro de un grupo de recursos. Puede usar un grupo de recursos existente o crear uno con el cmdlet **[New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup)**. Al crear un grupo de recursos, especifique el nombre y la ubicación.  

    ```powershell
    New-AzureRmResourceGroup -Name "test-rg" -Location "West US"
    ```
2. Use el cmdlet **[New-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault)** para crear el almacén de Recovery Services. Asegúrese de especificar para el almacén la misma ubicación del grupo de recursos.

    ```powershell
    New-AzureRmRecoveryServicesVault -Name "testvault" -ResourceGroupName "test-rg" -Location "West US"
    ```
3. Especifique el tipo de redundancia de almacenamiento que se usará: [almacenamiento con redundancia local (LRS)](../storage/common/storage-redundancy-lrs.md) o [almacenamiento con redundancia geográfica (GRS)](../storage/common/storage-redundancy-grs.md). En el ejemplo siguiente se muestra que la opción -BackupStorageRedundancy para testvault está establecida en GeoRedundant.

    ```powershell
    $vault1 = Get-AzureRmRecoveryServicesVault -Name "testvault"
    Set-AzureRmRecoveryServicesBackupProperties  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

   > [!TIP]
   > Muchos de los cmdlets de Azure Backup requieren el objeto de almacén de Recovery Services como entrada. Por este motivo, es conveniente almacenar el objeto de almacén de Recovery Services de Backup en una variable.
   >
   >

## <a name="view-the-vaults-in-a-subscription"></a>Visualización de los almacenes de una suscripción

Para ver todos los almacenes de la suscripción, use **[Get-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/get-azurermrecoveryservicesvault)**:

```powershell
Get-AzureRmRecoveryServicesVault
```

El resultado es similar al ejemplo siguiente, tenga en cuenta que los valores de ResourceGroupName y Location asociados se proporcionan.

```
Name              : Contoso-vault
ID                : /subscriptions/1234
Type              : Microsoft.RecoveryServices/vaults
Location          : WestUS
ResourceGroupName : Contoso-docs-rg
SubscriptionId    : 1234-567f-8910-abc
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
```


## <a name="back-up-azure-vms"></a>Copia de seguridad de máquinas virtuales de Azure

Use un almacén de Recovery Services para proteger sus máquinas virtuales. Antes de aplicar la protección, establezca el contexto de almacén (el tipo de datos protegido en el almacén) y compruebe la directiva de protección. La directiva de protección consiste en la programación que indica cuándo se debe ejecutar el trabajo de copia de seguridad y cuánto tiempo se conserva cada instantánea de copia de seguridad.

### <a name="set-vault-context"></a>Establecer el contexto de almacén

Antes de habilitar la protección en una máquina virtual, use **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)** para establecer el contexto de almacén. Una vez que se haya establecido el contexto de almacén, se aplica a todos los cmdlets posteriores. En el ejemplo siguiente se establece el contexto del almacén *testvault*.

```powershell
Get-AzureRmRecoveryServicesVault -Name "testvault" | Set-AzureRmRecoveryServicesVaultContext
```

### <a name="create-a-protection-policy"></a>Creación de una directiva de protección

Cuando se crea un almacén de Recovery Services, este incluye las directivas de retención y protección predeterminadas. La directiva de protección predeterminada activa un trabajo de copia de seguridad cada día a la hora indicada. La directiva de retención predeterminada conserva el punto de recuperación diario durante 30 días. Puede utilizar la directiva predeterminada para proteger rápidamente la máquina virtual y modificar la directiva más adelante con detalles diferentes.

Use **[Get-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupprotectionpolicy)** para ver las directivas de protección disponibles en el almacén. Puede usar este cmdlet para obtener una directiva específica o para ver las directivas asociadas a un tipo de carga de trabajo. En el ejemplo siguiente se obtienen las directivas para el tipo de carga de trabajo AzureVM.

```powershell
Get-AzureRmRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureVM"
```

La salida es similar a la del ejemplo siguiente:

```
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
DefaultPolicy        AzureVM            AzureVM              4/14/2016 5:00:00 PM
```

> [!NOTE]
> La zona horaria del campo BackupTime en PowerShell es UTC. Sin embargo, cuando el tiempo de copia de seguridad se muestra en Azure Portal, la hora se ajusta a la zona horaria local.
>
>

Una directiva de protección de copia de seguridad está asociada con al menos una directiva de retención. La directiva de retención define el tiempo que se conserva el punto de recuperación antes de que se elimine. Use **[Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupretentionpolicyobject)** para ver la directiva de retención predeterminada.  Del mismo modo, puede usar **[Get-AzureRmRecoveryServicesBackupSchedulePolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupschedulepolicyobject)** para obtener la directiva de programación predeterminada. El cmdlet **[New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)** crea un objeto de PowerShell que contiene información de la directiva de copia de seguridad. Los objetos de directiva de retención y programación se usan como entradas para el cmdlet **[New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)**. En el ejemplo siguiente se almacenan la directiva de programación y la directiva de retención en variables. En el ejemplo se usan esas variables para definir los parámetros al crear la directiva de protección *NewPolicy*.

```powershell
$schPol = Get-AzureRmRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureVM"
$retPol = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
New-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy" -WorkloadType "AzureVM" -RetentionPolicy $retPol -SchedulePolicy $schPol
```

La salida es similar a la del ejemplo siguiente:

```
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
NewPolicy           AzureVM            AzureVM              4/24/2016 1:30:00 AM
```

### <a name="enable-protection"></a>Habilitar protección

Una vez que haya definido la directiva de protección, todavía debe habilitar la directiva para un elemento. Use **[Enable-AzureRmRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/enable-azurermrecoveryservicesbackupprotection)** para habilitar la protección. Para habilitar la protección son necesarios dos objetos: el elemento y la directiva. Después de que la directiva se haya asociado con el almacén, el flujo de trabajo de copia de seguridad se desencadena a la hora definida en la programación de la directiva.

En los ejemplos siguientes se habilita la protección para el elemento V2VM mediante la directiva NewPolicy. Los ejemplos varían en función de si la VM está cifrada y del tipo de cifrado. 

Para habilitar la protección en **VM de Resource Manager sin cifrar**:

```powershell
$pol = Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

Para habilitar la protección en **VM cifradas (mediante BEK y KEK)**, debe conceder permiso al servicio Azure Backup para que lea las claves y los secretos del almacén de claves.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToKeys backup,get,list -PermissionsToSecrets get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
$pol = Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

Para habilitar la protección en **VM cifradas (mediante BEK únicamente)**, debe conceder permiso al servicio Azure Backup para que lea los secretos del almacén de claves.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToSecrets backup,get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
$pol = Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

> [!NOTE]
> Si está usando la nube de Azure Government, use el valor ff281ffe-705c-4f53-9f37-a40e6f2c68f3 para el parámetro **-ServicePrincipalName** en el cmdlet [Set-AzureRmKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy).
>
>

Para habilitar la protección en una VM clásica:

```powershell
$pol = Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V1VM" -ServiceName "ServiceName1"
```

### <a name="modify-a-protection-policy"></a>Modificación de una directiva de protección

Para modificar la directiva de protección, use [Set-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/set-azurermrecoveryservicesbackupprotectionpolicy) para modificar los objetos SchedulePolicy o RetentionPolicy.

En el ejemplo siguiente se cambia la retención del punto de recuperación a 365 días.

```powershell
$retPol = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
$retPol.DailySchedule.DurationCountInDays = 365
$pol = Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
Set-AzureRmRecoveryServicesBackupProtectionPolicy -Policy $pol  -RetentionPolicy $RetPol
```

## <a name="trigger-a-backup"></a>Desencadenar una copia de seguridad

Use **[Backup-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/backup-azurermrecoveryservicesbackupitem)** para desencadenar un trabajo de copia de seguridad. Si se trata de la copia de seguridad inicial, es una copia de seguridad completa. Las sucesivas copias de seguridad toman una copia incremental. Asegúrese de usar **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)** para establecer el contexto de almacén antes de desencadenar el trabajo de copia de seguridad. En el siguiente ejemplo se da por supuesto que ya se había establecido el contexto del almacén.

```powershell
$namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM"
$item = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType "AzureVM"
$job = Backup-AzureRmRecoveryServicesBackupItem -Item $item
```

La salida es similar a la del ejemplo siguiente:

```
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM              Backup              InProgress          4/23/2016                  5:00:30 PM                cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

> [!NOTE]
> La zona horaria de los campos de StartTime y EndTime en PowerShell es UTC. Sin embargo, cuando la hora se muestra en Azure Portal, esta se ajusta a la zona horaria local.
>
>

## <a name="monitoring-a-backup-job"></a>Supervisión de trabajos de copia de seguridad

Puede supervisar las operaciones de ejecución prolongada, como los trabajos de copia de seguridad, sin usar Azure Portal. Para obtener el estado de un trabajo en curso, use el cmdlet **[Get-AzureRmRecoveryservicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjob)**. Este cmdlet obtiene los trabajos de copia de seguridad de un almacén específico, y dicho almacén se especifica en el contexto de almacén. En el ejemplo siguiente se obtiene el estado de un trabajo en curso como una matriz y se almacena el estado en la variable $joblist.

```powershell
$joblist = Get-AzureRmRecoveryservicesBackupJob –Status "InProgress"
$joblist[0]
```

La salida es similar a la del ejemplo siguiente:

```
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM             Backup               InProgress            4/23/2016                5:00:30 PM                cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

En lugar de sondear si estos trabajos han finalizado (lo que supone un código adicional innecesario), use el cmdlet **[Wait-AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)**. Este cmdlet detiene la ejecución hasta que el trabajo se complete o se alcance el valor de tiempo de espera especificado.

```powershell
Wait-AzureRmRecoveryServicesBackupJob -Job $joblist[0] -Timeout 43200
```

## <a name="restore-an-azure-vm"></a>Restauración de máquinas virtuales de Azure

Hay una diferencia importante entre restaurar una VM mediante Azure Portal y hacerlo con PowerShell. Con PowerShell, la operación de restauración se completa una vez que se creen los discos y la información de configuración a partir del punto de recuperación. La operación de restauración no crea la máquina virtual. Para crear una máquina virtual desde el disco, consulte la sección sobre la [creación de la máquina virtual a partir de los discos almacenados](backup-azure-vms-automation.md#create-a-vm-from-restored-disks). Si no quiere restaurar toda la VM, sino que quiere restaurar o recuperar algunos archivos desde una copia de seguridad de la VM de Azure, consulte la [sección recuperación de archivos](backup-azure-vms-automation.md#restore-files-from-an-azure-vm-backup).

> [!Tip]
> La operación de restauración no crea la máquina virtual.
>
>

En el siguiente gráfico se muestra la jerarquía de objetos desde RecoveryServicesVault hasta BackupRecoveryPoint.

![Jerarquía de objetos de Recovery Services donde se muestra BackupContainer](./media/backup-azure-vms-arm-automation/backuprecoverypoint-only.png)

Para restaurar los datos de una copia de seguridad, identifique el elemento del que se realizó la copia de seguridad y el punto de recuperación que contiene los datos de un momento específico. Use el cmdlet **[Restore-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)** para restaurar los datos del almacén en la cuenta.

Los pasos básicos para restaurar una máquina virtual de Azure son los siguientes:

* Seleccione la máquina virtual.
* Elija un punto de recuperación.
* Restaure los discos.
* Cree la VM a partir de los discos almacenados.

### <a name="select-the-vm"></a>Selección de la máquina virtual

Para obtener el objeto de PowerShell que identifica el elemento de copia de seguridad correcto, comience en el contenedor del almacén y avance hacia abajo por la jerarquía de objetos. Para seleccionar el contenedor que representa la máquina virtual, use el cmdlet **[Get-AzureRmRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)** y canalícelo al cmdlet **[Get-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)**.

```powershell
$namedContainer = Get-AzureRmRecoveryServicesBackupContainer  -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM"
$backupitem = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer  -WorkloadType "AzureVM"
```

### <a name="choose-a-recovery-point"></a>Elección de un punto de recuperación

Use el cmdlet **[Get-AzureRmRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)** para enumerar todos los puntos de recuperación del elemento de copia de seguridad. Después, elija el punto de recuperación que se debe restaurar. Si no está seguro de qué punto de recuperación debe usar, se recomienda elegir el punto RecoveryPointType = AppConsistent más reciente de la lista.

En el siguiente script, la variable **$rp**es una matriz de puntos de recuperación para el elemento de copia de seguridad seleccionado de los últimos siete días. La matriz se ordena en orden inverso de tiempo con el punto de recuperación más reciente en el índice 0. Use la indexación de matrices de PowerShell estándar para seleccionar el punto de recuperación. En el ejemplo, $rp[0] selecciona el último punto de recuperación.

```powershell
$startDate = (Get-Date).AddDays(-7)
$endDate = Get-Date
$rp = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $backupitem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()
$rp[0]
```

La salida es similar a la del ejemplo siguiente:

```
RecoveryPointAdditionalInfo :
SourceVMStorageType         : NormalStorage
Name                        : 15260861925810
ItemName                    : VM;iaasvmcontainer;RGName1;V2VM
RecoveryPointId             : /subscriptions/XX/resourceGroups/ RGName1/providers/Microsoft.RecoveryServices/vaults/testvault/backupFabrics/Azure/protectionContainers/IaasVMContainer;iaasvmcontainer;RGName1;V2VM/protectedItems/VM;iaasvmcontainer; RGName1;V2VM/recoveryPoints/15260861925810
RecoveryPointType           : AppConsistent
RecoveryPointTime           : 4/23/2016 5:02:04 PM
WorkloadType                : AzureVM
ContainerName               : IaasVMContainer;iaasvmcontainer; RGName1;V2VM
ContainerType               : AzureVM
BackupManagementType        : AzureVM
```

### <a name="restore-the-disks"></a>Restauración de los discos

Use el cmdlet **[Restore-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)** para restaurar los datos y la configuración de un elemento de copia de seguridad a un punto de recuperación. Una vez que haya identificado un punto de recuperación, úselo como valor para el parámetro **-RecoveryPoint**. En el ejemplo anterior, **$rp[0]** era el punto de recuperación para usar. En el código de ejemplo siguiente, **$rp[0]** es el punto de recuperación que se va a restaurar en el disco.

Para restaurar los discos y la información de configuración:

```powershell
$restorejob = Restore-AzureRmRecoveryServicesBackupItem -RecoveryPoint $rp[0] -StorageAccountName "DestAccount" -StorageAccountResourceGroupName "DestRG"
$restorejob
```
La salida es similar a la del ejemplo siguiente:

```
WorkloadName     Operation          Status               StartTime                 EndTime            JobID
------------     ---------          ------               ---------                 -------          ----------
V2VM              Restore           InProgress           4/23/2016 5:00:30 PM                        cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

Use el cmdlet **[Wait-AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)** para esperar a que se complete el trabajo de restauración.

```powershell
Wait-AzureRmRecoveryServicesBackupJob -Job $restorejob -Timeout 43200
```

Una vez que se haya completado el trabajo de recuperación, use el cmdlet **[Get-AzureRmRecoveryServicesBackupJobDetails](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjobdetails)** para obtener los detalles de la operación de restauración. La propiedad JobDetails tiene la información necesaria para recompilar la máquina virtual.

```powershell
$restorejob = Get-AzureRmRecoveryServicesBackupJob -Job $restorejob
$details = Get-AzureRmRecoveryServicesBackupJobDetails -Job $restorejob
```

Una vez que restaure los discos, vaya a la siguiente sección para crear la máquina virtual.

## <a name="create-a-vm-from-restored-disks"></a>Creación de una máquina virtual a partir de discos restaurados

Tras haber restaurado los discos, siga estos pasos para crear y configurar la máquina virtual a partir del disco.

> [!NOTE]
> Para crear máquinas virtuales cifradas a partir de discos restaurados, el rol de Azure debe tener permiso para realizar la acción, **Microsoft.KeyVault/vaults/deploy/action**. Si su rol no tiene este permiso, cree un rol personalizado con esta acción. Para obtener más información, vea [Roles personalizados en RBAC de Azure](../role-based-access-control/custom-roles.md).
>
>

1. Realice una consulta destinada a las propiedades de los discos restaurados para obtener los detalles del trabajo.

   ```powershell
   $properties = $details.properties
   $storageAccountName = $properties["Target Storage Account Name"]
   $containerName = $properties["Config Blob Container Name"]
   $configBlobName = $properties["Config Blob Name"]
   ```

2. Establezca el contexto de Azure Storage y restaure el archivo de configuración JSON.

   ```powershell
   Set-AzureRmCurrentStorageAccount -Name $storageaccountname -ResourceGroupName "testvault"
   $destination_path = "C:\vmconfig.json"
   Get-AzureStorageBlobContent -Container $containerName -Blob $configBlobName -Destination $destination_path
   $obj = ((Get-Content -Path $destination_path -Raw -Encoding Unicode)).TrimEnd([char]0x00) | ConvertFrom-Json
   ```

3. Utilice el archivo de configuración JSON para crear la configuración de la máquina virtual.

   ```powershell
   $vm = New-AzureRmVMConfig -VMSize $obj.'properties.hardwareProfile'.vmSize -VMName "testrestore"
   ```

4. Conecte el disco del sistema operativo y los discos de datos. En este paso se proporcionan ejemplos de diversas configuraciones de VM administrada y cifrada. Use el ejemplo que se adapte a su configuración de VM.

   * **VM no administradas y no cifradas**: Use el ejemplo siguiente para VM no administradas y no cifradas.

       ```powershell
       Set-AzureRmVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.StorageProfile'.osDisk.vhd.Uri -CreateOption "Attach"
       $vm.StorageProfile.OsDisk.OsType = $obj.'properties.StorageProfile'.OsDisk.OsType
       foreach($dd in $obj.'properties.StorageProfile'.DataDisks)
       {
        $vm = Add-AzureRmVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
       }
       ```

   * **VM cifradas no administradas (solo mediante BEK)**: En el caso de las VM cifradas no administradas (cifradas solo mediante BEK), debe restaurar el secreto en el almacén de claves para poder asociar discos. Para obtener más información, consulte el artículo [Restauración de una máquina virtual de Azure desde un punto de recuperación de Azure Backup](backup-azure-restore-key-secret.md). En el ejemplo siguiente se muestra cómo adjuntar discos del sistema operativo y de datos para máquinas virtuales cifradas. Al establecer el disco del sistema operativo, asegúrese de mencionar el tipo de sistema operativo pertinente.

      ```powershell
      $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
      $dekUrl = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
      Set-AzureRmVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.storageProfile'.osDisk.vhd.uri -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows/Linux
      $vm.StorageProfile.OsDisk.OsType = $obj.'properties.storageProfile'.osDisk.osType
      foreach($dd in $obj.'properties.storageProfile'.dataDisks)
      {
       $vm = Add-AzureRmVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
      }
      ```
      Use el comando siguiente para habilitar manualmente el cifrado de los discos de datos.

      ```powershell
      Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $RG -VMName $vm -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $dekUrl -DiskEncryptionKeyVaultId $dekUrl -VolumeType Data
      ```

   * **VM cifradas no administradas (BEK y KEK)**: En el caso de las VM cifradas no administradas (cifradas mediante BEK y KEK), restaure la clave y el secreto en el almacén de claves para poder asociar los discos. Para obtener más información, consulte el artículo [Restauración de una máquina virtual de Azure desde un punto de recuperación de Azure Backup](backup-azure-restore-key-secret.md). En el ejemplo siguiente se muestra cómo adjuntar discos del sistema operativo y de datos para máquinas virtuales cifradas.

      ```powershell
      $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
      $kekUrl = "https://ContosoKeyVault.vault.azure.net:443/keys/ContosoKey007/x9xxx00000x0000x9b9949999xx0x006"
      $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
      Set-AzureRmVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.storageProfile'.osDisk.vhd.uri -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows
      $vm.StorageProfile.OsDisk.OsType = $obj.'properties.storageProfile'.osDisk.osType
      foreach($dd in $obj.'properties.storageProfile'.dataDisks)
       {
       $vm = Add-AzureRmVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
       }
      ```

      Use el comando siguiente para habilitar manualmente el cifrado de los discos de datos.

      ```powershell
      Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $RG -VMName $vm -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $dekUrl -DiskEncryptionKeyVaultId $dekUrl -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -VolumeType Data
      ```

   * **VM administradas y no cifradas**: En el caso de VM administradas y no cifradas, deberá crear discos administrados desde Blob Storage y, después, adjuntar los discos. Para obtener información detallada, vea el artículo [Adjuntar un disco de datos a una máquina virtual de Windows mediante PowerShell](../virtual-machines/windows/attach-disk-ps.md). En el ejemplo de código siguiente se muestra cómo adjuntar discos de datos para máquinas virtuales administradas y no cifradas.

      ```powershell
      $storageType = "StandardLRS"
      $osDiskName = $vm.Name + "_osdisk"
      $osVhdUri = $obj.'properties.storageProfile'.osDisk.vhd.uri
      $diskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $osVhdUri
      $osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk $diskConfig -ResourceGroupName "test"
      Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -CreateOption "Attach" -Windows
      foreach($dd in $obj.'properties.storageProfile'.dataDisks)
       {
       $dataDiskName = $vm.Name + $dd.name ;
       $dataVhdUri = $dd.vhd.uri ;
       $dataDiskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $dataVhdUri ;
       $dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk $dataDiskConfig -ResourceGroupName "test" ;
       Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -ManagedDiskId $dataDisk2.Id -Lun $dd.Lun -CreateOption "Attach"
       }
      ```

   * **VM administradas y cifradas (solo mediante BEK)**: En el caso de las VM administradas y cifradas (cifradas mediante BEK únicamente), debe crear discos administrados desde Blob Storage y luego asociar los discos. Para obtener información detallada, vea el artículo [Adjuntar un disco de datos a una máquina virtual de Windows mediante PowerShell](../virtual-machines/windows/attach-disk-ps.md). En el ejemplo de código siguiente se muestra cómo adjuntar discos de datos para máquinas virtuales administradas y cifradas.

      ```powershell
      $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
      $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
      $storageType = "StandardLRS"
      $osDiskName = $vm.Name + "_osdisk"
      $osVhdUri = $obj.'properties.storageProfile'.osDisk.vhd.uri
      $diskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $osVhdUri
      $osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk $diskConfig -ResourceGroupName "test"
      Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows
      foreach($dd in $obj.'properties.storageProfile'.dataDisks)
       {
       $dataDiskName = $vm.Name + $dd.name ;
       $dataVhdUri = $dd.vhd.uri ;
       $dataDiskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $dataVhdUri ;
       $dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk $dataDiskConfig -ResourceGroupName "test" ;
       Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -ManagedDiskId $dataDisk2.Id -Lun $dd.Lun -CreateOption "Attach"
       }
      ```

       Use el comando siguiente para habilitar manualmente el cifrado de los discos de datos.

       ```powershell
       Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $RG -VMName $vm -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -VolumeType Data
       ```

   * **VM administradas y cifradas (mediante BEK y KEK)**: En el caso de las VM administradas y cifradas (cifradas mediante BEK y KEK), debe crear discos administrados desde Blob Storage y luego asociar los discos. Para obtener información detallada, vea el artículo [Adjuntar un disco de datos a una máquina virtual de Windows mediante PowerShell](../virtual-machines/windows/attach-disk-ps.md). En el ejemplo de código siguiente se muestra cómo adjuntar discos de datos para máquinas virtuales administradas y cifradas.

      ```powershell
      $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
      $kekUrl = "https://ContosoKeyVault.vault.azure.net:443/keys/ContosoKey007/x9xxx00000x0000x9b9949999xx0x006"
      $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
      $storageType = "StandardLRS"
      $osDiskName = $vm.Name + "_osdisk"
      $osVhdUri = $obj.'properties.storageProfile'.osDisk.vhd.uri
      $diskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $osVhdUri
      $osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk $diskConfig -ResourceGroupName "test"
      Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows
      foreach($dd in $obj.'properties.storageProfile'.dataDisks)
       {
       $dataDiskName = $vm.Name + $dd.name ;
       $dataVhdUri = $dd.vhd.uri ;
       $dataDiskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $dataVhdUri ;
       $dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk $dataDiskConfig -ResourceGroupName "test" ;
       Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -ManagedDiskId $dataDisk2.Id -Lun $dd.Lun -CreateOption "Attach"
       }
      ```

      Use el comando siguiente para habilitar manualmente el cifrado de los discos de datos.

      ```powershell
      Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $RG -VMName $vm -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $dekUrl -DiskEncryptionKeyVaultId $dekUrl -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -VolumeType Data
      ```

5. Ajuste la configuración de la red.

    ```powershell
    $nicName="p1234"
    $pip = New-AzureRmPublicIpAddress -Name $nicName -ResourceGroupName "test" -Location "WestUS" -AllocationMethod Dynamic
    $virtualNetwork = New-AzureRmVirtualNetwork -ResourceGroupName "test" -Location "WestUS" -Name "testvNET" -AddressPrefix 10.0.0.0/16
    $virtualNetwork | Set-AzureRmVirtualNetwork
    $vnet = Get-AzureRmVirtualNetwork -Name "testvNET" -ResourceGroupName "test"
    $subnetindex=0
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName "test" -Location "WestUS" -SubnetId $vnet.Subnets[$subnetindex].Id -PublicIpAddressId $pip.Id
    $vm=Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
    ```
6. Cree la máquina virtual.

    ```powershell  
    New-AzureRmVM -ResourceGroupName "test" -Location "WestUS" -VM $vm
    ```

## <a name="restore-files-from-an-azure-vm-backup"></a>Restauración de archivos desde una copia de seguridad de la máquina virtual de Azure

Además de restaurar discos, también puede restaurar archivos individuales desde una copia de seguridad de la máquina virtual de Azure. La funcionalidad de restauración de archivos proporciona acceso a todos los archivos en un punto de recuperación. Administre los archivos a través del Explorador de archivos como lo haría con los archivos normales.

Los pasos básicos para restaurar un archivo desde una copia de seguridad de la VM de Azure son los siguientes:

* Selección de la máquina virtual
* Elección de un punto de recuperación
* Montaje de los discos del punto de recuperación
* Copia de los archivos necesarios
* Desmontaje de los discos


### <a name="select-the-vm"></a>Selección de la máquina virtual

Para obtener el objeto de PowerShell que identifica el elemento de copia de seguridad correcto, comience en el contenedor del almacén y avance hacia abajo por la jerarquía de objetos. Para seleccionar el contenedor que representa la máquina virtual, use el cmdlet **[Get-AzureRmRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)** y canalícelo al cmdlet **[Get-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)**.

```powershell
$namedContainer = Get-AzureRmRecoveryServicesBackupContainer  -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM"
$backupitem = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer  -WorkloadType "AzureVM"
```

### <a name="choose-a-recovery-point"></a>Elección de un punto de recuperación

Use el cmdlet **[Get-AzureRmRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)** para enumerar todos los puntos de recuperación del elemento de copia de seguridad. Después, elija el punto de recuperación que se debe restaurar. Si no está seguro de qué punto de recuperación debe usar, se recomienda elegir el punto RecoveryPointType = AppConsistent más reciente de la lista.

En el siguiente script, la variable **$rp**es una matriz de puntos de recuperación para el elemento de copia de seguridad seleccionado de los últimos siete días. La matriz se ordena en orden inverso de tiempo con el punto de recuperación más reciente en el índice 0. Use la indexación de matrices de PowerShell estándar para seleccionar el punto de recuperación. En el ejemplo, $rp[0] selecciona el último punto de recuperación.

```powershell
$startDate = (Get-Date).AddDays(-7)
$endDate = Get-Date
$rp = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $backupitem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()
$rp[0]
```

La salida es similar a la del ejemplo siguiente:

```
RecoveryPointAdditionalInfo :
SourceVMStorageType         : NormalStorage
Name                        : 15260861925810
ItemName                    : VM;iaasvmcontainer;RGName1;V2VM
RecoveryPointId             : /subscriptions/XX/resourceGroups/ RGName1/providers/Microsoft.RecoveryServices/vaults/testvault/backupFabrics/Azure/protectionContainers/IaasVMContainer;iaasvmcontainer;RGName1;V2VM/protectedItems/VM;iaasvmcontainer; RGName1;V2VM/recoveryPoints/15260861925810
RecoveryPointType           : AppConsistent
RecoveryPointTime           : 4/23/2016 5:02:04 PM
WorkloadType                : AzureVM
ContainerName               : IaasVMContainer;iaasvmcontainer; RGName1;V2VM
ContainerType               : AzureVM
BackupManagementType        : AzureVM
```

### <a name="mount-the-disks-of-recovery-point"></a>Montaje de los discos del punto de recuperación

Use el cmdlet **[Get-AzureRmRecoveryServicesBackupRPMountScript](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprpmountscript)** para obtener el script para montar todos los discos del punto de recuperación.

> [!NOTE]
> Los discos se montan como discos iSCSI conectados a la máquina donde se ejecuta el script. El montaje se produce inmediatamente y no incurre en cargos.
>
>

```powershell
Get-AzureRmRecoveryServicesBackupRPMountScript -RecoveryPoint $rp[0]
```

La salida es similar a la del ejemplo siguiente:

```
OsType  Password        Filename
------  --------        --------
Windows e3632984e51f496 V2VM_wus2_8287309959960546283_451516692429_cbd6061f7fc543c489f1974d33659fed07a6e0c2e08740.exe
```

Ejecute el script en la máquina en la que desea recuperar los archivos. Para ejecutar el script, debe escribir la contraseña proporcionada. Después de que los discos se conecten, use el Explorador de archivos de Windows para navegar por los nuevos volúmenes y archivos. Para obtener más información, consulte el artículo de Backup, [Recuperación de archivos desde una copia de seguridad de máquina virtual de Azure](backup-azure-restore-files-from-vm.md).

### <a name="unmount-the-disks"></a>Desmontaje de los discos

Una vez que se copien los archivos necesarios, desmonte los discos mediante el cmdlet **[Disable-AzureRmRecoveryServicesBackupRPMountScript](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/disable-azurermrecoveryservicesbackuprpmountscript?view=azurermps-5.0.0)**. Asegúrese de desmontar los discos de modo que se quite el acceso a los archivos del punto de recuperación.

```powershell
Disable-AzureRmRecoveryServicesBackupRPMountScript -RecoveryPoint $rp[0]
```

## <a name="next-steps"></a>Pasos siguientes

Si prefiere usar PowerShell para interactuar con los recursos de Azure, consulte el artículo de PowerShell [Implementación y administración de copia de seguridad para Windows Server](backup-client-automation.md). Si administra copias de seguridad de DPM, vea el artículo [Implementación y administración de copias de seguridad para DPM](backup-dpm-automation.md). Estos dos artículos tienen una versión para las implementaciones de Resource Manager y las clásicas.  
