---
author: orspod
ms.service: data-explorer
ms.topic: include
ms.date: 01/07/2020
ms.author: orspodek
ms.openlocfilehash: 0d78e48fead7b1f53e67860e6be8fe6d77469e87
ms.sourcegitcommit: d9ec6e731e7508d02850c9e05d98d26c4b6f13e6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2020
ms.locfileid: "76280615"
---
Azure Data Explorer cifra todos los datos en una cuenta de almacenamiento en reposo. De manera predeterminada, los datos se cifran con claves administradas por Microsoft. Para tener un mayor control sobre las claves de cifrado, puede proporcionar claves administradas por el cliente para el cifrado de datos. Las claves administradas por el cliente se deben almacenar en una instancia de [Azure Key Vault](/azure/key-vault/key-vault-overview). Puede crear sus propias claves y almacenarlas en un almacén de claves, o puede usar una API de Azure Key Vault para generarlas. El clúster de Azure Data Explorer y el almacén de claves deben estar en la misma región, pero pueden estar en distintas suscripciones. Para obtener una explicación detallada sobre las claves administradas por el cliente, consulte [Claves administradas por el cliente con Azure Key Vault](/azure/storage/common/storage-service-encryption) En este artículo se muestra cómo configurar las claves administradas por el cliente.

Para configurar las claves administradas por el cliente con Azure Data Explorer, debe [establecer dos propiedades en el almacén de claves](/azure/key-vault/key-vault-ovw-soft-delete): **Eliminación temporal** y **No purgar**. Estas propiedades no están habilitadas de forma predeterminada. Para habilitarlas, use [PowerShell](/azure/key-vault/key-vault-soft-delete-powershell) o la [CLI de Azure](/azure/key-vault/key-vault-soft-delete-cli). Solo se admiten claves RSA y el tamaño de clave 2048.

> [!NOTE]
> El cifrado de datos con claves administradas por el cliente no se admite en los [clústeres iniciales y seguidores](/azure/data-explorer/follower). 

## <a name="assign-an-identity-to-the-cluster"></a>Asignación de una identidad al clúster

Para habilitar claves administradas por el cliente para el clúster, primero asigne una identidad administrada que haya asignado el sistema al clúster. Usará esta identidad administrada para conceder los permisos del clúster para obtener acceso al almacén de claves. Para configurar identidades administradas asignadas por el sistema, consulte [Identidades administradas](/azure/data-explorer/managed-identities).

## <a name="create-a-new-key-vault"></a>Creación de un almacén de claves nuevo

Para crear un almacén de claves nuevo con PowerShell, llame a [New-AzKeyVault](/powershell/module/az.keyvault/new-azkeyvault). El almacén de claves que usa para almacenar las claves administradas por el cliente para el cifrado de Azure Data Explorer debe tener dos configuraciones de protección de claves habilitadas: **Eliminación temporal** y **Do Not Purge** (No purgar). Reemplace los valores del marcador de posición entre corchetes con sus propios valores en el ejemplo siguiente:

```azurepowershell-interactive
$keyVault = New-AzKeyVault -Name <key-vault> `
    -ResourceGroupName <resource_group> `
    -Location <location> `
    -EnableSoftDelete `
    -EnablePurgeProtection
```

## <a name="configure-the-key-vault-access-policy"></a>Configuración de la directiva de acceso del almacén de claves

A continuación, configure la directiva de acceso del almacén de claves para que el clúster tenga permisos de acceso. En este paso, usará la identidad administrada asignada por el sistema que asignó previamente al clúster. Para establecer la directiva de acceso del almacén de claves, llame a [Set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy). Reemplace los valores del marcador de posición entre corchetes por sus propios valores y use las variables definidas en los ejemplos anteriores.

```azurepowershell-interactive
Set-AzKeyVaultAccessPolicy `
    -VaultName $keyVault.VaultName `
    -ObjectId $cluster.Identity.PrincipalId `
    -PermissionsToKeys wrapkey,unwrapkey,get,recover
```

## <a name="create-a-new-key"></a>Creación de una nueva clave

Después, cree una clave nueva en el almacén de claves. Para crear una clave nueva, llame a [Add-AzKeyVaultKey](/powershell/module/az.keyvault/add-azkeyvaultkey). Reemplace los valores del marcador de posición entre corchetes por sus propios valores y use las variables definidas en los ejemplos anteriores.

```azurepowershell-interactive
$key = Add-AzKeyVaultKey -VaultName $keyVault.VaultName -Name <key> -Destination 'Software'
```
