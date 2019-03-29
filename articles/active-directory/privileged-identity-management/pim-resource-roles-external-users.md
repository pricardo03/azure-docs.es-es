---
title: Invitar a otros usuarios y asignar roles de recursos de Azure en PIM - Azure Active Directory | Microsoft Docs
description: Obtenga información sobre cómo invitar a usuarios externos y asignar roles de recursos de Azure en Azure AD Privileged Identity Management (PIM).
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
ms.date: 03/13/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 68c5e5e2ed0d3ec767a239439476a98bac73bcb4
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2019
ms.locfileid: "58576886"
---
# <a name="invite-guest-users-and-assign-azure-resource-roles-in-pim"></a>Invitar a usuarios y asignar roles de recursos de Azure en PIM

Azure Active Directory (Azure AD) negocio a negocio (B2B) es un conjunto de capacidades dentro de Azure AD que permite a las organizaciones a colaborar con usuarios externos invitados (invitados) y con cualquier cuenta de proveedores. Cuando se combina B2B con Azure AD Privileged Identity Management (PIM), puede seguir aplicar los requisitos de cumplimiento y gobierno a los invitados. Por ejemplo, puede usar estas características PIM para tareas de identidad de Azure con los invitados:

- Asignar acceso a recursos específicos de Azure
- Habilitar acceso Just-In-Time
- Especificar la duración de la asignación y la fecha de finalización
- Exigir MFA en la asignación activa o activación
- Realizar revisiones de acceso
- Usar alertas y registros de auditoría

En este artículo se describe cómo invitar a un invitado a su organización y administrar su acceso a recursos de Azure con Privileged Identity Management.

## <a name="when-would-you-invite-guests"></a>¿Cuándo se invitar a otros usuarios?

Estos son algunos escenarios de ejemplo cuando puede invitar a otros usuarios a su organización:

- Permitir que un proveedor externo por cuenta propia que solo tiene una cuenta de correo electrónico acceda a los recursos de Azure para la realización de un proyecto
- Permitir que a un asociado externo de una gran organización que usa Servicios de federación de Active Directory (AD FS) locales acceda a la aplicación de gastos
- Permitir que los ingenieros del servicio de soporte técnico que no están en su organización (por ejemplo, el soporte técnico de Microsoft) accedan temporalmente a los recursos de Azure para solucionar problemas

## <a name="how-does-collaboration-using-b2b-guests-work"></a>¿Cómo la colaboración usando B2B invitados de trabajo?

Cuando se usa la colaboración B2B, puede invitar a un usuario externo a su organización como invitado. El invitado parece estar en su organización, pero el invitado no tiene ninguna credencial asociada con él. Cada vez que un invitado tiene que autenticarse, deben autenticarse en su organización principal y no en su organización. Esto significa que si el invitado ya no tiene acceso a su organización principal, perderán el acceso a su organización. Por ejemplo, si el invitado deja su organización, que automáticamente pierdan el acceso a los recursos compartidos con ellos en Azure AD sin tener que hacer nada. Para más información sobre B2B, consulte [¿Qué es el acceso de usuarios invitados en Azure Active Directory B2B?](../b2b/what-is-b2b.md)

![B2B e invitado](./media/pim-resource-roles-external-users/b2b-external-user.png)

## <a name="check-guest-collaboration-settings"></a>Compruebe la configuración de colaboración de invitado

Para asegurarse de que se pueden invitar a otros usuarios en su organización, debe comprobar la configuración de colaboración de invitado.

1. Inicie sesión en el [portal de Azure](https://portal.azure.com/).

1. Haga clic en **Azure Active Directory** > **Configuración de usuario**.

1. Haga clic en **Administrar la configuración de colaboración externa**.

    ![Configuración de colaboración externa](./media/pim-resource-roles-external-users/external-collaboration-settings.png)

1. Asegúrese de que el conmutador **Los administradores y los usuarios del rol de invitador de personas pueden invitar** se establece en **Sí**.

## <a name="invite-a-guest-and-assign-a-role"></a>Invitar a un invitado y asignar un rol

PIM puede invitar a un invitado y hacer que sean válidos para un rol de recursos de Azure, al igual que un usuario de miembro.

1. Inicie sesión en [portal Azure](https://portal.azure.com/) con un usuario que sea miembro de la [con privilegios de administrador de roles](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) o [usuario administrador](../users-groups-roles/directory-assign-admin-roles.md#user-administrator) rol.

1. Abra **Azure AD Privileged Identity Management**.

1. Haga clic en **Recursos de Azure**.

1. Use **Filtro de recursos** para filtrar la lista de recursos administrados.

1. Haga clic en el recurso que quiera administrar, por ejemplo, un recurso, un grupo de recursos, una suscripción o un grupo de administración.

    Debe establecer el ámbito para que solo necesita que el invitado.

1. En Administrar, haga clic en **Roles** para ver la lista de roles para los recursos de Azure.

    ![Roles de recursos de Azure](./media/pim-resource-roles-external-users/resources-roles.png)

1. Haga clic en el rol mínimo que el usuario necesitará.

    ![Rol seleccionado](./media/pim-resource-roles-external-users/selected-role.png)

1. En la página del rol, haga clic en **Agregar miembro** para abrir el panel Nueva asignación.

1. Haga clic en **Seleccionar un miembro o grupo**.

    ![Seleccionar un miembro o grupo](./media/pim-resource-roles-external-users/select-member-group.png)

1. Para invitar a un invitado, haga clic en **invitar**.

    ![Invitación a un invitado](./media/pim-resource-roles-external-users/invite-guest.png)

1. Después de haber seleccionado un invitado, haga clic en **invitar**.

    El invitado debe agregarse como un miembro seleccionado.

1. En el **seleccionar un miembro o grupo** panel, haga clic en **seleccione**.

1. En el **la configuración de pertenencia** panel, seleccione el tipo de asignación y duración.

    ![Configuración de pertenencia](./media/pim-resource-roles-external-users/membership-settings.png)

1. Para completar la asignación, haga clic en **Listo** y, luego, en **Agregar**.

    La asignación de roles de invitado aparecerá en la lista de roles.

    ![Asignación de roles para invitados](./media/pim-resource-roles-external-users/role-assignment.png)

## <a name="activate-role-as-a-guest"></a>Activación de rol como invitado

Como un usuario externo, primero deberá aceptar la invitación para su organización de Azure AD y, posiblemente, activar su rol.

1. Abra el correo electrónico con su invitación. El correo electrónico se parecerá al siguiente:

    ![Invitación por correo electrónico](./media/pim-resource-roles-external-users/email-invite.png)

1. Haga clic en el vínculo **Inicio** del correo electrónico.

1. Después de revisar los permisos, haga clic en **Aceptar**.

    ![Revisión de los permisos](./media/pim-resource-roles-external-users/invite-accept.png)

1. Puede que se le pida aceptar los términos de uso y especificar si desea mantener la sesión iniciada.

    Se abre Azure Portal. Si solo tiene derecho a un rol, no tendrá acceso a los recursos.

1. Para activar su rol, abra el correo electrónico con el vínculo Activar rol. El correo electrónico se parecerá al siguiente:

    ![Invitación por correo electrónico](./media/pim-resource-roles-external-users/email-role-assignment.png)

1. Haga clic en **Activar rol** para abrir sus roles válidos en PIM.

    ![Mis roles - aptos](./media/pim-resource-roles-external-users/my-roles-eligible.png)

1. En Acción, haga clic en el vínculo **Activar**.

    Según la configuración de rol, deberá especificar la información para activar el rol.

1. Cuando haya especificado la configuración del rol, haga clic en **Activar** para activar el rol.

    ![Activación del rol](./media/pim-resource-roles-external-users/activate-role.png)

    A menos que el administrador deba aprobar la solicitud, debería tener acceso a los recursos especificados.

## <a name="view-activity-for-a-guest"></a>Ver la actividad de un invitado

Al igual que un usuario miembro, puede ver los registros de auditoría para realizar un seguimiento de qué hacen los invitados.

1. Como administrador, abra PIM y seleccione el recurso que se ha compartido.

1. Haga clic en **Auditoría de recursos** para ver la actividad de ese recurso. A continuación se muestra un ejemplo de la actividad para un grupo de recursos.

    ![Auditoría de recursos](./media/pim-resource-roles-external-users/audit-resource.png)

1. Para ver la actividad para el invitado, haga clic en **Azure Active Directory** > **usuarios** > nombre de invitado.

1. Haga clic en **registros de auditoría** para ver los registros de auditoría para la organización. Si es necesario, puede especificar filtros.

    ![auditoría de la organización](./media/pim-resource-roles-external-users/audit-directory.png)

## <a name="next-steps"></a>Pasos siguientes

- [Asignar roles de administrador de Azure AD en PIM](pim-how-to-add-role-to-user.md)
- [¿Qué es el acceso de usuarios invitados en Azure Active Directory B2B?](../b2b/what-is-b2b.md)
