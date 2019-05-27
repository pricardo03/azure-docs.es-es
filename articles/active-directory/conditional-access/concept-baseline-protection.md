---
title: Directivas de protección de acceso condicional previsto - Azure Active Directory
description: Directivas de acceso condicional de línea base para las organizaciones de proteger frente a ataques comunes
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
ms.openlocfilehash: ca062f4024bb5b0946812e00c8ccc1254b56d333
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/22/2019
ms.locfileid: "66003302"
---
# <a name="what-are-baseline-policies"></a>¿Cuáles son las directivas de línea de base?

Las directivas de línea de base son un conjunto de directivas predefinidas que ayudan a proteger frente a ataques comunes muchas de las organizaciones. Estos ataques comunes pueden incluir "phishing", reproducción y protector de contraseña. Las directivas de línea de base están disponibles en todas las ediciones de Azure AD. Microsoft ha realizado estas directivas de protección de la línea base disponible para todos los usuarios porque los ataques basados en la identidad han ido en aumento durante los últimos años. El objetivo de estas cuatro directivas consiste en asegurarse de que todas las organizaciones tienen un nivel básico de seguridad habilitada sin ningún costo adicional.  

Administración de directivas de acceso condicional personalizadas requiere una licencia de Azure AD Premium.

## <a name="baseline-policies"></a>Directivas de base de referencia

![Directivas de línea de base de acceso condicional en Azure portal](./media/concept-baseline-protection/conditional-access-baseline-policies.png)

Hay cuatro directivas de línea de base que las organizaciones pueden habilitar:

* [Requerir MFA para que los administradores](howto-baseline-protect-administrators.md)
* [Protección del usuario final (versión preliminar)](howto-baseline-protect-end-users.md)
* [Autenticación heredados de bloque (versión preliminar)](howto-baseline-protect-legacy-auth.md)
* [Requerir MFA para la administración de servicio (versión preliminar)](howto-baseline-protect-azure.md)

Cuatro de estas directivas afectará a los flujos de autenticación heredados, como POP, IMAP y clientes de escritorio de Office anteriores.

### <a name="require-mfa-for-admins"></a>Requerir MFA para administradores

Debido a la potencia y el acceso que tienen cuentas de administrador, debe tratar con cuidado especial. Es un método común para mejorar la protección de cuentas con privilegios requerir una forma más estricta de comprobación de la cuenta cuando se usan para iniciar sesión. En Azure Active Directory, puede obtener una comprobación de la cuenta más segura al requerir que los administradores registren y utilicen la autenticación multifactor de Azure.

[Requerir MFA para que los administradores](howto-baseline-protect-administrators.md) es una directiva de línea de base que requiere autenticación multifactor (MFA) para los siguientes roles de directorio, considera que los roles de Azure AD con más privilegios:

* Administrador global
* Administrador de SharePoint
* Administrador de Exchange
* Administrador de acceso condicional
* Administrador de seguridad
* Administrador de soporte técnico o administrador de contraseñas
* Administrador de facturación
* Administrador de usuarios

Si su organización tiene estas cuentas en uso en scripts o código, considere la posibilidad de reemplazarlos con [administra identidades](../managed-identities-azure-resources/overview.md). Como solución temporal, puede excluir determinadas cuentas de usuario de la directiva de base de referencia.

### <a name="end-user-protection-preview"></a>Protección del usuario final (versión preliminar)

Administradores con privilegios altos no son los únicos en los ataques de destino. Los actores no válidos tienden a dirigirse a usuarios normales. Después de obtener acceso, estos actores no válidos puede solicitar acceso a información privilegiada en nombre del titular de la cuenta original o descargar todo el directorio y realizar un ataque de suplantación de identidad en toda la organización. Es un método común para mejorar la protección para todos los usuarios necesitan una forma más estricta de comprobación de la cuenta cuando se detecta un inicio de sesión peligroso.

**Protección del usuario final (versión preliminar)** es una directiva de línea de base que protege todos los usuarios en un directorio. Si habilita esta directiva requiere que todos los usuarios se registren para Azure Multi-factor Authentication en 14 días. Una vez registrado, se pedirá a los usuarios para MFA solo durante los intentos de inicio de sesión de riesgo. Las cuentas de usuario en peligro se bloquean hasta que el restablecimiento de contraseña y el riesgo de despido.

### <a name="block-legacy-authentication-preview"></a>Autenticación heredados de bloque (versión preliminar)

Protocolos de autenticación heredados (p. ej.: IMAP, SMTP, POP3) son protocolos usados normalmente por los clientes de correo electrónico más antiguos para autenticarse. Protocolos heredados no admiten la autenticación multifactor. Incluso si tiene una directiva que requiera autenticación multifactor para su directorio, un actor perjudicial puede autenticarse con uno de estos protocolos heredados y omitir la autenticación multifactor.

Es la mejor manera de proteger la cuenta de las solicitudes de autenticación malintencionado realizadas por los protocolos heredados bloquearlas.

El **autenticación heredados de bloque (versión preliminar)** directiva de línea base bloquea las solicitudes de autenticación que se realizan mediante protocolos heredados. Autenticación moderna debe usarse para iniciar sesión correctamente para todos los usuarios. Se usa junto con las otras directivas de línea de base, se bloquearán las solicitudes procedentes de protocolos heredados. Además, todos los usuarios deberán MFA siempre que sea necesario. Esta directiva no bloquea a Exchange ActiveSync.

### <a name="require-mfa-for-service-management-preview"></a>Requerir MFA para la administración de servicio (versión preliminar)

Las organizaciones usar una variedad de servicios de Azure y administran desde herramientas basadas en Azure Resource Manager, como:

* Azure Portal
* Azure PowerShell
* Azure CLI

Uso de cualquiera de estas herramientas para realizar la administración de recursos es una acción con privilegios elevados. Estas herramientas pueden modificar las configuraciones de toda la suscripción, como la configuración de servicio y facturación de suscripción.

Para proteger las acciones con privilegios, esto **exigencia de MFA para la administración de servicio (versión preliminar)** directiva requerirá la autenticación multifactor para cualquier usuario tenga acceso a Azure portal, Azure PowerShell o CLI de Azure.

## <a name="enable-a-baseline-policy"></a>Habilitar una directiva de base de referencia

Para habilitar una directiva de línea de base:

1. Inicie sesión en el **portal Azure** como administrador global, Administrador de seguridad o administrador de acceso condicional.
1. Vaya a **Azure Active Directory** > **acceso condicional**.
1. En la lista de directivas, seleccione una directiva de línea de base que le gustaría habilitar.
1. Establecer **Habilitar directiva** a **en**.
1. Haga clic en Guardar.

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte:

* [Cinco pasos para asegurar su infraestructura de identidad](../../security/azure-ad-secure-steps.md)
* [¿Qué es el acceso condicional en Azure Active Directory?](overview.md)
* [Requerir MFA para que los administradores](howto-baseline-protect-administrators.md)
* [Protección del usuario final (versión preliminar)](howto-baseline-protect-end-users.md)
* [Autenticación heredados de bloque (versión preliminar)](howto-baseline-protect-legacy-auth.md)
* [Requerir MFA para la administración de servicio (versión preliminar)](howto-baseline-protect-azure.md)