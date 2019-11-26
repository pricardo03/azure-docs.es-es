---
title: 'Asignación de roles de Azure AD en Privileged Identity Management: Azure Active Directory | Microsoft Docs'
description: Aprenda a asignar roles de Azure AD en Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 10/22/2019
ms.author: curtand
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5ad03e69df4ca03d58bba98b912ade354f90042e
ms.sourcegitcommit: ec2b75b1fc667c4e893686dbd8e119e7c757333a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2019
ms.locfileid: "72809198"
---
# <a name="assign-azure-ad-roles-in-privileged-identity-management"></a>Asignación de roles de Azure AD en Privileged Identity Management

Con Azure Active Directory (Azure AD), un administrador global puede realizar asignaciones de roles de administrador de Azure AD **permanentes**. Estas asignaciones de roles se pueden crear mediante [Azure Portal](../users-groups-roles/directory-assign-admin-roles.md) o mediante [comandos de PowerShell](/powershell/module/azuread#directory_roles).

El servicio Azure AD Privileged Identity Management (PIM) permite también a los administradores de roles con privilegios realizar asignaciones de roles de administrador permanentes. Además, los administradores de rol con privilegios pueden hacer que los usuarios sean **aptos** para roles de administrador de Azure AD. Un administrador apto puede activar el rol cuando lo necesite y, cuando termina, sus permisos caducan.

## <a name="make-a-user-eligible-for-a-role"></a>Hacer que un usuario sea apto para un rol

Siga estos pasos para hacer que un usuario sea elegible para un rol de administrador de Azure AD.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) con un usuario que sea miembro del [con Administrador de rol con privilegios](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator).

    Para información sobre cómo conceder acceso a otro administrador para gestionar Privileged Identity Management, consulte [Concesión de acceso a otros administradores para administrar Privileged Identity Management](pim-how-to-give-access-to-pim.md).

1. Abra **Azure AD Privileged Identity Management**.

    Si aún no ha iniciado Privileged Identity Management en Azure Portal, vaya a [Primer uso de Privileged Identity Management](pim-getting-started.md).

1. Seleccione **Roles de Azure AD**.

1. Seleccione **Roles** o **Miembros**.

    ![Roles de Azure AD con las opciones de menú Roles y Miembros resaltadas](./media/pim-how-to-add-role-to-user/pim-directory-roles.png)

1. Seleccione **Agregar miembro** para abrir Agregar miembros administrados.

1. Seleccione **Seleccionar un rol**, seleccione un rol que desee administrar y luego **Seleccionar**.

    ![Panel Seleccionar rol que muestra los roles de Azure AD](./media/pim-how-to-add-role-to-user/pim-select-a-role.png)

1. Seleccione **Seleccionar miembros**, seleccione los usuarios que desea asignar al rol y, a continuación, **Seleccionar**.

    ![Panel Seleccionar miembros donde puede seleccionar un usuario](./media/pim-how-to-add-role-to-user/pim-select-members.png)

1. En Agregar miembros administrados, seleccione **Aceptar** para agregar el usuario al rol.

1. En la lista de roles, seleccione el que acaba de asignar para ver la lista de miembros.

     Cuando el rol esté asignado, el usuario que ha seleccionado aparecerá en la lista de miembros como **Elegible** para el rol.

    ![Los miembros de un rol se enumeran junto con su estado de activación.](./media/pim-how-to-add-role-to-user/pim-directory-role-eligible.png)

1. Ahora que el usuario es apto para el rol, hágale saber que puede activarlo de acuerdo con las instrucciones que se describen en [Activación de mis roles de Azure AD en Privileged Identity Management](pim-how-to-activate-role.md).

    A los administradores aptos se les pide que se registren en Azure Multi-factor Authentication (MFA) durante la activación. Si un usuario no puede registrarse en MFA o usa una cuenta de Microsoft (normalmente @outlook.com), deberá establecerlo como permanente en todos sus roles.

## <a name="make-a-role-assignment-permanent"></a>Hacer que una asignación de roles sea permanente

De forma predeterminada, los nuevos usuarios solo son aptos para un rol de administrador de Azure AD. Siga estos pasos si desea hacer que una asignación de roles sea permanente.

1. Abra **Azure AD Privileged Identity Management**.

1. Seleccione **Roles de Azure AD**.

1. Seleccione **Miembros**.

    ![Roles de Azure AD: lista de miembros que muestra el rol y el estado de activación](./media/pim-how-to-add-role-to-user/pim-directory-role-list-members.png)

1. Seleccione en un rol **Apto** que desee convertir en permanente.

1. Seleccione **Más** y, después, seleccione **Establecer como permanente**.

    ![Panel que muestra un usuario que es apto para un rol con las opciones de menú Más abiertas](./media/pim-how-to-add-role-to-user/pim-make-perm.png)

    El rol aparece ahora como **permanente**.

    ![Lista de miembros que muestra el rol y el estado de activación que ahora es permanente](./media/pim-how-to-add-role-to-user/pim-directory-role-list-members-permanent.png)

## <a name="remove-a-user-from-a-role"></a>Eliminación de un usuario de un rol

Puede quitar a los usuarios de las asignaciones de roles, pero asegúrese de que siempre haya al menos un usuario que sea un administrador global permanente. Si no está seguro de si los usuarios necesitan aún sus asignaciones de roles, puede [iniciar una revisión de acceso del rol](pim-how-to-start-security-review.md).

Siga estos pasos para quitar a un usuario específico de un rol de administrador de Azure AD.

1. Abra **Azure AD Privileged Identity Management**.

1. Seleccione **Roles de Azure AD**.

1. Seleccione **Miembros**.

    ![Roles de Azure AD: lista de miembros que muestra el rol y el estado de activación](./media/pim-how-to-add-role-to-user/pim-directory-role-list-members.png)

1. Seleccione la asignación de roles que desea quitar.

1. Seleccione **Más** y, a continuación, **Quitar**.

    ![Panel que muestra un usuario que tiene un rol permanente con las opciones de menú Más abiertas](./media/pim-how-to-add-role-to-user/pim-remove-role.png)

1. Cuando se le pida que confirme la acción, seleccione **Sí**.

    ![Mensaje que pregunta si quiere quitar al miembro del rol](./media/pim-how-to-add-role-to-user/pim-remove-role-confirm.png)

    La asignación de rol se quita.

## <a name="authorization-error-when-assigning-roles"></a>Error de autorización al asignar roles

Escenario: Como administrador de acceso de usuario o propietario activo de un recurso de Azure, puede ver el recurso dentro de Privileged Identity Management pero no realizar acciones tales como crear una asignación válida o ver una lista de asignaciones de roles en la página de información general sobre recursos. Cualquiera de estas acciones produce un error de autorización.

Para asignar roles, la entidad de servicio MS-PIM debe tener asignado el [rol de administrador de acceso de usuario](../../role-based-access-control/built-in-roles.md#user-access-administrator) en el control de acceso basado en rol de Azure para el acceso a recursos de Azure (en lugar de un rol de administración de Azure AD). En lugar de esperar hasta que se asigne a MS-PIM el rol de administrador de acceso de usuario, puede asignarlo manualmente.

En los siguientes pasos se asigna el rol de administrador de acceso de usuario a la entidad de servicio MS-PIM para una suscripción.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador global de la organización de Azure AD.

1. Elija **Todos los servicios** y, después, **Suscripciones**.

1. Elija su suscripción.

1. Haga clic en **Control de acceso (IAM)** .

1. Elija **Role assignments** (Asignación de roles) para ver la lista actual de las asignaciones de roles en el ámbito de la suscripción.

   ![Hoja Control de acceso (IAM) para una suscripción](./media/pim-how-to-add-role-to-user/ms-pim-access-control.png)

1. Compruebe si la entidad de servicio **MS PIM** tiene asignado el rol **Administrador de acceso de usuario**.

1. En caso contrario, elija **Agregar asignación de roles** para abrir el panel **Agregar asignación de roles**.

1. En la lista desplegable **Rol**, seleccione el rol **Administrador de acceso de usuario**.

1. En la lista **Seleccionar**, busque y seleccione la entidad de servicio **MS PIM**.

   ![Panel Agregar asignación de roles: adición de permisos para la entidad de servicio MS-PIM](./media/pim-how-to-add-role-to-user/ms-pim-add-permissions.png)

1. Elija **Guardar** para asignar el rol.

   Después de unos momentos, se asignará a la entidad de servicio MS-PIM el rol Administrador de acceso de usuario en el ámbito de la suscripción.

   ![Hoja Control de acceso (IAM) que muestra la asignación de roles de administrador de acceso del usuario para MS-PIM](./media/pim-how-to-add-role-to-user/ms-pim-user-access-administrator.png)

## <a name="next-steps"></a>Pasos siguientes

- [Configuración del rol de administrador de Azure AD en Privileged Identity Management](pim-how-to-change-default-settings.md)
- [Asignación de roles de recursos de Azure en Privileged Identity Management](pim-resource-roles-assign-roles.md)
