---
title: Activación de mis roles de directorio de Azure AD en PIM | Microsoft Docs
description: Aprenda a activar los roles de directorio de Azure AD en Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 08/27/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 59dab4651366c3ad6579e0da660baee0c653d1a3
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2018
ms.locfileid: "43666009"
---
# <a name="activate-my-azure-ad-directory-roles-in-pim"></a>Activación de mis roles de directorio de Azure AD en PIM

Azure Active Directory (Azure AD) Privileged Identity Management (PIM) simplifica la forma en que las empresas administran el acceso con privilegios a los recursos de Azure AD y otros servicios en línea de Microsoft, como Office 365 o Microsoft Intune.  

Si se le ha considerado apto para un rol administrativo, significa que puede activar el rol cuando tenga la necesidad de realizar acciones que requieran ciertos privilegios. Por ejemplo, si administra de vez en cuando características de Office 365, los administradores de roles con privilegios de su organización puede que no le hayan asignado el rol de administrador global permanente, ya que ese rol afecta también a otros servicios. En su lugar, pueden asignarle roles de Azure AD como administrador de Exchange Online. Puede solicitar la activación de ese rol cuando necesite sus privilegios y tendrá control de administrador durante un período predeterminado.

Este artículo se dirige a los administradores que necesitan activar su rol de directorio de Azure AD en PIM.

## <a name="activate-a-role"></a>Activación de un rol

Cuando necesite asumir un rol de directorio de Azure AD, puede solicitar la activación mediante la opción de navegación **Mis roles** de PIM.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).

1. Abra **Azure AD Privileged Identity Management**. Para obtener información acerca de cómo agregar el icono de PIM al panel, consulte [Comenzar a usar Azure AD Privileged Identity Management](pim-getting-started.md).

1. Haga clic en **Roles de directorio de Azure AD**.

1. Haga clic en **Mis roles** para ver una lista de sus roles de directorio de Azure AD elegibles.

    ![Roles de directorio de Azure AD - Mis roles](./media/pim-how-to-activate-role/directory-roles-my-roles.png)

1. Buscar el rol que desea activar.

    ![Roles de directorio de Azure AD: lista de Mis roles](./media/pim-how-to-activate-role/directory-roles-my-roles-activate.png)

1. Haga clic en **Activate** para abrir el panel de detalles de la activación de rol.

1. Si el rol requiere la autenticación multifactor (MFA), haga clic en **Compruebe su identidad antes de proceder**. Solo tiene que autenticarse una vez por sesión.

    ![Comprobar con MFA antes de la activación del rol](./media/pim-how-to-activate-role/directory-roles-my-roles-mfa.png)

1. Haga clic en **Comprobar mi identidad** y siga las instrucciones para realizar una comprobación de seguridad adicional.

    ![Comprobación de seguridad adicional](./media/pim-how-to-activate-role/additional-security-verification.png)

1. Haga clic en **Activar** para abrir el panel Activación.

    ![Panel Activación](./media/pim-how-to-activate-role/directory-roles-activate.png)

1. Si es necesario, especifique una hora de inicio de activación personalizada.

1. Especifique la duración de la activación.

1. En el campo **Motivo de la activación**, escriba el motivo de la solicitud de activación. Algunos roles requieren que proporcione un número de vale de problema.

    ![Panel Activación completado](./media/pim-how-to-activate-role/directory-roles-activation-pane.png)

1. Haga clic en **Activar**.

    Si el rol no requiere aprobación, se activa ahora y aparece en la lista de roles activos. Si el [rol requiere aprobación](./azure-ad-pim-approval-workflow.md) para activarse, aparecerá una notificación en la esquina superior del explorador que le informa de que la solicitud está pendiente de aprobación.

    ![Notificación de solicitud pendiente](./media/pim-how-to-activate-role/directory-roles-activate-notification.png)

## <a name="view-the-status-of-your-requests"></a>Visualización del estado de las solicitudes

Puede ver el estado de las solicitudes pendientes de activación.

1. Abra Azure AD Privileged Identity Management.

1. Haga clic en **Roles de directorio de Azure AD**.

1. Haga clic en **Mis solicitudes** para ver una lista de sus solicitudes.

    ![Roles de directorio de Azure AD - Mis solicitudes](./media/pim-how-to-activate-role/directory-roles-my-requests.png)

## <a name="use-a-role-immediately-after-activation"></a>Uso de un rol inmediatamente después de la activación

El almacenamiento en caché es el responsable de que las activaciones no se produzcan inmediatamente en Azure Portal sin una actualización. Si necesita reducir la posibilidad de retrasos tras la activación de un rol, puede usar la página **Acceso a la aplicación** del portal. Las aplicaciones a las que se accede desde esta página comprueban si hay nuevas asignaciones de roles de inmediato.

1. Abra Azure AD Privileged Identity Management.

1. Haga clic en la página **Acceso a la aplicación**.

    ![Acceso a la aplicación de PIM](./media/pim-how-to-activate-role/pim-application-access.png)

1. Haga clic en **Azure Active Directory** para volver a abrir el portal en la página **Todos los usuarios**.

    Al hacer clic en este vínculo, se fuerza la actualización y se comprueba si hay nuevas asignaciones de roles de Azure AD.

## <a name="deactivate-a-role"></a>Desactivación de un rol

Cuando se ha activado un rol, si alcanza su límite de tiempo (de duración elegible), se desactiva automáticamente.

Si completa pronto sus tareas de administrador, también puede desactivar un rol de forma manual en Azure AD Privileged Identity Management.

1. Abra Azure AD Privileged Identity Management.

1. Haga clic en **Roles de directorio de Azure AD**.

1. Haga clic en **Mis roles**.

1. Haga clic en **Roles activos** para ver la lista de roles activos.

1. Busque el rol que está usando y haga clic en **Desactivar**.

## <a name="cancel-a-pending-request"></a>Cancelación de una solicitud pendiente

Si no necesita activar un rol que requiera aprobación, puede cancelar una solicitud pendiente en cualquier momento.

1. Abra Azure AD Privileged Identity Management.

1. Haga clic en **Roles de directorio de Azure AD**.

1. Haga clic en **Mis solicitudes**.

1. En el rol que desea cancelar, haga clic en el botón **Cancelar**.

    Al hacer clic en Cancelar, la solicitud se cancelará. Para volver a activar el rol, tendrá que enviar una nueva solicitud de activación.

   ![Cancelación de una solicitud pendiente](./media/pim-how-to-activate-role/directory-role-cancel.png)

## <a name="next-steps"></a>Pasos siguientes

- [Activación de mis roles de recursos de Azure en PIM](pim-resource-roles-activate-your-roles.md)
