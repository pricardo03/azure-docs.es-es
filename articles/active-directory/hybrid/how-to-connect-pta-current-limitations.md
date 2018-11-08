---
title: 'Autenticación de paso a través de Azure AD: limitaciones actuales | Microsoft Docs'
description: En este artículo se describen las limitaciones actuales de la autenticación de paso a través de Azure Active Directory (Azure AD).
services: active-directory
keywords: Autenticación de paso a través de Azure AD Connect, instalación de Active Directory, componentes necesarios para Azure AD, SSO, inicio de sesión único
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/04/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: ae6964b36f837931f2278ceef0b330f9badb1d71
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2018
ms.locfileid: "51282720"
---
# <a name="azure-active-directory-pass-through-authentication-current-limitations"></a>Autenticación de paso a través de Azure Active Directory: limitaciones actuales

>[!IMPORTANT]
>La autenticación de paso a través de Azure Active Directory (Azure AD) es una característica gratuita y no es necesario tener ninguna versión de pago de Azure AD para usarla. La autenticación de paso a través solo está disponible en la instancia mundial de Azure AD y no en la [nube Microsoft Azure Alemania](http://www.microsoft.de/cloud-deutschland) ni en la [nube Microsoft Azure Government](https://azure.microsoft.com/features/gov/).

## <a name="supported-scenarios"></a>Escenarios admitidos

Se admiten los siguientes escenarios:

- El usuario inicia sesión en aplicaciones basadas en explorador web.
- El usuario inicia sesión en clientes Outlook que usan protocolos heredados como Exchange ActiveSync, EAS, SMTP, POP e IMAP.
- El usuario inicia sesión en aplicaciones del cliente de Office heredadas que admiten la [autenticación moderna](https://aka.ms/modernauthga): versiones Office 2010, 2013 y Office 2016.
- El usuario inicia sesión en aplicaciones de protocolo heredado, como la versión 1.0 (y otras) de PowerShell.
- Azure AD se combina con los dispositivos con Windows 10.
- Contraseñas de aplicaciones de Multi-Factor Authentication.

## <a name="unsupported-scenarios"></a>Escenarios no admitidos

Los siguientes escenarios _no_ son compatibles:

- Detección de usuarios con [credenciales filtradas](../reports-monitoring/concept-risk-events.md#leaked-credentials).
- Azure AD Domain Services necesita tener habilitada la sincronización de hash de contraseñas en el inquilino. Por lo tanto, los inquilinos que usan la autenticación de paso a través _únicamente_ no funcionan en escenarios que necesitan Azure AD Domain Services.
- La autenticación de paso a través no se viene integrada en [Azure AD Connect Health](whatis-hybrid-identity-health.md).

>[!IMPORTANT]
>Como solución alternativa _solo_ para escenarios no admitidos (excepto la integración de Azure AD Connect Health), habilite la sincronización de hash de contraseñas en la página [Características opcionales](how-to-connect-install-custom.md#optional-features) del Asistente para Azure AD Connect.

>[!NOTE]
Si habilita la sincronización de hash de contraseña, tiene la opción de autenticación mediante conmutación por error si se interrumpe la infraestructura local. La conmutación por error de la autenticación de paso a través a la sincronización de hash de contraseña no es automática. Deberá cambiar el método de inicio de sesión manualmente con Azure AD Connect. Si el servidor que ejecuta Azure AD Connect deja de funcionar, necesitará la ayuda de Soporte técnico de Microsoft para desactivar la autenticación de paso a través.

## <a name="next-steps"></a>Pasos siguientes
- [Inicio rápido](how-to-connect-pta-quick-start.md): poner en marcha la autenticación de paso a través de Azure AD.
- [Migración de AD FS a la autenticación de paso a través](https://aka.ms/ADFSTOPTADPDownload): una guía detallada para migrar desde AD FS (u cualquier otra tecnología de federación) a la autenticación de paso a través.
- [Bloqueo inteligente](../authentication/howto-password-smart-lockout.md): obtenga información sobre cómo configurar la funcionalidad de bloqueo inteligente en el inquilino para proteger las cuentas de usuario.
- [Profundización técnica](how-to-connect-pta-how-it-works.md): conozca cómo funciona la característica de autenticación de paso a través.
- [Preguntas más frecuentes](how-to-connect-pta-faq.md): encuentre respuesta a las preguntas más frecuentes sobre la característica de autenticación de paso a través.
- [Solución de problemas](tshoot-connect-pass-through-authentication.md): obtenga información sobre cómo resolver problemas comunes relacionados con la característica de autenticación de paso a través.
- [Análisis a fondo de la seguridad](how-to-connect-pta-security-deep-dive.md): obtenga información técnica sobre la característica de autenticación de paso a través.
- [SSO de conexión directa de Azure AD](how-to-connect-sso.md): obtenga más información sobre esta característica complementaria.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): use el foro de Azure Active Directory para solicitar nuevas características.
