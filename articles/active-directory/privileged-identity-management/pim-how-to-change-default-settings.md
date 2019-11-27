---
title: 'Configuración de roles de Azure AD en PIM: Azure AD | Microsoft Docs'
description: Aprenda a configurar roles de Azure AD en Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 11/08/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: ee99a8e75fe8da85b1cf82623ed110991db24b66
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2019
ms.locfileid: "74021809"
---
# <a name="configure-azure-ad-role-settings-in-privileged-identity-management"></a>Configuración de roles de Azure AD en Privileged Identity Management

Un administrador de roles con privilegios puede personalizar Privileged Identity Management (PIM) en su organización de Azure Active Directory (Azure AD), incluido el cambio de la experiencia de un usuario que va a activar una asignación de rol válida.

## <a name="open-role-settings"></a>Apertura de la configuración de roles

Siga estos pasos para abrir la configuración de un rol de Azure AD.

1. Abra **Azure AD Privileged Identity Management**.

1. Haga clic en **Roles de Azure AD**.

1. Haga clic en **Configuración**.

    ![Roles de AD Azure: configuración](./media/pim-how-to-change-default-settings/pim-directory-roles-settings.png)

1. Haga clic en **Roles**.

1. Haga clic en el rol cuya configuración desea configurar.

    ![Roles de AD Azure: roles de configuración](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-role.png)

    En la página de configuración de cada rol, hay varios valores que puede configurar. Dichos valores solo afectan a los usuarios que son asignaciones **elegibles**, no asignaciones **permanentes**.

## <a name="activations"></a>Activaciones

Use control deslizante **Activaciones** para establecer el tiempo máximo, en horas, que un rol permanece activo antes de expirar. Este valor puede oscilar entre 1 y 72 horas.

## <a name="notifications"></a>Notificaciones

Use el conmutador **Notificaciones** para especificar si los administradores recibirán notificaciones por correo electrónico cuando se activan los roles. Esta notificación puede ser útil para detectar activaciones no autorizadas o ilícitas.

Cuando se establece en **Habilitar**, las notificaciones se envían al:

- Administrador de roles con privilegios
- Administrador de seguridad
- Administrador global

Para más información, consulte [Notificaciones por correo electrónico en Privileged Identity Management](pim-email-notifications.md).

## <a name="incidentrequest-ticket"></a>Vale de solicitud o incidencia

Use el modificador **Vale de solicitud o incidente** para requerir que los administradores válidos incluyan un número de vale al activar su rol. Esta práctica puede hacer que las auditorías de acceso de los roles sean más eficaces.

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication

Use el modificador **Autenticación multifactor** para especificar si se requiere que los usuarios comprueben su identidad con MFA antes de poder activar sus roles. Solo tienen que comprobar su identidad una vez por sesión, no cada vez que activan un rol. Al habilitar MFA, es conveniente tener en cuenta dos sugerencias:

- Los usuarios que tienen cuentas de Microsoft para sus direcciones de correo electrónico (normalmente @outlook.com, pero no siempre) no se pueden registrar en Azure Multi-Factor Authentication. Si quiere asignar roles a los usuarios con cuentas de Microsoft, debe establecerlos en administradores permanentes o deshabilitar la autenticación multifactor para ese rol.
- No se puede deshabilitar Azure Multi-Factor Authentication para roles con privilegios elevados en Azure AD y Office 365. Esta característica de seguridad ayuda a proteger los roles siguientes:  
  
  - Administrador de Azure Information Protection
  - Administrador de facturación
  - Administrador de aplicaciones en la nube
  - Administrador de cumplimiento
  - Administrador de acceso condicional
  - Administrador de Dynamics 365
  - Aprobador del acceso a la Caja de seguridad del cliente
  - Escritores de directorios
  - Administrador de Exchange
  - Administrador global
  - Administrador de Intune
  - Administrador de Power BI
  - Administrador de roles con privilegios
  - Administrador de seguridad
  - Administrador de SharePoint
  - Administrador de Skype Empresarial
  - Administrador de usuarios

Para obtener más información, consulte [Autenticación multifactor y Privileged Identity Management](pim-how-to-require-mfa.md).

## <a name="require-approval"></a>Requerir aprobación

Si desea delegar la aprobación necesaria para activar un rol, siga estos pasos.

1. Establezca el modificador **Requerir aprobación** en **Habilitado**. El panel se expande con opciones para seleccionar aprobadores.

    ![Roles de Azure AD - Configuración - Requerir aprobación](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-require-approval.png)

    Si no especifica ningún aprobador, el administrador de roles con privilegios se convierte en el aprobador predeterminado y, a continuación, se le solicita que apruebe todas las solicitudes de activación para este rol.

1. Para especificar los aprobadores, haga clic en **Seleccionar aprobadores**.

    ![Roles de Azure AD - Configuración - Requerir aprobación](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-require-approval-select-approvers.png)

1. Seleccione uno o varios aprobadores además del administrador de roles con privilegios y, a continuación, haga clic en **Seleccionar**. Puede seleccionar usuarios o grupos. Se recomienda al menos dos aprobadores. Incluso si se agrega a sí mismo como aprobador, no se puede aprobar a sí mismo una activación de rol. Su elección aparecerá en la lista de aprobadores seleccionados.

1. Una vez que haya especificado todos los valores del rol, seleccione **Guardar** para guardar los cambios.

<!--PLACEHOLDER: Need an explanation of what the temporary Global Administrator setting is for.-->

## <a name="next-steps"></a>Pasos siguientes

- [Asignación de roles de Azure AD en Privileged Identity Management](pim-how-to-add-role-to-user.md)
- [Configuración de alertas de seguridad para roles de Azure AD en Privileged Identity Management](pim-how-to-configure-security-alerts.md)
