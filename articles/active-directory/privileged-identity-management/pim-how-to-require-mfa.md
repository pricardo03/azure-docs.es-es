---
title: 'Autenticación multifactor (MFA) y PIM: Azure Active Directory | Microsoft Docs'
description: Obtenga información sobre cómo Azure AD Privileged Identity Management (PIM) valida la autenticación multifactor (MFA).
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 08/31/2018
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 686c39c9fb1f1ff7c0ecf068a6612f530620d5dc
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804302"
---
# <a name="multi-factor-authentication-mfa-and-pim"></a>Autenticación multifactor (MFA) y PIM

Se recomienda exigir la autenticación multifactor (MFA) para todos los administradores. De esta forma, se reduce el riesgo de ataques debidos a que una contraseña se ha puesto en peligro.

Puede exigir que los usuarios completen un desafío de MFA cuando inician sesión. También puede exigir que los usuarios completen un desafío de MFA cuando activan un rol en Azure Active Directory (Azure AD) Privileged Identity Management (PIM). De este modo, si el usuario no realizó un desafío de MFA cuando inició sesión, se le pedirá que lo haga en PIM.

> [!IMPORTANT]
> Ahora mismo, Azure MFA solo funciona con cuentas profesionales o educativas, no con cuentas de Microsoft (por lo habitual una cuenta personal que se usa para iniciar sesión en servicios de Microsoft, como Skype, Xbox, Outlook.com, etc.). Por este motivo, un usuario que use una cuenta de Microsoft no puede ser un administrador apto porque no podrá usar MFA para activar sus roles. Si estos usuarios tienen que seguir administrando cargas de trabajo con una cuenta de Microsoft, conviértalos en administradores permanentes por el momento.

## <a name="how-pim-validates-mfa"></a>Cómo PIM valida MFA

Hay dos opciones para validar MFA cuando un usuario activa un rol.

La opción más sencilla es depender de Azure MFA en el caso de los usuarios que activan un rol con privilegios. Para ello, primero compruebe que dichos usuarios cuentan con una licencia, si es necesario, y que se han registrado en Azure MFA. Para obtener más información sobre cómo implementar Azure MFA, consulte [Implementación de Azure Multi-factor Authentication en la nube](../authentication/howto-mfa-getstarted.md). Se recomienda, aunque no es necesario, configurar Azure AD para imponer MFA en estos usuarios cuando inicien sesión. Esto se debe a que PIM realizará las comprobaciones de MFA.

Si los usuarios se autentican en el entorno local, también puede hacer que el proveedor de identidades se encargue de MFA. Por ejemplo, si ha configurado Servicios de federación de AD para exigir la autenticación basada en tarjeta inteligente antes de acceder a Azure AD, el artículo [Protección de recursos en la nube con Azure Multi-Factor Authentication y AD FS](../authentication/howto-mfa-adfs.md) incluye instrucciones para configurar AD FS para enviar notificaciones a Azure AD. Cuando un usuario intenta activar un rol, PIM aceptará que MFA ya se ha validado para el usuario cuando este último reciba las notificaciones adecuadas.

## <a name="next-steps"></a>Pasos siguientes

- [Configuración de roles de Azure AD en PIM](pim-how-to-change-default-settings.md)
- [Configuración de roles de recurso de Azure en PIM](pim-resource-roles-configure-role-settings.md)
