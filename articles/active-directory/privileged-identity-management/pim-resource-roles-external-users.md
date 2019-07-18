---
title: 'Invitación de invitados y asignación de roles de recursos de Azure en PIM: Azure Active Directory | Microsoft Docs'
description: Aprenda a invitar a usuarios invitados externos y a asignar roles de recursos de Azure en Azure AD Privileged Identity Management (PIM).
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
ms.date: 04/09/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 07476c9f5db64a5d107a493022fa3548fe0dae4c
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476356"
---
# <a name="invite-guest-users-and-assign-azure-resource-roles-in-pim"></a>Invitación de usuarios invitados y asignación de roles de recursos de Azure en PIM

Azure Active Directory (Azure AD) de negocio a negocio (B2B) es un conjunto de funcionalidades de Azure AD que permite a las organizaciones colaborar con usuarios invitados (invitados) y proveedores externos con cualquier cuenta. Cuando se combina B2B con Azure AD Privileged Identity Management (PIM), puede seguir aplicando los requisitos de cumplimiento y gobernanza a los invitados. Por ejemplo, puede usar estas características de PIM en tareas de identidad de Azure con invitados:

- Asignar acceso a recursos específicos de Azure
- Habilitar acceso Just-In-Time
- Especificar la duración de la asignación y la fecha de finalización
- Exigir MFA en la asignación activa o activación
- Realizar revisiones de acceso
- Usar alertas y registros de auditoría

En este artículo se describe cómo invitar a un invitado a su organización y administrar su acceso a recursos de Azure con Privileged Identity Management.

## <a name="when-would-you-invite-guests"></a>¿Cuándo invitaría a invitados?

A continuación se describen algunos escenarios de ejemplo en los que podría invitar a invitados a su organización:

- Permitir que un proveedor externo por cuenta propia que solo tiene una cuenta de correo electrónico acceda a los recursos de Azure para la realización de un proyecto
- Permitir que a un asociado externo de una gran organización que usa Servicios de federación de Active Directory (AD FS) locales acceda a la aplicación de gastos
- Permitir que los ingenieros del servicio de soporte técnico que no están en su organización (por ejemplo, el soporte técnico de Microsoft) accedan temporalmente a los recursos de Azure para solucionar problemas

## <a name="how-does-collaboration-using-b2b-guests-work"></a>¿Cómo funciona la colaboración mediante invitados B2B?

Al usar la colaboración B2B, puede invitar a un usuario externo a su organización como invitado. El invitado parece estar en la organización, pero no tiene credenciales asociadas a ella. Cada vez que un invitado debe autenticarse, tiene que hacerlo en su organización principal y no en la suya. Esto significa que, si el invitado ya no tiene acceso a su organización principal, también perderá el acceso a su organización. Por ejemplo, si el invitado abandona la organización, automáticamente pierde acceso a los recursos compartidos con él en Azure AD sin necesidad de hacer nada. Para más información sobre B2B, consulte [¿Qué es el acceso de usuarios invitados en Azure Active Directory B2B?](../b2b/what-is-b2b.md)

![Diagrama que muestra cómo aparece un usuario invitado en el directorio, pero se autentica en su directorio principal](./media/pim-resource-roles-external-users/b2b-external-user.png)

## <a name="check-guest-collaboration-settings"></a>Comprobación de la configuración de colaboración de invitado

Para asegurarse de que puede invitar a invitados a su organización, debe comprobar la configuración de colaboración de invitado.

1. Inicie sesión en el [portal de Azure](https://portal.azure.com/).

1. Haga clic en **Azure Active Directory** > **Configuración de usuario**.

1. Haga clic en **Administrar la configuración de colaboración externa**.

    ![Página de configuración de colaboración externa en la que se muestra la configuración de restricción de la colaboración, la invitación y los permisos](./media/pim-resource-roles-external-users/external-collaboration-settings.png)

1. Asegúrese de que el conmutador **Los administradores y los usuarios del rol de invitador de personas pueden invitar** se establece en **Sí**.

## <a name="invite-a-guest-and-assign-a-role"></a>Invitar a un invitado y asignar un rol

Mediante PIM, puede invitar a un invitado y permitirle adoptar un rol de recursos de Azure, igual que si fuera un usuario miembro.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) con un usuario que sea miembro del rol [Administrador de roles con privilegios](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) o [Administrador de usuarios](../users-groups-roles/directory-assign-admin-roles.md#user-administrator).

1. Abra **Azure AD Privileged Identity Management**.

1. Haga clic en **Recursos de Azure**.

1. Use **Filtro de recursos** para filtrar la lista de recursos administrados.

1. Haga clic en el recurso que quiera administrar, por ejemplo, un recurso, un grupo de recursos, una suscripción o un grupo de administración.

    Debe establecer el ámbito a solo lo que necesita el invitado.

1. En Administrar, haga clic en **Roles** para ver la lista de roles para los recursos de Azure.

    ![Lista de roles de recursos de Azure en la que se muestra el número de usuarios que están activos y se pueden elegir](./media/pim-resource-roles-external-users/resources-roles.png)

1. Haga clic en el rol mínimo que el usuario necesitará.

    ![Página del rol seleccionado en la que se muestran los miembros actuales de ese rol](./media/pim-resource-roles-external-users/selected-role.png)

1. En la página del rol, haga clic en **Agregar miembro** para abrir el panel Nueva asignación.

1. Haga clic en **Seleccionar un miembro o grupo**.

    ![Panel Nueva asignación: Seleccionar un miembro o grupo en el que se muestran los usuarios y grupos, con una opción de invitar](./media/pim-resource-roles-external-users/select-member-group.png)

1. Para invitar a un invitado, haga clic en **Invitar**.

    ![Página Invitar a un invitado con cuadros para escribir una dirección de correo electrónico y especificar un mensaje personal](./media/pim-resource-roles-external-users/invite-guest.png)

1. Después de haber seleccionado un invitado, haga clic en **Invitar**.

    Se debería agregar al invitado como un miembro seleccionado.

1. En el panel **Seleccionar un miembro o grupo**, haga clic en **Seleccionar**.

1. En el panel **Configuración de pertenencia**, seleccione el tipo de asignación y la duración.

    ![Página Nueva asignación: Configuración de pertenencia con opciones para especificar el tipo de asignación, la fecha de inicio y la fecha de finalización](./media/pim-resource-roles-external-users/membership-settings.png)

1. Para completar la asignación, haga clic en **Listo** y, luego, en **Agregar**.

    La asignación de roles de invitado aparecerá en la lista de roles.

    ![Página de roles en la que aparece el invitado como apto](./media/pim-resource-roles-external-users/role-assignment.png)

## <a name="activate-role-as-a-guest"></a>Activación del rol como invitado

Como usuario externo, primero debe aceptar la invitación para la organización de Azure AD y, posiblemente, activar su rol.

1. Abra el correo electrónico con la invitación. El correo electrónico se parecerá al siguiente:

    ![Invitación por correo electrónico con el nombre del directorio, el mensaje personal y un vínculo Comenzar](./media/pim-resource-roles-external-users/email-invite.png)

1. Haga clic en el vínculo **Inicio** del correo electrónico.

1. Después de revisar los permisos, haga clic en **Aceptar**.

    ![Página de revisión de permisos en un explorador con una lista de permisos que la organización quiere que revisen los usuarios](./media/pim-resource-roles-external-users/invite-accept.png)

1. Puede que se le pida que acepte las condiciones de uso y que especifique si quiere mantener la sesión iniciada.

    Se abre Azure Portal. Si solo tiene derecho a un rol, no tendrá acceso a los recursos.

1. Para activar su rol, abra el correo electrónico con el vínculo Activar rol. El correo electrónico se parecerá al siguiente:

    ![Mensaje de correo electrónico de PIM en el que se indica que es apto para un rol con un vínculo Activar rol](./media/pim-resource-roles-external-users/email-role-assignment.png)

1. Haga clic en **Activar rol** para abrir sus roles válidos en PIM.

    ![Página Mis roles en PIM en la que aparecen sus roles aptos](./media/pim-resource-roles-external-users/my-roles-eligible.png)

1. En Acción, haga clic en el vínculo **Activar**.

    Según la configuración de rol, deberá especificar la información para activar el rol.

1. Cuando haya especificado la configuración del rol, haga clic en **Activar** para activar el rol.

    ![Página Activar en la que aparece el ámbito y las opciones para especificar la hora de inicio, la duración y el motivo](./media/pim-resource-roles-external-users/activate-role.png)

    A menos que el administrador deba aprobar la solicitud, debería tener acceso a los recursos especificados.

## <a name="view-activity-for-a-guest"></a>Visualización de la actividad de un invitado

Al igual que un usuario miembro, puede ver los registros de auditoría para mantener el seguimiento de lo que hacen los invitados.

1. Como administrador, abra PIM y seleccione el recurso que se ha compartido.

1. Haga clic en **Auditoría de recursos** para ver la actividad de ese recurso. A continuación se muestra un ejemplo de la actividad para un grupo de recursos.

    ![Recursos de Azure: página Auditoría de recursos en la que aparece la hora, el solicitante y la acción](./media/pim-resource-roles-external-users/audit-resource.png)

1. Para ver la actividad del invitado, haga clic en **Azure Active Directory** > **Usuarios** > nombre del invitado.

1. Haga clic en **Registros de auditoría** para ver los registros de auditoría de la organización. Si es necesario, puede especificar filtros.

    ![Registros de auditoría del directorio en los que se muestran la fecha, el destino, quien lo ha iniciado y la actividad](./media/pim-resource-roles-external-users/audit-directory.png)

## <a name="next-steps"></a>Pasos siguientes

- [Asignación de roles de administrador de Azure AD en PIM](pim-how-to-add-role-to-user.md)
- [¿Qué es el acceso de usuarios invitados en Azure Active Directory B2B?](../b2b/what-is-b2b.md)
