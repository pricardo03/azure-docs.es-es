---
title: Autenticación del acceso a blobs y colas de Azure con Azure Active Directory (versión preliminar) | Microsoft Docs
description: Autenticación del acceso a blobs y colas de Azure con Azure Active Directory (versión preliminar).
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 10/15/2018
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 1a9283ad688c63642df880a74cca9189c7c59042
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55456704"
---
# <a name="authenticate-access-to-azure-blobs-and-queues-using-azure-active-directory-preview"></a>Autenticación del acceso a blobs y colas de Azure con Azure Active Directory (versión preliminar)

Azure Storage admite la autenticación y autorización con Azure Active Directory (AD) para los servicios Blob y Queue. Con Azure AD, puede usar el control de acceso basado en rol para conceder acceso a los usuarios, grupos o entidades de servicio de la aplicación. 

Si autentica a los usuarios o a las aplicaciones que usan las credenciales de Azure AD, mejorará la seguridad y le será más fácil usar esta opción en lugar de otros medios de autorización. Aunque puede seguir utilizando la autorización con clave compartida con las aplicaciones, el uso de Azure AD evita la necesidad de almacenar su clave de acceso de cuenta con el código. Asimismo, aún puede usar firmas de acceso compartido (SAS) para conceder acceso pormenorizado a los recursos de su cuenta de almacenamiento, pero Azure AD ofrece funcionalidades similares sin necesidad de administrar tokens de SAS ni de preocuparse sobre cómo revocar una SAS en peligro. Microsoft recomienda usar autenticación de Azure AD para las aplicaciones de Azure Storage cuando sea posible.

[!INCLUDE [storage-auth-aad-note-include](../../../includes/storage-auth-aad-note-include.md)]

## <a name="about-the-preview"></a>Acerca de la versión preliminar

Tenga en cuenta los siguientes puntos en relación con la versión preliminar:

- La integración de Azure AD está disponible para los servicios Blob y Queue únicamente en la versión preliminar.
- La integración de Azure AD está disponible para GPv1, GPv2 y cuentas de almacenamiento de Blob en todas las regiones públicas. 
- Solo se admiten las cuentas de almacenamiento creadas con el modelo de implementación de Resource Manager. 
- La compatibilidad con la información de identidad del autor de la llamada en el registro de Azure Storage Analytics estará disponible próximamente.
- La autorización de Azure AD de acceso a los recursos en cuentas de almacenamiento estándar se admite actualmente. La autorización de acceso a los blobs en páginas en cuentas de almacenamiento premium se admitirá pronto.
- Azure Storage es compatible con roles de control de acceso basado en rol tanto integrados como personalizados. Se pueden asignar roles con ámbito en la suscripción, el grupo de recursos, la cuenta de almacenamiento, o un contenedor individual o una cola.
- Las bibliotecas de cliente de Azure Storage que admiten actualmente la integración de Azure AD incluyen:
    - [.NET](https://www.nuget.org/packages/WindowsAzure.Storage)
    - [Java](http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage)
    - Python
        - [Blob, cola y archivos](https://github.com/Azure/azure-storage-python)
    - [Node.js](https://www.npmjs.com/package/azure-storage)
    - [JavaScript](https://aka.ms/downloadazurestoragejs)

## <a name="get-started-with-azure-ad-for-storage"></a>Introducción a Azure AD para Storage

El primer paso para usar la integración de Azure AD con Azure Storage es asignar roles RBAC para datos de almacenamiento a su entidad de servicio (usuario, grupo o entidad de servicio de aplicación) o identidades administradas de recursos de Azure. Los roles RBAC comprenden conjuntos comunes de permisos para contenedores y colas. Para más información acerca de los roles RBAC en Azure Storage, consulte [Manage access rights to storage data with RBAC (Preview)](storage-auth-aad-rbac.md) (Administración de los derechos de acceso a los datos de almacenamiento con RBAC [versión preliminar]).

Para usar Azure AD para autorizar el acceso a los recursos de almacenamiento en sus aplicaciones, debe solicitar un token de acceso de OAuth 2.0 desde el código. Para obtener información sobre cómo solicitar un token de acceso y usarlo para autorizar las solicitudes de Azure Storage, consulte [Authenticate with Azure AD from an Azure Storage application (Preview)](storage-auth-aad-app.md) (Autenticación con Azure AD desde una aplicación de Azure Storage [versión preliminar]). Si está usando una identidad administrada, consulte [Autenticación del acceso a blobs y colas con identidades administradas de Azure para los recursos de Azure (versión preliminar)](storage-auth-aad-msi.md).

La CLI de Azure y PowerShell admite ahora el inicio de sesión con una identidad de Azure AD. Después de iniciar sesión con una identidad de Azure AD, la sesión se ejecuta con esa identidad. Para obtener más información, consulte [Use an Azure AD identity to access Azure Storage with CLI or PowerShell (Preview)](storage-auth-aad-script.md) (Uso de una identidad de Azure AD para acceder a Azure Storage con la CLI o PowerShell [versión preliminar]).

## <a name="next-steps"></a>Pasos siguientes

Para información adicional sobre la integración de Azure AD para los blobs y las colas de Azure, consulte la entrada de blog del equipo de Azure Storage acerca del [anuncio de la versión preliminar de Autenticación de Azure AD para Azure Storage](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
