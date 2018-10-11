---
title: Replicación y conmutación por error de máquinas virtuales de VMware a Azure mediante PowerShell en Azure Site Recovery | Microsoft Docs
description: Aprenda a configurar la replicación y la conmutación por error a Azure de máquinas virtuales de VMware mediante PowerShell en Azure Site Recovery.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.date: 07/06/2018
ms.topic: conceptual
ms.author: sutalasi
ms.openlocfilehash: 070e51b132828e4fdf597d28fc2ad602adf76692
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2018
ms.locfileid: "48042809"
---
# <a name="replicate-and-fail-over-vmware-vms-to-azure-with-powershell"></a>Replicación y conmutación por error de máquinas virtuales de VMware a Azure con PowerShell

En este artículo, verá cómo replicar y conmutar por error máquinas virtuales de VMware en Azure con Azure PowerShell. 

Aprenderá a:

> [!div class="checklist"]
> - Crear un almacén de Recovery Services y a establecer el contexto de este.
> - Validar el registro de servidor en el almacén.
> - Configurar la replicación, incluida una directiva de replicación. Agregar una instancia de vCenter Server y detectar máquinas virtuales. Agregar una instancia de vCenter Server y detectar 
> - Crear cuentas de almacenamiento para almacenar los datos de replicación y replicar las máquinas virtuales.
> - Realizar una conmutación por error. Configurar los valores de la conmutación por error para la replicación de máquinas virtuales.

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar:

- Asegúrese de entender la [arquitectura y los componentes del escenario](vmware-azure-architecture.md).
- Revise los [requisitos de compatibilidad](site-recovery-support-matrix-to-azure.md) de todos los componentes.
- Tiene la versión 5.0.1 o posterior del módulo AzureRm PowerShell. Si necesita instalar o actualizar Azure PowerShell, siga la guía [Cómo instalar y configurar Azure PowerShell](/powershell/azureps-cmdlets-docs).

## <a name="log-into-azure"></a>Inicio de sesión en Azure

Inicie sesión en su suscripción de Azure con el cmdlet Connect-AzureRmAccount:

```azurepowershell
Connect-AzureRmAccount
```
Seleccione la suscripción de Azure en la que quiere replicar sus máquinas virtuales de VMware. Use el cmdlet Get-AzureRmSubscription para obtener la lista de suscripciones de Azure a las que tiene acceso. Seleccione la suscripción de Azure con la que quiere trabajar mediante el cmdlet Select-AzureRmSubscription.

```azurepowershell
Select-AzureRmSubscription -SubscriptionName "ASR Test Subscription"
```
## <a name="set-up-a-recovery-services-vault"></a>Configuración del almacén de Recovery Services

1. Cree el grupo de recursos en el que se creará el almacén de Recovery Services. En el ejemplo siguiente, el grupo de recursos se denomina VMwareDRtoAzurePS y se crea en la región de Asia Oriental.

   ```azurepowershell
   New-AzureRmResourceGroup -Name "VMwareDRtoAzurePS" -Location "East Asia"
   ```
   ```
   ResourceGroupName : VMwareDRtoAzurePS
   Location          : eastasia
   ProvisioningState : Succeeded
   Tags              :
   ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VMwareDRtoAzurePS
```
   
2. Cree un almacén de Recovery Services. En el ejemplo siguiente, el almacén de Recovery Services se denomina VMwareDRToAzurePs, y se crea en la región de Asia Oriental y en el grupo de recursos creado en el paso anterior.

   ```azurepowershell
   New-AzureRmRecoveryServicesVault -Name "VMwareDRToAzurePs" -Location "East Asia" -ResourceGroupName "VMwareDRToAzurePs"
   ```
   ```
   Name              : VMwareDRToAzurePs
   ID                : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VMwareDRToAzurePs/providers/Microsoft.RecoveryServices/vaults/VMwareDRToAzurePs
   Type              : Microsoft.RecoveryServices/vaults
   Location          : eastasia
   ResourceGroupName : VMwareDRToAzurePs
   SubscriptionId    : xxxxxxxx-xxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
   Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
   ``` 

3. Descargue la clave de registro del almacén. La clave de registro del almacén se usa para registrar el servidor de configuración local en este almacén. El registro forma parte del proceso de instalación de software del servidor de configuración.

   ```azurepowershell
   #Get the vault object by name and resource group and save it to the $vault PowerShell variable 
   $vault = Get-AzureRmRecoveryServicesVault -Name "VMwareDRToAzurePS" -ResourceGroupName "VMwareDRToAzurePS"

   #Download vault registration key to the path C:\Work
   Get-AzureRmRecoveryServicesVaultSettingsFile -SiteRecovery -Vault $Vault -Path "C:\Work\"
   ```
   ```
   FilePath
   --------
   C:\Work\VMwareDRToAzurePs_2017-11-23T19-52-34.VaultCredentials
   ```

4. Use la clave de registro del almacén descargado y siga los pasos que se describen en los artículos que se indican a continuación para completar la instalación y el registro del servidor de configuración.
   - [Selección de los objetivos de protección](vmware-azure-set-up-source.md#choose-your-protection-goals).
   - [Configuración del entorno de origen](vmware-azure-set-up-source.md#set-up-the-configuration-server) 

### <a name="set-the-vault-context"></a>Establecer el contexto de almacén

Establezca el contexto de almacén mediante el cmdlet Set-ASRVaultContext. Una vez establecido, las operaciones posteriores de Azure Site Recovery de la sesión de PowerShell se realizan en el contexto de almacén seleccionado.

> [!TIP]
> El módulo PowerShell de Azure Site Recovery (módulo AzureRm.RecoveryServices.SiteRecovery) viene con alias fáciles de usar para la mayoría de los cmdlets. Los cmdlets del módulo adoptan la forma *\<Operación>-**AzureRmRecoveryServicesAsr**\<Objeto>* y tienen alias equivalentes que adoptan la forma *\<Operación>-**ASR**\<Objeto>*. En este artículo se usan alias de cmdlet para facilitar la lectura.

En el ejemplo siguiente, los detalles del almacén desde la variable $vault se usan para especificar el contexto de almacén para la sesión de PowerShell.

   ```azurepowershell
   Set-ASRVaultContext -Vault $vault
   ```
   ```
   ResourceName      ResourceGroupName ResourceNamespace          ResouceType
   ------------      ----------------- -----------------          -----------
   VMwareDRToAzurePs VMwareDRToAzurePs Microsoft.RecoveryServices vaults
   ```

Como alternativa al cmdlet Set-ASRVaultContext, también puede usarse el cmdlet Import-AzureRmRecoveryServicesAsrVaultSettingsFile para establecer el contexto de almacén. Especifique la ruta de acceso donde se encuentra el archivo de clave de registro del almacén como el parámetro -path al cmdlet Import-AzureRmRecoveryServicesAsrVaultSettingsFile. Por ejemplo: 

   ```azurepowershell
   Get-AzureRmRecoveryServicesVaultSettingsFile -SiteRecovery -Vault $Vault -Path "C:\Work\"
   Import-AzureRmRecoveryServicesAsrVaultSettingsFile -Path "C:\Work\VMwareDRToAzurePs_2017-11-23T19-52-34.VaultCredentials"
   ```
En las secciones siguientes de este artículo se supone que se ha establecido el contexto de almacén para las operaciones de Azure Site Recovery.

## <a name="validate-vault-registration"></a>Validación del registro del almacén

En este ejemplo, tenemos lo siguiente:

- Un servidor de configuración (**ConfigurationServer**) que se ha registrado en este almacén.
- Un servidor de procesos adicional (**ScaleOut-ProcessServer**) que se ha registrado en *ConfigurationServer*.
- Varias cuentas (**vCenter_account**, **WindowsAccount** y **LinuxAccount**) que se han configurado en el servidor de configuración. Estas cuentas se usan para agregar el servidor de vCenter para detectar máquinas virtuales, así como para la instalación de inserción del software Mobility Service en los servidores de Windows y Linux que van a replicarse.

1. Los servidores de configuración registrados se representan mediante un objeto de tejido en Site Recovery. Obtenga la lista de objetos de tejido del almacén e identifique el servidor de configuración.

   ```azurepowershell
   # Verify that the Configuration server is successfully registered to the vault
   $ASRFabrics = Get-ASRFabric
   $ASRFabrics.count
   ```
   ```
   1
   ```
   ```azurepowershell
   #Print details of the Configuration Server
   $ASRFabrics[0]
   ```
   ```
   Name                  : 2c33d710a5ee6af753413e97f01e314fc75938ea4e9ac7bafbf4a31f6804460d
   FriendlyName          : ConfigurationServer
   ID                    : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VMwareDRToAzurePs/providers/Microsoft.RecoveryServices/vaults/VMwareDRToAzurePs/replicationFabrics
                           /2c33d710a5ee6af753413e97f01e314fc75938ea4e9ac7bafbf4a31f6804460d
   Type                  : Microsoft.RecoveryServices/vaults/replicationFabrics
   FabricType            : VMware
   SiteIdentifier        : ef7a1580-f356-4a00-aa30-7bf80f952510
   FabricSpecificDetails : Microsoft.Azure.Commands.RecoveryServices.SiteRecovery.ASRVMWareSpecificDetails
   ```

2. Identifique los servidores de procesos que pueden usarse para replicar máquinas.

   ```azurepowershell
   $ProcessServers = $ASRFabrics[0].FabricSpecificDetails.ProcessServers
   for($i=0; $i -lt $ProcessServers.count; $i++) {
    "{0,-5} {1}" -f $i, $ProcessServers[$i].FriendlyName
   }
   ```
   ```
   0     ScaleOut-ProcessServer
   1     ConfigurationServer
   ```

   De la salida anterior, ***$ProcessServers[0]*** corresponde a *ScaleOut-ProcessServer* y ***$ProcessServers[1]*** corresponde a la función de servidor de procesos en *ConfigurationServer*.

3. Identifique las cuentas que se han configurado en el servidor de configuración.

   ```azurepowershell
   $AccountHandles = $ASRFabrics[0].FabricSpecificDetails.RunAsAccounts
   #Print the account details
   $AccountHandles
   ```
   ```
   AccountId AccountName
   --------- -----------
   1         vCenter_account
   2         WindowsAccount
   3         LinuxAccount
   ```

   De la salida anterior, ***$AccountHandles[0]*** corresponde a la cuenta *vCenter_account*, ***$AccountHandles[1]*** a la cuenta *WindowsAccount* y ***$AccountHandles[2]*** a la cuenta *LinuxAccount*.

## <a name="create-a-replication-policy"></a>Creación de una directiva de replicación

En este paso, se crean dos directivas de replicación. Una directiva para replicar máquinas virtuales de VMware en Azure y otra para replicar máquinas virtuales conmutadas por error que se ejecutan en Azure de nuevo en el sitio de VMware local.

> [!NOTE]
> La mayoría de las operaciones de Azure Site Recovery se ejecutan de forma asincrónica. Cuando se inicia una operación, se envía un trabajo de Azure Site Recovery y se devuelve un objeto de seguimiento de trabajo. Este objeto de seguimiento de trabajo puede utilizarse para supervisar el estado de la operación.

1. Cree una directiva de replicación denominada *ReplicationPolicy* para replicar máquinas virtuales de VMware en Azure con las propiedades especificadas.

   ```azurepowershell
   $Job_PolicyCreate = New-ASRPolicy -VMwareToAzure -Name "ReplicationPolicy" -RecoveryPointRetentionInHours 24 -ApplicationConsistentSnapshotFrequencyInHours 4 -RPOWarningThresholdInMinutes 60

   # Track Job status to check for completion
   while (($Job_PolicyCreate.State -eq "InProgress") -or ($Job_PolicyCreate.State -eq "NotStarted")){ 
           sleep 10; 
           $Job_PolicyCreate = Get-ASRJob -Job $Job_PolicyCreate
   }

   #Display job status
   $Job_PolicyCreate
   ```
   ```
   Name             : 8d18e2d9-479f-430d-b76b-6bc7eb2d0b3e
   ID               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VMwareDRToAzurePs/providers/Microsoft.RecoveryServices/vaults/VMwareDRToAzurePs/replicationJobs/8d18e2d
                      9-479f-430d-b76b-6bc7eb2d0b3e
   Type             :
   JobType          : AddProtectionProfile
   DisplayName      : Create replication policy
   ClientRequestId  : a162b233-55d7-4852-abac-3d595a1faac2 ActivityId: 9895234a-90ea-4c1a-83b5-1f2c6586252a
   State            : Succeeded
   StateDescription : Completed
   StartTime        : 11/24/2017 2:49:24 AM
   EndTime          : 11/24/2017 2:49:23 AM
   TargetObjectId   : ab31026e-4866-5440-969a-8ebcb13a372f
   TargetObjectType : ProtectionProfile
   TargetObjectName : ReplicationPolicy
   AllowedActions   :
   Tasks            : {Prerequisites check for creating the replication policy, Creating the replication policy}
   Errors           : {}
   ```

2. Cree una directiva de replicación para usarla en la conmutación por recuperación de Azure en el sitio de VMware local.

   ```azurepowershell
   $Job_FailbackPolicyCreate = New-ASRPolicy -AzureToVMware -Name "ReplicationPolicy-Failback" -RecoveryPointRetentionInHours 24 -ApplicationConsistentSnapshotFrequencyInHours 4 -RPOWarningThresholdInMinutes 60
   ```

   Utilice los detalles del trabajo de *$Job_FailbackPolicyCreate* para realizar el seguimiento de la operación hasta su finalización.

   * Cree una asignación de contenedor de protección para asignar directivas de replicación con el servidor de configuración.

   ```azurepowershell
   #Get the protection container corresponding to the Configuration Server
   $ProtectionContainer = Get-ASRProtectionContainer -Fabric $ASRFabrics[0]

   #Get the replication policies to map by name.
   $ReplicationPolicy = Get-ASRPolicy -Name "ReplicationPolicy"
   $FailbackReplicationPolicy = Get-ASRPolicy -Name "ReplicationPolicy-Failback"

   # Associate the replication policies to the protection container corresponding to the Configuration Server. 

   $Job_AssociatePolicy = New-ASRProtectionContainerMapping -Name "PolicyAssociation1" -PrimaryProtectionContainer $ProtectionContainer -Policy $ReplicationPolicy

   # Check the job status
   while (($Job_AssociatePolicy.State -eq "InProgress") -or ($Job_AssociatePolicy.State -eq "NotStarted")){ 
           sleep 10; 
           $Job_AssociatePolicy = Get-ASRJob -Job $Job_AssociatePolicy
   }
   $Job_AssociatePolicy.State

   <# In the protection container mapping used for failback (replicating failed over virtual machines 
      running in Azure, to the primary VMware site.) the protection container corresponding to the 
      Configuration server acts as both the Primary protection container and the recovery protection
      container
   #>
    $Job_AssociateFailbackPolicy = New-ASRProtectionContainerMapping -Name "FailbackPolicyAssociation" -PrimaryProtectionContainer $ProtectionContainer -RecoveryProtectionContainer $ProtectionContainer -Policy $FailbackReplicationPolicy

   # Check the job status
   while (($Job_AssociateFailbackPolicy.State -eq "InProgress") -or ($Job_AssociateFailbackPolicy.State -eq "NotStarted")){ 
           sleep 10; 
           $Job_AssociateFailbackPolicy = Get-ASRJob -Job $Job_AssociateFailbackPolicy
   }
   $Job_AssociateFailbackPolicy.State

   ```

## <a name="add-a-vcenter-server-and-discover-vms"></a>Adición de una instancia de vCenter Server y detección de máquinas virtuales

Agregue una instancia de vCenter Server por dirección IP o nombre de host. El parámetro **-port** especifica el puerto en vCenter Server con el que se establecerá la conexión, el parámetro **-Name** especifica un nombre descriptivo que se usará para vCenter Server y el parámetro **-Account** especifica el identificador de cuenta en el servidor de configuración que se usará para detectar máquinas virtuales administradas por vCenter Server.

```azurepowershell
# The $AccountHandles[0] variable holds details of vCenter_account

$Job_AddvCenterServer = New-ASRvCenter -Fabric $ASRFabrics[0] -Name "MyvCenterServer" -IpOrHostName "10.150.24.63" -Account $AccountHandles[0] -Port 443

#Wait for the job to complete and ensure it completed successfully

while (($Job_AddvCenterServer.State -eq "InProgress") -or ($Job_AddvCenterServer.State -eq "NotStarted")) { 
        sleep 30; 
        $Job_AddvCenterServer = Get-ASRJob -Job $Job_AddvCenterServer
}
$Job_AddvCenterServer
```
```
Name             : 0f76f937-f9cf-4e0e-bf27-10c9d1c252a4
ID               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VMwareDRToAzurePs/providers/Microsoft.RecoveryServices/vaults/VMwareDRToAzurePs/replicationJobs/0f76f93
                   7-f9cf-4e0e-bf27-10c9d1c252a4
Type             :
JobType          : DiscoverVCenter
DisplayName      : Add vCenter server
ClientRequestId  : a2af8892-5686-4d64-a528-10445bc2f698 ActivityId: 7ec05aad-002e-4da0-991f-95d0de7a9f3a
State            : Succeeded
StateDescription : Completed
StartTime        : 11/24/2017 2:41:47 AM
EndTime          : 11/24/2017 2:44:37 AM
TargetObjectId   : 10.150.24.63
TargetObjectType : VCenter
TargetObjectName : MyvCenterServer
AllowedActions   :
Tasks            : {Adding vCenter server}
Errors           : {}
```

## <a name="create-storage-accounts-for-replication"></a>Creación de cuentas de almacenamiento para la replicación

En este paso, se crean las cuentas de almacenamiento que se usarán para la replicación. Estas cuentas de almacenamiento se usarán posteriormente para replicar máquinas virtuales. Asegúrese de que las cuentas de almacenamiento se creen en la misma región de Azure que el almacén. Puede omitir este paso si tiene previsto usar una cuenta de almacenamiento existente para la replicación.

> [!NOTE]
> Al replicar máquinas virtuales locales en una cuenta de almacenamiento Premium, debe especificar una cuenta de almacenamiento estándar adicional (cuenta de almacenamiento de registros). La cuenta de almacenamiento de registros contiene registros de replicación como un almacén intermedio hasta que se pueden aplicar los registros en el destino de almacenamiento Premium.
>

```azurepowershell

$PremiumStorageAccount = New-AzureRmStorageAccount -ResourceGroupName "VMwareDRToAzurePs" -Name "premiumstorageaccount1" -Location "East Asia" -SkuName Premium_LRS

$LogStorageAccount = New-AzureRmStorageAccount -ResourceGroupName "VMwareDRToAzurePs" -Name "logstorageaccount1" -Location "East Asia" -SkuName Standard_LRS

$ReplicationStdStorageAccount= New-AzureRmStorageAccount -ResourceGroupName "VMwareDRToAzurePs" -Name "replicationstdstorageaccount1" -Location "East Asia" -SkuName Standard_LRS
```

## <a name="replicate-vmware-vms"></a>Replicación de máquinas virtuales de VMware

Las máquinas virtuales tardan aproximadamente de 15 a 20 minutos en detectarse desde vCenter Server. Una vez detectado, se crea un objeto de elemento que se puede proteger en Azure Site Recovery para cada máquina virtual detectada. En este paso, tres de las máquinas virtuales detectadas se han replicado en las cuentas de Azure Storage que creó en el paso anterior.

Necesitará los siguientes detalles para proteger una máquina virtual detectada:

* El elemento que se puede proteger y que se va a replicar.
* La cuenta de almacenamiento en la que se va a replicar la máquina virtual. Además, se necesita un almacenamiento de registros para proteger las máquinas virtuales en una cuenta de almacenamiento Premium.
* El servidor de procesos que se usará para la replicación. La lista de servidores de procesos disponibles se ha recuperado y guardado en las variables ***$ProcessServers[0]***  *(ScaleOut-ProcessServer)* y ***$ProcessServers[1]*** *(ConfigurationServer)*.
* La cuenta que se usará para la instalación de inserción el software Mobility Service en las máquinas. La lista de cuentas disponibles se ha recuperado y almacenado en la variable ***$AccountHandles***.
* La asignación de contenedor de protección para la directiva de replicación que se usará para la replicación.
* El grupo de recursos en el que las máquinas virtuales deben crearse en la conmutación por error.
* Opcionalmente, la red virtual de Azure y la subred a la cual debe conectarse la máquina virtual conmutada por error.

Ahora, replique las siguientes máquinas virtuales mediante la configuración especificada en esta tabla:


|Máquina virtual  |Servidor de procesos        |Cuenta de almacenamiento              |Cuenta de almacenamiento del registro  |Directiva           |Cuenta para la instalación de Mobility Service|Grupo de recursos de destino  | Red virtual de destino  |Subred de destino  |
|-----------------|----------------------|-----------------------------|---------------------|-----------------|-----------------------------------------|-----------------------|-------------------------|---------------|
|Win2K12VM1       |ScaleOut-ProcessServer|premiumstorageaccount1       |logstorageaccount1   |ReplicationPolicy|WindowsAccount                           |VMwareDRToAzurePs      |ASR-vnet                 |Subnet-1       |
|CentOSVM1       |ConfigurationServer   |replicationstdstorageaccount1| N/D                 |ReplicationPolicy|LinuxAccount                             |VMwareDRToAzurePs      |ASR-vnet                 |Subnet-1       |   
|CentOSVM2       |ConfigurationServer   |replicationstdstorageaccount1| N/D                 |ReplicationPolicy|LinuxAccount                             |VMwareDRToAzurePs      |ASR-vnet                 |Subnet-1       |   

 
```azurepowershell

#Get the target resource group to be used
$ResourceGroup = Get-AzureRmResourceGroup -Name "VMwareToAzureDrPs"

#Get the target virtual network to be used
$RecoveryVnet = Get-AzureRmVirtualNetwork -Name "ASR-vnet" -ResourceGroupName "asrrg" 

#Get the protection container mapping for replication policy named ReplicationPolicy
$PolicyMap  = Get-ASRProtectionContainerMapping -ProtectionContainer $ProtectionContainer | where PolicyFriendlyName -eq "ReplicationPolicy"

#Get the protectable item corresponding to the virtual machine Win2K12VM1
$VM1 = Get-ASRProtectableItem -ProtectionContainer $ProtectionContainer -FriendlyName "Win2K12VM1"

# Enable replication for virtual machine Win2K12VM1
# The name specified for the replicated item needs to be unique within the protection container. Using a random GUID to ensure uniqueness
$Job_EnableRepication1 = New-ASRReplicationProtectedItem -VMwareToAzure -ProtectableItem $VM1 -Name (New-Guid).Guid -ProtectionContainerMapping $PolicyMap -RecoveryAzureStorageAccountId $PremiumStorageAccount.Id -LogStorageAccountId $LogStorageAccount.Id -ProcessServer $ProcessServers[0] -Account $AccountHandles[1] -RecoveryResourceGroupId $ResourceGroup.ResourceId -RecoveryAzureNetworkId $RecoveryVnet.Id -RecoveryAzureSubnetName "Subnet-1" 

#Get the protectable item corresponding to the virtual machine CentOSVM1
$VM2 = Get-ASRProtectableItem -ProtectionContainer $ProtectionContainer -FriendlyName "CentOSVM1"

# Enable replication for virtual machine CentOSVM1
$Job_EnableRepication2 = New-ASRReplicationProtectedItem -VMwareToAzure -ProtectableItem $VM2 -Name (New-Guid).Guid -ProtectionContainerMapping $PolicyMap -RecoveryAzureStorageAccountId $ReplicationStdStorageAccount.Id  -ProcessServer $ProcessServers[1] -Account $AccountHandles[2] -RecoveryResourceGroupId $ResourceGroup.ResourceId -RecoveryAzureNetworkId $RecoveryVnet.Id -RecoveryAzureSubnetName "Subnet-1"

#Get the protectable item corresponding to the virtual machine CentOSVM2
$VM3 = Get-ASRProtectableItem -ProtectionContainer $ProtectionContainer -FriendlyName "CentOSVM2"

# Enable replication for virtual machine CentOSVM2
$Job_EnableRepication3 = New-ASRReplicationProtectedItem -VMwareToAzure -ProtectableItem $VM3 -Name (New-Guid).Guid -ProtectionContainerMapping $PolicyMap -RecoveryAzureStorageAccountId $ReplicationStdStorageAccount.Id  -ProcessServer $ProcessServers[1] -Account $AccountHandles[2] -RecoveryResourceGroupId $ResourceGroup.ResourceId -RecoveryAzureNetworkId $RecoveryVnet.Id -RecoveryAzureSubnetName "Subnet-1" 

```

Cuando el trabajo de habilitación de replicación se completa correctamente, se inicia la replicación inicial para las máquinas virtuales. La replicación inicial puede tardar cierto tiempo según la cantidad de datos que se repliquen y el ancho de banda disponible para la replicación. Cuando se completa la replicación inicial, la máquina virtual pasa a un estado protegido. Una vez que la máquina virtual alcanza un estado protegido, puede realizar una conmutación por error de prueba para la máquina virtual, agregarla a planes de recuperación, etc.

Puede comprobar el estado de replicación y el mantenimiento de replicación de la máquina virtual con el cmdlet Get-ASRReplicationProtectedItem.

```azurepowershell
Get-ASRReplicationProtectedItem -ProtectionContainer $ProtectionContainer | Select FriendlyName, ProtectionState, ReplicationHealth
```
```
FriendlyName ProtectionState                 ReplicationHealth
------------ ---------------                 -----------------
CentOSVM1    Protected                       Normal
CentOSVM2    InitialReplicationInProgress    Normal
Win2K12VM1   Protected                       Normal
```

## <a name="configure-failover-settings"></a>Configurar los valores de la conmutación por error

La configuración de la conmutación por error de máquinas protegidas puede actualizarse mediante el cmdlet Set-ASRReplicationProtectedItem. Algunas de las opciones que se pueden actualizar a través de este cmdlet son:
* Nombre de la máquina virtual que se creará en la conmutación por error.
* Tamaño de la máquina virtual que se creará en la conmutación por error.
* Red virtual de Azure y subred a las que las NIC de la máquina virtual deberían conectarse en la conmutación por error.
* Conmutación por error en discos administrados.
* Aplicación de la ventaja de uso híbrido de Azure.
* Asignación de una dirección IP estática desde la red virtual de destino que se asignará a la máquina virtual en la conmutación por error.

En este ejemplo, actualizamos el tamaño de máquina virtual que se creará en la conmutación por error de la máquina virtual *Win2K12VM1* y especificamos que la máquina virtual usa discos administrados en la conmutación por error.

```azurepowershell
$ReplicatedVM1 = Get-ASRReplicationProtectedItem -FriendlyName "Win2K12VM1" -ProtectionContainer $ProtectionContainer

Set-ASRReplicationProtectedItem -InputObject $ReplicatedVM1 -Size "Standard_DS11" -UseManagedDisk True
```
```
Name             : cafa459c-44a7-45b0-9de9-3d925b0e7db9
ID               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VMwareDRToAzurePs/providers/Microsoft.RecoveryServices/vaults/VMwareDRToAzurePs/replicationJobs/cafa459
                   c-44a7-45b0-9de9-3d925b0e7db9
Type             :
JobType          : UpdateVmProperties
DisplayName      : Update the virtual machine
ClientRequestId  : b0b51b2a-f151-4e9a-a98e-064a5b5131f3 ActivityId: ac2ba316-be7b-4c94-a053-5363f683d38f
State            : InProgress
StateDescription : InProgress
StartTime        : 11/24/2017 2:04:26 PM
EndTime          :
TargetObjectId   : 88bc391e-d091-11e7-9484-000c2955bb50
TargetObjectType : ProtectionEntity
TargetObjectName : Win2K12VM1
AllowedActions   :
Tasks            : {Update the virtual machine properties}
Errors           : {}
```

## <a name="run-a-test-failover"></a>Ejecución de una conmutación por error de prueba

1. Ejecute una exploración de la recuperación ante desastres (conmutación por error de prueba) de la manera siguiente:

   ```azurepowershell
   #Test failover of Win2K12VM1 to the test virtual network "V2TestNetwork"

   #Get details of the test failover virtual network to be used
   TestFailovervnet = Get-AzureRmVirtualNetwork -Name "V2TestNetwork" -ResourceGroupName "asrrg" 

   #Start the test failover operation
   $TFOJob = Start-ASRTestFailoverJob -ReplicationProtectedItem $ReplicatedVM1 -AzureVMNetworkId $TestFailovervnet.Id -Direction PrimaryToRecovery
   ```
2. Una vez que el trabajo de conmutación por error de prueba se completa correctamente, puede observar que la máquina virtual con el sufijo *"-Test"* (Win2K12VM1-Test en este caso) en su nombre se ha creado en Azure.
3. Ahora puede conectarse a la máquina virtual conmutada por error de prueba y validar la conmutación por error de prueba.
4. Limpie la conmutación por error de prueba mediante el cmdlet Start-ASRTestFailoverCleanupJob. Esta operación elimina la máquina virtual creada como parte de la operación de conmutación por error de prueba.

   ```azurepowershell
   $Job_TFOCleanup = Start-ASRTestFailoverCleanupJob -ReplicationProtectedItem $ReplicatedVM1
   ```

## <a name="fail-over-to-azure"></a>Conmutación por error a Azure

En este paso, se conmuta por error la máquina virtual Win2K12VM1 en un punto de recuperación concreto.

1. Obtenga una lista de los puntos de recuperación disponibles que se pueden usar para la conmutación por error:
   ```azurepowershell
   # Get the list of available recovery points for Win2K12VM1
   $RecoveryPoints = Get-ASRRecoveryPoint -ReplicationProtectedItem $ReplicatedVM1
   "{0} {1}" -f $RecoveryPoints[0].RecoveryPointType, $RecoveryPoints[0].RecoveryPointTime
   ```
   ```
   CrashConsistent 11/24/2017 5:28:25 PM
   ```
   ```azurepowershell

   #Start the failover job
   $Job_Failover = Start-ASRUnplannedFailoverJob -ReplicationProtectedItem $ReplicatedVM1 -Direction PrimaryToRecovery -RecoveryPoint $RecoveryPoints[0]
   do {
           $Job_Failover = Get-ASRJob -Job $Job_Failover;
           sleep 60;
   } while (($Job_Failover.State -eq "InProgress") -or ($JobFailover.State -eq "NotStarted"))
   $Job_Failover.State
   ```
   ```
   Succeeded
   ```

2. Después de realizar la conmutación por error correctamente, puede confirmar la operación y configurar la replicación inversa desde Azure nuevamente en el sitio de VMware local.

## <a name="next-steps"></a>Pasos siguientes
Aprenda a automatizar más tareas mediante la [referencia de PowerShell de Azure Site Recovery](https://docs.microsoft.com/powershell/module/AzureRM.RecoveryServices.SiteRecovery).
