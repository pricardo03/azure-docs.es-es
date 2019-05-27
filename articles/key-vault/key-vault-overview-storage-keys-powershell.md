---
title: 'Cuenta de almacenamiento administrado por Azure Key Vault: versión de PowerShell'
description: La característica de cuenta de almacenamiento administrado ofrece una integración sin problemas entre Azure Key Vault y una cuenta de almacenamiento de Azure.
ms.topic: conceptual
ms.service: key-vault
author: msmbaldwin
ms.author: mbaldwin
manager: barbkess
ms.date: 03/01/2019
ms.openlocfilehash: 9b6089aa828b5667f100c1a8cbff3e69345e4512
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "66150420"
---
# <a name="azure-key-vault-managed-storage-account---powershell"></a>Cuenta de almacenamiento administrado por Azure Key Vault: PowerShell

> [!NOTE]
> [Integración de Azure Storage con Azure Active Directory (Azure AD) está ahora en versión preliminar](https://docs.microsoft.com/azure/storage/common/storage-auth-aad). Se recomienda usar Azure AD para la autenticación y autorización, que proporciona acceso basado en tokens OAuth2 a Azure Storage, al igual que Azure Key Vault. Esto le permite:
> - Autenticar la aplicación cliente mediante una identidad de aplicación o usuario, en lugar de las credenciales de cuenta de almacenamiento. 
> - Usar una [identidad administrada de Azure AD](/azure/active-directory/managed-identities-azure-resources/) cuando se ejecuta en Azure. Las identidades administradas eliminan la necesidad de autenticación del cliente en conjunto y de almacenar las credenciales en la aplicación o con ella.
> - Utilice el control de acceso basado en rol (RBAC) para administrar la autorización, que también es compatible con Key Vault.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Una [cuenta de Azure Storage](/azure/storage/storage-create-storage-account) utiliza una credencial que consta de un nombre de cuenta y una clave. La clave se genera automáticamente y actúa más como una "contraseña " que como una clave criptográfica. Key Vault puede administrar estas claves de cuenta de almacenamiento guardándolas como [secretos de Key Vault](/azure/key-vault/about-keys-secrets-and-certificates#key-vault-secrets). 

## <a name="overview"></a>Información general

La característica de cuenta de almacenamiento administrada de Key Vault realiza varias funciones de administración en su nombre:

- Muestra las claves (sincronización) con una cuenta de Azure Storage.
- Vuelve a generar (rotar) las claves periódicamente.
- Administra las claves de las cuentas de almacenamiento y de las cuentas de almacenamiento clásicas.
- Los valores de clave nunca se devuelven como respuesta al autor de la llamada.

Cuando use la característica de clave de cuenta de almacenamiento administrada:

- **Permita que solo Key Vault administre las claves de cuenta de almacenamiento**. No intente administrarlas usted mismo, ya que interferirá con los procesos de Key Vault.
- **No permita que más de un objeto de Key Vault administre las claves de cuenta de almacenamiento**.
- **No regenere manualmente las claves de cuenta de almacenamiento**. Se recomienda que las regenere a través de Key Vault.

El siguiente ejemplo muestra cómo permitir que Key Vault administre las claves de su cuenta de almacenamiento.

## <a name="authorize-key-vault-to-access-to-your-storage-account"></a>Autorice a Key Vault el acceso a la cuenta de almacenamiento

> [!IMPORTANT]
> Un inquilino de Azure AD proporciona a cada aplicación registrada una **[entidad de servicio](/azure/active-directory/develop/developer-glossary#service-principal-object)**, que actúa como la identidad de la aplicación. El identificador de aplicación de la entidad de servicio se usa cuando se le proporciona autorización para acceder a otros recursos de Azure, mediante control de acceso basado en rol (RBAC). Dado que Key Vault es una aplicación de Microsoft, está registrada previamente en todos los inquilinos de Azure AD con el mismo identificador de aplicación, dentro de cada nube de Azure:
> - Los inquilinos de Azure AD de la nube de Azure Government usan el identificador de aplicación `7e7c393b-45d0-48b1-a35e-2905ddf8183c`.
> - Los inquilinos de Azure AD de la nube pública de Azure y todos los demás usan el identificador de aplicación `cfa8b339-82a2-471a-a3c9-0fc0be7a4093`.

Para que Key Vault tenga acceso y pueda administrar las claves de cuenta de almacenamiento, debe autorizar su acceso a la cuenta de almacenamiento. La aplicación Key Vault necesita permisos para *mostrar* y *regenerar* las claves de la cuenta de almacenamiento. Estos permisos se habilitan a través del rol RBAC integrado [Rol de servicio de operador de claves de cuentas de almacenamiento](/azure/role-based-access-control/built-in-roles#storage-account-key-operator-service-role). 

Para asignar este rol a la entidad de servicio de Key Vault, que limita el ámbito a la cuenta de almacenamiento, siga estos pasos. Asegúrese de actualizar las variables `$resourceGroupName`, `$storageAccountName`, `$storageAccountKey` y `$keyVaultName` antes de ejecutar el script:

```azurepowershell-interactive
# TODO: Update with the resource group where your storage account resides, your storage account name, the name of your active storage account key, and your Key Vault instance name
$resourceGroupName = "rgContoso"
$storageAccountName = "sacontoso"
$storageAccountKey = "key1"
$keyVaultName = "kvContoso"
$keyVaultSpAppId = "cfa8b339-82a2-471a-a3c9-0fc0be7a4093" # See "IMPORTANT" block above for information on Key Vault Application IDs

# Authenticate your PowerShell session with Azure AD, for use with Azure Resource Manager cmdlets
$azureProfile = Connect-AzAccount

# Get a reference to your Azure storage account
$storageAccount = Get-AzStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $storageAccountName

# Assign RBAC role "Storage Account Key Operator Service Role" to Key Vault, limiting the access scope to your storage account. For a classic storage account, use "Classic Storage Account Key Operator Service Role." 
New-AzRoleAssignment -ApplicationId $keyVaultSpAppId -RoleDefinitionName 'Storage Account Key Operator Service Role' -Scope $storageAccount.Id
```

Tras la correcta asignación del rol, debería ver una salida similar a la del siguiente ejemplo:

```console
RoleAssignmentId   : /subscriptions/03f0blll-ce69-483a-a092-d06ea46dfb8z/resourceGroups/rgContoso/providers/Microsoft.Storage/storageAccounts/sacontoso/providers/Microsoft.Authorization/roleAssignments/189cblll-12fb-406e-8699-4eef8b2b9ecz
Scope              : /subscriptions/03f0blll-ce69-483a-a092-d06ea46dfb8z/resourceGroups/rgContoso/providers/Microsoft.Storage/storageAccounts/sacontoso
DisplayName        : Azure Key Vault
SignInName         :
RoleDefinitionName : storage account Key Operator Service Role
RoleDefinitionId   : 81a9662b-bebf-436f-a333-f67b29880f12
ObjectId           : 93c27d83-f79b-4cb2-8dd4-4aa716542e74
ObjectType         : ServicePrincipal
CanDelegate        : False
```

Si Key Vault ya se ha agregado al rol en la cuenta de almacenamiento, recibirá un error *"La asignación de roles ya existe."* . También puede comprobar la asignación de roles con la página "Control de acceso (IAM)" de la cuenta de almacenamiento en Azure Portal.  

## <a name="give-your-user-account-permission-to-managed-storage-accounts"></a>Concesión del permiso de cuenta de usuario a las cuentas de almacenamiento administradas

>[!TIP] 
> Del mismo modo que Azure AD proporciona una **entidad de servicio** para la identidad de una aplicación, se proporciona una **entidad de seguridad de usuario** para la identidad de un usuario. La entidad de seguridad de usuario puede recibir luego autorización de acceso a Key Vault a través de los permisos de la directiva de acceso de Key Vault.

Con la misma sesión de PowerShell, actualice la directiva de acceso de Key Vault para las cuentas de almacenamiento administradas. Este paso aplica los permisos de la cuenta de almacenamiento a su cuenta de usuario, lo que garantiza que tiene acceso a las características de la cuenta de almacenamiento administrada: 

```azurepowershell-interactive
# Give your user principal access to all storage account permissions, on your Key Vault instance

Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -UserPrincipalName $azureProfile.Context.Account.Id -PermissionsToStorage get, list, listsas, delete, set, update, regeneratekey, recover, backup, restore, purge
```

Tenga en cuenta que los permisos para las cuentas de almacenamiento no están disponibles en la página "Directivas de acceso" de la cuenta de almacenamiento en Azure Portal.

## <a name="add-a-managed-storage-account-to-your-key-vault-instance"></a>Adición de una cuenta de almacenamiento administrada a la instancia de Key Vault

Con la misma sesión de PowerShell, cree una cuenta de almacenamiento administrada en la instancia de Key Vault. El modificador `-DisableAutoRegenerateKey` especifica que no se deben regenerar las claves de la cuenta de almacenamiento.

```azurepowershell-interactive
# Add your storage account to your Key Vault's managed storage accounts
Add-AzKeyVaultManagedStorageAccount -VaultName $keyVaultName -AccountName $storageAccountName -AccountResourceId $storageAccount.Id -ActiveKeyName $storageAccountKey -DisableAutoRegenerateKey
```

Tras la correcta incorporación de la cuenta de almacenamiento sin regeneración de claves, debería ver una salida similar a la del siguiente ejemplo:

```console
Id                  : https://kvcontoso.vault.azure.net:443/storage/sacontoso
Vault Name          : kvcontoso
AccountName         : sacontoso
Account Resource Id : /subscriptions/03f0blll-ce69-483a-a092-d06ea46dfb8z/resourceGroups/rgContoso/providers/Microsoft.Storage/storageAccounts/sacontoso
Active Key Name     : key1
Auto Regenerate Key : False
Regeneration Period : 90.00:00:00
Enabled             : True
Created             : 11/19/2018 11:54:47 PM
Updated             : 11/19/2018 11:54:47 PM
Tags                : 
```

### <a name="enable-key-regeneration"></a>Habilitación de la regeneración de clave

Si quiere que Key Vault regenere las claves de cuenta de almacenamiento periódicamente, puede establecer un período de regeneración. En el ejemplo siguiente se establece un período de regeneración de tres días. Después de tres días, Key Vault regenerará "key1" y cambiará la clave activa de "key2" a "key1".

```azurepowershell-interactive
$regenPeriod = [System.Timespan]::FromDays(3)
Add-AzKeyVaultManagedStorageAccount -VaultName $keyVaultName -AccountName $storageAccountName -AccountResourceId $storageAccount.Id -ActiveKeyName $storageAccountKey -RegenerationPeriod $regenPeriod
```

Tras la correcta incorporación de la cuenta de almacenamiento con regeneración de claves, debería ver una salida similar a la del siguiente ejemplo:

```console
Id                  : https://kvcontoso.vault.azure.net:443/storage/sacontoso
Vault Name          : kvcontoso
AccountName         : sacontoso
Account Resource Id : /subscriptions/03f0blll-ce69-483a-a092-d06ea46dfb8z/resourceGroups/rgContoso/providers/Microsoft.Storage/storageAccounts/sacontoso
Active Key Name     : key1
Auto Regenerate Key : True
Regeneration Period : 3.00:00:00
Enabled             : True
Created             : 11/19/2018 11:54:47 PM
Updated             : 11/19/2018 11:54:47 PM
Tags                : 
```

## <a name="next-steps"></a>Pasos siguientes

- [Ejemplos de clave de cuenta de almacenamiento administrada](https://github.com/Azure-Samples?utf8=%E2%9C%93&q=key+vault+storage&type=&language=)
- [Información acerca de claves, secretos y certificados](about-keys-secrets-and-certificates.md)
- [Referencia de PowerShell para Key Vault](/powershell/module/az.keyvault/?view=azps-1.2.0#key_vault)
