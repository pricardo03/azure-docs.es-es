---
title: Asignación de roles de directorio a los usuarios mediante Azure AD PIM | Microsoft Docs
description: Aprenda a asignar roles de directorio a los usuarios mediante Azure Active Directory Privileged Identity Management y Azure Portal.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: protection
ms.date: 07/23/2018
ms.author: rolyon
ms.openlocfilehash: 1834addb4e51030afda43a2d7acad5d7ffc1889a
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2018
ms.locfileid: "39226703"
---
# <a name="assign-directory-roles-to-users-using-azure-ad-pim"></a>Asignación de roles de directorio a los usuarios mediante Azure AD PIM

Con Azure Active Directory (Azure AD), un administrador global puede realizar asignaciones de roles de directorio **permanentes**. Estas asignaciones de roles se pueden crear mediante [Azure Portal](../users-groups-roles/directory-assign-admin-roles.md) o mediante [comandos de PowerShell](/powershell/module/azuread#directory_roles).

El servicio Azure AD Privileged Identity Management (PIM) permite también a los administradores de roles con privilegios realizar asignaciones de roles de directorio permanentes. Además, los administradores de rol con privilegios pueden hacer que los usuarios sean **aptos** para roles de directorio. Un administrador apto puede activar el rol cuando lo necesite y, cuando termina, sus permisos caducan. Para información sobre los roles que puede administrar mediante PIM, consulte [Rol administrativo diferente en Azure Active Directory PIM](pim-roles.md).

## <a name="make-a-user-eligible-for-a-role"></a>Hacer que un usuario sea apto para un rol

Siga estos pasos para hacer que un usuario sea apto para un rol de directorio de Azure AD.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) con un usuario que sea miembro del [con Administrador de rol con privilegios](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator).

    Para información sobre cómo conceder a otro usuario acceso para administrar PIM, consulte [cómo conceder acceso a PIM](pim-how-to-give-access-to-pim.md).

1. Abra **Azure AD Privileged Identity Management**.

    Si aún no ha habilitado PIM en Azure Portal, vaya a [Comenzar a usar Azure AD Privileged Identity Management](pim-getting-started.md).

1. Haga clic en **Roles de directorio de Azure AD**.

1. Haga clic en **Rol (versión preliminar)** o **Miembros**.

    ![Roles de directorio de Azure AD](./media/pim-how-to-add-role-to-user/pim-directory-roles.png)

1. Haga clic en **Agregar miembro** para abrir Agregar miembros administrados.

1. Haga clic en **Seleccionar un rol**, haga clic en un rol que desee administrar y luego haga clic en **Seleccionar**.

    ![Seleccione un rol.](./media/pim-how-to-add-role-to-user/pim-select-a-role.png)

1. Haga clic en **Seleccionar miembros**, seleccione los usuarios que desea asignar al rol y, a continuación, haga clic en **Seleccionar**.

    ![Seleccione un rol.](./media/pim-how-to-add-role-to-user/pim-select-members.png)

1. En Agregar miembros administrados, haga clic en **Aceptar** para agregar el usuario al rol.

     Cuando el rol esté asignado, el usuario que seleccionó aparecerá en la lista de miembros como **apto** para el rol.

    ![Usuario apto para un rol](./media/pim-how-to-add-role-to-user/pim-directory-role-eligible.png)

1. Ahora que el usuario es apto para el rol, hágale saber que puede activarlo de acuerdo con las instrucciones que se describen en [Activación y desactivación de roles en Privileged Identity Management de Azure AD](pim-how-to-activate-role.md).

    A los administradores aptos se les pide que se registren en Azure Multi-factor Authentication (MFA) durante la activación. Si un usuario no puede registrarse en MFA o usa una cuenta de Microsoft (normalmente @outlook.com), deberá establecerlo como permanente en todos sus roles.

## <a name="make-a-role-assignment-permanent"></a>Hacer que una asignación de roles sea permanente

De forma predeterminada, los usuarios nuevos solo son aptos para un rol de directorio. Siga estos pasos si desea hacer que una asignación de roles sea permanente.

1. Abra **Azure AD Privileged Identity Management**.

1. Haga clic en **Roles de directorio de Azure AD**.

1. Haga clic en **Miembros**.

    ![Lista de miembros](./media/pim-how-to-add-role-to-user/pim-directory-role-list-members.png)

1. Haga clic en un rol **apto** que desea convertir en permanente.

1. Haga clic en **Más** y luego en **Establecer como permanente**.

    ![Hacer que una asignación de roles sea permanente](./media/pim-how-to-add-role-to-user/pim-make-perm.png)

    El rol aparece ahora como **permanente**.

    ![Lista de miembros con el cambio permanente](./media/pim-how-to-add-role-to-user/pim-directory-role-list-members-permanent.png)

## <a name="remove-a-user-from-a-role"></a>Eliminación de un usuario de un rol

Puede quitar a los usuarios de las asignaciones de roles, pero asegúrese de que siempre haya al menos un usuario que sea un administrador global permanente. Si no está seguro de si los usuarios necesitan aún sus asignaciones de roles, puede [iniciar una revisión de acceso del rol](pim-how-to-start-security-review.md).

Siga estos pasos para quitar a un usuario específico de un rol diferente.

1. Abra **Azure AD Privileged Identity Management**.

1. Haga clic en **Roles de directorio de Azure AD**.

1. Haga clic en **Miembros**.

    ![Lista de miembros](./media/pim-how-to-add-role-to-user/pim-directory-role-list-members.png)

1. Haga clic en una asignación de roles que desee quitar.

1. Haga clic en **Más** y después en **Quitar**.

    ![Quitar un rol](./media/pim-how-to-add-role-to-user/pim-remove-role.png)

1. En el mensaje que le pide confirmación, haga clic en **Sí**.

    ![Quitar un rol](./media/pim-how-to-add-role-to-user/pim-remove-role-confirm.png)

    La asignación de rol se quita.

## <a name="next-steps"></a>Pasos siguientes
[!INCLUDE [active-directory-privileged-identity-management-toc](../../../includes/active-directory-privileged-identity-management-toc.md)]
