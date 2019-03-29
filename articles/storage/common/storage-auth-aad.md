---
title: Autenticar el acceso a los blobs de Azure y colas con Azure Active Directory | Microsoft Docs
description: Autenticar el acceso a los blobs de Azure y colas con Azure Active Directory.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/28/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: e72400a759b976b1a2a6864b2fa7d7d91e16c62f
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2019
ms.locfileid: "58619295"
---
# <a name="authenticate-access-to-azure-blobs-and-queues-using-azure-active-directory"></a>Autenticar el acceso a los blobs de Azure y colas con Azure Active Directory

Azure Storage admite la autenticación y autorización con Azure Active Directory (AD) para los servicios Blob y Queue. Con Azure AD, puede usar el control de acceso basado en rol para conceder acceso a los usuarios, grupos o entidades de servicio de la aplicación. 

Si autentica a los usuarios o a las aplicaciones que usan las credenciales de Azure AD, mejorará la seguridad y le será más fácil usar esta opción en lugar de otros medios de autorización. Aunque puede seguir utilizando la autorización con clave compartida con las aplicaciones, el uso de Azure AD evita la necesidad de almacenar su clave de acceso de cuenta con el código. Asimismo, aún puede usar firmas de acceso compartido (SAS) para conceder acceso pormenorizado a los recursos de su cuenta de almacenamiento, pero Azure AD ofrece funcionalidades similares sin necesidad de administrar tokens de SAS ni de preocuparse sobre cómo revocar una SAS en peligro. Microsoft recomienda usar autenticación de Azure AD para las aplicaciones de Azure Storage cuando sea posible.

Autenticación y autorización con las credenciales de Azure AD está disponible para todas las cuentas de Blob storage en todas las regiones públicas, uso general v1 y uso general v2. Solo las cuentas de almacenamiento crean con la compatibilidad con modelo de implementación de Azure Resource Manager autorización de Azure AD.

## <a name="overview-of-azure-ad-for-blobs-and-queues"></a>Información general de Azure AD para blobs y colas

El primer paso para usar la integración de Azure AD con Azure Storage es asignar roles RBAC para datos de almacenamiento a su entidad de servicio (usuario, grupo o entidad de servicio de aplicación) o identidades administradas de recursos de Azure. Los roles RBAC comprenden conjuntos comunes de permisos para contenedores y colas. Para más información sobre la asignación de roles de RBAC de Azure Storage, consulte [administrar derechos de acceso a los datos de almacenamiento con RBAC](storage-auth-aad-rbac.md).

Para usar Azure AD para autorizar el acceso a los recursos de almacenamiento en sus aplicaciones, debe solicitar un token de acceso de OAuth 2.0 desde el código. Para obtener información sobre cómo solicitar un token de acceso y utilizarla para autorizar las solicitudes a Azure Storage, consulte [autenticar con Azure AD desde una aplicación de Azure Storage](storage-auth-aad-app.md). Si está utilizando una identidad administrada, consulte [autenticar el acceso a los blobs y colas de Azure administra las identidades para Azure Resources](storage-auth-aad-msi.md).

La CLI de Azure y PowerShell admite ahora el inicio de sesión con una identidad de Azure AD. Después de iniciar sesión con una identidad de Azure AD, la sesión se ejecuta bajo esa identidad. Para obtener más información, consulte [usar una identidad de Azure AD para acceder a Azure Storage con PowerShell o CLI](storage-auth-aad-script.md).

## <a name="rbac-roles-for-blobs-and-queues"></a>Roles RBAC para blobs y colas

Azure Active Directory (Azure AD) autoriza derechos de acceso a los recursos protegidos mediante el [control de acceso basado en rol (RBAC)](../../role-based-access-control/overview.md). Almacenamiento de Azure define un conjunto de roles RBAC integrados que abarcan conjuntos de permisos utilizados para tener acceso a datos blob y cola comunes. También puede definir roles personalizados para el acceso a los datos de blob y cola.

Cuando un rol de RBAC se asigna a una entidad de seguridad de Azure AD, Azure concede acceso a esos recursos para esa entidad de seguridad. El acceso se puede limitar al nivel de la suscripción, el grupo de recursos, la cuenta de almacenamiento o un contenedor individual o una cola. Una entidad de seguridad de Azure AD puede ser un usuario, un grupo, una entidad de servicio de aplicación, o un [la identidad de los recursos de Azure administrada](../../active-directory/managed-identities-azure-resources/overview.md).

[!INCLUDE [storage-auth-rbac-roles-include](../../../includes/storage-auth-rbac-roles-include.md)]

Para obtener información sobre cómo asignar un RBAC integrado para los recursos de almacenamiento de Azure, consulte uno de los siguientes temas:

- [Conceder acceso a datos blob y cola de Azure con RBAC en Azure portal](storage-auth-aad-rbac-portal.md)
- [Conceder acceso a datos de blob y cola de Azure con RBAC, mediante la CLI de Azure](storage-auth-aad-rbac-cli.md)
- [Conceder acceso a datos de blob y cola de Azure con RBAC con PowerShell](storage-auth-aad-rbac-powershell.md)

### <a name="access-permissions-granted-by-rbac-roles"></a>Permisos de acceso concedidos por los roles de RBAC 

Para más información sobre los permisos requeridos para llamar a operaciones de Azure Storage, consulte ///[Permissions for calling REST operations](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations) (Permisos para llamar a operaciones de REST).

## <a name="resource-scope"></a>Ámbito de recursos

[!INCLUDE [storage-auth-resource-scope-include](../../../includes/storage-auth-resource-scope-include.md)]

## <a name="next-steps"></a>Pasos siguientes

- [Compatibilidad de Azure Storage con Azure Active Directory en función de control de acceso disponible con carácter general](https://azure.microsoft.com/blog/azure-storage-support-for-azure-ad-based-access-control-now-generally-available/)
- [Autenticación con Azure Active Directory desde una aplicación para acceder a blobs y colas](storage-auth-aad-app.md)
- [Autenticar el acceso a los blobs y colas con identidades administradas para los recursos de Azure](storage-auth-aad-msi.md)
- Azure Files solo admite autenticación con Azure AD a través de SMB para máquinas virtuales unidas a dominios (versión preliminar). Para aprender a usar Azure AD a través de SMB de Azure Files, consulte [Introducción a la autenticación de Azure Active Directory a través de SMB para Azure Files (versión preliminar)](../files/storage-files-active-directory-overview.md).