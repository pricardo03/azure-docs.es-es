---
title: Administración de claves de cuenta de almacenamiento con Azure Key Vault y la CLI de Azure
description: Las claves de cuenta de almacenamiento proporcionan una integración perfecta entre Azure Key Vault y basada en claves de acceso a una cuenta de almacenamiento de Azure.
ms.topic: conceptual
services: key-vault
ms.service: key-vault
author: msmbaldwin
ms.author: mbaldwin
manager: barbkess
ms.date: 03/01/2019
ms.openlocfilehash: 91cc3f96f9cdd231c38232c972c2628d12b9f4b3
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66476152"
---
# <a name="manage-storage-account-keys-with-azure-key-vault-and-the-azure-cli"></a>Administración de claves de cuenta de almacenamiento con Azure Key Vault y la CLI de Azure 

Azure Key Vault administra las claves de cuentas de Azure storage y cuentas de almacenamiento clásicas. Puede usar la característica de cuenta de almacenamiento administrado de Key Vault para completar varias funciones de administración de claves para usted.

Una [cuenta de Azure Storage](/azure/storage/storage-create-storage-account) utiliza una credencial que consta de un nombre de cuenta y una clave. La clave se genera automáticamente y actúa como una contraseña, en lugar de un objeto como una clave criptográfica. Key Vault administra las claves de cuenta de almacenamiento guardándolos como [secretos de Key Vault](/azure/key-vault/about-keys-secrets-and-certificates#key-vault-secrets). Las claves se muestran (sincronizadas) con una cuenta de almacenamiento de Azure y periódicamente se vuelven a generar o _girado_. 

Cuando se usa la característica de clave de cuenta de almacenamiento administrado, tenga en cuenta lo siguiente:

- Los valores de clave nunca se devuelven en respuesta a un llamador.
- Solo Key Vault debe administrar las claves de cuenta de almacenamiento. No administrar las claves por su cuenta y evitar la interferencia entre procesos de Key Vault.
- Un único objeto de Key Vault debe administrar las claves de cuenta de almacenamiento. No permitir la administración de claves de varios objetos.
- Puede solicitar almacén de claves para administrar la cuenta de almacenamiento con una entidad de usuario, pero no con una entidad de servicio.
- Volver a generar las claves con Key Vault solo. No volver a generar manualmente las claves de cuenta de almacenamiento. 

> [!NOTE]
> Integración de Azure Storage con Azure Active Directory (Azure AD) es en la nube identidad y acceso de servicio de administración. de Microsoft
> Integración de Azure AD está disponible para [Azure blobs y colas](https://docs.microsoft.com/azure/storage/common/storage-auth-aad).
> Usar Azure AD para la autenticación y autorización.
> Azure AD proporciona acceso de OAuth2 basada en token para el almacenamiento de Azure como Azure Key Vault.
>
> Azure AD le permite autenticar la aplicación cliente mediante una identidad de aplicación o usuario, en lugar de las credenciales de cuenta de almacenamiento.
> Puede usar un [identidad administrada de Azure AD](/azure/active-directory/managed-identities-azure-resources/) cuando se ejecuta en Azure. Las identidades administradas eliminan la necesidad de autenticación del cliente y el almacenamiento de credenciales en o con la aplicación.
> Azure AD usa el control de acceso basado en roles (RBAC) para administrar la autorización, que también es compatible con Key Vault.

### <a name="service-principal-application-id"></a>Id. de aplicación de la entidad de servicio

Un inquilino de Azure AD proporciona a cada aplicación registrado con un [serviceprincipal](/azure/active-directory/develop/developer-glossary#service-principal-object). La entidad de servicio actúa como la identidad de aplicación (identificador). El identificador de aplicación se usa durante la instalación de autorización para tener acceso a otros recursos de Azure a través de RBAC.

Key Vault es una aplicación de Microsoft que previamente se ha registrado en AD Azure todos los inquilinos. Key Vault está registrado en el mismo identificador de aplicación y dentro de cada nube de Azure.

| Inquilinos | Nube | Identificador de aplicación |
| --- | --- | --- |
| Azure AD | Azure Government | `7e7c393b-45d0-48b1-a35e-2905ddf8183c` |
| Azure AD | Públicos de Azure | `cfa8b339-82a2-471a-a3c9-0fc0be7a4093` |
| Otros  | Cualquiera | `cfa8b339-82a2-471a-a3c9-0fc0be7a4093` |

<!-- Add closing sentences to summarize what the user accomplished in this section. -->

## <a name="prerequisites"></a>Requisitos previos

Antes de usar Key Vault para administrar su clave de cuenta de almacenamiento, revise los requisitos previos:

- Instale la [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli).
- Crear un [cuenta de Azure storage](https://azure.microsoft.com/services/storage/). Siga [estos pasos](https://docs.microsoft.com/azure/storage/).
- El nombre de cuenta de almacenamiento debe usar solo letras minúsculas y números. La longitud del nombre debe ser entre 3 y 24 caracteres.        
      
## <a name="manage-storage-account-keys"></a>Administración de claves de cuenta de almacenamiento

Hay cuatro pasos básicos para usar Key Vault para administrar claves de cuenta de almacenamiento:

1. Obtener una cuenta de almacenamiento.
1. Capturar un almacén de claves.
1. Agregar una cuenta de almacenamiento administrada de Key Vault en el almacén. Establecer `key1` como la clave activa con un período de regeneración de 180 días.
1. Use `key1` para establecer un contexto de almacenamiento para la cuenta de almacenamiento especificada.

> [!NOTE]
> Key Vault como un servicio se le asignan permisos de operador en la cuenta de almacenamiento.
> 
> Después de configurar las claves de cuenta de almacenamiento administrado de Azure Key Vault, solo cambie las claves con Key Vault.
> Para las claves de cuenta de almacenamiento administrado, Key Vault administra la rotación de la clave de cuenta de almacenamiento.

1. Después de crear una cuenta de almacenamiento, ejecute el comando siguiente para obtener el identificador de recurso de la cuenta de almacenamiento para administrar:
    ```
    az storage account show -n storageaccountname
    ```

    Copie el valor de identificador de recurso de la salida del comando:
    ```
    /subscriptions/<subscription ID>/resourceGroups/ResourceGroup/providers/Microsoft.Storage/storageAccounts/StorageAccountName
    ```

    Salida de ejemplo:
    ```
    "objectId": "93c27d83-f79b-4cb2-8dd4-4aa716542e74"
    ```
    
1. Asigne el rol RBAC "Rol de servicio de operador de almacenamiento cuenta claves" en Key Vault. Este rol limita el ámbito de acceso a la cuenta de almacenamiento. Para una cuenta de almacenamiento clásico, utilice la función "Clásico almacenamiento cuenta rol operador de claves Service".

    ```
    az role assignment create --role "Storage Account Key Operator Service Role"  --assignee-object-id <ObjectIdOfKeyVault> --scope 93c27d83-f79b-4cb2-8dd4-4aa716542e74
    ```
    
    `93c27d83-f79b-4cb2-8dd4-4aa716542e74` es el identificador de objeto de Key Vault en la nube pública de Azure. Para obtener el identificador de objeto de Key Vault en la nube de Azure Government, consulte [Id. de aplicación de la entidad de servicio](#service-principal-application-id).
    
1. Cree una cuenta de almacenamiento administrada de Key Vault:

    Establecer un período de regeneración de 90 días. Después de 90 días, Key Vault vuelve a generar `key1` e intercambia la clave activa de `key2` a `key1`. `key1` a continuación, se marca como activa. 
   
    ```
    az keyvault storage add --vault-name <YourVaultName> -n <StorageAccountName> --active-key-name key1 --auto-regenerate-key --regeneration-period P90D --resource-id <Id-of-storage-account>
    ```

<!-- Add closing sentences to summarize what the user accomplished in this section. -->

## <a name="create-and-generate-tokens"></a>Crear y generar los tokens

También puede solicitar almacén de claves para generar tokens de firma de acceso compartido. Una firma de acceso compartido ofrece acceso delegado a recursos en la cuenta de almacenamiento. Se puede conceder a los clientes acceso a los recursos en su cuenta de almacenamiento sin compartir las claves de cuenta. Una firma de acceso compartido proporciona una manera segura de compartir los recursos de almacenamiento sin poner en peligro las claves de cuenta.

Los comandos en esta sección completan las acciones siguientes:

- Establecer una definición de firma de acceso de cuenta se comparte `<YourSASDefinitionName>`. La definición se establece en una cuenta de almacenamiento de Key Vault administra `<YourStorageAccountName>` en el almacén de claves `<VaultName>`.
- Crear un token de firma de acceso compartido de cuenta para los servicios Blob, archivo, tabla y cola. El token se crea para tipos de recursos de servicio, el contenedor y objeto. El token se crea con todos los permisos, a través de https y con las fechas de inicio y finalización especificadas.
- Establezca una definición de firma de acceso de Key Vault administra almacenamiento compartido en el almacén. La definición tiene la plantilla de URI del token de firma de acceso compartido que se creó. La definición tiene el tipo de firma de acceso compartido `account` y es válido durante N días.
- Recuperar el token de acceso real del secreto de Key Vault que corresponde a la definición de la firma de acceso compartido.

Después de completar los pasos descritos en la sección anterior, ejecute los comandos siguientes para formular Key Vault para generar tokens de firma de acceso compartido. 

1. Crear una definición de la firma de acceso compartido. Una vez creada la definición de la firma de acceso compartido, pida a Key Vault para generar más de los tokens de firma de acceso compartido. Esta operación requiere la `storage` y `setsas` permisos.
    ```
    $sastoken = az storage account generate-sas --expiry 2020-01-01 --permissions rw --resource-types sco --services bfqt --https-only --account-name storageacct --account-key 00000000
    ```

    Para obtener ayuda acerca de la operación, consulte el [almacenamiento de az account enerate-sas](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-generate-sas) documentación de referencia.

    Después de la operación se ejecuta correctamente, copie la salida.
    ```console
       "se=2020-01-01&sp=***"
    ```

1. Use el `$sasToken` generado por el comando anterior y crear una definición de la firma de acceso compartido. Para obtener más información acerca de los parámetros del comando, consulte el [crear az keyvault almacenamiento sas definición](https://docs.microsoft.com/cli/azure/keyvault/storage/sas-definition?view=azure-cli-latest#required-parameters) documentación de referencia.
    ```
    az keyvault storage sas-definition create --vault-name <YourVaultName> --account-name <YourStorageAccountName> -n <NameOfSasDefinitionYouWantToGive> --validity-period P2D --sas-type account --template-uri $sastoken
    ```

    Cuando el usuario no tiene permisos para la cuenta de almacenamiento, primero obtenga el identificador de objeto del usuario:
    ```
    az ad user show --upn-or-object-id "developer@contoso.com"

    az keyvault set-policy --name <YourVaultName> --object-id <ObjectId> --storage-permissions backup delete list regeneratekey recover     purge restore set setsas update
    ```

<!-- Add closing sentences to summarize what the user accomplished in this section. -->

## <a name="fetch-tokens-in-code"></a>Capturar el token en código

Ejecutar operaciones en la cuenta de almacenamiento mediante la captura de [comparten los tokens de firma de acceso](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) de Key Vault.

Hay tres maneras para autenticarse en Key Vault:

- Usar una identidad de servicio administrada. Se recomienda este enfoque.
- Usar una entidad de servicio y un certificado. 
- Usar una entidad de servicio y la contraseña. No se recomienda este enfoque.

Para obtener más información, consulte [Azure Key Vault: Conceptos básicos](key-vault-whatis.md#basic-concepts).

El ejemplo siguiente muestra cómo capturar los tokens de firma de acceso compartido. Capturar los tokens después de crear una definición de la firma de acceso compartido. 

```cs
// After you get a security token, create KeyVaultClient with vault credentials.
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(securityToken));

// Get a shared access signature token for your storage from Key Vault.
// The format for SecretUri is https://<VaultName>.vault.azure.net/secrets/<ExamplePassword>
var sasToken = await kv.GetSecretAsync("SecretUri");

// Create new storage credentials by using the shared access signature token.
var accountSasCredential = new StorageCredentials(sasToken.Value);

// Use the storage credentials and the Blob storage endpoint to create a new Blob service client.
var accountWithSas = new CloudStorageAccount(accountSasCredential, new Uri ("https://myaccount.blob.core.windows.net/"), null, null, null);

var blobClientWithSas = accountWithSas.CreateCloudBlobClient();
```

Si el token de firma de acceso compartido está a punto de expirar, capturar el token de firma de acceso compartido nuevo de Key Vault y actualizar el código.

```cs
// If your shared access signature token is about to expire,
// get the shared access signature token again from Key Vault and update it.
sasToken = await kv.GetSecretAsync("SecretUri");
accountSasCredential.UpdateSASToken(sasToken);
```

<!-- Add closing sentences to summarize what the user accomplished in this section. -->

### <a name="azure-cli-commands"></a>Comandos de la CLI de Azure

Para obtener información acerca de los comandos de CLI de Azure que son relevantes para las cuentas de almacenamiento administrado, consulte el [almacenamiento de az keyvault](https://docs.microsoft.com/cli/azure/keyvault/storage?view=azure-cli-latest) documentación de referencia.

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre [claves, secretos y certificados](https://docs.microsoft.com/rest/api/keyvault/).
- Revise los artículos en el [blog del equipo de Azure Key Vault](https://blogs.technet.microsoft.com/kv/).
