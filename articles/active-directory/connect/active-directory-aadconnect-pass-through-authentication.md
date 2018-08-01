---
title: 'Azure AD Connect: Autenticación de paso a través | Microsoft Docs'
description: En este artículo se describe la autenticación de paso a través de Azure Active Directory (Azure AD) y cómo permite inicios de sesión de Azure AD mediante la validación de las contraseñas de los usuarios en una instancia local de Active Directory.
services: active-directory
keywords: qué es la autenticación de paso a través de Azure AD Connect, instalar Active Directory, componentes necesarios para Azure AD, SSO, inicio de sesión único
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: dfee42f813989da2333720ac92313344343d57a7
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2018
ms.locfileid: "39214036"
---
# <a name="user-sign-in-with-azure-active-directory-pass-through-authentication"></a>Inicio de sesión del usuario con la autenticación de paso a través de Azure Active Directory

## <a name="what-is-azure-active-directory-pass-through-authentication"></a>¿Qué es la autenticación de paso a través de Azure Active Directory?

La autenticación de paso a través de Azure Active Directory (Azure AD) permite a los usuarios iniciar sesión en aplicaciones basadas en la nube y locales con las mismas contraseñas. Esta característica proporciona a los usuarios una mejor experiencia (una contraseña menos que recordar) y reduce los costos del departamento de soporte técnico de TI dado que es menos probable que olviden cómo iniciar sesión. Cuando los usuarios inician sesión con Azure AD, esta característica valida sus contraseñas directamente con la instancia de Active Directory local.

>[!VIDEO https://www.youtube.com/embed/PyeAC85Gm7w]

Esta característica es una alternativa a la [sincronización de hash de contraseña de Azure AD](active-directory-aadconnectsync-implement-password-hash-synchronization.md), que proporciona la misma ventaja de autenticación en la nube a las organizaciones. Sin embargo, las organizaciones que deseen aplicar sus directivas locales de seguridad y contraseñas de Active Directory, pueden usar la autenticación de paso a través en su lugar. Consulte [esta guía](https://docs.microsoft.com/azure/security/azure-ad-choose-authn) para ver una comparación de los distintos métodos de inicio de sesión de Azure AD y cómo elegir el método correcto para su organización.

![Autenticación de paso a través de Azure AD](./media/active-directory-aadconnect-pass-through-authentication/pta1.png)

Puede combinar la autenticación de paso a través con la característica de [inicio de sesión único de conexión directa](active-directory-aadconnect-sso.md) (SSO). De esta manera, cuando los usuarios accedan a las aplicaciones en sus máquinas corporativas dentro de la red de la empresa, no tendrán que escribir la contraseña para iniciar sesión.

## <a name="key-benefits-of-using-azure-ad-pass-through-authentication"></a>Principales ventajas del uso de la autenticación de paso a través de Azure AD

- *Mejor experiencia del usuario*
  - Los usuarios usan las mismas contraseñas para iniciar sesión tanto en las aplicaciones basadas en la nube como en las locales.
  - Los usuarios dedican menos tiempo a hablar con el departamento de soporte técnico de TI para que resuelvan problemas relacionados con las contraseñas.
  - Los usuarios pueden realizar las tareas de [administración de contraseñas de autoservicio](../authentication/active-directory-passwords-overview.md) en la nube.
- *Es fácil de implementar y administrar*
  - No se requieren complejas implementaciones locales ni la configuración de la red.
  - Solo necesita instalar en local un agente ligero.
  - Sin sobrecarga de administración. El agente recibe automáticamente las mejoras y las correcciones de errores.
- *Protección*
  - Las contraseñas locales nunca se almacenan en la nube.
  - El agente solo realiza conexiones salientes desde dentro de la red. Por lo tanto, no es necesario instalar el agente en una red perimetral, también conocida como DMZ.
  - Protege las cuentas de usuario y, para ello, trabaja sin problemas con [directivas de acceso condicional de Azure AD](../active-directory-conditional-access-azure-portal.md), incluida la autenticación multifactor, el [bloqueo de autenticación heredada](../authentication/howto-password-smart-lockout.md) y el [filtrado de ataques por fuerza bruta](../active-directory-conditional-access-conditions.md).
- *Alta disponibilidad*
  - Se pueden instalar agentes adicionales en varios servidores locales para lograr una alta disponibilidad de las solicitudes de inicio de sesión.

## <a name="feature-highlights"></a>Características destacadas

- Admite el inicio de sesión de usuario en todas las aplicaciones basadas en explorador web y en las aplicaciones cliente de Microsoft Office que usan la [autenticación moderna](https://aka.ms/modernauthga).
- Los nombres de usuario de inicio de sesión pueden ser el predeterminado local (`userPrincipalName`) u otro atributo (conocido como `Alternate ID`) configurado en Azure AD Connect.
- La característica funciona sin problemas con características de [acceso condicional](../active-directory-conditional-access-azure-portal.md), como Multi-Factor Authentication (MFA), para ayudar a proteger a los usuarios.
- Se integra con la [administración de contraseñas de autoservicio](../authentication/active-directory-passwords-overview.md) basada en la nube, incluida la escritura diferida de contraseñas en Active Directory local y la protección con contraseña mediante la prohibición de contraseñas usadas habitualmente.
- Se admiten entornos de varios bosques si hay relaciones de confianza de bosque entre los bosques de AD y si el enrutamiento de sufijos de nombre está configurado correctamente.
- Es una característica gratuita y no es necesario usar ninguna versión de pago de Azure AD para usarla.
- Puede habilitarse a través de [Azure AD Connect](active-directory-aadconnect.md).
- Usa un agente local ligero que escucha las solicitudes de validación de contraseña y las responde.
- La instalación de varios agentes proporciona una alta disponibilidad de las solicitudes de inicio de sesión.
- [Protege](../authentication/howto-password-smart-lockout.md) las cuentas locales frente a ataques de contraseña por fuerza bruta en la nube.

## <a name="next-steps"></a>Pasos siguientes

- [Inicio rápido](active-directory-aadconnect-pass-through-authentication-quick-start.md): preparación y ejecución de la autenticación de paso a través de Azure AD.
- [Migración de AD FS a la autenticación de paso a través](https://github.com/Identity-Deployment-Guides/Identity-Deployment-Guides/blob/master/Authentication/Migrating%20from%20Federated%20Authentication%20to%20Pass-through%20Authentication.docx): una guía detallada para migrar desde AD FS (u cualquier otra tecnología de federación) a la autenticación de paso a través.
- [Bloqueo inteligente](../authentication/howto-password-smart-lockout.md): configuración de la funcionalidad Bloqueo inteligente en su inquilino para proteger las cuentas de usuario.
- [Limitaciones actuales](active-directory-aadconnect-pass-through-authentication-current-limitations.md): conozca qué escenarios son compatibles y cuáles no.
- [Información técnica detallada](active-directory-aadconnect-pass-through-authentication-how-it-works.md): descripción del funcionamiento de esta característica.
- [Preguntas más frecuentes](active-directory-aadconnect-pass-through-authentication-faq.md): respuestas a las preguntas más frecuentes.
- [Solución de problemas](active-directory-aadconnect-troubleshoot-pass-through-authentication.md): información para resolver problemas habituales de esta característica.
- [Análisis a fondo de la seguridad](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md): información técnica detallada adicional sobre la característica.
- [SSO de conexión directa de Azure AD](active-directory-aadconnect-sso.md): más información sobre esta característica complementaria.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): para la tramitación de solicitudes de nuevas características.
