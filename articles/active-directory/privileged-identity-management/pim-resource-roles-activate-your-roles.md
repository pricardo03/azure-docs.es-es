---
title: 'Activación de mis roles de recursos de Azure en PIM: Azure Active Directory | Microsoft Docs'
description: Aprenda a activar sus roles de recursos de Azure en Azure AD Privileged Identity Management (PIM).
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
ms.date: 06/28/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: bdce060006f65f2b0fb08023066ee504578bc552
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2019
ms.locfileid: "67501667"
---
# <a name="activate-my-azure-resource-roles-in-pim"></a>Activación de mis roles de recursos de Azure en PIM

Con Azure Active Directory (Azure AD) Privileged Identity Management (PIM), los miembros de roles aptos para recursos de Azure pueden programar la activación para una fecha y hora futuras. Asimismo, también pueden seleccionar una duración de activación específica dentro del valor máximo establecido (configurado por los administradores).

Este artículo se dirige a los miembros que necesitan activar su rol de recursos de Azure en PIM.

## <a name="activate-a-role"></a>Activación de un rol

Cuando necesite asumir un rol de recursos de Azure, puede solicitar la activación mediante la opción de navegación **Mis roles** de PIM.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).

1. Abra **Azure AD Privileged Identity Management**. Para obtener información acerca de cómo agregar el icono de PIM al panel, consulte [Comenzar a usar Azure AD Privileged Identity Management](pim-getting-started.md).

1. Haga clic en **Mis roles**.

    ![Mi página de roles en la que se muestran los roles que el usuario puede activar](./media/pim-resource-roles-activate-your-roles/resources-my-roles.png)

1. Haga clic en **Roles de recursos de Azure** para ver una lista de los roles de recursos de Azure aptos.

   ![Mis roles: página de roles de recursos de Azure](./media/pim-resource-roles-activate-your-roles/resources-my-roles-azure-resources.png) 

1. En la lista **Roles de recursos de Azure**, busque el rol que desea activar.

    ![Roles de recursos de Azure: mi lista de roles elegibles](./media/pim-resource-roles-activate-your-roles/resources-my-roles-activate.png)

1. Haga clic en **Activar** para abrir el panel de activación.

1. Si el rol requiere la autenticación multifactor (MFA), haga clic en **Compruebe su identidad antes de proceder**. Solo tiene que autenticarse una vez por sesión.

    ![Comprobación de mi identidad con MFA antes de la activación del rol](./media/pim-resource-roles-activate-your-roles/resources-my-roles-mfa.png)

1. Haga clic en **Comprobar mi identidad** y siga las instrucciones para realizar una comprobación de seguridad adicional.

    ![Pantalla para proporcionar una comprobación de seguridad, como un código PIN](./media/pim-resource-roles-activate-your-roles/resources-mfa-enter-code.png)

1. Si desea especificar un ámbito reducido, haga clic en **Ámbito** para abrir el panel de filtro de recursos.

    Es una práctica recomendada solo solicitar acceso a los recursos que necesita. En el panel de filtro de recursos, puede especificar los grupos de recursos o recursos a los que necesita acceso.

    ![Activar: panel de filtros de recursos para especificar el ámbito](./media/pim-resource-roles-activate-your-roles/resources-my-roles-resource-filter.png)

1. Si es necesario, especifique una hora de inicio de activación personalizada. El miembro se activaría después de la hora seleccionada.

1. En el campo **Razón**, escriba el motivo de la solicitud de activación.

    ![Panel Activar completado con el ámbito, la hora de inicio, la duración y el motivo](./media/pim-resource-roles-activate-your-roles/resources-my-roles-activate-done.png)

1. Haga clic en **Activar**.

    Si el rol no requiere aprobación, se activa y se agrega a la lista de roles activos. Si quiere usar el rol, siga los pasos de la siguiente sección.

    Si el [rol requiere aprobación](pim-resource-roles-approval-workflow.md) para activarse, aparecerá una notificación en la esquina superior del explorador que le informa de que la solicitud está pendiente de aprobación.

    ![La solicitud de activación está pendiente de notificación de aprobación](./media/pim-resource-roles-activate-your-roles/resources-my-roles-activate-notification.png)

## <a name="use-a-role-immediately-after-activation"></a>Uso de un rol inmediatamente después de la activación

En caso de cualquier retraso tras la activación, siga estos pasos para usar los roles de recursos de Azure de forma inmediata.

1. Abra Azure AD Privileged Identity Management.

1. Haga clic en **Mis roles** para ver una lista de sus roles de Azure AD y roles de recursos de Azure aptos.

1. Haga clic en **Roles de recursos de Azure**.

1. Haga clic en la pestaña **Roles activos**.

1. Cuando el rol esté activo, cierre sesión en el portal e iníciela de nuevo.

    El rol debe estar ahora disponible para su uso.

## <a name="view-the-status-of-your-requests"></a>Visualización del estado de las solicitudes

Puede ver el estado de las solicitudes pendientes de activación.

1. Abra Azure AD Privileged Identity Management.

1. Haga clic en **Mis solicitudes** para ver una lista de sus solicitudes de rol de Azure AD y rol de recursos de Azure.

    ![Mis solicitudes: página de recursos de Azure que muestra las solicitudes pendientes](./media/pim-resource-roles-activate-your-roles/resources-my-requests.png)

1. Desplácese a la derecha para ver la columna **Estado de solicitud**.

## <a name="cancel-a-pending-request"></a>Cancelación de una solicitud pendiente

Si no necesita activar un rol que requiera aprobación, puede cancelar una solicitud pendiente en cualquier momento.

1. Abra Azure AD Privileged Identity Management.

1. Haga clic en **Mis solicitudes**.

1. Para el rol que desea cancelar, haga clic en el vínculo **Cancelar**.

    Al hacer clic en Cancelar, la solicitud se cancela. Para volver a activar el rol, tendrá que enviar una nueva solicitud de activación.

   ![Mi lista de solicitudes con la acción Cancelar resaltada](./media/pim-resource-roles-activate-your-roles/resources-my-requests-cancel.png)

## <a name="troubleshoot"></a>Solución de problemas

### <a name="permissions-are-not-granted-after-activating-a-role"></a>Permisos no concedidos después de activar un rol

Al activar un rol en PIM, la activación podría no propagarse al instante a todos los portales que requieren el rol con privilegios. A veces, incluso si el cambio se propaga, el almacenamiento en caché web en un portal puede provocar que el cambio no surta efecto de inmediato. Esto es lo que debe de hacer si se retrasa la activación.

1. Cierre sesión en Azure Portal y vuelva a iniciar sesión.

    Al activar un rol de recurso de Azure, verá las fases de la activación. Una vez que finalizan todas las fases, verá un vínculo para **Cerrar sesión**. Puede usar este vínculo para cerrar la sesión. Esto solucionará la mayoría de los casos de retraso de activación.

1. En PIM, compruebe que aparece como miembro del rol.

## <a name="next-steps"></a>Pasos siguientes

- [Ampliación o renovación de roles de recursos de Azure en PIM](pim-resource-roles-renew-extend.md)
- [Activación de mis roles de Azure AD en PIM](pim-how-to-activate-role.md)
