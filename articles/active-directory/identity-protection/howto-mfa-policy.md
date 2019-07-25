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
ms.openlocfilehash: 434d07163713a139b42a5dbe1664f81dafc2a1ca
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67108940"
---
# <a name="how-to-configure-the-azure-multi-factor-authentication-registration-policy"></a>Instrucciones: Configuración de la directiva de registro de Azure Multi-Factor Authentication

Azure AD Identity Protection le ayuda a administrar la puesta en servicio del registro de Multi-Factor Authentication (MFA) mediante la configuración de una directiva de acceso condicional para requerir el registro de MFA, independientemente de la aplicación en que inicie sesión. En este artículo se explica para qué se puede usar la directiva y cómo configurarla.

## <a name="what-is-the-azure-multi-factor-authentication-registration-policy"></a>¿Qué es la directiva de registro de la autenticación multifactor de Azure?

Azure Multi-factor Authentication proporciona un medio para verificar su identidad más allá del nombre de usuario y la contraseña. Proporciona una segunda capa de seguridad a los inicios de sesión de los usuarios. Para que los usuarios puedan responder a las solicitudes MFA, primero debe registrarse para Azure Multi-Factor Authentication.

Se recomienda requerir Azure Multi-Factor Authentication en los inicios de sesión de usuario por los siguientes motivos:

- Ofrece autenticación segura con una gama de opciones de comprobación sencillas.
- Desempeña un papel fundamental en la preparación de su organización para protegerse y recuperarse en caso de eventos de riesgo en Identity Protection

Para obtener más información sobre MFA, consulte [Qué es Azure Multi-Factor Authentication](../authentication/howto-mfa-getstarted.md)

## <a name="how-do-i-access-the-registration-policy"></a>¿Cómo puedo acceder a la directiva de registro?

En la hoja **Azure AD Identity Protection**, en la sección [Configurar](https://portal.azure.com/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/SignInPolicy), está la directiva de registro de MFA.

![Directiva de MFA](./media/howto-mfa-policy/1014.png)

## <a name="policy-settings"></a>Configuración de directiva

Al configurar la directiva de registro de MFA, deberá realizar los siguientes cambios de configuración:

- Usuarios y grupos a los que se aplica la directiva. Recuerde excluir las [cuentas de acceso de emergencia](../users-groups-roles/directory-emergency-access.md) de la organización.

    ![Usuarios y grupos](./media/howto-mfa-policy/11.png)

- El control que quiere aplicar: **se requiere registro en Azure MFA**

    ![Access](./media/howto-mfa-policy/12.png)

- La opción Aplicar directiva debe establecerse en **Activado**.

    ![Aplicar directiva](./media/howto-mfa-policy/14.png)

- **Guarde** la directiva

## <a name="user-experience"></a>Experiencia del usuario

Para obtener información general de la experiencia de usuario relacionada, consulte:

- [Flujo de registro de autenticación multifactor](flows.md#multi-factor-authentication-registration).  
- [Experiencias de inicio de sesión con Azure AD Identity Protection](flows.md).  

## <a name="next-steps"></a>Pasos siguientes

Para obtener una visión general de Azure AD Identity Protection, consulte la [Introducción a Azure AD Identity Protection](overview.md).
