---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 09/19/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 903074c78180ab2cd755abcf4207232f2851804e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47019700"
---
[Azure Files](../articles/storage/files/storage-files-introduction.md) admite la autenticación basada en identidades mediante SMB (Bloque de mensajes del servidor) (versión preliminar) mediante [Azure Active Directory (Azure AD) Domain Services](../articles/active-directory-domain-services/active-directory-ds-overview.md). Las máquinas virtuales (VM) Windows unidas al dominio pueden acceder a los recursos compartidos de archivos de Azure con las credenciales de [Azure AD](../articles/active-directory/fundamentals/active-directory-whatis.md). 

Azure AD autentica una identidad como un usuario, grupo o entidad de servicio con el [control de acceso basado en roles (RBAC)](../articles/role-based-access-control/overview.md). Puede definir roles de RBAC personalizados que engloben los conjuntos de permisos comunes utilizados para acceder a Azure Files. Al asignar un rol de RBAC personalizado a una identidad de Azure AD, a esta última se le concede acceso a un recurso compartido de archivos de Azure según esos permisos.

Como parte de la versión preliminar, Azure Files también admite la conservación, herencia y aplicación de las [DACL de NTFS](https://technet.microsoft.com/library/2006.01.howitworksntfs.aspx) en todos los archivos y directorios de un recurso compartido de archivos. Si copia datos desde un recurso compartido de archivos a Azure Files, o viceversa, puede especificar se mantengan las DACL de NTFS. De este modo puede implementar escenarios de copia de seguridad mediante Azure Files, conservando las DACL de NTFS entre el recurso compartido de archivos local y el recurso compartido de archivos en la nube. 

> [!NOTE]
> La autenticación de Azure AD a través de SMB no se admite en máquinas virtuales Linux para la versión preliminar. Solo se admiten máquinas virtuales Windows Server.
>
> La autenticación de Azure AD solo está disponible para las cuentas de almacenamiento creadas después del 24 de septiembre de 2018.
