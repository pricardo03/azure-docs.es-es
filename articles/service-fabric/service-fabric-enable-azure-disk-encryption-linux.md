---
title: Habilitar el cifrado de disco para los clústeres de Linux de Azure Service Fabric | Microsoft Docs
description: En este artículo se describe cómo habilitar el cifrado de disco para el conjunto de escalado de un clúster de Service Fabric en Azure mediante Azure Resource Manager y Azure Key Vault.
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
ms.openlocfilehash: f580bf02b222f01a3d5aad1254f208791ea22b38
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59046988"
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



[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Requisitos previos

* **Registro automático**: para su uso, la versión preliminar del cifrado de disco de un conjunto de escalado de máquinas virtuales requiere el registro automático
* Puede realizar el registro automático de la suscripción mediante la ejecución de los pasos siguientes: 
```powershell
Register-AzProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
```
* Espere unos 10 minutos hasta que el estado sea "Registrado". Puede comprobar el estado ejecutando el comando siguiente: 
```powershell
Get-AzProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
Register-AzResourceProvider -ProviderNamespace Microsoft.Compute
```
* **Azure Key Vault**: cree un almacén de claves en la misma suscripción y región que el conjunto de escalado de máquina virtual y establezca la directiva de acceso "EnabledForDiskEncryption" en el almacén de claves mediante el cmdlet de PS. También puede establecer la directiva usando la interfaz de usuario de Key Vault en Azure Portal: 
```powershell
Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -EnabledForDiskEncryption
```
* Instalar la versión más reciente [CLI de Azure](/cli/azure/install-azure-cli) , que contiene los nuevos comandos de cifrado.
* Instale la versión más reciente del [SDK de Azure desde Azure PowerShell](https://github.com/Azure/azure-powershell/releases). Los siguientes son el conjunto de escalado de máquinas virtuales ADE cmdlets para habilitar ([establecer](/powershell/module/az.compute/set-azvmssdiskencryptionextension)) cifrado, recuperar ([obtener](/powershell/module/az.compute/get-azvmssvmdiskencryption)) estado de cifrado y quitar ([deshabilitar](/powershell/module/az.compute/disable-azvmssdiskencryption)) cifrado en escala instancia del conjunto. 

| Get-Help | Versión |  Origen  |
| ------------- |-------------| ------------|
| Get-AzVmssDiskEncryptionStatus   | 1.0.0 o versiones posteriores | Az.Compute |
| Get-AzVmssVMDiskEncryptionStatus   | 1.0.0 o versiones posteriores | Az.Compute |
| Disable-AzVmssDiskEncryption   | 1.0.0 o versiones posteriores | Az.Compute |
| Get-AzVmssDiskEncryption   | 1.0.0 o versiones posteriores | Az.Compute |
| Get-AzVmssVMDiskEncryption   | 1.0.0 o versiones posteriores | Az.Compute |
| Set-AzVmssDiskEncryptionExtension   | 1.0.0 o versiones posteriores | Az.Compute |


## <a name="supported-scenarios-for-disk-encryption"></a>Escenarios admitidos para el cifrado de disco
* El cifrado de conjuntos de escalado de máquinas virtuales se admite solo en conjuntos de escalado creados con discos administrados y no es compatible con conjuntos de escalado con discos nativos (o no administrados).
* El cifrado de conjuntos de escalado de máquinas virtuales se admite para volúmenes de datos para conjuntos de escalado de máquinas virtuales Linux. El cifrado de discos del sistema operativo no se admite en la versión preliminar actual para Linux.
* Crear una nueva imagen de máquina virtual del conjunto de escalado de máquinas virtuales y las operaciones de actualización no se admiten en la vista previa actual.


### <a name="create-new-linux-cluster-and-enable-disk-encryption"></a>Creación de un nuevo clúster de Linux y habilitación del cifrado de disco

Use los siguientes comandos para crear clúster y habilitar el cifrado de disco mediante un certificado autofirmado & plantilla de Azure Resource Manager.

### <a name="sign-in-to-azure"></a>Inicio de sesión en Azure  

```powershell

Login-AzAccount
Set-AzContext -SubscriptionId <guid>

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

Este es el comando equivalente de la CLI para hacer lo mismo. Cambie los valores de las instrucciones declare por los valores adecuados. La CLI admite todos los demás parámetros que admiten los comandos de PowerShell anteriores.

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

#### <a name="linux-data-disk-mounting"></a>Montaje de disco de datos de Linux
Antes de proceder a realizar el cifrado en un conjunto de escalado de máquinas virtuales Linux, es necesario asegurarse de que el disco de datos agregado está correctamente montado. Inicie sesión para la máquina virtual de clúster de Linux y ejecute el comando LSBLK. El resultado debe mostrar ese disco de datos agregado en la columna de punto de montaje.


#### <a name="deploy-application-to-linux-service-fabric-cluster"></a>Implementación de una aplicación en un clúster de Service Fabric con Linux
Siga los pasos y la guía para [implementar una aplicación en el clúster](service-fabric-quickstart-containers-linux.md)


#### <a name="enable-disk-encryption-for-service-fabric-linux-cluster-virtual-machine-scale-set-created-above"></a>Habilitación del cifrado de disco para el conjunto de escalado de máquinas virtuales del clúster de Service Fabric con Linux creado anteriormente
 
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

#### <a name="validate-if-disk-encryption-enabled-for-linux-virtual-machine-scale-set"></a>Compruebe si el cifrado del disco está habilitado para el conjunto de escalado de máquinas virtuales Linux.
Obtenga el estado de un conjunto de escalado de máquinas virtuales completo o de cualquier instancia de máquina virtual del conjunto de escalado. A continuación, puede ver los comandos.
Además el usuario puede iniciar sesión en la máquina virtual de clúster de Linux y ejecute el comando LSBLK. El resultado debe mostrar ese disco de datos agregado en la columna de punto de montaje y la columna de tipo como cifrado para el disco de datos agregado.

```powershell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Get-AzVmssDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName

Get-AzVmssVMDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName -InstanceId "0"

```

```azurecli
az vmss encryption show -g <resourceGroupName> -n <VMSS name>

```

#### <a name="disable-disk-encryption-for-service-fabric-cluster-virtual-machine-scale-set"></a>Deshabilitación del cifrado de disco para el conjunto de escalado de máquinas virtuales del clúster de Service Fabric 
La deshabilitación del cifrado del disco se aplica al conjunto de escalado de máquinas virtuales completo y no a cada instancia 

```powershell
$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Disable-AzVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $VmssName

```

```azurecli
az vmss encryption disable -g <resourceGroupName> -n <VMSS name>

```


## <a name="next-steps"></a>Pasos siguientes
En este punto, dispone de un clúster seguro y el procedimiento para habilitar o deshabilitar el cifrado del disco para el conjunto de escalado de máquinas virtuales del clúster de Service Fabric con Linux. A continuación, [Cifrado de disco para Windows](service-fabric-enable-azure-disk-encryption-windows.md) 
