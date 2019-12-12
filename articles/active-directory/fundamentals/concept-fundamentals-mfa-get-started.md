---
title: 'Habilitación de Multi-Factor Authentication para una organización: Azure Active Directory'
description: Habilitación de Azure MFA para una organización según la licencia
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 12/06/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: fe0d79f228ee2b84c00a35d4836cbda6a4847a42
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2019
ms.locfileid: "74932177"
---
# <a name="enable-multi-factor-authentication-for-your-organization"></a>Habilitación de Azure Multi-Factor Authentication para una organización

Hay varias maneras de habilitar Azure Multi-Factor Authentication (MFA) para los usuarios de Azure Active Directory (AD) en función de las licencias que posea una organización. 

![Investigación de las señales y aplicación de MFA si es necesario](./media/concept-fundamentals-mfa-get-started/verify-signals-and-perform-mfa-if-required.png)

Según nuestros estudios, es prácticamente improbable que una cuenta se comprometa si se usa MFA.

Así pues, ¿cómo activa la organización la autenticación multifactor incluso de forma gratuita antes de convertirse en una estadística?

## <a name="free-option"></a>Opción gratuita

Los clientes que usen las ventajas gratuitas de Azure AD pueden emplear [valores predeterminados de seguridad](../fundamentals/concept-fundamentals-security-defaults.md) para habilitar la autenticación multifactor en su entorno.

## <a name="office-365"></a>Office 365

En el caso de los clientes con Office 365, hay dos opciones:

- Se pueden habilitar [valores predeterminados de seguridad](concept-fundamentals-security-defaults.md) mediante Azure AD para proteger a todos los usuarios con Azure Multi-Factor Authentication.
- Si su organización requiere una mayor granularidad a la hora de proporcionar autenticación multifactor, las licencias de Office incluyen funcionalidades de [MFA por usuario](../authentication/howto-mfa-userstates.md). Los administradores habilitan y aplican la MFA por usuario de forma individual.

## <a name="azure-ad-premium-p1"></a>Azure AD Premium P1

Para clientes con Azure AD Premium P1 o licencias similares que incluyen esta funcionalidad, como Enterprise Mobility + Security E3, Microsoft 365 F1 o Microsoft 365 E3: 

La recomendación es usar [directivas de acceso condicional](../conditional-access/concept-conditional-access-policy-common.md) para obtener la mejor experiencia de usuario.

## <a name="azure-ad-premium-p2"></a>Azure AD Premium P2

Para clientes con Azure AD Premium P2 o licencias similares que incluyen esta funcionalidad, como Enterprise Mobility + Security E5 o Microsoft 365 E5: 

La recomendación es usar [directivas de acceso condicional](../conditional-access/concept-conditional-access-policy-common.md) junto con [Identity Protection](../identity-protection/overview-v2.md) para ofrecer la mejor experiencia de usuario y flexibilidad de cumplimiento.

## <a name="authentication-methods"></a>Métodos de autenticación

|   | Valores predeterminados de seguridad | Todos los demás métodos |
| --- | --- | --- |
| Notificación a través de aplicación móvil | X | X |
| Código de verificación de aplicación móvil o token de hardware |   | X |
| Mensaje de texto al teléfono |   | X |
| Llamada al teléfono |   | X |
| Contraseñas de aplicación |   | X** |

** Las contraseñas de aplicación solo están disponibles en MFA por usuario con escenarios de autenticación heredados si las habilitan los administradores.

## <a name="next-steps"></a>Pasos siguientes

[Página de precios de Azure AD](https://azure.microsoft.com/pricing/details/active-directory/)
