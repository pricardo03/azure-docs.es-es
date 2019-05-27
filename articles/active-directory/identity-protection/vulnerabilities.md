---
title: Vulnerabilidades detectadas por Azure Active Directory Identity Protection
description: Información general de las vulnerabilidades detectadas por Azure Active Directory Identity Protection.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: article
ms.date: 04/09/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0e792551f4cac857f56454c67d527e01cb9c4281
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66113117"
---
# <a name="vulnerabilities-detected-by-azure-active-directory-identity-protection"></a>Vulnerabilidades detectadas por Azure Active Directory Identity Protection

Puntos vulnerables son puntos débiles en un entorno que puede ser aprovechada por un atacante. Se recomienda que los administradores solucionar estas vulnerabilidades para mejorar la seguridad de su organización.

![Vulnerabilidades que informa Identity Protection](./media/vulnerabilities/identity-protection-vulnerabilities.png)

En las siguientes secciones se ofrece información general de las vulnerabilidades de las que informa Identity Protection.

## <a name="multi-factor-authentication-registration-not-configured"></a>Registro de la autenticación multifactor no configurado

Este punto vulnerable ayuda a evaluar la implementación de Azure Multi-factor Authentication en su organización.

Azure Multi-factor Authentication proporciona una segunda capa de seguridad para la autenticación de usuario. Azure Multi-Factor Authentication ayuda a proteger el acceso a los datos y las aplicaciones, al tiempo que satisface la demanda de los usuarios de un proceso de inicio de sesión simple. Azure Multi-factor Authentication proporciona opciones de comprobación sencillas, como:

* llamada de teléfono
* mensaje de texto
* Notificación de la aplicación móvil
* Código de verificación de OTP

Se recomienda pedir Azure Multi-Factor Authentication para los inicios de sesión de usuario. La autenticación multifactor desempeña un papel clave en las directivas de acceso condicional basadas en riesgos disponibles mediante Identity Protection.

Para más información, consulte [¿Qué es Azure Multi-Factor Authentication?](../authentication/multi-factor-authentication.md)

## <a name="unmanaged-cloud-apps"></a>Aplicaciones de nube no administradas

Este punto vulnerable ayuda a identificar aplicaciones en la nube no administradas en su organización.

El personal de TI a menudo está consciente de todas las aplicaciones en la nube en su organización. Es fácil entender por qué a los administradores les podría preocupar el acceso no autorizado a datos corporativos, la posible pérdida de datos y otros riesgos relacionados con la seguridad.

Se recomienda implementar Cloud Discovery para detectar las aplicaciones de nube no administradas y para administrar estas aplicaciones con Azure Active Directory.

Para más información, consulte [Cloud Discovery](/cloud-app-security/set-up-cloud-discovery).

## <a name="security-alerts-from-privileged-identity-management"></a>Alertas de seguridad de Privileged Identity Management

Este punto vulnerable ayuda a detectar y resolver alertas acerca de las identidades con privilegios en su organización.  

Para que los usuarios puedan realizar operaciones con privilegios, las organizaciones tienen que proporcionar a sus usuarios acceso con privilegios temporal o permanente en Azure AD, recursos de Azure u Office 365, y otras aplicaciones de SaaS. Cada uno de estos usuarios con privilegios aumenta la superficie de ataque de su organización. Este punto vulnerable ayuda a identificar a los usuarios con acceso con privilegios innecesarios y realizar la acción apropiada para reducir o eliminar el riesgo que suponen.

Se recomienda que las organizaciones usen Azure AD Privileged Identity Management para administrar, controlar y supervisar identidades con privilegios en Azure AD, así como otros servicios en línea de Microsoft como Office 365 o Microsoft Intune.

Para obtener más información, vea [Azure AD Privileged Identity Management](../privileged-identity-management/pim-configure.md).

## <a name="see-also"></a>Vea también

[Azure Active Directory Identity Protection](../active-directory-identityprotection.md)
