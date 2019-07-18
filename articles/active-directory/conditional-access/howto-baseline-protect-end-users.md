---
title: 'Directiva de base de referencia: Protección del usuario final (versión preliminar) - Azure Active Directory'
description: Directiva de acceso condicional para requerir autenticación multifactor para los usuarios
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
ms.openlocfilehash: f2644e0e35139ac470b89f6af1b95cf510f60a0a
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561020"
---
# <a name="baseline-policy-end-user-protection-preview"></a>Directiva de base de referencia: Protección del usuario final (versión preliminar)

Se tiende a pensar que las cuentas de administrador son las únicas cuentas que necesitan protección con autenticación multifactor (MFA). Los administradores tienen un amplio acceso a información confidencial y pueden realizar cambios en la configuración de toda la suscripción. Pero los delincuentes tienden a dirigirse a usuarios finales. Después de obtener acceso, estos delincuentes pueden solicitar acceso a información privilegiada en nombre del titular original de la cuenta o descargar todo el directorio para realizar un ataque de suplantación de identidad en toda la organización. Un método común para mejorar la protección de todos los usuarios es exigir una forma más estricta de verificación de cuentas, como la autenticación multifactor (MFA).

Para lograr un equilibrio razonable de seguridad y facilidad de uso, no se debería solicitar que los usuarios se autentiquen cada vez que inician sesión. Las solicitudes de autenticación que reflejan el comportamiento normal del usuario, como un inicio de sesión desde el mismo dispositivo y desde la misma ubicación, tienen un riesgo bajo. Solo se deben consultar con desafíos de MFA los inicios de sesión que se consideren peligrosos y muestren las características de un delincuente.

La protección del usuario final es una [directiva de base de referencia](concept-baseline-protection.md) de MFA basada en riesgos que protege a todos los usuarios en un directorio, incluidos todos los roles de administrador. Si habilita esta directiva es necesario que todos los usuarios se registren para MFA mediante la aplicación de Microsoft Authenticator. Los usuarios pueden omitir la solicitud de registro de MFA durante 14 días, tras los cuales se bloqueará el inicio de sesión hasta que se registren para MFA. Una vez registrados para MFA, se empleará MFA con los usuarios solo durante los intentos de inicio de sesión de riesgo. Las cuentas de usuario en peligro se bloquean hasta que se restablezca la contraseña y se hayan descartado los eventos de riesgo.

> [!NOTE]
> Esta directiva se aplica a todos los usuarios, incluidas las cuentas de invitado, y se evaluará al iniciar sesión en todas las aplicaciones.

## <a name="recovering-compromised-accounts"></a>Recuperar las cuentas en peligro

Para ayudar a proteger a nuestros clientes, el servicio de credenciales filtradas de Microsoft busca pares de nombre de usuario y contraseña disponibles públicamente. Si coinciden con alguno de los usuarios de Microsoft, se protege esa cuenta inmediatamente. Los usuarios que se identifican con credenciales filtradas se confirman como en peligro. Estos usuarios no podrán iniciar sesión hasta que restablezcan su contraseña.

Los usuarios con una licencia de Azure AD Premium asignada pueden restaurar el acceso a través del autoservicio de restablecimiento de contraseña (SSPR) si la funcionalidad está habilitada en su directorio. Los usuarios sin una licencia premium que queden bloqueados deben ponerse en contacto con el administrador para realizar un restablecimiento de contraseña manual y descartar el evento de riesgo del usuario marcado.

### <a name="steps-to-unblock-a-user"></a>Pasos para desbloquear a un usuario

Confirme que la directiva ha bloqueado al usuario al examinar los registros de inicio de sesión del usuario.

1. Un administrador debe iniciar sesión en **Azure Portal** e ir a **Azure Active Directory** > **Usuarios** > hacer clic en el nombre del usuario y navegar a Inicios de sesión.
1. Para iniciar el restablecimiento de contraseña de un usuario bloqueado, el administrador debe navegar a **Azure Active Directory** > **Usuarios marcados en riesgo**.
1. Haga clic en el usuario cuya cuenta está bloqueada para ver la información sobre la actividad reciente de inicio de sesión del usuario.
1. Haga clic en Restablecer la contraseña para asignar una contraseña temporal que debe cambiarse al realizar el siguiente inicio de sesión.
1. Haga clic en Descartar todos los eventos para restablecer la puntuación de riesgo del usuario.

Ahora el usuario puede iniciar sesión, restablecer su contraseña y acceder a la aplicación.

## <a name="deployment-considerations"></a>Consideraciones de la implementación

Dado que la directiva de **protección del usuario final** se aplica a todos los usuarios de su directorio, se deben tener en cuenta varias consideraciones para garantizar que la implementación se realiza sin problemas. Estas consideraciones incluyen la identificación de usuarios y entidades de servicio de Azure AD que no pueden o no deben ejecutar MFA, así como aplicaciones y clientes que se usan en la organización y que no admiten la autenticación moderna.

### <a name="legacy-protocols"></a>Protocolos heredados

Los clientes de correo usan los protocolos de autenticación heredados (IMAP, SMTP, POP3, etc.) para realizar solicitudes de autenticación. Estos protocolos no son compatibles con MFA.  La mayoría de las cuentas en riesgo que ha encontrado Microsoft se deben a que los delincuentes realizan ataques contra los protocolos heredados al intentar omitir la MFA. Para asegurarse de que se requiere la MFA al iniciar sesión en una cuenta y que los delincuentes no pueden omitir la MFA, esta directiva bloquea todas las solicitudes de autenticación realizadas a las cuentas de administrador de protocolos heredados.

> [!WARNING]
> Antes de habilitar esta directiva, asegúrese de que los usuarios no usan protocolos de autenticación heredados. Consulte el artículo [Control de Bloqueo de autenticación heredada a Azure AD con acceso condicional](howto-baseline-protect-legacy-auth.md#identify-legacy-authentication-use) para obtener más información.

## <a name="enable-the-baseline-policy"></a>Habilitar la directiva de base de referencia

La **Directiva de base de referencia: Protección del usuario final (versión preliminar)** viene preconfigurada y se mostrará en la parte superior cuando navegue a la hoja Acceso condicional en Azure Portal.

Para habilitar esta directiva y proteger a los usuarios:

1. Inicie sesión en  **Azure Portal** como administrador global, administrador de seguridad o administrador de acceso condicional.
1. Vaya a **Azure Active Directory** > **Acceso condicional**.
1. En la lista de directivas, seleccione **Directiva de base de referencia: Protección del usuario final (versión preliminar)** .
1. Establezca **Habilitar directiva** en **Usar la directiva inmediatamente**.
1. Haga clic en **Guardar**.

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte:

* [Directivas de la protección de base de referencia de acceso condicional](concept-baseline-protection.md)
* [Cinco pasos para asegurar su infraestructura de identidad](../../security/azure-ad-secure-steps.md)
* [¿Qué es el acceso condicional en Azure Active Directory?](overview.md)
