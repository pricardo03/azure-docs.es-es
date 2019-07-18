---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 06/18/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: ff2ed5abbf2ce67a0b96a5da450b83832403db1f
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67269363"
---
[Azure Files](../articles/storage/files/storage-files-introduction.md) admite la autenticación basada en identidades mediante SMB (Bloque de mensajes del servidor) (versión preliminar) mediante [Azure Active Directory (Azure AD) Domain Services](../articles/active-directory-domain-services/overview.md). Las máquinas virtuales (VM) Windows unidas al dominio pueden acceder a los recursos compartidos de archivos de Azure con las credenciales de [Azure AD](../articles/active-directory/fundamentals/active-directory-whatis.md). 

Puede administrar el nivel de acceso a un recurso compartido de Azure Files para una identidad como un usuario o grupo de Azure AD con [control de acceso basado en roles (RBAC)](../articles/role-based-access-control/overview.md). Puede definir roles de RBAC personalizados que engloben los conjuntos de permisos comunes utilizados para acceder a Azure Files. Al asignar un rol de RBAC personalizado a una identidad de Azure AD, a esta última se le concede acceso a un recurso compartido de archivos de Azure según esos permisos.

Como parte de la versión preliminar, Azure Files también admite la conservación, herencia y aplicación de las [DACL de NTFS](https://technet.microsoft.com/library/2006.01.howitworksntfs.aspx) en todos los archivos y directorios de un recurso compartido de archivos. Si copia datos desde un recurso compartido de archivos a Azure Files, o viceversa, puede especificar se mantengan las DACL de NTFS. De este modo puede implementar escenarios de copia de seguridad mediante Azure Files, conservando las DACL de NTFS entre el recurso compartido de archivos local y el recurso compartido de archivos en la nube. 

> [!NOTE]
> - No se admite la autenticación del servicio de dominio de Azure AD para el acceso SMB para VM de Linux. Solo se admiten máquinas virtuales Windows.
> - No se admite la autenticación del servicio de dominio de Azure AD para el acceso SMB para la máquina unida al dominio de Active Directory. Mientras tanto, puede aprovechar [Azure File Sync](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning) para comenzar a migrar sus datos a Azure Files y continuar aplicando el control de acceso mediante credenciales de AD desde sus máquinas unidas al dominio de AD local. 
> - La autenticación del servicio de dominio de Azure AD para acceso SMB solo está disponible para las cuentas de almacenamiento creadas después del 24 de septiembre de 2018.
> - No se admite la autenticación del servicio de dominio de Azure AD para acceso SMB ni NTFS DACL persistentes en recursos compartidos de Azure Files administrados por el servicio Azure File Sync. 
