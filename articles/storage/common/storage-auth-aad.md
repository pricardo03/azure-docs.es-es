---
title: Autenticación del acceso a Azure Storage con Azure Active Directory (versión preliminar) | Microsoft Docs
description: Autentique el acceso a Azure Storage con Azure Active Directory (versión preliminar).
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 06/01/2018
ms.author: tamram
ms.openlocfilehash: 9a0782b96b45d27c9b7e603959ecadf5b2632064
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2018
ms.locfileid: "34737653"
---
# <a name="authenticate-access-to-azure-storage-using-azure-active-directory-preview"></a>Autenticación del acceso a Azure Storage con Azure Active Directory (versión preliminar)

Azure Storage admite la autenticación y autorización con Azure Active Directory (AD) para los servicios Blob y Queue. Con Azure AD, puede usar el control de acceso basado en rol para conceder acceso a los usuarios, grupos o entidades de servicio de la aplicación. 

La autorización de las aplicaciones que tienen acceso a Azure Storage con Azure AD proporciona una mayor seguridad y facilidad de uso en relación con otras opciones de autorización. Aunque puede seguir utilizando la autorización con clave compartida con las aplicaciones, el uso de Azure AD evita la necesidad de almacenar su clave de acceso de cuenta con el código. De forma similar, aún puede usar firmas de acceso compartido (SAS) para conceder acceso específico a los recursos en su cuenta de almacenamiento, pero Azure AD ofrece capacidades similares sin necesidad de administrar tokens de SAS ni preocuparse sobre cómo revocar una SAS en peligro.

## <a name="about-the-preview"></a>Acerca de la versión preliminar

Tenga en cuenta los siguientes puntos en relación con la versión preliminar:

- La integración de Azure AD está disponible para los servicios Blob y Queue únicamente en la versión preliminar.
- La integración de Azure AD está disponible para GPv1, GPv2 y cuentas de almacenamiento de Blob en todas las regiones públicas. 
- Solo se admiten las cuentas de almacenamiento creadas con el modelo de implementación de Resource Manager. 
- La compatibilidad con la información de identidad del autor de la llamada en el registro de Azure Storage Analytics estará disponible próximamente.
- La autorización de Azure AD de acceso a los recursos en cuentas de almacenamiento estándar se admite actualmente. La autorización de acceso a los blobs en páginas en cuentas de almacenamiento premium se admitirá pronto.
- Azure Storage es compatible con roles de control de acceso basado en rol tanto integrados como personalizados. Se pueden asignar roles con ámbito en la suscripción, el grupo de recursos, la cuenta de almacenamiento, o un contenedor individual o una cola.
- Las bibliotecas de cliente de Azure Storage que admiten actualmente la integración de Azure AD incluyen:
    - [.NET](https://www.nuget.org/packages/WindowsAzure.Storage/9.2.0)
    - [Java](http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage) (uso de la versión preliminar 7.1.x)
    - Python
        - [Blob](https://github.com/Azure/azure-storage-python/releases/tag/v1.2.0rc1-blob)
        - [Cola](https://github.com/Azure/azure-storage-python/releases/tag/v1.2.0rc1-queue)
    - [Node.js](https://www.npmjs.com/package/azure-storage)
    - [JavaScript](https://aka.ms/downloadazurestoragejs))

> [!IMPORTANT]
> Esta versión preliminar está destinada para usos distintos del de producción. Los Acuerdos de nivel de servicio (SLA) de producción no estarán disponibles hasta que la integración de Azure AD para Azure Storage se declare disponible con carácter general. Si la integración de Azure AD no se admite todavía para su escenario, siga usando la autorización con clave compartida o tokens de SAS en las aplicaciones.
>
> Durante la versión preliminar, las asignaciones de roles de control de acceso basado en rol pueden tardar hasta cinco minutos en propagarse.
>
> La integración de Azure AD con Azure Storage requiere HTTPS para las operaciones de Azure Storage.

## <a name="get-started-with-azure-ad-for-storage"></a>Introducción a Azure AD para Storage

El primer paso para usar la integración de Azure AD con Azure Storage es asignar roles RBAC para datos de almacenamiento a su entidad de seguridad (usuario, grupo o entidad de servicio de aplicación) o Managed Service Identity (MSI). Los roles RBAC comprenden conjuntos comunes de permisos para contenedores y colas. Para más información acerca de los roles RBAC en Azure Storage, consulte [Manage access rights to storage data with RBAC (Preview)](storage-auth-aad-rbac.md) (Administración de los derechos de acceso a los datos de almacenamiento con RBAC [versión preliminar]).

Para usar Azure AD para autorizar el acceso a los recursos de almacenamiento en sus aplicaciones, debe solicitar un token de acceso de OAuth 2.0 desde el código. Para obtener información sobre cómo solicitar un token de acceso y usarlo para autorizar las solicitudes de Azure Storage, consulte [Authenticate with Azure AD from an Azure Storage application (Preview)](storage-auth-aad-app.md) (Autenticación con Azure AD desde una aplicación de Azure Storage [versión preliminar]). Si utiliza una identidad de Managed Service Identity (MSI), consulte [Authenticate with Azure AD from an Azure VM Managed Service Identity (Preview)](storage-auth-aad-msi.md) (Autenticación con Azure AD desde una identidad de Managed Service Identity de Azure VM [versión preliminar]).

La CLI de Azure y PowerShell admite ahora el inicio de sesión con una identidad de Azure AD. Después de iniciar sesión con una identidad de Azure AD, la sesión se ejecuta con esa identidad. Para obtener más información, consulte [Use an Azure AD identity to access Azure Storage with CLI or PowerShell (Preview)](storage-auth-aad-script.md) (Uso de una identidad de Azure AD para acceder a Azure Storage con la CLI o PowerShell [versión preliminar]).

## <a name="next-steps"></a>Pasos siguientes

Para información adicional sobre la integración de Azure AD para los blobs y las colas de Azure, consulte la entrada de blog del equipo de Azure Storage acerca del [anuncio de la versión preliminar Autenticación de Azure AD para Azure Storage](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
