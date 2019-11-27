---
title: Configuración de la recuperación ante desastres de máquinas virtuales de Hyper-V mediante Azure Site Recovery y PowerShell
description: Automatice la recuperación ante desastres de máquinas virtuales de Hyper-V en Azure con el servicio Azure Site Recovery mediante PowerShell y Azure Resource Manager.
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 06/18/2019
ms.author: sutalasi
ms.openlocfilehash: 73f5f64a64ab28cdb4b57d0904911f62c2020cf0
ms.sourcegitcommit: a22cb7e641c6187315f0c6de9eb3734895d31b9d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2019
ms.locfileid: "74082676"
---
# <a name="set-up-disaster-recovery-to-azure-for-hyper-v-vms-using-powershell-and-azure-resource-manager"></a>Configurar la recuperación ante desastres en Azure para máquinas virtuales de Hyper-V mediante PowerShell y Azure Resource Manager

Gracias a [Azure Site Recovery](site-recovery-overview.md), puede mejorar su estrategia de continuidad de negocio y de recuperación ante desastres (BCDR) mediante la coordinación de la replicación, la conmutación por error y la recuperación de máquinas virtuales de Azure (VM), de máquinas virtuales locales y de servidores físicos.

En este artículo se describe la forma de usar Windows PowerShell con Azure Resource Manager para replicar las máquinas virtuales de Hyper-V en Azure. En el ejemplo que usaremos en este artículo se mostrará cómo replicar en Azure una máquina virtual que se ejecute en un host de Hyper-V.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="azure-powershell"></a>Azure PowerShell

Azure PowerShell ofrece comandos cmdlet para administrar Azure con Windows PowerShell. Los cmdlets de PowerShell de Site Recovery que están disponibles con Azure PowerShell para Azure Resource Manager le permiten proteger y recuperar los servidores en Azure.

No es necesario ser un experto en PowerShell para leer este artículo, pero sí debe conocer conceptos básicos, como módulos, comandos cmdlet y sesiones. Le recomendamos que lea [Introducción a Windows PowerShell](https://technet.microsoft.com/library/hh857337.aspx) y [Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md) (Usar Azure PowerShell con Azure Resource Manager).

> [!NOTE]
> Los asociados de Microsoft que forman parte del programa Proveedor de soluciones en la nube (CSP) pueden configurar y administrar la protección de los servidores de sus clientes en sus suscripciones a CSP correspondientes (suscripciones de inquilino).
>
>

## <a name="before-you-start"></a>Antes de comenzar
Asegúrese de que tiene preparados estos requisitos previos:

* Una cuenta de [Microsoft Azure](https://azure.microsoft.com/) . Puede comenzar con una [evaluación gratuita](https://azure.microsoft.com/pricing/free-trial/). También puede leer sobre los precios de [Azure Site Recovery Manager](https://azure.microsoft.com/pricing/details/site-recovery/).
* Azure PowerShell. Para obtener información acerca de esta versión y cómo instalarla, consulte [Instalar Azure PowerShell](/powershell/azure/install-az-ps).

Además, en el ejemplo específico que se describe en este artículo verá que necesita cumplir los siguientes requisitos previos:

* Un host de Hyper-V que ejecute Windows Server 2012 R2 o Microsoft Hyper-V Server 2012 R2 que contenga una o varias máquinas virtuales. Los servidores de Hyper-V deben estar conectados a Internet, directamente o a través de un proxy.
* Las máquinas virtuales que quiera replicar deben cumplir con [estos requisitos previos](hyper-v-azure-support-matrix.md#replicated-vms).

## <a name="step-1-sign-in-to-your-azure-account"></a>Paso 1: Inicio de sesión en la cuenta de Azure.

1. Abra una consola de PowerShell y ejecute este comando para iniciar sesión en la cuenta de Azure. El cmdlet abrirá una página web que le solicitará las credenciales de la cuenta: **Connect-AzAccount**.
    - Como alternativa, puede incluir sus credenciales de cuenta como un parámetro en el cmdlet **Connect-AzAccount** mediante el parámetro **-Credential**.
    - Si usted es un asociado CSP que trabaja en nombre de un inquilino, especifique el cliente como inquilino usando su TenantID o su nombre de dominio principal de inquilino. Por ejemplo:  **Connect-AzAccount -Tenant "fabrikam.com"**
2. Ya que una cuenta puede tener varias suscripciones, le recomendamos que asocie la suscripción que quiera usar a esa cuenta:

    `Select-AzSubscription -SubscriptionName $SubscriptionName`

3. Compruebe que la suscripción esté registrada para usar los proveedores de Azure para Recovery Services y Site Recovery mediante los comandos siguientes:

    `Get-AzResourceProvider -ProviderNamespace  Microsoft.RecoveryServices`

4. Si **RegistrationState** está establecido en el valor **Registrado** en la salida de los comandos, puede continuar con el paso 2. Si no es así, debe registrar al proveedor que falta en su suscripción mediante estos comandos:

    `Register-AzResourceProvider -ProviderNamespace Microsoft.RecoveryServices`

5. Compruebe que los proveedores se registraron correctamente mediante los siguientes comandos:

    `Get-AzResourceProvider -ProviderNamespace  Microsoft.RecoveryServices`

## <a name="step-2-set-up-the-vault"></a>Paso 2: configuración del almacén

1. Cree un grupo de recursos de Azure Resource Manager en el que pueda crear el almacén, o bien use un grupo de recursos existente. Cree un nuevo grupo de recursos tal y como se indica a continuación. La variable $ResourceGroupName contiene el nombre del grupo de recursos que quiere crear y la variable $Geo contiene la región de Azure en la que se va a crear el grupo de recursos (por ejemplo: "Sur de Brasil").

    `New-AzResourceGroup -Name $ResourceGroupName -Location $Geo`

2. Para obtener una lista de los grupos de recursos de la suscripción, use el cmdlet **Get-AzResourceGroup**.
2. Cree un nuevo almacén de Azure Recovery Services como sigue:

        $vault = New-AzRecoveryServicesVault -Name <string> -ResourceGroupName <string> -Location <string>

    Puede recuperar una lista de los almacenes existentes mediante el cmdlet **Get-AzRecoveryServicesVault**.


## <a name="step-3-set-the-recovery-services-vault-context"></a>Paso 3: configuración del contexto de almacén de Recovery Services

Establezca el contexto de almacén de la manera siguiente:

`Set-AsrVaultSettings -Vault $vault`

## <a name="step-4-create-a-hyper-v-site"></a>Paso 4: creación de un sitio Hyper-V

1. Cree un nuevo sitio de Hyper-V como sigue:

        $sitename = "MySite"                #Specify site friendly name
        New-AsrFabric -Type HyperVSite -Name $sitename

2. Este cmdlet inicia un trabajo de Site Recovery para crear el sitio y devuelve un objeto de trabajo de Site Recovery. Espere a que el trabajo se complete y compruebe que se ha hecho correctamente.
3. Para recuperar el objeto de trabajo y comprobar el estado actual del trabajo, use el **cmdlet Get-AsrJob**.
4. Genere y descargue una clave de registro para el sitio; para ello, proceda como sigue:

    ```
    $SiteIdentifier = Get-AsrFabric -Name $sitename | Select -ExpandProperty SiteIdentifier
    $path = Get-AzRecoveryServicesVaultSettingsFile -Vault $vault -SiteIdentifier $SiteIdentifier -SiteFriendlyName $sitename
    ```

5. Copie la clave descargada en el host de Hyper-V. Necesita la clave para registrar el host de Hyper-V en el sitio

## <a name="step-5-install-the-provider-and-agent"></a>Paso 5: Instalación del proveedor y el agente

1. Descargue el instalador de la última versión del proveedor de [Microsoft](https://aka.ms/downloaddra).
2. Ejecute el instalador en el host de Hyper-V.
3. Al final de la instalación, vaya al paso que describe el registro.
4. Cuando se le solicite, proporcione la clave descargada y complete el registro del host de Hyper-V.
5. Compruebe que el host de Hyper-V se registró en el sitio tal como se indica a continuación:

        $server =  Get-AsrFabric -Name $siteName | Get-AsrServicesProvider -FriendlyName $server-friendlyname

Si ejecuta un servidor central de Hyper-V, descargue el archivo de instalación y haga lo siguiente:
1. Extraiga los archivos de AzureSiteRecoveryProvider.exe a un directorio local mediante este comando: ```AzureSiteRecoveryProvider.exe /x:. /q```
2. Ejecute ```.\setupdr.exe /i```. Los resultados se registran en %Programdata%\ASRLogs\DRASetupWizard.log.

3. Registre el servidor mediante este comando:

    ```cd  C:\Program Files\Microsoft Azure Site Recovery Provider\DRConfigurator.exe" /r /Friendlyname "FriendlyName of the Server" /Credentials "path to where the credential file is saved"```


## <a name="step-6-create-a-replication-policy"></a>Paso 6: Creación de una directiva de replicación

Antes de comenzar, recuerde que la cuenta de almacenamiento especificada debe estar en la misma región de Azure que el almacén y que debe tener habilitada la replicación geográfica.

1. Cree una directiva de replicación como sigue:

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                    #specify the number of recovery points
        $storageaccountID = Get-AzStorageAccount -Name "mystorea" -ResourceGroupName "MyRG" | Select -ExpandProperty Id

        $PolicyResult = New-AsrPolicy -Name $PolicyName -ReplicationProvider “HyperVReplicaAzure” -ReplicationFrequencyInSeconds $ReplicationFrequencyInSeconds  -RecoveryPoints $Recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId $storageaccountID

2. Compruebe el trabajo devuelto para asegurarse de que la creación de la directiva de replicación se realiza correctamente.

3. Obtenga el contenedor de protección correspondiente al sitio; para ello, proceda como sigue:

        $protectionContainer = Get-AsrProtectionContainer
3. Asocie el contenedor de protección con la directiva de replicación tal como se indica:

        $Policy = Get-AsrPolicy -FriendlyName $PolicyName
        $associationJob  = New-AsrProtectionContainerMapping -Name $mappingName -Policy $Policy -PrimaryProtectionContainer $protectionContainer[0]
4. Espere a que finalice el trabajo de asociación.

5. Recupere las asignaciones de los contenedores de protección.

        $ProtectionContainerMapping = Get-ASRProtectionContainerMapping -ProtectionContainer $protectionContainer

## <a name="step-7-enable-vm-protection"></a>Paso 7: habilitación de la protección de máquina virtual

1. Recupere el elemento protegible correspondiente a la máquina virtual que quiere proteger, tal y como se indica a continuación:

        $VMFriendlyName = "Fabrikam-app"                    #Name of the VM
        $ProtectableItem = Get-AsrProtectableItem -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName
2. Proteja la máquina virtual. Si la máquina virtual que se protege tiene más de un disco conectado, especifique el disco del sistema operativo con el parámetro *OSDiskName* .

        $OSType = "Windows"                                 # "Windows" or "Linux"
        $DRjob = New-AsrReplicationProtectedItem -ProtectableItem $VM -Name $VM.Name -ProtectionContainerMapping $ProtectionContainerMapping -RecoveryAzureStorageAccountId $StorageAccountID -OSDiskName $OSDiskNameList[$i] -OS $OSType -RecoveryResourceGroupId $ResourceGroupID

3. Espere a que las máquinas virtuales lleguen a un estado protegido después de la replicación inicial. Esta operación puede tardar unos minutos en función de factores, como el volumen de datos que se van a replicar y el ancho de banda del canal de subida disponible en Azure. Cuando se alcanza un estado protegido, los valores State y StateDescription del trabajo se actualizan tal como se indica a continuación:

        PS C:\> $DRjob = Get-AsrJob -Job $DRjob

        PS C:\> $DRjob | Select-Object -ExpandProperty State
        Succeeded

        PS C:\> $DRjob | Select-Object -ExpandProperty StateDescription
        Completed
4. Actualice las propiedades de recuperación (como el tamaño de rol de la máquina virtual) y la red de Azure a la cual se asociarán el adaptador de red de la máquina virtual tras la conmutación por error.

        PS C:\> $nw1 = Get-AzVirtualNetwork -Name "FailoverNw" -ResourceGroupName "MyRG"

        PS C:\> $VMFriendlyName = "Fabrikam-App"

        PS C:\> $rpi = Get-AsrReplicationProtectedItem -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

        PS C:\> $UpdateJob = Set-AsrReplicationProtectedItem --InputObject $rpi -PrimaryNic $VM.NicDetailsList[0].NicId -RecoveryNetworkId $nw1.Id -RecoveryNicSubnetName $nw1.Subnets[0].Name

        PS C:\> $UpdateJob = Get-AsrJob -Job $UpdateJob

        PS C:\> $UpdateJob| select -ExpandProperty state
        Get-AsrJob -Job $job | select -ExpandProperty state

        Succeeded



## <a name="step-8-run-a-test-failover"></a>Paso 8: Ejecución de una conmutación por error de prueba
1. Ejecute la conmutación por error de prueba de la manera siguiente:

        $nw = Get-AzVirtualNetwork -Name "TestFailoverNw" -ResourceGroupName "MyRG" #Specify Azure vnet name and resource group

        $rpi = Get-AsrReplicationProtectedItem -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

        $TFjob =Start-AsrTestFailoverJob -ReplicationProtectedItem $VM -Direction PrimaryToRecovery -AzureVMNetworkId $nw.Id
2. Compruebe que la prueba de la máquina virtual se crea en Azure. El trabajo de conmutación por error de prueba se suspende después de crear la máquina virtual de prueba en Azure.
3. Para limpiar y completar la conmutación por error de prueba, ejecute:

        $TFjob = Start-AsrTestFailoverCleanupJob -ReplicationProtectedItem $rpi -Comment "TFO done"

## <a name="next-steps"></a>Pasos siguientes
[Más información](https://docs.microsoft.com/powershell/module/az.recoveryservices) sobre Azure Site Recovery con los cmdlets de PowerShell de Azure Resource Manager.
