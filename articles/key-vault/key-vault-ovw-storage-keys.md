---
ms.assetid: ''
title: 'Cuenta de almacenamiento administrado por Azure Key Vault: CLI'
description: Las claves de cuenta de almacenamiento proporcionan una integración sin problemas entre Azure Key Vault y el acceso basado en claves a la cuenta de Azure Storage.
ms.topic: conceptual
services: key-vault
ms.service: key-vault
author: prashanthyv
ms.author: pryerram
manager: mbaldwin
ms.date: 10/03/2018
ms.openlocfilehash: 152e1e5892e3a72286205c2f5bf4e18b2a2bcbf7
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55814850"
---
# <a name="azure-key-vault-managed-storage-account---cli"></a>Cuenta de almacenamiento administrado por Azure Key Vault: CLI

> [!NOTE]
> [Integración de Azure Storage con Azure Active Directory (Azure AD) está ahora en versión preliminar](https://docs.microsoft.com/azure/storage/common/storage-auth-aad). Se recomienda usar Azure AD para la autenticación y autorización, que proporciona acceso basado en tokens OAuth2 a Azure Storage, al igual que Azure Key Vault. Esto le permite:
> - Autenticar la aplicación cliente mediante una identidad de aplicación o usuario, en lugar de las credenciales de cuenta de almacenamiento. 
> - Usar una [identidad administrada de Azure AD](/azure/active-directory/managed-identities-azure-resources/) cuando se ejecuta en Azure. Las identidades administradas eliminan la necesidad de autenticación del cliente en conjunto y de almacenar las credenciales en la aplicación o con ella.
> - Utilice el control de acceso basado en rol (RBAC) para administrar la autorización, que también es compatible con Key Vault.

- Azure Key Vault administra las claves de una cuenta de Azure Storage (ASA).
    - Internamente, Azure Key Vault puede enumerar (sincronizar) las claves con una cuenta de Azure Storage.    
    - Azure Key Vault vuelve a generar (rotar) las claves periódicamente.
    - Los valores de clave nunca se devuelven como respuesta al autor de la llamada.
    - Azure Key Vault administra las claves de las cuentas de almacenamiento y de las cuentas de almacenamiento clásicas.
    
> [!IMPORTANT]
> Un inquilino de Azure AD proporciona a cada aplicación registrada una **[entidad de servicio](/azure/active-directory/develop/developer-glossary#service-principal-object)**, que actúa como la identidad de la aplicación. El identificador de aplicación de la entidad de servicio se usa cuando se le proporciona autorización para acceder a otros recursos de Azure, mediante control de acceso basado en rol (RBAC). Dado que Key Vault es una aplicación de Microsoft, está registrada previamente en todos los inquilinos de Azure AD con el mismo identificador de aplicación, dentro de cada nube de Azure:
> - Los inquilinos de Azure AD de la nube de Azure Government usan el identificador de aplicación `7e7c393b-45d0-48b1-a35e-2905ddf8183c`.
> - Los inquilinos de Azure AD de la nube pública de Azure y todos los demás usan el identificador de aplicación `cfa8b339-82a2-471a-a3c9-0fc0be7a4093`.

<a name="prerequisites"></a>Requisitos previos
--------------
1. [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) Instalación de la CLI de Azure   
2. [Creación de una cuenta de almacenamiento](https://azure.microsoft.com/services/storage/)
    - Siga los pasos de este [documento](https://docs.microsoft.com/azure/storage/) para crear una cuenta de almacenamiento  
    - **Instrucciones de nomenclatura:** Los nombres de las cuentas de almacenamiento deben tener entre 3 y 24 caracteres y solo pueden incluir números y letras en minúscula.        
      
<a name="step-by-step-instructions-on-how-to-use-key-vault-to-manage-storage-account-keys"></a>Instrucciones paso a paso sobre cómo usar Key Vault para administrar claves de cuenta de almacenamiento
--------------------------------------------------------------------------------
En las siguientes instrucciones, Key Vault se asignará como un servicio con permisos de operador en la cuenta de almacenamiento

> [!NOTE]
> Tenga en cuenta que, una vez que haya configurado cuentas de claves de almacenamiento administradas de Azure Key Vault, ya **NO** se pueden cambiar excepto con Key Vault. El almacenamiento administrado de las claves de cuentas implica que Key Vault administraría la rotación de la clave de la cuenta de almacenamiento

1. Después de crear una cuenta de almacenamiento, ejecute el siguiente comando para obtener el identificador de recurso de la cuenta de almacenamiento que desea administrar

    ```
    az storage account show -n storageaccountname 
    ```
    Copie el campo de identificador del resultado del comando anterior.
    
2. Para obtener el identificador del objeto de la entidad de servicio de Azure Key Vault, ejecute el siguiente comando:

    ```
    az ad sp show --id cfa8b339-82a2-471a-a3c9-0fc0be7a4093
    ```
    
    Después de que el comando haya finalizado correctamente, busque el identificador de objeto en el resultado.
    ```console
        {
            ...
            "objectId": "93c27d83-f79b-4cb2-8dd4-4aa716542e74"
            ...
        }
    ```
    
3. Asignación del rol Operador de claves de almacenamiento a la identidad de Azure Key Vault

    ```
    az role assignment create --role "Storage Account Key Operator Service Role"  --assignee-object-id <ObjectIdOfKeyVault> --scope <IdOfStorageAccount>
    ```
    
4. Cree una cuenta de almacenamiento administrada por Key Vault.     <br /><br />
   A continuación, se establece un período de regeneración de 90 días. Después de 90 días, Key Vault regenerará "key1" y cambiará la clave activa de "key2" a "key1". Ahora se marcará Key1 como la clave activa. 
   
    ```
    az keyvault storage add --vault-name <YourVaultName> -n <StorageAccountName> --active-key-name key1 --auto-regenerate-key --regeneration-period P90D --resource-id <Id-of-storage-account>
    ```
    En caso de que el usuario no haya creado la cuenta de almacenamiento y no tenga permisos para la cuenta de almacenamiento, los pasos siguientes establecen los permisos de la cuenta para asegurarse de que puede administrar todos los permisos de almacenamiento en el almacén de claves.
    
 > [!NOTE] 
 > En caso de que el usuario no tenga permisos para la cuenta de almacenamiento, primero debemos obtener el identificador de objeto del usuario


    ```
    az ad user show --upn-or-object-id "developer@contoso.com"

    az keyvault set-policy --name <YourVaultName> --object-id <ObjectId> --storage-permissions backup delete list regeneratekey recover     purge restore set setsas update
    
    ```
    
## <a name="how-to-access-your-storage-account-with-sas-tokens"></a>Acceso a la cuenta de almacenamiento con tokens de SAS

En esta sección, analizaremos cómo puede llevar a cabo operaciones en la cuenta de almacenamiento mediante la captura de [tokens de SAS](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) desde Key Vault.

En la sección siguiente, mostraremos cómo capturar la clave de la cuenta de almacenamiento que se almacena en Key Vault y la usaremos para crear una definición de SAS (firma de acceso compartido) para la cuenta de almacenamiento.

> [!NOTE] 
  Existen tres maneras de autenticarse en Key Vault, como puede leer en los [conceptos básicos](key-vault-whatis.md#basic-concepts).
> - Mediante Managed Service Identity (altamente recomendado)
> - Mediante la entidad de servicio y el certificado 
> - Mediante la entidad de servicio y la contraseña (NO recomendado)

```cs
// Once you have a security token from one of the above methods, then create KeyVaultClient with vault credentials
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(securityToken));

// Get a SAS token for our storage from Key Vault. SecretUri is of the format https://<VaultName>.vault.azure.net/secrets/<ExamplePassword>
var sasToken = await kv.GetSecretAsync("SecretUri");

// Create new storage credentials using the SAS token.
var accountSasCredential = new StorageCredentials(sasToken.Value);

// Use the storage credentials and the Blob storage endpoint to create a new Blob service client.
var accountWithSas = new CloudStorageAccount(accountSasCredential, new Uri ("https://myaccount.blob.core.windows.net/"), null, null, null);

var blobClientWithSas = accountWithSas.CreateCloudBlobClient();
```

Si el token de SAS está a punto de expirar, debería volver a capturar el token de SAS desde Key Vault y actualizar el código.

```cs
// If your SAS token is about to expire, get the SAS Token again from Key Vault and update it.
sasToken = await kv.GetSecretAsync("SecretUri");
accountSasCredential.UpdateSASToken(sasToken);
```


### <a name="relavant-azure-cli-cmdlets"></a>Cmdlets de la CLI de Azure pertinentes
[Cmdlets de almacenamiento de la CLI de Azure](https://docs.microsoft.com/cli/azure/keyvault/storage?view=azure-cli-latest)

### <a name="relevant-powershell-cmdlets"></a>Cmdlets de PowerShell relevantes

- [Get-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.keyvault/get-azurekeyvaultmanagedstorageaccount)
- [Add-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Add-AzureKeyVaultManagedStorageAccount)
- [Get-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Get-AzureKeyVaultManagedStorageSasDefinition)
- [Update-AzureKeyVaultManagedStorageAccountKey](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Update-AzureKeyVaultManagedStorageAccountKey)
- [Remove-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.keyvault/remove-azurekeyvaultmanagedstorageaccount)
- [Remove-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Remove-AzureKeyVaultManagedStorageSasDefinition)
- [Set-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Set-AzureKeyVaultManagedStorageSasDefinition)

## <a name="see-also"></a>Otras referencias

- [Información acerca de claves, secretos y certificados](https://docs.microsoft.com/rest/api/keyvault/)
- [Blog del equipo de Key Vault](https://blogs.technet.microsoft.com/kv/)
