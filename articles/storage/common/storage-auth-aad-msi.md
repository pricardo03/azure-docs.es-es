---
title: 'Autenticación del acceso a blobs y colas con identidades administradas de Azure Active Directory para los recursos de Azure (versión preliminar): Azure Storage | Microsoft Docs'
description: El almacenamiento de blobs y colas de Azure admite la autenticación de Azure Active Directory con identidades administradas para los recursos de Azure. Puede usar identidades administradas para los recursos de Azure para autenticar el acceso a blobs y colas desde aplicaciones que se ejecutan en máquinas virtuales de Azure, aplicaciones de función, conjuntos de escalado de máquinas virtuales y otros. Si usa identidades administradas para los recursos de Azure y aprovecha la capacidad de autenticación de Azure AD, puede evitar el almacenamiento de las credenciales en las aplicaciones que se ejecutan en la nube.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 09/05/2018
ms.author: tamram
ms.component: common
ms.openlocfilehash: 67e0731c1f10bb635baa4e0d1a26dce0a336b555
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44090362"
---
# <a name="authenticate-access-to-blobs-and-queues-with-azure-managed-identities-for-azure-resources-preview"></a>Autenticación del acceso a blobs y colas con identidades administradas de Azure para los recursos de Azure (versión preliminar)

El almacenamiento de blobs y colas de Azure admite la autenticación de Azure Active Directory (Azure AD) con [identidades administradas para los recursos de Azure](../../active-directory/managed-identities-azure-resources/overview.md). Puede usar identidades administradas para los recursos de Azure para autenticar el acceso a blobs y colas desde aplicaciones que se ejecutan en máquinas virtuales (VM) de Azure, aplicaciones de función, conjuntos de escalado de máquinas virtuales y otros. Si usa identidades administradas para los recursos de Azure y aprovecha la capacidad de autenticación de Azure AD, puede evitar el almacenamiento de las credenciales en las aplicaciones que se ejecutan en la nube.  

Para conceder permisos a una identidad administrada a un contenedor de blobs o cola, se asigna un rol de control de acceso basado en rol (RBAC) a la identidad administrada que abarca los permisos para ese recurso en el ámbito adecuado. Para más información sobre los roles RBAC en el almacenamiento, consulte [Administración de los derechos de acceso a los datos de Azure Storage con RBAC (versión preliminar)](storage-auth-aad-rbac.md). 

En este artículo se muestra la autenticación en el almacenamiento de blobs y colas de Azure con una identidad administrada desde una máquina virtual de Azure.  

[!INCLUDE [storage-auth-aad-note-include](../../../includes/storage-auth-aad-note-include.md)]

## <a name="enable-managed-identities-on-a-vm"></a>Habilitación de identidades administradas en una máquina virtual

Para poder usar las identidades administradas para los recursos de Azure para autenticar el acceso a blobs y colas desde la máquina virtual, primero debe habilitar las identidades administradas para dichos recursos en esa máquina virtual. Para aprender a habilitar las identidades administradas para los recursos de Azure, consulte uno de estos artículos:

- [Azure Portal](https://docs.microsoft.com/azure/active-directory/managed-service-identity/qs-configure-portal-windows-vm)
- [Azure PowerShell](../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md)
- [CLI de Azure](../../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md)
- [Plantilla de Azure Resource Manager](../../active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm.md)
- [SDK de Azure](../../active-directory/managed-identities-azure-resources/qs-configure-sdk-windows-vm.md)

## <a name="get-a-managed-identity-access-token"></a>Obtención de un token de acceso de identidad administrada

Para autenticarse con una identidad administrada, la aplicación o el script debe adquirir un token de acceso de identidad administrada. Para aprender a adquirir un token de acceso, consulte [Cómo usar identidades administradas de recursos de Azure en una máquina virtual de Azure para adquirir un token de acceso](../../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md).

## <a name="net-code-example-create-a-block-blob"></a>Ejemplo de código .NET: creación de un blob en bloques

El ejemplo de código supone que tiene un token de acceso de identidad administrada. El token de acceso se utiliza para autorizar a la identidad administrada para crear un blob en bloques.

### <a name="add-references-and-using-statements"></a>Incorporación de referencias y uso de instrucciones  

En Visual Studio, instale la versión preliminar de la biblioteca de clientes de Azure Storage. En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** y después **Consola del Administrador de paquetes**. En la consola, escriba el siguiente comando:

```
Install-Package https://www.nuget.org/packages/WindowsAzure.Storage  
```

Agregue lo siguiente al código mediante instrucciones:

```dotnet
using Microsoft.WindowsAzure.Storage.Auth;
```

### <a name="create-credentials-from-the-managed-identity-access-token"></a>Creación de credenciales desde el token de acceso de identidad administrada

Para crear el blob en bloques, use la clase **TokenCredentials** que proporciona La versión preliminar del paquete. Construya una nueva instancia de **TokenCredentials**, pasando el token de acceso de identidad administrada que obtuvo anteriormente:

```dotnet
// Create storage credentials from your managed identity access token.
TokenCredential tokenCredential = new TokenCredential(accessToken);
StorageCredentials storageCredentials = new StorageCredentials(tokenCredential);

// Create a block blob using the credentials.
CloudBlockBlob blob = new CloudBlockBlob(new Uri("https://storagesamples.blob.core.windows.net/sample-container/Blob1.txt"), storageCredentials);
``` 

> [!NOTE]
> La integración de Azure AD con Azure Storage requiere que se utilice HTTPS para las operaciones de Azure Storage.

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre los roles RBAC en Azure Storage, consulte [Administración de los derechos de acceso a los datos de Azure Storage con RBAC (versión preliminar)](storage-auth-aad-rbac.md).
- Para información sobre la autorización de acceso a los contenedores y las colas desde las aplicaciones de almacenamiento, consulte el artículo sobre el [uso de Azure AD con aplicaciones de almacenamiento](storage-auth-aad-app.md).
- Para más información sobre cómo iniciar sesión en la CLI de Azure y en PowerShell con una identidad de Azure AD, consulte [Use an Azure AD identity to access Azure Storage with CLI or PowerShell (Preview)](storage-auth-aad-script.md) (Uso de una identidad de Azure AD para acceder a Azure Storage con la CLI o PowerShell [versión preliminar]).
- Para información adicional sobre la integración de Azure AD para los blobs y las colas de Azure, consulte la entrada de blog del equipo de Azure Storage acerca del [anuncio de la versión preliminar Autenticación de Azure AD para Azure Storage](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
