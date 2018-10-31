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
ms.openlocfilehash: adc8b84f0f22e85de88c4bd80c10a2a35d7b490a
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2018
ms.locfileid: "49114607"
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
      
<a name="step-by-step-instructions"></a>Instrucciones paso a paso
-------------------------

1. Obtenga el identificador de recurso de la cuenta de Azure Storage que desea administrar.
    a. Después de crear una cuenta de almacenamiento, ejecute el siguiente comando para obtener el identificador de recurso de la cuenta de almacenamiento que desea administrar
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
   El siguiente comando solicita a Key Vault la regeneración periódica de las claves de acceso del almacenamiento, con un período de regeneración. A continuación, se establece un período de regeneración de 90 días. Después de 90 días, Key Vault regenerará "key1" y cambiará la clave activa de "key2" a "key1".
   ### <a name="key-regeneration"></a>Regeneración de claves
    ```
    az keyvault storage add --vault-name <YourVaultName> -n <StorageAccountName> --active-key-name key2 --auto-generate-key --regeneration-period P90D --resource-id <Resource-id-of-storage-account>
    ```
    En caso de que el usuario no haya creado la cuenta de almacenamiento y no tenga permisos para la cuenta de almacenamiento, los pasos siguientes establecen los permisos de la cuenta para asegurarse de que puede administrar todos los permisos de almacenamiento en el almacén de claves.
    [!NOTE] Si el usuario no tiene permisos para la cuenta de almacenamiento, primero debemos obtener el identificador de objeto del usuario

    ```
    az ad user show --upn-or-object-id "developer@contoso.com"

    az keyvault set-policy --name <YourVaultName> --object-id <ObjectId> --storage-permissions backup delete list regeneratekey recover purge restore set setsas update
    ```

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
