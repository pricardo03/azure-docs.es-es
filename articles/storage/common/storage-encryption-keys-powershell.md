---
title: Configuración de las claves administradas por el cliente para el cifrado de Azure Storage desde PowerShell
description: Configurar las claves que administra el cliente para el cifrado de Azure Storage desde PowerShell Las claves administradas del cliente le permiten crear, rotar, deshabilitar y revocar los controles de acceso.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/16/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: be876b370cd476bee2af7d90a9f0433fd80de3b4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65233678"
---
# <a name="configure-customer-managed-keys-for-azure-storage-encryption-from-powershell"></a>Configuración de las claves administradas por el cliente para el cifrado de Azure Storage desde PowerShell

[!INCLUDE [storage-encryption-configure-keys-include](../../../includes/storage-encryption-configure-keys-include.md)]

En este artículo se muestra cómo configurar un almacén de claves con claves que administra el cliente mediante PowerShell.

## <a name="assign-an-identity-to-the-storage-account"></a>Asignación de una identidad a la cuenta de almacenamiento

Para habilitar claves que administra el cliente para la cuenta de almacenamiento, debe asignar primero una identidad administrada que haya asignado el sistema a la cuenta de almacenamiento. Usará esta identidad administrada para conceder los permisos de la cuenta de almacenamiento para obtener acceso al almacén de claves.

Para asignar una identidad administrada con PowerShell, llame a [Set-AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount). No olvide reemplazar los valores del marcador de posición entre corchetes con sus propios valores.

```powershell
$storageAccount = Set-AzStorageAccount -ResourceGroupName <resource_group> `
    -Name <storage-account> `
    -AssignIdentity
```

Para obtener información sobre cómo configurar identidades administradas que el sistema ha asignado con PowerShell, consulte [Configurar identidades administradas para recursos de Azure en una máquina virtual de Azure con PowerShell](../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md).

## <a name="create-a-new-key-vault"></a>Creación de un almacén de claves nuevo

Para crear un almacén de claves nuevo con PowerShell, llame a [New-AzKeyVault](/powershell/module/az.keyvault/new-azkeyvault). El almacén de claves que se usará para almacenar las claves que administre el cliente para el cifrado de Azure Storage debe tener dos configuraciones de protección de claves habilitadas: la **eliminación temporal** y la opción de **no purgar**. 

No olvide reemplazar los valores del marcador de posición entre corchetes con sus propios valores. 

```powershell
$keyVault = New-AzKeyVault -Name <key-vault> `
    -ResourceGroupName <resource_group> `
    -Location <location> `
    -EnableSoftDelete `
    -EnablePurgeProtection
```

## <a name="configure-the-key-vault-access-policy"></a>Configuración de la directiva de acceso del almacén de claves

A continuación, configure la directiva de acceso del almacén de claves, para que la cuenta de almacenamiento tenga permiso para acceder a él. En este paso, usará la identidad administrada que asignó previamente a la cuenta de almacenamiento.

Para establecer la directiva de acceso del almacén de claves, llame a [Set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy). Recuerde reemplazar los valores de marcador de posición entre corchetes por sus propios valores y usar las variables definidas en los ejemplos anteriores.

```powershell
Set-AzKeyVaultAccessPolicy `
    -VaultName $keyVault.VaultName `
    -ObjectId $storageAccount.Identity.PrincipalId `
    -PermissionsToKeys wrapkey,unwrapkey,get,recover
```

## <a name="create-a-new-key"></a>Creación de una nueva clave

Después, cree una clave nueva en el almacén de claves. Para crear una clave nueva, llame a [Add-AzKeyVaultKey](/powershell/module/az.keyvault/add-azkeyvaultkey). Recuerde reemplazar los valores de marcador de posición entre corchetes por sus propios valores y usar las variables definidas en los ejemplos anteriores.

```powershell
$key = Add-AzKeyVaultKey -VaultName $keyVault.VaultName -Name <key> -Destination 'Software'
```

## <a name="configure-encryption-with-customer-managed-keys"></a>Configuración del cifrado con claves que administra el cliente

De forma predeterminada, el cifrado de Azure Storage usa claves que administra Microsoft. En este paso, configure la cuenta de Azure Storage para usar las claves que administra el cliente y especifique la clave para asociar a la cuenta de almacenamiento.

Para actualizar la configuración de cifrado de la cuenta de almacenamiento, llame a [Set-AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount). Recuerde reemplazar los valores de marcador de posición entre corchetes por sus propios valores y usar las variables definidas en los ejemplos anteriores.

```powershell
Set-AzStorageAccount -ResourceGroupName $storageAccount.ResourceGroupName `
    -AccountName $storageAccount.StorageAccountName `
    -KeyvaultEncryption `
    -KeyName $key.Name `
    -KeyVersion $key.Version `
    -KeyVaultUri $keyVault.VaultUri
```

## <a name="update-the-key-version"></a>Actualización de la versión de la clave

Al crear una nueva versión de una clave, tendrá que actualizar la cuenta de almacenamiento para que utilice la versión nueva. En primer lugar, llame a [Get-AzKeyVaultKey](/powershell/module/az.keyvault/get-azkeyvaultkey) para obtener la versión más reciente de la clave. A continuación, llame a [Set-AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount) para actualizar la configuración de cifrado de la cuenta de almacenamiento y así poder usar la nueva versión de la clave, tal como se muestra en la sección anterior.

## <a name="next-steps"></a>Pasos siguientes

- [Cifrado de Azure Storage para datos en reposo](storage-service-encryption.md) 
- [¿Qué es Azure Key Vault?](https://docs.microsoft.com/azure/key-vault/key-vault-whatis)
