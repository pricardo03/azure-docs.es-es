---
title: Habilitar el cifrado de disco para los clústeres de Linux de Azure Service Fabric | Microsoft Docs
description: En este artículo se describe cómo habilitar el cifrado de disco para los nodos de clúster de Azure Service Fabric en Linux mediante el uso de Azure Resource Manager y Azure Key Vault.
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
ms.openlocfilehash: 47b07188d1757708fb494c6a66e93379657e806a
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2019
ms.locfileid: "66258762"
---
# <a name="enable-disk-encryption-for-azure-service-fabric-cluster-nodes-in-linux"></a>Habilitar el cifrado de disco para los nodos de clúster de Azure Service Fabric en Linux 
> [!div class="op_single_selector"]
> * [Cifrado de disco para Linux](service-fabric-enable-azure-disk-encryption-linux.md)
> * [Cifrado de disco para Windows](service-fabric-enable-azure-disk-encryption-windows.md)
>
>

En este tutorial, obtendrá información sobre cómo habilitar el cifrado de disco en los nodos de clúster de Azure Service Fabric en Linux. Deberá seguir estos pasos para cada uno de los tipos de nodos y conjuntos de escalado de máquinas virtuales. Para el cifrado de los nodos, vamos a usar la funcionalidad de Azure Disk Encryption en conjuntos de escalado de máquinas virtuales.

La guía trata los temas siguientes:

* Conceptos clave a tener en cuenta cuando se establece habilitar el cifrado de disco en el escalado de máquinas virtuales del clúster de Service Fabric en Linux.
* Los pasos que se deben seguir antes de habilitar el cifrado de disco en Service Fabric del clúster los nodos de Linux.
* Pasos a seguir para habilitar el cifrado de disco en los nodos de clúster de Service Fabric en Linux.



[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Requisitos previos

 **Registro automático**

La versión preliminar de cifrado de disco para el conjunto de escalado de máquinas virtuales requiere el registro de autoservicio. Para ello, siga los pasos que se describen a continuación:

1. Ejecute el siguiente comando: 
    ```powershell
    Register-AzProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
    ```
2. Espere unos 10 minutos hasta que se lee el estado *registrado*. Puede comprobar el estado, ejecute el comando siguiente:
    ```powershell
    Get-AzProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
    Register-AzResourceProvider -ProviderNamespace Microsoft.Compute
    ```
**Azure Key Vault**

1. Cree un almacén de claves en la misma suscripción y región que el conjunto de escalado. A continuación, seleccione el **EnabledForDiskEncryption** directiva en el almacén de claves de acceso mediante el cmdlet de PowerShell. También puede establecer la directiva mediante el uso de la interfaz de usuario del almacén de claves en el portal de Azure con el siguiente comando:
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
* Se admiten cifrado y deshabilitación del cifrado de volúmenes de datos y del sistema operativo en conjuntos de escalado de máquinas virtuales de Linux. 
* No se admiten las operaciones de actualización y crear una nueva imagen de máquina virtual (VM) para conjuntos de escalado de máquinas virtuales en la vista previa actual.


## <a name="create-a-new-cluster-and-enable-disk-encryption"></a>Crear un nuevo clúster y habilitar el cifrado de disco

Use los siguientes comandos para crear un clúster y habilitar el cifrado de disco mediante el uso de una plantilla de Azure Resource Manager y un certificado autofirmado.

### <a name="sign-in-to-azure"></a>Inicio de sesión en Azure  

Inicie sesión con los siguientes comandos:

```powershell

Login-AzAccount
Set-AzContext -SubscriptionId <guid>

```

```CLI

azure login
az account set --subscription $subscriptionId

```

### <a name="use-the-custom-template-that-you-already-have"></a>Uso de la plantilla personalizada ya existente 

Si necesita crear una plantilla personalizada, recomendamos encarecidamente que use una de las plantillas en el [ejemplos de plantilla de creación de clústeres de Azure Service Fabric](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master) página. 

Si ya tiene una plantilla personalizada, compruebe que los tres parámetros relacionados con el certificado en la plantilla y el archivo de parámetros se denominan como sigue. Asegúrese también de que los valores son null como sigue:

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

Dado que el cifrado de disco de datos solo se admite para conjuntos de escalado de máquinas virtuales de Linux, debe agregar un disco de datos mediante una plantilla de Resource Manager. Actualizar la plantilla para el aprovisionamiento de disco de datos como sigue:

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

Este es el comando equivalente de la CLI. Cambie los valores de las instrucciones declare en los valores adecuados. La CLI admite todos los demás parámetros que admite el comando de PowerShell anterior.

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

### <a name="mount-a-data-disk-to-a-linux-instance"></a>Montar un disco de datos a una instancia de Linux
Antes de continuar con el cifrado en un conjunto de escalado de máquinas virtuales, asegúrese de que está montado el disco de datos se ha agregado correctamente. Inicie sesión la máquina virtual del clúster de Linux y ejecute el **LSBLK** comando. El resultado debe mostrar ese disco de datos agregados en el **punto de montaje** columna.


### <a name="deploy-application-to-a-service-fabric-cluster-in-linux"></a>Implementar la aplicación en un clúster de Service Fabric en Linux
Para implementar una aplicación en el clúster, siga los pasos y orientación en [inicio rápido: Implementar contenedores Linux en Service Fabric](service-fabric-quickstart-containers-linux.md).


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

### <a name="validate-if-disk-encryption-is-enabled-for-a-virtual-machine-scale-set-in-linux"></a>Validar si está habilitado el cifrado de disco para un conjunto de escalado de Linux
Para obtener el estado de un conjunto de escalado de máquina virtual completa o cualquier instancia de un conjunto de escalado, ejecute los siguientes comandos.
Además, puede iniciar sesión en la máquina virtual del clúster de Linux y ejecutar el **LSBLK** comando. El resultado debe mostrar el disco de datos agregados en el **punto de montaje** columna y el **tipo** debe leer columna *Crypt*.

```powershell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Get-AzVmssDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName

Get-AzVmssVMDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName -InstanceId "0"

```

```azurecli
az vmss encryption show -g <resourceGroupName> -n <VMSS name>

```

### <a name="disable-disk-encryption-for-a-virtual-machine-scale-set-in-a-service-fabric-cluster"></a>Deshabilitar el cifrado de disco para un conjunto de escalado en un clúster de Service Fabric
Deshabilitar el cifrado de disco para un conjunto mediante la ejecución de los siguientes comandos de escalado de máquina virtual. Tenga en cuenta que la deshabilitación del cifrado de disco se aplica el conjunto de escalado de máquina virtual completa y no una instancia individual.

```powershell
$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Disable-AzVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $VmssName

```

```azurecli
az vmss encryption disable -g <resourceGroupName> -n <VMSS name>

```


## <a name="next-steps"></a>Pasos siguientes
En este momento, debe haber un clúster seguro y saber cómo habilitar y deshabilitar el cifrado de disco para los nodos de clúster de Service Fabric y conjuntos de escalado de máquinas virtuales. Para obtener instrucciones similares en los nodos de clúster de Service Fabric en Linux, consulte [cifrado de disco para Windows](service-fabric-enable-azure-disk-encryption-windows.md). 
