---
title: Autenticar el acceso a los blobs de Azure y colas con Azure Active Directory | Microsoft Docs
description: Autenticar el acceso a los blobs de Azure y colas con Azure Active Directory.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/21/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: ff543b7275ab05a83b1be1d156cbc6059a3b5430
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2019
ms.locfileid: "58369904"
---
# <a name="authenticate-access-to-azure-blobs-and-queues-using-azure-active-directory"></a>Autenticar el acceso a los blobs de Azure y colas con Azure Active Directory

Azure Storage admite la autenticación y autorización con Azure Active Directory (AD) para los servicios Blob y Queue. Con Azure AD, puede usar el control de acceso basado en rol para conceder acceso a los usuarios, grupos o entidades de servicio de la aplicación. 

Si autentica a los usuarios o a las aplicaciones que usan las credenciales de Azure AD, mejorará la seguridad y le será más fácil usar esta opción en lugar de otros medios de autorización. Aunque puede seguir utilizando la autorización con clave compartida con las aplicaciones, el uso de Azure AD evita la necesidad de almacenar su clave de acceso de cuenta con el código. Asimismo, aún puede usar firmas de acceso compartido (SAS) para conceder acceso pormenorizado a los recursos de su cuenta de almacenamiento, pero Azure AD ofrece funcionalidades similares sin necesidad de administrar tokens de SAS ni de preocuparse sobre cómo revocar una SAS en peligro. Microsoft recomienda usar autenticación de Azure AD para las aplicaciones de Azure Storage cuando sea posible.

Autenticación y autorización con las credenciales de Azure AD está disponible para todas las cuentas de Blob storage en todas las regiones públicas, uso general v1 y uso general v2. Solo las cuentas de almacenamiento crean con la compatibilidad con modelo de implementación de Azure Resource Manager autorización de Azure AD.

[!INCLUDE [storage-auth-aad-note-include](../../../includes/storage-auth-aad-note-include.md)]

## <a name="overview-of-azure-ad-for-blobs-and-queues"></a>Información general de Azure AD para blobs y colas

El primer paso para usar la integración de Azure AD con Azure Storage es asignar roles RBAC para datos de almacenamiento a su entidad de servicio (usuario, grupo o entidad de servicio de aplicación) o identidades administradas de recursos de Azure. Los roles RBAC comprenden conjuntos comunes de permisos para contenedores y colas. Para más información sobre la asignación de roles de RBAC de Azure Storage, consulte [administrar derechos de acceso a los datos de almacenamiento con RBAC](storage-auth-aad-rbac.md).

Para usar Azure AD para autorizar el acceso a los recursos de almacenamiento en sus aplicaciones, debe solicitar un token de acceso de OAuth 2.0 desde el código. Para obtener información sobre cómo solicitar un token de acceso y utilizarla para autorizar las solicitudes a Azure Storage, consulte [autenticar con Azure AD desde una aplicación de Azure Storage](storage-auth-aad-app.md). Si está utilizando una identidad administrada, consulte [autenticar el acceso a los blobs y colas de Azure administra las identidades para Azure Resources](storage-auth-aad-msi.md).

La CLI de Azure y PowerShell admite ahora el inicio de sesión con una identidad de Azure AD. Después de iniciar sesión con una identidad de Azure AD, la sesión se ejecuta bajo esa identidad. Para obtener más información, consulte [usar una identidad de Azure AD para acceder a Azure Storage con PowerShell o CLI](storage-auth-aad-script.md).

## <a name="rbac-roles-for-blobs-and-queues"></a>Roles RBAC para blobs y colas

Azure Active Directory (Azure AD) autoriza derechos de acceso a los recursos protegidos mediante el [control de acceso basado en rol (RBAC)](../../role-based-access-control/overview.md). Azure Storage define un conjunto de roles de RBAC integrados que abarcan conjuntos comunes de permisos utilizados para acceder a los contenedores o las colas. 

Cuando un rol de RBAC se asigna a una entidad de seguridad de Azure AD, Azure concede acceso a esos recursos para esa entidad de seguridad. El acceso se puede limitar al nivel de la suscripción, el grupo de recursos, la cuenta de almacenamiento o un contenedor individual o una cola. Una entidad de seguridad de Azure AD puede ser un usuario, un grupo, una entidad de servicio de aplicación, o un [la identidad de los recursos de Azure administrada](../../active-directory/managed-identities-azure-resources/overview.md).

[!INCLUDE [storage-auth-rbac-roles-include](../../../includes/storage-auth-rbac-roles-include.md)]

Para obtener información sobre cómo asignar un rol integrado en el portal de Azure, consulte [conceder acceso a los contenedores de Azure y colas con RBAC en Azure portal](storage-auth-aad-rbac.md).

### <a name="access-permissions-granted-by-rbac-roles"></a>Permisos de acceso concedidos por los roles de RBAC 

Para más información sobre los permisos requeridos para llamar a operaciones de Azure Storage, consulte ///[Permissions for calling REST operations](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations) (Permisos para llamar a operaciones de REST).

## <a name="next-steps"></a>Pasos siguientes

- [Conceder acceso a los contenedores de Azure y colas con RBAC en Azure portal](storage-auth-aad-rbac.md)
- [Autenticación con Azure Active Directory desde una aplicación para acceder a blobs y colas](storage-auth-aad-app.md)
- [Autenticar el acceso a los blobs y colas con identidades administradas para los recursos de Azure](storage-auth-aad-msi.md)
- [Usar una identidad de Azure AD para acceder a Azure Storage con PowerShell o CLI](storage-auth-aad-script.md)