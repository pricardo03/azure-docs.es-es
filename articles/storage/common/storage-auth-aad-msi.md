---
title: Autenticación con Azure AD en una máquina virtual de Azure mediante Managed Service Identity (versión preliminar) | Microsoft Docs
description: Autenticación con Azure AD en una máquina virtual de Azure mediante Managed Service Identity (versión preliminar)
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 05/18/2018
ms.author: tamram
ms.openlocfilehash: 080cb3ee536227e5ddce3fac856de79b2b061dcf
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2018
ms.locfileid: "38970775"
---
# <a name="authenticate-with-azure-ad-from-an-azure-managed-service-identity-preview"></a>Autenticación con Azure AD mediante Azure Managed Service Identity (versión preliminar)

Azure Storage admite la autenticación de Azure Active Directory (Azure AD) con [Managed Service Identity](../../active-directory/managed-service-identity/overview.md). Managed Service Identity (MSI) proporciona una identidad administrada automáticamente en Azure Active Directory (Azure AD). Puede usar MSI para autenticarse en Azure Storage desde aplicaciones que se ejecutan en máquinas virtuales de Azure, aplicaciones de función, conjuntos de escalado de máquinas virtuales, etc. Si usa MSI y aprovecha la capacidad de autenticación de Azure AD, puede evitar el almacenamiento de las credenciales en las aplicaciones que se ejecutan en la nube.  

Para conceder permisos a una identidad de servicio administrada para contenedores de almacenamiento o colas, se asigna un rol RBAC que abarca los permisos de almacenamiento de MSI. Para más información sobre los roles RBAC en el almacenamiento, consulte [Administración de los derechos de acceso a los datos de Azure Storage con RBAC (versión preliminar)](storage-auth-aad-rbac.md). 

> [!IMPORTANT]
> Esta versión preliminar está destinada para usos distintos del de producción. Los Acuerdos de nivel de Servicio (SLA) de producción no estarán disponibles hasta que la integración de Azure AD para Azure Storage se declare disponible con carácter general. Si la integración de Azure AD no se admite todavía para su escenario, siga usando la autorización con clave compartida o tokens de SAS en las aplicaciones. Para más información sobre la versión preliminar, consulte [Authenticate access to Azure Storage using Azure Active Directory (Preview)](storage-auth-aad.md) (Autenticación del acceso en Azure Storage mediante Azure Active Directory [versión preliminar]).
>
> Durante la versión preliminar, las asignaciones de roles RBAC pueden tardar hasta cinco minutos en propagarse.

En este artículo se muestra la autenticación en Azure Storage mediante MSI desde una máquina virtual de Azure.  

## <a name="enable-msi-on-the-vm"></a>Habilitación de MSI en la máquina virtual

Para poder usar MSI para la autenticación en Azure Storage desde la máquina virtual, primero debe habilitar MSI en ella. Para más información sobre cómo habilitar MSI, consulte uno de los siguientes artículos:

- [Azure Portal](https://docs.microsoft.com/azure/active-directory/managed-service-identity/qs-configure-portal-windows-vm)
- [Azure PowerShell](../../active-directory/managed-service-identity/qs-configure-powershell-windows-vm.md)
- [CLI de Azure](../../active-directory/managed-service-identity/qs-configure-cli-windows-vm.md)
- [Plantilla de Azure Resource Manager](../../active-directory/managed-service-identity/qs-configure-template-windows-vm.md)
- [SDK de Azure](../../active-directory/managed-service-identity/qs-configure-sdk-windows-vm.md)

## <a name="get-an-msi-access-token"></a>Obtención de un token de acceso de MSI

Para autenticarse mediante MSI, la aplicación o el script deben adquirir un token de acceso de MSI. Para más información sobre la adquisición de un token de acceso, consulte [How to use an Azure VM Managed Service Identity (MSI) for sign-in and token acquisition](../../active-directory/managed-service-identity/how-to-use-vm-token.md) (Uso de Managed Service Identity (MSI) en una máquina virtual de Azure para la adquisición de tokens).

## <a name="net-code-example-create-a-block-blob"></a>Ejemplo de código .NET: creación de un blob en bloques

En el ejemplo de código se supone que tiene un token de acceso de MSI. El token de acceso se utiliza para autorizar la identidad del servicio administrado para crear un blob en bloques.

### <a name="add-references-and-using-statements"></a>Incorporación de referencias y uso de instrucciones  

En Visual Studio, instale la versión preliminar de la biblioteca de clientes de Azure Storage. En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** y después **Consola del Administrador de paquetes**. En la consola, escriba el siguiente comando:

```
Install-Package https://www.nuget.org/packages/WindowsAzure.Storage/9.2.0  
```

Agregue lo siguiente al código mediante instrucciones:

```dotnet
using Microsoft.WindowsAzure.Storage.Auth;
```

### <a name="create-credentials-from-the-msi-access-token"></a>Creación de credenciales desde el token de acceso de MSI

Para crear el blob en bloques, use la clase **TokenCredentials** que proporciona La versión preliminar del paquete. Construya una nueva instancia de **TokenCredentials** y pase el token de acceso de MSI que obtuvo anteriormente:

```dotnet
// Create storage credentials from your MSI access token.
TokenCredential tokenCredential = new TokenCredential(msiAccessToken);
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
