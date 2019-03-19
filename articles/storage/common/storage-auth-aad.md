---
title: Autenticación del acceso a blobs y colas de Azure con Azure Active Directory (versión preliminar) | Microsoft Docs
description: Autenticación del acceso a blobs y colas de Azure con Azure Active Directory (versión preliminar).
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 02/13/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: dcf7d237c8cfbf52a804e428d84fff0bb328c7c8
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58012109"
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
    - [Java](https://mvnrepository.com/artifact/com.microsoft.azure/azure-storage)
    - Python
        - [Blob, cola y archivos](https://github.com/Azure/azure-storage-python)
    - [Node.js](https://www.npmjs.com/package/azure-storage)
    - [JavaScript](https://aka.ms/downloadazurestoragejs)

## <a name="overview-of-azure-ad-for-storage"></a>Información general de Azure AD para el almacenamiento

El primer paso para usar la integración de Azure AD con Azure Storage es asignar roles RBAC para datos de almacenamiento a su entidad de servicio (usuario, grupo o entidad de servicio de aplicación) o identidades administradas de recursos de Azure. Los roles RBAC comprenden conjuntos comunes de permisos para contenedores y colas. Para más información sobre la asignación de roles de RBAC de Azure Storage, consulte [administrar derechos de acceso a los datos de almacenamiento con RBAC (versión preliminar)](storage-auth-aad-rbac.md).

Para usar Azure AD para autorizar el acceso a los recursos de almacenamiento en sus aplicaciones, debe solicitar un token de acceso de OAuth 2.0 desde el código. Para obtener información sobre cómo solicitar un token de acceso y usarlo para autorizar las solicitudes de Azure Storage, consulte [Authenticate with Azure AD from an Azure Storage application (Preview)](storage-auth-aad-app.md) (Autenticación con Azure AD desde una aplicación de Azure Storage [versión preliminar]). Si está usando una identidad administrada, consulte [Autenticación del acceso a blobs y colas con identidades administradas de Azure para los recursos de Azure (versión preliminar)](storage-auth-aad-msi.md).

La CLI de Azure y PowerShell admite ahora el inicio de sesión con una identidad de Azure AD. Después de iniciar sesión con una identidad de Azure AD, la sesión se ejecuta con esa identidad. Para obtener más información, consulte [Use an Azure AD identity to access Azure Storage with CLI or PowerShell (Preview)](storage-auth-aad-script.md) (Uso de una identidad de Azure AD para acceder a Azure Storage con la CLI o PowerShell [versión preliminar]).

## <a name="rbac-roles-for-blobs-and-queues"></a>Roles RBAC para blobs y colas

Azure Active Directory (Azure AD) autoriza derechos de acceso a los recursos protegidos mediante el [control de acceso basado en rol (RBAC)](../../role-based-access-control/overview.md). Azure Storage define un conjunto de roles de RBAC integrados que abarcan conjuntos comunes de permisos utilizados para acceder a los contenedores o las colas. 

Cuando un rol de RBAC se asigna a una entidad de seguridad de Azure AD, Azure concede acceso a esos recursos para esa entidad de seguridad. El acceso se puede limitar al nivel de la suscripción, el grupo de recursos, la cuenta de almacenamiento o un contenedor individual o una cola. Una entidad de seguridad de Azure AD puede ser un usuario, un grupo, una entidad de servicio de aplicación, o un [la identidad de los recursos de Azure administrada](../../active-directory/managed-identities-azure-resources/overview.md). 

[!INCLUDE [storage-auth-rbac-roles-include](../../../includes/storage-auth-rbac-roles-include.md)]

Para obtener información sobre cómo asignar un rol integrado en el portal de Azure, consulte [conceder acceso a los contenedores de Azure y colas con RBAC en Azure portal (versión preliminar)](storage-auth-aad-rbac.md).

### <a name="access-permissions-granted-by-rbac-roles"></a>Permisos de acceso concedidos por los roles de RBAC 

En la tabla siguiente se resume los derechos de acceso concedidos por los roles integrados para los distintos niveles de ámbito:

|Ámbito|Propietario de datos de blob|Colaborador de datos de blob|Lector de datos de blob|Colaborador de datos de cola|Lector de datos de cola|
|---|---|---|---|---|---|
|Nivel de suscripción|Acceso de lectura y escritura, y administración del control de acceso POSIX a todos los contenedores y blobs de la suscripción|Acceso de lectura/escritura a todos los contenedores y blobs en la suscripción| Acceso de lectura a todos los contenedores y blobs en la suscripción|Acceso de lectura/escritura a todas las colas en la suscripción|Acceso de lectura a todas las colas en la suscripción|
|Nivel de grupo de recursos|Acceso de lectura y escritura, y administración del control de acceso POSIX a todos los contenedores y blobs del grupo de recursos|Acceso de lectura/escritura a todos los contenedores y blobs en el grupo de recursos|Acceso de lectura a todos los contenedores y blobs en el grupo de recursos|Acceso de lectura/escritura para todas las colas en el grupo de recursos|Acceso de lectura a todas las colas en el grupo de recursos|
|Nivel de cuenta de almacenamiento|Acceso de lectura y escritura, y administración del control de acceso POSIX a todos los contenedores y blobs de la cuenta de almacenamiento|Acceso de lectura/escritura a todos los contenedores y blobs en la cuenta de almacenamiento|Acceso de lectura a todos los contenedores y blobs en la cuenta de almacenamiento|Acceso de lectura/escritura a todas las colas en la cuenta de almacenamiento|Acceso de lectura a todas las colas en la cuenta de almacenamiento|
|Nivel de contenedor/cola|Acceso de lectura y escritura, y administración del control de acceso POSIX a los contenedores especificados y sus blobs.|Acceso de lectura/escritura al contenedor especificado y sus blobs|Acceso de lectura al contenedor especificado y sus blobs|Acceso de lectura/escritura a la cola especificada|Acceso de lectura a la cola especificada|

Para más información sobre los permisos requeridos para llamar a operaciones de Azure Storage, consulte ///[Permissions for calling REST operations](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations) (Permisos para llamar a operaciones de REST).

## <a name="next-steps"></a>Pasos siguientes

Para información adicional sobre la integración de Azure AD para los blobs y las colas de Azure, consulte la entrada de blog del equipo de Azure Storage acerca del [anuncio de la versión preliminar de Autenticación de Azure AD para Azure Storage](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
