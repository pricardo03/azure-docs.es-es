---
title: 'Autorización del acceso a blobs y colas con Azure Active Directory e identidades administradas para los recursos de Azure: Azure Storage'
description: Compatibilidad con Azure BLOB Queue Storage que autoriza el acceso a los recursos con Azure Active Directory y las identidades administradas para los recursos de Azure. Puede usar identidades administradas para los recursos de Azure para autenticar el acceso a blobs y colas desde aplicaciones que se ejecutan en máquinas virtuales de Azure, aplicaciones de funciones, conjuntos de escalado de máquinas virtuales y otros.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 07/18/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 0f1c66126a1aa9a6ebf6f78ac6fb1ba37ba41829
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2019
ms.locfileid: "68985424"
---
# <a name="authorize-access-to-blobs-and-queues-with-azure-active-directory-and-managed-identities-for-azure-resources"></a>Autorización del acceso a blobs y colas con Azure Active Directory e identidades administradas para los recursos de Azure

El almacenamiento de blobs y colas de Azure admite la autenticación de Azure Active Directory (Azure AD) con [identidades administradas para los recursos de Azure](../../active-directory/managed-identities-azure-resources/overview.md). Las identidades administradas para recursos de Azure pueden autorizar el acceso a datos de blobs y colas con credenciales de Azure AD desde aplicaciones que se ejecutan en máquinas virtuales (VM) de Azure, aplicaciones de función, conjuntos de escalado de máquinas virtuales y otros servicios. Si usa identidades administradas para recursos de Azure junto con autenticación de Azure AD, puede evitar el almacenamiento de credenciales con las aplicaciones que se ejecutan en la nube.  

En este artículo se muestra cómo autorizar el acceso a datos de blobs y colas con una identidad administrada desde una máquina virtual de Azure.

## <a name="enable-managed-identities-on-a-vm"></a>Habilitación de identidades administradas en una máquina virtual

Para poder usar identidades administradas para recursos de Azure a fin de autorizar el acceso a blobs y colas desde la máquina virtual, primero debe habilitar las identidades administradas para los recursos de Azure en la máquina virtual. Para aprender a habilitar las identidades administradas para los recursos de Azure, consulte uno de estos artículos:

- [Azure Portal](https://docs.microsoft.com/azure/active-directory/managed-service-identity/qs-configure-portal-windows-vm)
- [Azure PowerShell](../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md)
- [CLI de Azure](../../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md)
- [Plantilla de Azure Resource Manager](../../active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm.md)
- [Bibliotecas cliente de Azure Resource Manager](../../active-directory/managed-identities-azure-resources/qs-configure-sdk-windows-vm.md)

## <a name="grant-permissions-to-an-azure-ad-managed-identity"></a>Conceder permisos a una identidad administrada de Azure AD

Para autorizar una solicitud a Blob Service o Queue Service desde una identidad administrada de la aplicación Azure Storage, primero configure los valores del control de acceso basado en roles (RBAC) de esa identidad administrada. Azure Storage define los roles RBAC que abarcan los permisos para los datos de blobs y colas. Cuando el rol RBAC se asigna a una identidad administrada, a esta se le conceden esos permisos a los datos de blobs o colas en el ámbito adecuado.

Para obtener más información sobre la asignación de roles RBAC, vea alguno de los siguientes artículos:

- [Concesión de acceso a datos de blob y cola de Azure con RBAC en Azure Portal](storage-auth-aad-rbac-portal.md)
- [Concesión de acceso a datos de blob y cola de Azure con RBAC mediante la CLI de Azure](storage-auth-aad-rbac-cli.md)
- [Conceder acceso a datos de blob y cola de Azure con RBAC mediante PowerShell](storage-auth-aad-rbac-powershell.md)

## <a name="azure-storage-resource-id"></a>Identificador de recurso de Azure Storage

[!INCLUDE [storage-resource-id-include](../../../includes/storage-resource-id-include.md)]

## <a name="net-code-example-create-a-block-blob"></a>Ejemplo de código .NET: Creación de un blob en bloques

En el ejemplo de código se muestra cómo obtener un token de OAuth 2.0 de Azure AD y usarlo para autorizar una solicitud para crear un blob en bloques. Para que este ejemplo funcione, siga primero los pasos descritos en las secciones anteriores.

[!INCLUDE [storage-app-auth-lib-include](../../../includes/storage-app-auth-lib-include.md)]

### <a name="add-the-callback-method"></a>Adición del método de devolución de llamada

El método de devolución de llamada comprueba la hora de expiración del token y lo renueva en caso necesario:

```csharp
private static async Task<NewTokenAndFrequency> TokenRenewerAsync(Object state, CancellationToken cancellationToken)
{
    // Specify the resource ID for requesting Azure AD tokens for Azure Storage.
    // Note that you can also specify the root URI for your storage account as the resource ID.
    const string StorageResource = "https://storage.azure.com/";  

    // Use the same token provider to request a new token.
    var authResult = await ((AzureServiceTokenProvider)state).GetAuthenticationResultAsync(StorageResource);

    // Renew the token 5 minutes before it expires.
    var next = (authResult.ExpiresOn - DateTimeOffset.UtcNow) - TimeSpan.FromMinutes(5);
    if (next.Ticks < 0)
    {
        next = default(TimeSpan);
        Console.WriteLine("Renewing token...");
    }

    // Return the new token and the next refresh time.
    return new NewTokenAndFrequency(authResult.AccessToken, next);
}
```

### <a name="get-a-token-and-create-a-block-blob"></a>Obtención de un token y creación de un blob en bloques

La biblioteca App Authentication proporciona la clase **AzureServiceTokenProvider**. Una instancia de esta clase se puede pasar a una devolución de llamada que obtiene un token y luego renueva el token antes de que expire.

En el ejemplo siguiente se obtiene un token y se usa para crear un nuevo blob; luego se usa el mismo token para leer el blob.

```csharp
const string blobName = "https://storagesamples.blob.core.windows.net/sample-container/blob1.txt";

// Get the initial access token and the interval at which to refresh it.
AzureServiceTokenProvider azureServiceTokenProvider = new AzureServiceTokenProvider();
var tokenAndFrequency = await TokenRenewerAsync(azureServiceTokenProvider,CancellationToken.None);

// Create storage credentials using the initial token, and connect the callback function
// to renew the token just before it expires
TokenCredential tokenCredential = new TokenCredential(tokenAndFrequency.Token,
                                                        TokenRenewerAsync,
                                                        azureServiceTokenProvider,
                                                        tokenAndFrequency.Frequency.Value);

StorageCredentials storageCredentials = new StorageCredentials(tokenCredential);

// Create a blob using the storage credentials.
CloudBlockBlob blob = new CloudBlockBlob(new Uri(blobName),
                                            storageCredentials);

// Upload text to the blob.
await blob.UploadTextAsync(string.Format("This is a blob named {0}", blob.Name));

// Continue to make requests against Azure Storage.
// The token is automatically refreshed as needed in the background.
do
{
    // Read blob contents
    Console.WriteLine("Time accessed: {0} Blob Content: {1}",
                        DateTimeOffset.UtcNow,
                        await blob.DownloadTextAsync());

    // Sleep for ten seconds, then read the contents of the blob again.
    Thread.Sleep(TimeSpan.FromSeconds(10));
} while (true);
```

Para obtener más información sobre la biblioteca App Authentication, vea [Autenticación entre servicios en Azure Key Vault mediante .NET](../../key-vault/service-to-service-authentication.md).

Para obtener más información sobre cómo adquirir un token de acceso, vea [Cómo usar identidades administradas de recursos de Azure en una máquina virtual de Azure para adquirir un token de acceso](../../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md).

> [!NOTE]
> Para autorizar solicitudes en datos de blob o cola con Azure AD, debe usar HTTPS para esas solicitudes.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información sobre los roles RBAC para Azure Storage, vea [Administración de los derechos de acceso a los datos de almacenamiento con RBAC](storage-auth-aad-rbac.md).
- Para información sobre la autorización de acceso a los contenedores y las colas desde las aplicaciones de almacenamiento, consulte el artículo sobre el [uso de Azure AD con aplicaciones de almacenamiento](storage-auth-aad-app.md).
- Para obtener información sobre cómo ejecutar comandos de PowerShell y la CLI de Azure con credenciales de Azure AD, vea [Ejecución de comandos de la CLI de Azure o PowerShell con credenciales de Azure AD para acceder a datos de blob o cola](storage-auth-aad-script.md).
