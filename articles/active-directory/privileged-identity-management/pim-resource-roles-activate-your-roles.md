---
title: Activar mis roles de recursos de Azure en PIM - Azure Active Directory | Microsoft Docs
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
ms.date: 04/09/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 04e8615cc5534255308c35fa1f675ef3a85aa84e
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59493559"
---
# <a name="activate-my-azure-resource-roles-in-pim"></a>Activación de mis roles de recursos de Azure en PIM

Con Azure Active Directory (Azure AD) Privileged Identity Management (PIM), los miembros del rol para recursos de Azure pueden programar la activación de una fecha y hora futuras. Asimismo, también pueden seleccionar una duración de activación específica dentro del valor máximo establecido (configurado por los administradores).

Este artículo se dirige a los miembros que necesitan activar su rol de recursos de Azure en PIM.

## <a name="activate-a-role"></a>Activación de un rol

Cuando necesite asumir un rol de recursos de Azure, puede solicitar la activación mediante la opción de navegación **Mis roles** de PIM.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).

1. Abra **Azure AD Privileged Identity Management**. Para obtener información acerca de cómo agregar el icono de PIM al panel, consulte [Comenzar a usar Azure AD Privileged Identity Management](pim-getting-started.md).

1. Haga clic en **Mis roles**.

    ![Roles de recursos de Azure - Mis roles y roles de Azure AD](./media/pim-resource-roles-activate-your-roles/resources-my-roles.png)

1. Haga clic en **roles de recursos de Azure** para ver una lista de los roles de recursos de Azure válidas.

   ![Roles de recursos de Azure](./media/pim-resource-roles-activate-your-roles/resources-my-roles-azure-resources.png) 

1. En la lista **Roles de recursos de Azure**, busque el rol que desea activar.

    ![Roles de recursos de Azure: mi lista de roles](./media/pim-resource-roles-activate-your-roles/resources-my-roles-activate.png)

1. Haga clic en **Activar** para abrir el panel de activación.

1. Si el rol requiere la autenticación multifactor (MFA), haga clic en **Compruebe su identidad antes de proceder**. Solo tiene que autenticarse una vez por sesión.

    ![Comprobar con MFA antes de la activación del rol](./media/pim-resource-roles-activate-your-roles/resources-my-roles-mfa.png)

1. Haga clic en **Comprobar mi identidad** y siga las instrucciones para realizar una comprobación de seguridad adicional.

    ![Comprobación de seguridad adicional](./media/pim-resource-roles-activate-your-roles/resources-mfa-enter-code.png)

1. Si desea especificar un ámbito reducido, haga clic en **Ámbito** para abrir el panel de filtro de recursos.

    Es una práctica recomendada solo solicitar acceso a los recursos que necesita. En el panel de filtro de recursos, puede especificar los grupos de recursos o recursos a los que necesita acceso.

    ![Activar: filtro de recursos](./media/pim-resource-roles-activate-your-roles/resources-my-roles-resource-filter.png)

1. Si es necesario, especifique una hora de inicio de activación personalizada. El miembro se activaría después de la hora seleccionada.

1. En el campo **Razón**, escriba el motivo de la solicitud de activación.

    ![Panel de activación completado](./media/pim-resource-roles-activate-your-roles/resources-my-roles-activate-done.png)

1. Haga clic en **Activar**.

    Si el rol no requiere aprobación, se activa y se agrega a la lista de roles activos. Si desea usar el rol, siga los pasos descritos en la sección siguiente.

    Si el [rol requiere aprobación](pim-resource-roles-approval-workflow.md) para activarse, aparecerá una notificación en la esquina superior del explorador que le informa de que la solicitud está pendiente de aprobación.

    ![Notificación de solicitud pendiente](./media/pim-resource-roles-activate-your-roles/resources-my-roles-activate-notification.png)

## <a name="use-a-role-immediately-after-activation"></a>Uso de un rol inmediatamente después de la activación

En el caso de cualquier retraso tras la activación, siga estos pasos después de activar para usar los roles de recursos de Azure de forma inmediata.

1. Abra Azure AD Privileged Identity Management.

1. Haga clic en **Mis roles** para ver una lista de su derecho a roles de recursos de Azure y roles de Azure AD.

1. Haga clic en **roles de recursos de Azure**.

1. Haga clic en el **roles activos** ficha.

1. Una vez que el rol está activo, cierre sesión en el portal e iníciela de nuevo.

    El rol debe ser ahora disponible para su uso.

## <a name="view-the-status-of-your-requests"></a>Visualización del estado de las solicitudes

Puede ver el estado de las solicitudes pendientes de activación.

1. Abra Azure AD Privileged Identity Management.

1. Haga clic en **Mis solicitudes** para ver una lista de su rol de Azure AD y el rol de recursos de Azure las solicitudes.

    ![Roles de Azure AD y los roles de recursos de Azure - Mis solicitudes](./media/pim-resource-roles-activate-your-roles/resources-my-requests.png)

1. Desplácese a la derecha para ver la columna **Estado de solicitud**.

## <a name="cancel-a-pending-request"></a>Cancelación de una solicitud pendiente

Si no necesita activar un rol que requiera aprobación, puede cancelar una solicitud pendiente en cualquier momento.

1. Abra Azure AD Privileged Identity Management.

1. Haga clic en **Mis solicitudes**.

1. Para el rol que desea cancelar, haga clic en el vínculo **Cancelar**.

    Al hacer clic en Cancelar, se cancelará la solicitud. Para volver a activar el rol, tendrá que enviar una nueva solicitud de activación.

   ![Cancelación de una solicitud pendiente](./media/pim-resource-roles-activate-your-roles/resources-my-requests-cancel.png)

## <a name="troubleshoot"></a>Solución de problemas

### <a name="permissions-not-granted-after-activating-a-role"></a>Permisos no concedidos después de activar un rol

Cuando se activa un rol en PIM, transcurren al menos 10 minutos antes de que pueda acceder al portal de administración deseado o realizar funciones en una carga de trabajo administrativa específica. Una vez completada la activación, cierre sesión en el portal de Azure e inicie sesión en empezar a usar la función recién creada.

Si necesita pasos de solución de problemas adicionales, consulte el artículo sobre la [solución de problemas de permisos elevados](https://social.technet.microsoft.com/wiki/contents/articles/37568.troubleshooting-elevated-permissions-with-azure-ad-privileged-identity-management.aspx).

### <a name="cannot-activate-a-role-due-to-a-resource-lock"></a>No se puede activar un rol debido a un bloqueo de recursos

Si recibe un mensaje de que un recurso de Azure está bloqueado al intentar activar un rol, es posible que un recurso dentro del ámbito de una asignación de roles tenga un bloqueo de recurso. Los bloqueos protegen los recursos de la eliminación accidental o de cambios inesperados. Un bloqueo también impide que PIM quite una asignación de roles en el recurso al final del período de activación. Puesto que PIM no puede funcionar correctamente cuando se aplica un bloqueo, PIM impide a los usuarios la activación de roles en el recurso. Hay dos maneras de solucionar este problema:

- Elimine el bloqueo, como se describe en [Bloqueo de recursos para impedir cambios inesperados](../../azure-resource-manager/resource-group-lock-resources.md).
- Si desea mantener el bloqueo, realice la asignación de roles permanente o use una cuenta de emergencia.

## <a name="next-steps"></a>Pasos siguientes

- [Ampliación o renovación de roles de recursos de Azure en PIM](pim-resource-roles-renew-extend.md)
- [Activar mis roles de Azure AD en PIM](pim-how-to-activate-role.md)
