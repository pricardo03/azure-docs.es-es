---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 10/24/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 0659e57f5a5b223c199becf492b27c7a70cbdc63
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2019
ms.locfileid: "73612169"
---
Los discos administrados de Azure cifran automáticamente los datos de forma predeterminada cuando se guardan en la nube. El cifrado del lado servidor protege los datos y le ayuda a cumplir los compromisos de cumplimiento y seguridad de la organización. Los datos de los discos administrados de Azure se cifran de forma transparente mediante [cifrado AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) de 256 bits, uno de los cifrados de bloques más sólidos que hay disponibles, y son compatibles con FIPS 140-2.   

El cifrado no afecta al rendimiento de los discos administrados. No hay ningún coste adicional por el cifrado.

Para más información sobre de los módulos criptográficos subyacentes en los discos administrados de Azure, consulte [Cryptography API: Next Generation](https://docs.microsoft.com/windows/desktop/seccng/cng-portal)

## <a name="about-encryption-key-management"></a>Información sobre la administración de claves de cifrado

Puede confiar en las claves administradas por la plataforma para el cifrado del disco administrado o puede administrar el cifrado con sus propias claves (versión preliminar pública). Si opta por administrar el cifrado con sus propias claves, puede especificar una *clave administrada por el cliente* que se usará para cifrar y descifrar todos los datos de discos administrados. 

En las secciones siguientes se describe cada una de las opciones de administración de claves con mayor detalle.

## <a name="platform-managed-keys"></a>Claves administradas por la plataforma

De forma predeterminada, los discos administrados usan claves de cifrado administradas por la plataforma. A partir del 10 de junio de 2017, todos los nuevos discos administrados, instantáneas e imágenes, así como los datos nuevos que se escriban en discos administrados existentes, se cifran automáticamente en reposo con claves administradas por la plataforma. 

## <a name="customer-managed-keys-public-preview"></a>Claves administradas por el cliente (versión preliminar pública)

Puede optar por administrar el cifrado en el nivel de cada disco administrado, con sus propias claves. El cifrado del lado servidor de discos administrados con claves administradas por el cliente ofrece una experiencia integrada con Azure Key Vault. Puede importar [las claves RSA](../articles/key-vault/key-vault-hsm-protected-keys.md) a su instancia de Key Vault o generar nuevas claves RSA en Azure Key Vault. Azure Managed Disks controla el cifrado y descifrado de forma totalmente transparente con [cifrado de sobre](../articles/storage/common/storage-client-side-encryption.md#encryption-via-the-envelope-technique). Cifra los datos con una clave de cifrado de datos (DEK) basada en [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) 256, que, a su vez, está protegida con las claves del usuario. Tiene que conceder acceso a los discos administrados en su instancia de Key Vault para usar sus propias claves para cifrar y descifrar la clave DEK. Esto le permite controlar completamente los datos y las claves. Puede deshabilitar las claves o revocar el acceso a los discos administrados en cualquier momento. También puede auditar el uso de la clave de cifrado con la supervisión de Azure Key Vault para asegurarse de que solo los discos administrados u otros servicios de Azure de confianza tengan acceso a las claves.

En el siguiente diagrama se muestra cómo los discos administrados utilizan Azure Active Directory y Azure Key Vault para realizar solicitudes con la clave administrada por el cliente:

![Flujo de trabajo de claves administradas por el cliente de discos administrados](media/disk-storage-encryption/customer-managed-keys-sse-managed-disks-workflow.png)


En la lista siguiente se explican los pasos numerados del diagrama:

1. Un administrador de Azure Key Vault crea recursos de almacén de claves.
1. El administrador del almacén de claves importa sus claves RSA a Key Vault o genera nuevas claves RSA en Key Vault.
1. Ese administrador crea una instancia del recurso Conjunto de cifrado de disco y especifica un identificador de Azure Key Vault y una dirección URL de clave. Conjunto de cifrado de disco es un nuevo recurso que se ha introducido para simplificar la administración de claves de los discos administrados. 
1. Cuando se crea un conjunto de cifrado de disco, se crea una [identidad administrada asignada por el sistema](../articles/active-directory/managed-identities-azure-resources/overview.md) en Azure Active Directory (AD) y se asocia al conjunto de cifrado de disco. 
1. El administrador de Azure Key Vault concede entonces al administrador de la identidad administrada permiso para realizar operaciones en el almacén de claves.
1. Un usuario de máquina virtual crea discos asociándolos con el conjunto de cifrado de disco. El usuario de la máquina virtual también puede habilitar el cifrado del lado servidor con claves administradas por el cliente para los recursos existentes asociándolos al conjunto de cifrado de disco. 
1. Los discos administrados usan la identidad administrada para enviar solicitudes a la instancia de Azure Key Vault.
1. Para leer o escribir datos, los discos administrados envían solicitudes a Azure Key Vault para cifrar (encapsular) y descifrar (desencapsular) la clave de cifrado de datos con el fin de realizar el cifrado y descifrado de los datos. 

Para revocar el acceso a las claves administradas por el cliente, vea [PowerShell de Azure Key Vault](https://docs.microsoft.com/powershell/module/azurerm.keyvault/) y [CLI de Azure Key Vault](https://docs.microsoft.com/cli/azure/keyvault). La revocación del acceso bloquea de manera eficaz el acceso a todos los datos de la cuenta de almacenamiento, ya que Azure Storage no puede acceder a la clave de cifrado.

### <a name="supported-scenarios-and-restrictions"></a>Escenarios y restricciones admitidos

En la versión preliminar, solo se admiten los siguientes escenarios:

- Cree una máquina virtual (VM) a partir de una imagen de Azure Marketplace y cifre el disco del sistema operativo con cifrado del lado servidor y claves administradas por el cliente.
- Cree una imagen personalizada cifrada con cifrado del lado servidor y claves administradas por el cliente.
- Cree una máquina virtual a partir de una imagen personalizada y cifre el disco del sistema operativo con cifrado del lado servidor y claves administradas por el cliente.
- Cree discos de datos cifrados con cifrado del lado servidor y claves administradas por el cliente.
- Cree instantáneas que estén cifradas con cifrado del lado servidor y claves administradas por el cliente.
- Cree conjuntos de escalado de máquinas virtuales que estén cifrados con cifrado del lado servidor y claves administradas por el cliente.

La versión preliminar tiene también las siguientes restricciones:

- Solo está disponible en la región Centro-oeste de EE. UU.
- Los discos creados a partir de imágenes personalizadas que están cifradas con cifrado del lado servidor y las claves administradas por el cliente deben cifrarse con las mismas claves administradas por el cliente y deben estar en la misma suscripción.
- Las instantáneas creadas a partir de discos que están cifrados con cifrado del lado servidor y claves administradas por el cliente deben cifrarse con las mismas claves administradas por el cliente.
- Las imágenes personalizadas cifradas con cifrado del lado servidor y claves administradas por el cliente no se pueden usar en la galería de imágenes compartidas.
- La instancia de Key Vault debe encontrarse en la misma región y suscripción que las calves administradas por el cliente.
- Los discos, las instantáneas y las imágenes cifrados con claves administradas por el cliente no se pueden trasladar a otra suscripción.

### <a name="setting-up-your-azure-key-vault"></a>Configuración de Azure Key Vault

1.  Cree una instancia de Azure Key Vault y la clave de cifrado.

    Al crear la instancia de Key Vault, debe habilitar la eliminación temporal y la protección de purgas. La eliminación temporal garantiza que la instancia de Key Vault conserva una clave eliminada durante un período de retención determinado (valor predeterminado de 90 días). La protección de purgas garantiza que una clave eliminada no se puede eliminar permanentemente hasta que transcurra el período de retención. Esta configuración le protege contra la pérdida de datos debido a la eliminación accidental. Estos valores son obligatorios cuando se usa una instancia de Key Vault para cifrar discos administrados.

    ```powershell
    $keyVault = New-AzKeyVault -Name myKeyVaultName ` 
    -ResourceGroupName myRGName ` 
    -Location westcentralus ` 
    -EnableSoftDelete ` 
    -EnablePurgeProtection 
     
    $key = Add-AzKeyVaultKey -VaultName $keyVault.VaultName ` 
    -Name myKeyName ` 
    -Destination Software `  
    ```

1.  Cree una instancia de un objeto DiskEncryptionSet. 
    
    ```powershell
    New-AzResourceGroupDeployment -ResourceGroupName myRGName ` 
      -TemplateUri "https://raw.githubusercontent.com/ramankumarlive/manageddiskscmkpreview/master/CreateDiskEncryptionSet.json" ` 
      -diskEncryptionSetName "myDiskEncryptionSet1" ` 
      -keyVaultId "/subscriptions/mySubscriptionId/resourceGroups/myRGName/providers/Microsoft.KeyVault/vaults/myKeyVaultName" ` 
      -keyVaultKeyUrl "https://myKeyVaultName.vault.azure.net/keys/myKeyName/403445136dee4a57af7068cab08f7d42" ` 
      -region "WestCentralUS"
    ```

1.  Conceda al recurso DiskEncryptionSet acceso al almacén de claves.

    ```powershell
    $identity = Get-AzADServicePrincipal -DisplayName myDiskEncryptionSet1  
     
    Set-AzKeyVaultAccessPolicy ` 
        -VaultName $keyVault.VaultName ` 
        -ObjectId $identity.Id ` 
        -PermissionsToKeys wrapkey,unwrapkey,get 
     
    New-AzRoleAssignment ` 
        -ObjectId $identity.Id ` 
        -RoleDefinitionName "Reader" ` 
        -ResourceName $keyVault.VaultName ` 
        -ResourceType "Microsoft.KeyVault/vaults" ` 
        -ResourceGroupName myRGName `  
    ```

### <a name="create-a-vm-using-a-marketplace-image-encrypting-the-os-and-data-disks-with-customer-managed-keys-via-a-resource-manager-template"></a>Creación de una máquina virtual con una imagen de Marketplace, cifrado del sistema operativo y de los discos de datos con claves administradas por el cliente a través de una plantilla de Resource Manager

```
$password=ConvertTo-SecureString -String "myVMPassword" `
  -AsPlainText -Force
New-AzResourceGroupDeployment -ResourceGroupName CMKTesting `
  -TemplateUri "https://raw.githubusercontent.com/ramankumarlive/manageddiskscmkpreview/master/CreateVMWithDisksEncryptedWithCMK.json" `
  -virtualMachineName "myVMName" `
  -adminPassword $password `
  -vmSize "Standard_DS3_V2" `
  -diskEncryptionSetId "/subscriptions/mySubscriptionId/resourceGroups/myRGName/providers/Microsoft.Compute/diskEncryptionSets/myDiskEncryptionSet1" `
  -region "westcentralus" 
```

### <a name="create-an-empty-disk-encrypted-using-server-side-encryption-with-customer-managed-keys-and-attach-it-to-a-vm"></a>Creación de un disco vacío cifrado con cifrado del lado servidor y claves administradas por el cliente y su conexión a una máquina virtual

```PowerShell
$vmName = "yourVMName"
$rgName = "yourRGName"
$diskName = "yourDiskName"
$diskSKU = "Premium_LRS"
$diskSizeinGiB = "30"
$diskEncryptionSetId = "/subscriptions/<subscriptionID>/resourceGroups/yourRGName/providers/Microsoft.Compute/diskEncryptionSets/<yourDiskEncryptionSetName>"
$region = "westcentralus"
$diskLUN = 1

New-AzResourceGroupDeployment -ResourceGroupName $rgName `
  -TemplateUri "https://raw.githubusercontent.com/ramankumarlive/manageddiskscmkpreview/master/CreateEmptyDataDiskEncryptedWithSSECMK.json" `
  -diskName $diskName `
  -diskSkuName $diskSKU `
  -dataDiskSizeInGb $diskSizeinGiB `
  -diskEncryptionSetId $diskEncryptionSetId `
  -region $region 

$vm = Get-AzVM -Name $vmName -ResourceGroupName $rgName 
$disk = Get-AzDisk -DiskName $diskName -ResourceGroupName $rgName
$vm = Add-AzVMDataDisk -VM $vm -Name $diskName -CreateOption Attach -ManagedDiskId $disk.Id -Lun 1
Update-AzVM -ResourceGroupName $rgName -VM $vm
```


> [!IMPORTANT]
> Las claves administradas por el cliente dependen de identidades administradas para los recursos de Azure, una característica de Azure Active Directory (Azure AD). Al configurar claves administradas por el cliente, se asigna automáticamente una identidad administrada a los recursos en segundo plano. Si posteriormente mueve la suscripción, el grupo de recursos o el disco administrado de un directorio de Azure AD a otro, la identidad administrada asociada a los discos administrados no se transfiere al nuevo inquilino, por lo que es posible que las claves administradas por el cliente dejen de funcionar. Para obtener más información, consulte [Transferencia de una suscripción entre directorios de Azure AD](../articles/active-directory/managed-identities-azure-resources/known-issues.md#transferring-a-subscription-between-azure-ad-directories).

## <a name="server-side-encryption-versus-azure-disk-encryption"></a>Cifrado del lado servidor frente a Azure Disk Encryption

[Azure Disk Encryption](../articles/security/fundamentals/azure-disk-encryption-vms-vmss.md) aprovecha la característica [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) de Windows y la característica [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) de Linux para cifrar los discos administrados con claves administradas por el cliente dentro de la máquina virtual invitada.  El cifrado del lado servidor con claves administradas por el cliente mejora en ADE al permitir el uso de cualquier tipo de sistema operativo y de imágenes para las máquinas virtuales mediante el cifrado de datos en el servicio Storage.

## <a name="next-steps"></a>Pasos siguientes

- [¿Qué es Azure Key Vault?](../articles/key-vault/key-vault-overview.md)
