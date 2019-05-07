---
title: Configuración de la directiva de registro de autenticación multifactor en Azure Active Directory Identity Protection | Microsoft Docs
description: Aprenda cómo configurar la directiva de registro de autenticación multifactor en Azure AD Identity Protection.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: article
ms.date: 05/01/2019
author: MicrosoftGuyJFlo
manager: daveba
ms.author: joflore
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1f4083ddf849842358f7699badca6598e56e4dee
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65139383"
---
# <a name="how-to-configure-the-azure-multi-factor-authentication-registration-policy"></a>Instrucciones: Configurar la directiva de registro de Azure Multi-factor Authentication

Azure AD Identity Protection ayuda a administrar la puesta en servicio de registro de la autenticación multifactor (MFA) mediante la configuración de directiva de acceso condicional para requerir MFA registro independientemente de qué aplicaciones están iniciando sesión. Este artículo explica lo que puede usarse la directiva y cómo configurarlo.

## <a name="what-is-the-azure-multi-factor-authentication-registration-policy"></a>¿Qué es la directiva de registro de Azure Multi-factor Authentication?

Azure Multi-factor Authentication proporciona un medio para comprobar que usan más de un nombre de usuario y contraseña. Proporciona una segunda capa de seguridad para inicios de sesión de usuario. En el orden de los usuarios puedan responder a las solicitudes MFA, primero debe registrar para la autenticación multifactor de Azure.

Se recomienda requerir Azure Multi-factor Authentication para inicios de sesión de usuario porque lo:

- Ofrece autenticación segura con una gama de opciones de comprobación sencillas.
- Desempeña un papel fundamental en la preparación de su organización para protegerse y recuperarse de eventos de riesgo en Identity Protection

Para obtener más detalles sobre la MFA, consulte [¿qué es Azure Multi-factor Authentication?](../authentication/howto-mfa-getstarted.md)

## <a name="how-do-i-access-the-registration-policy"></a>¿Cómo puedo acceder a la directiva de registro?

En la hoja **Azure AD Identity Protection**, en la sección [Configurar](https://portal.azure.com/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/SignInPolicy), está la directiva de registro de MFA.

![Directiva de MFA](./media/howto-mfa-policy/1014.png)

## <a name="policy-settings"></a>Configuración de directiva

Al configurar la directiva de registro de MFA, deberá realizar los siguientes cambios de configuración:

- Los usuarios y grupos que se aplica la directiva. No olvide excluir de la organización [cuentas de acceso de emergencia](../users-groups-roles/directory-emergency-access.md).

    ![Usuarios y grupos](./media/howto-mfa-policy/11.png)

- El control que desea aplicar - **registro requiere Azure MFA**

    ![Access](./media/howto-mfa-policy/12.png)

- Aplicar la directiva debe establecerse en **en**.

    ![Aplicar directiva](./media/howto-mfa-policy/14.png)

- **Guardar** la directiva

## <a name="user-experience"></a>Experiencia del usuario

Para obtener información general de la experiencia de usuario relacionada, consulte:

- [Flujo de registro de autenticación multifactor](flows.md#multi-factor-authentication-registration).  
- [Experiencias de inicio de sesión con Azure AD Identity Protection](flows.md).  

## <a name="next-steps"></a>Pasos siguientes

Para obtener una visión general de Azure AD Identity Protection, consulte la [Introducción a Azure AD Identity Protection](overview.md).
