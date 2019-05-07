---
title: Autenticar el acceso a los blobs de Azure y colas con Azure Active Directory | Microsoft Docs
description: Autenticar el acceso a los blobs de Azure y colas con Azure Active Directory.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/21/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 66051bd0f8be349f748c72218d538bba273be8f6
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65147265"
---
# <a name="authenticate-access-to-azure-blobs-and-queues-using-azure-active-directory"></a>Autenticar el acceso a los blobs de Azure y colas con Azure Active Directory

Azure Storage admite la autenticación y autorización con Azure Active Directory (AD) para los servicios Blob y Queue. Con Azure AD, puede usar el control de acceso basado en rol para conceder acceso a los usuarios, grupos o entidades de servicio de la aplicación. 

Si autentica a los usuarios o a las aplicaciones que usan las credenciales de Azure AD, mejorará la seguridad y le será más fácil usar esta opción en lugar de otros medios de autorización. Aunque puede seguir utilizando la autorización con clave compartida con las aplicaciones, el uso de Azure AD evita la necesidad de almacenar su clave de acceso de cuenta con el código. Asimismo, aún puede usar firmas de acceso compartido (SAS) para conceder acceso pormenorizado a los recursos de su cuenta de almacenamiento, pero Azure AD ofrece funcionalidades similares sin necesidad de administrar tokens de SAS ni de preocuparse sobre cómo revocar una SAS en peligro. Microsoft recomienda usar autenticación de Azure AD para las aplicaciones de Azure Storage cuando sea posible.

Autenticación y autorización con las credenciales de Azure AD está disponible para todo propósito general y cuentas de Blob storage en todas las regiones públicas y nubes nacionales. Solo las cuentas de almacenamiento crean con la compatibilidad con modelo de implementación de Azure Resource Manager autorización de Azure AD.

## <a name="overview-of-azure-ad-for-blobs-and-queues"></a>Información general de Azure AD para blobs y colas

Cuando una entidad de seguridad (usuario, grupo o aplicación) intenta tener acceso a un recurso de blob o cola, la solicitud debe estar autorizada a menos que sea un blob disponible para el acceso anónimo. Con Azure AD, acceso a un recurso es un proceso de dos pasos. En primer lugar, se autentica la identidad de la entidad de seguridad y se devuelve un token de OAuth 2.0. A continuación, el token se pasa como parte de una solicitud al servicio Blob o cola y el servicio utiliza para autorizar el acceso al recurso especificado.

El paso de autenticación requiere que una aplicación solicita un token de acceso de OAuth 2.0 en tiempo de ejecución. Si una aplicación se está ejecutando desde dentro de una entidad de Azure como una máquina virtual de Azure, un conjunto de escalado de máquina virtual o una aplicación de Azure Functions, puede usar un [identidad administrada](../../active-directory/managed-identities-azure-resources/overview.md) para el acceso a blobs o colas. Para obtener información sobre cómo autorizar las solicitudes realizadas por una identidad administrada para el servicio Azure Blob o cola, vea [autenticar el acceso a los blobs y colas con Azure Active Directory y las identidades administradas para Azure Resources](storage-auth-aad-msi.md).

El paso de autorización requiere que uno o varios roles RBAC se asignen a la entidad de seguridad. Azure Storage proporciona roles RBAC que abarcan conjuntos de permisos para los datos de blob y cola comunes. Los roles que se asignan a una entidad de seguridad determinan los permisos que tendrá la entidad de seguridad. Para más información sobre la asignación de roles de RBAC de Azure Storage, consulte [administrar derechos de acceso a los datos de almacenamiento con RBAC](storage-auth-aad-rbac.md).

Las aplicaciones nativas y aplicaciones web que realizan solicitudes al servicio Azure Blob o cola también se pueden autenticar con Azure AD. Para obtener información sobre cómo solicitar un token de acceso y utilizarla para autorizar las solicitudes de datos de blob o cola, vea [autenticar con Azure AD desde una aplicación de Azure Storage](storage-auth-aad-app.md).

## <a name="assigning-rbac-roles-for-access-rights"></a>Asignación de roles de RBAC para los derechos de acceso

Azure Active Directory (Azure AD) autoriza derechos de acceso a los recursos protegidos mediante el [control de acceso basado en rol (RBAC)](../../role-based-access-control/overview.md). Almacenamiento de Azure define un conjunto de roles RBAC integrados que abarcan conjuntos de permisos utilizados para tener acceso a datos blob y cola comunes. También puede definir roles personalizados para el acceso a los datos de blob y cola.

Cuando un rol de RBAC se asigna a una entidad de seguridad de Azure AD, Azure concede acceso a esos recursos para esa entidad de seguridad. El acceso se puede limitar al nivel de la suscripción, el grupo de recursos, la cuenta de almacenamiento o un contenedor individual o una cola. Una entidad de seguridad de Azure AD puede ser un usuario, un grupo, una entidad de servicio de aplicación, o un [la identidad de los recursos de Azure administrada](../../active-directory/managed-identities-azure-resources/overview.md).

### <a name="built-in-rbac-roles-for-blobs-and-queues"></a>Roles de RBAC integrados para blobs y colas

[!INCLUDE [storage-auth-rbac-roles-include](../../../includes/storage-auth-rbac-roles-include.md)]

Para obtener información sobre cómo asignar un rol RBAC integrado a una entidad de seguridad, consulte uno de los siguientes artículos:

- [Conceder acceso a datos blob y cola de Azure con RBAC en Azure portal](storage-auth-aad-rbac-portal.md)
- [Conceder acceso a datos de blob y cola de Azure con RBAC, mediante la CLI de Azure](storage-auth-aad-rbac-cli.md)
- [Conceder acceso a datos de blob y cola de Azure con RBAC con PowerShell](storage-auth-aad-rbac-powershell.md)

Para más información acerca de cómo se definen los roles integrados para Azure Storage, consulte [Descripción de definiciones de roles](../../role-based-access-control/role-definitions.md#management-and-data-operations-preview). Para obtener información acerca de cómo crear roles personalizados de RBAC, consulte [crear roles personalizados para el Control de acceso](../../role-based-access-control/custom-roles.md).

### <a name="access-permissions-for-data-operations"></a>Permisos de acceso para las operaciones de datos

Para obtener más información sobre los permisos requeridos para llamar a operaciones de servicio Blob o cola específicas, consulte [permisos para llamar al blob y cola de operaciones de datos](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations).

## <a name="resource-scope"></a>Ámbito de recursos

[!INCLUDE [storage-auth-resource-scope-include](../../../includes/storage-auth-resource-scope-include.md)]

## <a name="access-data-with-an-azure-ad-account"></a>Acceso a datos con una cuenta de Azure AD

Acceso a los datos de blob o cola a través del portal de Azure, PowerShell o CLI de Azure se pueden autorizar mediante el uso de la cuenta de Azure AD del usuario o mediante el uso de las claves de acceso de cuenta (autenticación Shared Key).

### <a name="data-access-from-the-azure-portal"></a>Acceso a datos desde el portal de Azure

El portal de Azure puede usar su cuenta de Azure AD o las claves de acceso de cuenta para tener acceso a datos blob y cola de una cuenta de almacenamiento de Azure. Qué esquema de autorización que usa el portal de Azure depende de los roles RBAC que tiene asignados.

Al intentar tener acceso a datos blob o cola, el portal de Azure comprueba primero si se le ha asignado un rol de RBAC con **Microsoft.Storage/storageAccounts/listkeys/action**. Si se ha asignado un rol con esta acción, el portal de Azure usa la clave de cuenta para tener acceso a datos blob y cola a través de la autorización de clave compartida. Si no se ha asignado un rol con esta acción, el portal de Azure intenta obtener acceso a datos mediante la cuenta de Azure AD.

Para tener acceso a blob o cola datos desde el portal de Azure con su cuenta de Azure AD, necesitará permisos para tener acceso a datos blob y cola, y también necesitará permisos para navegar por los recursos de la cuenta de almacenamiento en el portal de Azure. Los roles integrados que proporciona el almacenamiento de Azure conceden acceso a recursos de blob y cola, pero no conceden permisos a los recursos de la cuenta de almacenamiento. Por este motivo, acceso al portal también requiere la asignación de un rol de Azure Resource Manager, como la [lector](../../role-based-access-control/built-in-roles.md#reader) rol, con ámbito en el nivel de la cuenta de almacenamiento o superior. El **lector** rol concede los permisos más restringidos, pero otro rol de Azure Resource Manager que concede acceso a los recursos de administración de cuenta de almacenamiento también es aceptable. Para obtener más información acerca de cómo asignar permisos a los usuarios obtener acceso a datos en el portal de Azure con una cuenta de Azure AD, consulte [conceder acceso a datos blob y cola de Azure con RBAC en Azure portal](storage-auth-aad-rbac-portal.md).

El portal de Azure indica qué esquema de autorización está en uso al navegar a un contenedor o una cola. Para obtener más información sobre el acceso a datos en el portal, consulte [usar Azure portal para acceder a los datos blob o cola](storage-access-blobs-queues-portal.md).

### <a name="data-access-from-powershell-or-azure-cli"></a>Acceso a datos desde PowerShell o CLI de Azure

PowerShell y CLI de Azure admiten el inicio de sesión con credenciales de Azure AD. Después de iniciar sesión, se ejecuta la sesión con esas credenciales. Para obtener más información, consulte [comandos ejecutar la CLI de Azure o PowerShell con credenciales de Azure AD para tener acceso a datos blob o cola](storage-auth-aad-script.md).

## <a name="azure-ad-authentication-over-smb-for-azure-files"></a>Autenticación de Azure AD a través de SMB de Azure Files

Azure Files solo admite autenticación con Azure AD a través de SMB para máquinas virtuales unidas a dominios (versión preliminar). Para obtener información sobre el uso de Azure AD a través de SMB de Azure Files, consulte [autenticación de información general de Azure Active Directory a través de SMB para Azure Files (versión preliminar)](../files/storage-files-active-directory-overview.md).

## <a name="next-steps"></a>Pasos siguientes

- [Autenticar el acceso a los blobs y colas con Azure Active Directory y las identidades administradas para los recursos de Azure](storage-auth-aad-msi.md)
- [Autenticación con Azure Active Directory desde una aplicación para acceder a blobs y colas](storage-auth-aad-app.md)
- [Compatibilidad de Azure Storage con Azure Active Directory en función de control de acceso disponible con carácter general](https://azure.microsoft.com/blog/azure-storage-support-for-azure-ad-based-access-control-now-generally-available/)
