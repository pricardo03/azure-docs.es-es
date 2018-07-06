---
title: Asignación de un usuario a roles de administrador en Azure Active Directory | Microsoft Docs
description: Procedimiento para cambiar la información administrativa de los usuarios en Azure Active Directory
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: quickstart
ms.date: 06/25/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.openlocfilehash: ec30f1507bfa45c29709a7f4b7dc1e91aa25ca57
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37440755"
---
# <a name="assign-a-user-to-administrator-roles-in-azure-active-directory"></a>Asignación de un usuario a roles de administrador en Azure Active Directory
En este artículo se explica cómo asignar un rol administrativo a un usuario en Azure Active Directory (Azure AD). Para obtener más información sobre cómo agregar nuevos usuarios en su organización, consulte [Incorporación de nuevos usuarios a Azure Active Directory](../add-users-azure-active-directory.md). De forma predeterminada, los usuarios agregados no tienen permisos de administrador, pero puede asignárselos en cualquier momento.

## <a name="assign-a-role-to-a-user"></a>Asignar de un rol a un usuario
1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta que tenga el rol de administrador global o de administrador de rol con privilegios en el directorio.

2. Seleccione **Azure Active Directory**, seleccione **Usuarios** y, a continuación, seleccione un usuario específico de la lista.

    ![Apertura de Administración de usuarios](./media/active-directory-users-assign-role-azure-portal/create-users-user-management.png)

3. Para el usuario seleccionado, seleccione **Rol de directorio**, seleccione **Agregar rol** y, a continuación, seleccione los roles de administrador adecuados de la lista **Roles de directorio**, como **Administrador de acceso condicional**. Para más información acerca de los roles de administrador, consulte [Asignación de roles de administrador en Azure AD](../users-groups-roles/directory-assign-admin-roles.md). 

    ![Asignación de un rol a un usuario](./media/active-directory-users-assign-role-azure-portal/create-users-assign-role.png)

1. Presione **Seleccionar** para guardar.

## <a name="next-steps"></a>Pasos siguientes
* [Guía de inicio rápido: Agregar o eliminar usuarios en Azure Active Directory](add-users-azure-active-directory.md)
* [Administrar perfiles de usuario](active-directory-users-profile-azure-portal.md)
* [Adición de usuarios invitados de otro directorio](../b2b/what-is-b2b.md) 
* [Asignar a un usuario a un rol de Azure AD](active-directory-users-assign-role-azure-portal.md)
* [Restaurar un usuario eliminado](active-directory-users-restore.md)
