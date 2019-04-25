---
title: Autenticación multifactor (MFA) y PIM - Azure Active Directory | Microsoft Docs
description: Obtenga información sobre cómo Azure AD Privileged Identity Management (PIM) valida la autenticación multifactor (MFA).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 08/31/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8831a838a403d58d8673c6400e0dda06c03dc69f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60442682"
---
# <a name="multi-factor-authentication-mfa-and-pim"></a>Autenticación multifactor (MFA) y PIM

Se recomienda exigir la autenticación multifactor (MFA) para todos los administradores. De esta forma, se reduce el riesgo de ataques debidos a que una contraseña se ha puesto en peligro.

Puede exigir que los usuarios completen un desafío de MFA cuando inician sesión. También puede requerir que los usuarios completen un desafío de MFA cuando activan un rol en Azure Active Directory (Azure AD) Privileged Identity Management (PIM). De este modo, si el usuario no realizó un desafío de MFA cuando inició sesión, se le pedirá que lo haga en PIM.

> [!IMPORTANT]
> Ahora mismo, Azure MFA solo funciona con cuentas profesionales o educativas, no con cuentas de Microsoft (por lo habitual una cuenta personal que se usa para iniciar sesión en servicios de Microsoft, como Skype, Xbox, Outlook.com, etc.). Por este motivo, un usuario que use una cuenta de Microsoft no puede ser un administrador apto porque no podrá usar MFA para activar sus roles. Si estos usuarios tienen que seguir administrando cargas de trabajo con una cuenta de Microsoft, conviértalos en administradores permanentes por el momento.

## <a name="how-pim-validates-mfa"></a>Cómo PIM valida MFA

Hay dos opciones para validar MFA cuando un usuario activa un rol.

La opción más sencilla es depender de Azure MFA en el caso de los usuarios que activan un rol con privilegios. Para ello, primero compruebe que dichos usuarios cuentan con una licencia, si es necesario, y que se han registrado en Azure MFA. Para obtener más información sobre cómo implementar Azure MFA, consulte [Implementación de Azure Multi-factor Authentication en la nube](../authentication/howto-mfa-getstarted.md). Se recomienda, aunque no es necesario, configurar Azure AD para imponer MFA en estos usuarios cuando inicien sesión. Esto se debe a que PIM realizará las comprobaciones de MFA.

Si los usuarios se autentican en el entorno local, también puede hacer que el proveedor de identidades se encargue de MFA. Por ejemplo, si ha configurado Servicios de federación de AD para exigir la autenticación basada en tarjeta inteligente antes de acceder a Azure AD, el artículo [Protección de recursos en la nube con Azure Multi-Factor Authentication y AD FS](../authentication/howto-mfa-adfs.md) incluye instrucciones para configurar AD FS para enviar notificaciones a Azure AD. Cuando un usuario intenta activar un rol, PIM aceptará que MFA ya se ha validado para el usuario cuando este último reciba las notificaciones adecuadas.

## <a name="next-steps"></a>Pasos siguientes

- [Configuración de rol de Azure AD en PIM](pim-how-to-change-default-settings.md)
- [Configuración de roles de recurso de Azure en PIM](pim-resource-roles-configure-role-settings.md)
