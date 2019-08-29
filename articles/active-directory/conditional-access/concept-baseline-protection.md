---
title: 'Directivas de la protección de base de referencia de acceso condicional: Azure Active Directory'
description: Directivas de acceso condicional de base de referencia para proteger las organizaciones frente a ataques comunes
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 572371f4abec413be5a2320c7d69d8126f26924f
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2019
ms.locfileid: "69533059"
---
# <a name="what-are-baseline-policies"></a>¿Qué son las directivas de base de referencia?

Las directivas de base de referencia son un conjunto de directivas predefinidas que ayudan a proteger a las organizaciones frente a muchos ataques comunes. Estos ataques comunes pueden incluir difusión de contraseñas, reproducción y suplantación de identidad (phishing). Las directivas de base de referencia están disponibles en todas las ediciones de Azure AD. Microsoft pone estas directivas de protección de base de referencia a disposición de todos los usuarios porque los ataques basados en la identidad han ido en aumento en los últimos años. El objetivo de estas cuatro directivas consiste en asegurarse de que todas las organizaciones gocen de un nivel básico de seguridad sin ningún costo adicional.  

Para administrar las directivas personalizadas de acceso condicional, se necesita una licencia de Azure AD Premium.

## <a name="baseline-policies"></a>Directivas de base de referencia

![Directivas de base de referencia de acceso condicional en Azure Portal](./media/concept-baseline-protection/conditional-access-baseline-policies.png)

Hay cuatro directivas de base de referencia que las organizaciones pueden habilitar:

* [Requerir MFA para administradores (versión preliminar)](howto-baseline-protect-administrators.md)
* [Protección del usuario final (versión preliminar)](howto-baseline-protect-end-users.md)
* [Bloquear la autenticación heredada (versión preliminar)](howto-baseline-protect-legacy-auth.md)
* [Requerir MFA para la administración de servicios (versión preliminar)](howto-baseline-protect-azure.md)

Estas cuatro directivas de base de referencia afectarán a los flujos de autenticación heredados, como POP, IMAP y clientes de escritorio de Office anteriores.

### <a name="require-mfa-for-admins-preview"></a>Requerir MFA para administradores (versión preliminar)

Dadas las facultades y el acceso de las cuentas de administrador, debe tratarlas con un cuidado especial. Un método común para mejorar la protección de las cuentas con privilegios es exigir una forma de verificación de la cuenta más estricta cuando se emplean para iniciar sesión. En Azure Active Directory, puede exigir a los administradores que se registren y usen Azure Multi-Factor Authentication para conseguir una verificación de cuentas más estricta.

[Requerir MFA para los administradores (versión preliminar)](howto-baseline-protect-administrators.md) es una directiva de base de referencia que requiere autenticación multifactor (MFA) para los siguientes roles de directorio, considerados los roles de Azure AD con más privilegios:

* Administrador global
* Administrador de SharePoint
* Administrador de Exchange
* Administrador de acceso condicional
* Administrador de seguridad
* Administrador del departamento de soporte técnico/administrador de contraseñas
* Administrador de facturación
* Administrador de usuarios

Si su organización usa estas cuentas en scripts o código, piense en la posibilidad de reemplazarlas por [identidades administradas](../managed-identities-azure-resources/overview.md).

### <a name="end-user-protection-preview"></a>Protección del usuario final (versión preliminar)

Los administradores con privilegios elevados no son los únicos blancos de los ataques. Los delincuentes tienden a dirigirse a usuarios normales. Después de obtener acceso, estos delincuentes pueden solicitar acceso a información privilegiada en nombre del titular original de la cuenta o descargar todo el directorio y realizar un ataque de suplantación de identidad en toda la organización. Un método común para mejorar la protección de todos los usuarios es exigir una forma más estricta de verificación de cuentas cuando se detecta un inicio de sesión de riesgo.

**Protección del usuario final (versión preliminar)** es una directiva de base de referencia que protege a todos los usuarios en un directorio. Habilitar esta directiva requiere que todos los usuarios se registren en Azure Multi-factor Authentication en el plazo de 14 días. Una vez registrados, se empleará MFA con los usuarios solo durante los intentos de inicio de sesión de riesgo. Las cuentas de usuario en peligro se bloquean hasta que se restablece la contraseña y se descarta el riesgo. 

[!NOTE]
Los usuarios marcados anteriormente como de riesgo se bloquean hasta que se restablezca la contraseña y se descarte el riesgo tras la activación de la directiva.

### <a name="block-legacy-authentication-preview"></a>bloqueo de la autenticación heredada (versión preliminar)

Los protocolos de autenticación heredados (p. ej.: IMAP, SMTP, POP3) son protocolos usados normalmente por los clientes de correo electrónico más antiguos para autenticarse. Los protocolos heredados no admiten la autenticación multifactor. Incluso si tiene una directiva que exige la autenticación multifactor para su directorio, un delincuente puede autenticarse con uno de estos protocolos heredados y omitir la autenticación multifactor.

La mejor manera de proteger la cuenta frente a solicitudes de autenticación malintencionadas hechas por los protocolos heredados es bloquearlas.

La directiva de base de referencia **Bloquear la autenticación heredada (versión preliminar)** bloquea las solicitudes de autenticación que se hacen mediante protocolos heredados. Todos los usuarios deben usar una autenticación moderna para iniciar sesión correctamente. Usadas junto con las otras directivas de base de referencia, las solicitudes procedentes de protocolos heredados se bloquearán. Además, todos los usuarios deberán aplicar MFA cuando se les exija. Esta directiva no bloquea a Exchange ActiveSync.

### <a name="require-mfa-for-service-management-preview"></a>Requerir MFA para la administración de servicios (versión preliminar)

Las organizaciones usan una variedad de servicios de Azure y los administran desde herramientas basadas en Azure Resource Manager, como:

* Portal de Azure
* Azure PowerShell
* CLI de Azure

Usar cualquiera de estas herramientas para la administración de recursos es una acción con privilegios elevados. Estas herramientas pueden modificar las configuraciones de toda la suscripción, como la configuración del servicio y la facturación de la suscripción.

Para proteger las acciones con privilegios, esta directiva **Requerir MFA para la administración de servicios (versión preliminar)** exigirá la autenticación multifactor para cualquier usuario que acceda a Azure Portal, Azure PowerShell o la CLI de Azure.

## <a name="enable-a-baseline-policy"></a>Habilitar una directiva de base de referencia

Para habilitar una directiva de base de referencia:

1. Inicie sesión en **Azure Portal** como administrador global, administrador de seguridad o administrador de Acceso condicional.
1. Vaya a **Azure Active Directory** > **Acceso condicional**.
1. En la lista de directivas, seleccione una directiva de base de referencia que te gustaría habilitar.
1. Establezca **Habilitar directiva** en **Activado**.
1. Haga clic en Guardar.

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte:

* [Cinco pasos para asegurar su infraestructura de identidad](../../security/fundamentals/steps-secure-identity.md)
* [¿Qué es el acceso condicional en Azure Active Directory?](overview.md)
* [Requerir MFA para administradores (versión preliminar)](howto-baseline-protect-administrators.md)
* [Protección del usuario final (versión preliminar)](howto-baseline-protect-end-users.md)
* [Bloquear la autenticación heredada (versión preliminar)](howto-baseline-protect-legacy-auth.md)
* [Requerir MFA para la administración de servicios (versión preliminar)](howto-baseline-protect-azure.md)
