---
title: Activación de mis roles de recursos de Azure en PIM | Microsoft Docs
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
ms.component: pim
ms.date: 08/31/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 59bce2c61db5838bb21a29757d4e354311ecffd5
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2018
ms.locfileid: "43666254"
---
# <a name="activate-my-azure-resource-roles-in-pim"></a>Activación de mis roles de recursos de Azure en PIM

Con Azure AD Privileged Identity Management (PIM), los miembros del rol elegibles para los recursos de Azure pueden programar la activación de una fecha y hora futuras. Asimismo, también pueden seleccionar una duración de activación específica dentro del valor máximo establecido (configurado por los administradores).

Este artículo se dirige a los miembros que necesitan activar su rol de recursos de Azure en PIM.

## <a name="activate-a-role"></a>Activación de un rol

Cuando necesite asumir un rol de recursos de Azure, puede solicitar la activación mediante la opción de navegación **Mis roles** de PIM.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).

1. Abra **Azure AD Privileged Identity Management**. Para obtener información acerca de cómo agregar el icono de PIM al panel, consulte [Comenzar a usar Azure AD Privileged Identity Management](pim-getting-started.md).

1. Haga clic en **Mis roles** para ver una lista de sus roles de directorio de Azure AD y roles de recursos de Azure elegibles.

    ![Roles de recursos de Azure y roles de directorio de Azure AD: Mis roles](./media/pim-resource-roles-activate-your-roles/resources-my-roles.png)

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

    Si el rol no requiere aprobación, se activa ahora y aparece en la lista de roles activos. Si el [rol requiere aprobación](pim-resource-roles-approval-workflow.md) para activarse, aparecerá una notificación en la esquina superior del explorador que le informa de que la solicitud está pendiente de aprobación.

    ![Notificación de solicitud pendiente](./media/pim-resource-roles-activate-your-roles/resources-my-roles-activate-notification.png)

## <a name="view-the-status-of-your-requests"></a>Visualización del estado de las solicitudes

Puede ver el estado de las solicitudes pendientes de activación.

1. Abra Azure AD Privileged Identity Management.

1. Haga clic en **Mis solicitudes** para ver una lista de sus solicitudes de rol de directorio de Azure AD y rol de recursos de Azure.

    ![Roles de directorio de Azure AD y roles de recursos de Azure: Mis roles](./media/pim-resource-roles-activate-your-roles/resources-my-requests.png)

1. Desplácese a la derecha para ver la columna **Estado de solicitud**.

## <a name="use-a-role-immediately-after-activation"></a>Uso de un rol inmediatamente después de la activación

El almacenamiento en caché es el responsable de que las activaciones no se produzcan inmediatamente en Azure Portal sin una actualización. Si necesita reducir la posibilidad de retrasos tras la activación de un rol, puede usar la página **Acceso a la aplicación** del portal. Las aplicaciones a las que se accede desde esta página comprueban si hay nuevas asignaciones de roles de inmediato.

1. Abra Azure AD Privileged Identity Management.

1. Haga clic en la página **Acceso a la aplicación**.

    ![Acceso a la aplicación de PIM: captura de pantalla](./media/pim-resource-roles-activate-your-roles/pim-application-access.png)

1. Haga clic en **Recursos de Azure** para volver a abrir el portal en la página **Todos los recursos**.

    Al hacer clic en este vínculo, se fuerza la actualización y se comprueba si hay nuevas asignaciones de roles de recursos de Azure.

## <a name="cancel-a-pending-request"></a>Cancelación de una solicitud pendiente

Si no necesita activar un rol que requiera aprobación, puede cancelar una solicitud pendiente en cualquier momento.

1. Abra Azure AD Privileged Identity Management.

1. Haga clic en **Mis solicitudes**.

1. Para el rol que desea cancelar, haga clic en el vínculo **Cancelar**.

    Al hacer clic en Cancelar, la solicitud se cancelará. Para volver a activar el rol, tendrá que enviar una nueva solicitud de activación.

   ![Cancelación de una solicitud pendiente](./media/pim-resource-roles-activate-your-roles/resources-my-requests-cancel.png)

## <a name="next-steps"></a>Pasos siguientes

- [Ampliación o renovación de roles de recursos de Azure en PIM](pim-resource-roles-renew-extend.md)
- [Activación de mis roles de directorio de Azure AD en PIM](pim-how-to-activate-role.md)