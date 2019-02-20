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
ms.date: 10/03/2018
ms.openlocfilehash: 9b1a4e23ed0da0637b44ac52dd4d1baeb22cd6ce
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2019
ms.locfileid: "56118061"
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
Desde un punto de vista conceptual, la lista de pasos que se han seguido es:
- Primero debemos obtener una cuenta de almacenamiento (que existe previamente)
- A continuación, capturamos un almacén de claves (que existe previamente)
- Después, agregamos al almacén una cuenta de almacenamiento que administra Key Vault, estableciendo Key1 como la clave activa y con un período de regeneración de 180 días
- Por último, establezca un contexto de almacenamiento para la cuenta de almacenamiento especificada, con Key1

En las siguientes instrucciones, Key Vault se asignará como un servicio con permisos de operador en la cuenta de almacenamiento

> [!NOTE]
> Tenga en cuenta que, una vez que haya configurado cuentas de claves de almacenamiento administradas de Azure Key Vault, ya **NO** se pueden cambiar excepto con Key Vault. El almacenamiento administrado de las claves de cuentas implica que Key Vault administraría la rotación de la clave de la cuenta de almacenamiento

1. Después de crear una cuenta de almacenamiento, ejecute el siguiente comando para obtener el identificador de recurso de la cuenta de almacenamiento que desea administrar

    ```
    az storage account show -n storageaccountname 
    ```
    Copie el campo de identificador del resultado del comando anterior.
    
2. Para obtener el identificador del objeto de la entidad de servicio de Azure Key Vault, ejecute el siguiente comando

    ```
    az ad sp show --id cfa8b339-82a2-471a-a3c9-0fc0be7a4093
    ```
    
    Después de que el comando haya finalizado correctamente, busque el identificador de objeto en el resultado:
    ```console
        {
            ...
            "objectId": "93c27d83-f79b-4cb2-8dd4-4aa716542e74"
            ...
        }
    ```
    
3. Asigne el rol Operador de claves de almacenamiento a la identidad de Azure Key Vault.

    ```
    az role assignment create --role "Storage Account Key Operator Service Role"  --assignee-object-id <ObjectIdOfKeyVault> --scope <IdOfStorageAccount>
    ```
    
4. Cree una cuenta de almacenamiento administrada por Key Vault.     <br /><br />
   A continuación, se establece un período de regeneración de 90 días. Después de 90 días, Key Vault regenerará "key1" y cambiará la clave activa de "key2" a "key1". Ahora se marcará Key1 como la clave activa. 
   
    ```
    az keyvault storage add --vault-name <YourVaultName> -n <StorageAccountName> --active-key-name key1 --auto-regenerate-key --regeneration-period P90D --resource-id <Id-of-storage-account>
    ```
    En caso de que el usuario no haya creado la cuenta de almacenamiento y no tenga permisos para la cuenta de almacenamiento, los pasos siguientes establecen los permisos de la cuenta para asegurarse de que puede administrar todos los permisos de almacenamiento en el almacén de claves.
    

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

2. En este paso, se usará la salida ($sasToken) que se generó anteriormente para crear una definición de SAS. Para obtener más documentación consulte [aquí](https://docs.microsoft.com/cli/azure/keyvault/storage/sas-definition?view=azure-cli-latest#required-parameters)   

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

### <a name="relevant-azure-cli-commands"></a>Comandos pertinentes de la CLI de Azure

[Comandos de Storage de la CLI de Azure](https://docs.microsoft.com/cli/azure/keyvault/storage?view=azure-cli-latest)

## <a name="see-also"></a>Otras referencias

- [Información acerca de claves, secretos y certificados](https://docs.microsoft.com/rest/api/keyvault/)
- [Blog del equipo de Key Vault](https://blogs.technet.microsoft.com/kv/)
