---
title: Habilitar el cifrado de disco para los clústeres de Windows de Azure Service Fabric | Microsoft Docs
description: En este artículo se describe cómo habilitar el cifrado de disco para los nodos de clúster de Azure Service Fabric con Azure Key Vault en Azure Resource Manager.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: navya
ms.assetid: 15d0ab67-fc66-4108-8038-3584eeebabaa
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/22/2019
ms.author: aljo
ms.openlocfilehash: c31fc43729bcb58c755959db0c8bc5185b8197f4
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66471416"
---
# <a name="enable-disk-encryption-for-azure-service-fabric-cluster-nodes-in-windows"></a>Habilitar el cifrado de disco para los nodos de clúster de Azure Service Fabric en Windows 
> [!div class="op_single_selector"]
> * [Cifrado de disco para Windows](service-fabric-enable-azure-disk-encryption-windows.md)
> * [Cifrado de disco para Linux](service-fabric-enable-azure-disk-encryption-linux.md)
>
>

En este tutorial, obtendrá información sobre cómo habilitar el cifrado de disco en los nodos de clúster de Service Fabric en Windows. Deberá seguir estos pasos para cada uno de los tipos de nodos y conjuntos de escalado de máquinas virtuales. Para el cifrado de los nodos, vamos a usar la funcionalidad de Azure Disk Encryption en conjuntos de escalado de máquinas virtuales.

La guía trata los temas siguientes:

* Conceptos clave a tener en cuenta al habilitar el cifrado de disco en el escalado de máquinas virtuales del clúster de Service Fabric se establece en Windows.
* Los pasos que se deben seguir antes de habilitar el cifrado de disco en Service Fabric nodos del clúster en Windows.
* Pasos a seguir para habilitar el cifrado de disco en los nodos de clúster de Service Fabric en Windows.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Requisitos previos

**Registro automático** 

La versión preliminar de cifrado de disco para el conjunto de escalado de máquinas virtuales requiere el registro de autoservicio. Para ello, siga los pasos que se describen a continuación: 

1. En primer lugar, ejecute el siguiente comando:
    ```powershell
    Register-AzProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
    ```
2. Espere unos 10 minutos hasta que se lee el estado *registrado*. Puede comprobar el estado, ejecute el comando siguiente: 
    ```powershell
    Get-AzProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
    Register-AzResourceProvider -ProviderNamespace Microsoft.Compute
    ```
**Azure Key Vault** 

1. Crear un almacén de claves en la misma suscripción y región que el conjunto de escalado, a continuación, seleccione el **EnabledForDiskEncryption** directiva en el almacén de claves de acceso mediante el cmdlet de PowerShell. También puede establecer la directiva mediante el uso de la interfaz de usuario del almacén de claves en el portal de Azure con el siguiente comando:
    ```powershell
    Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -EnabledForDiskEncryption
    ```
2. Instale la versión más reciente de la [CLI de Azure](/cli/azure/install-azure-cli), que contiene los nuevos comandos de cifrado.
3. Instale la versión más reciente de la [SDK de Azure desde Azure PowerShell](https://github.com/Azure/azure-powershell/releases) release. Estos son los cmdlets de Azure Disk Encryption para habilitar de conjunto de escalado de máquinas virtuales ([establecer](/powershell/module/az.compute/set-azvmssdiskencryptionextension)) cifrado, recuperar ([obtener](/powershell/module/az.compute/get-azvmssvmdiskencryption)) estado de cifrado y quitar ([deshabilitar](/powershell/module/az.compute/disable-azvmssdiskencryption)) el cifrado en la escala establece la instancia.

| Get-Help | Version |  Origen  |
| ------------- |-------------| ------------|
| Get-AzVmssDiskEncryptionStatus   | 1.0.0 o versiones posteriores | Az.Compute |
| Get-AzVmssVMDiskEncryptionStatus   | 1.0.0 o versiones posteriores | Az.Compute |
| Disable-AzVmssDiskEncryption   | 1.0.0 o versiones posteriores | Az.Compute |
| Get-AzVmssDiskEncryption   | 1.0.0 o versiones posteriores | Az.Compute |
| Get-AzVmssVMDiskEncryption   | 1.0.0 o versiones posteriores | Az.Compute |
| Set-AzVmssDiskEncryptionExtension   | 1.0.0 o versiones posteriores | Az.Compute |


## <a name="supported-scenarios-for-disk-encryption"></a>Escenarios admitidos para el cifrado de disco
* Cifrado para conjuntos de escalado de máquinas virtuales solo se admite para conjuntos de escalado creados con discos administrados. No es compatible con conjuntos de escalado con disco nativo (o no administrado).
* Se admite el cifrado para el sistema operativo y los volúmenes de datos de escalado de máquinas virtuales se establece en Windows. Deshabilitar cifrado también se admite para el sistema operativo y volúmenes de datos de escalado de máquinas virtuales se establece en Windows.
* No se admiten las operaciones de actualización y crear una nueva imagen de máquina virtual para conjuntos de escalado de máquinas virtuales en la vista previa actual.


## <a name="create-a-new-cluster-and-enable-disk-encryption"></a>Crear un nuevo clúster y habilitar el cifrado de disco

Use los siguientes comandos para crear un clúster y habilitar el cifrado de disco con una plantilla de Azure Resource Manager y un certificado autofirmado.

### <a name="sign-in-to-azure"></a>Inicio de sesión en Azure 
Inicie sesión con los siguientes comandos:
```powershell
Login-AzAccount
Set-AzContext -SubscriptionId <guid>

```

```azurecli

azure login
az account set --subscription $subscriptionId

```

### <a name="use-the-custom-template-that-you-already-have"></a>Uso de la plantilla personalizada ya existente 

Si necesita crear una plantilla personalizada para satisfacer sus necesidades, es muy recomendable que comience con una de las plantillas que están disponibles en el [ejemplos de plantilla de creación de clústeres de Azure Service Fabric](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master) página. Para [personalizar la plantilla del clúster] [ customize-your-cluster-template] sección, consulte las instrucciones siguientes.

Si ya tiene una plantilla personalizada, compruebe que los tres parámetros relacionados con el certificado en la plantilla y el archivo de parámetros tengan los nombres siguientes y que los valores son null como sigue:

```Json
   "certificateThumbprint": {
      "value": ""
    },
    "sourceVaultValue": {
      "value": ""
    },
    "certificateUrlValue": {
      "value": ""
    },
```


```powershell
$resourceGroupLocation="westus"
$resourceGroupName="mycluster"
$CertSubjectName="mycluster.westus.cloudapp.azure.com"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$certOutputFolder="c:\certificates"

$parameterFilePath="c:\templates\templateparam.json"
$templateFilePath="c:\templates\template.json"


New-AzServiceFabricCluster -ResourceGroupName $resourceGroupName -CertificateOutputFolder $certOutputFolder -CertificatePassword $certpassword -CertificateSubjectName $CertSubjectName -TemplateFile $templateFilePath -ParameterFile $parameterFilePath 

```


```azurecli

declare certPassword=""
declare resourceGroupLocation="westus"
declare resourceGroupName="mylinux"
declare certSubjectName="mylinuxsecure.westus.cloudapp.azure.com"
declare parameterFilePath="c:\mytemplates\linuxtemplateparm.json"
declare templateFilePath="c:\mytemplates\linuxtemplate.json"
declare certOutputFolder="c:\certificates"


az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --certificate-output-folder $certOutputFolder --certificate-password $certPassword  \
    --certificate-subject-name $certSubjectName \
    --template-file $templateFilePath --parameter-file $parametersFilePath

```

### <a name="deploy-an-application-to-a-service-fabric-cluster-in-windows"></a>Implementar una aplicación en un clúster de Service Fabric en Windows
Para implementar una aplicación en el clúster, siga los pasos y orientación en [implementación y quitar aplicaciones con PowerShell](service-fabric-deploy-remove-applications.md).


### <a name="enable-disk-encryption-for-the-virtual-machine-scale-sets-created-previously"></a>Habilitar el cifrado de disco para los conjuntos de escalado de máquina virtual que creó anteriormente

Para habilitar el cifrado de disco para el escalado de máquinas virtuales establece que ha creado a través de los pasos anteriores, ejecute los siguientes comandos:
 
```powershell

$VmssName = "nt1vm"
$vaultName = "mykeyvault"
$resourceGroupName = "mycluster"
$KeyVault = Get-AzKeyVault -VaultName $vaultName -ResourceGroupName $rgName
$DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri
$KeyVaultResourceId = $KeyVault.ResourceId

Set-AzVmssDiskEncryptionExtension -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType All

```

```azurecli

az vmss encryption enable -g <resourceGroupName> -n <VMSS name> --disk-encryption-keyvault <KeyVaultResourceId>

```


### <a name="validate-if-disk-encryption-is-enabled-for-a-virtual-machine-scale-set-in-windows"></a>Validar si está habilitado el cifrado de disco para un conjunto de escalado en Windows
Obtener el estado de un conjunto de escalado de máquina virtual completa o cualquier instancia de un conjunto de escalado mediante la ejecución de los siguientes comandos.

```powershell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Get-AzVmssDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName

Get-AzVmssVMDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName -InstanceId "0"

```

```azurecli

az vmss encryption show -g <resourceGroupName> -n <VMSS name>

```


Además, puede iniciar sesión en el conjunto de escalado de máquina virtual y asegúrese de que las unidades se cifran.

### <a name="disable-disk-encryption-for-a-virtual-machine-scale-set-in-a-service-fabric-cluster"></a>Deshabilitar el cifrado de disco para un conjunto de escalado en un clúster de Service Fabric 
Deshabilitar el cifrado de disco para un conjunto mediante la ejecución de los siguientes comandos de escalado de máquina virtual. Tenga en cuenta que la deshabilitación del cifrado de disco se aplica el conjunto de escalado de máquina virtual completa y no una instancia individual.

```powershell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Disable-AzVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $VmssName

```

```CLI

az vmss encryption disable -g <resourceGroupName> -n <VMSS name>

```


## <a name="next-steps"></a>Pasos siguientes
En este momento, debe haber un clúster seguro y saber cómo habilitar y deshabilitar el cifrado de disco para los nodos de clúster de Service Fabric y conjuntos de escalado de máquinas virtuales. Para obtener instrucciones similares en los nodos de clúster de Service Fabric en Linux, consulte [Disk Encryption para Linux](service-fabric-enable-azure-disk-encryption-linux.md).

[customize-your-cluster-template]: https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure#creating-a-custom-arm-template
