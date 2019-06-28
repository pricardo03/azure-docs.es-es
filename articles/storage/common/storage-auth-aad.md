---
title: Autenticación del acceso a blobs y colas de Azure con Azure Active Directory | Microsoft Docs
description: Autentique el acceso a blobs y colas de Azure con Azure Active Directory.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/21/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 66051bd0f8be349f748c72218d538bba273be8f6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65147265"
---
# <a name="authenticate-access-to-azure-blobs-and-queues-using-azure-active-directory"></a>Autenticación del acceso a blobs y colas de Azure con Azure Active Directory

Azure Storage admite la autenticación y autorización con Azure Active Directory (AD) para los servicios Blob y Queue. Con Azure AD, puede usar el control de acceso basado en rol para conceder acceso a los usuarios, grupos o entidades de servicio de la aplicación. 

Si autentica a los usuarios o a las aplicaciones que usan las credenciales de Azure AD, mejorará la seguridad y le será más fácil usar esta opción en lugar de otros medios de autorización. Aunque puede seguir utilizando la autorización con clave compartida con las aplicaciones, el uso de Azure AD evita la necesidad de almacenar su clave de acceso de cuenta con el código. Asimismo, aún puede usar firmas de acceso compartido (SAS) para conceder acceso pormenorizado a los recursos de su cuenta de almacenamiento, pero Azure AD ofrece funcionalidades similares sin necesidad de administrar tokens de SAS ni de preocuparse sobre cómo revocar una SAS en peligro. Microsoft recomienda usar autenticación de Azure AD para las aplicaciones de Azure Storage cuando sea posible.

La autenticación y la autorización con credenciales de Azure AD está disponible para todas las cuentas de propósito general y de Blob Storage en todas las regiones públicas y nubes nacionales. Solo las cuentas de almacenamiento creadas con el modelo de implementación de Azure Resource Manager admiten la autorización de Azure AD.

## <a name="overview-of-azure-ad-for-blobs-and-queues"></a>Información general sobre Azure AD para blobs y colas

Cuando una entidad de seguridad (un usuario, un grupo o una aplicación) intenta acceder a un recurso de blob o cola, la solicitud debe autorizarse, a menos que sea un blob disponible para acceso anónimo. Con Azure AD, el acceso a un recurso es un proceso de dos pasos. En primer lugar, se autentica la identidad de la entidad de seguridad y se devuelve un token de OAuth 2.0. Luego el token se pasa como parte de una solicitud a Blob Service o Queue Service y el servicio lo usa para autorizar el acceso al recurso especificado.

El paso de autenticación exige que una aplicación solicite un token de acceso de OAuth 2.0 en tiempo de ejecución. Si una aplicación se está ejecutando desde una entidad de Azure como una máquina virtual de Azure, un conjunto de escalado de máquinas virtuales o una aplicación de Azure Functions, puede usar una [identidad administrada](../../active-directory/managed-identities-azure-resources/overview.md) para el acceso a blobs o colas. Para obtener más información sobre cómo autorizar solicitudes realizadas por una identidad administrada a Azure Blob Service o Queue Service, vea [Autenticación del acceso a blobs y colas con Azure Active Directory e identidades administradas para los recursos de Azure](storage-auth-aad-msi.md).

El paso de autorización exige que se asignen uno o varios roles RBAC a la entidad de seguridad. Azure Storage proporciona roles RBAC que abarcan conjuntos comunes de permisos para datos de blobs y colas. Los roles que se asignan a una entidad de seguridad determinan los permisos que tiene esa entidad de seguridad. Para obtener más información sobre la asignación de roles RBAC en Azure Storage, vea [Administración de los derechos de acceso a los datos de almacenamiento con RBAC](storage-auth-aad-rbac.md).

Las aplicaciones nativas y las aplicaciones web que realizan solicitudes a Azure Blob Service o Queue Service también se pueden autenticar con Azure AD. Para obtener información sobre cómo solicitar un token de acceso y usarlo para autorizar solicitudes de datos de blobs o colas, vea [Autenticación con Azure Active Directory desde una aplicación de Azure Storage](storage-auth-aad-app.md).

## <a name="assigning-rbac-roles-for-access-rights"></a>Asignación de roles RBAC para derechos de acceso

Azure Active Directory (Azure AD) autoriza derechos de acceso a los recursos protegidos mediante el [control de acceso basado en rol (RBAC)](../../role-based-access-control/overview.md). Azure Storage define un conjunto de roles RBAC integrados que abarcan conjuntos comunes de permisos que se emplean para acceder a los datos de blobs o colas. También puede definir roles personalizados para el acceso a datos de blobs y colas.

Cuando un rol RBAC se asigna a una entidad de seguridad de Azure AD, Azure concede acceso a esos recursos a esa entidad de seguridad. El acceso se puede limitar al nivel de la suscripción, el grupo de recursos, la cuenta de almacenamiento o un contenedor individual o una cola. Una entidad de seguridad de Azure AD puede ser un usuario, un grupo, una entidad de servicio de aplicación o una [identidad administrada para recursos de Azure](../../active-directory/managed-identities-azure-resources/overview.md).

### <a name="built-in-rbac-roles-for-blobs-and-queues"></a>Roles RBAC integrados para blobs y colas

[!INCLUDE [storage-auth-rbac-roles-include](../../../includes/storage-auth-rbac-roles-include.md)]

Para obtener información sobre cómo asignar un rol RBAC integrado a una entidad de seguridad, vea alguno de los siguientes artículos:

- [Concesión de acceso a datos de blob y cola de Azure con RBAC en Azure Portal](storage-auth-aad-rbac-portal.md)
- [Concesión de acceso a datos de blob y cola de Azure con RBAC mediante la CLI de Azure](storage-auth-aad-rbac-cli.md)
- [Concesión de acceso a datos de blob y cola de Azure con RBAC mediante PowerShell](storage-auth-aad-rbac-powershell.md)

Para más información acerca de cómo se definen los roles integrados para Azure Storage, consulte [Descripción de definiciones de roles](../../role-based-access-control/role-definitions.md#management-and-data-operations-preview). Para obtener más información sobre la creación de roles RBAC personalizados, vea [Creación de roles personalizados para el control de acceso basado en roles de Azure](../../role-based-access-control/custom-roles.md).

### <a name="access-permissions-for-data-operations"></a>Permisos de acceso para operaciones de datos

Para obtener detalles sobre los permisos necesarios para llamar a operaciones concretas de Blob Service o Queue Service, vea [Permisos para llamar a operaciones de datos de blobs y colas](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations).

## <a name="resource-scope"></a>Ámbito de recursos

[!INCLUDE [storage-auth-resource-scope-include](../../../includes/storage-auth-resource-scope-include.md)]

## <a name="access-data-with-an-azure-ad-account"></a>Acceso a datos con una cuenta de Azure AD

El acceso a los datos de blobs o colas a través de Azure Portal, PowerShell o la CLI de Azure se puede autorizar mediante la cuenta de Azure AD del usuario o las claves de acceso de cuenta (autenticación de clave compartida).

### <a name="data-access-from-the-azure-portal"></a>Acceso a datos desde Azure Portal

Azure Portal puede usar la cuenta de Azure AD o las claves de acceso de cuenta para acceder a datos de blobs y colas en una cuenta de Azure Storage. El esquema de autorización que use Azure Portal depende de los roles RBAC que tenga asignados.

Si intenta acceder a datos de blobs o colas, Azure Portal primero comprueba si tiene asignado un rol RBAC con **Microsoft.Storage/storageAccounts/listkeys/action**. Si tiene un rol asignado con esta acción, Azure Portal usa la clave de cuenta para acceder a los datos de blobs y colas mediante autorización de clave compartida. Si no tiene un rol asignado con esta acción, Azure Portal intenta acceder a los datos mediante la cuenta de Azure AD.

Para acceder a datos de blobs o colas desde Azure Portal con la cuenta de Azure AD, necesita permisos para acceder a datos de blobs y colas y, además, permisos para examinar los recursos de la cuenta de almacenamiento en Azure Portal. Los roles integrados que proporciona Azure Storage conceden acceso a recursos de blobs y colas, pero no conceden permisos a los recursos de la cuenta de almacenamiento. Por este motivo, el acceso al portal también requiere la asignación de un rol de Azure Resource Manager, como el rol [Lector](../../role-based-access-control/built-in-roles.md#reader), con ámbito limitado al nivel de la cuenta de almacenamiento o superior. El rol **Lector** concede los permisos más restringidos, pero otro rol de Azure Resource Manager que conceda acceso a los recursos de administración de la cuenta de almacenamiento también es aceptable. Para obtener más información sobre cómo asignar permisos a los usuarios para el acceso a los datos de Azure Portal con una cuenta de Azure AD, vea [Concesión de acceso a datos de blob y cola de Azure con RBAC en Azure Portal](storage-auth-aad-rbac-portal.md).

Azure Portal indica qué esquema de autorización se está usando al examinar un contenedor o una cola. Para obtener más información sobre el acceso a datos en el portal, vea [Usar Azure Portal para acceder a datos de blob o cola](storage-access-blobs-queues-portal.md).

### <a name="data-access-from-powershell-or-azure-cli"></a>Acceso a datos desde PowerShell o la CLI de Azure

PowerShell y la CLI de Azure admiten el inicio de sesión con credenciales de Azure AD. Después de iniciar sesión, la sesión se ejecuta con esas credenciales. Para obtener más información, vea [Ejecución de comandos de la CLI de Azure o PowerShell con credenciales de Azure AD para acceder a datos de blob o cola](storage-auth-aad-script.md).

## <a name="azure-ad-authentication-over-smb-for-azure-files"></a>Autenticación de Azure AD a través de SMB para Azure Files

Azure Files solo admite autenticación con Azure AD a través de SMB para máquinas virtuales unidas a dominios (versión preliminar). Para obtener información sobre el empleo de Azure AD a través de SMB para Azure Files, vea [Introducción a la autenticación de Azure Active Directory a través de SMB para Azure Files, versión preliminar](../files/storage-files-active-directory-overview.md).

## <a name="next-steps"></a>Pasos siguientes

- [Autenticación del acceso a blobs y colas con Azure Active Directory e identidades administradas para los recursos de Azure](storage-auth-aad-msi.md)
- [Autenticación con Azure Active Directory desde una aplicación para acceder a blobs y colas](storage-auth-aad-app.md)
- [Compatibilidad de Azure Storage con el control de acceso basado en Azure Active Directory disponible con carácter general](https://azure.microsoft.com/blog/azure-storage-support-for-azure-ad-based-access-control-now-generally-available/)
