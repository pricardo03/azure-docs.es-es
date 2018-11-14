---
ms.assetid: ''
title: Claves de cuenta de almacenamiento de Azure Key Vault
description: Las claves de cuenta de almacenamiento proporcionan una integración sin problemas entre Azure Key Vault y el acceso basado en claves a la cuenta de Azure Storage.
ms.topic: conceptual
services: key-vault
ms.service: key-vault
author: bryanla
ms.author: bryanla
manager: mbaldwin
ms.date: 10/03/2018
ms.openlocfilehash: f3f310c247aea3842b5ec7a9a1409032d5bdc0bf
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "50912924"
---
# <a name="azure-key-vault-storage-account-keys"></a>Claves de cuenta de almacenamiento de Azure Key Vault

> [!NOTE]
> [Azure Storage ahora admite la autorización de AAD](https://docs.microsoft.com/azure/storage/common/storage-auth-aad). Se recomienda usar Azure Active Directory para la autenticación y autorización en el almacenamiento, ya que los usuarios no tienen que preocuparse por la rotación de claves de la cuenta de Azure Storage.

- Azure Key Vault administra las claves de una cuenta de Azure Storage (ASA).
    - Internamente, Azure Key Vault puede enumerar (sincronizar) las claves con una cuenta de Azure Storage.    
    - Azure Key Vault vuelve a generar (rotar) las claves periódicamente.
    - Los valores de clave nunca se devuelven como respuesta al autor de la llamada.
    - Azure Key Vault administra las claves de las cuentas de almacenamiento y de las cuentas de almacenamiento clásicas.

<a name="prerequisites"></a>Requisitos previos
--------------
1. [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) Instalación de la CLI de Azure   
2. [Creación de una cuenta de almacenamiento](https://azure.microsoft.com/services/storage/)
    - Siga los pasos de este [documento](https://docs.microsoft.com/azure/storage/) para crear una cuenta de almacenamiento  
    - **Guía de nomenclatura:** los nombres de las cuentas de almacenamiento deben tener entre 3 y 24 caracteres y solo pueden incluir números y letras en minúscula.        
      
<a name="step-by-step-instructions-on-how-to-use-key-vault-to-manage-storage-account-keys"></a>Instrucciones paso a paso sobre cómo usar Key Vault para administrar claves de cuenta de almacenamiento
--------------------------------------------------------------------------------
En las siguientes instrucciones, Key Vault se asignará como un servicio con permisos de operador en la cuenta de almacenamiento

> [!NOTE]
> Tenga en cuenta que, una vez que haya configurado cuentas de claves de almacenamiento administradas de Azure Key Vault, ya **NO** se pueden cambiar excepto con Key Vault. El almacenamiento administrado de las claves de cuentas implica que Key Vault administraría la rotación de la clave de la cuenta de almacenamiento

1. Después de crear una cuenta de almacenamiento, ejecute el siguiente comando para obtener el identificador de recurso de la cuenta de almacenamiento que desea administrar

    ```
    az storage account show -n storageaccountname (Copy ID out of the result of this command)
    ```
    
2. Obtención del identificador de aplicación de la entidad de servicio de Azure Key Vault 

    ```
    az ad sp show --id cfa8b339-82a2-471a-a3c9-0fc0be7a4093
    ```
    
3. Asignación del rol Operador de claves de almacenamiento a la identidad de Azure Key Vault

    ```
    az role assignment create --role "Storage Account Key Operator Service Role"  --assignee-object-id hhjkh --scope idofthestorageaccount
    ```
    
4. Cree una cuenta de almacenamiento administrada por Key Vault.     <br /><br />
   A continuación, se establece un período de regeneración de 90 días. Después de 90 días, Key Vault regenerará "key1" y cambiará la clave activa de "key2" a "key1".
   
    ```
    az keyvault storage add --vault-name <YourVaultName> -n <StorageAccountName> --active-key-name key2 --auto-regenerate-key --regeneration-period P90D --resource-id <Resource-id-of-storage-account>
    ```
    En caso de que el usuario no haya creado la cuenta de almacenamiento y no tenga permisos para la cuenta de almacenamiento, los pasos siguientes establecen los permisos de la cuenta para asegurarse de que puede administrar todos los permisos de almacenamiento en el almacén de claves.
 > [!NOTE] 
    En caso de que el usuario no tenga permisos para la cuenta de almacenamiento, primero debemos obtener el identificador de objeto del usuario

    ```
    az ad user show --upn-or-object-id "developer@contoso.com"

    az keyvault set-policy --name <YourVaultName> --object-id <ObjectId> --storage-permissions backup delete list regeneratekey recover     purge restore set setsas update
    ```
### <a name="relavant-azure-cli-cmdlets"></a>Cmdlets de la CLI de Azure pertinentes
- [Cmdlets de almacenamiento de la CLI de Azure](https://docs.microsoft.com/cli/azure/keyvault/storage?view=azure-cli-latest)

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
