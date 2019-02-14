---
title: Invitación de usuarios externos y asignación de roles de recursos de Azure en PIM | Microsoft Docs
description: Aprenda a invitar a usuarios externos y a asignar roles de recursos de Azure en Azure AD Privileged Identity Management (PIM).
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
ms.date: 11/29/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: a90d0d3d3f484044a0ffbab7a3c24a76c40aa74c
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56208290"
---
# <a name="invite-external-users-and-assign-azure-resource-roles-in-pim"></a>Invitación de usuarios externos y asignación de roles de recursos de Azure en PIM

Azure Activity Directory (Azure AD) de negocio a negocio (B2B) es un conjunto de funcionalidades de Azure AD que permite a las organizaciones colaborar con usuarios y proveedores externos con cualquier cuenta. Cuando se combina B2B con Azure AD Privileged Identity Management (PIM), puede seguir aplicando los requisitos de cumplimiento y gobierno a los usuarios externos. Por ejemplo, puede usar estas características de PIM en recursos de Azure con usuarios externos:

- Asignar acceso a recursos específicos de Azure
- Habilitar acceso Just-In-Time
- Especificar la duración de la asignación y la fecha de finalización
- Exigir MFA en la asignación activa o activación
- Realizar revisiones de acceso
- Usar alertas y registros de auditoría

En este artículo se describe cómo invitar a un usuario externo a su directorio y administrar su acceso a recursos de Azure con PIM.

## <a name="when-would-you-invite-external-users"></a>¿Cuándo invitaría a usuarios externos?

A continuación se describen algunos escenarios de ejemplo en los que podría invitar a usuarios externos a su directorio:

- Permitir que un proveedor externo por cuenta propia que solo tiene una cuenta de correo electrónico acceda a los recursos de Azure para la realización de un proyecto
- Permitir que a un asociado externo de una gran organización que usa Servicios de federación de Active Directory (AD FS) locales acceda a la aplicación de gastos
- Permitir que los ingenieros del servicio de soporte técnico que no están en su organización (por ejemplo, el soporte técnico de Microsoft) accedan temporalmente a los recursos de Azure para solucionar problemas

## <a name="how-does-external-collaboration-using-b2b-work"></a>¿Cómo funciona la colaboración externa mediante B2B?

Cuando usa B2B, puede invitar a un usuario externo a su directorio. El usuario externo parece estar en el directorio al que se le ha invitado, pero no tiene credenciales asociadas a él. Cada vez que los usuarios externos deben autenticarse, lo deben hacer en su directorio particular y no en el directorio al que están invitados. Esto significa que si los usuarios externos ya no tienen acceso a su directorio particular, perderán automáticamente acceso al directorio al que están invitados. Por ejemplo, si el usuario externo abandona la organización, automáticamente pierde acceso a los recursos compartidos con él en el directorio al que está invitado sin necesidad de hacer nada. Para más información sobre B2B, consulte [¿Qué es el acceso de usuarios invitados en Azure Active Directory B2B?](../b2b/what-is-b2b.md)

![B2B y los usuarios externos](./media/pim-resource-roles-external-users/b2b-external-user.png)

## <a name="check-external-collaboration-settings"></a>Comprobación de la configuración de colaboración externa

Para asegurarse de que puede invitar a usuarios externos a su directorio, debe comprobar la configuración de colaboración externa.

1. Inicie sesión en el [portal de Azure](https://portal.azure.com/).

1. Haga clic en **Azure Active Directory** > **Configuración de usuario**.

1. Haga clic en **Administrar la configuración de colaboración externa**.

    ![Configuración de colaboración externa](./media/pim-resource-roles-external-users/external-collaboration-settings.png)

1. Asegúrese de que el conmutador **Los administradores y los usuarios del rol de invitador de personas pueden invitar** se establece en **Sí**.

## <a name="invite-an-external-user-and-assign-a-role"></a>Invitación de usuarios externos y asignación de un rol

Mediante PIM, puede invitar a un usuario externo y permitirle adoptar un rol de recursos de Azure, igual que si fuera un usuario miembro.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) con un usuario que sea miembro del rol [Administrador de rol con privilegios](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) o [Administrador de cuentas de usuario](../users-groups-roles/directory-assign-admin-roles.md#user-account-administrator).

1. Abra **Azure AD Privileged Identity Management**.

1. Haga clic en **Recursos de Azure**.

1. Use **Filtro de recursos** para filtrar la lista de recursos administrados.

1. Haga clic en el recurso que quiera administrar, por ejemplo, un recurso, un grupo de recursos, una suscripción o un grupo de administración.

    Debe establecer el ámbito a solo lo que necesita el usuario externo.

1. En Administrar, haga clic en **Roles** para ver la lista de roles para los recursos de Azure.

    ![Roles de recursos de Azure](./media/pim-resource-roles-external-users/resources-roles.png)

1. Haga clic en el rol mínimo que el usuario necesitará.

    ![Rol seleccionado](./media/pim-resource-roles-external-users/selected-role.png)

1. En la página del rol, haga clic en **Agregar miembro** para abrir el panel Nueva asignación.

1. Haga clic en **Seleccionar un miembro o grupo**.

    ![Seleccionar un miembro o grupo](./media/pim-resource-roles-external-users/select-member-group.png)

1. Para invitar a un usuario externo, haga clic en **Invitar**.

    ![Invitación a un invitado](./media/pim-resource-roles-external-users/invite-guest.png)

1. Después de haber especificado un usuario externo, haga clic en **Invitar**.

    El usuario externo se debe agregar como un miembro seleccionado.

1. En el panel Seleccionar un miembro o grupo, haga clic en **Seleccionar**.

1. En el panel Configuración de pertenencia, seleccione el tipo de asignación y la duración.

    ![Configuración de pertenencia](./media/pim-resource-roles-external-users/membership-settings.png)

1. Para completar la asignación, haga clic en **Listo** y, luego, en **Agregar**.

    La asignación de roles de usuario externo aparece en la lista de roles.

    ![Asignación de roles de usuario externo](./media/pim-resource-roles-external-users/role-assignment.png)

## <a name="activate-role-as-an-external-user"></a>Activación de roles como usuario externo

Como usuario externo, primero debe aceptar la invitación para el directorio de Azure AD y, posiblemente, activar su rol.

1. Abra el correo electrónico con su invitación al directorio. El correo electrónico se parecerá al siguiente:

    ![Invitación por correo electrónico](./media/pim-resource-roles-external-users/email-invite.png)

1. Haga clic en el vínculo **Inicio** del correo electrónico.

1. Después de revisar los permisos, haga clic en **Aceptar**.

    ![Revisión de los permisos](./media/pim-resource-roles-external-users/invite-accept.png)

1. Puede que se le pida aceptar los términos de uso y especificar si desea mantener la sesión iniciada.

    Se abre Azure Portal. Si solo tiene derecho a un rol, no tendrá acceso a los recursos.

1. Para activar su rol, abra el correo electrónico con el vínculo Activar rol. El correo electrónico se parecerá al siguiente:

    ![Invitación por correo electrónico](./media/pim-resource-roles-external-users/email-role-assignment.png)

1. Haga clic en **Activar rol** para abrir sus roles válidos en PIM.

    ![Mis roles: válidos](./media/pim-resource-roles-external-users/my-roles-eligible.png)

1. En Acción, haga clic en el vínculo **Activar**.

    Según la configuración de rol, deberá especificar la información para activar el rol.

1. Cuando haya especificado la configuración del rol, haga clic en **Activar** para activar el rol.

    ![Activación del rol](./media/pim-resource-roles-external-users/activate-role.png)

    A menos que el administrador deba aprobar la solicitud, debería tener acceso a los recursos especificados.

## <a name="view-activity-for-an-external-user"></a>Visualización de la actividad para un usuario externo

Al igual que un usuario miembro, puede ver los registros de auditoría para mantener el seguimiento de lo que hacen los usuarios externos.

1. Como administrador, abra PIM y seleccione el recurso que se ha compartido.

1. Haga clic en **Auditoría de recursos** para ver la actividad de ese recurso. A continuación se muestra un ejemplo de la actividad para un grupo de recursos.

    ![Auditoría de recursos](./media/pim-resource-roles-external-users/audit-resource.png)

1. Para ver la actividad del usuario externo, haga clic en **Azure Active Directory** > **Usuarios** > Usuario externo.

1. Haga clic en **Registros de auditoría** para ver los registros de auditoría del directorio. Si es necesario, puede especificar filtros.

    ![Auditoría de directorio](./media/pim-resource-roles-external-users/audit-directory.png)

## <a name="next-steps"></a>Pasos siguientes

- [Asignación de roles de directorio de Azure AD en PIM](pim-how-to-add-role-to-user.md)
- [¿Qué es el acceso de usuarios invitados en Azure Active Directory B2B?](../b2b/what-is-b2b.md)
