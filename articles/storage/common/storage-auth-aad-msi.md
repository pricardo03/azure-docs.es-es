---
title: 'Autenticar el acceso a los blobs y colas con identidades de Azure Active Directory administrado para Azure resources: almacenamiento de Azure | Microsoft Docs'
description: El almacenamiento de blobs y colas de Azure admite la autenticación de Azure Active Directory con identidades administradas para los recursos de Azure. Puede usar identidades administradas para los recursos de Azure para autenticar el acceso a blobs y colas desde aplicaciones que se ejecutan en máquinas virtuales de Azure, aplicaciones de función, conjuntos de escalado de máquinas virtuales y otros. Si usa identidades administradas para los recursos de Azure y aprovecha la capacidad de autenticación de Azure AD, puede evitar el almacenamiento de las credenciales en las aplicaciones que se ejecutan en la nube.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/21/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 76bf36f63051f02d6c37261799342f424609f9b2
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "58442299"
---
# <a name="authenticate-access-to-blobs-and-queues-with-managed-identities-for-azure-resources"></a>Autenticar el acceso a los blobs y colas con identidades administradas para los recursos de Azure

El almacenamiento de blobs y colas de Azure admite la autenticación de Azure Active Directory (Azure AD) con [identidades administradas para los recursos de Azure](../../active-directory/managed-identities-azure-resources/overview.md). Las identidades administradas para recursos de Azure permiten autenticar el acceso a blobs y colas con las credenciales de Azure AD desde aplicaciones que se ejecutan en máquinas virtuales (VM) de Azure, aplicaciones de función, conjuntos de escalado de máquinas virtuales y otros. Si usa identidades administradas para los recursos de Azure y aprovecha la capacidad de autenticación de Azure AD, puede evitar el almacenamiento de las credenciales en las aplicaciones que se ejecutan en la nube.  

Para conceder permisos a una identidad administrada a un contenedor de blobs o cola, se asigna un rol de control de acceso basado en rol (RBAC) a la identidad administrada que abarca los permisos para ese recurso en el ámbito adecuado. Para obtener más información acerca de los roles RBAC en el almacenamiento, consulte [administrar derechos de acceso a los datos de almacenamiento con RBAC](storage-auth-aad-rbac.md). 

En este artículo se muestra la autenticación en el almacenamiento de blobs y colas de Azure con una identidad administrada desde una máquina virtual de Azure.  

## <a name="enable-managed-identities-on-a-vm"></a>Habilitación de identidades administradas en una máquina virtual

Para poder usar las identidades administradas para los recursos de Azure para autenticar el acceso a blobs y colas desde la máquina virtual, primero debe habilitar las identidades administradas para dichos recursos en esa máquina virtual. Para aprender a habilitar las identidades administradas para los recursos de Azure, consulte uno de estos artículos:

- [Azure Portal](https://docs.microsoft.com/azure/active-directory/managed-service-identity/qs-configure-portal-windows-vm)
- [Azure PowerShell](../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md)
- [CLI de Azure](../../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md)
- [Plantilla de Azure Resource Manager](../../active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm.md)
- [SDK de Azure](../../active-directory/managed-identities-azure-resources/qs-configure-sdk-windows-vm.md)

## <a name="assign-an-rbac-role-to-an-azure-ad-managed-identity"></a>Asignar un rol RBAC a una identidad administrada de Azure AD

Para autenticar una identidad administrada desde la aplicación Azure Storage, primero configure los valores del control de acceso basado en roles (RBAC) para esa identidad administrada. Azure Storage define los roles RBAC que abarcan los permisos para los contenedores y las colas. Cuando se asigna el rol de RBAC a una identidad administrada, se le concede acceso a ese recurso. Para obtener más información, consulte [administrar derechos de acceso a datos de Azure Blob y cola con RBAC](storage-auth-aad-rbac.md).

## <a name="get-a-managed-identity-access-token"></a>Obtención de un token de acceso de identidad administrada

Para autenticarse con una identidad administrada, la aplicación o el script debe adquirir un token de acceso de identidad administrada. Para aprender a adquirir un token de acceso, consulte [Cómo usar identidades administradas de recursos de Azure en una máquina virtual de Azure para adquirir un token de acceso](../../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md).

Para autorizar las operaciones de blob y cola con un token de OAuth, debe usar HTTPS.

## <a name="net-code-example-create-a-block-blob"></a>Ejemplo de código .NET: Creación de un blob en bloques

El ejemplo de código supone que tiene un token de acceso de identidad administrada. El token de acceso se utiliza para autorizar a la identidad administrada para crear un blob en bloques.

### <a name="add-references-and-using-statements"></a>Incorporación de referencias y uso de instrucciones  

En Visual Studio, instale la biblioteca de cliente de Azure Storage. En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** y después **Consola del Administrador de paquetes**. En la consola, escriba el siguiente comando:

```
Install-Package https://www.nuget.org/packages/WindowsAzure.Storage  
```

Agregue lo siguiente al código mediante instrucciones:

```dotnet
using Microsoft.WindowsAzure.Storage.Auth;
```

### <a name="create-credentials-from-the-managed-identity-access-token"></a>Creación de credenciales desde el token de acceso de identidad administrada

Para crear el blob en bloques, utilice el **TokenCredentials** clase. Construya una nueva instancia de **TokenCredentials**, pasando el token de acceso de identidad administrada que obtuvo anteriormente:

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

- Para obtener más información acerca de los roles RBAC para el almacenamiento de Azure, consulte [administrar derechos de acceso a los datos de almacenamiento con RBAC](storage-auth-aad-rbac.md).
- Para información sobre la autorización de acceso a los contenedores y las colas desde las aplicaciones de almacenamiento, consulte el artículo sobre el [uso de Azure AD con aplicaciones de almacenamiento](storage-auth-aad-app.md).
- Para obtener información sobre cómo iniciar sesión PowerShell y CLI de Azure con una identidad de Azure AD, consulte [usar una identidad de Azure AD para acceder a Azure Storage con PowerShell o CLI](storage-auth-aad-script.md).
