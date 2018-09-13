---
title: Configuración de roles de directorio de Azure AD en PIM | Microsoft Docs
description: Aprenda a configurar los roles de directorio de Azure AD en Azure AD Privileged Identity Management (PIM).
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
ms.openlocfilehash: 2d7226f18eb922eaba3c8184656560c33202ef56
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2018
ms.locfileid: "43665441"
---
# <a name="configure-azure-ad-directory-role-settings-in-pim"></a>Configuración de roles de directorio de Azure AD en PIM

Un administrador de roles con privilegios puede personalizar Privileged Identity Management (PIM) de Azure AD en su organización, como cambiar la experiencia de un usuario que va a activar una asignación de rol apto.

## <a name="open-role-settings"></a>Apertura de la configuración de roles

Siga estos pasos para abrir la configuración de un rol de directorio de Azure AD.

1. Abra **Azure AD Privileged Identity Management**.

1. Haga clic en **Roles de directorio de Azure AD**.

1. Haga clic en **Configuración**.

    ![Roles de directorio de Azure AD - Configuración](./media/pim-how-to-change-default-settings/pim-directory-roles-settings.png)

1. Haga clic en **Roles**.

1. Haga clic en el rol cuya configuración desea configurar.

    ![Roles de directorio de Azure AD: roles de configuración](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-role.png)

    En la página de configuración de cada rol, hay varios valores que puede configurar. Dichos valores solo afectan a los usuarios que son asignaciones **elegibles**, no asignaciones **permanentes**.

## <a name="activations"></a>Activaciones

Use control deslizante **Activaciones** para establecer el tiempo máximo, en horas, que un rol permanece activo antes de expirar. Este valor puede oscilar entre 1 y 72 horas.

## <a name="notifications"></a>Notificaciones

Use el modificador **Notificaciones** para especificar si el sistema envía correos electrónicos a los administradores para confirmar que han activado un rol. Puede ser útil para detectar activaciones no autorizadas o ilegales.

## <a name="incidentrequest-ticket"></a>Vale de solicitud o incidencia

Use el modificador **Vale de solicitud o incidente** para especificar si se requiere que los administradores elegibles incluyan un número de vale al activar su rol. Puede ser útil al realizar auditorías de acceso a rol.

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication

Use el modificador **Autenticación multifactor** para especificar si se requiere que los usuarios comprueben su identidad con MFA antes de poder activar sus roles. Solo tiene que realizar esta acción una vez por sesión, no cada vez que activen un rol. Al habilitar MFA, es conveniente tener en cuenta dos sugerencias:

* Los usuarios que tienen cuentas de Microsoft para sus direcciones de correo electrónico (normalmente @outlook.com, pero no siempre) no se pueden registrar en Azure MFA. Si quiere asignar roles a los usuarios con cuentas de Microsoft, debe establecerlos en administradores permanentes o deshabilitar MFA para ese rol.
* No se puede deshabilitar MFA para roles con privilegios elevados en Azure AD y Office 365. Esta es una característica de seguridad porque estos roles deben protegerse cuidadosamente:  
  
  * Administrador de aplicaciones
  * Administrador del servidor proxy de la aplicación
  * Administrador de facturación  
  * Administrador de cumplimiento  
  * Administrador de servicios de CRM
  * Aprobador del acceso a la Caja de seguridad del cliente
  * Escritor de directorio  
  * Administrador de Exchange  
  * Administrador global
  * Administrador de servicios de Intune
  * Administrador de buzón de correo  
  * Soporte para asociados de nivel 1  
  * Soporte para asociados de nivel 2  
  * Administrador de roles con privilegios
  * Administrador de seguridad  
  * Administrador de SharePoint  
  * Administrador de Skype Empresarial  
  * Administrador de cuenta de usuario  

Para obtener más información, consulte [Autenticación multifactor (MFA) y PIM](pim-how-to-require-mfa.md).

## <a name="require-approval"></a>Requerir aprobación

Si desea solicitar aprobación para activar un rol, siga estos pasos.

1. Establezca el modificador **Requerir aprobación** en **Habilitado**. El panel se expande con opciones para seleccionar aprobadores.

    ![Roles de directorio de Azure AD - Configuración - Requerir aprobación](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-require-approval.png)

    Si **NO** especifica ningún aprobador, los administradores de roles con privilegios pasarán a ser los aprobadores predeterminados. Se debería requerir que los administradores de roles con privilegios aprueben **TODAS** las solicitudes de activación de este rol.

1. Para especificar los aprobadores, haga clic en **Seleccionar aprobadores**.

    ![Roles de directorio de Azure AD - Configuración - Requerir aprobación](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-require-approval-select-approvers.png)

1. Seleccione uno o varios aprobadores y haga clic en **Seleccionar**. Puede seleccionar usuarios o grupos. Se recomienda al menos 2 aprobadores. No se admite la aprobación automática.

    Su elección aparecerá en la lista de aprobadores seleccionados.

1. Una vez que haya especificado la configuración de todos los roles, haga clic en **Guardar** para guardar los cambios.


<!--PLACEHOLDER: Need an explanation of what the temporary Global Administrator setting is for.-->

## <a name="next-steps"></a>Pasos siguientes

- [Asignación de roles de directorio de Azure AD en PIM](pim-how-to-add-role-to-user.md)
- [Configuración de alertas de seguridad para roles de directorio de Azure AD en PIM](pim-how-to-configure-security-alerts.md)
