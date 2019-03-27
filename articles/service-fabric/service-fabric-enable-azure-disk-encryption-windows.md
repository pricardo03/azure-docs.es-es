---
title: Habilitar el cifrado de disco para los clústeres de Windows de Azure Service Fabric | Microsoft Docs
description: En este artículo se describe cómo habilitar el cifrado de disco para los nodos de un clúster de Service Fabric en Azure mediante Azure Resource Manager y Azure Key Vault.
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
ms.openlocfilehash: a620563be9ffe18ae0f7fa4a78df83ea5b35a5d2
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "58488293"
---
# <a name="enable-disk-encryption-for-service-fabric-windows-cluster-nodes"></a>Habilitación del cifrado de disco para nodos de clústeres con Windows de Service Fabric 
> [!div class="op_single_selector"]
> * [Cifrado de disco para Windows](service-fabric-enable-azure-disk-encryption-windows.md)
> * [Cifrado de disco para Linux](service-fabric-enable-azure-disk-encryption-linux.md)
>
>

Siga los pasos siguientes para habilitar el cifrado de disco en los nodos de un clúster con Windows de Service Fabric. Debe hacer esto para cada uno de los tipos de nodo o de los conjuntos de escalado de máquinas virtuales. Para cifrar los nodos, aprovechamos la funcionalidad Azure Disk Encryption en los conjuntos de escalado de máquinas virtuales.

La guía abarca los siguientes procedimientos:

* Conceptos clave que debe tener en cuenta para habilitar el cifrado de disco en un conjunto de escalado de máquinas virtuales de un clúster con Windows de Service Fabric.
* Pasos de requisitos previos que se deben seguir antes de habilitar el cifrado de disco en un conjunto de escalado de máquinas virtuales de un clúster con Windows de Service Fabric.
* Pasos que se deben seguir para habilitar el cifrado de disco en un conjunto de escalado de máquinas virtuales de un clúster con Windows de Service Fabric.


## <a name="prerequisites"></a>Requisitos previos
* **Registro automático**: para su uso, la versión preliminar del cifrado de disco de un conjunto de escalado de máquinas virtuales requiere el registro automático
* Puede realizar el registro automático de la suscripción mediante la ejecución de los pasos siguientes: 
```powershell
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
```
* Espere unos 10 minutos hasta que el estado sea "Registrado". Puede comprobar el estado ejecutando el comando siguiente: 
```powershell
Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
```
* **Azure Key Vault**: cree un almacén de claves en la misma suscripción y región que el conjunto de escalado y establezca la directiva de acceso "EnabledForDiskEncryption" en el almacén de claves mediante el cmdlet de PS. También puede establecer la directiva usando la interfaz de usuario de Key Vault en Azure Portal: 
```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -EnabledForDiskEncryption
```
* Instale la versión más reciente de la [CLI de Azure 2.0](/cli/azure/install-azure-cli?view=azure-cli-latest), que tiene nuevos comandos de cifrado.
* Instale la versión más reciente del [SDK de Azure desde Azure PowerShell](https://github.com/Azure/azure-powershell/releases). Estos son los cmdlets de ADE para habilitar ([Set](/powershell/module/azurerm.compute/set-azurermvmssdiskencryptionextension?view=azurermps-4.4.1)) el cifrado, recuperar ([Get](/powershell/module/azurerm.compute/get-azurermvmssvmdiskencryption?view=azurermps-4.4.1)) el estado de cifrado y eliminar ([disable](/powershell/module/azurerm.compute/disable-azurermvmssdiskencryption?view=azurermps-4.4.1)) el cifrado en la instancia del conjunto de escalado.

| Get-Help | Versión |  Origen  |
| ------------- |-------------| ------------|
| Get-AzureRmVmssDiskEncryptionStatus   | 3.4.0 o superior | AzureRM.Compute |
| Get-AzureRmVmssVMDiskEncryptionStatus   | 3.4.0 o superior | AzureRM.Compute |
| Disable-AzureRmVmssDiskEncryption   | 3.4.0 o superior | AzureRM.Compute |
| Get-AzureRmVmssDiskEncryption   | 3.4.0 o superior | AzureRM.Compute |
| Get-AzureRmVmssVMDiskEncryption   | 3.4.0 o superior | AzureRM.Compute |
| Set-AzureRmVmssDiskEncryptionExtension   | 3.4.0 o superior | AzureRM.Compute |


## <a name="supported-scenarios-for-disk-encryption"></a>Escenarios admitidos para el cifrado de disco
* El cifrado de conjuntos de escalado de máquinas virtuales se admite solo en conjuntos de escalado creados con discos administrados y no es compatible con conjuntos de escalado con discos nativos (o no administrados).
* Cifrado de conjunto de escalado de máquina virtual es compatible con volúmenes de datos y del sistema operativo para el conjunto de escalado de máquinas virtuales de Windows. También se admite la deshabilitación del cifrado de volúmenes de datos y del sistema operativo en conjuntos de escalado de Windows.
* Crear una nueva imagen de máquina virtual del conjunto de escalado de máquinas virtuales y las operaciones de actualización no se admiten en la vista previa actual.


### <a name="create-new-cluster-and-enable-disk-encryption"></a>Creación de un nuevo clúster y habilitación del cifrado de disco

Use los siguientes comandos para crear clúster y habilitar el cifrado de disco mediante un certificado autofirmado & plantilla de Azure Resource Manager.

### <a name="sign-in-to-azure"></a>Inicio de sesión en Azure 

```powershell
Login-AzureRmAccount
Set-AzureRmContext -SubscriptionId <guid>

```

```azurecli

azure login
az account set --subscription $subscriptionId

```

#### <a name="use-the-custom-template-that-you-already-have"></a>Uso de la plantilla personalizada ya existente 

Si tiene que crear una plantilla personalizada para adaptarla a sus necesidades, es muy aconsejable comenzar con una de las ya disponibles en las [plantillas de ejemplo de Azure Service Fabric](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master). Siga las instrucciones y explicaciones para [personalizar la plantilla del clúster][customize-your-cluster-template] en la sección que viene a continuación.

Si ya tiene una plantilla personalizada, asegúrese de volver a comprobar que los tres parámetros relacionados con el certificado de la plantilla y el archivo de parámetros tengan los nombres siguientes y que los valores sean null como se indica a continuación.

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


New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -CertificateOutputFolder $certOutputFolder -CertificatePassword $certpassword -CertificateSubjectName $CertSubjectName -TemplateFile $templateFilePath -ParameterFile $parameterFilePath 

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

#### <a name="deploy-application-to-windows-service-fabric-cluster"></a>Implementación de una aplicación en un clúster de Service Fabric con Windows
Siga los pasos y la guía para [implementar una aplicación en el clúster](service-fabric-deploy-remove-applications.md)


#### <a name="enable-disk-encryption-for-service-fabric-cluster-virtual-machine-scale-set-created-above"></a>Habilitación del cifrado de disco para el conjunto de escalado de máquinas virtuales del clúster de Service Fabric creado anteriormente
 
```powershell

$VmssName = "nt1vm"
$vaultName = "mykeyvault"
$resourceGroupName = "mycluster"
$KeyVault = Get-AzureRmKeyVault -VaultName $vaultName -ResourceGroupName $rgName
$DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri
$KeyVaultResourceId = $KeyVault.ResourceId

Set-AzureRmVmssDiskEncryptionExtension -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType All

```

```azurecli

az vmss encryption enable -g <resourceGroupName> -n <VMSS name> --disk-encryption-keyvault <KeyVaultResourceId>

```


#### <a name="validate-if-disk-encryption-enabled-for-windows-virtual-machine-scale-set"></a>Compruebe si el cifrado del disco está habilitado para el conjunto de escalado de máquinas virtuales Windows.
Obtenga el estado de un conjunto de escalado de máquinas virtuales completo o de cualquier instancia del conjunto de escalado. A continuación, puede ver los comandos.
Además el usuario puede iniciar sesión en la máquina virtual en el conjunto de escalado y asegúrese de que las unidades están cifradas

```powershell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Get-AzureRmVmssDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName

Get-AzureRmVmssVMDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName -InstanceId "0"

```

```azurecli

az vmss encryption show -g <resourceGroupName> -n <VMSS name>

```


#### <a name="disable-disk-encryption-for-service-fabric-cluster-virtual-machine-scale-set"></a>Deshabilitación del cifrado de disco para el conjunto de escalado de máquinas virtuales del clúster de Service Fabric 
La deshabilitación del cifrado del disco se aplica al conjunto de escalado de máquinas virtuales completo y no a cada instancia 

```powershell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Disable-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $VmssName

```

```CLI

az vmss encryption disable -g <resourceGroupName> -n <VMSS name>

```


## <a name="next-steps"></a>Pasos siguientes
En este punto, dispone de un clúster seguro y el procedimiento para habilitar o deshabilitar el cifrado del disco para el conjunto de escalado de máquinas virtuales del clúster de Service Fabric. A continuación, [Cifrado de disco para Linux](service-fabric-enable-azure-disk-encryption-linux.md) 

[customize-your-cluster-template]: https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure#creating-a-custom-arm-template