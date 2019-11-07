---
title: 'Autorización del acceso a blobs y colas con Azure Active Directory e identidades administradas para los recursos de Azure: Azure Storage'
description: Compatibilidad con Azure BLOB Queue Storage que autoriza el acceso a los recursos con Azure Active Directory y las identidades administradas para los recursos de Azure. Puede usar identidades administradas para los recursos de Azure para autenticar el acceso a blobs y colas desde aplicaciones que se ejecutan en máquinas virtuales de Azure, aplicaciones de funciones, conjuntos de escalado de máquinas virtuales y otros.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 10/17/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: d77ab142e227cfaa6533395cc256d992e698dd17
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73495931"
---
# <a name="authorize-access-to-blobs-and-queues-with-azure-active-directory-and-managed-identities-for-azure-resources"></a>Autorización del acceso a blobs y colas con Azure Active Directory e identidades administradas para los recursos de Azure

El almacenamiento de blobs y colas de Azure admite la autenticación de Azure Active Directory (Azure AD) con [identidades administradas para los recursos de Azure](../../active-directory/managed-identities-azure-resources/overview.md). Las identidades administradas para recursos de Azure pueden autorizar el acceso a datos de blobs y colas con credenciales de Azure AD desde aplicaciones que se ejecutan en máquinas virtuales (VM) de Azure, aplicaciones de función, conjuntos de escalado de máquinas virtuales y otros servicios. Si usa identidades administradas para recursos de Azure junto con autenticación de Azure AD, puede evitar el almacenamiento de credenciales con las aplicaciones que se ejecutan en la nube.  

En este artículo se muestra cómo autorizar el acceso a los datos de blobs o colas desde una máquina virtual de Azure mediante identidades administradas para recursos de Azure. También se describe cómo probar el código en el entorno de desarrollo.

## <a name="enable-managed-identities-on-a-vm"></a>Habilitación de identidades administradas en una máquina virtual

Para poder usar identidades administradas para recursos de Azure a fin de autorizar el acceso a blobs y colas desde la máquina virtual, primero debe habilitar las identidades administradas para los recursos de Azure en la máquina virtual. Para aprender a habilitar las identidades administradas para los recursos de Azure, consulte uno de estos artículos:

- [Azure Portal](https://docs.microsoft.com/azure/active-directory/managed-service-identity/qs-configure-portal-windows-vm)
- [Azure PowerShell](../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md)
- [CLI de Azure](../../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md)
- [Plantilla de Azure Resource Manager](../../active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm.md)
- [Bibliotecas cliente de Azure Resource Manager](../../active-directory/managed-identities-azure-resources/qs-configure-sdk-windows-vm.md)

Para más información sobre las identidades administradas, consulte [Identidades administradas para recursos de Azure](../../active-directory/managed-identities-azure-resources/overview.md).

## <a name="authenticate-with-the-azure-identity-library-preview"></a>Autenticación con la biblioteca de identidades de Azure (versión preliminar)

La biblioteca cliente de identidades de Azure para .NET (versión preliminar) autentica una entidad de seguridad. Cuando el código se ejecuta en Azure, la entidad de seguridad es una identidad administrada para los recursos de Azure.

Cuando el código se ejecuta en el entorno de desarrollo, la autenticación se puede administrar automáticamente o puede requerir el inicio de sesión en un explorador, según las herramientas que use. Microsoft Visual Studio admite el inicio de sesión único (SSO), de forma que la cuenta de usuario de Azure AD activa se utiliza automáticamente para la autenticación. Para más información sobre el inicio de sesión único, consulte [Inicio de sesión único en las aplicaciones](../../active-directory/manage-apps/what-is-single-sign-on.md).

Otras herramientas de desarrollo pueden pedirle que inicie sesión mediante un explorador web. También puede usar una entidad de servicio para autenticarse desde el entorno de desarrollo. Para más información, consulte [Creación de una identidad para aplicaciones de Azure en el portal](../../active-directory/develop/howto-create-service-principal-portal.md).

Después de la autenticación, la biblioteca cliente de identidades de Azure obtiene una credencial de token. Esta credencial de token se encapsula después en el objeto de cliente de servicio que se crea para realizar operaciones en Azure Storage. La biblioteca administra este escenario sin problemas mediante la obtención de la credencial de token adecuada.

Para más información sobre la biblioteca cliente de Azure Identity, consulte [Azure Identity client library for .NET](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/identity/Azure.Identity) (Biblioteca cliente de Azure Identity para .NET).

## <a name="assign-rbac-roles-for-access-to-data"></a>Asignación de roles RBAC para el acceso a los datos

Cuando una entidad de seguridad de Azure AD intenta acceder a los datos de blobs o colas, dicha entidad debe tener permisos para el recurso. Si la entidad de seguridad es una identidad administrada en Azure o una cuenta de usuario de Azure AD que ejecuta código en el entorno de desarrollo, se le debe asignar un rol de RBAC que conceda acceso a los datos de blobs o colas en Azure Storage. Para información sobre la asignación de permisos a través de RBAC, consulte la sección titulada **Asignación de roles RBAC para derechos de acceso** en [Autorización del acceso a blobs y colas de Azure con Azure Active Directory](../common/storage-auth-aad.md#assign-rbac-roles-for-access-rights).

## <a name="install-the-preview-packages"></a>Instalar los paquetes de versión preliminar

En los ejemplos de este artículo se usa la versión preliminar más reciente de la biblioteca cliente de Azure Storage para Blob Storage. Para instalar el paquete de versión preliminar, ejecute el siguiente comando desde la consola del administrador de paquetes NuGet:

```powershell
Install-Package Azure.Storage.Blobs -IncludePrerelease
```

En los ejemplos de este artículo también se usa la versión preliminar más reciente de la [biblioteca cliente de Azure Identity para .NET](https://www.nuget.org/packages/Azure.Identity/) para autenticarse con credenciales de Azure AD. Para instalar el paquete de versión preliminar, ejecute el siguiente comando desde la consola del administrador de paquetes NuGet:

```powershell
Install-Package Azure.Identity -IncludePrerelease
```

## <a name="net-code-example-create-a-block-blob"></a>Ejemplo de código .NET: Creación de un blob en bloques

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

Para obtener una credencial de token que el código pueda usar para autorizar solicitudes para Azure Storage, cree una instancia de la clase [DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential). En el ejemplo de código siguiente se muestra cómo obtener la credencial de token autenticada y usarla para crear un objeto de cliente de servicio y, luego, usar el cliente de servicio para cargar un nuevo blob:

```csharp
async static Task CreateBlockBlobAsync(string accountName, string containerName, string blobName)
{
    // Construct the blob container endpoint from the arguments.
    string containerEndpoint = string.Format("https://{0}.blob.core.windows.net/{1}",
                                                accountName,
                                                containerName);

    // Get a credential and create a client object for the blob container.
    BlobContainerClient containerClient = new BlobContainerClient(new Uri(containerEndpoint),
                                                                    new DefaultAzureCredential());

    try
    {
        // Create the container if it does not exist.
        await containerClient.CreateIfNotExistsAsync();

        // Upload text to a new block blob.
        string blobContents = "This is a block blob.";
        byte[] byteArray = Encoding.ASCII.GetBytes(blobContents);

        using (MemoryStream stream = new MemoryStream(byteArray))
        {
            await containerClient.UploadBlobAsync(blobName, stream);
        }
    }
    catch (StorageRequestFailedException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

> [!NOTE]
> Para autorizar solicitudes en datos de blob o cola con Azure AD, debe usar HTTPS para esas solicitudes.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información sobre los roles RBAC para Azure Storage, vea [Administración de los derechos de acceso a los datos de almacenamiento con RBAC](storage-auth-aad-rbac.md).
- Para información sobre la autorización de acceso a los contenedores y las colas desde las aplicaciones de almacenamiento, consulte el artículo sobre el [uso de Azure AD con aplicaciones de almacenamiento](storage-auth-aad-app.md).
- Para obtener información sobre cómo ejecutar comandos de PowerShell y la CLI de Azure con credenciales de Azure AD, vea [Ejecución de comandos de la CLI de Azure o PowerShell con credenciales de Azure AD para acceder a datos de blob o cola](storage-auth-aad-script.md).
