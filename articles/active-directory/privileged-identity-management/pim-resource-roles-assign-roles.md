---
title: Asignación de roles de recursos de Azure en PIM | Microsoft Docs
description: Aprenda a asignar roles de recursos de Azure en Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 08/30/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: ee09526c2bf1ebf9821e1f84dc99bfc8635b9ee2
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58086197"
---
# <a name="assign-azure-resource-roles-in-pim"></a>Asignación de roles de recursos de Azure en PIM

Azure AD PIM puede administrar los roles incorporados de los recursos de Azure, así como los roles personalizados, entre ellos, los siguientes:

- Propietario
- Administrador de acceso de usuario
- Colaborador
- Administrador de seguridad
- Y muchos otros

> [!NOTE]
> Los usuarios o los miembros de un grupo asignados a los roles Propietario o Administrador de acceso de usuario y los administradores globales que permiten la administración de suscripciones de Azure AD son administradores de recursos. Estos administradores pueden asignar roles, configurar opciones de rol y revisar el acceso con PIM a los recursos de Azure. Consulte la lista de [roles integrados en los recursos de Azure](../../role-based-access-control/built-in-roles.md).

## <a name="assign-a-role"></a>Asignar un rol

Siga estos pasos para hacer que un usuario sea elegible para un rol de directorio de Azure AD.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) con un usuario que sea miembro del [con Administrador de rol con privilegios](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator).

    Para obtener información acerca de cómo conceder otro acceso de administrador para administrar PIM, consulte [Concesión de acceso para administrar Azure AD Privileged Identity Management](pim-how-to-give-access-to-pim.md).

1. Abra **Azure AD Privileged Identity Management**.

    Si aún no ha iniciado PIM en Azure Portal, vaya a [Primer uso de PIM](pim-getting-started.md).

1. Haga clic en **Recursos de Azure**.

1. Use **Filtro de recursos** para filtrar la lista de recursos administrados.

    ![Lista de recursos de Azure para administrar](./media/pim-resource-roles-assign-roles/resources-list.png)

1. Haga clic en el recurso que quiera administrar, por ejemplo, una suscripción o un grupo de administración.

1. En Administrar, haga clic en **Roles** para ver la lista de roles para los recursos de Azure.

    ![Roles de recursos de Azure](./media/pim-resource-roles-assign-roles/resources-roles.png)

1. Haga clic en **Agregar miembro** para abrir el panel Nueva asignación.

1. Haga clic en **Seleccionar un rol** para abrir el panel Seleccionar un rol.

    ![Panel Nueva asignación](./media/pim-resource-roles-assign-roles/resources-select-role.png)

1. Haga clic en el rol que quiera asignar y luego haga clic en **Seleccionar**.

    Se abre el panel Seleccionar un miembro o grupo.

1. Haga clic en el miembro o grupo que quiera asignar al rol y luego haga clic en **Seleccionar**.

    ![Panel Seleccionar un miembro o grupo](./media/pim-resource-roles-assign-roles/resources-select-member-or-group.png)

    Se abre el panel Configuración de pertenencia.

1. En la lista **Tipo de asignación**, seleccione **Apto** o **Activo**.

    ![Panel Configuración de pertenencia](./media/pim-resource-roles-assign-roles/resources-membership-settings-type.png)

    PIM para Azure Resources ofrece dos tipos de asignación distintos:

    - Las asignaciones tipo **Apto** requieren que el miembro del rol realice una acción para usar el rol. Entre las acciones se puede incluir realizar una comprobación de autenticación multifactor (MFA), proporcionar una justificación de negocios o solicitar la aprobación de los aprobadores designados.

    - Las asignaciones tipo **Activo** no requieren que el miembro realice ninguna acción para usar el rol. Los miembros asignados como activos tienen siempre los privilegios asignados al rol.

1. Si la asignación debe ser permanente (siempre apta o asignada de forma permanente), active la casilla **Elegibilidad permanente**.

    Según la configuración del rol, es posible que la casilla no aparezca o que no se pueda modificar.

1. Para especificar una duración de asignación específica, desactive la casilla y modifique los cuadros de fecha y hora de inicio o finalización.

    ![Configuración de pertenencias: fecha y hora](./media/pim-resource-roles-assign-roles/resources-membership-settings-date.png)

1. Cuando termine, haga clic en **Listo**.

    ![Nueva asignación: agregar](./media/pim-resource-roles-assign-roles/resources-new-assignment-add.png)

1. Para crear la nueva asignación de roles, haga clic en **Agregar**. Se muestra una notificación del estado.

    ![Nueva asignación: notificación](./media/pim-resource-roles-assign-roles/resources-new-assignment-notification.png)

## <a name="update-or-remove-an-existing-role-assignment"></a>Actualizar o quitar una asignación de roles existente

Siga estos pasos para actualizar o quiotar una asignación de roles existente.

1. Abra **Azure AD Privileged Identity Management**.

1. Haga clic en **Recursos de Azure**.

1. Haga clic en el recurso que quiera administrar, por ejemplo, una suscripción o un grupo de administración.

1. En Administrar, haga clic en **Roles** para ver la lista de roles para los recursos de Azure.

    ![Roles de recursos de Azure: seleccionar rol](./media/pim-resource-roles-assign-roles/resources-update-select-role.png)

1. Haga clic en el rol que quiere actualizar o quitar.

1. Busque la asignación de roles en las pestañas **Roles elegibles** o **Roles activos**.

    ![Actualizar o quitar la asignación de roles](./media/pim-resource-roles-assign-roles/resources-update-remove.png)

1. Haga clic en **Actualizar** o **Quitar** para actualizar o quitar la asignación de roles.

    Para obtener información sobre cómo ampliar una asignación de roles, consulte [Ampliación o renovación de roles de recursos de Azure en PIM](pim-resource-roles-renew-extend.md).

## <a name="next-steps"></a>Pasos siguientes

- [Ampliación o renovación de roles de recursos de Azure en PIM](pim-resource-roles-renew-extend.md)
- [Configuración de roles de recurso de Azure en PIM](pim-resource-roles-configure-role-settings.md)
- [Asignación de roles de directorio de Azure AD en PIM](pim-how-to-add-role-to-user.md)
