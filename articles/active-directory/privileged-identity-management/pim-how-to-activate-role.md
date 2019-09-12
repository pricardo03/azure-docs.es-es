---
title: 'Activación de mis roles de Azure AD en PIM: Azure Active Directory | Microsoft Docs'
description: Aprenda a activar los roles de Azure AD en Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 06/28/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: f9d9b2aff1d196d8b2987d77046831e7200ee2fe
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804476"
---
# <a name="activate-my-azure-ad-roles-in-pim"></a>Activación de mis roles de Azure AD en PIM

Azure Active Directory (Azure AD) Privileged Identity Management (PIM) simplifica la forma en que las empresas administran el acceso con privilegios a los recursos de Azure AD y otros servicios en línea de Microsoft, como Office 365 o Microsoft Intune.  

Si se le ha considerado apto para un rol administrativo, significa que puede activar el rol cuando tenga la necesidad de realizar acciones que requieran ciertos privilegios. Por ejemplo, si administra de vez en cuando características de Office 365, los administradores de roles con privilegios de su organización puede que no le hayan asignado el rol de administrador global permanente, ya que ese rol afecta también a otros servicios. En su lugar, pueden asignarle roles de Azure AD como administrador de Exchange Online. Puede solicitar la activación de ese rol cuando necesite sus privilegios y tendrá control de administrador durante un período predeterminado.

Este artículo se dirige a los administradores que necesitan activar su rol de Azure AD en PIM.

## <a name="activate-a-role"></a>Activación de un rol

Cuando necesite asumir un rol de Azure AD, puede solicitar la activación mediante la opción de navegación **Mis roles** de PIM.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).

1. Abra **Azure AD Privileged Identity Management**. Para obtener información acerca de cómo agregar el icono de PIM al panel, consulte [Comenzar a usar Azure AD Privileged Identity Management](pim-getting-started.md).

1. Haga clic en **Roles de Azure AD**.

1. Haga clic en **Mis roles** para ver una lista de sus roles de Azure AD elegibles.

    ![Roles de Azure AD: mis roles en los que se muestra la lista de roles elegibles o activos](./media/pim-how-to-activate-role/directory-roles-my-roles.png)

1. Buscar el rol que desea activar.

    ![Roles de Azure AD: mi lista de roles elegibles que muestra el vínculo Activar](./media/pim-how-to-activate-role/directory-roles-my-roles-activate.png)

1. Haga clic en **Activate** para abrir el panel de detalles de la activación de rol.

1. Si el rol requiere la autenticación multifactor (MFA), haga clic en **Compruebe su identidad antes de proceder**. Solo tiene que autenticarse una vez por sesión.

    ![Comprobación del panel mi identidad con MFA antes de la activación del rol](./media/pim-how-to-activate-role/directory-roles-my-roles-mfa.png)

1. Haga clic en **Comprobar mi identidad** y siga las instrucciones para realizar una comprobación de seguridad adicional.

    ![Página de comprobación de seguridad adicional que le pregunta cómo contactar con usted](./media/pim-how-to-activate-role/additional-security-verification.png)

1. Haga clic en **Activar** para abrir el panel Activación.

    ![Panel de activación para especificar la hora de inicio, la duración, el vale y el motivo](./media/pim-how-to-activate-role/directory-roles-activate.png)

1. Si es necesario, especifique una hora de inicio de activación personalizada.

1. Especifique la duración de la activación.

1. En el campo **Motivo de la activación**, escriba el motivo de la solicitud de activación. Algunos roles requieren que proporcione un número de vale de problema.

    ![Panel de activación completado con un hora de inicio personalizada, la duración, el vale y el motivo](./media/pim-how-to-activate-role/directory-roles-activation-pane.png)

1. Haga clic en **Activar**.

    Si el rol no requiere aprobación, aparecerá el panel **Estado de activación**, donde se muestra el estado de la activación.

    ![Página de estado de activación que muestra las tres fases de activación](./media/pim-how-to-activate-role/activation-status.png)

    Una vez que se hayan completado todas las fases, haga clic en el vínculo **Cerrar sesión** para cerrar sesión en Azure Portal. Ahora, cuando vuelva a iniciar sesión en el portal, podrá usar el rol.

    Si el [rol requiere aprobación](./azure-ad-pim-approval-workflow.md) para activarse, aparecerá una notificación en la esquina superior del explorador que le informa de que la solicitud está pendiente de aprobación.

    ![La solicitud de activación está pendiente de notificación de aprobación](./media/pim-how-to-activate-role/directory-roles-activate-notification.png)

## <a name="view-the-status-of-your-requests"></a>Visualización del estado de las solicitudes

Puede ver el estado de las solicitudes pendientes de activación.

1. Abra Azure AD Privileged Identity Management.

1. Haga clic en **Roles de Azure AD**.

1. Haga clic en **Mis solicitudes** para ver una lista de sus solicitudes.

    ![Roles de Azure AD: lista Mis solicitudes](./media/pim-how-to-activate-role/directory-roles-my-requests.png)

## <a name="deactivate-a-role"></a>Desactivación de un rol

Cuando se ha activado un rol, si alcanza su límite de tiempo (de duración elegible), se desactiva automáticamente.

Si completa pronto sus tareas de administrador, también puede desactivar un rol de forma manual en Azure AD Privileged Identity Management.

1. Abra Azure AD Privileged Identity Management.

1. Haga clic en **Roles de Azure AD**.

1. Haga clic en **Mis roles**.

1. Haga clic en **Roles activos** para ver la lista de roles activos.

1. Busque el rol que está usando y haga clic en **Desactivar**.

## <a name="cancel-a-pending-request"></a>Cancelación de una solicitud pendiente

Si no necesita activar un rol que requiera aprobación, puede cancelar una solicitud pendiente en cualquier momento.

1. Abra Azure AD Privileged Identity Management.

1. Haga clic en **Roles de Azure AD**.

1. Haga clic en **Mis solicitudes**.

1. En el rol que desea cancelar, haga clic en el botón **Cancelar**.

    Al hacer clic en Cancelar, la solicitud se cancelará. Para volver a activar el rol, tendrá que enviar una nueva solicitud de activación.

   ![Lista Mis solicitudes con el botón Cancelar resaltado](./media/pim-how-to-activate-role/directory-role-cancel.png)

## <a name="troubleshoot"></a>Solución de problemas

### <a name="permissions-are-not-granted-after-activating-a-role"></a>Permisos no concedidos después de activar un rol

Al activar un rol en PIM, la activación podría no propagarse al instante a todos los portales que requieren el rol con privilegios. A veces, incluso si el cambio se propaga, el almacenamiento en caché web en un portal puede provocar que el cambio no surta efecto de inmediato. Esto es lo que debe de hacer si se retrasa la activación.

1. Cierre sesión en Azure Portal y vuelva a iniciar sesión.

    Al activar un rol de Azure AD, verá las fases de la activación. Una vez que finalizan todas las fases, verá un vínculo para **Cerrar sesión**. Puede usar este vínculo para cerrar la sesión. Esto solucionará la mayoría de los casos de retraso de activación.

1. En PIM, compruebe que aparece como miembro del rol.

## <a name="next-steps"></a>Pasos siguientes

- [Activación de mis roles de recursos de Azure en PIM](pim-resource-roles-activate-your-roles.md)
