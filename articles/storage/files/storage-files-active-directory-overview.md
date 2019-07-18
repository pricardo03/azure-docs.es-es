---
title: 'Introducción de la autenticación de Azure Active Directory sobre SMB para Azure Files (versión preliminar): Azure Storage'
description: Azure Files admite la autenticación basada en identidades sobre SMB (Bloque de mensajes del servidor) (versión preliminar) mediante Azure Active Directory (Azure AD) Domain Services. Las máquinas virtuales Windows unidas al dominio pueden entonces acceder a los recursos compartidos de archivos de Azure con las credenciales de Azure AD.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 06/18/2019
ms.author: rogarana
ms.openlocfilehash: 21087424be1a7a3edfe2dddcbec830bd74559b23
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67269371"
---
# <a name="overview-of-azure-files-azure-active-directory-domain-service-aad-ds-authentication-support-for-smb-access-preview"></a>Introducción del soporte de la autenticación de Azure Active Directory sobre SMB para Azure Files (versión preliminar)
[!INCLUDE [storage-files-aad-auth-include](../../../includes/storage-files-aad-auth-include.md)]

Para aprender a habilitar la autenticación de AAD DS para Azure Files, consulte [Habilitación de la autenticación de Azure Active Directory a través de SMB para Azure Files (versión preliminar)](storage-files-active-directory-enable.md).

## <a name="glossary"></a>Glosario 
Es útil entender algunos términos clave relacionados con la autenticación de Azure AD Domain Service sobre SMB para Azure Files:

-   **Azure Active Directory (Azure AD)**  
    Azure Active Directory (Azure AD) es el directorio multiinquilino basado en la nube y el servicio de administración de identidades de Microsoft. Azure AD combina servicios de directorio fundamentales, administración del acceso a las aplicaciones y protección de identidades en una única solución. Para más información, consulte [¿Qué es Azure Active Directory?](../../active-directory/fundamentals/active-directory-whatis.md)

-   **Azure AD Domain Services**  
    Azure AD Domain Services proporciona servicios de dominio administrados como, por ejemplo, unión a un dominio, directivas de grupo, LDAP y autenticación Kerberos/NTLM. Estos servicios son totalmente compatibles con Windows Server Active Directory. Para más información, consulte [Azure Active Directory (AD) Domain Services](../../active-directory-domain-services/overview.md).

-   **Control de acceso basado en rol (RBAC) de Azure**  
    El control de acceso basado en roles (RBAC) de Azure permite realizar una administración detallada del acceso para Azure. Con RBAC, puede administrar el acceso a los recursos mediante la concesión a los usuarios del menor número de permisos necesarios para realizar su trabajo. Para más información sobre RBAC, consulte [¿Qué es el control de acceso basado en rol (RBAC) en Azure?](../../role-based-access-control/overview.md)

-   **Autenticación Kerberos**

    Kerberos es un protocolo de autenticación que se utiliza para comprobar la identidad de un usuario o un host. Para más información sobre Kerberos, consulte [Introducción a la autenticación Kerberos](https://docs.microsoft.com/windows-server/security/kerberos/kerberos-authentication-overview).

-  **Protocolo Bloque de mensajes del servidor (SMB)**  
    SMB es un protocolo de uso compartido de archivos de red estándar del sector. SMB también se conoce como sistema de archivos de Internet común o CIFS. Para más información sobre SMB, consulte [Microsoft SMB Protocol and CIFS Protocol Overview](https://docs.microsoft.com/windows/desktop/FileIO/microsoft-smb-protocol-and-cifs-protocol-overview) (Introducción a los protocolos CIFS y SMB de Microsoft).

## <a name="advantages-of-azure-ad-domain-service-authentication"></a>Ventajas de la autenticación de Azure AD Domain Service
La autenticación de Azure AD Domain Service para Azure Files ofrece varias ventajas respecto al uso de la autenticación de clave compartida:

-   **Ampliar la experiencia tradicional de acceso compartido de archivos basada en identidad en la nube con Azure AD y Azure AD Domain Service**  
    Si tiene previsto llevar la aplicación a la nube, mediante el reemplazo de los servidores de archivos tradicionales con Azure Files, tal vez quiera que la aplicación se autentique con las credenciales de Azure AD para acceder a los datos de los archivos. Azure Files permite el uso de credenciales de Azure AD para acceder a Azure Files a través de SMB desde una VM de Windows unida a un dominio de AAD DS. También puede optar por sincronizar todos los objetos de Active Directory locales con Azure AD para conservar los nombres de usuario, las contraseñas y otras asignaciones de grupo.

-   **Aplicar el control de acceso granular en recursos compartidos de archivos de Azure**  
    Puede conceder permisos a una identidad específica en el nivel de recurso compartido, directorio o archivo. Por ejemplo, suponga que tiene varios equipos que utilizan un solo recurso compartido de archivos de Azure para la colaboración en proyectos. Puede conceder a todos los equipos acceso a directorios no confidenciales, al tiempo que limita el acceso a directorios que contienen datos financieros confidenciales únicamente a su equipo de financiero. 

-   **Realizar una copia de seguridad de las listas de control de acceso junto con los datos**  
    Puede usar Azure Files para realizar copias de seguridad de los recursos compartidos de archivos locales existentes. Azure Files conserva las listas de control de acceso junto con los datos cuando se hace una copia de seguridad de un recurso compartido en Azure Files sobre SMB.

## <a name="how-it-works"></a>Cómo funciona
Azure Files utiliza Azure AD Domain Services para admitir la autenticación Kerberos con credenciales de Azure AD desde máquinas virtuales unidas al dominio. Antes de poder usar Azure AD con Azure Files, primero debe habilitar Azure AD Domain Services y unirse al dominio de las máquinas virtuales desde las que piensa acceder a los datos del archivo. La máquina virtual unida a un dominio debe residir en la misma red virtual que Azure AD Domain Services. 

Cuando una identidad asociada con una aplicación que se ejecuta en una máquina virtual intenta acceder a los datos de Azure Files, la solicitud se envía a Azure AD Domain Services para autenticar la identidad. Si la autenticación es correcta, Azure AD Domain Services devuelve un token de Kerberos. La aplicación envía una solicitud que incluye el token de Kerberos, y Azure Files usa ese token para autorizar la solicitud. Azure Files recibe el token solo y no conserva las credenciales de Azure AD.

![Captura de pantalla que muestra un diagrama de autenticación de Azure AD sobre SMB](media/storage-files-active-directory-overview/azure-active-directory-over-smb-for-files-overview.png)

### <a name="enable-azure-ad-domain-service-authentication-for-smb-access"></a>Habilitación de la autenticación de Azure AD Domain Service para acceso SMB
Puede habilitar la autenticación de Azure AD Domain Service para Azure Files en las cuentas de almacenamiento nuevas y existentes creadas después del 24 de septiembre de 2018. 

Antes de habilitar esta característica, compruebe que Azure AD Domain Services se haya implementado para el inquilino principal de Azure AD con el que está asociada la cuenta de almacenamiento. Si aún no ha configurado Azure AD Domain Services, siga la guía paso a paso proporcionada en [Habilitación de Azure Active Directory Domain Services mediante Azure Portal](../../active-directory-domain-services/create-instance.md).

La implementación de Azure AD Domain Services normalmente tarda de 10 a 15 minutos. Después de implementar Azure AD Domain Services, puede habilitar la autenticación de Azure AD sobre SMB para Azure Files. Para obtener más información, vea [Habilitación de la autenticación de Azure Active Directory a través de SMB para Azure Files (versión preliminar)](storage-files-active-directory-enable.md). 

### <a name="configure-share-level-permissions-for-azure-files"></a>Configuración de los permisos de nivel de recurso compartido para Azure Files
Cuando se ha habilitado la autenticación de Azure AD Domain Service, puede configurar roles de RBAC personalizados para las identidades de Azure AD y asignar derechos de acceso a cualquier recurso compartido de archivos en la cuenta de almacenamiento.

Cuando una aplicación que se ejecuta en una máquina virtual unida a un dominio intenta montar un recurso compartido de archivos de Azure o acceder a un directorio o archivo, se comprueban las credenciales de Azure AD de la aplicación para garantizar los permisos adecuados de nivel de recurso compartido y los permisos NTFS. Para más información sobre cómo configurar permisos de nivel de recurso compartido, consulte [Habilitación de la autenticación de Azure Active Directory Domain Service a través de SMB para Azure Files (versión preliminar)](storage-files-active-directory-enable.md).

### <a name="configure-directory--or-file-level-permissions-for-azure-files"></a>Configuración de los permisos de nivel de archivo o directorio para Azure Files 
Azure Files aplica permisos de archivo NTFS estándar en el nivel de archivo y directorio, incluido el directorio raíz. La configuración de permisos de nivel de archivo o directorio solo se admite sobre SMB. Monte el recurso compartido de archivos de destino de la máquina virtual y configure los permisos mediante el comando de Windows [icacls](https://docs.microsoft.com/windows-server/administration/windows-commands/icacls) o [Set-ACL](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-acl). 

> [!NOTE]
> No se admite la configuración de los permisos NTFS mediante el explorador de archivos de Windows en la versión preliminar.

### <a name="use-the-storage-account-key-for-superuser-permissions"></a>Uso de la clave de cuenta de almacenamiento para los permisos de superusuario 
Un usuario que posee la clave de cuenta de almacenamiento puede acceder a Azure Files con permisos de superusuario. Los permisos de superusuario superan todas las restricciones de control de acceso configuradas a nivel de recurso compartido con RBAC e impuestas por Azure AD. Los permisos de superusuario se necesitan para montar un recurso compartido de archivos de Azure. 

> [!IMPORTANT]
> Como parte de las prácticas recomendadas de seguridad, evite el uso compartido de las claves de la cuenta de almacenamiento y aproveche los permisos de Azure AD siempre que sea posible.

### <a name="preserve-directory-and-file-acls-for-data-import-to-azure-file-shares"></a>Conservación de las listas de control de acceso de archivos y directorios para la importación de datos a los recursos compartidos de archivos de Azure
Azure Files admite la conservación de las listas de control de acceso de directorios o archivos al copiar los datos a los recursos compartidos de archivos de Azure. Puede copiar las listas de control de acceso en un directorio o archivo a Azure Files. Por ejemplo, puede usar [robocopy](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy) con marca `/copy:s` para copiar tanto los datos como las listas de control de acceso en un recurso compartido de archivos de Azure. La conservación de ACL está activada de forma predeterminada y no es necesario habilitar explícitamente la característica de autenticación de Azure AD Domain Services en su cuenta de almacenamiento. 

## <a name="pricing"></a>Precios
No hay ningún cargo adicional por servicio para habilitar la autenticación de Azure AD sobre SMB en la cuenta de almacenamiento. Para más información sobre los precios, consulte las páginas [Precios de Azure Files](https://azure.microsoft.com/pricing/details/storage/files/) y [Precios de Azure AD Domain Services](https://azure.microsoft.com/pricing/details/active-directory-ds/).

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre Azure Files y la autenticación de Azure AD a través de SMB, consulte estos recursos:

- [Introducción a Azure Files](storage-files-introduction.md)
- [Habilitación de la autenticación de Azure Active Directory a través de SMB para Azure Files (versión preliminar)](storage-files-active-directory-enable.md)
- [P+F](storage-files-faq.md)
