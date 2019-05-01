---
title: Implementar una máquina virtual de Azure desde un disco duro virtual de usuario | Azure Marketplace
description: Explica cómo implementar una imagen de disco duro virtual de usuario para crear una instancia de máquina virtual de Azure.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: article
ms.date: 11/29/2018
ms.author: pabutler
ms.openlocfilehash: e4da523fa54a513fe77fda037aea0a5fd530250b
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64938237"
---
# <a name="deploy-an-azure-vm-from-a-user-vhd"></a>Implementación de una máquina virtual de Azure desde un disco duro virtual de usuario

En este artículo se explica cómo implementar una imagen generalizada de VHD para crear un nuevo recurso de máquina virtual de Azure, mediante la plantilla suministrada de Azure Resource Manager y el script de Azure PowerShell.

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

## <a name="vhd-deployment-template"></a>Plantilla de implementación de disco duro virtual

Copie la plantilla de Azure Resource Manager para la [implementación de VHD](cpp-deploy-json-template.md) en un archivo local denominado `VHDtoImage.json`.  Edite este archivo para proporcionar valores para los parámetros siguientes. 

|  **Parámetro**             |   **Descripción**                                                              |
|  -------------             |   ---------------                                                              |
| ResourceGroupName          | El nombre del grupo de recursos de Azure existente.  Normalmente se usa el mismo grupo de recursos asociado al almacén de claves  |
| TemplateFile               | Ruta de acceso completa al archivo `VHDtoImage.json`                                    |
| userStorageAccountName     | Nombre de la cuenta de almacenamiento                                                    |
| sNameForPublicIP           | Nombre DNS para la dirección IP pública. Debe estar en minúscula                                  |
| subscriptionId             | Identificador de la suscripción de Azure                                                  |
| Location                   | Ubicación geográfica Azure estándar del grupo de recursos                       |
| vmName                     | Nombre de la máquina virtual                                                    |
| vaultName                  | Nombre del almacén de claves.                                                          |
| vaultResourceGroup         | Grupo de recursos del almacén de claves.
| certificateUrl             | Dirección URL del certificado, incluida la versión almacenada en el almacén de claves, por ejemplo: `https://testault.vault.azure.net/secrets/testcert/b621es1db241e56a72d037479xab1r7` |
| vhdUrl                     | Dirección URL del disco duro virtual                                                   |
| vmSize                     | tamaño de la instancia de máquina virtual                                           |
| publicIPAddressName        | Nombre de la dirección IP pública                                                  |
| virtualNetworkName         | Nombre de la red virtual                                                    |
| nicName                    | Nombre de la tarjeta de interfaz de red de la red virtual                     |
| adminUserName              | Nombre de usuario de la cuenta de administrador                                          |
| adminPassword              | Contraseña de administrador                                                          |
|  |  |


## <a name="powershell-script"></a>Script de PowerShell

Copie y edite el script siguiente para proporcionar valores para las variables `$storageaccount` y `$vhdUrl`.  Ejecútelo para crear un recurso de máquina virtual de Azure a partir de su disco duro virtual generalizado existente.

```powershell
# storage account of existing generalized VHD 
$storageaccount = "testwinrm11815"

# generalized VHD URL
$vhdUrl = "https://testwinrm11815.blob.core.windows.net/vhds/testvm1234562016651857.vhd"

echo "New-AzResourceGroupDeployment -Name "dplisvvm$postfix" -ResourceGroupName "$rgName" -TemplateFile "C:\certLocation\VHDtoImage.json" -userStorageAccountName "$storageaccount" -dnsNameForPublicIP "$vmName" -subscriptionId "$mysubid" -location "$location" -vmName "$vmName" -vaultName "$kvname" -vaultResourceGroup "$rgName" -certificateUrl $objAzureKeyVaultSecret.Id  -vhdUrl "$vhdUrl" -vmSize "Standard_A2" -publicIPAddressName "myPublicIP1" -virtualNetworkName "myVNET1" -nicName "myNIC1" -adminUserName "isv" -adminPassword $pwd"

#deploying VM with existing VHD
New-AzResourceGroupDeployment -Name "dplisvvm$postfix" -ResourceGroupName "$rgName" -TemplateFile "C:\certLocation\VHDtoImage.json" -userStorageAccountName "$storageaccount" -dnsNameForPublicIP "$vmName" -subscriptionId "$mysubid" -location "$location" -vmName "$vmName" -vaultName "$kvname" -vaultResourceGroup "$rgName" -certificateUrl $objAzureKeyVaultSecret.Id  -vhdUrl "$vhdUrl" -vmSize "Standard_A2" -publicIPAddressName "myPublicIP1" -virtualNetworkName "myVNET1" -nicName "myNIC1" -adminUserName "isv" -adminPassword $pwd 

```

## <a name="next-steps"></a>Pasos siguientes

Una vez implementada la máquina virtual, está listo para [certificar la imagen de máquina virtual](./cpp-certify-vm.md).
