---
ms.assetid: ''
title: 'Cuenta de almacenamiento administrado por Azure Key Vault: CLI'
description: Las claves de cuenta de almacenamiento proporcionan una integración sin problemas entre Azure Key Vault y el acceso basado en claves a la cuenta de Azure Storage.
ms.topic: conceptual
services: key-vault
ms.service: key-vault
author: prashanthyv
ms.author: pryerram
manager: barbkess
ms.date: 03/01/2019
ms.openlocfilehash: c2107e501affd5e3dd22e0fbc83d078b51d414a5
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "57841147"
---
# <a name="azure-key-vault-managed-storage-account---cli"></a>Cuenta de almacenamiento administrado por Azure Key Vault: CLI

> [!NOTE]
> [Integración de Azure Storage con Azure Active Directory (Azure AD) está ahora en versión preliminar](https://docs.microsoft.com/azure/storage/common/storage-auth-aad). Se recomienda usar Azure AD para la autenticación y autorización, que proporciona acceso basado en tokens OAuth2 a Azure Storage, al igual que Azure Key Vault. Esto le permite:
> - Autenticar la aplicación cliente mediante una identidad de aplicación o usuario, en lugar de las credenciales de cuenta de almacenamiento. 
> - Usar una [identidad administrada de Azure AD](/azure/active-directory/managed-identities-azure-resources/) cuando se ejecuta en Azure. Las identidades administradas eliminan la necesidad de autenticación del cliente en conjunto y de almacenar las credenciales en la aplicación o con ella.
> - Utilice el control de acceso basado en rol (RBAC) para administrar la autorización, que también es compatible con Key Vault.

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
- Solicitar almacén de claves para administrar la cuenta de almacenamiento puede hacerse mediante una entidad de usuario por ahora y no una entidad de servicio

El siguiente ejemplo muestra cómo permitir que Key Vault administre las claves de su cuenta de almacenamiento.

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
Desde un punto de vista conceptual, la lista de pasos que se han seguido es:
- Primero debemos obtener una cuenta de almacenamiento (que existe previamente)
- A continuación, capturamos un almacén de claves (que existe previamente)
- Después, agregamos al almacén una cuenta de almacenamiento que administra Key Vault, estableciendo Key1 como la clave activa y con un período de regeneración de 180 días
- Por último, establezca un contexto de almacenamiento para la cuenta de almacenamiento especificada, con Key1

En las siguientes instrucciones, Key Vault se asignará como un servicio con permisos de operador en la cuenta de almacenamiento

> [!NOTE]
> Tenga en cuenta que, una vez que haya configurado cuentas de claves de almacenamiento administradas de Azure Key Vault, ya **NO** se pueden cambiar excepto con Key Vault. El almacenamiento administrado de las claves de cuentas implica que Key Vault administraría la rotación de la clave de la cuenta de almacenamiento

> [!IMPORTANT]
> Un inquilino de Azure AD proporciona a cada aplicación registrada una **[entidad de servicio](/azure/active-directory/develop/developer-glossary#service-principal-object)**, que actúa como la identidad de la aplicación. El identificador de aplicación de la entidad de servicio se usa cuando se le proporciona autorización para acceder a otros recursos de Azure, mediante control de acceso basado en rol (RBAC). Dado que Key Vault es una aplicación de Microsoft, está registrada previamente en todos los inquilinos de Azure AD con el mismo identificador de aplicación, dentro de cada nube de Azure:
> - Los inquilinos de Azure AD de la nube de Azure Government usan el identificador de aplicación `7e7c393b-45d0-48b1-a35e-2905ddf8183c`.
> - Los inquilinos de Azure AD de la nube pública de Azure y todos los demás usan el identificador de aplicación `cfa8b339-82a2-471a-a3c9-0fc0be7a4093`.


1. Después de crear una cuenta de almacenamiento, ejecute el siguiente comando para obtener el identificador de recurso de la cuenta de almacenamiento que desea administrar

    ```
    az storage account show -n storageaccountname 
    ```
    Campo de Id. de copia fuera el resultado del comando anterior que se parece a esta:
    ```
    /subscriptions/0xxxxxx-4310-48d9-b5ca-0xxxxxxxxxx/resourceGroups/ResourceGroup/providers/Microsoft.Storage/storageAccounts/StorageAccountName
    ```
            "objectId": "93c27d83-f79b-4cb2-8dd4-4aa716542e74"
    
2. Asigne el rol RBAC "Rol de servicio de almacenamiento cuenta clave operador" en Key Vault, limitar el ámbito de acceso a la cuenta de almacenamiento. Para una cuenta de almacenamiento clásico, usar "Clásico almacenamiento cuenta clave operador rol del servicio."
    ```
    az role assignment create --role "Storage Account Key Operator Service Role"  --assignee-object-id <ObjectIdOfKeyVault> --scope 93c27d83-f79b-4cb2-8dd4-4aa716542e74
    ```
    
    '93c27d83-f79b-4cb2-8dd4-4aa716542e74' es el identificador de objeto de Key Vault en la nube pública. Para obtener el identificador de objeto de Key Vault en nubes nacionales consulte la sección importante anterior
    
3. Cree una cuenta de almacenamiento administrada por Key Vault.     <br /><br />
   A continuación, se establece un período de regeneración de 90 días. Después de 90 días, Key Vault regenerará "key1" y cambiará la clave activa de "key2" a "key1". Ahora se marcará Key1 como la clave activa. 
   
    ```
    az keyvault storage add --vault-name <YourVaultName> -n <StorageAccountName> --active-key-name key1 --auto-regenerate-key --regeneration-period P90D --resource-id <Id-of-storage-account>
    ```

<a name="step-by-step-instructions-on-how-to-use-key-vault-to-create-and-generate-sas-tokens"></a>Instrucciones paso a paso sobre cómo usar Key Vault para crear y generar tokens de SAS
--------------------------------------------------------------------------------
También puede pedir a Key Vault que genere tokens de SAS (Firma de acceso compartido). Una firma de acceso compartido ofrece acceso delegado a recursos en la cuenta de almacenamiento. Con una SAS, puede conceder a los clientes acceso a los recursos de su cuenta de almacenamiento sin compartir las claves de la cuenta. Este es el aspecto clave de usar las firmas de acceso compartido en las aplicaciones: una SAS es una forma segura de compartir los recursos de almacenamiento sin poner en peligro las claves de cuenta.

Una vez completados los pasos enumerados anteriormente, puede ejecutar los comandos siguientes para solicitar a Key Vault que genere los tokens de SAS en su lugar. 

Con los pasos siguientes se pueden conseguir las siguientes cosas:
- Establecer una definición de SAS de cuenta llamada "<YourSASDefinitionName>" en una cuenta de almacenamiento que administra Key Vault "<YourStorageAccountName>"en el almacén "<VaultName>". 
- Crear un token de SAS de cuenta para los servicios de blobs, archivos, tablas y colas, para los tipos de recursos de servicio, contenedor y objeto, con todos los permisos, a través de https y con las fechas de inicio y finalización especificadas
- Establecer en el almacén una definición de SAS de almacenamiento que administra Key Vault, con el identificador URI de la plantilla como el token de SAS que se creó anteriormente, del tipo de SAS "account" y válido durante N días
- Recuperar el token de acceso real a partir del secreto de Key Vault que se corresponde con la definición de SAS

1. En este paso se creará una definición de SAS. Una vez creada, puede pedir a Key Vault que genere más tokens SAS para usted. Esta operación requiere el permiso storage/setsas.

```
$sastoken = az storage account generate-sas --expiry 2020-01-01 --permissions rw --resource-types sco --services bfqt --https-only --account-name storageacct --account-key 00000000
```
Puede obtener más ayuda sobre la operación anterior [aquí](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-generate-sas)

Si esta operación se ejecuta correctamente, verá una salida parecida a la que se muestra a continuación. Cópiela

```console
   "se=2020-01-01&sp=***"
```

1. En este paso, se usará la salida ($sasToken) que se generó anteriormente para crear una definición de SAS. Para obtener más documentación consulte [aquí](https://docs.microsoft.com/cli/azure/keyvault/storage/sas-definition?view=azure-cli-latest#required-parameters)   

```
az keyvault storage sas-definition create --vault-name <YourVaultName> --account-name <YourStorageAccountName> -n <NameOfSasDefinitionYouWantToGive> --validity-period P2D --sas-type account --template-uri $sastoken
```
                        

 > [!NOTE] 
 > En caso de que el usuario no tenga permisos para la cuenta de almacenamiento, primero debemos obtener el identificador de objeto del usuario

 ```
 az ad user show --upn-or-object-id "developer@contoso.com"

 az keyvault set-policy --name <YourVaultName> --object-id <ObjectId> --storage-permissions backup delete list regeneratekey recover     purge restore set setsas update
 ```
    
## <a name="fetch-sas-tokens-in-code"></a>Captura de los tokens de SAS en el código

En esta sección, analizaremos cómo puede llevar a cabo operaciones en la cuenta de almacenamiento mediante la captura de [tokens de SAS](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) desde Key Vault.

En la siguiente sección, demostraremos cómo capturar los tokens de SAS, una vez que se ha creado una definición de SAS como se indicó anteriormente.

> [!NOTE]
>   Existen tres maneras de autenticarse en Key Vault, como puede leer en los [conceptos básicos](key-vault-whatis.md#basic-concepts).
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

### <a name="relevant-azure-cli-commands"></a>Comandos pertinentes de la CLI de Azure

[Comandos de Storage de la CLI de Azure](https://docs.microsoft.com/cli/azure/keyvault/storage?view=azure-cli-latest)

## <a name="see-also"></a>Vea también

- [Información acerca de claves, secretos y certificados](https://docs.microsoft.com/rest/api/keyvault/)
- [Blog del equipo de Key Vault](https://blogs.technet.microsoft.com/kv/)
