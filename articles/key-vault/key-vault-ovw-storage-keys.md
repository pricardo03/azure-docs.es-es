---
title: Administración de claves de cuenta de almacenamiento con Azure Key Vault y la CLI de Azure
description: Las claves de cuenta de almacenamiento proporcionan integración sin problemas entre Azure Key Vault y el acceso basado en claves a una cuenta de almacenamiento de Azure.
ms.topic: conceptual
services: key-vault
ms.service: key-vault
author: msmbaldwin
ms.author: mbaldwin
manager: rkarlin
ms.date: 03/01/2019
ms.openlocfilehash: 68c9279333e9ee92cbca1034f70801c153b6cdb8
ms.sourcegitcommit: e97a0b4ffcb529691942fc75e7de919bc02b06ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/15/2019
ms.locfileid: "71000548"
---
# <a name="manage-storage-account-keys-with-azure-key-vault-and-the-azure-cli"></a>Administración de claves de cuenta de almacenamiento con Azure Key Vault y la CLI de Azure 

Azure Key Vault administra las claves para las cuentas de almacenamiento de Azure y las cuentas de almacenamiento clásicas. Puede usar la característica de cuenta de almacenamiento administrada de Key Vault para completar varias funciones de administración para usted.

Una [cuenta de Azure Storage](/azure/storage/storage-create-storage-account) utiliza una credencial que consta de un nombre de cuenta y una clave. La clave se genera automáticamente y actúa como una contraseña y no como una clave criptográfica. Key Vault administra las claves de cuenta de almacenamiento guardándolas como [secretos de Key Vault](/azure/key-vault/about-keys-secrets-and-certificates#key-vault-secrets). Las claves se enumeran (sincronizan) con una cuenta de almacenamiento de Azure y se regeneran o _rotan_ periódicamente. 

Cuando use la característica de clave de cuenta de almacenamiento administrada, tenga en cuenta los puntos siguientes:

- Los valores de clave nunca se devuelven como respuesta a un autor de la llamada.
- Solo Key Vault debe administrar las claves de cuenta de almacenamiento. No administre las claves por su cuenta y evite interferir en los procesos de Key Vault.
- Solo un único objeto de Key Vault debe administrar las claves de cuenta de almacenamiento. No permita la administración de claves desde varios objetos.
- Puede solicitar a Key Vault que administre la cuenta de almacenamiento con una entidad de seguridad de usuario, pero no con una entidad de servicio.
- Regenere las claves solo con Key Vault. No regenere manualmente las claves de cuenta de almacenamiento. 

> [!NOTE]
> La integración de Azure Storage con Azure Active Directory (Azure AD) es un servicio de administración de acceso y de identidades basado en la nube de Microsoft.
> La integración con Azure AD está disponible para [blobs y colas de Azure](../storage/common/storage-auth-aad.md).
> Use Azure AD para autenticación y autorización.
> Azure AD proporciona acceso basado en token OAuth2 a Azure Storage exactamente igual que Azure Key Vault.
>
> Azure AD le permite autenticar la aplicación cliente mediante una identidad de aplicación o usuario, en lugar de las credenciales de cuenta de almacenamiento.
> Puede usar una [identidad administrada de Azure AD](/azure/active-directory/managed-identities-azure-resources/) cuando realice la ejecución en Azure. Las identidades administradas eliminan la necesidad de autenticación del cliente y de almacenar las credenciales en la aplicación o con ella.
> Azure AD usa el control de acceso basado en rol (RBAC) para administrar la autorización, que también es compatible con Key Vault.

### <a name="service-principal-application-id"></a>Id. de aplicación de la entidad de servicio

Un inquilino de Azure AD proporciona a cada aplicación registrada una [entidad de servicio](/azure/active-directory/develop/developer-glossary#service-principal-object). La entidad de servicio actúa como la identidad de aplicación (id.). La identidad de aplicación se usa durante la configuración de la autorización para acceder a otros recursos de Azure a través de RBAC.

Key Vault es una aplicación de Microsoft que previamente se ha registrado en todos los inquilinos de Azure AD. Key Vault se ha registrado en la misma identidad de aplicación y dentro de cada nube de Azure.

| Inquilinos | Nube | Identificador de aplicación |
| --- | --- | --- |
| Azure AD | Azure Government | `7e7c393b-45d0-48b1-a35e-2905ddf8183c` |
| Azure AD | Pública de Azure | `cfa8b339-82a2-471a-a3c9-0fc0be7a4093` |
| Otros  | Any | `cfa8b339-82a2-471a-a3c9-0fc0be7a4093` |

<!-- Add closing sentences to summarize what the user accomplished in this section. -->

## <a name="prerequisites"></a>Requisitos previos

Antes de usar Key Vault para administrar la clave de cuenta de almacenamiento, revise los requisitos previos:

- Instale la [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli).
- Cree una [cuenta de almacenamiento de Azure](https://azure.microsoft.com/services/storage/). Siga [estos pasos](../storage/index.yml).
- El nombre de la cuenta de almacenamiento solo debe contener letras minúsculas y números. El nombre debe tener entre 3 y 24 caracteres.        
      
## <a name="manage-storage-account-keys"></a>Administración de las claves de cuenta de almacenamiento

Hay cuatro pasos básicos para usar Key Vault para administrar las claves de cuenta de almacenamiento:

1. Obtener una cuenta de almacenamiento existente.
1. Capturar un almacén de claves existente.
1. Agregar una cuenta de almacenamiento administrada de Key Vault al almacén. Establecer `key1` como la clave activa con un período de regeneración de 90 días.
1. Usar `key1` para establecer un contexto de almacenamiento para la cuenta de almacenamiento especificada.

> [!NOTE]
> Key Vault como servicio tiene asignados permisos de operador en la cuenta de almacenamiento.
> 
> Después de configurar las claves de cuenta de almacenamiento administrada de Azure Key Vault, cambie solamente las claves mediante Key Vault.
> Para las claves de cuenta de almacenamiento administrada, Key Vault administra la rotación de la clave de cuenta de almacenamiento.

1. Después de crear una cuenta de almacenamiento, ejecute el siguiente comando para obtener el identificador de recurso de la cuenta de almacenamiento que desea administrar:
    ```
    az storage account show -n storageaccountname
    ```

    Copie el valor del identificador del recurso de la salida del comando:
    ```
    /subscriptions/<subscription ID>/resourceGroups/ResourceGroup/providers/Microsoft.Storage/storageAccounts/StorageAccountName
    ```

    Salida de ejemplo:
    ```
    "objectId": "93c27d83-f79b-4cb2-8dd4-4aa716542e74"
    ```
    
1. Asigne el rol RBAC "Rol de servicio del operador de claves de cuentas de almacenamiento" a Key Vault. Este rol limita el ámbito de acceso a la cuenta de almacenamiento. Para una cuenta de almacenamiento clásica, use el rol "Rol de servicio de operador de claves de cuentas de almacenamiento clásicas".

    ```
    az role assignment create --role "Storage Account Key Operator Service Role" --assignee-object-id 93c27d83-f79b-4cb2-8dd4-4aa716542e74 --scope "/subscriptions/<subscriptionID>/resourceGroups/<StorageAccountResourceGroupName>/providers/Microsoft.Storage/storageAccounts/<StorageAccountName>"
    ```
    
    `93c27d83-f79b-4cb2-8dd4-4aa716542e74` es el identificador de objeto para Key Vault en la nube pública de Azure. Para obtener el identificador de objeto de Key Vault en la nube de Azure Government, consulte [Id. de aplicación de la entidad de servicio](#service-principal-application-id).
    
1. Cree una cuenta de almacenamiento administrada de Key Vault:

    Establezca un período de regeneración de 90 días. Después de 90 días, Key Vault regenera `key1` e intercambia la clave activa de `key2` a `key1`. Después, `key1` se marca como la clave activa. 
   
    ```
    az keyvault storage add --vault-name <YourVaultName> -n <StorageAccountName> --active-key-name key1 --auto-regenerate-key --regeneration-period P90D --resource-id <Id-of-storage-account>
    ```

<!-- Add closing sentences to summarize what the user accomplished in this section. -->

## <a name="create-and-generate-tokens"></a>Creación y generación de tokens

También puede pedir a Key Vault que genere tokens de firma de acceso compartido. Una firma de acceso compartido ofrece acceso delegado a recursos en la cuenta de almacenamiento. Puede conceder a los clientes acceso a los recursos de su cuenta de almacenamiento sin compartir las claves de la cuenta. Una firma de acceso compartido le proporciona una forma segura de compartir los recursos de almacenamiento sin poner en peligro las claves de cuenta.

Los comandos de esta sección completan las acciones siguientes:

- Establecer una definición de firma de acceso compartido de cuenta `<YourSASDefinitionName>`. La definición se establece en una cuenta de almacenamiento administrada de Key Vault `<YourStorageAccountName>` en el almacén de claves `<VaultName>`.
- Crear un token de firma de acceso compartido de cuenta para los servicios Blob, File, Table y Queue. El token se crea para los tipos de recurso Servicio, Contenedor y Objeto. El token se crea con todos los permisos, a través de https, y con las fechas de inicio y finalización especificadas.
- Establecer una definición de firma de acceso compartido de almacenamiento administrado de Key Vault en el almacén. La definición tiene el URI de plantilla del token de firma de acceso compartido que se creó. La definición tiene el tipo de firma de acceso compartido `account` y es válido durante N días.
- Recuperar el token de acceso real del secreto de Key Vault que corresponde a la definición de la firma de acceso compartido.

Después de completar los pasos descritos en la sección anterior, ejecute los comandos siguientes para pedir a Key Vault que genere tokens de firma de acceso compartido. 

1. Cree una definición de firma de acceso compartido. Una vez creada la definición de la firma de acceso compartido, pida a Key Vault que regenere más tokens de firma de acceso compartido. Esta operación requiere los permisos `storage` y `setsas`.
    ```
    $sastoken = az storage account generate-sas --expiry 2020-01-01 --permissions rw --resource-types sco --services bfqt --https-only --account-name storageacct --account-key 00000000
    ```

    Para ayuda sobre la operación, consulte la documentación de referencia de [az storage account generate-sas](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-generate-sas).

    Después de que la operación se ejecute correctamente, copie la salida.
    ```console
       "se=2020-01-01&sp=***"
    ```

1. Use el `$sasToken` generado por el comando anterior y cree una definición de firma de acceso compartido. Para más información sobre los parámetros del comando, consulte la documentación de referencia de [az keyvault storage sas-definition create](https://docs.microsoft.com/cli/azure/keyvault/storage/sas-definition?view=azure-cli-latest#required-parameters).
    ```
    az keyvault storage sas-definition create --vault-name <YourVaultName> --account-name <YourStorageAccountName> -n <NameOfSasDefinitionYouWantToGive> --validity-period P2D --sas-type account --template-uri $sastoken
    ```

    En caso de que el usuario no tenga permisos para la cuenta de almacenamiento, obtenga primero el identificador de objeto del usuario:
    ```
    az ad user show --upn-or-object-id "developer@contoso.com"

    az keyvault set-policy --name <YourVaultName> --object-id <ObjectId> --storage-permissions backup delete list regeneratekey recover     purge restore set setsas update
    ```

<!-- Add closing sentences to summarize what the user accomplished in this section. -->

## <a name="fetch-tokens-in-code"></a>Captura de los tokens en el código

Ejecute las operaciones en la cuenta de almacenamiento capturando los [tokens de firma de acceso compartido](../storage/common/storage-dotnet-shared-access-signature-part-1.md) de Key Vault.

Hay tres formas de autenticarse en Key Vault:

- Usar una identidad de servicio administrada. Este enfoque es muy recomendable.
- Usar una entidad de servicio y un certificado. 
- Usar una entidad de servicio y una contraseña. Este enfoque no es recomendable.

Para más información, consulte [Azure Key Vault: Conceptos básicos](basic-concepts.md).

En el ejemplo siguiente se muestra cómo capturar los tokens de firma de acceso compartido. Los tokens se capturan después de crear una definición de firma de acceso compartido. 

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

Si el token de firma de acceso compartido está a punto de expirar, capture el token de firma de acceso compartido de nuevo de Key Vault y actualice el código.

```cs
// If your shared access signature token is about to expire,
// get the shared access signature token again from Key Vault and update it.
sasToken = await kv.GetSecretAsync("SecretUri");
accountSasCredential.UpdateSASToken(sasToken);
```

<!-- Add closing sentences to summarize what the user accomplished in this section. -->

### <a name="azure-cli-commands"></a>Comandos de la CLI de Azure

Para información sobre los comandos de la CLI de Azure que son pertinentes para las cuentas de almacenamiento administradas, consulte la documentación de referencia de [az keyvault storage](https://docs.microsoft.com/cli/azure/keyvault/storage?view=azure-cli-latest).

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre [claves, secretos y certificados](https://docs.microsoft.com/rest/api/keyvault/).
- Revise los artículos del [blog del equipo de Azure Key Vault](https://blogs.technet.microsoft.com/kv/).
