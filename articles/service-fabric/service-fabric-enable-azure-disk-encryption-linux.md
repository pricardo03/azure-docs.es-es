---
title: Habilitación del cifrado de disco para clústeres con Linux de Service Fabric | Microsoft Docs
description: En este artículo se describe cómo habilitar el cifrado de disco para el conjunto de escalado de un clúster de Service Fabric en Azure mediante Azure Resource Manager y Azure Key Vault.
services: service-fabric
documentationcenter: .net
author: v-viban
manager: navya
ms.assetid: 15d0ab67-fc66-4108-8038-3584eeebabaa
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/24/2018
ms.author: v-viban
ms.openlocfilehash: 46f7f88768ab7ae9d84f392f340750865fef3b96
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "34660253"
---
# <a name="enable-disk-encryption-for-service-fabric-linux-cluster-nodes"></a>Habilitación del cifrado de disco para nodos de clústeres con Linux de Service Fabric 
> [!div class="op_single_selector"]
> * [Cifrado de disco para Linux](service-fabric-enable-azure-disk-encryption-linux.md)
> * [Cifrado de disco para Windows](service-fabric-enable-azure-disk-encryption-windows.md)
>
>

Siga los pasos siguientes para habilitar el cifrado de disco en los nodos de un clúster con Linux de Service Fabric. Debe hacer esto para cada uno de los tipos de nodo o de los conjuntos de escalado de máquinas virtuales. Para cifrar los nodos, aprovechamos la funcionalidad Azure Disk Encryption en los conjuntos de escalado de máquinas virtuales.

La guía abarca los siguientes procedimientos:

* Conceptos clave que debe tener en cuenta para habilitar el cifrado de disco en un conjunto de escalado de máquinas virtuales de un clúster con Linux de Service Fabric.
* Pasos de requisitos previos que se deben seguir antes de habilitar el cifrado de disco en un conjunto de escalado de máquinas virtuales de un clúster con Linux de Service Fabric.
* Pasos que se deben seguir para habilitar el cifrado de disco en un conjunto de escalado de máquinas virtuales de un clúster con Linux de Service Fabric.


## <a name="prerequisites"></a>requisitos previos

1. **Registro automático**: para su uso, la versión preliminar del cifrado de disco de un conjunto de escalado de máquinas virtuales requiere el registro automático
2. Puede realizar el registro automático de la suscripción mediante la ejecución de los pasos siguientes: 
```Powershell
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
```
3. Espere unos 10 minutos hasta que el estado sea "Registrado". Puede comprobar el estado ejecutando el comando siguiente: 
```Powershell
Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
```
4. **Azure Key Vault**: cree un almacén de claves en la misma suscripción y región que el conjunto de escalado de máquina virtual y establezca la directiva de acceso "EnabledForDiskEncryption" en el almacén de claves mediante el cmdlet de PS. También puede establecer la directiva usando la interfaz de usuario de Key Vault en Azure Portal: 
```Powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -EnabledForDiskEncryption
```
5. Instale la versión más reciente de la [CLI de Azure 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest), que tiene nuevos comandos de cifrado.
6. Instale la versión más reciente del [SDK de Azure desde Azure PowerShell](https://github.com/Azure/azure-powershell/releases). Los siguientes son los cmdlets de VMSS ADE para habilitar ([Set](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/set-azurermvmssdiskencryptionextension?view=azurermps-4.4.1)) el cifrado, recuperar ([Get](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/get-azurermvmssvmdiskencryption?view=azurermps-4.4.1)) el estado de cifrado y quitar ([Disable](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/disable-azurermvmssdiskencryption?view=azurermps-4.4.1)) el cifrado en la instancia del conjunto de escalado. 

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
* El cifrado de conjuntos de escalado de máquinas virtuales se admite para volúmenes de datos para conjuntos de escalado de máquinas virtuales Linux. El cifrado de discos del sistema operativo no se admite en la versión preliminar actual para Linux.
* En la versión preliminar actual no se admiten operaciones de restablecimiento de la imagen de las máquinas virtuales del conjunto de escalado ni operaciones de actualización.


### <a name="create-new-linux-cluster-and-enable-disk-encryption"></a>Creación de un nuevo clúster de Linux y habilitación del cifrado de disco

Use los siguientes comandos para crear el clúster y habilitar el cifrado de disco con una plantilla de Azure Resource Manager y un certificado autofirmado.

### <a name="log-in-to-azure"></a>Inicio de sesión en Azure  

```Powershell

Login-AzureRmAccount
Set-AzureRmContext -SubscriptionId <guid>

```

```CLI

azure login
az account set --subscription $subscriptionId

```

#### <a name="use-the-custom-template-that-you-already-have"></a>Uso de la plantilla personalizada ya existente 

Si tiene que crear una plantilla personalizada para adaptarla a sus necesidades, es muy aconsejable comenzar con una de las ya disponibles en las [plantillas de ejemplo de Azure Service Fabric](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master) para clústeres de Linux. 

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

Dado que para el conjunto de escalado de máquinas virtuales Linux solo se admite el cifrado de disco de datos, es necesario agregar un disco de datos mediante una plantilla de Azure Resource Manager. Actualice la plantilla para el aprovisionamiento de disco de datos como se indica a continuación:

```Json
   
   "storageProfile": { 
            "imageReference": { 
              "publisher": "[parameters('vmImagePublisher')]", 
              "offer": "[parameters('vmImageOffer')]", 
              "sku": "[parameters('vmImageSku')]", 
              "version": "[parameters('vmImageVersion')]" 
            }, 
            "osDisk": { 
              "caching": "ReadOnly", 
              "createOption": "FromImage", 
              "managedDisk": { 
                "storageAccountType": "[parameters('storageAccountType')]" 
              } 
           }, 
                "dataDisks": [ 
                { 
                    "diskSizeGB": 1023, 
                    "lun": 0, 
                    "createOption": "Empty" 
   
```
 

```Powershell


$resourceGroupLocation="westus"
$resourceGroupName="mycluster"
$CertSubjectName="mycluster.westus.cloudapp.azure.com"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$certOutputFolder="c:\certificates"

$parameterFilePath="c:\templates\templateparam.json"
$templateFilePath="c:\templates\template.json"


New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -CertificateOutputFolder $certOutputFolder -CertificatePassword $certpassword -CertificateSubjectName $CertSubjectName -TemplateFile $templateFilePath -ParameterFile $parameterFilePath 

```

Este es el comando equivalente de la CLI para hacer lo mismo. Cambie los valores de las instrucciones declare por los valores adecuados. La CLI admite todos los demás parámetros que admiten los comandos de PowerShell anteriores.

```CLI

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

#### <a name="linux-data-disk-mounting"></a>Montaje de disco de datos de Linux
Antes de proceder a realizar el cifrado en un conjunto de escalado de máquinas virtuales Linux, es necesario asegurarse de que el disco de datos agregado está correctamente montado. Inicie sesión en la máquina virtual del clúster de Linux y ejecute el comando LSBLK. El resultado debe mostrar ese disco de datos agregado en la columna de punto de montaje.


#### <a name="deploy-application-to-linux-service-fabric-cluster"></a>Implementación de una aplicación en un clúster de Service Fabric con Linux
Siga los pasos y la guía para [implementar una aplicación en el clúster](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-quickstart-containers-linux)


#### <a name="enable-disk-encryption-for-service-fabric-linux-cluster-virtual-machine-scale-set-created-above"></a>Habilitación del cifrado de disco para el conjunto de escalado de máquinas virtuales del clúster de Service Fabric con Linux creado anteriormente
 
```Powershell
$VmssName = "nt1vm"
$vaultName = "mykeyvault"
$resourceGroupName = "mycluster"
$KeyVault = Get-AzureRmKeyVault -VaultName $vaultName -ResourceGroupName $rgName
$DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri
$KeyVaultResourceId = $KeyVault.ResourceId

Set-AzureRmVmssDiskEncryptionExtension -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType All

```

```CLI

az vmss encryption enable -g <resourceGroupName> -n <VMSS name> --disk-encryption-keyvault <KeyVaultResourceId>

```

#### <a name="validate-if-disk-encryption-enabled-for-linux-virtual-machine-scale-set"></a>Compruebe si el cifrado del disco está habilitado para el conjunto de escalado de máquinas virtuales Linux.
Obtenga el estado de un conjunto de escalado de máquinas virtuales completo o de cualquier instancia de máquina virtual del conjunto de escalado. A continuación, puede ver los comandos.
El usuario también puede iniciar sesión en la máquina virtual del clúster de Linux y ejecutar el comando LSBLK. El resultado debe mostrar ese disco de datos agregado en la columna de punto de montaje y la columna de tipo como cifrado para el disco de datos agregado.

```Powershell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Get-AzureRmVmssDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName

Get-AzureRmVmssVMDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName -InstanceId "0"

```

```CLI

az vmss encryption show -g <resourceGroupName> -n <VMSS name>

```



#### <a name="disable-disk-encryption-for-service-fabric-cluster-virtual-machine-scale-set"></a>Deshabilitación del cifrado de disco para el conjunto de escalado de máquinas virtuales del clúster de Service Fabric 
La deshabilitación del cifrado del disco se aplica al conjunto de escalado de máquinas virtuales completo y no a cada instancia 

```Powershell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Disable-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $VmssName

```

```CLI

az vmss encryption disable -g <resourceGroupName> -n <VMSS name>

```


## <a name="next-steps"></a>Pasos siguientes
En este punto, dispone de un clúster seguro y el procedimiento para habilitar o deshabilitar el cifrado del disco para el conjunto de escalado de máquinas virtuales del clúster de Service Fabric con Linux. A continuación, [Cifrado de disco para Windows](service-fabric-enable-azure-disk-encryption-windows.md) 

