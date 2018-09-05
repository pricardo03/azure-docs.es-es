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
ms.openlocfilehash: 20a704a0d5b61134a61b5cbf02a1c71dbc7039e1
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189344"
---
# <a name="configure-azure-ad-directory-role-settings-in-pim"></a>Configuración de roles de directorio de Azure AD en PIM

Un administrador de roles con privilegios puede personalizar Privileged Identity Management (PIM) de Azure AD en su organización, como cambiar la experiencia de un usuario que va a activar una asignación de rol apto.

## <a name="open-role-settings"></a>Apertura de la configuración de roles

1. Abra **Azure AD Privileged Identity Management**.

1. Haga clic en **Roles de directorio de Azure AD**.

1. Haga clic en **Configuración**.

    ![Roles de directorio de Azure AD - Configuración](./media/pim-how-to-change-default-settings/pim-directory-roles-settings.png)

1. Haga clic en **Roles**.

1. Haga clic en el rol cuya configuración desea configurar.

    ![Roles de directorio de Azure AD: roles de configuración](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-role.png)

    En la página de configuración de cada rol, hay varios valores que puede configurar. Dichos valores solo afectan a los usuarios que son asignaciones **elegibles**, no asignaciones **permanentes**.

## <a name="activations"></a>Activaciones

El control deslizante **Activaciones** es el tiempo máximo, en horas, que un rol permanece activo antes de expirar. Este valor puede oscilar entre 1 y 72 horas.

## <a name="notifications"></a>Notificaciones

El conmutador **Notificaciones** permite elegir si el sistema envía correos electrónicos a los administradores para confirmar que han activado un rol. Puede ser útil para detectar activaciones no autorizadas o ilegales.

## <a name="incidentrequest-ticket"></a>Vale de solicitud o incidencia

El conmutador **Vale de solicitud o incidencia** le permite elegir si se requiere que los administradores aptos incluyan un número de vale al activar su rol. Puede ser útil al realizar auditorías de acceso a rol.

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication

El conmutador **Multi-Factor Authentication** le permite elegir si se requiere que los usuarios comprueben su identidad con MFA para poder activar sus roles. Solo tiene que realizar esta acción una vez por sesión, no cada vez que activen un rol. Al habilitar MFA, es conveniente tener en cuenta dos sugerencias:

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

Para más información acerca del uso de MFA con PIM, consulte [Exigencia de MFA en Privileged Identity Management de Azure AD](pim-how-to-require-mfa.md).

## <a name="require-approval"></a>Requerir aprobación

El conmutador **Requerir aprobación** le permite elegir si desea solicitar aprobación para activar este rol.

1. Al establecer el modificador en **Habilitado**, el panel se expande con opciones para seleccionar aprobadores.

    ![Roles de directorio de Azure AD - Configuración - Requerir aprobación](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-require-approval.png)

    Si **NO** especifica ningún aprobador, los administradores de roles con privilegios pasarán a ser los aprobadores predeterminados. Se debería requerir que los administradores de roles con privilegios aprueben **TODAS** las solicitudes de activación de este rol.

1. Para especificar los aprobadores, haga clic en **Seleccionar aprobadores**.

    ![Roles de directorio de Azure AD - Configuración - Requerir aprobación](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-require-approval-select-approvers.png)

1. Seleccione uno o varios aprobadores y haga clic en **Seleccionar**. Puede seleccionar usuarios o grupos. Se recomienda al menos 2 aprobadores. No se admite la aprobación automática.

    Su elección aparecerá en la lista de aprobadores seleccionados.

1. Una vez que haya especificado la configuración de todos los roles, haga clic en **Guardar** para guardar los cambios.


<!--PLACEHOLDER: Need an explanation of what the temporary Global Administrator setting is for.-->

## <a name="next-steps"></a>Pasos siguientes

- [Exigencia de MFA en Privileged Identity Management de Azure AD](pim-how-to-require-mfa.md)
- [Configuración de alertas de seguridad en Privileged Identity Management de Azure AD](pim-how-to-configure-security-alerts.md)
