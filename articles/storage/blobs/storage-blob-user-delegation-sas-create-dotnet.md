---
title: 'Creación de una SAS de delegación de usuarios para un contenedor o blob con .NET (versión preliminar): Azure Storage'
description: Obtenga información sobre cómo crear una SAS de delegación de usuarios con credenciales de Azure Active Directory en Azure Storage mediante la biblioteca de cliente de .NET.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 08/12/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: blobs
ms.openlocfilehash: bed95c070649785a701f9d08a98faf29c8ee1413
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2019
ms.locfileid: "68990592"
---
# <a name="create-a-user-delegation-sas-for-a-container-or-blob-with-net-preview"></a>Creación de una SAS de delegación de usuarios para un contenedor o blob con .NET (versión preliminar)

[!INCLUDE [storage-auth-sas-intro-include](../../../includes/storage-auth-sas-intro-include.md)]

En este artículo se muestra cómo usar credenciales de Azure Active Directory (Azure AD) para crear una SAS de delegación de usuarios para un contenedor o blob con la [biblioteca cliente de Azure Storage para .NET](https://www.nuget.org/packages/Azure.Storage.Blobs).

[!INCLUDE [storage-auth-user-delegation-include](../../../includes/storage-auth-user-delegation-include.md)]

## <a name="install-the-preview-packages"></a>Instalar los paquetes de versión preliminar

En los ejemplos de este artículo se usa la versión preliminar más reciente de la biblioteca cliente de Azure Storage para Blob Storage. Para instalar el paquete de versión preliminar, ejecute el siguiente comando desde la consola del administrador de paquetes NuGet:

```
Install-Package Azure.Storage.Blobs -IncludePrerelease
```

En los ejemplos de este artículo también se usa la versión preliminar más reciente de la [biblioteca cliente de Azure Identity para .NET](https://www.nuget.org/packages/Azure.Identity/) para autenticarse con credenciales de Azure AD. La biblioteca cliente de Azure Identity autentica una entidad de seguridad. La entidad de seguridad autenticada puede entonces crear la SAS de delegación de usuarios. Para más información sobre la biblioteca cliente de Azure Identity, consulte [Azure Identity client library for .NET](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/identity/Azure.Identity) (Biblioteca cliente de Azure Identity para .NET).

```
Install-Package Azure.Identity -IncludePrerelease
```

## <a name="create-a-service-principal"></a>Creación de una entidad de servicio

Para autenticarse con credenciales de Azure AD a través de la biblioteca cliente de Azure Identity, use una entidad de servicio o una identidad administrada como entidad de seguridad, en función de dónde se ejecute el código. Si el código se ejecuta en un entorno de desarrollo, use una entidad de servicio con fines de prueba. Si el código se ejecuta en Azure, use una identidad administrada. En este artículo se supone que ejecuta código desde el entorno de desarrollo, y se muestra cómo usar una entidad de servicio para crear la SAS de delegación de usuarios.

Para crear una entidad de servicio con la CLI de Azure y asignar el rol RBAC, use el comando [az ad sp create-for-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac). Proporcione un rol de acceso a datos Azure Storage para asignar a la nueva entidad de servicio. El rol tiene que incluir la acción **Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey**. Para más información acerca de los roles integrados que se proporcionan para Azure Storage, consulte [Roles integrados en los recursos de Azure](../../role-based-access-control/built-in-roles.md).

Además, proporcione el ámbito de la asignación de roles. La entidad de servicio creará la clave de delegación de usuario, que es una operación que se realiza en el nivel de la cuenta de almacenamiento, por lo que el ámbito de la asignación de roles debe encontrarse en el nivel de la cuenta de almacenamiento, el grupo de recursos o la suscripción. Para más información sobre los permisos de RBAC para crear una SAS de delegación de usuarios, consulte la sección **Asignar permisos con RBAC** de [Create a user delegation SAS (REST API)](/rest/api/storageservices/create-a-user-delegation-sas) (Creación de una SAS de delegación de usuario [API de REST]).

Si no tiene permisos suficientes para asignar un rol a la entidad de servicio, puede que tenga que pedir al propietario o administrador de la cuenta que realice la asignación de roles.

En el ejemplo siguiente se usa la CLI de Azure para crear una nueva entidad de servicio y asignarle el rol **lector de datos de blob de almacenamiento** con el ámbito de cuenta.

```azurecli-interactive
az ad sp create-for-rbac \
    --name <service-principal> \
    --role "Storage Blob Data Reader" \
    --scopes /subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>
```

El comando `az ad sp create-for-rbac` devuelve una lista de propiedades de entidad de servicio en formato JSON. Copie estos valores para poder usarlos con el fin de crear las variables de entorno necesarias en el paso siguiente.

```json
{
    "appId": "generated-app-ID",
    "displayName": "service-principal-name",
    "name": "http://service-principal-uri",
    "password": "generated-password",
    "tenant": "tenant-ID"
}
```

> [!IMPORTANT]
> La propagación de las asignaciones de roles RBAC pueden tardar unos minutos.

## <a name="set-environment-variables"></a>Establecimiento de variables de entorno

La biblioteca cliente de Azure Identity lee valores de tres variables de entorno en tiempo de ejecución para autenticar la entidad de servicio. En la tabla siguiente se describe el valor que se debe establecer para cada variable de entorno.

|Variable de entorno|Valor
|-|-
|`AZURE_CLIENT_ID`|Id. de la aplicación de la entidad de servicio
|`AZURE_TENANT_ID`|Id. de inquilino de Azure AD de la entidad de servicio
|`AZURE_CLIENT_SECRET`|Contraseña generada para la entidad de servicio

> [!IMPORTANT]
> Después de establecer las variables de entorno, cierre y vuelva a abrir la ventana de la consola. Si usa Visual Studio u otro entorno de desarrollo, es posible que tenga que reiniciar el entorno de desarrollo para que registre las nuevas variables de entorno.

## <a name="add-using-directives"></a>Adición de directivas using

Agregue las siguientes directivas `using` al código para usar las versiones preliminares de Azure Identity y las bibliotecas cliente de Azure Storage.

```csharp
using System;
using System.IO;
using System.Threading.Tasks;
using Azure.Identity;
using Azure.Storage;
using Azure.Storage.Sas;
using Azure.Storage.Blobs;
using Azure.Storage.Blobs.Models;
```

## <a name="authenticate-the-service-principal"></a>Autenticación de la entidad de servicio

Para autenticar la entidad de servicio, cree una instancia de la clase [DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential). El constructor `DefaultAzureCredential` lee las variables de entorno que creó anteriormente.

En el fragmento de código siguiente se muestra cómo obtener la credencial autenticada y usarla para crear un cliente de servicio para Blob Storage.

```csharp
string blobEndpoint = string.Format("https://{0}.blob.core.windows.net", accountName);

BlobServiceClient blobClient = new BlobServiceClient(new Uri(blobEndpoint),
                                                     new DefaultAzureCredential());
```

## <a name="get-the-user-delegation-key"></a>Obtención de la clave de delegación de usuario

Cada SAS se firma con una clave. Para crear una SAS de delegación de usuarios, primero debe solicitar una clave de delegación de usuario, que se usará para firmar la SAS. La clave de delegación de usuarios es análoga a la clave de cuenta usada para firmar una SAS de servicio o una SAS de cuenta, excepto en que se basa en las credenciales de Azure AD. Cuando un cliente solicita una clave de delegación de usuario mediante un token de OAuth 2.0, Azure Storage devuelve la clave de delegación de usuario en nombre del usuario.

Una vez que tenga la clave de delegación de usuario, puede usar esa clave para crear cualquier número de firmas de acceso compartido de delegación de usuarios durante la vigencia de la clave. La clave de delegación de usuario es independiente del token de OAuth 2.0 que se usa para adquirirla, por lo que no es necesario renovar el token siempre y cuando la clave siga siendo válida. Puede especificar que la clave sea válida para un período de hasta 7 días.

Use uno de los métodos siguientes para solicitar la clave de delegación de usuario:

- [GetUserDelegationKey](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.getuserdelegationkey)
- [GetUserDelegationKeyAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.getuserdelegationkeyasync)

El siguiente fragmento de código obtiene la clave de delegación de usuario y escribe sus propiedades:

```csharp
UserDelegationKey key = await blobClient.GetUserDelegationKeyAsync(DateTimeOffset.UtcNow,
                                                                   DateTimeOffset.UtcNow.AddDays(7));

Console.WriteLine("User delegation key properties:");
Console.WriteLine("Key signed start: {0}", key.SignedStart);
Console.WriteLine("Key signed expiry: {0}", key.SignedExpiry);
Console.WriteLine("Key signed object ID: {0}", key.SignedOid);
Console.WriteLine("Key signed tenant ID: {0}", key.SignedTid);
Console.WriteLine("Key signed service: {0}", key.SignedService);
Console.WriteLine("Key signed version: {0}", key.SignedVersion);
```

## <a name="create-the-sas-token"></a>Creación del token de SAS

En el fragmento de código siguiente se muestra cómo crear un nuevo [BlobSasBuilder](/dotnet/api/azure.storage.sas.blobsasbuilder) y proporcionar los parámetros para la SAS de delegación de usuarios. Después, el fragmento de código llama a [ToSasQueryParameters](/dotnet/api/azure.storage.sas.blobsasbuilder.tosasqueryparameters) para obtener la cadena del token de SAS. Por último, el código genera el URI completo, incluida la dirección de recursos y el token de SAS.

```csharp
BlobSasBuilder builder = new BlobSasBuilder()
{
    ContainerName = containerName,
    BlobName = blobName,
    Permissions = "r",
    Resource = "b",
    StartTime = DateTimeOffset.UtcNow,
    ExpiryTime = DateTimeOffset.UtcNow.AddMinutes(5)
};

string sasToken = sasBuilder.ToSasQueryParameters(key, accountName).ToString();

UriBuilder fullUri = new UriBuilder()
{
    Scheme = "https",
    Host = string.Format("{0}.blob.core.windows.net", accountName),
    Path = string.Format("{0}/{1}", containerName, blobName),
    Query = sasToken
};
```

## <a name="example-get-a-user-delegation-sas"></a>Ejemplo: Obtención de una SAS de delegación de usuarios

En el siguiente método de ejemplo se muestra el código completo para autenticar la entidad de seguridad y crear la SAS de delegación de usuarios:

```csharp
async static Task<Uri> GetUserDelegationSasBlob(string accountName, string containerName, string blobName)
{
    // Construct the blob endpoint from the account name.
    string blobEndpoint = string.Format("https://{0}.blob.core.windows.net", accountName);

    // Create a new Blob service client with Azure AD credentials.  
    BlobServiceClient blobClient = new BlobServiceClient(new Uri(blobEndpoint), 
                                                            new DefaultAzureCredential());

    // Get a user delegation key for the Blob service that's valid for seven days.
    // Use the key to generate any number of shared access signatures over the lifetime of the key.
    UserDelegationKey key = await blobClient.GetUserDelegationKeyAsync(DateTimeOffset.UtcNow,
                                                                       DateTimeOffset.UtcNow.AddDays(7));

    // Read the key's properties.
    Console.WriteLine("User delegation key properties:");
    Console.WriteLine("Key signed start: {0}", key.SignedStart);
    Console.WriteLine("Key signed expiry: {0}", key.SignedExpiry);
    Console.WriteLine("Key signed object ID: {0}", key.SignedOid);
    Console.WriteLine("Key signed tenant ID: {0}", key.SignedTid);
    Console.WriteLine("Key signed service: {0}", key.SignedService);
    Console.WriteLine("Key signed version: {0}", key.SignedVersion);

    // Create a SAS token that's valid a short interval.
    BlobSasBuilder sasBuilder = new BlobSasBuilder()
    {
        ContainerName = containerName,
        BlobName = blobName,
        Permissions = "r",
        Resource = "b",
        StartTime = DateTimeOffset.UtcNow,
        ExpiryTime = DateTimeOffset.UtcNow.AddMinutes(5)
    };

    // Use the key to get the SAS token.
    string sasToken = sasBuilder.ToSasQueryParameters(key, accountName).ToString();

    // Construct the full URI, including the SAS token.
    UriBuilder fullUri = new UriBuilder()
    {
        Scheme = "https",
        Host = string.Format("{0}.blob.core.windows.net", accountName),
        Path = string.Format("{0}/{1}", containerName, blobName),
        Query = sasToken
    };

    Console.WriteLine("User delegation SAS URI: {0}", fullUri);
    return fullUri.Uri;
}
```

## <a name="example-read-a-blob-with-a-user-delegation-sas"></a>Ejemplo: Lectura de un blob con una SAS de delegación de usuarios

En el ejemplo siguiente se prueba la SAS de delegación de usuarios creada en el ejemplo anterior desde una aplicación cliente simulada. Si la SAS es válida, la aplicación cliente puede leer el contenido del blob. Si la SAS no es válida, por ejemplo, si ha expirado, Azure Storage devuelve el código de error 403 (prohibido).

```csharp
private static async Task ReadBlobWithSasAsync(Uri sasUri)
{
    // Try performing blob operations using the SAS provided.

    // Create a blob client object for blob operations.
    BlobClient blobClient = new BlobClient(sasUri, null);

    // Download and read the contents of the blob.
    try
    {
        // Download blob contents to a stream and read the stream.
        BlobDownloadInfo blobDownloadInfo = await blobClient.DownloadAsync();
        using (StreamReader reader = new StreamReader(blobDownloadInfo.Content, true))
        {
            string line;
            while ((line = reader.ReadLine()) != null)
            {
                Console.WriteLine(line);
            }
        }

        Console.WriteLine();
        Console.WriteLine("Read operation succeeded for SAS {0}", sasUri);
        Console.WriteLine();
    }
    catch (StorageRequestFailedException e)
    {
        // Check for a 403 (Forbidden) error. If the SAS is invalid, 
        // Azure Storage returns this error.
        if (e.Status == 403)
        {
            Console.WriteLine("Read operation failed for SAS {0}", sasUri);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }
        else
        {
            Console.WriteLine(e.Message);
            Console.ReadLine();
            throw;
        }
    }
}
```

[!INCLUDE [storage-blob-dotnet-resources-include](../../../includes/storage-blob-dotnet-resources-include.md)]

## <a name="see-also"></a>Otras referencias

- [Get User Delegation Key operation](/rest/api/storageservices/get-user-delegation-key) (Obtener la operación de clave de delegación de usuario)
- [Create a user delegation SAS (REST API)](/rest/api/storageservices/create-a-user-delegation-sas) (Creación de una SAS de delegación de usuario [API de REST])
