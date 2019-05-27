---
title: 'Directiva de línea base protección de usuario final (versión preliminar): Azure Active Directory'
description: Directiva de acceso condicional para requerir autenticación multifactor para usuarios
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
ms.openlocfilehash: 35cd52fb82e5e4cce759be5dfdd8872f64802459
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/22/2019
ms.locfileid: "66003167"
---
# <a name="baseline-policy-end-user-protection-preview"></a>Directiva de línea de base: Protección del usuario final (versión preliminar)

Tendemos a pensar que las cuentas de administrador son las únicas cuentas que necesitan protección con autenticación multifactor (MFA). Los administradores tienen un amplio acceso a información confidencial y pueden realizar cambios en la configuración de toda la suscripción. Sin embargo, los actores no válidos tienden a los usuarios finales de destino. Después de obtener acceso, estos actores no válidos puede solicitar acceso a información privilegiada en nombre del titular de la cuenta original o descargar todo el directorio para llevar a cabo un ataque de suplantación de identidad en toda la organización. Es un método común para mejorar la protección para todos los usuarios necesitan una forma más estricta de comprobación de la cuenta, como la autenticación multifactor (MFA).

Para lograr un equilibrio razonable de seguridad y facilidad de uso, los usuarios no deberían pedirá cada vez que inicio de sesión único. Las solicitudes de autenticación que reflejen el comportamiento normal del usuario, como inicio de sesión desde el mismo dispositivo desde la misma ubicación, tienen la posibilidad de riesgo bajo. Solo inicios de sesión que se consideren peligrosos y muestran las características de un actor perjudicial deben consultarse con desafíos MFA.

![Requerir MFA para los usuarios](./media/howto-baseline-protect-end-users/baseline-policy-end-user-protection.png)

Protección del usuario final es un MFA basado en riesgos [directiva de línea base](concept-baseline-protection.md) que protege todos los usuarios en un directorio, incluidos todos los roles de administrador. Si habilita esta directiva requiere que todos los usuarios deben registrarse en MFA mediante la aplicación de autenticador. Los usuarios pueden omitir el símbolo del sistema de registro MFA durante 14 días, tras el cual se bloqueará de iniciar sesión hasta que se registren en MFA. Una vez registrado para MFA, se pedirá a los usuarios para MFA solo durante los intentos de inicio de sesión de riesgo. Las cuentas de usuario en peligro se bloquean hasta que se restablece su contraseña y los eventos de riesgo se han descartado.

> [!NOTE]
> Esta directiva se aplica a todos los usuarios, incluidas las cuentas de invitado y se evaluará al iniciar sesión en todas las aplicaciones.

## <a name="recovering-compromised-accounts"></a>Recuperación de cuentas en peligro

Para ayudar a proteger a nuestros clientes, las credenciales filtradas un servicio de Microsoft busca pares de nombre de usuario y contraseña disponible públicamente. Si coinciden con uno de nuestros usuarios, protegemos esa cuenta inmediatamente. Usuarios identificados como si tuviera una credencial filtrada resulte se confirman en peligro. Estos usuarios no podrá iniciar sesión hasta que se restablezca su contraseña.

Los usuarios que se les asignados una licencia de Azure AD Premium pueden restaurar el acceso a través de restablecimiento de contraseña de autoservicio (SSPR) si la funcionalidad está habilitada en su directorio. Los usuarios sin una licencia premium que quedaban bloqueados deben ponerse en contacto con el administrador para realizar un restablecimiento de contraseña manual y descartar el evento de riesgo del usuario marcado.

### <a name="steps-to-unblock-a-user"></a>Pasos para desbloquear un usuario

Confirme que el usuario ha estado bloqueado por la directiva mediante el examen de registros de inicio de sesión del usuario.

1. Un administrador debe iniciar sesión en el **portal de Azure** y vaya a **Azure Active Directory** > **usuarios** > haga clic en el nombre del usuario y navegar para inicios de sesión.
1. Para iniciar el restablecimiento de contraseña en un usuario bloqueado, un administrador debe navegar a **Azure Active Directory** > **usuarios marcados en riesgo**
1. Haga clic en el usuario cuya cuenta está bloqueada para ver información sobre la actividad del usuario reciente inicio de sesión.
1. Haga clic en Restablecer la contraseña para asignar una contraseña temporal que debe cambiarse al realizar el siguiente inicio de sesión.
1. Haga clic en Descartar todos los eventos de restablecimiento de puntuación de riesgo del usuario.

El usuario puede ahora iniciar sesión, restablecer su contraseña y acceder a la aplicación.

## <a name="deployment-considerations"></a>Consideraciones de la implementación

Dado que el **protección del usuario final** directiva se aplica a todos los usuarios de su directorio, varias consideraciones deben realizarse para garantizar una implementación sin problemas. Estas consideraciones son la identificación de usuarios y entidades de servicio de Azure AD que no se puede o no se debe realizar la MFA, así como las aplicaciones y los clientes usados por la organización que no admiten la autenticación moderna.

### <a name="legacy-protocols"></a>Protocolos heredados

Protocolos de autenticación heredados (IMAP, SMTP, POP3, etc.) se usan los clientes de correo para realizar solicitudes de autenticación. Estos protocolos no son compatibles con MFA.  La mayoría de las cuentas comprometidas vistos por Microsoft está provocada por los actores no válidos la realización de ataques contra los protocolos heredados intenta omitir MFA. Para asegurarse de que se requiere MFA al iniciar sesión en una cuenta y los actores no válidos no pueden omitir MFA, esta directiva bloquea todas las solicitudes de autenticación realizadas a las cuentas de administrador de protocolos heredados.

> [!WARNING]
> Antes de habilitar esta directiva, asegúrese de que los usuarios no usan protocolos de autenticación heredados. Consulte el artículo [Cómo: Autenticación heredada de bloque en Azure AD con el acceso condicional](howto-baseline-protect-legacy-auth.md#identify-legacy-authentication-use) para obtener más información.

### <a name="user-exclusions"></a>Exclusiones de usuario

Esta directiva de línea de base proporciona la opción de excluir a los usuarios. Antes de habilitar la directiva para el inquilino, se recomienda excluir las cuentas siguientes:

* **Acceso de emergencia** o **emergencia** cuentas para evitar el bloqueo de cuentas de todos los inquilinos. En el improbable caso de que todos los administradores quedan bloqueados fuera de su inquilino, la cuenta administrativa de acceso de emergencia puede usarse para iniciar sesión en los pasos de inquilinos take para recuperar el acceso.
   * Se puede encontrar más información en el artículo, [administrar cuentas de acceso de emergencia en Azure AD](../users-groups-roles/directory-emergency-access.md).
* **Las cuentas de servicio** y **principios de servicio**, como la cuenta Azure AD Connect Sync. Cuentas de servicio son cuentas no interactivas que no están asociadas a un usuario concreto. Normalmente se utilizan por servicios back-end y permitir el acceso mediante programación a las aplicaciones. Puesto que no se puede completar mediante programación MFA, se deben excluir las cuentas de servicio.
   * Si su organización tiene estas cuentas en uso en scripts o código, considere la posibilidad de reemplazarlos con [administra identidades](../managed-identities-azure-resources/overview.md). Como solución temporal, puede excluir estas cuentas específicas de la directiva de línea de base.
* Usuarios que no tiene o no podrá usar un Smartphone.
   * Esta directiva requiere que los usuarios deben registrarse en MFA mediante la aplicación Microsoft Authenticator.

## <a name="enable-the-baseline-policy"></a>Habilitar la directiva de línea base

La directiva **directiva de línea base: Protección del usuario final (versión preliminar)** viene previamente configurada y se mostrarán en la parte superior cuando vaya a la hoja de acceso condicional en Azure portal.

Para habilitar esta directiva y proteger a los administradores:

1. Inicie sesión en el **portal Azure** como administrador global, Administrador de seguridad o administrador de acceso condicional.
1. Vaya a **Azure Active Directory** > **acceso condicional**.
1. En la lista de directivas, seleccione **directiva de línea base: Protección del usuario final (versión preliminar)**.
1. Establecer **Habilitar directiva** a **usar la directiva inmediatamente**.
1. Agregar exclusiones de cualquier usuario haciendo clic en **usuarios** > **Seleccionar usuarios excluidos** y elegir los usuarios que deben excluirse. Haga clic en **seleccione** , a continuación, **realiza**.
1. Haga clic en **guardar**.

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte:

* [Directivas de protección de acceso condicional básicas](concept-baseline-protection.md)
* [Cinco pasos para asegurar su infraestructura de identidad](../../security/azure-ad-secure-steps.md)
* [¿Qué es el acceso condicional en Azure Active Directory?](overview.md)
