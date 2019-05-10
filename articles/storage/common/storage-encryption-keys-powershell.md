---
title: Configurar las claves administradas por el cliente para el cifrado de almacenamiento de Azure desde PowerShell
description: Obtenga información sobre cómo usar PowerShell para configurar las claves administradas por el cliente para el cifrado de almacenamiento de Azure. Claves administradas por el cliente permiten crear, girar, deshabilitar y revocar el acceso a los controles.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/16/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: be876b370cd476bee2af7d90a9f0433fd80de3b4
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2019
ms.locfileid: "65233678"
---
# <a name="configure-customer-managed-keys-for-azure-storage-encryption-from-powershell"></a>Configurar las claves administradas por el cliente para el cifrado de almacenamiento de Azure desde PowerShell

[!INCLUDE [storage-encryption-configure-keys-include](../../../includes/storage-encryption-configure-keys-include.md)]

En este artículo se muestra cómo configurar un almacén de claves con claves administradas por el cliente con PowerShell.

## <a name="assign-an-identity-to-the-storage-account"></a>Asignar una identidad para la cuenta de almacenamiento

Para habilitar claves administradas por el cliente para la cuenta de almacenamiento, primero asignar una identidad administrada asignado por el sistema a la cuenta de almacenamiento. Usará esta identidad administrada para conceder los permisos de cuenta de almacenamiento para acceder al almacén de claves.

Para asignar una identidad administrada con PowerShell, llame al [conjunto AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount). No olvide reemplazar los valores de marcador de posición entre corchetes por los suyos propios.

```powershell
$storageAccount = Set-AzStorageAccount -ResourceGroupName <resource_group> `
    -Name <storage-account> `
    -AssignIdentity
```

Para obtener más información sobre cómo configurar asignado por el sistema de identidades administradas con PowerShell, consulte [configurar administradas las identidades de los recursos de Azure en una máquina virtual de Azure con PowerShell](../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md).

## <a name="create-a-new-key-vault"></a>Crear un nuevo almacén de claves

Para crear un nuevo almacén de claves con PowerShell, llame a [New AzKeyVault](/powershell/module/az.keyvault/new-azkeyvault). El almacén de claves que se usará para almacenar las claves administradas por el cliente para el cifrado de almacenamiento de Azure debe tener dos configuraciones de protección de claves habilitadas, **eliminación temporal** y **no purgar**. 

No olvide reemplazar los valores de marcador de posición entre corchetes por los suyos propios. 

```powershell
$keyVault = New-AzKeyVault -Name <key-vault> `
    -ResourceGroupName <resource_group> `
    -Location <location> `
    -EnableSoftDelete `
    -EnablePurgeProtection
```

## <a name="configure-the-key-vault-access-policy"></a>Configurar la directiva de acceso del almacén de claves

A continuación, configure la directiva de acceso para el almacén de claves para que la cuenta de almacenamiento tenga permisos para acceder a él. En este paso, usará la identidad administrada que asignó previamente a la cuenta de almacenamiento.

Para establecer la directiva de acceso para el almacén de claves, llame a [conjunto AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy). Recuerde reemplazar los valores de marcador de posición entre corchetes por los suyos propios y para usar las variables definidas en los ejemplos anteriores.

```powershell
Set-AzKeyVaultAccessPolicy `
    -VaultName $keyVault.VaultName `
    -ObjectId $storageAccount.Identity.PrincipalId `
    -PermissionsToKeys wrapkey,unwrapkey,get,recover
```

## <a name="create-a-new-key"></a>Crear una nueva clave

A continuación, cree una nueva clave en el almacén de claves. Para crear una nueva clave, llame a [agregar AzKeyVaultKey](/powershell/module/az.keyvault/add-azkeyvaultkey). Recuerde reemplazar los valores de marcador de posición entre corchetes por los suyos propios y para usar las variables definidas en los ejemplos anteriores.

```powershell
$key = Add-AzKeyVaultKey -VaultName $keyVault.VaultName -Name <key> -Destination 'Software'
```

## <a name="configure-encryption-with-customer-managed-keys"></a>Configurar el cifrado con claves administradas por el cliente

De forma predeterminada, el cifrado de almacenamiento de Azure usa claves administradas por Microsoft. En este paso, configure la cuenta de almacenamiento de Azure para usar claves administradas por el cliente y especifique la clave para asociar la cuenta de almacenamiento.

Llame a [conjunto AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount) para actualizar la configuración de cifrado de la cuenta de almacenamiento. Recuerde reemplazar los valores de marcador de posición entre corchetes por los suyos propios y para usar las variables definidas en los ejemplos anteriores.

```powershell
Set-AzStorageAccount -ResourceGroupName $storageAccount.ResourceGroupName `
    -AccountName $storageAccount.StorageAccountName `
    -KeyvaultEncryption `
    -KeyName $key.Name `
    -KeyVersion $key.Version `
    -KeyVaultUri $keyVault.VaultUri
```

## <a name="update-the-key-version"></a>Actualizar la versión de la clave

Cuando se crea una nueva versión de una clave, deberá actualizar la cuenta de almacenamiento para usar la nueva versión. En primer lugar, llame a [Get AzKeyVaultKey](/powershell/module/az.keyvault/get-azkeyvaultkey) para obtener la versión más reciente de la clave. A continuación, llame a [conjunto AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount) para actualizar la configuración de cifrado de la cuenta de almacenamiento para usar la nueva versión de la clave, como se muestra en la sección anterior.

## <a name="next-steps"></a>Pasos siguientes

- [Cifrado de Azure Storage para datos en reposo](storage-service-encryption.md) 
- [¿Qué es Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis)?
