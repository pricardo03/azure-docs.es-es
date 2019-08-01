---
title: Vulnerabilidades detectadas por Azure Active Directory Identity Protection
description: Información general de las vulnerabilidades detectadas por Azure Active Directory Identity Protection.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 04/09/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 446e2ef33b714afe06f24f3dbfc46bc6a42790b8
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/19/2019
ms.locfileid: "68335159"
---
# <a name="vulnerabilities-detected-by-azure-active-directory-identity-protection"></a>Vulnerabilidades detectadas por Azure Active Directory Identity Protection

Los puntos vulnerables son puntos débiles de un entorno que pueden ser aprovechados por un atacante. Recomendamos a los administradores abordar estas vulnerabilidades para mejorar la seguridad de la organización.

![Vulnerabilidades informadas por Identity Protection](./media/vulnerabilities/identity-protection-vulnerabilities.png)

En las siguientes secciones se ofrece información general de las vulnerabilidades de las que informa Identity Protection.

## <a name="multi-factor-authentication-registration-not-configured"></a>Registro de Multi-Factor Authentication no configurado

Este punto vulnerable ayuda a evaluar la implementación de Azure Multi-Factor Authentication en la organización.

Azure Multi-Factor Authentication proporciona una segunda capa de seguridad a la autenticación de usuario. Azure Multi-Factor Authentication ayuda a proteger el acceso a los datos y las aplicaciones, al tiempo que satisface la demanda de los usuarios de un proceso de inicio de sesión simple. Azure Multi-factor Authentication proporciona opciones de verificación sencillas de usar, como:

* llamada de teléfono
* mensaje de texto
* Notificación en aplicación móvil
* Código de verificación OTP

Se recomienda pedir Azure Multi-Factor Authentication para los inicios de sesión de usuario. La autenticación multifactor desempeña un papel clave en las directivas de acceso condicional basadas en riesgos disponibles mediante Identity Protection.

Para más información, consulte [¿Qué es Azure Multi-Factor Authentication?](../authentication/multi-factor-authentication.md)

## <a name="unmanaged-cloud-apps"></a>Aplicaciones en la nube no administradas

Este punto vulnerable ayuda a identificar aplicaciones en la nube no administradas en su organización.

El personal de TI a menudo desconoce todas las aplicaciones en la nube de la organización. Es fácil entender por qué a los administradores les podría preocupar el acceso no autorizado a datos corporativos, la posible pérdida de datos y otros riesgos relacionados con la seguridad.

Le recomendamos que implemente Cloud Discovery para detectar aplicaciones en la nube no administradas, así como administrar estas aplicaciones con Azure Active Directory.

Para más información, consulte [Cloud Discovery](/cloud-app-security/set-up-cloud-discovery).

## <a name="security-alerts-from-privileged-identity-management"></a>Alertas de seguridad de administración de identidades con privilegios

Este punto vulnerable ayuda a detectar y resolver alertas acerca de las identidades con privilegios en su organización.  

Para que los usuarios puedan realizar operaciones con privilegios, las organizaciones tienen que proporcionar a sus usuarios acceso con privilegios temporal o permanente en Azure AD, recursos de Azure u Office 365, y otras aplicaciones de SaaS. Cada uno de estos usuarios con privilegios aumenta la superficie de ataque de su organización. Este punto vulnerable ayuda a identificar a los usuarios con acceso con privilegios innecesarios y realizar la acción apropiada para reducir o eliminar el riesgo que suponen.

Recomendamos que las organizaciones utilicen Azure AD Privileged Identity Management para administrar, controlar y supervisar las identidades con privilegios en Azure AD y en otros servicios en línea de Microsoft, como Office 365 o Microsoft Intune.

Para obtener más información, vea [Azure AD Privileged Identity Management](../privileged-identity-management/pim-configure.md).

## <a name="see-also"></a>Otras referencias

[Azure Active Directory Identity Protection](../active-directory-identityprotection.md)
